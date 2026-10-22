# Alur Operasional Pemasukan & Pengeluaran Barang (djbc_kb)

> **Terakhir diperbarui:** 22 Oktober 2026

Dokumen ini menjelaskan alur operasional pencatatan mutasi fisik barang yang terintegrasi langsung dengan dokumen kepabeanan pada sistem BZone Kawasan Berikat.

---

## Alur Pemasukan Barang (Incoming Operations)

Pemasukan barang ke Kawasan Berikat dapat berasal dari impor luar negeri, sesama Tempat Penimbunan Berikat, maupun dari pemasok dalam negeri (TLDDP).

### Prosedur Input Dokumen Pemasukan pada Stock Picking

1. Masuk ke menu **Inventory > Operations > Receipts** atau melalui modul **Purchase Order**.
2. Buka dokumen penerimaan barang (*Picking*) yang akan diproses.
3. Pada tab **Customs Document / Dokumen Pabean**:
   * **Jenis Dokumen BC**: Pilih jenis dokumen yang sesuai (`BC 2.3`, `BC 4.0`, atau `BC 2.7`).
   * **Nomor Pendaftaran**: Masukkan nomor pendaftaran resmi (6 digit) dari Kantor Pabean.
   * **Tanggal Pendaftaran**: Masukkan tanggal pendaftaran dokumen pabean.
   * **Nomor Pengajuan (CAR)**: Masukkan 26 digit nomor pengajuan (jika ada).
   * **Pemasok / Pengirim**: Pastikan entitas mitra (*partner*) sesuai dengan dokumen pabean.
4. Lakukan validasi penerimaan barang (*Validate Picking*).

![Formulir Penerimaan Barang dengan Dokumen Pabean](/assets/images/kb_incoming_picking.png)
*Gambar 4.1: Pengisian data dokumen kepabeanan pada proses penerimaan barang di modul Stock Picking.*

> [!IMPORTANT]
> **Kriteria Kelayakan Masuk Laporan Pemasukan DJBC:**  
> Agar penerimaan barang otomatis tercatat pada **Laporan Pemasukan Barang per Dokumen Pabean**:
> 1. Status picking telah divalidasi tuntas menjadi **Done**.
> 2. Tab **Customs Document** wajib terisi Jenis Dokumen BC (BC 2.3/4.0/2.7), Nomor Pendaftaran (6 digit), dan Tanggal Pendaftaran.
> 3. Tipe operasi gudang (*Operation Type*) memiliki konfigurasi *DJBC Movement Type = in*.  
> Transaksi penerimaan tanpa dokumen pabean hanya dicatat sebagai mutasi internal non-fasilitas.

---

## Alur Pengeluaran Barang (Outgoing Operations)

Pengeluaran barang dari Kawasan Berikat dapat berupa penjualan lokal (TLDDP), ekspor ke luar negeri, maupun pengiriman subkontrak/antar-fasilitas.

### Prosedur Input Dokumen Pengeluaran

1. Masuk ke menu **Inventory > Operations > Delivery Orders** atau melalui pesanan penjualan (**Sales Order**).
2. Buka dokumen pengiriman barang yang siap dikirim.
3. Pada tab **Customs Document / Dokumen Pabean**:
   * Tentukan tipe dokumen pengeluaran:
     * **BC 2.5**: Untuk pengeluaran barang ke wilayah lokal (TLDDP) yang dikenakan Bea Masuk dan Pajak Dalam Rangka Impor (PDRI).
     * **BC 4.1**: Untuk pengeluaran barang asal lokal (BC 4.0) kembali ke TLDDP.
     * **BC 2.7**: Untuk pengeluaran bahan baku/barang jadi ke sesama Kawasan Berikat (termasuk subkontrak).
     * **BC 3.0 / PEB**: Untuk realisasi ekspor barang jadi ke luar negeri.
   * Lengkapi nomor dan tanggal pendaftaran resmi.
4. Klik tombol **Validate** untuk memproses pengeluaran stok.

![Formulir Pengeluaran Barang dengan Dokumen Pabean](/assets/images/kb_outgoing_picking.png)
*Gambar 4.2: Input dokumen BC 2.5 / BC 3.0 pada dokumen Delivery Order.*

> [!IMPORTANT]
> **Kriteria Kelayakan Masuk Laporan Pengeluaran DJBC:**  
> Pengiriman barang hanya akan ditarik ke **Laporan Pengeluaran Barang per Dokumen Pabean** jika Delivery Order berstatus **Done**, Operation Type diset *Movement Type = out*, serta nomor dan tanggal pendaftaran dokumen pabean pengeluaran telah diisi.

---

## Penanganan Retur Penjualan & Pembelian

* **Retur Penjualan (Customer Return)**:
  Apabila terjadi pengembalian barang dari pelanggan ke dalam Kawasan Berikat, barang yang masuk kembali akan dicatat pada kolom pemasukan laporan mutasi berdasarkan referensi dokumen pabean asalnya.
* **Retur Pembelian (Vendor Return)**:
  Pengembalian material ke pemasok luar negeri atau lokal memerlukan dokumen pengeluaran pabean yang sesuai dengan persetujuan Kantor Pengawasan Bea Cukai.

> [!TIP]
> Manfaatkan fitur **Filter Dokumen Pabean berdasarkan Sales Order & Purchase Order** yang baru dirilis untuk mempercepat pencocokan nomor dokumen secara otomatis.
