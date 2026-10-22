# Keuangan & Akuntansi (Accounting & Finance)

> **Terakhir diperbarui:** 22 Oktober 2026

Modul Keuangan & Akuntansi (*Accounting & Finance*) mengelola pengakuan hutang usaha (*Vendor Bills*), piutang usaha (*Customer Invoices*), eksekusi pembayaran kas/bank, hingga penyusunan laporan keuangan komprehensif seperti Laporan Laba Rugi (*Profit and Loss*).

---

## 1. Alur Finansial Penjualan & Pembelian

```text
[ Tagihan Pembelian (Vendor Bill) ]      [ Tagihan Penjualan (Customer Invoice) ]
                 │                                        │
                 ▼                                        ▼
    [ Status Validasi: Posted ]             [ Konfirmasi Draft Invoice ]
                 │                                        │
                 ▼                                        ▼
    [ Klik Pay / Bayar Tagihan ]            [ Klik Pay / Registrasi Penerimaan ]
                 │                                        │
                 ▼                                        ▼
  (Pilih Bank/Kas & Metode Bayar)         (Pilih Bank/Kas & Metode Bayar)
                 │                                        │
                 ▼                                        ▼
      [ Status Menjadi: Paid ]                [ Status Menjadi: Paid ]
                 │                                        │
                 └───────────────────┬────────────────────┘
                                     ▼
                    [ Laporan Finansial & Audit ]
                    (Profit & Loss / Balance Sheet)
```

---

## 2. Pemrosesan Pembayaran Tagihan Pemasok (Vendor Bills)

Pelunasan tagihan atas pembelian material atau jasa yang telah diterima:

1. Buka modul **Accounting**.
2. Masuk ke menu **Vendors → Bills**.
3. Pilih baris tagihan vendor yang memiliki status pembayaran **Not Paid**.
4. Klik tombol **Pay** (atau **Register Payment**) pada bagian atas formulir tagihan.

![Pembayaran Tagihan Vendor Bill](../assets/images/accounting_vendor_bill_payment.png)
*Gambar 7.1: Formulir eksekusi pembayaran hutang usaha kepada vendor.*

5. Pada jendela registrasi pembayaran:
   * **Journal**: Pilih akun kas atau rekening bank yang digunakan untuk transfer dana.
   * **Payment Method**: Tentukan instrumen pembayaran (*Bank Transfer*, *Manual*, *Cheque*).
   * **Amount**: Pastikan nominal pembayaran telah sesuai dengan jumlah tagihan (atau nominal termin pembayaran).
   * **Payment Date**: Masukkan tanggal pencairan dana.
6. Klik tombol **Create Payment**.
7. Status tagihan otomatis berubah menjadi **In Payment** atau **Paid**.

---

## 3. Konfirmasi & Pelunasan Faktur Penjualan (Customer Invoices)

Penatausahaan penagihan piutang dan penerimaan kas dari pelanggan:

1. Buka modul **Accounting**.
2. Masuk ke menu **Customers → Invoices**.
3. Pilih faktur pelanggan yang masih berada dalam status draf (**Draft**).
4. Pastikan data tanggal penagihan (**Invoice Date**) dan jatuh tempo (**Due Date**) telah terisi dengan benar.
5. Klik tombol **Confirm** untuk memvalidasi faktur menjadi piutang resmi.
6. Saat pelanggan mentransfer pembayaran, klik tombol **Pay** (atau **Register Payment**).

![Konfirmasi dan Pelunasan Faktur Pelanggan](../assets/images/accounting_customer_invoice_confirm.png)
*Gambar 7.2: Formulir konfirmasi faktur penjualan dan registrasi penerimaan pembayaran.*

7. Pilih **Journal** bank penerima, metode pembayaran, dan nominal dana yang masuk.
8. Klik tombol **Create Payment**.
9. Status faktur terkonfirmasi berubah menjadi **In Payment** / **Paid**.

---

## 4. Penarikan Laporan Keuangan (Profit and Loss)

Laporan Laba Rugi (*Profit and Loss / Income Statement*) menyajikan ringkasan pendapatan, harga pokok penjualan (HPP), dan beban operasional perusahaan dalam periode tertentu.

### Langkah-langkah Menarik Laporan:

1. Masuk ke menu **Reporting → Profit and Loss**.
2. Tentukan periode waktu evaluasi:
   * Tentukan tanggal mulai (**Start Date**) dan tanggal akhir (**End Date**).
   * Pilih opsi pembanding (*Comparison*) jika ingin menyandingkan dengan periode bulan atau tahun sebelumnya.
3. Klik tombol **Print** untuk mengunduh laporan dalam format dokumen PDF resmi, atau pilih **Export (XLSX)** untuk analisis spreadsheet.

![Penarikan Laporan Laba Rugi Profit and Loss](../assets/images/accounting_profit_loss_report.png)
*Gambar 7.3: Antarmuka laporan laba rugi dengan filter periode tanggal.*

4. Klik tombol **Continue** pada dialog konfirmasi jika diperlukan.
5. Dokumen laporan keuangan siap digunakan untuk evaluasi manajemen maupun lampiran audit fiskal.
