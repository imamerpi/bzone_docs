# Konfigurasi Dasar & Master Dokumen (djbc_app)

Modul **`djbc_app`** berfungsi sebagai modul induk (*parent dependency*) bagi seluruh ekosistem BZone. Modul ini menyediakan master data kepabeanan standar, validasi nomor pendaftaran pabean, dan aturan periode pelaporan.

---

## Master Jenis Dokumen Pabean

Setiap transaksi penerimaan (*incoming*) maupun pengeluaran (*outgoing*) barang pada perusahaan penerima fasilitas kepabeanan wajib memiliki nomor jenis dokumen BC yang valid.

Pada menu **Laporan Bea Cukai > Configuration > Document Type**, sistem telah menyediakan jenis dokumen kepabeanan standar:

* **BC 2.3**: Dokumen impor barang untuk dimasukkan ke Tempat Penimbunan Berikat (Kawasan Berikat).
* **BC 2.5**: Dokumen pengeluaran barang dari Kawasan Berikat ke Tempat Lain Dalam Daerah Pabean (TLDDP / Lokal).
* **BC 4.0**: Dokumen pemasukan barang dari TLDDP ke Kawasan Berikat.
* **BC 4.1**: Dokumen pengeluaran barang dari Kawasan Berikat ke TLDDP dengan fasilitas tertentu.
* **BC 2.7**: Dokumen perpindahan barang antar-Kawasan Berikat atau Tempat Penimbunan Berikat lainnya.
* **PPFTZ**: Dokumen kepabeanan untuk kawasan perdagangan bebas (Free Trade Zone).

![Formulir Master Jenis Dokumen Pabean](/assets/images/djbc_document_type.png)
*Gambar 2.1: Daftar konfigurasi tipe dokumen kepabeanan pada modul BZone.*

> [!WARNING]
> Jangan mengubah kode dokumen (*Code*) atau tipe pendaftaran resmi jika sistem telah berjalan secara operasional karena akan memengaruhi validasi laporan ke portal Bea Cukai.

---

## Standarisasi Satuan Ukuran (UOM) DJBC

Regulasi Bea Cukai mewajibkan penggunaan kode satuan internasional yang terdaftar pada tabel referensi kepabeanan (misal: `KGM` untuk Kilogram, `PCE` untuk Pieces, `MTR` untuk Meter).

Langkah pemetaan satuan:
1. Buka menu **Inventory > Configuration > Units of Measure**.
2. Buka salah satu satuan ukuran yang digunakan.
3. Pada tab **DJBC Mapping**, pastikan field **DJBC Code** terisi kode resmi Bea Cukai.

![Mapping Satuan UOM DJBC](/assets/images/djbc_uom_mapping.png)
*Gambar 2.2: Pemetaan satuan barang internal ke kode resmi DJBC.*

---

## Manajemen Kemasan (Packaging)

Pengisian jenis kemasan (misal: Pallet, Drum, Box, Bag) diperlukan pada saat pencatatan dokumen pabean BC 2.3 atau BC 2.5.

* Masuk ke **Laporan Bea Cukai > Configuration > Kemasan**.
* Pastikan kode kemasan sesuai referensi resmi (contoh: `PX` untuk Pallet, `BX` untuk Box).

---

## Master Periode Pelaporan (Report Period)

Master Periode Pelaporan digunakan sebagai acuan rentang tanggal standar untuk mempermudah dan mempercepat pengguna saat menarik laporan berkala (misal: bulanan, triwulanan, atau tahunan). Fitur ini berfungsi sebagai jalan pintas (*helper*) pengisian otomatis tanggal pada wizard laporan (*Date Range Selector*), **bukan** untuk mengunci transaksi ataupun mutasi barang.

* Masuk ke menu **Laporan Bea Cukai > Configuration > Report Period**.
* Buat periode baru dengan mengisi:
  * **Period**: Nama identifikasi periode (contoh: `Januari 2025`, `Q1 2025`).
  * **Code**: Kode unik periode (contoh: `2025-01`).
  * **Date Start**: Tanggal awal periode pelaporan.
  * **Date End**: Tanggal akhir periode pelaporan.

> [!TIP]
> Saat membuka wizard penarikan laporan (misalnya Laporan Mutasi IT Inventory), memilih salah satu **Periode Laporan** akan otomatis mengisi kolom **Dari Tanggal** (*Date Start*) dan **Sampai Tanggal** (*Date End*). Pengguna juga tetap dapat mengisi atau mengubah tanggal secara manual jika ingin menarik data dengan rentang waktu kustom.
