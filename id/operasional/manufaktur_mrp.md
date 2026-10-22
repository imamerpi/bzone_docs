# Siklus Manufaktur & Bill of Materials (MRP)

> **Terakhir diperbarui:** 22 Oktober 2026

Modul Manufaktur (*Manufacturing / MRP*) mengatur formula komposisi resep bahan baku (*Bill of Materials*), penerbitan Surat Perintah Kerja / Pesanan Produksi (*Manufacturing Order*), pemindahan material lantai pabrik (*Shop Floor Transfer*), hingga konversi bahan mentah menjadi barang jadi atau barang dalam proses (*Work-in-Progress / WIP*).

---

## 1. Alur Siklus Produksi

```text
[ Susun Bill of Materials (BoM) ]
                 │
                 ▼
[ Buat Manufacturing Order (MO) ]
                 │
                 ▼
[ Konfirmasi MO (Confirm) ]
                 │
                 ▼
[ Transfer Bahan Baku ke Lantai Produksi ]
                 │
                 ▼
[ Eksekusi & Validasi Produksi (Produce All) ]
                 │
                 ▼
[ Mutasi Stok Bahan Baku Berkurang & Barang Jadi Bertambah ]
```

---

## 2. Penyusunan Bill of Materials (BoM)

*Bill of Materials* (BoM) adalah daftar formula komponen atau bahan baku yang dibutuhkan untuk menghasilkan sejumlah unit produk jadi tertentu.

### Langkah-langkah Pembuatan BoM:

1. Buka modul **Manufacturing**.
2. Masuk ke menu **Products → Bills of Materials**.
3. Klik tombol **New**.

![Penyusunan Bill of Materials BoM](../assets/images/mrp_bom_form.png)
*Gambar 6.1: Formulir pembuatan formula Bill of Materials (BoM) produk.*

4. Lengkapi kolom data BoM:
   * **Product**: Pilih barang jadi atau barang setengah jadi yang akan diproduksi.
   * **Quantity**: Tentukan kuantitas acuan produksi (misal: `1.00 Unit` atau `100.00 Unit`).
   * **Reference**: Kode identifikasi formula atau versi BoM (opsional).
   * **BoM Type**: Pilih **Manufacture this product** untuk produksi standar internal.
   * **Tab Components**: Klik **Add a line** untuk memasukkan daftar seluruh material/bahan baku beserta kuantitas kebutuhan per unit acuan.
5. Klik ikon **Simpan** (*Save*) pada pojok kiri atas antarmuka. BoM siap digunakan sebagai referensi otomatis saat perintah produksi dibuat.

---

## 3. Pembuatan & Konfirmasi Manufacturing Order (MO)

*Manufacturing Order* (MO) merupakan instruksi kerja resmi untuk memproduksi sejumlah produk jadi di lini pabrik.

### Langkah-langkah Pembuatan MO:

1. Masuk ke menu **Operations → Manufacturing Orders**.
2. Klik tombol **New**.

![Pembuatan Manufacturing Order MO](../assets/images/mrp_mo_creation.png)
*Gambar 6.2: Formulir pembuatan Manufacturing Order baru.*

3. Isi parameter produksi:
   * **Product**: Pilih produk yang akan diproduksi.
   * **Quantity**: Masukkan target jumlah unit yang akan diproduksi.
   * **Bill of Material**: Pilih formula BoM yang relevan. Sistem secara otomatis akan memuat seluruh daftar komponen bahan baku yang dibutuhkan ke dalam tab *Components*.
4. Klik tombol **Confirm**. Status pesanan berubah menjadi **Confirmed**.

---

## 4. Pemindahan Bahan Baku (Material Transfer)

Untuk menjaga akurasi lokasi persediaan bahan baku fisik dan sistem:

1. Periksa ketersediaan komponen bahan baku pada tabel komponen.
2. Jika gudang bahan baku terpisah dari lokasi lantai produksi (*Production Floor*), lakukan pemindahan material:
   * Buka menu pergerakan transfer atau klik opsi **Transfer** pada pesanan.
   * Pastikan lokasi asal (**Source Location**, misal: `WH/Raw Material`) dan lokasi tujuan (**Destination Location**, misal: `WH/Production Floor`) sudah sesuai.
   * Klik tombol **Validate** untuk memproses perpindahan fisik bahan baku.

![Pemindahan Bahan Baku ke Lantai Produksi](../assets/images/mrp_material_transfer.png)
*Gambar 6.3: Validasi mutasi bahan baku dari gudang penyimpanan ke lantai kerja.*

---

## 5. Penyelesaian Produksi & Pembaruan Stok (Produce All)

1. Kembali ke formulir pesanan kerja dengan mengklik tautan dokumen **Manufacturing Order**.
2. Masukkan kuantitas riil yang berhasil diselesaikan pada kolom kuantitas selesai.
3. Klik tombol **Produce All** pada bagian atas formulir.

![Penyelesaian Produksi dan Verifikasi Stok](../assets/images/mrp_produce_all_done.png)
*Gambar 6.4: Eksekusi Produce All dan pembaruan kuantitas On-Hand stok barang jadi.*

4. Status Manufacturing Order berubah menjadi **Done**:
   * Stok komponen bahan baku otomatis terpotong (*consumed*) dari gudang.
   * Stok barang jadi bertambah ke dalam persediaan (*finished goods*).
5. Verifikasi penambahan stok barang jadi:
   * Buka menu **Products → Products**.
   * Buka kartu produk yang baru saja diproduksi.
   * Field **On Hand** menunjukkan peningkatan jumlah unit produk secara akurat dan *real-time*.

> [!NOTE]
> Transaksi pemakaian bahan baku dan hasil produksi dari *Manufacturing Order* ini secara otomatis menjadi sumber data bagi **Laporan Pertanggungjawaban Mutasi Bahan Baku** serta **Laporan Pertanggungjawaban Mutasi Barang Jadi (WIP & Finished Goods)** pada modul BZone IT Inventory.
