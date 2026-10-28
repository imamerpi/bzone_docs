# Master Data Produk & Rute Logistik

Master data produk merupakan fondasi utama operasional ERP dan pelaporan kepabeanan BZone IT Inventory. Setiap barang yang disimpan, diproduksi, dibeli, atau dijual wajib memiliki master data dengan parameter akuntansi, rute logistik, dan klasifikasi kepabeanan yang tepat.

---

## 1. Pembuatan Produk Baru

Untuk membuat data master produk baru pada sistem:

1. Buka modul **Inventory** atau **Sales** / **Purchase**.

![Tampilan Menu Utama Odoo](/assets/images/main_menu.png)
*Gambar 1.1: Menu Utama Odoo 20.*

2. Masuk ke menu **Products → Products**.
3. Klik tombol **New** pada pojok kiri atas antarmuka.

![Tampilan Formulir Master Produk Baru](/assets/images/master_produk_general.png)
*Gambar 1.2: Formulir pembuatan master data produk baru pada Odoo 20.*

---

## 2. Pengaturan Tab General Information

Pada bagian utama dan tab **General Information**, lengkapi parameter berikut:

| Parameter / Field | Deskripsi & Nilai Rekomendasi |
| :--- | :--- |
| **Product Name** | Nama lengkap atau deskripsi resmi barang. |
| **Can be Sold** | Centang jika produk merupakan barang komersial yang dijual ke *Customer*. |
| **Can be Purchased** | Centang jika produk diadakan melalui pembelian ke *Vendor*. |
| **Can be Used for DJBC** | Otomatis tercentang bila produk diklasifikasikan untuk kepatuhan Bea Cukai. |
| **Product Type** | Pilih **Goods** (Storable) untuk barang fisik yang dilacak kuantitas stoknya.|
|| Pilih **Service** untuk tipe produk seperti jasa. |
| **Invoicing Policy** | Pilih **Ordered quantities** atau **Delivered quantities** sesuai kebijakan penagihan. |
||**Ordered quantities**: Penagihan bisa dilakukan berdasarkan jumlah barang yang dipesan atau Sales Order sudah di konfirm. |
||**Delivered quantities**: Penagihan bisa dilakukan ketika barang sudah dikirim. |
| **Track Inventory** | Tentukan metode pelacakan: *By Unique Serial Number*, *By Lots*, atau *No Tracking*. |
| **Sales Price** | Harga jual dasar satuan barang (sebelum pajak). |
| **Customer Taxes** | Tarif Pajak Pertambahan Nilai (PPN) keluaran yang berlaku. |
| **Cost** | Nilai perolehan atau estimasi biaya standar barang atau HPP. |
| **Product Category** | Kategori barang (menentukan metode penilaian persediaan dan akun akuntansi). |
| **Internal Reference** | Kode part number internal (SKU) perusahaan. |
| **Barcode** | Kode batang atau EAN untuk pemindaian scanner barcode. |


> [!NOTE]
> Pemilihan **Product Category** secara otomatis menentukan pemetaan akun persediaan (*Inventory Valuation*) serta default klasifikasi IT Inventory DJBC (Bahan Baku, Barang Jadi, atau Barang Penolong).

---

## 3. Konfigurasi Rute Logistik (Tab Inventory)

Rute (*Routes*) menentukan bagaimana sistem memicu pengadaan dan pergerakan stok ketika ada kebutuhan barang.

1. Buka tab **Inventory**.
2. Pada bagian **Routes**, aktifkan opsi rute yang sesuai:

Jika merupakan **Barang Jadi/Finish Good** maka perlu dicentang: **Make to Order (MTO)** dan **Manufacture**.

![Konfigurasi Rute Logistik Produk](../assets/images/master_produk_routes.png)
*Gambar 1.3: Pemilihan rute logistik (Buy, MTO, Manufacture) pada tab Inventory.*

Jika merupakan **Barang Dalam Proses/Work in Progress** maka perlu dicentang: **Manufacture WIP**.

![Konfigurasi Rute Logistik Produk](../assets/images/barang_wip.png)
*Gambar 1.4: Pemilihan rute logistik (Manufacture WIP) pada tab Inventory.*

Jika merupakan **Bahan Baku/Raw Material** maka perlu dicentang: **Buy**, **Purchase Request**, dan **Make to Order (MTO)**

![Konfigurasi Rute Logistik Produk](../assets/images/barang_raw_material.png)
*Gambar 1.5: Pemilihan rute logistik (Buy, Purchase Request, MTO) pada tab Inventory.*

### Penjelasan Rute Operasional:

* **Buy**: Menginstruksikan sistem untuk otomatis membuat draft *Purchase Order* / RFQ saat persediaan menipis di bawah titik *reordering rule*.
* **Purchase Request**: Opsi otomatisasi pengajuan formulir permintaan pembelian internal saat persediaan mencapai batas minimum.
* **Make to Order (MTO)**: Menginstruksikan sistem untuk tidak mengambil dari stok gudang reguler, melainkan otomatis membuat *Purchase Order* atau *Manufacturing Order* khusus setiap kali *Sales Order* dikonfirmasi.
* **Manufacture**: Menandakan produk diproduksi secara internal di pabrik melalui pesanan produksi (*Manufacturing Order*).
* **Manufacture WIP**: Menandai barang setengah jadi (*Work-in-Progress*) yang melewati tahapan proses produksi antara.
* **Resupply Subcontractor on Order**: Digunakan jika bahan baku dikirimkan ke pihak subkontraktor untuk proses maklon.
* **Dropship**: Pengiriman barang dari pemasok langsung ke pembeli akhir tanpa transit fisik di gudang internal.


### Parameter Fisik & Logistik Lainnya:
* **Weight & Volume**: Berat kotor/bersih dan volume fisik barang untuk perhitungan manifes logistik dan dokumen pabean.
* **Customer Lead Time**: Estimasi hari kerja yang dibutuhkan dari konfirmasi pesanan hingga barang siap dikirim.


---

## 4. Parameter Kepabeanan (Tab DJBC Information)

Khusus perusahaan dengan fasilitas Tempat Penimbunan Berikat (Kawasan Berikat, KITE, atau KEK), tab kepabeanan wajib dilengkapi:


1. Masuk ke tab **DJBC Information**.
Ada beberapa opsi untuk menentukan barang tersebut akan dilaporkan kemana, yaitu:
* **DJBC Raw Material**: Centang opsi ini jika produk merupakan bahan baku impor/lokal yang wajib dilaporkan pada Laporan Pertanggungjawaban Mutasi Bahan Baku (IT Inventory).
* **DJBC Finished Good**: Centang opsi ini jika produk merupakan barang jadi impor/lokal yang wajib dilaporkan pada Laporan Pertanggungjawaban Mutasi Barang Jadi (IT Inventory).
* **DJBC Scrap**: Centang opsi ini jika produk merupakan barang sisa impor/lokal yang wajib dilaporkan pada Laporan Pertanggungjawaban Mutasi Barang Sisa (IT Inventory).
* **DJBC WIP**: Centang opsi ini jika produk merupakan barang setengah jadi impor/lokal yang wajib dilaporkan pada Laporan Pertanggungjawaban Mutasi Barang Setengah Jadi (IT Inventory).
* **DJBC Mesin**: Centang opsi ini jika produk merupakan mesin impor/lokal yang wajib dilaporkan pada Laporan Pertanggungjawaban Mesin (IT Inventory).

![Pengaturan Parameter DJBC](../assets/images/master_produk_djbc.png)
*Gambar 1.6: Konfigurasi klasifikasi DJBC dan HS Code pada formulir produk.*

3. **HS Code**: Masukkan 8 digit kode *Harmonized System* resmi untuk keperluan penetapan pos tarif pabean dan validasi integrasi CEISA.
4. **Origin of Goods**: Tentukan negara asal barang (*Country of Origin*) untuk dokumen deklarasi pabean impor/ekspor.
5. Klik ikon **Simpan** (*Save*) pada pojok kiri atas untuk menyimpan master produk.

> [!IMPORTANT]
> Konsistensi pengisian **HS Code** dan **DJBC Classification** sangat menentukan validitas data pada saat pengiriman dokumen ke portal CEISA 4.0 serta pelaporan 7 Laporan Mutasi IT Inventory.
