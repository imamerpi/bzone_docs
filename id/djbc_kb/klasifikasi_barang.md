# Klasifikasi Kategori Barang IT Inventory (djbc_kb)

Pada fasilitas Kawasan Berikat, setiap barang yang berada di dalam area pabrik wajib diklasifikasikan ke dalam kategori IT Inventory yang baku sesuai **Peraturan Direktur Jenderal Bea dan Cukai Nomor PER-02/BC/2019**.

---

## 6 Kategori Wajib IT Inventory

Sistem BZone membagi seluruh produk ke dalam 6 kategori pelaporan mutasi:

| Kategori Barang | Kode DJBC | Deskripsi Operasional | Laporan Terkait |
| :--- | :--- | :--- | :--- |
| **Bahan Baku** | `RM` (Raw Material) | Material utama yang diolah menjadi produk jadi | Laporan Mutasi Bahan Baku & Penolong |
| **Bahan Penolong** | `AUX` (Auxiliary) | Bahan pembantu dalam proses produksi | Laporan Mutasi Bahan Baku & Penolong |
| **Barang Jadi** | `FG` (Finished Goods) | Hasil akhir produksi yang siap dikirim/diekspor | Laporan Mutasi Barang Jadi |
| **Barang Dalam Proses** | `WIP` (Work In Process) | Barang setengah jadi di lantai produksi/shopfloor | Laporan Mutasi Barang Dalam Proses |
| **Mesin & Peralatan** | `MAC` (Machinery) | Aset mesin pabrik & suku cadang dengan fasilitas | Laporan Mutasi Mesin & Peralatan |
| **Reject & Scrap** | `SCR` (Scrap) | Sisa potongan, limbah industri, atau barang rusak | Laporan Mutasi Barang Sisa / Scrap |

---

## Konfigurasi Kategori pada Master Produk

Pengaturan kategori IT Inventory dapat ditentukan pada level **Kategori Produk (*Product Category*)** atau langsung pada masing-masing **Master Produk (*Product Template*)**.

### 1. Pengaturan di Product Category
1. Buka menu **Inventory > Configuration > Product Categories**.
2. Pilih salah satu kategori (misal: *Raw Materials*).
3. Pada bagian **DJBC Information**, tentukan field **Kategori Barang IT Inventory**:
   * Pilih antara: `Bahan Baku`, `Bahan Penolong`, `Barang Jadi`, `Barang Dalam Proses`, `Mesin`, atau `Barang Sisa/Scrap`.

![Konfigurasi Kategori Produk DJBC](/assets/images/kb_product_category.png)
*Gambar 3.1: Menentukan klasifikasi kategori IT Inventory pada kategori produk.*

### 2. Override di Master Produk
Jika ada produk khusus yang memerlukan klasifikasi berbeda dari kategori induknya:

1. Buka menu **Master Data > Products**.
2. Masuk ke tab **Customs / DJBC**.
3. Centang atau pilih klasifikasi kategori IT Inventory yang sesuai.

> [!NOTE]
> Jika field klasifikasi pada Master Produk dikosongkan, sistem secara otomatis akan menggunakan (*fallback*) klasifikasi dari Kategori Produk induknya.

---

## Validasi Stok & Saldo Awal

Saat pertama kali mengimplementasikan sistem:
* Seluruh barang yang memiliki fasilitas penangguhan bea masuk wajib diinput nomor dokumen pabean asalnya pada saldo awal stok.
* Jangan mencampurkan barang non-fasilitas (lokal bebas) dengan barang fasilitas tanpa pencatatan jenis dokumen yang terpisah.

> [!WARNING]
> Kesalahan dalam menentukan kategori barang (misal: Bahan Baku terdaftar sebagai Mesin) akan menyebabkan ketidaksesuaian saldo pada saat pemeriksaan Laporan Pertanggungjawaban Mutasi oleh pihak Bea Cukai.
