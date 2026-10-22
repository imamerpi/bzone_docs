# 收货与发货（库存）

> **最后更新：** 22 Oktober 2026

库存模块控制所有出入库物料的物理移动。在 BZone 生态系统中，每一次仓库移动都可以直接关联到官方海关申报单证编号（BC 2.3、BC 4.0、BC 2.5、BC 4.1 或 PEB），以确保 IT 库存审计的实时合规。

---

## 1. 库存移动概览

```text
    [ 采购入库 (PO) ]                  [ 销售出库 (SO) ]
                 │                                │
                 ▼                                ▼
   [ 入库收货 (Receipts) ]         [ 出库发货订单 (Delivery Orders) ]
                 │                                │
                 ├─────────────────────────────────┤
                 ▼                                ▼
   [ 校验目的库位 ]                  [ 校验收货地址 ]
                 │                                │
                 ├─────────────────────────────────┤
                 ▼                                ▼
   [ 选择海关单证关联 ]              [ 选择海关单证关联 ]
                 │                                │
                 ▼                                ▼
          [ 点击确认 ]                       [ 点击确认 ]
                 │                                │
                 ▼                                ▼
    (完成 / 库存入库)             (完成 / 库存发货给客户)
```

---

## 2. 入库收货（Incoming Receipts）

入库收货记录从供应商处收到的原材料、消耗品或成品的实际到货情况。

### 收货确认步骤：

1. 打开 **库存（Inventory）** 模块。
2. 在 **收货（Receipts）** 操作卡片中，点击待处理任务计数器（例如 **X To Receive**）。
3. 打开目标收货单据（可按采购订单源单号搜索，例如 `P00003`）。

![入库收货表单](../assets/images/inventory_receipt_incoming.png)
*图 5.1：入库收货表单，含仓库库位和海关单证参考。*

4. 检查核心字段：
   * **收货来源（Receive From）**：供应商名称。
   * **目的库位（Destination Location）**：收货仓库区域（例如 `WH/Stock` 或 `WH/Raw Materials`）。
   * **海关单证（Customs Document）**：选择关联的海关单证（如保税进口的 BC 2.3 或国内入库的 BC 4.0）。
5. 核对 **完成数量（Quantity Done）** 列中的实际数量。
6. 点击 **确认（Validate）**。
7. 状态更新为 **已完成（Done）**。库存余额立即增加，入库变动记录记入 IT 库存。

---

## 3. 出库发货订单（Outgoing Deliveries）

发货订单记录向客户发出的成品或商品。

### 发货确认步骤：

1. 打开 **库存（Inventory）** 模块。
2. 在 **发货订单（Delivery Orders）** 卡片中，点击发货队列（例如 **X To Deliver**）。
3. 选择状态为 **就绪（Ready）** 的订单（确认仓库库存已预留）。

![出库发货订单表单](../assets/images/inventory_delivery_outgoing.png)
*图 5.2：客户出库发货订单表单。*

4. 校验发货详情：
   * **收货地址（Delivery Address）**：目的地客户地址。
   * **源库位（Source Location）**：拣货仓库库位（例如 `WH/Stock` 或 `WH/Finished Goods`）。
   * **海关单证（Customs Document）**：选择官方出库报关单（如本地销售的 BC 2.5 或出口的 BC 3.0 PEB）。
5. 核对 **完成数量（Quantity Done）** 列中已装载的项目。
6. 点击 **确认（Validate）**。
7. 状态更新为 **已完成（Done）**。仓库库存减少，出库记录记入 IT 库存。

> [!IMPORTANT]
> 在点击 **确认（Validate）** 之前选择合适的 **海关单证（Customs Document）** 是维护海关审计不可中断的数字审计追踪的关键。