# Konektor Integrasi INSW IT Inventory (djbc_insw)

Modul **`djbc_insw`** (INSW Connector) menghubungkan sistem ERP BZone secara langsung dengan **Portal Lembaga National Single Window (LNSW)** Kementerian Keuangan melalui protokol REST API IT Inventory (`https://api.insw.go.id`).

---

## 1. Gambaran Umum Integrasi INSW

Pemerintah Indonesia melalui LNSW dan Direktorat Jenderal Bea dan Cukai mewajibkan perusahaan di Kawasan Berikat, Kawasan Ekonomi Khusus (KEK), dan fasilitas kepabeanan lainnya untuk menyelaraskan sistem IT Inventory perusahaan dengan portal nasional INSW.

Modul ini memfasilitasi:

1. **Transmisi Saldo Awal Otomatis**: Pengiriman data cut-off saldo awal persediaan bahan baku, barang jadi, mesin, dan sisa produksi.
2. **Sinkronisasi Transaksi Mutasi**: Pengiriman bukti penerimaan dan pengeluaran barang (`stock.picking`) secara langsung saat pergerakan divalidasi.
3. **Pelaporan Stock Opname**: Pengiriman hasil verifikasi fisik persediaan tahunan langsung ke server INSW.
4. **Update Dokumen Pabean**: Pembaruan dokumen pabean terlampir pada transaksi INSW yang sudah terkirim.
5. **Penarikan Data & Audit Trail**: Penarikan data transaksi dari server INSW untuk rekonsiliasi, *cleansing*, dan finalisasi saldo awal.

---

## 2. Struktur Menu di Odoo

Sesuai dengan `views/menu.xml` di modul `djbc_insw`, struktur menu di Odoo adalah:

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
│   ├── Get Kegiatan                                   (insw_kegiatan_wizard_menu) — group_system
│   ├── Cleansing Data Percobaan                      (insw_cleansing_wizard_menu) — group_system
│   └── Finalisasi Saldo Awal                         (insw_finalize_sa_wizard_menu) — group_system
└── Configuration                                     (djbc_insw_menu_configuration)
    └── API Log                                       (insw_api_log_menu)
```

> [!NOTE]
> Menu **Saldo Awal** (`insw_saldo_awal_menu`) berada di bawah **Inventory > Operations > Adjustments** (parent `stock.menu_stock_adjustments`), bukan di bawah root INSW Connector. Hal ini karena dokumen saldo awal merupakan dokumen penyesuaian stok resmi Odoo.

---

## 3. Konfigurasi Kredensial API INSW

1. Masuk ke **INSW Connector > Configuration > Settings** (atau **Inventory > Configuration > Settings** untuk modul `djbc_app`).
2. Pada blok **Setting INSW API**:
   * **INSW Key**: Token header `x-insw-key` dari LNSW.
   * **Unique Key**: Token header `x-unique-key`.
   * **API Environment**: `dev` / `testing` (sandbox) atau `prod` / `production`.
3. Pada blok **INSW Additional Setting**:
   * **Flip Pabean Number**: Aktifkan bila penomoran internal perusahaan menukar referensi picking vs. dokumen pabean.
4. Klik **Save**.

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

## 4. Pemetaan Master Referensi INSW

Sebelum transmisi, master data Odoo harus dipetakan ke kode standar INSW melalui menu **INSW Connector > Referensi**:

| Menu Referensi | Model | Kapan Diisi |
|---|---|---|
| **Jenis Transaksi** | `insw.ref.jenis.transaksi` | Master read-only hasil pull dari server INSW. |
| **Kategori Barang** | `insw.ref.kategori.barang` | Dipakai untuk memetakan `product.category` (`INSW Kategori`): `BAHAN BAKU`, `BARANG JADI`, `MESIN`, dll. |
| **Dokumen Pabean** | `insw.ref.dokumen.pabean` | Memetakan `djbc_app.document_type` ke kode INSW: `23` (BC 2.3), `25` (BC 2.5), `40` (BC 4.0), dll. |
| **Satuan** | `insw.ref.satuan` | Memetakan `uom.uom` (`INSW Satuan`): `PCE`, `KGM`, `LTR`, dll. |
| **Kegiatan** | `insw.ref.kegiatan` | Master kode kegiatan standar (`INCOMING`, `OUTGOING`, `SALDO AWAL`). |

---

## 5. Pengiriman Saldo Awal Persediaan

1. Buka **Inventory > Operations > Adjustments > Saldo Awal** (menu `insw_saldo_awal_menu`).
2. Klik **Create** — dokumen baru berstatus **Draft**.
3. Isi **date** dan baris barang pada tab **Products** (`line_ids`):
   * `product_id` (wajib `is_storable=True`), deskripsi, `uom_id`, `qty_available` (read-only), `list_price`.
4. Klik **Confirm** → status `confirm`.
5. Klik **Send to INSW** → sistem POST ke endpoint saldo awal. Status berubah ke **sent**.

> [!TIP]
> Tombol **Api Log** pada pojok kiri atas akan muncul setelah ada transmisi. Gunakan untuk audit trail.

---

## 6. Pengiriman Transaksi Mutasi (Stock Picking)

Pengiriman digabung dengan view `stock.picking` dari modul `djbc_app` dan `stock`:

1. Buka **Inventory > Operations > Transfers** atau formulir **Stock Picking**.
2. Pastikan field **Dokumen Pabean** (`djbc_custom_document_id`) terisi.
3. Tab **Kegiatan** muncul jika `djbc_kek_movement_type ∈ ('in', 'out')`. Pilih:
   * `kegiatan_id` (domain kode `30`/`31`).
   * `kegiatan_id` ini akan otomatis mengisi `name` (Nomor Kegiatan) dan `create_date` (Tanggal Kegiatan).
4. Klik **Send to INSW** (tombol `send_insw`):
   * Tersedia jika `djbc_kek_movement_type ∈ ('in', 'out')`, `state = 'done'`, dan belum terkirim (`insw_trx_id` kosong, `insw_status != 200`).
   * BZone menyusun payload JSON (NPWP, NIB, kegiatan, item, dokumen pabean terlampir) dan POST ke `https://api.insw.go.id/api/inventory/transaksi`.
5. Field `insw_trx_id` dan `insw_status` akan terisi otomatis. Badge `insw_status` muncul di list view (`200` hijau, `400/401/404` merah).

### Update Dokumen Pabean Setelah Terkirim

Jika transaksi INSW sudah terkirim dan Anda ingin **memperbarui dokumen pabean terlampir** (misal: ada perubahan nomor/tanggal dokumen), gunakan wizard **Update Dokumen Pabean INSW**:

1. Klik tombol **Update Dok. Pabean INSW** pada formulir Stock Picking (visible jika `insw_trx_id` terisi dan `state = 'done'`).
2. Pilih **Transaksi** (`trx_id`) dan **Mode** (`single` / `bulk`).
3. Untuk mode `single`, pilih **Barang Transaksi** (`barang_trx_id`) terkait.
4. Pilih **Dokumen Pabean** (`dokumen_id`), isi `nomor_dokumen` dan `tanggal_dokumen`.
5. Klik **Kirim ke INSW**.

> Atau akses langsung dari menu **INSW Connector > Penarikan Data > Update Dokumen Pabean**.

---

## 7. Pelaporan Hasil Stock Opname ke INSW

Modul `djbc_insw` mewarisi `djbc_stock_opname.view_stock_opname_form` dan menambahkan 2 tombol:

| Tombol | Tampil Jika | Fungsi |
|---|---|---|
| **INSW Stock Opname** | `state = 'done'`, belum diinventarisasi (`is_inventoried = False`), `so_trx_id` kosong | Kirim data stock opname ke INSW. |
| **INSW Adjustment** | `state = 'done'`, sudah diinventarisasi (`is_inventoried = True`), `adj_trx_id` kosong | Kirim data penyesuaian/penyesuaian fisik ke INSW. |

Setelah terkirim, field `so_trx_id` atau `adj_trx_id` akan terisi pada tab **INSW Info**.

Field tambahan: `unit_price` (pada setiap line barang) dan `partner_id` (readonly setelah done).

---

## 8. Penarikan Data, Audit Trail, dan Tools Sistem

### 8.1. Wizard Get Kegiatan (`insw.kegiatan.wizard`)
Untuk menarik data kegiatan dari server INSW berdasarkan rentang tanggal dan jenis kegiatan:

* Buka **INSW Connector > Penarikan Data > Get Kegiatan** (group `base.group_system`).
* Isi `tgl_awal`, `tgl_akhir`, dan `kegiatan_id`.
* Klik **Confirm** — sistem menarik data via API.

> Formulir menampilkan banner dev/prod sesuai nilai `is_production`.

### 8.2. Wizard Cleansing Data Percobaan (`insw.cleansing.wizard`)
Untuk membersihkan data percobaan dari server INSW:

* Buka **INSW Connector > Penarikan Data > Cleansing Data Percobaan** (group `base.group_system`).
* Isi `npwp` perusahaan.
* Klik **Confirm**.

### 8.3. Wizard Finalisasi Saldo Awal (`insw.finalize.sa.wizard`)
Untuk menandai bahwa saldo awal sudah final (hanya untuk lingkungan dev/testing):

* Buka **INSW Connector > Penarikan Data > Finalisasi Saldo Awal** (group `base.group_system`).
* Isi `npwp`.
* Tombol **Confirm** hanya muncul jika `is_production = False`.

### 8.4. Menu Transaksi (`insw.transaksi`)
Read-only list & form yang menampilkan hasil penarikan data dari INSW. Field penting: `no_dok_kegiatan`, `name` (ID Transaksi), `kegiatan_id`, `tgl_kegiatan`, `nama_entitas`. Tab **Barang Transaksi** berisi daftar barang (`trx_ids`).

### 8.5. Menu Barang Transaksi (`insw.barang.transaksi`)
Read-only list & form per barang. Tab **Dokumen** menampilkan daftar dokumen pabean terlampir.

### 8.6. API Log
Buka **INSW Connector > Configuration > API Log** untuk melihat seluruh komunikasi REST API. Tombol pintas **Api Log** juga tersedia pada formulir:
* **Stock Picking** (smart button `api_log_count`).
* **Stock Opname** (smart button `api_log_count`).
* **Saldo Awal** (smart button `api_log_count`).

Riwayat API log mencakup: timestamp, URL endpoint, JSON request, response code, dan pesan.

---

## 9. Daftar Istilah

| Istilah | Penjelasan |
|---|---|
| **LNSW** | Lembaga National Single Window — operator portal INSW. |
| **INSW** | Indonesia National Single Window — platform integrasi lintas K/L untuk perizinan ekspor-impor. |
| **Saldo Awal** | Posisi persediaan awal yang dilaporkan saat integrasi pertama. |
| **Stock Opname (SO)** | Hasil verifikasi fisik persediaan. |
| **Adjustment (Adj)** | Penyesuaian kuantitas selisih fisik vs. sistem. |
| **Cleansing** | Pembersihan data percobaan di server INSW. |
| **Finalisasi** | Penanda bahwa saldo awal sudah final. |
| **Kegiatan** | Tipe pergerakan: `INCOMING`, `OUTGOING`, atau `SALDO AWAL`. |
