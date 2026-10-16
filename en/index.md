# BZone IT Inventory & Customs Guide

Welcome to the **BZone IT Inventory** documentation portal. This system is designed to fulfill regulatory compliance with the Directorate General of Customs and Excise (DJBC) of the Republic of Indonesia, specifically for **Bonded Zones (Kawasan Berikat - KB)**, **KITE**, **Free Trade Zones (FTZ)**, and **CEISA 4.0** integrations.

---

## System Overview

BZone seamlessly bridges ERP enterprise operations (Procurement, Warehouse, Production, and Sales) with Indonesian customs documentation in real-time. Every inventory movement is logged and reconciled to ensure compliance with official customs audit regulations.

> [!NOTE]
> All incoming and outgoing cargo movements must be linked to a valid customs registration document (e.g. BC 2.3, BC 2.5, BC 4.0, or BC 4.1).

---

## Core Modules Architecture

1. **BZone Core (`djbc_app`)**:
   Main foundation providing customs document configurations, UOM standards, packaging classifications, reporting periods, and audit trail logs.
2. **Bonded Zone (`djbc_kb`)**:
   IT Inventory classification, inbound and outbound handling under duty exemption/deferment, WIP shopfloor tracking, and the 7 Mandatory Customs Mutation Reports.
3. **KITE Facility (`djbc_kite`)**:
   Customs duty exemption and drawback for export manufacturing inputs, raw material conversion tracking (BoM), and the 8 Official KITE Compliance Reports.
4. **Special Economic Zone (`djbc_kek`)**:
   SEZ compliance with unified PPKEK entry/exit declarations (LDP/TLDDP), tax holidays and duty-free incentives, and the 4 Mandatory KEK IT Inventory Reports.
5. **Stock Opname (`djbc_stock_opname`)**:
   Customs assignment letter recording, annual warehouse physical counting, book vs. physical variance reconciliation, and official stock opname card generation.
6. **Customs Rate Management (`erpi_custom_rate`)**:
   Manual foreign currency rate specification for POs, SOs, and Invoices aligning with weekly Ministry of Finance Decree (*Kurs KMK*) exchange rates.
7. **INSW Service Connector (`djbc_insw`)**:
   Direct electronic synchronization of beginning balances, inbound/outbound transfers, and stock opname results with the national INSW portal.
8. **CEISA Core (`djbc_ceisa_core`)**:
   Centralized CEISA 4.0 OpenAPI integration layer, OAuth2 token authentication, exchange rate/tariff/lartas/manifest sync services.
9. **CEISA TPB (`djbc_ceisa_tpb`)**:
   Customs declaration authoring and submission for Bonded Storage Facilities (BC 1.6, 2.3, 2.5, 2.6.1, 2.6.2, 2.7, 4.0, 4.1) and real-time response tracking.
10. **CEISA Import (`djbc_ceisa_impor`)**:
   Authoring and submission of Import Declaration Documents (PIB / BC 2.0) for general imports from outside the customs area, with 5-layer validation and auto-generation of Importer/Owner/Shipper/Seller entities.
11. **CEISA Export (`djbc_ceisa_expor`)**:
   Authoring and submission of Export Declaration Documents (PEB / BC 3.0) and PEB from PLB (BC 3.3), including Foreign Bank, Goods Readiness data, and automatic NPE retrieval.
12. **CEISA FTZ (`djbc_ceisa_ftz`)**:
   Authoring and submission of PPFTZ documents (511/513/522/531) for companies in KPBPB Batam, Bintan, Karimun, and Sabang with a dedicated Strategy Pattern serializer per document type.

![BZone Main Dashboard Overview](/assets/images/dashboard_overview.png)
*Figure 1.1: BZone Customs Document and IT Inventory status overview dashboard.*

---

## User Guide Navigation

### 1. System Master Data
* **[Product Master Data & Logistics Routes](/bzone/docs/master_data/produk)**
* **[Business Partners (Vendor & Customer)](/bzone/docs/master_data/mitra_bisnis)**

### 2. Standard Operational Cycles
* **[Sales Operational Cycle (Sales & MTO)](/bzone/docs/operasional/penjualan_sales)**
* **[Procurement Operational Cycle (Purchase)](/bzone/docs/operasional/pengadaan_purchase)**
* **[Goods Receipt & Delivery (Inventory)](/bzone/docs/operasional/pergudangan_inventory)**
* **[Manufacturing & BoM (MRP)](/bzone/docs/operasional/manufaktur_mrp)**
* **[Accounting & Finance Management](/bzone/docs/operasional/keuangan_accounting)**

### 3. BZone Customs Compliance
* **[Basic Configurations & Master Documents](/bzone/docs/djbc_app/konfigurasi_dasar)**
* **[Item Classification in IT Inventory](/bzone/docs/djbc_kb/klasifikasi_barang)**
* **[Inbound & Outbound Workflows](/bzone/docs/djbc_kb/pemasukan_pengeluaran)**
* **[KITE Facility Guide](/bzone/docs/djbc_kite/panduan_fasilitas_kite)**
* **[Special Economic Zone (KEK) Guide](/bzone/docs/djbc_kek/panduan_fasilitas_kek)**
* **[7 Mandatory IT Inventory Mutation Reports](/bzone/docs/djbc_kb/laporan_mutasi_it_inventory)**
* **[Physical Inventory / Stock Opname Execution](/bzone/docs/djbc_stock_opname/pelaksanaan_stock_opname)**
* **[Customs & Tax Exchange Rate Management](/bzone/docs/erpi_custom_rate/manajemen_kurs_pabean)**
* **[CEISA 4.0 Foundation Integration](/bzone/docs/djbc_ceisa_core/integrasi_ceisa_core)**
* **[CEISA 4.0 TPB Document Management](/bzone/docs/djbc_ceisa_tpb/pengelolaan_dokumen_tpb)**
* **[CEISA 4.0 Import Documentation (BC 2.0)](/bzone/docs/djbc_ceisa_impor/dokumentasi_impor)**
* **[CEISA 4.0 Export Documentation (BC 3.0 & 3.3)](/bzone/docs/djbc_ceisa_expor/dokumentasi_expor)**
* **[CEISA 4.0 PPFTZ Documentation (KPBPB)](/bzone/docs/djbc_ceisa_ftz/dokumentasi_ftz)**
* **[INSW IT Inventory Integration Connector](/bzone/docs/djbc_insw/konektor_layanan_insw)**

