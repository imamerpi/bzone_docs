# CEISA Import Module (djbc_ceisa_impor)

The **`djbc_ceisa_impor`** module manages **Import Declaration Documents (BC 2.0 / PIB)** and submits them to the CEISA 4.0 portal of the Indonesian Directorate General of Customs and Excise (DJBC). This document is a user guide for Exim Operations, Logistics, Customs Brokers (PPJK), and Company Customs Staff.

> [!IMPORTANT]
> Per the implementation in `__manifest__.py` and the `_check_user_kode_dokumen_permission` validation in the `ceisa.import.document` model, **this module only supports BC 2.0 documents** (reference code `20`). For BC 2.3 (import into Bonded Zone) and BC 2.7 (inter-TPB), use the [`djbc_ceisa_tpb`](../djbc_ceisa_tpb/pengelolaan_dokumen_tpb.md) module.

---

## 1. Module Scope & Document Type

| BC Code | Document Type | Purpose |
|---|---|---|
| **BC 2.0** | Import Declaration (PIB) | General import from outside the customs area (LDP) to a Temporary Storage Facility (TPS) at a seaport or airport, with the obligation to pay Import Duty, VAT, Article 22 Income Tax, and other applicable taxes (excise, Art 23, etc.). |

> [!TIP]
> This module produces the equivalent of the PIB document in the **CEISA 4.0** host-to-host application from DJBC. Make sure you have a valid **DJBC Company Code** and **Company NPWP** before starting data entry.

---

## 2. Import Document Form Anatomy

The BC 2.0 document is organized into several tabs following the CEISA standard:

```mermaid
graph TD
    A["Header & 26-Digit Submission No."] --> B["Customs Entities (Importer, Owner, Shipper, Seller)"]
    A --> C["Supporting Documents (Invoice, B/L, AWB, etc.)"]
    A --> D["Transport Mode & Ports"]
    A --> E["Packaging & Containers"]
    A --> F["Goods, Tariff & Import Conditions"]
    A --> G["Authorized Signatory Declaration"]
```

---

## 3. Document Lifecycle

| State | Description | Available Actions |
|---|---|---|
| **Draft** | New document / being prepared. All fields freely editable. | Edit all fields, Add Item, Auto-Fill |
| **Confirm** | Data is internally valid. Status is locked for submission. | **Send to CEISA**, Set to Draft |
| **Sent** | Document has been successfully transmitted to CEISA and received an NPP. | **Check Status & Response**, **Start Revision** |

> [!WARNING]
> The **Confirm → Sent** transition only occurs if all `check_*` validations (see Section 5) pass. Otherwise, the system raises a **UserError** with a list of incomplete fields.

---

## 4. Step-by-Step Filling Procedure

Open the **DJBC CEISA > Import > Import Documents** menu and click **Create**.

### Step 1: Header Tab

* **26-Digit Submission Number (Nomor Aju)** — Auto-generated when you select **Customs Office** (`kode_kantor`) and **Document Code** (`kode_dokumen`). Pattern: `[4-digit office][2-digit BC][6-digit company code][14-digit sequence]`. Example: `0403002000012325101000012345`.
* **Customs Office** — Select according to the supervising office where the TPS is located.
* **Loading Port, Destination Port, TPS** — All required.
* **Tutup PU Code** — Must be one of: `11`, `12`, or `14`. Other codes are rejected by `check_detail_dokumen`.
* **Currency Code & NDPBM** — The weekly KMK tax rate is auto-filled when a currency is selected. See [exchange rate guide](../erpi_custom_rate/manajemen_kurs_pabean.md).

  ![Import Header](/assets/images/ceisa_impor_header.png)

### Step 2: Entities Tab

When the document is created, the system **automatically generates 4 entity lines** following the BC 2.0 standard:

| Series | Code | Role | Auto-Fill |
|---|---|---|---|
| 1 | `1` | **Importer** | Yes, from `company.partner_id` |
| 2 | `7` | **Cargo Owner** | No |
| 3 | `9` | **Shipper** | No |
| 4 | `10` | **Seller** | No |

**Required** fields for the Importer entity: Name, Address, 16-digit NPWP, NIB, **API Type** (`01` APIU or `02` APIP), and **Identity Type** (codes 2/3/4/5/6).

**Required** fields for the Cargo Owner: Name, Address, Identity Number, and **Affiliation** (`AFL`, `CTR`, `FAM`, or `TAH`).

  ![Import Entities Tab](/assets/images/ceisa_impor_entitas.png)

### Step 3: Supporting Documents Tab

Choose the supporting document type per the CEISA reference codes:

| Code | Document |
|---|---|
| `380` | Commercial Invoice |
| `705` | Bill of Lading (B/L) |
| `740` | Air Waybill (AWB) |
| `217` | Packing List |
| `861` | Certificate of Origin (SKA) |
| `261` | Sales Contract |
| `731` | Supporting Customs Document (e.g., BC 1.6 from TPS) |

  ![Import Supporting Documents](/assets/images/ceisa_impor_dokumen_pelengkap.png)

### Step 4: Transport, Packaging, Container Tab

* **Transport Mode** — Choose mode (Sea / Air / Land) and the vehicle/vessel name.
* **Country Flag Code** — 2-letter ISO capital letters (e.g., `SG`, `CN`, `JP`, `US`).
* **Container Number** — 11 digits (4-letter prefix + 7 digits), include the size (`20` / `40` / `45` ft).
* **Quantity and Type of Packaging** — CEISA units such as `CTN` (carton), `PLT` (pallet), `BG` (bag), `DR` (drum).

  ![Import Transport and Container](/assets/images/ceisa_impor_pengangkut.png)

### Step 5: Goods Tab (Item Details)

* Select **Product** from master → the system auto-fills **HS Code** (`pos_tarif`) and **CEISA Unit** from `product.product`.
* Enter **Quantity** and **CIF Value** per line.
* Select **Customs Valuation Method**: Method 1 to 6. If not Method 1, fill in the **Reason** in `alasan_metode_penentuan_nilai`.
* **Goods Tariff** tab (Import Duty, VAT, Art 22 Tax, Excise) is auto-generated after HS Code is filled (see [CEISA Reference Guide](panduan_pengisian_ceisa.md) for automatic calculation from the TPB module).
* **Special Specification** tab (for certain HS categories) and **Voluntary Declaration** can be filled as needed.

  ![Import Goods Details](/assets/images/ceisa_impor_barang.png)

### Step 6: Signatory Tab

* **Signature City** — Auto-filled from `company.partner_id.city_id` or `company.city_id`.
* **Signatory Name** — Select from `hr.employee` (auto-fills the **Job Position**).
* **Disclaimer** — Set to `1` (Yes) to declare approval.

---

## 5. Automatic Pre-Submission Validation

Before the status changes from **Draft → Confirm → Sent**, the system runs **5 validation functions** (`check_*` in `ceisa.import.document`):

| Function | Validation Performed |
|---|---|
| `check_detail_dokumen` | 26-digit Submission Number, Customs Office, Loading/Destination Port, TPS, Currency, Payment Method, Tutup PU Code (11/12/14), Import Type, Arrival Date / Signature Date, Signature City/Name/Position. |
| `check_entitas` | Importer (1) and Owner (7) entities are mandatory. Importer must have Name, Address, NPWP, NIB, API (01/02), and Identity Type. |
| `check_dokumen_pelengkap` | Minimum supporting documents (Invoice & B/L/AWB). |
| `check_kemasan_pengangkut` | Packaging and transport data are filled. |
| `check_barang` | Each item has HS Code, unit, value, and `jenis_kemasan`. |

If any of these fail, the document will not transition to `Confirm`. Use the **Confirm** button to trigger validation, then correct the flagged fields.

---

## 6. Sending the Document to CEISA (Send Document)

1. Click **Confirm** — status changes to `Confirm`.
2. Click **Send Document** — the system POSTs to the endpoint:
   ```
   POST {CEISA_BASE_URL}/openapi/document?isFinal=false&isRevision=false
   ```
3. The body is sent as **JSON** assembled by `_prepare_ceisa_import_data()` (containing header, entities, goods, goodsTarif, goodsDokumen, goodsPemilik, etc.).
4. If the server returns **401 Unauthorized**, the system automatically refreshes the token via `ceisa.auth.get_auth_headers(force=True)` and retries.
5. Successful response → status changes to `Sent`, the JSON payload is recorded in `ir.api.log` (via the **Api Log** button), and a notification email is sent to the user (if `ceisa.send_email_on_success=True`).

> [!TIP]
> The `isFinal` parameter is controlled by the System Parameter `ceisa.is_final_dokumen`. Default is `false` (draft). Set it to `true` if you want the document to be finalized on first submission.

---

## 7. Revising a Sent Document

The **Start Revision** button is only available for documents in `Sent` status. The system will:

1. Increment the `revision_count` and `seri` in the header.
2. Revert the status to `Draft`.
3. Set `is_revision = True` so the server receives `isRevision=true` on resubmission.

---

## 8. Key Terminology

| Term | Description |
|---|---|
| **PIB (BC 2.0)** | Pemberitahuan Impor Barang — the official customs declaration for general imports. |
| **NPP** | Nomor Pendaftaran Pabean — proof the document is registered in CEISA. |
| **SPPB** | Surat Persetujuan Pengeluaran Barang — release permit from the TPS. |
| **SPJM** | Surat Pemberitahuan Jalur Merah — notice for physical inspection. |
| **NDPBM** | Nilai Dasar Perhitungan Bea Masuk — weekly KMK tax exchange rate. |
| **FOB / CIF / Freight / Insurance** | Customs value components (see TPB guide for full formulas). |
| **HS Code** | 8-digit code based on BTKI (Indonesian Customs Tariff Book). |

---

> This document is a non-technical guide for the **`djbc_ceisa_impor`** module. For exports, refer to the [CEISA Export Documentation](../djbc_ceisa_expor/dokumentasi_expor.md). For TPB/BC 2.3/BC 4.0/BC 2.5 transactions, use the [`djbc_ceisa_tpb`](../djbc_ceisa_tpb/pengelolaan_dokumen_tpb.md) module.
