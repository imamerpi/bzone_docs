# 进口便利化用于出口目的（djbc_kite）

> **最后更新：** 22 Oktober 2026

**`djbc_kite`** 模块管理在 **Kemudahan Impor Tujuan Ekspor (KITE)** 便利化下运营的制造业的海关作业和法定库存责任申报，完全符合海关与消费税总局（DJBC）法规。

---

## 法规框架与 KITE 概念

KITE 便利化（受 **PMK No. 145/PMK.04/2022** 和 **PER-14/BC/2022** 约束）为进口原材料用于加工、组装或装配成出口商品的出口导向制造企业提供财政减免：

1. **KITE 豁免（*Pembebasan*）**：对用于出口制造的进口原材料免除进口关税、增值税（VAT）和奢侈品销售税。
2. **KITE 退税（*Pengembalian*）**：对已成功出口成品中所含进口材料的已付进口关税予以退还。
3. **KITE 的 IT 库存合规**：
   * 内部库存系统必须可问责地区分便利化材料和非便利化材料。
   * 通过物料清单（BoM）精确跟踪材料的消耗转化为成品。
   * 官方变动报表必须通过实时电子渠道可供海关检查官员访问。

---

## 主数据配置

在开始交易之前配置以下基础记录：

### 1. KITE 产品标记（`product.template`）
* 打开 **库存 > 产品 > 产品（Inventory > Products > Products）**。
* 在海关选项卡下，启用 **可用于 DJBC KITE（Can be Used for DJBC KITE）**（`djbc_kite_ok`）。
* 此设置也可以通过 **产品类别（Product Categories）**（`product.category`）全局管理。

### 2. KITE 作业类型（`stock.picking.type`）
* 打开 **库存 > 配置 > 作业类型（Inventory > Configuration > Operations Types）**。
* 打开相关的转移类型并配置：
  * **DJBC 的 KITE 移动类型（DJBC's KITE Movement Type）**：指定方向为 **入库（In）** 或 **出库（Out）**。
  * **DJBC 的 KITE 废料？（DJBC's KITE Scrap?）**：如该作业记录生产废料、不合格品或废料移动，则启用。
  * **DJBC 的 KITE 调整？（DJBC's KITE Adjustment?）**：如该作业记录物理计数差异调整，则启用。
  * **DJBC KITE 单证必填（DJBC KITE Doc Required）**：如转移必须验证官方海关登记详情，则启用。

### 3. 仓库库位分类（`stock.location`）
* 指定内部存储库位：
  * 原材料存储库位
  * 在制品（WIP）库位
  * 成品存储库位
  * 废料 / 不合格品库位

---

## 制造与运营工作流

KITE 方案下的制造运营与海关申报直接集成：

1. **原材料入库**：
   * 原材料在海关报关单 **BC 2.0 / BC 2.3** 或 **BC 2.4**（进口）以及 **BC 4.0**（本地供应）下登记。
   * 已验证的收货转移更新库存余额并记录到 **KITE 入库报表**。
2. **生产消耗（Manufacturing）**：
   * 车间物料发放参考 Odoo 制造模块中的物料清单（BoM）。
   * 每个生产订单（MO）的实际物料消耗记录在 **原材料与辅助材料使用报表** 中。
3. **生产产出收货**：
   * 生产完成将 WIP 转化为成品和废料。
   * 成品转移到仓库存储并记录在 **生产产出入库报表** 中。
4. **出口实现**：
   * 海外发运在 **BC 3.0（出口报关 / PEB）** 下申报。
   * 成功的出口是履行 KITE 进口关税担保的法定义务基础。

---

## 8 份必备法定 KITE 报表

BZone 在 **海关报表 > 报表 > 进口便利化用于出口目的（Laporan Bea Cukai > Reporting > Kemudahan Impor Tujuan Expor）** 下提供 **8 份法定 IT 库存 KITE 报表**：

| 编号 | 法定报表 | 描述与参考单证 |
|---|---|---|
| **1** | **入库货物报表（Inbound Goods Report）** | 所有收到材料的分项汇总（BC 2.0、BC 2.3、BC 2.4、BC 4.0）。 |
| **2** | **出库货物报表（Outbound Goods Report）** | 出口发运、分包和本地销售的分项汇总。 |
| **3** | **原材料使用报表（Raw Material Usage Report）** | 每个生产订单（MO/SPK）的材料消耗明细。 |
| **4** | **生产产出报表（Production Output Report）** | 从生产线收到的成品汇总到仓库库存。 |
| **5** | **原材料变动报表（Raw Material Mutation Report）** | 期初余额、收货、生产使用、调整和期末库存对账。 |
| **6** | **成品变动报表（Finished Goods Mutation Report）** | 期初余额、生产添加、出口发运和期末余额对账。 |
| **7** | **废料 / 不合格品变动报表（Scrap / Reject Mutation Report）** | 废料产生、本地处理、销毁和废料期末余额的可问责性。 |
| **8** | **在制品（WIP）变动报表（Work in Process (WIP) Mutation Report）** | 当前在活动生产订单中投入的原材料对账。 |

---

## 通过向导生成报表

1. 打开 **海关报表 > 报表 > 进口便利化用于出口目的（Laporan Bea Cukai > Reporting > Kemudahan Impor Tujuan Expor）**。
2. 选择所需的报表（例如 **原材料使用报表**）。
3. 在弹出的向导中：
   * 选择预定义的 **报表期间（Report Period）** 或手动输入 **起始日期（Date Start）** 和 **结束日期（Date End）**。
   * 选择导出格式：**屏幕视图（Screen View）**（*HTML*）、官方 **Py3o ODT/PDF** 或 **Excel 电子表格（XLSX）**。
4. 点击 **打印 / 预览（Print / Preview）**。

> [!TIP]
> 所有 KITE 变动报表遵循标准化的审计对账公式：
> **期末余额** = **期初余额** + **入库** − **出库 / 使用** ± **调整**