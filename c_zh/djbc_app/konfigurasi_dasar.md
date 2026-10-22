# 基础配置与主文档（djbc_app）

> **最后更新：** 22 Oktober 2026

**`djbc_app`** 模块是整个 BZone 生态系统的核心基础，提供标准化的海关主数据、海关登记验证以及报表期间规则。

---

## 海关单证类型主数据

在享有海关优惠的设施中，每一次货物进出移动都必须关联有效的印尼海关单证类型。

通过 **海关报表 > 配置 > 单证类型（Laporan Bea Cukai > Configuration > Document Type）**，系统预定义了官方海关单证类型：

* **BC 2.3**：保税仓储设施（Kawasan Berikat）进口报关。
* **BC 2.5**：保税区到国内海关区域（TLDDP / 本地市场）的出库报关。
* **BC 4.0**：从国内区域（TLDDP）到保税区的入库报关。
* **BC 4.1**：保税区出库至 TLDDP 的本地原产货物报关。
* **BC 2.7**：保税区之间或其他保税设施之间的货物转移。

![海关单证类型配置](/assets/images/djbc_document_type.png)
*图 2.1：BZone 中海关单证类型的配置。*

> [!WARNING]
> 系统正式上线运行后，请勿更改官方编码或登记类型，因为它涉及与海关系统的数据对账。

---

## DJBC 计量单位（UOM）标准化

印尼海关法规要求使用标准国际计量单位（如 `KGM` 表示千克、`PCE` 表示件、`MTR` 表示米）。

UOM 映射步骤：

1. 进入 **库存 > 配置 > 计量单位（Inventory > Configuration > Units of Measure）**。
2. 打开所需的计量单位。
3. 在 **DJBC 映射（DJBC Mapping）** 选项卡中，确保 **DJBC 编码（DJBC Code）** 字段已填写官方编码。

![UOM 与 DJBC 标准映射](/assets/images/djbc_uom_mapping.png)
*图 2.2：将内部计量单位映射到官方 DJBC 标准。*

---

## 包装管理

创建报关单时，包装类型规格（如托盘、桶、箱、袋）是必填项。

* 进入 **海关报表 > 主类型 > 包装（Laporan Bea Cukai > Master Type > Kemasan）**。
* 确认包装编码符合海关标准（如 `PX` 表示托盘，`BX` 表示箱）。

---

## 主报表期间（Report Period）

主报表期间提供预定义的日期范围，以简化和加速定期报表（如月度、季度或年度报表）的生成。此功能作为快捷方式，可自动填充报表向导（*Date Range Selector*）中的日期字段，**不用于**锁定操作事务或库存移动。

* 进入 **海关报表 > 配置 > 报表期间（Laporan Bea Cukai > Configuration > Report Period）**。
* 通过指定以下内容创建新期间：
  * **期间（Period）**：期间标识符（例如 `January 2025`、`Q1 2025`）。
  * **代码（Code）**：唯一期间代码（例如 `2025-01`）。
  * **开始日期（Date Start）**：期间起始日期。
  * **结束日期（Date End）**：期间截止日期。

> [!TIP]
> 通过报表向导（如 IT 库存变动报表）生成报表时，选择 **报表期间（Report Period）** 将自动填充 **起始日期（Date Start）** 和 **结束日期（Date End）** 字段。如需自定义查询区间，用户仍可手动输入或调整日期范围。