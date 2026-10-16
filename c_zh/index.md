# BZone IT 库存与海关文档指南

欢迎访问 **BZone IT 库存** 文档门户。本系统旨在满足印度尼西亚共和国海关与消费税总局（DJBC）的合规要求，特别是 **保税区（Kawasan Berikat - KB）**、**KITE 便利化**、**自由贸易区（FTZ）** 以及 **CEISA 4.0** 集成。

---

## 系统概述

BZone 将 ERP 企业运营（采购、仓储、生产、销售）与印度尼西亚海关文档进行实时无缝对接。每一笔库存移动都被记录并对账，确保符合官方海关审计法规。

> [!NOTE]
> 所有进出货物运输必须关联有效的海关登记单证（如 BC 2.3、BC 2.5、BC 4.0 或 BC 4.1）。

---

## 核心模块架构

BZone 生态系统由以下模块组成：

1. **BZone 核心（`djbc_app`）**：
   主要基础模块，提供海关文档配置、计量单位标准、包装分类、报表期间以及审计日志功能。
2. **保税区（`djbc_kb`）**：
   IT 库存分类、享受关税减免/延期的进出库管理、车间在制品跟踪，以及 7 份必备海关变动报表。
3. **KITE 便利化（`djbc_kite`）**：
   为出口制造业原料提供关税豁免与退税、物料消耗（BoM）跟踪，以及 8 份官方 KITE 合规报表。
4. **经济特区（`djbc_kek`）**：
   经济特区合规管理，包括统一的 PPKEK 进出申报（LDP/TLDDP）、税收优惠与免税待遇，以及 4 份必备 KEK IT 库存报表。
5. **库存盘点（`djbc_stock_opname`）**：
   海关任务函记录、年度仓储实物盘点、账面与实物差异对账，以及官方盘点卡片生成。
6. **海关汇率管理（`erpi_custom_rate`）**：
   手动设定外币汇率用于采购订单、销售订单和发票，与每周财政部令（*Kurs KMK*）汇率保持一致。
7. **INSW 服务连接器（`djbc_insw`）**：
   将期初余额、进出库转移和盘点结果与国家 INSW 门户直接电子同步。
8. **CEISA 核心（`djbc_ceisa_core`）**：
   集中式 CEISA 4.0 OpenAPI 集成层、OAuth2 令牌认证、汇率/税则/限制/舱单同步服务。
9. **CEISA 保税区（`djbc_ceisa_tpb`）**：
   保税仓储场所（BC 1.6、2.3、2.5、2.6.1、2.6.2、2.7、4.0、4.1）的报关单起草与提交，以及实时响应跟踪。
10. **CEISA 进口（`djbc_ceisa_impor`）**：
    进口报关单（PIB / BC 2.0）的起草与提交，适用于海关区域外的普通进口业务，并提供 5 层验证以及进口商/货主/发货人/卖方实体自动生成。
11. **CEISA 出口（`djbc_ceisa_expor`）**：
    出口报关单（PEB / BC 3.0）以及 PLB 出口报关单（BC 3.3）的起草与提交，包括外汇银行、货物就绪数据以及 NPE 自动获取。
12. **CEISA 自由贸易区（`djbc_ceisa_ftz`）**：
    为巴淡、民丹、卡里蒙和沙旺 KPBPB 内企业起草并提交 PPFTZ 单证（511/513/522/531），采用按单证类型分类的策略模式序列化器。

![BZone 主仪表板概览](/assets/images/dashboard_overview.png)
*图 1.1：BZone 海关单证与 IT 库存状态监控仪表板。*

---

## 用户指南导航

### 1. 系统主数据
* **[产品主数据与物流路线](/bzone/docs/master_data/produk)**
* **[业务合作伙伴（供应商与客户）](/bzone/docs/master_data/mitra_bisnis)**

### 2. 标准业务流程
* **[销售流程（销售与 MTO）](/bzone/docs/operasional/penjualan_sales)**
* **[采购流程](/bzone/docs/operasional/pengadaan_purchase)**
* **[收货与发货（库存）](/bzone/docs/operasional/pergudangan_inventory)**
* **[生产与物料清单（MRP）](/bzone/docs/operasional/manufaktur_mrp)**
* **[财务与会计管理](/bzone/docs/operasional/keuangan_accounting)**

### 3. BZone 海关合规
* **[基础配置与主文档](/bzone/docs/djbc_app/konfigurasi_dasar)**
* **[IT 库存商品分类](/bzone/docs/djbc_kb/klasifikasi_barang)**
* **[入库与出库流程](/bzone/docs/djbc_kb/pemasukan_pengeluaran)**
* **[KITE 便利化指南](/bzone/docs/djbc_kite/panduan_fasilitas_kite)**
* **[经济特区（KEK）指南](/bzone/docs/djbc_kek/panduan_fasilitas_kek)**
* **[7 份必备 IT 库存变动报表](/bzone/docs/djbc_kb/laporan_mutasi_it_inventory)**
* **[库存盘点执行](/bzone/docs/djbc_stock_opname/pelaksanaan_stock_opname)**
* **[海关与税务汇率管理](/bzone/docs/erpi_custom_rate/manajemen_kurs_pabean)**
* **[CEISA 4.0 基础集成](/bzone/docs/djbc_ceisa_core/integrasi_ceisa_core)**
* **[CEISA 4.0 TPB 单证管理](/bzone/docs/djbc_ceisa_tpb/pengelolaan_dokumen_tpb)**
* **[CEISA 4.0 进口文档（BC 2.0）](/bzone/docs/djbc_ceisa_impor/dokumentasi_impor)**
* **[CEISA 4.0 出口文档（BC 3.0 与 3.3）](/bzone/docs/djbc_ceisa_expor/dokumentasi_expor)**
* **[CEISA 4.0 PPFTZ 文档（KPBPB）](/bzone/docs/djbc_ceisa_ftz/dokumentasi_ftz)**
* **[INSW IT 库存集成连接器](/bzone/docs/djbc_insw/konektor_layanan_insw)**