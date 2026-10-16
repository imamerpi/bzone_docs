# 

# 

# 

# 

# 

# 

# 

# 

# 

# **USER GUIDELINE ODOO 19**

# **B-ZONE**

# **PT EKA REKA PALAKERTI INDONESIA**

# 

## **DAFTAR ISI** {#daftar-isi}

[**DAFTAR ISI**	2](#daftar-isi)

[Modul Sales	3](#heading)

[Modul Purchase	5](#modul-purchase)

[Modul Inventory	7](#modul-inventory)

[Modul Accounting & Finance	12](#modul-accounting-&-finance)

[Modul Manufacturing	18](#modul-manufacturing)

[Modul Laporan Bea Cukai	24](#modul-laporan-bea-cukai)

[Create Produk	33](#create-produk)

[Membuat Master Data Vendor	36](#membuat-master-data-vendor)

[Membuat Master Data Customer	37](#membuat-master-data-customer)

##  {#heading}

## Modul Sales 

1. Login ke Odoo dan buka modul Sales.

2. Kemudian klik tombol “New”.

![][image1]

3. Buat Quotation baru → isi customer, product, quantity dan unit price jika belum ada. Kemudian munculkan kolom “Route” jika belum ada dengan klik icon ![][image2] dan checklist pilihan Route .

4. Setelah kolom “Route” muncul isi pilihan “Make to Order (MTO)”. Lalu klik tombol “Confirm”.

5. Buat Invoice (Klik tombol “Create Invoice”) dari Sales Order.

6. Register Payment dan cek status terbayar (checklist bullet point yang dibutuhkan lalu klik tombol “Create Draft”.

Keterangan :

1. Reguler Invoice			: pembayaran secara reguler.

2. Down Payment (percentage)		: pembayaran di muka dalam bentuk persentase.

3. Down Payment (fixed amount)	: pembayaran di muka dalam jumlah nominal yang telah 

   					  Ditentukan.

7. Maka draft customer invoice telah terbentuk.

## Modul Purchase  {#modul-purchase}

1. Masuk ke modul Purchase.

2. Buat Purchase Request atau Request for Quotation (RFQ) dengan klik tombol “New”.

![][image3]

3. Pilih supplier dan klik tombol “Confirm Order” menjadi Purchase Order (PO).

4. Terima barang (Goods Receipt) dan cek qty di inventory.

5. Tampilan ketika Bagian Gudang telah melakukan penerimaan barang, tombol “Receive Products” otomatis akan hilang. Klik tombol “Create Bill” untuk membuat Bill.

6. Isi tanggal “Bill Date” lalu klik tombol “Confirm”.

7. Maka vendor bill telah terposted.

## Modul Inventory  {#modul-inventory}

1. Untuk melakukan penerimaan Barang, buka modul Inventory. 

2. Lihat struktur lokasi/gudang (Pada gudang “Receipts” klik tombol “ 2 To Receive”).

3. Terima barang dari Purchase (Vendor A), Misal Source Document No P00003.

4. Pada kolom “Destinantion Location” pilih lokasi gudang tempat untuk menerima barang tersebut dan  pada kolom “Custom Document” pilih nomor dokumen yang telah dibuat di Laporan Bea Cukai, kemudian klik tombol “Validate”.

5. Status otomatis menjadi “Done” atau barang telah diterima. 

6. Proses delivery order untuk sales (Outgoing Shipment), pada kolom “Delivery Orders” klik tombol “1 to Deliver”.

7. Klik kolom berikut yang memiliki status “Ready”.

8. Pada kolom “Source Location” pilih lokasi barang tersebut akan dikirimkan dan pilih nomor dokumen pada kolom “Customs Document” yang telah dibuat di Laporan Bea Cukai sebelumnya. Kemudian klik tombol “Validate”.

9. Maka status otomatis menjadi “Done” dan barang telah dikirim.

10. Untuk melakukan pengiriman Barang, buka modul Inventory. Lihat pada kolom “Delivery Orders” klik tombol “1 To Deliver”.

11. Klik kolom berikut yang memiliki status “Ready”.

12. Pastikan “Source Location” dan “Delivery Address” sudah sesuai yang dibutuhkan, kemudian pilih “Customer Document” yang telah dibuat sebelumnya di modul laporan bea cukai. Selanjutnya klik tombol “Validate”.

13. Maka status otomatis menjadi “Done” dan barang berhasil dikirim.

## Modul Accounting & Finance  {#modul-accounting-&-finance}

1. Buka modul Accounting dan lihat Dashboard.

2. Proses invoice penjualan & pembelian.

3. Untuk memproses Vendor Bill Pembelian, klik menu “Vendors” lalu klik submenu “Bills”.

4. Klik kolom Bill yang memiliki status “Not Paid”.

5. Klik tombol “Pay” untuk melakukan pembayaran.

6. Pilih Journal, Payment Method (Metode Pembayaran) yang dibutuhkan dan pastikan Amount sudah sesuai dengan nominal yang dibayar, lalu klik tombol “Create Payment”.

7. Maka status menjadi Paid atau telah dibayarkan kepada vendor.

8. Untuk memproses pembayaran invoice customer, klik menu “Customer” lalu klik submenu “Invoices”.

9. Klik kolom customer invoice yang memiliki status “Draft”.

10. Isi “Invoice Date” lalu klik tombol “Confirm”.

11. Klik tombol “Pay” untuk memproses pembayaran customer.

12. Pilih Journal, Payment Method (Metode Pembayaran) yang dibutuhkan dan pastikan Amount sudah sesuai dengan nominal yang dibayar, lalu klik tombol “Create Payment”.

13. Maka status otomatis menjadi “Paid” atau sudah dibayar oleh customer.

14. Menampilkan laporan laba rugi dan neraca dasar (klik menu “Reporting” lalu klik submenu “Profit and Loss”).

15. Pilih “Start Date” dan “End Date” untuk menarik reporting yan dibutuhkan, lalu klik tombol “Print”.

16. Selanjutnya klik tombol “Continue”.

17. Berikut tampilan Profit and Loss yang berhasil terunduh.

## Modul Manufacturing  {#modul-manufacturing}

1. Buka modul Manufacturing (MRP).

2. Buat Bill of Materials (BoM) untuk produk .

3. Klik tombol “New”.

![][image4]

4. Buat Bill of Material (Isi data seperti contoh di bawah).

1. Product (Isi produk yang akan diproduksi)

2. Quantity (Isi jumlah produk yang akan diproduksi)

3. Reference (Isi reference produk jika diperlukan)

4. BoM Type (Cheklist bullet print tipe BoM yang dibutuhkan)

5. Component ( Pada tab component pilih bahan apa saja yang digunakan untuk membuat produk termasuk jumlah bahan yang dibutuhkan)

6. Selanjutnya klik icon       untuk menyimpan data. 

5. Maka Bill of Materials otomatis terbentuk dan siap digunakan untuk melakukan proses Manufacturing Order.

6. Buat Manufacturing Order (Klik menu “Operation” lalu klik submenu “Manufacturing Orders”).

7. Selanjutnya klik tombol “New”.

![][image5]

8. Lakukan proses Manufacturing Order (Isi data seperti contoh di bawah).

1. Product (pilih produk yang akan diproses produksi)

2. Quantity (Isi jumlah produk yang akan di produksi

3. Bill of Material (Pilih Bill of Material yang telah dibuat sebelumnya, maka component bahan yang digunakan untuk memproduksi produk otomatis akan terisi).

4. Selanjutnya klik tombol “Confirm”.

9. Jalankan proses produksi.

10. Pada kolom quantity isi jumlah produk yang akan diproduksi terlebih dahulu dari keseluruhan jumlah produk yang dibutuhkan, kemudian klik bagian “Transfer”. 

11. Pindahkan bahan produksi dari gudang material ke gudang produksi (pastikan gudang sudah sesuai, lihat kolom source location dan destination location), kemudian klik tombol “Validate”.

12. Setelah itu kembali ke kolom manufacturing order dengan klik bagian “Manufacturing”.

13. Selanjutnya klik tombol “Produce All” untuk memproduksi produk.

14. Maka produk paper bag berhasil terproduksi.

15. Untuk mengecek jumlah produk yang telah diproduksi klik menu “Products” lalu klik submenu “ Products”.

16. Klik produk yang dimaksud (Paper Bag).

17. Lihat pada bagian “On Hand”. Menunjukkan bahwa produk berupa paper bag sejumlah 100 buah telah selesai diproduksi.

## Modul Laporan Bea Cukai  {#modul-laporan-bea-cukai}

1. Buka modul Laporan Bea Cukai

2. Kemudian klik tombol “New”.

![][image6]

3. Buat Nomor Document Laporan Bea Cukai seperti contoh berikut.

1. No Dokumen	: Isi nomor dengan 7 digit angka

2. Tipe		: Pilih tipe dokumen yang dibutuhkan

3. Kabar Code	: Isi kabar code jika diperlukan

4. Tanggal	: Pilih tanggal dokumen yang dibutuhkan

5. Active		: Cheklist checkbox active jika statusnya aktif

6. Partner	: Pilih partner yang dibuatkan nomor dokumen berikut.

7. Selanjutnya klik icon  ![][image7] untuk menyimpan data. 

4. Maka nomor dokumen otomatis terbentuk.

5. Reporting Laporan Pemasukan Barang Per Dokumen Pabean (Klik menu “Reporting” lalu klik submenu “Laporan Pemasukan Barang Per Dokumen Pabean)

6. Pilih “Periode Laporan” yang telah dibuat dan bagian kolom Mulai Tanggal dan Sampai Tanggal akan otomatis terisi, lalu klik tombol “Cetak PDF”.

7. Berikut adalah tampilan Laporan Pemasukan Barang Per Dokumen Pabean yang berhasil terunduh.

8. Reporting Laporan Pengeluaran Barang Per Dokumen Pabean (Klik menu “Reporting” lalu klik submenu “Laporan Pengeluaran Barang Per Dokumen Pabean).

9. Pilih “Periode Laporan” yang telah dibuat dan bagian kolom Mulai Tanggal dan Sampai Tanggal akan otomatis terisi, lalu klik tombol “Cetak PDF”.

10. Berikut adalah tampilan Laporan Pengeluaran Barang yang berhasil terunduh.

11. Reporting Laporan Pertanggungjawaban Mutasi Bahan Baku/Bahan Penolong (Klik menu “Reporting” lalu klik submenu “Laporan Pertanggungjawaban Mutasi Bahan Baku/Bahan”)

12. Pilih “Periode Laporan” yang telah dibuat dan bagian kolom Mulai Tanggal dan Sampai Tanggal akan otomatis terisi, lalu klik tombol “Cetak PDF”.

13. Berikut adalah tampilan Laporan Pertanggungjawaban Mutasi Bahan Baku/Bahan Penolong yang berhasil terunduh.

14. Reporting Laporan Posisi Barang dalam Proses (WIP)

15. Pilih “Periode Laporan” yang telah dibuat dan bagian kolom Mulai Tanggal dan Sampai Tanggal akan otomatis terisi, lalu klik tombol “Cetak PDF”.

16. Berikut adalah tampilan Laporan Posisi Barang dalam Proses (WIP).

17. Reporting Laporan Pertanggungjawaban Mutasi Barang Jadi (Klik menu “Reporting” lalu klik submenu “Laporan Pertanggungjawaban Mutasi Barang Jadi”).

18. Pilih “Periode Laporan” yang telah dibuat dan bagian kolom Mulai Tanggal dan Sampai Tanggal akan otomatis terisi, lalu klik tombol “Cetak PDF”.

19. Berikut adalah tampilan Laporan Pertanggungjawaban Mutasi Barang Jadi.

20. Reporting Laporan Pertanggungjawaban Barang Reject dan Scrap (Klik menu “Reporting” lalu klik submenu “Laporan Pertanggungjawaban Barang Reject dan Scrap”).

21. Pilih “Periode Laporan” yang telah dibuat dan bagian kolom Mulai Tanggal dan Sampai Tanggal akan otomatis terisi, lalu klik tombol “Cetak PDF”.

22. Berikut adalah tampilan Laporan Pertanggungjawaban Barang Reject dan Scrap.

23. Reporting Laporan Pertanggungjawaban Mutasi Mesin dan Peralatan (Klik menu “Reporting” lalu klik submenu “Laporan Pertanggungjawaban Mutasi Mesin dan Peralatan”).

24. Pilih “Periode Laporan” yang telah dibuat dan bagian kolom Mulai Tanggal dan Sampai Tanggal akan otomatis terisi, lalu klik tombol “Cetak PDF”.

25. Berikut adalah tampilan Laporan Pertanggungjawaban Mutasi dan Peralatan.

26. Membuat Periode Laporan (Klik menu “Configuration” lalu klik submenu “Report Period”).

27. Selanjutnya klik tombol “New”.

![][image8]

28. Isi seperti contoh berikut, lalu klik icon      untuk menyimpan gambar. 

## Create Produk {#create-produk}

1. Buka modul Inventory.

## 

2. Klik Menu “Products” lalu klik submenu “Products”.

## 

3. Selanjutnya klik tombol “New”.

![][image9]

4. Akan muncul tampilan sebagai berikut.

## 

5. Isi data seperti contoh di bawah.

1. **Product** (Isi produk yang akan dibuat)

2. Checklist Produk berikut jika :  
1) **Sales** (cheklist bagian checkbox sales jika produk tersebut dapat dijual)  
2) **Purchase** (checklist bagian checkbox sales jika produk tersebut dapat dibeli)  
3) **Can be Used for DJBC Kawasan Berikat** (checkbox akan otomatis tercheklist jika produk tersebut disetting ke dalam kategori bahan baku material (lihat pada kolom Category))

3. Klik tab “**General Information**” dan isi informasi seperti contoh di bawah.  
1) **Product Type** (Klik bullet point goods jika produk tersebut berupa barang)  
2) **Invoicing Policy** (pilih orderes quantity jika kebijakan penagihan invoice produk berdasarkan penerimaan quantity barang)  
3) **Track Inventory** (Pilih produk tersebut akan ditracking berdasarkan apa)  
4) **Valuation by Lot/Serial number** (cheklist checkbox tersebut jika setiap unit produk akan dilacak berdasarkan kelompok atau lot tertentu).  
5) **Sales Price** (Isi harga jual produk)  
6) **Sales Taxes** (Tambahkan ppn jika produk jual dikenakan pajak)  
7) **Cost** (Isi harga beli produk)  
8) **Purchase Taxes** (Tambahkan ppn jika produk beli dikenakan pajak)  
9) **Category** (Pilih kategori produk)  
10) **Reference** (Isi reference produk jika diperlukan)  
11) **Barcode** (Isi kode barcode jika diperlukan)

    ## 

6. Kemudian klik bagian tab “Inventory” lalu isi data sesuai kebutuhan.

1. **Routes**, checklist rute produk jika kondisi produk sebagai berikut.  
1) **Buy** (untuk produk yang bisa dibeli)  
2) **Make to Order (MTO)** ( untuk produk jual yang diproduksi internal)  
3) **Manufacture** (untuk produk yang bisa diproduksi)  
4) **Manufacture WIP** (Jika dicentang, menandakan bahwa produk sedang dalam proses produksi (Work In Progress)).  
5) **Resupply Subcontractor on Order** (untuk bahan material produk subcont)  
6) **Dropship** (untuk produk dropship yang tanpa transit)

2. **Purchase Request** (Jika dicentang, Odoo akan otomatis membuat permintaan pembelian ketika stok produk menipis).  
3. **Weight** (Isi berat produk)  
4. **Volume** (Isi volume produk)  
5. **Customer Lead Time** (Waktu yang dibutuhkan untuk memproses dan mengirimkan produk kepada pelanggan setelah pesanan dibuat)  
6. **HS Code** (Kode yang digunakan untuk mengklasifikasikan produk atau barang untuk tujuan tarif bea cukai internasional).  
7. **Origin of Goods** (Mengidentifikasi asal barang ini dapat memengaruhi tarif dan kebijakan perdagangan, serta memudahkan proses impor dan ekspor barang).

7. Selanjutnya klik “**DJBC Information**” dan beri checklist bagian “**DJBC Raw Material**” jika produk tersebut terdaftar sebagai bahan baku yang masuk dalam kategori **Raw Material** menurut Direktorat Jenderal Bea dan Cukai (DJBC) dan akan diproses sesuai dengan regulasi terkait bahan baku yang perlu diawasi untuk tujuan pajak, impor, atau ekspor. Selanjutnya klik icon     untuk menyimpan data.

8. Maka produk berhasil terbuat.

## Membuat Master Data Vendor {#membuat-master-data-vendor}

1. Buka modul “Purchase”.

2. Kemudian klik menu “Orders” lalu klik submenu “Vendors”.

3. Selanjutnya klik tombol “New”.

![][image10]

4. Isi data seperti contoh berikut.  
1. **Jenis Vendor** (Checklist bullet point menjadi hijau untuk jenis vendor individual atau company)  
2. **Name** (Isi nama Vendor)  
3. **Address** (Isi alamat Vendor tersebut)  
4. **Contact** (Isi contact Vendor seperti email, phone dan data lainnya yang dibutuhkan pada kolom di bawah).  
5. Pada masing-masing tab di bawah (contacts & addresses, Sales & Purchase, Invoicing, Internal Notes), lengkapi data jika diperlukan.  
6. Setelah semua data yang dibutuhkan terisi, klik icon       untuk menyimpan data.

5. Maka data vendor berhasil ditambahkan.

## Membuat Master Data Customer {#membuat-master-data-customer}

1. Buka modul “Sales”.

## 

2. Kemudian klik menu “Orders” lalu klik submenu “Customers”.

## 

3. Isi data seperti contoh berikut.  
1. **Jenis Customer** (Checklist bullet point menjadi hijau untuk jenis customer individual atau company)  
2. **Name** ( Isi nama customer)  
3. **Address** (Isi alamat customer tersebut)  
4. **Contact** (Isi contact customer dan data lainnya yang dibutuhkan pada kolom di bawah).  
5. Pada masing-masing tab di bawah (contacts & addresses, Sales & Purchase, Accounting, Internal Notes), lengkapi data jika diperlukan.  
6. Setelah semua data yang dibutuhkan terisi, klik icon        untuk menyimpan data.

## 

4. Maka data customer berhasil ditambahkan.