# BZone Ecosystem Changelog

Dokumentasi riwayat pembaruan, penambahan fitur, peningkatan sistem, dan perbaikan pada seluruh ekosistem produk BZone.

---

## [v19.0.1.0.2] - 2026-09-08

### Keamanan & Proteksi Sistem
- [Baru] Notifikasi Keamanan Telegram: Notifikasi otomatis terkirim langsung ke grup/akun Telegram administrator ketika terdeteksi pemblokiran akun pengguna akibat percobaan login tidak sah yang berulang.
- [Peningkatan] Penguatan Keamanan Akses: Peningkatan mekanisme proteksi terhadap percobaan login berulang (brute-force prevention) dengan parameter keamanan yang lebih fleksibel.

### Kepabeanan & Dokumen Pabean
- [Baru] Format Pelaporan TPB 4.0: Dukungan penuh pencetakan dan ekspor laporan Tempat Penimbunan Berikat format standar 4.0.
- [Peningkatan] Integrasi Dokumen Pabean & Logistik: Penyaringan otomatis dokumen pabean berdasarkan nomor referensi Sales Order (SO) dan Purchase Order (PO) pada modul penerimaan dan pengiriman barang (Stock Picking).
- [Peningkatan] Tampilan Dokumen Kawasan Bebas (FTZ): Penyempurnaan formulir dan tata letak data dokumen Free Trade Zone untuk memudahkan verifikasi data kepabeanan.
- [Peningkatan] Autocomplete Data Entitas: Fitur pengisian otomatis data entitas pengangkut dan penerima pada dokumen pabean untuk mempercepat proses input data.
- [Peningkatan] Standarisasi Nomor Dokumen: Optimalisasi urutan penomoran otomatis (sequence) dokumen pabean untuk mencegah duplikasi nomor.

### IT Inventory & Pelaporan Mutasi
- [Peningkatan] Rekonsiliasi Mutasi Barang: Penyesuaian kalkulasi pada laporan mutasi, di mana retur penjualan kini tercatat secara otomatis dan akurat pada kolom pemasukan barang.
- [Perbaikan] Format Laporan Fasilitas KITE: Perbaikan format pemisah data (delimiter) dan validasi kalkulasi pada laporan realisasi impor dan ekspor fasilitas KITE.

### Laporan Finansial & Akuntansi
- [Peningkatan] Antarmuka Laporan Finansial Interaktif: Pembaruan tampilan laporan keuangan (Neraca, Laba Rugi, Buku Besar, Trial Balance) dengan pemilih tanggal (Date Picker) modern dan navigasi yang lebih intuitif.
- [Perbaikan] Filter Cetak Laporan Keuangan: Perbaikan filter periode tanggal pada pencetakan dokumen laporan keuangan.

### Manufaktur & Pengalaman Pengguna
- [Peningkatan] Fleksibilitas Antarmuka Shopfloor: Daftar stasiun kerja (Workcenter) pada antarmuka produksi kini mendukung pengguliran halaman (scrollable) yang responsif untuk berbagai ukuran layar.

---

## [v19.0.1.0.1] - 2026-08-20

### Keamanan & Akses Pengguna
- [Baru] Sistem Proteksi Brute-Force Login: Penerapan pembatasan batas gagal login untuk melindungi akun pengguna dari upaya akses ilegal.

### Integrasi Layanan Kepabeanan
- [Peningkatan] Kompatibilitas Layanan Pabean: Pembaruan metode koneksi dan autentikasi untuk sinkronisasi data dokumen kepabeanan yang lebih stabil dan aman.
- [Peningkatan] Pemantauan Log Koneksi: Peningkatan pencatatan log interaksi data kepabeanan guna mempermudah audit dan penelusuran status transmisi.
- [Peningkatan] Menu Penarikan Data Pabean: Penambahan antarmuka terpadu untuk permintaan dan penarikan data layanan kepabeanan secara langsung.
- [Peningkatan] Pencarian Dokumen Pabean: Penambahan filter dan pencarian komprehensif pada daftar dokumen pabean.

### Keuangan & Multi-Mata Uang
- [Peningkatan] Informasi Nilai Mata Uang Ganda: Penyajian nilai nominal transaksi dalam mata uang perusahaan dan valuta asing (foreign currency) secara berdampingan pada dokumen transaksi.
- [Peningkatan] Saldo Awal Neraca: Penambahan penyajian Saldo Awal (Initial Balance) dan filter pencarian cepat akun pada Neraca dan Neraca Saldo (Trial Balance).
- [Perbaikan] Penanganan Referensi Transaksi: Tampilan referensi transaksi tetap tersaji dengan jelas meskipun deskripsi utama tidak diisi.

---

## [v19.0.1.0.0] - 2026-07-20

### Ekosistem IT Inventory & Kepabeanan
- [Baru] Rilis Resmi BZone untuk Odoo 19: Fondasi sistem IT Inventory terintegrasi kepabeanan, mendukung pelaporan Bea Cukai (BC 2.0, BC 2.3, BC 2.5, BC 4.0, PPFTZ).
- [Baru] Manajemen Kemasan & Satuan: Standarisasi satuan ukuran (UOM) dan kemasan barang sesuai regulasi kepabeanan.
- [Baru] Pelacakan Mutasi & Lot/Batch: Pelacakan riwayat pergerakan bahan baku, barang dalam proses, dan barang jadi dari penerimaan hingga pengeluaran.
- [Baru] Periode Pelaporan Terstandarisasi: Wizard pemilihan rentang tanggal dan standarisasi periode pelaporan IT Inventory Bea Cukai.
- [Baru] Jejak Audit (Audit Trail): Pencatatan riwayat aktivitas transaksi kepabeanan untuk kepatuhan regulasi Bea Cukai.
