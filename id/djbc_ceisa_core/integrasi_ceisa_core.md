# Integrasi Fondasi CEISA 4.0 (djbc_ceisa_core)

Modul **`djbc_ceisa_core`** adalah lapisan integrasi utama (*core gateway*) yang menghubungkan seluruh operasional kepabeanan BZone di Odoo dengan ekosistem **CEISA 4.0 (Customs-Excise Information System and Automation)** Direktorat Jenderal Bea dan Cukai (DJBC) Republik Indonesia via RESTful OpenAPI.

---

## Arsitektur & Peran CEISA Core

Modul ini bertindak sebagai jembatan tunggal untuk autentikasi terpusat, sinkronisasi master referensi kepabeanan nasional, dan pencatatan jejak audit (*API logging*) untuk semua modul layanan pabean turunan:

```mermaid
graph TD
    subgraph Odoo ERP & BZone
        CORE["djbc_ceisa_core (Gateway & Auth)"]
        TPB["djbc_ceisa_tpb (Dokumen TPB)"]
        IMP["djbc_ceisa_impor (Dokumen Impor)"]
        EXP["djbc_ceisa_expor (Dokumen Ekspor)"]
        FTZ["djbc_ceisa_ftz (Dokumen FTZ)"]
        TPB --> CORE
        IMP --> CORE
        EXP --> CORE
        FTZ --> CORE
    end
    subgraph Portal Bea Cukai
        CEISA["CEISA 4.0 OpenAPI Portal"]
        AUTH["SSO & Token Service"]
        REF["Layanan Referensi (Kurs, Tarif, Lartas)"]
        DOC["Endpoint Pengajuan Dokumen Pabean"]
        CORE --> AUTH
        CORE --> REF
        CORE --> DOC
    end
```

### Modul Layanan yang Bergantung pada CEISA Core:
* **`djbc_ceisa_tpb`**: Dokumen Pabean Tempat Penimbunan Berikat (Kawasan Berikat, Gudang Berikat, dsb.).
* **`djbc_ceisa_impor`**: Pemberitahuan Impor Barang (PIB / BC 2.0).
* **`djbc_ceisa_expor`**: Pemberitahuan Ekspor Barang (PEB / BC 3.0).
* **`djbc_ceisa_ftz`**: Pemberitahuan Pabean Free Trade Zone (PPFTZ).
* **`djbc_ceisa_barang_kiriman`**: Dokumen Consignment Note / Barang Kiriman.

---

## Konfigurasi Kredensial & Autentikasi CEISA

Untuk mengaktifkan koneksi ke portal Bea Cukai, administrator wajib mengisi parameter koneksi pada menu **CEISA Connector > Configuration > Settings**:

### 1. Parameter Akun CEISA OpenAPI
* **Username**: Nama pengguna akun portal pengguna jasa Bea Cukai.
* **Password**: Kata sandi portal Bea Cukai.
* **Bea Cukai API Key**: Kunci API unik yang di-generate melalui menu profil pengguna di portal CEISA OpenAPI (`https://openapi.beacukai.go.id`).
* **CEISA API URL**: Alamat endpoint API resmi (misal: `https://openapi.beacukai.go.id` untuk produksi).
* **Company Code (Kode Perusahaan)**: 6 digit kode perusahaan resmi yang diterbitkan Bea Cukai (digunakan dalam pembentukan 26 digit Nomor Pengajuan / Nomor Aju dokumen pabean).

### 2. Mekanisme Token OAuth2
* Sistem menggunakan alur autentikasi token Bearer (`/v1/openapi-auth/user/login`).
* Token akses berlaku selama 300 detik (5 menit) dan token penyegaran (*refresh token*) berlaku 24 jam.
* BZone secara cerdas mengelola masa aktif token dan melakukan **Auto-Refresh** otomatis dengan buffer pengaman 30 detik sebelum token kedaluwarsa.

---

## Layanan Sinkronisasi Master Referensi

Modul `djbc_ceisa_core` menyediakan wizard sinkronisasi data referensi resmi langsung dari server Bea Cukai:

### 1. Sinkronisasi Kurs Mingguan KMK
* Menu: **CEISA Connector > Layanan CEISA > Sinkronisasi Kurs**.
* Menarik kurs valuta asing resmi Keputusan Menteri Keuangan (KMK) untuk perhitungan Nilai Dasar Penghitungan Bea Masuk (NDPBM).
* Dapat dijalankan terjadwal (*automated cron*) setiap hari Rabu saat pembaruan kurs KMK mingguan dirilis.

### 2. Sinkronisasi Pos Tarif & Bea Masuk
* Menu: **CEISA Connector > Layanan CEISA > Sinkronisasi Tarif**.
* Menarik tarif Bea Masuk (BM), PPN, PPnBM, dan PPh Pasal 22 untuk HS Code **8 digit** spesifik sesuai Buku Tarif Kepabeanan Indonesia (BTKI).
* **Ketentuan HS Code**: Gunakan kode pos tarif **8 digit penuh** (level terendah BTKI/AHTN, bukan kode header 2/4/6 digit) karena layanan Tarif dan Lartas CEISA 4.0 hanya memvalidasi pembebanan pungutan pada pos tarif 8 digit.

### 3. Pengecekan Aturan Larangan & Pembatasan (Lartas)
* Menu: **CEISA Connector > Layanan CEISA > Sinkronisasi Lartas**.
* Mengetahui izin instansi teknis (Kementerian Perdagangan, Karantina, BPOM, dsb.) yang dipersyaratkan sebelum barang impor/ekspor dimasukkan atau dikeluarkan berdasarkan HS Code 8 digit.

### 4. Sinkronisasi Manifes TPS
* Menu: **CEISA Connector > Layanan CEISA > Sinkronisasi Manifes**.
* Menarik data inward/outward manifest (BC 1.1) dari Tempat Penimbunan Sementara (TPS) pelabuhan laut atau udara berdasarkan nomor pos dan subpos.

### 5. Penarikan Status & Respon Mandiri
* Menu: **CEISA Connector > Layanan CEISA > Status & Respon**.
* Klik tombol **Tarik Status & Respon BC** pada bagian atas daftar atau formulir **Status & Respon**, lalu masukkan **26 digit Nomor Aju** pada jendela yang muncul.
* Sistem otomatis menghubungi server CEISA dan mencatat:
    * **Riwayat Status**: Urutan proses dokumen pabean beserta Nomor Daftar, Tanggal Daftar, Waktu Status, dan Kode Status terbaru.
    * **Riwayat Respon**: Daftar respon resmi (SPPB, NPP, SPJM, Reject, dll.) lengkap dengan tanggal dan waktu respon serta pesan dari Bea Cukai.
    * **Lampiran & Pratinjau PDF Otomatis di Chatter**: Jika respon CEISA menyertakan berkas PDF, sistem otomatis mengunduh dan melampirkannya ke **Chatter** dokumen pabean terkait (TPB, Impor, Ekspor, atau FTZ) maupun record **Status & Respon** lengkap dengan panel pratinjau PDF di samping formulir.

> [!IMPORTANT]
> **Ketentuan Penarikan Status & Respon (`Data tidak ditemukan`):**
> Data **Status & Respon** dari CEISA hanya dapat ditarik apabila dokumen pabean tersebut **sudah keluar Nomor Pendaftarannya (Nomor Daftar)** di portal Bea Cukai. Jika dokumen baru dikirim/berstatus draf dan **belum memiliki Nomor Pendaftaran**, maka saat ditarik akan muncul pesan **`Data tidak ditemukan`**. Hal ini normal dari sisi server CEISA—silakan lakukan penarikan kembali setelah dokumen memperoleh Nomor Pendaftaran.

---

## Sinkronisasi Master Data Pendukung

Sebelum membuat dokumen pabean, pastikan master data internal Odoo telah dipetakan:

1. **Kode Pos Tarif Produk (HS Code - 8 Digit)**:
    * Buka menu **Inventory > Products > Products** (atau master **HS Code**).
    * Pada tab **CEISA / DJBC Information**, tentukan **Pos Tarif / HS Code** produk (wajib **8 digit** pos tarif nasional sesuai BTKI, tanpa memasukkan hierarki *parent* 2, 4, atau 6 digit) beserta satuan CEISA standar.
2. **Identitas Mitra Bisnis (Partner Kepabeanan)**:
    * Buka master kontak pelanggan atau vendor (**Contacts**).
    * Pada tab **CEISA**, lengkapi **NPWP (16 digit)**, **NIB (Nomor Induk Berusaha)**, dan **Kode Entitas** pabean yang relevan.
3. **Standarisasi Satuan Ukuran (UOM CEISA)**:
    * Seluruh satuan ukuran di Odoo dipetakan ke kode satuan internasional CEISA (misal: `KGM` untuk kilogram, `PCE` untuk pieces, `LTR` untuk liter).

---

## Pemantauan & Jejak Audit (API Logging)

Setiap pertukaran data (request payload dan response payload) antara BZone dan CEISA tercatat secara permanen:

* **Menu API Log**: Akses melalui **CEISA Connector > Configuration > API Log**.
* Menyimpan informasi lengkap:
    * Endpoint URL yang diakses.
    * HTTP Status Code (200 OK, 400 Bad Request, 401 Unauthorized, 500 Server Error).
    * Request JSON Body dan Response JSON Body.
    * Waktu pemrosesan (*Execution Duration*).
* Memudahkan tim IT dan operasional dalam melakukan analisis kendala jika terdapat penolakan (*reject*) dari sistem Bea Cukai.

> [!TIP]
> Lakukan pengujian koneksi awal dengan menekan tombol **Test Connection** pada pengaturan CEISA sebelum memulai pengajuan dokumen transaksi pabean perdana.
