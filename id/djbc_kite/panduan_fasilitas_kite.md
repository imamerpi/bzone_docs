# Fasilitas Kemudahan Impor Tujuan Ekspor (djbc_kite)

Modul **`djbc_kite`** dirancang khusus untuk mengelola operasional kepabeanan dan pelaporan pertanggungjawaban persediaan bagi perusahaan penerima fasilitas **Kemudahan Impor Tujuan Ekspor (KITE)** sesuai regulasi Direktorat Jenderal Bea dan Cukai (DJBC).

---

## Regulasi & Konsep Fasilitas KITE

Fasilitas KITE (diatur antara lain melalui **PMK Nomor 145/PMK.04/2022** jo. **PER-14/BC/2022**) memberikan insentif fiskal bagi industri manufaktur yang mengolah, merakit, atau memasang barang impor untuk kemudian diekspor kembali:

1. **KITE Pembebasan**: Pembebasan Bea Masuk serta Pajak Pertambahan Nilai (PPN) dan Pajak Penjualan atas Barang Mewah (PPnBM) terutang tidak dipungut atas impor bahan baku untuk diolah menjadi barang jadi ekspor.
2. **KITE Pengembalian**: Pengembalian Bea Masuk yang telah dibayar atas impor bahan baku untuk diolah menjadi barang jadi yang telah diekspor.
3. **Kewajiban IT Inventory KITE**:
   * Sistem pencatatan persediaan internal perusahaan (*IT Inventory*) wajib memisahkan secara akuntabel persediaan bahan baku fasilitas dan non-fasilitas.
   * Wajib mencatat konversi pemakaian bahan baku menjadi barang jadi berdasarkan formula produksi (*Bill of Materials* / BoM).
   * Menyediakan laporan pertanggungjawaban mutasi barang pabean yang dapat diakses oleh petugas Bea Cukai secara *real-time*.

---

## Konfigurasi Master Data KITE

Sebelum memproses transaksi operasional, lakukan konfigurasi master data terkait:

### 1. Klasifikasi Produk KITE (`product.template`)
* Buka master produk pada menu **Inventory > Products > Products**.
* Pada tab kepabeanan atau formulir produk, pastikan opsi **Can be Used for DJBC KITE** (`djbc_kite_ok`) telah aktif.
* Opsi ini dapat dikonfigurasikan otomatis secara terpusat melalui master **Kategori Produk** (`product.category`).

### 2. Tipe Operasi Gudang KITE (`stock.picking.type`)
* Buka menu **Inventory > Configuration > Operations Types**.
* Buka tipe pergerakan barang yang digunakan untuk aktivitas KITE, lalu atur field berikut:
  * **DJBC's KITE Movement Type**: Tentukan apakah pergerakan bertindak sebagai **In** (Pemasukan) atau **Out** (Pengeluaran).
  * **DJBC's KITE Scrap?**: Centang jika operasi ini mencatat pergerakan barang sisa, limbah, atau reject.
  * **DJBC's KITE Adjustment?**: Centang jika operasi ini mencatat selisih/penyesuaian persediaan.
  * **DJBC KITE Doc Required**: Aktifkan jika pergerakan wajib melampirkan nomor pendaftaran dokumen pabean sah.

### 3. Klasifikasi Lokasi Gudang KITE (`stock.location`)
* Tentukan lokasi fisik gudang penyimpanan:
  * Gudang Bahan Baku (*Raw Material Location*)
  * Gudang Barang Dalam Proses (*WIP Location*)
  * Gudang Barang Jadi (*Finished Goods Location*)
  * Lokasi Penampungan Sisa Produksi (*Scrap Location*)

---

## Alur Transaksi & Produksi KITE

Alur operasional manufaktur KITE di BZone terintegrasi penuh dengan dokumen pabean:

1. **Pemasukan Bahan Baku**:
   * Impor bahan baku fasilitas didaftarkan dengan dokumen **BC 2.0 / BC 2.3** atau **BC 2.4**.
   * Pemasukan bahan baku lokal pelengkap didaftarkan dengan dokumen **BC 4.0**.
   * Saat validasi dokumen penerimaan (*Receipt Picking*), data secara otomatis masuk ke buku stok dan **Laporan Pemasukan KITE**.
2. **Pemakaian Produksi (Manufacturing)**:
   * Pengeluaran material ke lantai pabrik (*Shopfloor*) mengacu pada *Bill of Materials* (BoM) di modul Manufacturing Odoo.
   * Sistem mencatat jumlah riil bahan baku yang dikonsumsi per Surat Perintah Kerja (SPK / MO) ke **Laporan Pemakaian Bahan Baku & Penolong**.
3. **Pemasukan Hasil Produksi**:
   * Penyelesaian proses manufaktur (*Work in Process* / WIP) menghasilkan Barang Jadi (*Finished Goods*) dan Barang Sisa (*Scrap*).
   * Hasil produksi masuk ke gudang barang jadi dan dicatat pada **Laporan Pemasukan Hasil Produksi**.
4. **Pengeluaran Ekspor**:
   * Penyerahan produk jadi ke luar negeri dilindungi dokumen **BC 3.0 (Pemberitahuan Ekspor Barang / PEB)**.
   * Realisasi ekspor ini menjadi dasar pelunasan atau penyelesaian pertanggungjawaban fasilitas bea masuk KITE ke kantor Bea Cukai.

---

## 8 Laporan Pertanggungjawaban Resmi KITE

Sistem BZone menyediakan **8 Laporan Wajib IT Inventory KITE** yang dapat diakses pada menu **Laporan Bea Cukai > Reporting > Kemudahan Impor Tujuan Expor**:

| No | Nama Laporan Resmi | Deskripsi & Dokumen Acuan |
|---|---|---|
| **1** | **Laporan Pemasukan Barang** | Rekapitulasi seluruh bukti penerimaan material impor/lokal (BC 2.0, BC 2.3, BC 2.4, BC 4.0). |
| **2** | **Laporan Pengeluaran Barang** | Rekapitulasi pengeluaran produk ekspor (BC 3.0), subkontrak, atau penyerahan lokal. |
| **3** | **Laporan Pemakaian Bahan Baku & Penolong** | Rincian pemakaian bahan baku impor & lokal per bukti perintah kerja produksi (MO/SPK). |
| **4** | **Laporan Pemasukan Hasil Produksi** | Rekapitulasi bukti serah terima produk jadi dari lini manufaktur ke gudang barang jadi. |
| **5** | **Laporan Mutasi Bahan Baku & Bahan Penolong** | Rekonsiliasi saldo awal, pemasukan, pemakaian, penyesuaian, dan saldo akhir bahan baku. |
| **6** | **Laporan Mutasi Barang Jadi (Finished Goods)** | Rekonsiliasi saldo awal, penerimaan hasil produksi, realisasi ekspor, dan saldo akhir barang jadi. |
| **7** | **Laporan Mutasi Barang Sisa / Reject / Scrap** | Pertanggungjawaban saldo awal, timbulan sisa proses pabrik, penjualan lokal, pemusnahan, dan saldo akhir scrap. |
| **8** | **Laporan Mutasi Barang Dalam Proses (WIP)** | Rekonsiliasi posisi kuantitas bahan baku yang masih berada dalam proses perakitan/produksi. |

---

## Langkah Penarikan Laporan (Reporting Wizard)

1. Masuk ke menu **Laporan Bea Cukai > Reporting > Kemudahan Impor Tujuan Expor**.
2. Pilih laporan yang ingin dicetak atau ditinjau (misal: **Laporan Pemakaian Bahan Baku & Bahan Penolong**).
3. Pada wizard popup yang tampil:
   * Tentukan **Periode Laporan** (*Report Period*) untuk mengisi rentang tanggal secara otomatis, atau atur kolom **Dari Tanggal** dan **Sampai Tanggal** secara manual.
   * Pilih format keluaran: **Layar / Screen** (*HTML Preview*), cetakan resmi **Py3o ODT/PDF**, atau spreadsheet **Excel (XLSX)**.
4. Klik tombol **Print / Preview** untuk menghasilkan laporan resmi.

> [!TIP]
> Seluruh laporan mutasi KITE menganut formula rekonsiliasi matematis:
> **Saldo Akhir** = **Saldo Awal** + **Pemasukan** − **Pengeluaran/Pemakaian** ± **Penyesuaian (Adjustment)**
