# Master Data Mitra Bisnis (Vendor & Customer)

> **Terakhir diperbarui:** 22 Oktober 2026

Master data mitra bisnis (*Partners*) mencakup profil pemasok (*Vendors*) dan pelanggan (*Customers*). Pada sistem BZone, data mitra bisnis tidak hanya digunakan untuk transaksi komersial (PO, SO, dan Faktur), melainkan juga menjadi identitas entitas resmi (Importir, Eksportir, Pengirim, Penerima, dan PPJK) pada dokumen pabean CEISA.

---

## 1. Pembuatan Master Data Vendor

Vendor adalah pihak pemasok bahan baku, bahan penolong, mesin, atau jasa operasional.

### Langkah-langkah Pembuatan:

1. Buka modul **Purchase**.
2. Masuk ke menu **Orders → Vendors**.
3. Klik tombol **New**.

![Formulir Master Data Vendor](../assets/images/master_vendor_form.png)
*Gambar 2.1: Formulir pembuatan profil master vendor baru.*

### Pengisian Data Vendor:

1. **Tipe Entitas**: Pilih antara **Company** (Badan Usaha / Perusahaan) atau **Individual** (Perorangan).
2. **Name**: Masukkan nama resmi vendor sesuai dokumen legalitas / NPWP.
3. **Address**: Lengkapi detail alamat jalan, kota, provinsi, kode pos, dan negara.
4. **Contact Details**: Isi nomor telepon, ponsel, dan alamat surel (*email*) resmi untuk korespondensi transaksi.
5. **Tab Invoicing**:
   * Masukkan Nomor Pokok Wajib Pajak (NPWP / Tax ID).
   * Daftarkan nomor rekening bank pemasok untuk kebutuhan pembayaran transfer.
6. **Tab Sales & Purchase**:
   * Tentukan syarat pembayaran standar (*Payment Terms*, misal: *Immediate*, *30 Days*).
7. Klik ikon **Simpan** (*Save*) untuk menyimpan data vendor.

---

## 2. Pembuatan Master Data Customer

Customer adalah pihak pembeli atau penerima barang hasil produksi / dagangan perusahaan.

### Langkah-langkah Pembuatan:

1. Buka modul **Sales**.
2. Masuk ke menu **Orders → Customers**.
3. Klik tombol **New**.

![Formulir Master Data Customer](../assets/images/master_customer_form.png)
*Gambar 2.2: Formulir pembuatan profil master customer baru.*

### Pengisian Data Customer:

1. **Tipe Entitas**: Pilih **Company** atau **Individual**.
2. **Name**: Masukkan nama lengkap pelanggan/pembeli.
3. **Address & Delivery**: Masukkan alamat faktur (*Invoice Address*) dan alamat tujuan pengiriman (*Delivery Address*).
4. **Contact Details**: Masukkan kontak narahubung utama (*Contact Person*), telepon, dan email.
5. **Tab Invoicing / Accounting**:
   * Isi nomor NPWP (*Tax ID*) pelanggan untuk penerbitan faktur pajak elektronik.
   * Tentukan *Credit Limit* (batas plafon piutang) jika perusahaan memberlakukan kebijakan kontrol kredit.
6. **Tab Sales & Purchase**:
   * Tentukan daftar harga standar (*Pricelist*).
   * Tentukan syarat pembayaran (*Payment Terms*).
7. Klik ikon **Simpan** (*Save*) untuk menyimpan data pelanggan.

---

## 3. Integrasi Mitra Bisnis dengan Dokumen Kepabeanan

> [!NOTE]
> Saat modul **BZone CEISA** aktif, identitas nama, NPWP 16 digit/NITKU, alamat lengkap, dan kode negara mitra bisnis akan otomatis tersinkronisasi sebagai entitas deklarasi pabean:
> * **Vendor Asing**: Terpetakan sebagai *Seller* / *Exporter* pada dokumen impor BC 2.0 / BC 2.3.
> * **Vendor Lokal**: Terpetakan sebagai *Pengirim Barang* pada dokumen pemasukan BC 4.0.
> * **Customer Luar Negeri**: Terpetakan sebagai *Consignee* / *Buyer* pada dokumen ekspor PEB BC 3.0.
> * **Customer Lokal**: Terpetakan sebagai *Penerima Barang* pada dokumen pengeluaran BC 2.5 / BC 4.1.
