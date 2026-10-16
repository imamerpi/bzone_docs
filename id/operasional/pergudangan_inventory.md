# Penerimaan & Pengiriman Barang (Inventory)

Modul Pergudangan (*Inventory*) mengatur pergerakan fisik seluruh barang keluar-masuk perusahaan. Pada ekosistem BZone, setiap transaksi mutasi gudang juga dapat langsung dihubungkan dengan nomor pendaftaran dokumen pabean resmi Bea Cukai (BC 2.3, BC 4.0, BC 2.5, BC 4.1, atau PEB) sehingga rekonsiliasi data IT Inventory berjalan secara *real-time*.

---

## 1. Alur Manajemen Persediaan

```text
  [ Transaksi Pengadaan (PO) ]              [ Transaksi Penjualan (SO) ]
                │                                         │
                ▼                                         ▼
   [ Penerimaan Barang (Receipts) ]          [ Pengiriman Barang (Deliveries) ]
                │                                         │
                ├─────────────────────────────────────────┤
                ▼                                         ▼
    [ Cek Lokasi Gudang Penyimpanan ]         [ Verifikasi Alamat Kirim ]
                │                                         │
                ├─────────────────────────────────────────┤
                ▼                                         ▼
   [ Pilih Referensi Dokumen Pabean ]        [ Pilih Referensi Dokumen Pabean ]
                │                                         │
                ▼                                         ▼
         [ Klik Validate ]                         [ Klik Validate ]
                │                                         │
                ▼                                         ▼
  (Status Done / Stok Masuk Gudang)         (Status Done / Stok Terkirim)
```

---

## 2. Penerimaan Barang Masuk (Incoming Receipts)

Penerimaan barang digunakan untuk mencatat masuknya bahan baku, material penolong, atau barang jadi dari pihak pemasok ke lokasi gudang perusahaan.

### Langkah-langkah Penerimaan:

1. Buka modul **Inventory**.
2. Pada kartu operasi gudang **Receipts**, klik kartu tugas antrean (misal: **X to Receive**).
3. Buka dokumen penerimaan yang sesuai (dapat dicari berdasarkan nomor *Source Document* Purchase Order, contoh: `P00003`).

![Penerimaan Barang Masuk Receipts](../assets/images/inventory_receipt_incoming.png)
*Gambar 5.1: Formulir penerimaan barang masuk dengan pemilihan lokasi dan dokumen kepabeanan.*

4. Periksa field utama:
   * **Receive From**: Nama vendor pemasok.
   * **Destination Location**: Pastikan lokasi gudang penerimaan sudah tepat (contoh: `WH/Stock` atau `WH/Bahan Baku`).
   * **Customs Document**: Pilih nomor dokumen kepabeanan yang sesuai (misal: nomor BC 2.3 untuk impor atau BC 4.0 untuk pemasukan lokal dari Kawasan Berikat).
5. Pada tabel produk, verifikasi jumlah barang yang dihitung fisik pada kolom **Quantity Done**.
6. Klik tombol **Validate**.
7. Status pergerakan berubah menjadi **Done**. Kuantitas stok otomatis bertambah di buku gudang serta tercatat pada buku penerimaan IT Inventory.

---

## 3. Pengiriman Barang Keluar (Outgoing Delivery Orders)

Pengiriman barang digunakan untuk mendokumentasikan keluarnya barang jadi atau barang dagangan kepada pelanggan (*Customer*).

### Langkah-langkah Pengiriman:

1. Buka modul **Inventory**.
2. Pada kartu operasi gudang **Delivery Orders**, klik antrean pengiriman (misal: **X to Deliver**).
3. Pilih transaksi yang memiliki status **Ready** (menandakan stok barang di gudang telah siap dan dialokasikan).

![Pengiriman Barang Keluar Delivery Orders](../assets/images/inventory_delivery_outgoing.png)
*Gambar 5.2: Formulir pengiriman pesanan penjualan ke pelanggan.*

4. Periksa detail informasi pengiriman:
   * **Delivery Address**: Alamat tujuan pengantaran pelanggan.
   * **Source Location**: Lokasi rak/gudang asal barang dikeluarkan (contoh: `WH/Stock` atau `WH/Barang Jadi`).
   * **Customs Document**: Pilih nomor dokumen pabean pengeluaran resmi (misal: BC 2.5 untuk penjualan lokal Kawasan Berikat atau PEB BC 3.0 untuk ekspor).
5. Pastikan kuantitas fisik yang dimuat ke armada transportasi telah sesuai pada kolom **Quantity Done**.
6. Klik tombol **Validate**.
7. Status dokumen pengiriman berubah menjadi **Done**, stok gudang berkurang, dan mutasi pengeluaran resmi tercatat pada IT Inventory.

> [!IMPORTANT]
> Mengaitkan nomor **Customs Document** saat menekan tombol **Validate** memastikan keabsahan audit jejak digital (*audit trail*) transaksi logistik perusahaan terhadap laporan pabean resmi DJBC.
