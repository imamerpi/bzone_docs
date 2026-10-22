# Siklus Operasional Penjualan (Sales)

> **Terakhir diperbarui:** 22 Oktober 2026

Siklus penjualan mencakup tahapan penawaran harga (*Quotation*), pemesanan penjualan (*Sales Order*), otomasi produksi berdasarkan pesanan (*Make to Order / MTO*), hingga penerbitan faktur tagihan pelanggan (*Customer Invoice*).

---

## 1. Alur Kerja Penjualan

```text
[ Buat Penawaran (Quotation) ]
              │
              ▼
[ Atur Rute MTO / Stok Standar ]
              │
              ▼
[ Konfirmasi Menjadi Sales Order (SO) ]
              │
              ├──────────────────────────────┐
              ▼                              ▼
  [ Memicu Produksi / Pengadaan ]    [ Buat Invoice (DP / Reguler) ]
              │                              │
              ▼                              ▼
  [ Pengiriman Barang (DO) ]         [ Registrasi Pembayaran ]
```

---

## 2. Pembuatan Quotation & Pengaturan Rute MTO

1. Buka modul **Sales**.
2. Klik tombol **New** untuk membuat lembar penawaran harga baru.
3. Pilih nama pelanggan pada kolom **Customer**.
4. Pada tab **Order Lines**, klik **Add a product** lalu pilih barang yang dipesan beserta kuantitas (*Quantity*) dan harga satuan (*Unit Price*).

![Pembuatan Quotation dan Pengaturan Rute MTO](../assets/images/sales_quotation_mto.png)
*Gambar 3.1: Penginputan quotation baru dengan pemilihan rute Make to Order (MTO).*

5. Untuk pesanan barang yang memerlukan produksi khusus (tanpa mengambil stok bebas di gudang):
   * Munculkan kolom **Route** jika belum terlihat dengan mengklik ikon *tiga titik vertikal / slider opsi kolom* di sebelah kanan header baris produk, lalu centang **Route**.
   * Pada kolom **Route**, pilih opsi **Make to Order (MTO)**.

> [!TIP]
> Dengan memilih rute **Make to Order (MTO)**, sistem secara otomatis akan menerbitkan *Manufacturing Order* (jika rute produk adalah *Manufacture*) atau draft *Purchase Order* (jika rute produk adalah *Buy*) segera setelah Sales Order dikonfirmasi.

---

## 3. Konfirmasi Sales Order

1. Periksa kembali total nilai pesanan, pajak (PPN), dan alamat pengiriman (*Delivery Address*).
2. Klik tombol **Confirm** pada pojok kiri atas.
3. Status dokumen akan berubah dari **Quotation** menjadi **Sales Order**.
4. Dokumen pengiriman barang (*Delivery Order*) otomatis terbentuk dan dapat diakses melalui smart button **Delivery** di bagian atas formulir.

---

## 4. Penerbitan Faktur Penjualan (Create Invoice)

Setelah Sales Order terkonfirmasi, bagian penagihan dapat menerbitkan tagihan kepada pelanggan:

1. Pada lembar Sales Order, klik tombol **Create Invoice**.
2. Pada jendela popup yang muncul, pilih tipe tagihan yang diinginkan:

| Tipe Tagihan | Deskripsi Penggunaan |
| :--- | :--- |
| **Regular invoice** | Penagihan penuh sesuai kuantitas barang yang dipesan atau dikirim. |
| **Down payment (percentage)** | Penagihan uang muka berdasarkan persentase (misal: 30% dari total nilai pesanan). |
| **Down payment (fixed amount)** | Penagihan uang muka dalam bentuk nominal tetap yang telah disepakati. |

![Konfirmasi Sales Order dan Penerbitan Faktur](../assets/images/sales_order_confirmed.png)
*Gambar 3.2: Opsi penerbitan Customer Invoice (Uang Muka / Reguler).*

3. Klik tombol **Create Draft** untuk membentuk draf faktur pelanggan (*Customer Invoice*).
4. Masuk ke draf faktur, pastikan tanggal faktur (*Invoice Date*) telah terisi, kemudian klik tombol **Confirm**.

---

## 5. Pencatatan Pembayaran Pelanggan (Register Payment)

1. Pada formulir Customer Invoice yang telah berstatus **Posted**:
2. Klik tombol **Register Payment**.

![Pencatatan Pembayaran Tagihan Penjualan](../assets/images/sales_register_payment.png)
*Gambar 3.3: Formulir registrasi pembayaran dari pelanggan.*

3. Lengkapi formulir registrasi pembayaran:
   * **Journal**: Pilih rekening bank atau kas penerima pembayaran.
   * **Payment Method**: Pilih metode pembayaran (*Manual*, *Bank Transfer*, dsb.).
   * **Amount**: Pastikan nominal sesuai dengan dana yang diterima.
   * **Payment Date**: Tanggal mutasi kas/bank masuk.
4. Klik tombol **Create Payment**.
5. Status faktur otomatis diperbarui menjadi **In Payment** atau **Paid**.
