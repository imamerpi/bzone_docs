# INSW IT Inventory Integration Connector (djbc_insw)

The **`djbc_insw`** (INSW Connector) module interfaces the BZone ERP ecosystem directly with the **National Single Window Agency (Lembaga National Single Window / LNSW)** portal under the Ministry of Finance via standard IT Inventory REST APIs (`https://api.insw.go.id`).

---

## 1. Integration Overview

The Indonesian government requires companies operating within Bonded Zones, Special Economic Zones (KEK), and other customs-supervised facilities to synchronize their operational IT Inventory data with the central national INSW portal.

Key capabilities provided:

1. **Automated Beginning Balance Transmission**: Transmission of opening inventory cut-off records for raw materials, finished goods, machinery, and production scrap.
2. **Real-Time Transaction Synchronization**: Transmitting inbound receipts and outbound dispatches (`stock.picking`) linked with their protective customs declaration documents.
3. **Stock Opname Reporting**: Directly submitting annual physical count reconciliation results to INSW servers.
4. **Customs Document Update**: Updating the protective customs document on previously sent INSW transactions.
5. **Data Retrieval & Audit Trail**: Pulling transactions from INSW servers for reconciliation, cleansing, and beginning balance finalization.

---

## 2. Menu Structure in Odoo

Per the `views/menu.xml` of the `djbc_insw` module, the menu structure in Odoo is:

```
INSW Connector                                       (djbc_insw_menu_root)
├── Referensi                                        (djbc_insw_menu_referensi)
│   ├── Jenis Transaksi
│   ├── Kategori Barang
│   ├── Dokumen Pabean
│   ├── Satuan
│   └── Kegiatan
├── Penarikan Data                                    (djbc_insw_menu_penarikan_data)
│   ├── Update Dokumen Pabean                         (menu_insw_update_dokumen)
│   ├── Transaksi                                     (insw_transaksi_menu)
│   ├── Barang Transaksi                              (insw_barang_transaksi_menu)
│   ├── Get Kegiatan                                  (insw_kegiatan_wizard_menu) — group_system
│   ├── Cleansing Data Percobaan                      (insw_cleansing_wizard_menu) — group_system
│   └── Finalisasi Saldo Awal                         (insw_finalize_sa_wizard_menu) — group_system
└── Configuration                                     (djbc_insw_menu_configuration)
    └── API Log                                       (insw_api_log_menu)
```

> [!NOTE]
> The **Saldo Awal** menu (`insw_saldo_awal_menu`) is located under **Inventory > Operations > Adjustments** (parent `stock.menu_stock_adjustments`), not under the INSW Connector root. This is because the beginning balance document is an official Odoo inventory adjustment document.

---

## 3. Configuring INSW API Credentials

1. Navigate to **INSW Connector > Configuration > Settings** (or **Inventory > Configuration > Settings** for the `djbc_app` module).
2. Under **Setting INSW API**:
   * **INSW Key**: The `x-insw-key` authorization token from LNSW.
   * **Unique Key**: The `x-unique-key` header token.
   * **API Environment**: `dev` / `testing` (sandbox) or `prod` / `production`.
3. Under **INSW Additional Setting**:
   * **Flip Pabean Number**: Enable if your company's internal numbering inverts picking references with customs document references.
4. Click **Save**.

```
+-------------------------------------------------------------+
| Setting INSW API                                            |
| INSW Key:         [ ****************************** ]        |
| Unique Key:       [ ****************************** ]        |
| API Environment:  [ Production / Testing ]                  |
| [x] Flip Pabean Number                                      |
+-------------------------------------------------------------+
```

---

## 4. Mapping Master Data to INSW Standards

Before any transmission, Odoo master records must be mapped to the official INSW references through the **INSW Connector > Referensi** menu:

| Reference Menu | Model | When to Fill |
|---|---|---|
| **Jenis Transaksi** | `insw.ref.jenis.transaksi` | Read-only master from INSW server pull. |
| **Kategori Barang** | `insw.ref.kategori.barang` | Used to map `product.category` (`INSW Kategori`): `BAHAN BAKU`, `BARANG JADI`, `MESIN`, etc. |
| **Dokumen Pabean** | `insw.ref.dokumen.pabean` | Maps `djbc_app.document_type` to INSW codes: `23` (BC 2.3), `25` (BC 2.5), `40` (BC 4.0), etc. |
| **Satuan** | `insw.ref.satuan` | Maps `uom.uom` (`INSW Satuan`): `PCE`, `KGM`, `LTR`, etc. |
| **Kegiatan** | `insw.ref.kegiatan` | Master of standard activity codes (`INCOMING`, `OUTGOING`, `SALDO AWAL`). |

---

## 5. Beginning Balance Submission (Saldo Awal)

1. Open **Inventory > Operations > Adjustments > Saldo Awal** (menu `insw_saldo_awal_menu`).
2. Click **Create** — a new document initializes in **Draft** status.
3. Fill in **date** and product lines on the **Products** tab (`line_ids`):
   * `product_id` (must be `is_storable=True`), description, `uom_id`, `qty_available` (read-only), `list_price`.
4. Click **Confirm** → status becomes `confirm`.
5. Click **Send to INSW** → system POSTs to the beginning balance endpoint. Status changes to `sent`.

> [!TIP]
> The **Api Log** smart button (top-left) appears once a transmission occurs. Use it for the audit trail.

---

## 6. Inbound & Outbound Movement Transmission (Stock Picking)

Transmission is integrated with the `stock.picking` form from `djbc_app` and `stock`:

1. Open a warehouse transfer in **Inventory > Operations > Transfers** or on the **Stock Picking** form.
2. Ensure the **Customs Document** field (`djbc_custom_document_id`) is filled.
3. The **Kegiatan** tab appears when `djbc_kek_movement_type ∈ ('in', 'out')`. Select:
   * `kegiatan_id` (domain code `30` / `31`).
   * This auto-fills `name` (Activity Number) and `create_date` (Activity Date).
4. Click **Send to INSW** (`send_insw` button):
   * Visible if `djbc_kek_movement_type ∈ ('in', 'out')`, `state = 'done'`, and not yet sent (`insw_trx_id` empty, `insw_status != 200`).
   * BZone builds a JSON payload (NPWP, NIB, activity, items, attached customs documents) and POSTs to `https://api.insw.go.id/api/inventory/transaksi`.
5. The `insw_trx_id` and `insw_status` fields are populated automatically. The `insw_status` badge appears in the list view (`200` green, `400/401/404` red).

### Updating the Customs Document After Submission

If an INSW transaction has already been sent and you need to **update the protective customs document** (e.g., the document number/date has changed), use the **Update Dokumen Pabean INSW** wizard:

1. Click the **Update Dok. Pabean INSW** button on the Stock Picking form (visible when `insw_trx_id` is filled and `state = 'done'`).
2. Select the **Transaction** (`trx_id`) and the **Mode** (`single` / `bulk`).
3. For `single` mode, select the related **Barang Transaksi** (`barang_trx_id`).
4. Select the **Customs Document** (`dokumen_id`), fill in `nomor_dokumen` and `tanggal_dokumen`.
5. Click **Kirim ke INSW**.

> Or access directly via **INSW Connector > Penarikan Data > Update Dokumen Pabean**.

---

## 7. Reporting Physical Stock Opname to INSW

The `djbc_insw` module inherits `djbc_stock_opname.view_stock_opname_form` and adds 2 buttons:

| Button | Visibility | Function |
|---|---|---|
| **INSW Stock Opname** | `state = 'done'`, not yet inventoried (`is_inventoried = False`), `so_trx_id` empty | Send stock opname data to INSW. |
| **INSW Adjustment** | `state = 'done'`, already inventoried (`is_inventoried = True`), `adj_trx_id` empty | Send physical adjustment data to INSW. |

Once sent, the `so_trx_id` or `adj_trx_id` field is populated on the **INSW Info** tab.

Additional fields: `unit_price` (per product line) and `partner_id` (read-only after done).

---

## 8. Data Retrieval, Audit Trail, and System Tools

### 8.1. Get Kegiatan Wizard (`insw.kegiatan.wizard`)
To retrieve activity data from the INSW server by date range and activity type:

* Open **INSW Connector > Penarikan Data > Get Kegiatan** (group `base.group_system`).
* Fill in `tgl_awal`, `tgl_akhir`, and `kegiatan_id`.
* Click **Confirm** — the system pulls data via API.

> The form shows a dev/prod banner based on `is_production`.

### 8.2. Cleansing Data Percobaan Wizard (`insw.cleansing.wizard`)
To clean up trial data from the INSW server:

* Open **INSW Connector > Penarikan Data > Cleansing Data Percobaan** (group `base.group_system`).
* Fill in the company `npwp`.
* Click **Confirm**.

### 8.3. Finalisasi Saldo Awal Wizard (`insw.finalize.sa.wizard`)
To mark the beginning balance as finalized (dev/testing only):

* Open **INSW Connector > Penarikan Data > Finalisasi Saldo Awal** (group `base.group_system`).
* Fill in `npwp`.
* The **Confirm** button is only shown when `is_production = False`.

### 8.4. Transaksi Menu (`insw.transaksi`)
Read-only list & form displaying the data pulled from INSW. Key fields: `no_dok_kegiatan`, `name` (Transaction ID), `kegiatan_id`, `tgl_kegiatan`, `nama_entitas`. The **Barang Transaksi** tab contains the list of items (`trx_ids`).

### 8.5. Barang Transaksi Menu (`insw.barang.transaksi`)
Read-only list & form per item. The **Dokumen** tab displays the list of attached customs documents.

### 8.6. API Log
Open **INSW Connector > Configuration > API Log** to view all REST API communications. Smart button shortcuts are also available on the forms of:
* **Stock Picking** (smart button `api_log_count`).
* **Stock Opname** (smart button `api_log_count`).
* **Saldo Awal** (smart button `api_log_count`).

The API log history includes: timestamp, endpoint URL, JSON request, response code, and message.

---

## 9. Key Terminology

| Term | Description |
|---|---|
| **LNSW** | Lembaga National Single Window — the operator of the INSW portal. |
| **INSW** | Indonesia National Single Window — the cross-ministry platform for export-import licensing. |
| **Saldo Awal** | Beginning balance — the initial inventory position reported at first integration. |
| **Stock Opname (SO)** | Result of physical inventory verification. |
| **Adjustment (Adj)** | Reconciliation of physical vs. system variance. |
| **Cleansing** | Removal of trial data from the INSW server. |
| **Finalisasi** | Mark the beginning balance as finalized. |
| **Kegiatan** | Movement type: `INCOMING`, `OUTGOING`, or `SALDO AWAL`. |
