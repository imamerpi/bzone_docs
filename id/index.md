# Panduan BZone IT Inventory & Kepabeanan

Selamat datang di portal dokumentasi **BZone IT Inventory**. Sistem ini dirancang untuk memenuhi kepatuhan regulasi Direktorat Jenderal Bea dan Cukai (DJBC) Republik Indonesia, khususnya untuk fasilitas **Kawasan Berikat (KB)**, **Kemudahan Impor Tujuan Ekspor (KITE)**, **Free Trade Zone (FTZ)**, dan integrasi layanan **CEISA 4.0**.

---

## Gambaran Umum Sistem

BZone mengintegrasikan proses operasional ERP (Pengadaan, Gudang, Produksi, dan Penjualan) secara *real-time* dengan pencatatan dokumen kepabeanan Republik Indonesia. Setiap pergerakan barang secara otomatis membentuk rekonsiliasi data IT Inventory sesuai standar audit kepabeanan.

> [!NOTE]
> Seluruh pergerakan barang masuk dan keluar pada sistem BZone wajib mengacu pada nomor pendaftaran dokumen pabean yang sah (misal: BC 2.3, BC 2.5, BC 4.0, atau BC 4.1).

---

## Struktur Modul Utama

Ekosistem BZone tersusun atas modul-modul terintegrasi:

1. **BZone Core (`djbc_app`)**:
   Fondasi utama manajemen dokumen pabean, standarisasi satuan ukuran (UOM), jenis kemasan, periode pelaporan, dan jejak audit (*audit trail*).
2. **Kawasan Berikat (`djbc_kb`)**:
   Klasifikasi kategori barang IT Inventory, alur barang masuk/keluar berfasilitas penangguhan bea masuk, pencatatan proses produksi/WIP, dan 7 Laporan Mutasi Resmi DJBC.
3. **Kemudahan Impor Tujuan Ekspor (`djbc_kite`)**:
   Fasilitas pembebasan dan pengembalian bea masuk untuk bahan baku olahan ekspor, pencatatan pemakaian bahan (BoM), serta 8 Laporan Pertanggungjawaban Resmi KITE.
4. **Kawasan Ekonomi Khusus (`djbc_kek`)**:
   Pengelolaan kepabeanan KEK dengan dokumen mandiri PPKEK (LDP/TLDDP), fasilitas pembebasan bea masuk & PPN tidak dipungut, dan 4 Laporan Wajib IT Inventory KEK.
5. **Stock Opname (`djbc_stock_opname`)**:
   Pencatatan surat tugas, pelaksanaan perhitungan fisik tahunan di gudang, rekonsiliasi selisih buku vs fisik, dan penerbitan kartu stock opname resmi.
6. **Kurs Pabean KMK (`erpi_custom_rate`)**:
   Penetapan kurs manual multi-mata uang pada PO, SO, dan Invoice sesuai tarif kurs mingguan Keputusan Menteri Keuangan (KMK).
7. **Konektor Layanan INSW (`djbc_insw`)**:
   Sinkronisasi data elektronik saldo awal, mutasi penerimaan/pengeluaran, dan pelaporan stock opname ke portal nasional INSW.
8. **CEISA Core (`djbc_ceisa_core`)**:
   Gateway integrasi terpusat CEISA 4.0 OpenAPI, manajemen autentikasi OAuth2 token, sinkronisasi kurs KMK, pos tarif, lartas, dan manifes TPS.
9. **CEISA TPB (`djbc_ceisa_tpb`)**:
   Penyusunan dan pengiriman dokumen resmi Tempat Penimbunan Berikat (BC 1.6, 2.3, 2.5, 2.6.1, 2.6.2, 2.7, 4.0, 4.1) serta penarikan respon pabean (SPPB/NPP/Reject).
10. **CEISA Impor (`djbc_ceisa_impor`)**:
   Penyusunan dan pengiriman dokumen Pemberitahuan Impor Barang (PIB / BC 2.0) untuk impor umum dari luar daerah pabean, dengan validasi 5 lapis dan auto-generate entitas Importir/Pemilik/Pengirim/Penjual.
11. **CEISA Ekspor (`djbc_ceisa_expor`)**:
   Penyusunan dan pengiriman dokumen Pemberitahuan Ekspor Barang (PEB / BC 3.0) dan PEB dari PLB (BC 3.3), termasuk data Bank Devisa, Kesiapan Barang, dan auto-fetch NPE.
12. **CEISA FTZ (`djbc_ceisa_ftz`)**:
   Penyusunan dan pengiriman dokumen PPFTZ (511/513/522/531) untuk perusahaan di KPBPB Batam, Bintan, Karimun, dan Sabang dengan Strategy Pattern serializer khusus per jenis dokumen.

![Tampilan Dashboard Utama BZone](/assets/images/dashboard_overview.png)
*Gambar 1.1: Dashboard pemantauan dokumen pabean dan status IT Inventory BZone.*

---

## Langkah Awal Penggunaan

Untuk memulai implementasi BZone di perusahaan Anda, ikuti tahapan panduan terstruktur berikut:

1. **Persiapan Master Data**:
   * Daftarkan seluruh item persediaan melalui panduan **[Master Data Produk & Rute](/bzone/docs/master_data/produk)**.
   * Buat profil pemasok dan pelanggan pada **[Master Mitra Bisnis (Vendor & Customer)](/bzone/docs/master_data/mitra_bisnis)**.
2. **Penguasaan Operasional Bisnis Standar**:
   * Pelajari pemesanan penjualan dan otomasi MTO pada **[Siklus Penjualan (Sales)](/bzone/docs/operasional/penjualan_sales)**.
   * Jalankan pengadaan material melalui **[Siklus Pengadaan (Purchase)](/bzone/docs/operasional/pengadaan_purchase)**.
   * Eksekusi penerimaan dan pengiriman pada **[Penerimaan & Pengiriman Barang (Inventory)](/bzone/docs/operasional/pergudangan_inventory)**.
   * Atur perakitan/komposisi bahan pada **[Siklus Manufaktur & BoM (MRP)](/bzone/docs/operasional/manufaktur_mrp)**.
   * Lakukan penagihan dan pembayaran pada **[Keuangan & Akuntansi (Accounting)](/bzone/docs/operasional/keuangan_accounting)**.
3. **Kepatuhan Regulasi Kepabeanan BZone**:
   * Pelajari **[Konfigurasi Dasar & Master Dokumen](/bzone/docs/djbc_app/konfigurasi_dasar)** untuk mengatur jenis dokumen BC dan satuan barang DJBC.
   * Atur **[Klasifikasi Kategori Barang](/bzone/docs/djbc_kb/klasifikasi_barang)** pada master produk (Bahan Baku, Penolong, Barang Jadi, WIP, Mesin, Scrap).
   * Jalankan transaksi kepabeanan sesuai panduan **[Alur Pemasukan & Pengeluaran](/bzone/docs/djbc_kb/pemasukan_pengeluaran)** dan manajemen **[Kurs Pabean KMK](/bzone/docs/erpi_custom_rate/manajemen_kurs_pabean)**.
   * Tinjau fasilitas spesifik: **[Kemudahan Impor Tujuan Ekspor (KITE)](/bzone/docs/djbc_kite/panduan_fasilitas_kite)** atau **[Kawasan Ekonomi Khusus (KEK)](/bzone/docs/djbc_kek/panduan_fasilitas_kek)** sesuai izin fasilitas perusahaan.
   * Lakukan penarikan dan rekonsiliasi data pada **[7 Laporan Mutasi IT Inventory](/bzone/docs/djbc_kb/laporan_mutasi_it_inventory)**.
   * Jalankan pencacahan fisik tahunan melalui **[Pelaksanaan Stock Opname](/bzone/docs/djbc_stock_opname/pelaksanaan_stock_opname)**.
   * Konfigurasikan konektivitas **[Integrasi Fondasi CEISA 4.0](/bzone/docs/djbc_ceisa_core/integrasi_ceisa_core)** dan lakukan transmisi dokumen pada **[Pengelolaan Dokumen TPB](/bzone/docs/djbc_ceisa_tpb/pengelolaan_dokumen_tpb)**, **[Dokumentasi Impor BC 2.0](/bzone/docs/djbc_ceisa_impor/dokumentasi_impor)**, **[Dokumentasi Ekspor BC 3.0 & 3.3](/bzone/docs/djbc_ceisa_expor/dokumentasi_expor)**, atau **[Dokumentasi PPFTZ (KPBPB)](/bzone/docs/djbc_ceisa_ftz/dokumentasi_ftz)**.
   * Integrasikan pengiriman data berkala ke portal nasional via **[Konektor Layanan INSW](/bzone/docs/djbc_insw/konektor_layanan_insw)**.

> [!TIP]
> Pastikan hak akses pengguna (*User Security Groups*) telah dikonfigurasikan dengan tepat sebelum operator memulai penginputan transaksi harian.

