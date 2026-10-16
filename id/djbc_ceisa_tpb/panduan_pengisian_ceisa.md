# Panduan Pengisian Formulir CEISA 4.0 untuk TPB (djbc_ceisa_tpb)

Dokumen ini adalah **panduan pengguna (User Guide)** bagi tim Operasional Exim, Logistik, PPJK, dan Staf Kepabeanan Perusahaan dalam mengisi setiap tab pada formulir dokumen kepabeanan CEISA 4.0 untuk fasilitas **Tempat Penimbunan Berikat (TPB)**. Panduan ini bersifat non-teknis dan disusun berdasarkan praktik terbaik kepabeanan Indonesia.

---

## 1. Aturan Dasar & Prinsip Penting Pengisian

Sebelum menekan tombol **Confirm** pada dokumen TPB, pahami dulu tiga aturan emas yang wajib dipatuhi agar dokumen tidak ditolak (*reject*) oleh server Bea Cukai.

### 1.1 Nilai CIF dan Nilai Barang Tidak Boleh Nol (0)

Server CEISA akan otomatis menolak dokumen yang memiliki **nilai CIF = 0** atau **nilai barang per item = 0**, karena dianggap tidak logis secara komersial dan melanggar asas *transaction value* pada **UU Kepabeanan No. 17 Tahun 2006**. Bea Cukai beranggapan bahwa setiap barang yang berpindah di kawasan pabean pasti memiliki nilai ekonomis, sekecil apa pun.

### 1.2 Penanganan Barang Sampel, Hadiah, Garansi, dan Free of Charge (FOC)

Untuk kondisi khusus seperti barang sampel gratis, hadiah promosi, barang garansi (replacement), atau pengiriman **Free of Charge (FOC)**, dilarang mengosongkan nilai atau menulis angka 0. Solusinya:

* Gunakan **Proforma Invoice** dengan judul tegas: **"Value for Customs Purposes Only"** atau **"Harga Patokan Bea Masuk"**.
* Tetapkan nilai referensi yang wajar berdasarkan **harga pasar**, **invoice sebelumnya**, atau **data harga produksi**.
* Cantumkan **keterangan "FOC"** atau **"Gratis"** pada kolom informasi tambahan agar auditor Bea Cukai memahami konteksnya.

### 1.3 Penggunaan Kurs Pajak Mingguan (NDPBM KMK)

Seluruh perhitungan nilai pabean dalam Rupiah **wajib** menggunakan **Kurs Pajak** yang ditetapkan Menteri Keuangan setiap minggu (lazim disebut **NDPBM KMK**), bukan kurs jual bank atau kurs transaksi komersial.

> [!TIP]
> Kurs yang digunakan adalah kurs pada **tanggal pendaftaran dokumen** di CEISA. Pastikan kurs mingguan telah di-*update* pada sistem BZone melalui menu **Kurs Pajak Pabean** sebelum membuat dokumen TPB baru.

---

## 2. Tata Cara Pengisian Formulir Langkah-demi-Langkah

Buka modul **CEISA Connector > Pabean > TPB**, lalu pilih jenis dokumen yang akan dibuat. Ikuti setiap langkah di bawah ini secara berurutan.

### Langkah 1: Tab Header Dokumen

Tab ini adalah identitas utama dokumen pabean Anda.

* **Nomor Aju 26 Digit**: Dibentuk otomatis oleh sistem saat Anda menyimpan draft.
* **Kantor Bea Cukai Pemantauan**: Pilih sesuai izin TPB (contoh: `040300` untuk Kantor Pengawasan Kawasan Berikat).
* **Jenis Dokumen Pabean**: Pilih sesuai tujuan transaksi:

  ![Pilihan Jenis Dokumen BC](/assets/images/ceisa_tpb_header.png)

  | Kode BC | Peruntukan Singkat |
  |---|---|
  | **BC 2.3** | Pemasukan barang impor ke TPB (bahan baku / barang modal dari luar negeri). |
  | **BC 2.5** | Penjualan / penyerahan barang jadi dari TPB ke pasar lokal. |
  | **BC 4.0** | Pembelian lokal barang ke TPB dengan PPN tidak dipungut. |
  | **BC 4.1** | Pengembalian / penyerahan barang dari TPB ke TLDDP. |
  | **BC 2.7** | Pemindahan barang antar TPB. |
  | **BC 2.6.1 / 2.6.2** | Pengeluaran / pengembalian subkontrak. |

### Langkah 2: Tab Entitas

Entitas adalah pihak-pihak yang terlibat dalam transaksi. Sistem akan mengurutkan prioritas otomatis, tetapi pastikan data berikut benar:

* **Pengusaha TPB (Seri 1)**: Wajib diisi dan otomatis menjadi entitas utama. NPWP 16 digit, NIB, dan nomor izin TPB harus valid.
* **Pemilik Barang**: Pihak yang memiliki secara hukum atas barang (bisa sama dengan Pengusaha TPB).
* **Penerima / Consignee**: Pihak yang menerima barang di tujuan.
* **Pengirim / Shipper**: Pemasok di luar negeri (untuk impor) atau di TLDDP (untuk BC 4.0/4.1).
* **Jenis API**: Pilih **API-P** (Pemberitahuan Impor Barang) untuk impor umum, atau sesuai dokumen izin yang diterbitkan KPPBC.

  ![Form Entitas Pabean](/assets/images/ceisa_tpb_entitas.png)

### Langkah 3: Tab Dokumen Pelengkap

Tab ini memuat dokumen pendukung transaksi. **Urutan seri sangat penting** karena server CEISA memvalidasi berdasarkan prioritas:

1. **Invoice (Faktur Dagang)** - **Wajib Seri 1**. Untuk proforma, gunakan tipe dokumen `PROFORMA INVOICE`.
2. **Bill of Lading (B/L) atau Surat Jalan / Air Waybill (AWB)** - **Wajib Seri 2** untuk barang yang melewati pelabuhan/bandara.
3. **Dokumen Pendukung Lainnya** - Disusul setelah Seri 2, misalnya: Packing List, SKA (Surat Keterangan Asal / Form E / Form D), Kontrak, dan Daftar Suku Cadang.

  ![Urutan Dokumen Pelengkap](/assets/images/ceisa_tpb_dokumen_pelengkap.png)

> [!IMPORTANT]
> Jika Invoice dan B/L/AWB tidak berada di Seri 1 dan Seri 2, dokumen akan ditolak otomatis oleh sistem Bea Cukai.

### Langkah 4: Tab Pengangkut

Mengisi data pergerakan fisik barang dari titik asal ke titik tujuan.

* **Moda Angkut**: Pilih sesuai moda: Laut / Udara / Darat.
* **Nama Sarana Pengangkut**: Nama kapal / nomor penerbangan / nomor polisi kendaraan.
* **Kode bendera negara**: Gunakan kode 2 huruf sesuai standar **ISO Alpha-2 Capital**, contoh: `ID` (Indonesia), `SG` (Singapura), `CN` (Tiongkok), `JP` (Jepang).
* **Nomor Manifes BC 1.1**: Masukkan nomor manifes dari perusahaan pelayaran atau agen forwarder.

  ![Data Sarana Pengangkut](/assets/images/ceisa_tpb_pengangkut.png)

### Langkah 5: Tab Kemasan & Kontainer

Mengisi data pengemasan fisik barang.

* **Jumlah dan Jenis Kemasan**: Pilih kode kemasan sesuai satuan baku CEISA, misalnya: `CTN` (Carton), `PK` (Pack), `PLT` (Pallet), `BG` (Bag), `DR` (Drum).
* **Jumlah per Jenis**: Isi kuantitas kemasan, contoh: `10` untuk `10 Carton`.
* **Nomor Kontainer**: Masukkan **11 digit nomor kontainer** sesuai yang tercetak fisik di peti kemas (4 huruf prefix + 7 digit nomor). Cantumkan juga ukuran dan jenis kontainer (20ft, 40ft, 45ft).

  ![Detil Kemasan & Kontainer](/assets/images/ceisa_tpb_kemasan.png)

### Langkah 6: Tab Transaksi & Keuangan

Tab ini adalah **jantung perhitungan pabean** karena menjadi dasar pengenaan Bea Masuk dan pajak.

* **Kode Valuta**: Pilih kode mata uang asing sesuai invoice, contoh: `USD`, `CNY`, `EUR`, `JPY`.
* **Nilai FOB (Free on Board)**: Nilai barang di pelabuhan muat.
* **Ongkos Kirim (Freight)**: Biaya pengangkutan barang dari gudang asal ke pelabuhan tujuan.
* **Asuransi (Insurance)**: Premi asuransi pengiriman barang.
* **Nilai CIF**: Akan terhitung otomatis oleh sistem.
* **Harga Penyerahan**: Nilai penyerahan dalam Rupiah untuk transaksi lokal (BC 4.0/4.1/2.5).
* **Tanggal dan Kurs KMK**: Akan terisi otomatis sesuai tanggal dokumen dan master kurs pajak.

  ![Tab Keuangan dan CIF](/assets/images/ceisa_tpb_keuangan.png)

### Langkah 7: Tab Detail Barang & Bahan Baku

Mengisi rincian barang per item secara detail.

* **HS Code 8 Digit**: Pilih sesuai **BTKI** (Buku Tarif Kepabeanan Indonesia), contoh: `8473.30.90` untuk part komputer.
* **Deskripsi Barang**: Tulis deskripsi yang jelas dan sesuai fisik barang agar pemeriksaan dapat berjalan lancar.
* **Satuan CEISA**: Pilih dari kode satuan baku, contoh: `PCE` (Piece), `KGM` (Kilogram), `MTR` (Meter), `LTR` (Liter).
* **Kuantitas & Nilai**: Isi qty barang dan nilai CIF per item.
* **Konversi Bahan Baku (BC 2.5)**: Untuk penjualan lokal barang jadi yang diolah dari bahan baku impor eks **BC 2.3**, sistem akan otomatis menarik **History Penggunaan Bahan Baku** sebagai dasar penghitungan Bea Masuk dan tarif.

  ![Detail Barang dan Tarif](/assets/images/ceisa_tpb_barang.png)

---

## 3. Cara & Rumus Perhitungan Pabean

Bagian ini menjelaskan rumus sederhana yang dapat Anda gunakan untuk **mengecek sendiri** nilai pungutan sebelum dokumen dikirim.

### 3.1 Rumus Dasar Perhitungan

| No | Komponen | Rumus |
|---|---|---|
| 1 | Nilai CIF (Valas) | **CIF = FOB + Freight + Asuransi** |
| 2 | Nilai CIF (Rupiah) | **CIF IDR = CIF Valas x Kurs Pajak KMK** |
| 3 | Bea Masuk (BM) | **BM = Tarif % x CIF Rupiah** |
| 4 | Nilai Impor (Dasar Pajak) | **Dasar Pajak = CIF Rupiah + Bea Masuk** |
| 5 | PPN Impor (11%) | **PPN = 11% x Dasar Pajak** |
| 6 | PPh Pasal 22 | **PPh 22 = 2,5% x Dasar Pajak** (jika memiliki API) atau **7,5% x Dasar Pajak** (jika non-API) |

> [!TIP]
> Nilai **Dasar Pajak** inilah yang menjadi dasar pengenaan PPN dan PPh Pasal 22. Pastikan seluruh komponen di atasnya telah benar.

### 3.2 Studi Kasus Simulasi Perhitungan

Berikut contoh nyata agar lebih mudah dipahami oleh staf operasional.

**Data Importasi:**

* Barang: 100 PCE Part Komputer
* HS Code: `8473.30.90`, Bea Masuk: `5%`
* FOB: USD `2.000`
* Freight: USD `100`
* Asuransi: USD `50`
* Kurs KMK: `Rp 16.000 / USD`

**Langkah Perhitungan:**

1. CIF Valas = `2.000 + 100 + 50` = **USD 2.150**
2. CIF Rupiah = `2.150 x 16.000` = **Rp 34.400.000**
3. Bea Masuk = `5% x 34.400.000` = **Rp 1.720.000**
4. Dasar Pajak = `34.400.000 + 1.720.000` = **Rp 36.120.000**
5. PPN 11% = `11% x 36.120.000` = **Rp 3.973.200**
6. PPh Pasal 22 (API) = `2,5% x 36.120.000` = **Rp 903.000**
7. **Total Bayar Tunai = Rp 1.720.000 + Rp 3.973.200 + Rp 903.000 = Rp 6.596.200**

> [!IMPORTANT]
> Pada **BC 2.3 untuk Kawasan Berikat**, Bea Masuk, PPN, dan PPh Pasal 22 seluruhnya **ditangguhkan dan tidak dipungut** karena fasilitas Kawasan Berikat. Total bayar tunai pada dokumen BC 2.3 = **Rp 0**, tetapi kewajibannya tetap tercatat di IT Inventory dan wajib direalisasikan melalui ekspor atau penjualan lokal BC 2.5.

### 3.3 Ketentuan Khusus Perhitungan Dokumen BC 2.5 (Pengeluaran ke Lokal / TLDDP)

Dokumen **BC 2.5** digunakan saat barang hasil olahan atau barang modal dari TPB (Kawasan Berikat) dikeluarkan ke pasar domestik (TLDDP). Karena fasilitas penangguhan berakhir, perusahaan wajib melunasi Bea Masuk dan Pajak (PPN, PPh). Berdasarkan **PMK 131/PMK.04/2018 jo PMK 65/PMK.04/2021 Pasal 30**, terdapat aturan khusus:

#### 1. Perbedaan Opsi Cara Perhitungan (`kode_perhitungan`)
Sistem menyediakan field **Perhitungan (`kode_perhitungan`)** pada item barang:

* **Opsi 1: Harga Pengeluaran (Kode `1`)**
  * **Dasar Bea Masuk (DPP BM)**: Menggunakan **Harga Penyerahan (Harga Jual)** barang jadi.
  * **Pos Tarif / HS Code**: Menggunakan HS Code **Barang Jadi**.
  * **Rumus**:
    * $\text{Bea Masuk} = \text{Harga Penyerahan} \times \text{Tarif BM Barang Jadi}\%$
    * $\text{PPN (11%)} = \text{Harga Penyerahan} \times 11\%$
    * $\text{PPh Pasal 22} = (\text{Harga Penyerahan} + \text{Bea Masuk}) \times \text{Tarif PPh}\%$
  * **Total Bayar**: $\text{Nilai BM} + \text{Nilai PPN} + \text{Nilai PPh}$.

* **Opsi 0: Harga Pemasukan (Kode `0`)**
  * **Prinsip**: Tarif dan nilai pungutan merefer langsung dari rincian bahan baku yang digunakan pada **Tab Bahan Baku (`bahan_baku_ids`)**.
  * Apabila barang jadi menggunakan kombinasi bahan baku impor (ex-BC 2.3) dan bahan baku lokal (ex-BC 4.0), maka pada tab **Barang Tarif** akan muncul **4 jenis pungutan**:
    1. **3 Jenis Pungutan milik Bahan Baku Impor (ex-BC 2.3)**:
       * **a. BM (Bea Masuk)**: $\sum (\text{CIF Rupiah BB Impor} \times \text{Tarif BM BB Impor}\%)$
       * **b. PPH (PPh Impor)**: $(\sum \text{CIF Rupiah BB Impor} + \text{Nilai BM}) \times \text{Tarif PPh}\%$ (Importir API: 2,5%, Non-API: 7,5%)
       * **c. PPN (PPN Impor)**: $(\sum \text{CIF Rupiah BB Impor} + \text{Nilai BM}) \times 11\%$
    2. **1 Jenis Pungutan milik Bahan Baku Lokal (ex-BC 4.0)**:
       * **d. PPN Lokal (`PPNLOKAL`)**: $\sum (\text{Harga BB Lokal}) \times 11\%$
       * *(Bahan baku lokal tidak dikenakan BM dan PPh Pasal 22)*.
  * **Di Tabel Pungutan Dokumen Header**: Sistem otomatis merangkum keempat pungutan tersebut sebagai pungutan yang wajib dibayar (Fasilitas `1` DIBAYAR).

#### 2. Perlakuan Barang Non-Impor / Asal Murni Lokal
* Apabila barang jadi diolah murni dari bahan baku lokal (asal BC 4.0 / `kode_asal_bahan_baku = 1`):
  * **Bea Masuk = 0% (Rp 0)**
  * **PPh Pasal 22 Impor = 0% (Rp 0)**
  * **Hanya dikenakan PPN Lokal 11%**: $\text{PPN} = \text{Harga Penyerahan} \times 11\%$.

#### 3. Ketentuan PPh Pasal 22 Impor & Alasan Respon API CEISA Tidak Ada PPH
* **Mengapa Tarikan API CEISA (`ceisa.tarif`) Hanya Ada BM dan PPN?**
  * Endpoint OpenAPI CEISA `/openapi/tarif-hs` mengambil data dari **BTKI (Buku Tarif Kepabeanan Indonesia)** yang merupakan tarif pabean berbasis nomor HS Code (komoditas).
  * PPh Pasal 22 Impor diatur oleh Direktorat Jenderal Pajak / Kementerian Keuangan berdasarkan **subjek pajak / kepemilikan izin API**, bukan tarif tetap per HS Code. Oleh sebab itu, server API CEISA **tidak pernah mengirimkan baris PPH** pada endpoint `tarif-hs`.
* **Dasar Hukum Tarif 2,5%:**
  * Sesuai **PMK No. 34/PMK.010/2017 jo PMK No. 41/PMK.010/2022**:
    * Importir dengan **API (Angka Pengenal Importir / NIB)**: Tarif **2,5%** dari Dasar Pengenaan Pajak ($\text{DPP PPh} = \text{Harga Penyerahan/CIF} + \text{Nilai BM}$).
    * Importir **Non-API**: Tarif **7,5%**.
    * Pemegang **SKB (Surat Keterangan Bebas PPh 22)**: Tarif **0%** (Fasilitas Bebas).
  * Di Kawasan Berikat (TPB), seluruh pengusaha TPB berstatus badan usaha ber-API. Sistem BZone otomatis menerapkan default **2,5%** pada barang impor/olahan impor, namun kolom tarif tetap fleksibel untuk diedit jika perusahaan non-API atau memiliki SKB.

#### 4. Mekanisme Anti-Duplikasi Record Barang Tarif (Update In-Place)
* Pada saat pengguna mengganti HS Code, mengosongkan lalu memilih kembali HS Code, atau menekan tombol **"Tarik / Refresh Tarif CEISA"**:
  * Sistem BZone memetakan baris tarif yang sudah ada (`BM`, `PPH`, `PPN`) dan melakukan pembaruan langsung di baris yang sama (**`COMMAND.UPDATE` / `(1, id, vals)`**).
  * Sistem otomatis mendeteksi dan menghapus baris duplikat (**`COMMAND.DELETE` / `(2, id, 0)`**).
  * Jika HS Code dikosongkan, baris tarif dihapus secara bersih sehingga tidak ada record ganda/ndobel di tampilan antarmuka pengguna.

### 3.4 Ketentuan Pungutan pada Pemindahan Antar TPB (BC 2.7)
Berbeda dengan BC 2.5, dokumen **BC 2.7** adalah pemindahan barang antar Tempat Penimbunan Berikat (Kawasan Berikat ke Kawasan Berikat lainnya):
* **Fasilitas Pungutan**: Bea Masuk mendapat fasilitas **Ditangguhkan (Kode Fasilitas 3)** dan PPN/PPh mendapat fasilitas **Tidak Dipungut (Kode Fasilitas 5 atau 6)**.
* Nilai tarif PPN tetap dicantumkan 11%, namun **Nilai Bayar Tunai = Rp 0** (seluruh nilai masuk ke Nilai Fasilitas) karena barang belum beredar bebas di pasar lokal.

---

## 4. Panduan Memilih Kode Fasilitas Pungutan

Setiap pungutan Bea Masuk, PPN, dan PPh pada dokumen TPB memiliki **kode fasilitas** yang menjelaskan status pungutan. Memilih kode yang salah adalah penyebab paling umum dokumen TPB ditolak.

| Kode | Status | Kapan Dipakai | Dasar Hukum |
|---|---|---|---|
| **1** | **Dibayar** | Bea Masuk atas penjualan lokal dari TPB ke TLDDP melalui BC **2.5**. Tarif PPN/PPh tetap dipungut penuh. | UU Kepabeanan No. 17/2006, UU HPP Perpajakan |
| **3** | **Ditangguhkan** | Bea Masuk atas pemasukan bahan baku impor ke Kawasan Berikat melalui BC **2.3**. Tidak bayar sekarang, tapi tetap dicatat sebagai utang. | PMK 131/PMK.04/2018 tentang Kawasan Berikat |
| **6** | **Tidak Dipungut** | PPN/PPh atas BC 2.3 dan pembelian lokal masuk TPB melalui BC **4.0** (PPN tidak dipungut sesuai fasilitas TPB). | PP No. 147/2000 jo. PMK 131/2018 |
| **8** | **Dijaminkan** | Bea Masuk atas pengeluaran subkontrak / perbaikan ke luar pabrik melalui BC **2.6.1**, dengan jaminan bank garansi. | PMK 272/PMK.04/2020 |
| **5** | **Dibebaskan** | Bea Masuk atas impor dengan tarif preferensi **SKA Form D / Form AK / Form E** (preferential tariff). | SKA FTA: CEPA, ACFTA, IJEPA, dll. |
| **7** | **Sudah Dilunasi** | Bea Masuk, PPN, atau PPh yang telah dilunasi di muka (misal: melalui setor manual ke bank persepsi sebelum dokumen dikirim). | UU Kepabeanan No. 17/2006 |

> [!TIP]
> Gunakan **panduan singkat** di bawah ini saat ragu:
> * Impor bahan baku ke KB → **Kode 3**
> * Pembelian lokal ke TPB → **Kode 6** (untuk PPN)
> * Jual lokal ke pasar (BC 2.5) → **Kode 1**
> * Subkontrak keluar (BC 2.6.1) → **Kode 8**
> * Impor dengan tarif preferensi → **Kode 5**

---

## 5. Checklist Verifikasi Sebelum Pengiriman ke CEISA

Sebelum mengklik **Send Document**, pastikan seluruh poin di bawah ini telah dicek untuk meminimalisir risiko **Reject**.

* [ ] **Nomor Aju 26 digit** terbentuk otomatis dan terlihat wajar.
* [ ] **Tanggal Kurs Pajak** sesuai dengan minggu NDPBM KMK yang berlaku.
* [ ] **Pengusaha TPB** berada pada posisi entitas Seri 1 dengan NPWP 16 digit valid.
* [ ] **Invoice** berada pada urutan Seri 1 (jenis `380`/`PROFORMA`).
* [ ] **B/L atau AWB** berada pada urutan Seri 2 (jenis `705`/`740`).
* [ ] **Kode bendera negara** ditulis dalam 2 huruf kapital standar ISO.
* [ ] **Nilai CIF total tidak 0** dan sesuai FOB + Freight + Asuransi.
* [ ] **Selisih kurs** CIF Valas dan CIF Rupiah adalah hasil perkalian dengan NDPBM KMK (bukan kurs bank).
* [ ] **HS Code 8 digit** telah sesuai dengan deskripsi fisik barang.
* [ ] **Kode fasilitas pungutan** (1/3/5/6/7/8) sudah tepat sesuai jenis transaksi.
* [ ] **Kuantitas dan satuan** pada item barang konsisten dengan packing list.
* [ ] **Tanda tangan dan pernyataan** pengisian data telah di-*centang* oleh PPJK atau staf kepabeanan.
* [ ] **Tidak ada karakter aneh** (seperti tanda kutip pada kolom angka) yang biasanya membuat JSON schema CEISA gagal parsing.

> [!IMPORTANT]
> Jika salah satu poin di atas tidak terpenuhi, jangan paksakan pengiriman. Gunakan tombol **Confirm** untuk menjalankan validasi internal BZone, dan perbaiki terlebih dahulu jika ada *error* yang tampil.

---

> Dokumen ini adalah panduan non-teknis yang melengkapi referensi modul **`djbc_ceisa_tpb`**. Untuk detail struktur JSON dan alur API ke server CEISA, silakan merujuk ke dokumen [Pengelolaan Dokumen TPB CEISA 4.0](pengelolaan_dokumen_tpb.md).
