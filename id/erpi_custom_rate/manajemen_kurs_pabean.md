# Manajemen Kurs Pabean KMK (erpi_custom_rate)

> **Terakhir diperbarui:** 22 Oktober 2026

Modul **`erpi_custom_rate`** menyediakan kapabilitas penentuan kurs mata uang manual (*Custom Currency Rate*) pada transaksi Pembelian (*Purchase Order*), Penjualan (*Sale Order*), dan Faktur Akuntansi (*Customer Invoices & Vendor Bills*).

---

## Urgensi Kurs Pabean & Pajak (KMK)

Pada kepabeanan Indonesia, transaksi pengadaan impor (dokumen BC 2.0, BC 2.3, BC 2.7) dan penjualan ekspor (BC 3.0) umumnya menggunakan mata uang asing (valas) seperti **USD, EUR, JPY, atau SGD**.

1. **Perbedaan Kurs Operasional vs. Fiskal**:
   * Odoo secara bawaan (*standard*) menggunakan kurs bank atau kurs tengah pasar harian.
   * Direktorat Jenderal Bea dan Cukai (DJBC) dan Direktorat Jenderal Pajak (DJP) mewajibkan penghitungan nilai pabean (*Customs Value*), bea masuk, PPN, dan PPh Pasal 22 mengacu pada **Kurs Keputusan Menteri Keuangan (KMK)** yang diterbitkan mingguan.
2. **Kebutuhan Custom Rate**:
   * Memastikan nilai pembukuan hutang/piutang dan harga pokok persediaan (*inventory valuation*) saat barang masuk ke fasilitas Kawasan Berikat sesuai persis dengan nilai yang tercantum pada dokumen pabean resmi.
   * Menghindari selisih pembulatan kurs antara formulir pabean dan pembukuan akuntansi keuangan perusahaan.

---

## Fitur Utama Modul

* **Use Custom Rate**: Opsi checkbox fleksibel pada setiap dokumen transaksi valas.
* **Custom Rate Input**: Kolom input nilai kurs manual dengan presisi hingga 6 digit desimal (contoh: `16250.000000`).
* **Multi-Dokumen Support**: Sinkronisasi kurs terintegrasi mulai dari:
  * Pengadaan Barang (*Purchase Orders*)
  * Penerimaan Tagihan Pemasok (*Vendor Bills / account.move*)
  * Order Penjualan (*Sale Orders*)
  * Penerbitan Faktur Penjualan (*Customer Invoices / account.move*)

---

## Penerapan pada Dokumen Transaksi

### 1. Modul Pembelian (Purchase Order)

Saat membuat pesanan pembelian valuta asing ke pemasok luar negeri atau kawasan pabean lain:

1. Buka menu **Purchase > Orders > Purchase Orders** dan buat dokumen baru.
2. Pilih pemasok dan tentukan mata uang valas pada kolom **Currency** (misal: `USD`).
3. Pada tab utama atau header transaksi:
   * Centang opsi **Use Custom Rate**.
   * Kolom **Custom Rate** akan terbuka. Masukkan nilai tukar KMK resmi yang berlaku (misal: `16,350.00`).
4. Saat PO dikonfirmasi dan ditarik menjadi faktur tagihan (*Create Bill*), nilai kurs manual ini akan secara otomatis diwariskan ke tagihan pemasok (*Vendor Bill*).

```
+-------------------------------------------------------------+
| Currency: [ USD ]        [x] Use Custom Rate                |
| Custom Rate: [ 16350.000000 ]                               |
+-------------------------------------------------------------+
```

---

### 2. Modul Penjualan (Sale Order)

Pada transaksi penjualan berorientasi ekspor atau penyerahan lokal antar fasilitas kepabeanan:

1. Buka menu **Sales > Orders > Orders** dan buat pesanan baru.
2. Tentukan mata uang valas (*Foreign Currency*).
3. Centang **Use Custom Rate** dan masukkan kurs yang disepakati dalam kontrak dagang atau kurs KMK yang berlaku pada tanggal transaksi.
4. Nilai ekuivalen mata uang dasar perusahaan (*Company Currency / IDR*) akan otomatis terkalkulasi secara proporsional.

---

### 3. Faktur & Pembukuan Akuntansi (Account Move)

Pada modul Akuntansi (*Invoicing / Accounting*):

1. Setiap kali dokumen faktur (*Invoice / Vendor Bill*) dibuat dengan mata uang selain mata uang perusahaan:
   * Sistem mendeteksi kondisi multi-currency (`not_company_currency = True`).
   * Pengguna dapat mencentang opsi **Use Custom Rate** kapan saja sebelum faktur divalidasi (*Posted*).
2. Dampak terhadap jurnal akuntansi:
   * Kolom `currency_rate` dan `invoice_currency_rate` pada baris jurnal (*journal items*) akan di-override menggunakan nilai *Custom Rate*.
   * Nilai debit/kredit dalam rupiah (IDR) dikonversi secara akurat berdasarkan tarif kurs manual tersebut.
   * Laba/rugi selisih kurs yang terealisasi (*Exchange Difference*) saat proses pembayaran (*Payment Reconciliation*) akan dihitung secara presisi dari titik acuan kurs ini.

> [!TIP]
> Jika opsi **Use Custom Rate** tidak dicentang, sistem akan secara otomatis kembali menggunakan kurs default dari tabel mata uang harian Odoo (`res.currency.rate`).
