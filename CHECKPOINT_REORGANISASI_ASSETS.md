# Checkpoint: Rencana Reorganisasi Tangkapan Layar (Screenshots) BZone Docs

Dokumen ini adalah rencana dan panduan eksekusi untuk menata ulang folder `assets/images/` di `bzone_docs` agar terstruktur per modul dan mudah dicari saat dokumentasi semakin berkembang.

---

## 1. Struktur Target Folder

Folder `assets/images/` akan dikelompokkan ke dalam subfolder berbasis modul:

```text
assets/images/
├── common/                  # Aset umum, logo, icon, placeholder
├── master_data/             # Produk, Mitra Bisnis, Bahan Baku, WIP
├── djbc_core/               # Konfigurasi dasar modul pabean (UoM, Jenis Dokumen)
├── ceisa_expor/             # Tangkapan layar alur Dokumen Ekspor (BC 3.0 / BC 3.3)
├── ceisa_tpb/               # Tangkapan layar alur Dokumen TPB (BC 2.3, 2.7, 4.0, dsb)
├── ceisa_impor/             # Tangkapan layar alur Dokumen Impor (BC 2.0)
├── ceisa_ftz/               # Tangkapan layar alur Dokumen FTZ (PPFTZ)
├── kawasan_berikat/         # IT Inventory, Mutasi, dan Rekonsiliasi Pabean
└── operasional/             # Alur proses operasional ERP
    ├── sales/               # Penjualan
    ├── purchase/            # Pengadaan / Pembelian
    ├── mrp/                 # Manufaktur & BoM
    └── accounting/          # Keuangan, Faktur, Vendor Bill
```

---

## 2. Pemetaan File Saat Ini (55 File)

| Subfolder Target | File Gambar |
| :--- | :--- |
| **`common/`** | `icon.png`<br>`menu_utama_odoo.png`<br>`dashboard_overview.png`<br>`placeholder_screenshot.svg`<br>`placeholder_header.png`<br>`placeholder_entitas.png`<br>`placeholder_dokumen_pelengkap.png`<br>`placeholder_pengangkut.png`<br>`placeholder_kemasan.png`<br>`placeholder_keuangan.png`<br>`placeholder_barang.png` |
| **`master_data/`** | `master_customer_form.png`<br>`master_vendor_form.png`<br>`master_produk_general.png`<br>`master_produk_routes.png`<br>`master_produk_djbc.png`<br>`barang_raw_material.png`<br>`barang_wip.png` |
| **`djbc_core/`** | `djbc_document_type.png`<br>`djbc_uom_mapping.png` |
| **`ceisa_expor/`** | `ceisa_expor_header.png`<br>`ceisa_expor_entitas.png`<br>`ceisa_expor_dokumen_pelengkap.png`<br>`ceisa_expor_pengangkut.png`<br>`ceisa_expor_bankdevisa.png`<br>`ceisa_expor_barang.png` |
| **`ceisa_tpb/`** | `ceisa_tpb_header.png`<br>`ceisa_tpb_entitas.png`<br>`ceisa_tpb_dokumen_pelengkap.png`<br>`ceisa_tpb_pengangkut.png`<br>`ceisa_tpb_kemasan.png`<br>`ceisa_tpb_keuangan.png`<br>`ceisa_tpb_barang.png` |
| **`kawasan_berikat/`** | `kb_product_category.png`<br>`kb_report_wizard.png`<br>`kb_mutasi_report_preview.png`<br>`kb_checklist_transaksi_laporan.png`<br>`kb_checklist_transaksi_laporan.svg`<br>`kb_incoming_picking.png`<br>`kb_outgoing_picking.png` |
| **`operasional/sales/`** | `sales_quotation_mto.png`<br>`sales_order_confirmed.png`<br>`sales_register_payment.png` |
| **`operasional/purchase/`** | `purchase_rfq_new.png`<br>`purchase_order_confirm.png`<br>`purchase_vendor_bill.png` |
| **`operasional/mrp/`** | `mrp_bom_form.png`<br>`mrp_mo_creation.png`<br>`mrp_material_transfer.png`<br>`mrp_produce_all_done.png` |
| **`operasional/accounting/`** | `accounting_vendor_bill_payment.png`<br>`accounting_customer_invoice_confirm.png`<br>`accounting_profit_loss_report.png` |

---

## 3. Skrip Eksekusi Pemindahan (Bash)

Jalankan perintah ini dari terminal pada direktori `/home/addons/bzone_docs/` saat Anda siap mengeksekusinya:

```bash
cd /home/addons/bzone_docs/assets/images

# 1. Buat folder target
mkdir -p common master_data djbc_core ceisa_expor ceisa_tpb ceisa_impor ceisa_ftz kawasan_berikat operasional/sales operasional/purchase operasional/mrp operasional/accounting

# 2. Pindahkan file kategori Common
mv icon.png menu_utama_odoo.png dashboard_overview.png placeholder_* common/ 2>/dev/null || true

# 3. Pindahkan file kategori Master Data
mv master_customer_form.png master_vendor_form.png master_produk_*.png barang_raw_material.png barang_wip.png master_data/ 2>/dev/null || true

# 4. Pindahkan file kategori DJBC Core
mv djbc_document_type.png djbc_uom_mapping.png djbc_core/ 2>/dev/null || true

# 5. Pindahkan file kategori CEISA Expor
mv ceisa_expor_*.png ceisa_expor/ 2>/dev/null || true

# 6. Pindahkan file kategori CEISA TPB
mv ceisa_tpb_*.png ceisa_tpb/ 2>/dev/null || true

# 7. Pindahkan file kategori Kawasan Berikat
mv kb_*.png kb_*.svg kawasan_berikat/ 2>/dev/null || true

# 8. Pindahkan file Operasional ERP
mv sales_*.png operasional/sales/ 2>/dev/null || true
mv purchase_*.png operasional/purchase/ 2>/dev/null || true
mv mrp_*.png operasional/mrp/ 2>/dev/null || true
mv accounting_*.png operasional/accounting/ 2>/dev/null || true
```

---

## 4. Kompatibilitas dengan Modul `bzone_documentation`

Agar pemindahan file ke subfolder tidak memutus link lama di Markdown (`404 Not Found`):
Controller `bzone_serve_asset` di `bzone_documentation/controllers/doc_controller.py` telah memiliki kemampuan resolusi berbasis nama file (`basename`). Jika file berada di subfolder `assets/images/ceisa_expor/ceisa_expor_barang.png`, request ke `/assets/images/ceisa_expor_barang.png` dapat ditemukan secara rekursif.

---

## 5. Skrip Pengecekan Gambar yang Tidak Digunakan (Optional)

Skrip Python berikut dapat disimpan dan dijalankan untuk mengecek apakah ada gambar di `assets/images/` yang tidak pernah dirujuk dalam dokumen Markdown (`id/`, `en/`, `c_zh/`):

```python
import os
import re

ROOT_DIR = "/home/addons/bzone_docs"
IMAGES_DIR = os.path.join(ROOT_DIR, "assets", "images")
LANG_DIRS = [os.path.join(ROOT_DIR, d) for d in ["id", "en", "c_zh"]]

# 1. Kumpulkan semua gambar yang ada di disk
all_images = set()
for root, _, files in os.walk(IMAGES_DIR):
    for f in files:
        if f.lower().endswith(('.png', '.jpg', '.jpeg', '.svg', '.webp')):
            all_images.add(f)

# 2. Pindai referensi di semua file markdown
referenced_images = set()
img_pattern = re.compile(r'!\[.*?\]\((.*?)\)')

for ldir in LANG_DIRS:
    for root, _, files in os.walk(ldir):
        for f in files:
            if f.endswith('.md'):
                fpath = os.path.join(root, f)
                with open(fpath, 'r', encoding='utf-8', errors='ignore') as fp:
                    content = fp.read()
                    matches = img_pattern.findall(content)
                    for m in matches:
                        clean_name = os.path.basename(m.split('?')[0].split('#')[0])
                        referenced_images.add(clean_name)

# 3. Tampilkan hasil
unused = sorted(list(all_images - referenced_images))
print(f"Total gambar di assets: {len(all_images)}")
print(f"Total gambar dirujuk di dokumen: {len(referenced_images)}")
print(f"Gambar yang tidak terpakai ({len(unused)}):")
for u in unused:
    print(f"  - {u}")
```
