# 生产与物料清单（MRP）

生产模块（MRP）负责管理配方（*物料清单 Bills of Materials / BoM*）、工单和生产订单（*Manufacturing Orders / MO*）、车间物料转移以及从原材料到在制品（WIP）或成品的转化。

---

## 1. 生产工作流概览

```text
[ 配置物料清单 (BoM) ]
                  │
                  ▼
[ 创建生产订单 (MO) ]
                  │
                  ▼
[ 确认生产订单 (Confirm) ]
                  │
                  ▼
[ 将原材料转移至生产车间 ]
                  │
                  ▼
[ 执行与确认 (Produce All) ]
                  │
                  ▼
[ 原材料消耗与成品入库 ]
```

---

## 2. 定义物料清单（BoM）

*物料清单*（BoM）定义了生产特定成品所需的精确原材料清单和数量。

### BoM 设置步骤：

1. 打开 **生产（Manufacturing）** 模块。
2. 进入 **产品 → 物料清单（Products → Bills of Materials）**。
3. 点击 **新建（New）**。

![物料清单表单视图](../assets/images/mrp_bom_form.png)
*图 6.1：物料清单（BoM）配置表单。*

4. 填写 BoM 详情：
   * **产品（Product）**：选择目标成品或组件。
   * **数量（Quantity）**：参考批量大小（例如 `1.00 Unit` 或 `100.00 Units`）。
   * **参考（Reference）**：工程配方代码或版本标识。
   * **BoM 类型（BoM Type）**：选择 **Manufacture this product（生产此产品）**。
   * **组件选项卡（Components Tab）**：点击 **添加行（Add a line）** 列出组件物料及每个单位所需数量。
5. 点击 **保存（Save）**。

---

## 3. 创建与确认生产订单（MO）

*生产订单*（MO）代表授权在生产车间生产产品的工单。

### MO 创建步骤：

1. 进入 **作业 → 生产订单（Operations → Manufacturing Orders）**。
2. 点击 **新建（New）**。

![生产订单创建视图](../assets/images/mrp_mo_creation.png)
*图 6.2：生产订单创建表单。*

3. 设置生产参数：
   * **产品（Product）**：选择要生产的产品。
   * **数量（Quantity）**：输入生产数量。
   * **物料清单（Bill of Material）**：选择相关的 BoM。所需组件将自动填充。
4. 点击 **确认（Confirm）**。单据状态变为 **已确认（Confirmed）**。

---

## 4. 转移原材料（物料转移）

为保持物理和数字库存的准确性：

1. 检查组件库存可用性。
2. 如需将原材料从中央存储转移到生产车间：
   * 进入转移订单或点击 **转移（Transfer）**。
   * 校验 **源库位（Source Location）**（如 `WH/Raw Materials`）和 **目的库位（Destination Location）**（如 `WH/Production Floor`）。
   * 点击 **确认（Validate）** 执行实际转移。

![物料转移至生产车间](../assets/images/mrp_material_transfer.png)
*图 6.3：确认组件转移至生产车间。*

---

## 5. 完成生产与库存验证（Produce All）

1. 返回生产订单。
2. 输入实际成品数量。
3. 点击顶部的 **全部生产（Produce All）**。

![完成生产并验证库存](../assets/images/mrp_produce_all_done.png)
*图 6.4：执行全部生产（Produce All）并验证在手成品库存。*

4. 生产订单状态变为 **已完成（Done）**：
   * 原材料从仓库记录中消耗。
   * 成品增加到在手库存中。
5. 验证在手成品：
   * 进入 **产品 → 产品（Products → Products）**。
   * 打开新生产的产品卡片。
   * **在手数量（On Hand）** 指标会反映更新后的库存数量。

> [!NOTE]
> 生产订单的组件消耗与成品产出直接计入官方 IT 库存 **原材料变动报表** 和 **成品变动报表**。