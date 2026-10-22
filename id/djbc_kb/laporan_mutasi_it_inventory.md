# 7 Laporan Mutasi Resmi IT Inventory DJBC (djbc_kb)

> **Terakhir diperbarui:** 22 Oktober 2026

Sistem BZone menyediakan **7 Laporan Pertanggungjawaban Resmi** yang wajib diakses oleh petugas pemeriksa Bea Cukai secara *online* dan *real-time* sesuai **Peraturan Direktur Jenderal Bea dan Cukai Nomor PER-02/BC/2019**.

---

## Ringkasan 7 Laporan Resmi IT Inventory

Seluruh laporan dapat diakses melalui menu utama **Laporan Bea Cukai > Reporting**:

1. **Laporan Pemasukan Barang per Dokumen Pabean**:
   Merekapitulasi seluruh bukti penerimaan barang berdasarkan jenis dokumen (BC 2.3, BC 4.0, BC 2.7 Masuk).
2. **Laporan Pengeluaran Barang per Dokumen Pabean**:
   Merekapitulasi seluruh pengiriman barang keluar Kawasan Berikat (BC 2.5, BC 4.1, BC 2.7 Keluar, BC 3.0 PEB).
3. **Laporan Mutasi Bahan Baku & Bahan Penolong**:
   Menyajikan saldo awal, pemasukan, pengeluaran untuk produksi, penyesuaian (*adjustment*), dan saldo akhir material.
4. **Laporan Mutasi Barang Jadi (Finished Goods)**:
   Menyajikan saldo awal hasil produksi, penambahan dari lantai pabrik, pengeluaran ekspor/lokal, dan saldo akhir gudang barang jadi.
5. **Laporan Posisi Barang Dalam Proses (WIP)**:
   Mencatat bahan baku yang sedang diproses di *manufacturing order* / lini perakitan yang belum selesai menjadi produk akhir.
6. **Laporan Mutasi Mesin & Peralatan Kantor**:
   Pertanggungjawaban aset mesin dan suku cadang pabrik yang memperoleh fasilitas pembebasan atau penangguhan bea masuk.
7. **Laporan Mutasi Barang Sisa / Reject / Scrap**:
   Mencatat sisa potongan material, limbah produksi, atau barang cacat (*reject*) sebelum dimusnahkan atau dijual ke lokal.

---

## Langkah Penarikan Laporan (Wizard Reporting)

1. Buka menu **Laporan Bea Cukai > Reporting**.
2. Pilih jenis laporan yang diinginkan (misal: **Laporan Mutasi Bahan Baku**).
3. Pada wizard popup yang muncul:
   * **Tanggal Mulai (*Start Date*)**: Tentukan batas awal periode pemeriksaan.
   * **Tanggal Akhir (*End Date*)**: Tentukan batas akhir periode pemeriksaan.
   * **Format Output**: Pilih antara tampilan layar (*HTML View*), dokumen cetak resmi (*Py3o ODT/PDF*), atau spreadsheet (*XLSX*).
4. Klik tombol **Print / Preview**.

![Wizard Penarikan Laporan Mutasi Bea Cukai](/assets/images/kb_report_wizard.png)
*Gambar 5.1: Wizard pemilihan tanggal dan format laporan IT Inventory Kawasan Berikat.*

---

## Rumus Rekonsiliasi Saldo Mutasi

> [!NOTE]
> **Formula Perhitungan:**  
> **Saldo Akhir** = **Saldo Awal** + **Pemasukan** − **Pengeluaran** ± **Penyesuaian (Adjustment)**

* **Pemasukan**: Berasal dari transaksi penerimaan yang divalidasi dokumen pabean masuk.
* **Pengeluaran**: Berasal dari pemakaian produksi (MO) atau pengiriman delivery order.
* **Penyesuaian**: Berasal dari selisih fisik hasil pelaksanaan **Stock Opname IT Inventory**.

![Tampilan Laporan Mutasi Bahan Baku](/assets/images/kb_mutasi_report_preview.png)
*Gambar 5.2: Contoh tampilan tabel laporan pertanggungjawaban mutasi bahan baku.*

---

## Pelaksanaan Stock Opname IT Inventory

Regulasi Bea Cukai mewajibkan perusahaan Kawasan Berikat melakukan stock opname minimal 1 (satu) kali dalam setahun:

1. Buat dokumen stock opname di menu **Laporan Bea Cukai > Stock Opname**.
2. Masukkan hasil perhitungan fisik di gudang (*real count*).
3. Jika terdapat perbedaan antara saldo buku IT Inventory dan fisik:
   * Sistem akan menghitung selisih (*quantity difference*).
   * Cetak **Berita Acara Stock Opname** untuk diserahkan ke Kantor Pelayanan Bea Cukai.
   * Validasi penyesuaian agar saldo buku diperbarui secara akuntabel.

> [!WARNING]
> Selisih lebih atau selisih kurang yang tidak dapat dipertanggungjawabkan penyebabnya dapat dikenakan sanksi administrasi atau tagihan pungutan bea masuk oleh pihak Bea Cukai.

---

## Panduan Kelayakan Data Masuk Laporan (Checklist & Verifikasi)

Agar suatu produk atau transaksi pergerakan barang dapat terangkum dengan akurat pada **Laporan Pemasukan**, **Laporan Pengeluaran**, maupun **Laporan Mutasi**, sistem menerapkan serangkaian kriteria validasi berbasis data operasional Odoo.

![Checklist Kelayakan Data Laporan Bea Cukai](/assets/images/kb_checklist_transaksi_laporan.png)
*Gambar 5.3: Pratinjau alur verifikasi data dan checklist kelayakan transaksi untuk pelaporan DJBC.*

### 1. Kriteria untuk Laporan Pemasukan Barang (Inbound)
Transaksi penerimaan barang (*Receipt / Stock Picking*) akan otomatis masuk ke **Laporan Pemasukan Barang per Dokumen Pabean** apabila memenuhi 4 kondisi:

* **Status Picking Wajib "Done"**: Transaksi penerimaan gudang harus sudah selesai divalidasi (*Validate*). Status *Draft*, *Waiting*, atau *Ready* tidak akan dihitung.
* **Tipe Operasi Gudang (Operation Type)**:
  * Field **DJBC Movement Type** pada Operation Type wajib bernilai `Pemasukan (in)`.
  * Bukan merupakan transaksi *Scrap* atau *Adjustment*.
* **Dokumen Pabean (Customs Document) Terisi Lengkap**:
  * Pada tab **Customs Document / Dokumen Pabean** di formulir Picking, kolom **Jenis Dokumen BC** (`BC 2.3`, `BC 4.0`, atau `BC 2.7`), **Nomor Pendaftaran** (6 digit), dan **Tanggal Pendaftaran** wajib terisi.

> [!IMPORTANT]
> Transaksi penerimaan gudang yang **tidak** mengisi tab Dokumen Pabean akan dianggap sebagai mutasi internal non-fasilitas dan **tidak akan ditarik** ke Laporan Pemasukan DJBC.

* **Data Produk & Nilai Pembelian**:
  * Produk wajib memiliki **Kode Barang** (*Internal Reference*).
  * Kuantitas diterima lebih besar dari 0.
  * Jika terhubung dengan Purchase Order (PO), nilai valuta asing, mata uang, dan konversi Rupiah (berdasarkan Kurs KMK mingguan) otomatis dikompilasi ke laporan.

### 2. Kriteria untuk Laporan Pengeluaran Barang (Outbound)
Transaksi pengiriman barang (*Delivery Order / Stock Picking*) akan otomatis masuk ke **Laporan Pengeluaran Barang per Dokumen Pabean** apabila memenuhi 4 kondisi:

* **Status Delivery Order Wajib "Done"**: Pengiriman barang harus sudah divalidasi tuntas.
* **Tipe Operasi Gudang**:
  * Field **DJBC Movement Type** pada Operation Type pengeluaran wajib bernilai `Pengeluaran (out)`.
  * Bukan merupakan tipe operasi *Adjustment*.
* **Dokumen Pabean Pengeluaran Terisi**:
  * Pada tab **Customs Document** di formulir Delivery Order, field **Jenis Dokumen BC** (`BC 2.5`, `BC 4.1`, `BC 2.7`, atau `BC 3.0 / PEB`), **Nomor Pendaftaran**, dan **Tanggal Pendaftaran** wajib terisi.
* **Data Penjualan & Nilai Penyerahan**:
  * Produk memiliki Kode Barang.
  * Nilai transaksi mengacu pada harga satuan pada Sales Order (SO) dikalikan kuantitas pengiriman, dikonversi menggunakan Kurs KMK resmi.

### 3. Kriteria untuk 7 Laporan Mutasi IT Inventory
Agar suatu produk terdaftar dalam tabel mutasi dan kuantitas pergerakannya (saldo awal, pemasukan, pengeluaran, penyesuaian, saldo akhir) terhitung dengan benar:

* **Syarat Master Data Produk**:
  * **Produk Aktif**: Field `Active` bernilai `True` (tidak diarsip), baik pada level `product.template` maupun `product.product`.
  * **Kode Barang (Internal Reference)**: Wajib terisi karena kode ini menjadi acuan utama identifikasi Bea Cukai.
  * **Flag Fasilitas Kawasan Berikat**: Pada tab **Customs / DJBC**, opsi **Kawasan Berikat** (`djbc_kb_ok`) wajib dicentang aktif (`True`).
  * **Kategori IT Inventory Tepat**: Centang salah satu klasifikasi yang sesuai:
    * `Bahan Baku` (`djbc_rm`) → Masuk ke Laporan Mutasi Bahan Baku & Penolong.
    * `Barang Jadi` (`djbc_fg`) → Masuk ke Laporan Mutasi Barang Jadi.
    * `Barang Dalam Proses` (`djbc_wip`) → Masuk ke Laporan Posisi WIP.
    * `Mesin & Peralatan` (`djbc_mesin`) → Masuk ke Laporan Mutasi Mesin & Peralatan.
    * `Barang Sisa/Reject` (`djbc_scrap`) → Masuk ke Laporan Mutasi Scrap.
* **Syarat Master Lokasi Gudang (Stock Location)**:
  * Mutasi kuantitas dihitung berdasarkan pergerakan fisik masuk/keluar dari lokasi internal yang telah ditandai flag DJBC:
    * Lokasi Bahan Baku (`rm`)
    * Lokasi Barang Jadi (`fg`)
    * Lokasi Produksi/WIP (`wip`)
    * Lokasi Mesin (`mesin`)
    * Lokasi Scrap (`scrap`)
  * Konfigurasi ini dapat dicek melalui menu **Inventory > Configuration > Locations**.
* **Perhitungan Saldo & Periode Wizard**:
  * **Saldo Awal**: Akumulasi seluruh pergerakan stok (*Stock Move Done*) sebelum tanggal mulai (*Start Date*) yang dipilih pada wizard.
  * **Pemasukan Periode**: Stock move yang masuk ke lokasi DJBC terkait dalam rentang tanggal wizard.
  * **Pengeluaran Periode**: Stock move yang keluar dari lokasi DJBC terkait dalam rentang tanggal wizard.
  * **Penyesuaian (Adjustment)**: Dihasilkan dari selisih fisik modul **Stock Opname** yang telah divalidasi dalam periode laporan.

---

## Tabel Diagnostik & Troubleshooting Laporan

Jika terdapat produk atau transaksi yang belum muncul di laporan, gunakan tabel diagnostik berikut untuk menemukan solusi cepat:

| Gejala Masalah | Kemungkinan Penyebab | Langkah Penyelesaian |
| :--- | :--- | :--- |
| **Produk tidak muncul sama sekali di tabel Laporan Mutasi** | Opsi fasilitas KB belum aktif, atau produk tidak memiliki Kode Barang. | Buka Master Produk, pastikan **Kode Barang** (*Internal Reference*) terisi, dan centang opsi **Kawasan Berikat** (`djbc_kb_ok`) pada tab Customs. |
| **Produk muncul di jenis laporan mutasi yang salah** (misal: Bahan Baku muncul di Barang Jadi) | Pengaturan kategori IT Inventory pada produk atau kategori produk tidak tepat. | Periksa tab Customs pada Master Produk atau Kategori Produk, lalu sesuaikan centang kategori (misal: ubah dari FG ke RM). |
| **Penerimaan barang tidak muncul di Laporan Pemasukan** | 1. Picking belum berstatus *Done*.<br/>2. Tab Dokumen Pabean belum diisi nomor/tanggal pendaftaran.<br/>3. Operation Type belum diset `Movement Type = in`. | 1. Lakukan validasi (*Validate*) pada picking.<br/>2. Buka tab Customs Document dan isi jenis dokumen (BC 2.3/4.0/2.7), nomor, dan tanggal pendaftaran.<br/>3. Periksa konfigurasi Operation Type di modul Inventory. |
| **Pengiriman barang tidak muncul di Laporan Pengeluaran** | 1. Delivery order belum berstatus *Done*.<br/>2. Dokumen kepabeanan pengeluaran belum diinput. | Pastikan Delivery Order divalidasi dan lengkapi jenis dokumen (BC 2.5/4.1/2.7/3.0) serta nomor registrasi pabean. |
| **Angka pemasukan/pengeluaran pada Laporan Mutasi bernilai 0 padahal ada transaksi** | 1. Tanggal transaksi berada di luar rentang tanggal wizard.<br/>2. Lokasi gudang belum ditandai flag DJBC (`rm`, `fg`, dsb.). | 1. Sesuaikan kembali tanggal mulai dan akhir pada wizard laporan.<br/>2. Masuk ke **Inventory > Configuration > Locations**, buka lokasi gudang terkait, dan pastikan field klasifikasi lokasi DJBC sudah terpilih. |
