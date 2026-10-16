# Siklus Operasional Pengadaan (Purchase)

Siklus pengadaan mencakup tahapan pengajuan permintaan penawaran harga (*Request for Quotation / RFQ*), persetujuan pesanan pembelian (*Purchase Order / PO*), penerimaan fisik material (*Goods Receipt*), serta verifikasi tagihan pemasok (*Vendor Bill*).

---

## 1. Alur Kerja Pengadaan

```text
[ Buat RFQ / Permintaan Pembelian ]
                 │
                 ▼
[ Pilih Pemasok & Negosiasi Harga ]
                 │
                 ▼
[ Konfirmasi Menjadi Purchase Order (PO) ]
                 │
        ┌────────┴────────┐
        ▼                 ▼
[ Penerimaan Barang ]   [ Terbitkan Vendor Bill ]
(Goods Receipt di Gudang)  (Pencatatan Hutang Usaha)
        │                 │
        └────────┬────────┘
                 ▼
      [ Rekonsiliasi & Pembayaran ]
```

---

## 2. Pembuatan Request for Quotation (RFQ)

1. Buka modul **Purchase**.
2. Klik tombol **New** untuk membuat lembar pengadaan baru.
3. Pada kolom **Vendor**, pilih pihak pemasok yang dituju.
4. Tentukan **Order Deadline** dan estimasi tanggal kedatangan barang (**Expected Arrival**).
5. Pada tab **Products**, klik **Add a product** untuk memilih material atau barang yang akan dibeli, lengkapi kuantitas serta harga satuan penawaran.

![Pembuatan Permintaan Penawaran RFQ Baru](../assets/images/purchase_rfq_new.png)
*Gambar 4.1: Formulir pembuatan Request for Quotation (RFQ) pembelian.*

---

## 3. Konfirmasi Menjadi Purchase Order (PO)

1. Setelah harga dan syarat pengiriman disetujui, klik tombol **Confirm Order**.
2. Status dokumen otomatis berubah dari **RFQ** menjadi **Purchase Order (PO)**.
3. Sistem secara otomatis membentuk dokumen penerimaan gudang (*Receipt*) yang terhubung melalui smart button **Receipt** di pojok kanan atas formulir PO.

![Konfirmasi PO dan Penerimaan Barang](../assets/images/purchase_order_confirm.png)
*Gambar 4.2: Tampilan Purchase Order terkonfirmasi dengan tombol penerimaan barang.*

---

## 4. Penerimaan Fisik Barang di Gudang (Goods Receipt)

1. Petugas gudang dapat memproses penerimaan langsung melalui smart button **Receipt** pada PO, atau melalui menu **Inventory → Operations → Receipts**.
2. Verifikasi kuantitas fisik yang tiba dibandingkan kuantitas pesanan.
3. Jika seluruh barang telah diterima dan divalidasi, tombol **Receive Products** pada formulir Purchase Order akan otomatis menghilang dan kuantitas *Received* terisi penuh.

---

## 5. Penerbitan & Konfirmasi Tagihan Pemasok (Create Bill)

Setelah barang fisik tiba dan diperiksa, bagian akuntansi/pembelian mencatat tagihan resmi dari pemasok:

1. Pada lembar Purchase Order, klik tombol **Create Bill**.
2. Sistem akan membuka draf tagihan vendor (*Vendor Bill*).
3. Masukkan tanggal faktur vendor pada kolom **Bill Date**.
4. Masukkan nomor faktur atau referensi surat jalan pemasok pada kolom **Vendor Reference**.
5. Klik tombol **Confirm**.

![Penerbitan dan Konfirmasi Vendor Bill](../assets/images/purchase_vendor_bill.png)
*Gambar 4.3: Pembuatan dan konfirmasi faktur pembelian pemasok (Vendor Bill).*

6. Status tagihan vendor berubah menjadi **Posted** (status pembayaran: *Not Paid*) dan siap dijadwalkan untuk pelunasan pada modul Accounting.

> [!NOTE]
> Nilai hutang usaha (*Accounts Payable*) dan nilai penerimaan persediaan (*Inventory Clearing*) otomatis terbentuk dalam jurnal akuntansi saat Vendor Bill dikonfirmasi.
