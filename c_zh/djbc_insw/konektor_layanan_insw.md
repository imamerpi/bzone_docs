# INSW IT 库存集成连接器（djbc_insw）

**`djbc_insw`**（INSW 连接器）模块通过标准 IT 库存 REST API（`https://api.insw.go.id`）将 BZone ERP 生态系统直接对接财政部下属的 **国家单一窗口机构（Lembaga National Single Window / LNSW）** 门户。

---

## 1. 集成概述

印度尼西亚政府要求在保税区、经济特区（KEK）及其他海关监管设施内运营的公司，将其运营 IT 库存数据与中央国家 INSW 门户同步。

提供的主要功能：

1. **自动期初余额传输**：传输原材料、成品、机械和生产废料的期初库存截止记录。
2. **实时交易同步**：传输与保护性海关报关单关联的入库收货和出库发货（`stock.picking`）。
3. **库存盘点上报**：直接将年度物理盘点对账结果提交到 INSW 服务器。
4. **海关单证更新**：更新先前发送的 INSW 交易上的保护性海关单证。
5. **数据检索与审计追踪**：从 INSW 服务器拉取交易以进行对账、清理和期初余额定稿。

---

## 2. Odoo 中的菜单结构

根据 `djbc_insw` 模块的 `views/menu.xml`，Odoo 中的菜单结构为：

```
INSW 连接器                                       (djbc_insw_menu_root)
├── 参考资料                                      (djbc_insw_menu_referensi)
│   ├── 交易类型
│   ├── 商品类别
│   ├── 海关单证
│   ├── 单位
│   └── 业务活动
├── 数据拉取                                      (djbc_insw_menu_penarikan_data)
│   ├── 更新海关单证                              (menu_insw_update_dokumen)
│   ├── 交易                                      (insw_transaksi_menu)
│   ├── 交易商品                                  (insw_barang_transaksi_menu)
│   ├── 获取业务活动                              (insw_kegiatan_wizard_menu) — group_system
│   ├── 清理测试数据                              (insw_cleansing_wizard_menu) — group_system
│   └── 定稿期初余额                              (insw_finalize_sa_wizard_menu) — group_system
└── 配置                                          (djbc_insw_menu_configuration)
    └── API 日志                                  (insw_api_log_menu)
```

> [!NOTE]
> **期初余额（Saldo Awal）** 菜单（`insw_saldo_awal_menu`）位于 **库存 > 作业 > 调整（Inventory > Operations > Adjustments）**（父菜单 `stock.menu_stock_adjustments`）之下，而不是在 INSW 连接器根菜单下。这是因为期初余额单证是官方的 Odoo 库存调整单证。

---

## 3. 配置 INSW API 凭证

1. 进入 **INSW 连接器 > 配置 > 设置（INSW Connector > Configuration > Settings）**（或 `djbc_app` 模块的 **库存 > 配置 > 设置（Inventory > Configuration > Settings）**）。
2. 在 **设置 INSW API（Setting INSW API）** 下：
   * **INSW 密钥（INSW Key）**：来自 LNSW 的 `x-insw-key` 授权令牌。
   * **唯一密钥（Unique Key）**：`x-unique-key` 头令牌。
   * **API 环境（API Environment）**：`dev` / `testing`（沙盒）或 `prod` / `production`。
3. 在 **INSW 附加设置（INSW Additional Setting）** 下：
   * **翻转海关单证号（Flip Pabean Number）**：如果贵公司的内部编号将拣货参考与海关单证参考反转，则启用。
4. 点击 **保存（Save）**。

```
+-------------------------------------------------------------+
| 设置 INSW API                                              |
| INSW 密钥:      [ ****************************** ]          |
| 唯一密钥:        [ ****************************** ]          |
| API 环境:        [ 生产 / 测试 ]                            |
| [x] 翻转海关单证号                                          |
+-------------------------------------------------------------+
```

---

## 4. 主数据与 INSW 标准映射

在任何传输之前，Odoo 主记录必须通过 **INSW 连接器 > 参考资料（INSW Connector > Referensi）** 菜单映射到官方 INSW 参考：

| 参考菜单 | 模型 | 填写时机 |
|---|---|---|
| **交易类型（Jenis Transaksi）** | `insw.ref.jenis.transaksi` | 从 INSW 服务器拉取的主数据（只读）。 |
| **商品类别（Kategori Barang）** | `insw.ref.kategori.barang` | 用于映射 `product.category`（`INSW Kategori`）：`BAHAN BAKU`、`BARANG JADI`、`MESIN` 等。 |
| **海关单证（Dokumen Pabean）** | `insw.ref.dokumen.pabean` | 将 `djbc_app.document_type` 映射到 INSW 编码：`23`（BC 2.3）、`25`（BC 2.5）、`40`（BC 4.0）等。 |
| **单位（Satuan）** | `insw.ref.satuan` | 映射 `uom.uom`（`INSW Satuan`）：`PCE`、`KGM`、`LTR` 等。 |
| **业务活动（Kegiatan）** | `insw.ref.kegiatan` | 标准业务代码主数据（`INCOMING`、`OUTGOING`、`SALDO AWAL`）。 |

---

## 5. 期初余额提交（Saldo Awal）

1. 打开 **库存 > 作业 > 调整 > 期初余额（Inventory > Operations > Adjustments > Saldo Awal）**（菜单 `insw_saldo_awal_menu`）。
2. 点击 **创建（Create）** —— 新单据初始化为 **草稿（Draft）** 状态。
3. 在 **产品（Products）** 选项卡上填写日期和产品行（`line_ids`）：
   * `product_id`（必须 `is_storable=True`）、描述、`uom_id`、`qty_available`（只读）、`list_price`。
4. 点击 **确认（Confirm）** → 状态变为 `confirm`。
5. 点击 **发送到 INSW（Send to INSW）** → 系统将 POST 到期初余额端点。状态变为 `sent`。

> [!TIP]
> **API 日志（Api Log）** 智能按钮（左上角）一旦发生传输就会显示。可用它进行审计追踪。

---

## 6. 入库与出库移动传输（Stock Picking）

传输与 `djbc_app` 和 `stock` 的 `stock.picking` 表单集成：

1. 在 **库存 > 作业 > 转移（Inventory > Operations > Transfers）** 中打开仓库转移或在 **拣货（Stock Picking）** 表单上打开。
2. 确保 **海关单证（Customs Document）** 字段（`djbc_custom_document_id`）已填写。
3. 当 `djbc_kek_movement_type ∈ ('in', 'out')` 时，会出现 **业务活动（Kegiatan）** 选项卡。选择：
   * `kegiatan_id`（领域编码 `30` / `31`）。
   * 这将自动填充 `name`（活动编号）和 `create_date`（活动日期）。
4. 点击 **发送到 INSW（Send to INSW）**（`send_insw` 按钮）：
   * 当 `djbc_kek_movement_type ∈ ('in', 'out')`、`state = 'done'` 且尚未发送（`insw_trx_id` 为空、`insw_status != 200`）时可见。
   * BZone 构建 JSON 有效负载（NPWP、NIB、活动、商品、附加的海关单证）并 POST 到 `https://api.insw.go.id/api/inventory/transaksi`。
5. `insw_trx_id` 和 `insw_status` 字段会自动填充。`insw_status` 徽章出现在列表视图中（`200` 绿色，`400/401/404` 红色）。

### 提交后更新海关单证

如果 INSW 交易已发送，而您需要 **更新保护性海关单证**（例如单证编号/日期已更改），请使用 **更新 INSW 海关单证（Update Dokumen Pabean INSW）** 向导：

1. 单击拣货表单上的 **更新 INSW 海关单证（Update Dok. Pabean INSW）** 按钮（当 `insw_trx_id` 已填写且 `state = 'done'` 时可见）。
2. 选择 **交易（Transaction）**（`trx_id`）和 **模式（Mode）**（`single` / `bulk`）。
3. 对于 `single` 模式，选择相关的 **交易商品（Barang Transaksi）**（`barang_trx_id`）。
4. 选择 **海关单证（Customs Document）**（`dokumen_id`），填写 `nomor_dokumen` 和 `tanggal_dokumen`。
5. 点击 **发送到 INSW（Kirim ke INSW）**。

> 或直接通过 **INSW 连接器 > 数据拉取 > 更新海关单证（INSW Connector > Penarikan Data > Update Dokumen Pabean）** 访问。

---

## 7. 向 INSW 上报物理库存盘点

`djbc_insw` 模块继承 `djbc_stock_opname.view_stock_opname_form` 并新增 2 个按钮：

| 按钮 | 可见性 | 功能 |
|---|---|---|
| **INSW 库存盘点（INSW Stock Opname）** | `state = 'done'`、尚未盘点（`is_inventoried = False`）、`so_trx_id` 为空 | 将库存盘点数据发送到 INSW。 |
| **INSW 调整（INSW Adjustment）** | `state = 'done'`、已盘点（`is_inventoried = True`）、`adj_trx_id` 为空 | 将物理调整数据发送到 INSW。 |

发送后，`so_trx_id` 或 `adj_trx_id` 字段将在 **INSW 信息（INSW Info）** 选项卡上填充。

附加字段：`unit_price`（每个产品行）和 `partner_id`（完成后只读）。

---

## 8. 数据检索、审计追踪与系统工具

### 8.1. 获取业务活动向导（`insw.kegiatan.wizard`）
要按日期范围和活动类型从 INSW 服务器检索活动数据：

* 打开 **INSW 连接器 > 数据拉取 > 获取业务活动（INSW Connector > Penarikan Data > Get Kegiatan）**（组 `base.group_system`）。
* 填写 `tgl_awal`、`tgl_akhir` 和 `kegiatan_id`。
* 点击 **确认（Confirm）** —— 系统通过 API 拉取数据。

> 表单根据 `is_production` 显示 dev/prod 横幅。

### 8.2. 清理测试数据向导（`insw.cleansing.wizard`）
要从 INSW 服务器清理测试数据：

* 打开 **INSW 连接器 > 数据拉取 > 清理测试数据（INSW Connector > Penarikan Data > Cleansing Data Percobaan）**（组 `base.group_system`）。
* 填写公司 `npwp`。
* 点击 **确认（Confirm）**。

### 8.3. 定稿期初余额向导（`insw.finalize.sa.wizard`）
要将期初余额标记为已定稿（仅 dev/测试）：

* 打开 **INSW 连接器 > 数据拉取 > 定稿期初余额（INSW Connector > Penarikan Data > Finalisasi Saldo Awal）**（组 `base.group_system`）。
* 填写 `npwp`。
* **确认（Confirm）** 按钮仅在 `is_production = False` 时显示。

### 8.4. 交易菜单（`insw.transaksi`）
显示从 INSW 拉取的数据的只读列表和表单。关键字段：`no_dok_kegiatan`、`name`（交易 ID）、`kegiatan_id`、`tgl_kegiatan`、`nama_entitas`。**交易商品（Barang Transaksi）** 选项卡包含商品列表（`trx_ids`）。

### 8.5. 交易商品菜单（`insw.barang.transaksi`）
每个商品的只读列表和表单。**单证（Dokumen）** 选项卡显示附加的海关单证列表。

### 8.6. API 日志
打开 **INSW 连接器 > 配置 > API 日志（INSW Connector > Configuration > API Log）** 查看所有 REST API 通信。智能按钮快捷方式也可在以下位置找到：
* **拣货（Stock Picking）**（智能按钮 `api_log_count`）。
* **库存盘点（Stock Opname）**（智能按钮 `api_log_count`）。
* **期初余额（Saldo Awal）**（智能按钮 `api_log_count`）。

API 日志历史包括：时间戳、端点 URL、JSON 请求、响应代码和消息。

---

## 9. 关键术语

| 术语 | 描述 |
|---|---|
| **LNSW** | Lembaga National Single Window —— INSW 门户的运营机构。 |
| **INSW** | Indonesia National Single Window —— 跨部委的进出口许可平台。 |
| **期初余额（Saldo Awal）** | 首次集成时报告的初始库存头寸。 |
| **库存盘点（Stock Opname, SO）** | 物理库存验证的结果。 |
| **调整（Adjustment, Adj）** | 物理与系统差异的对账。 |
| **清理（Cleansing）** | 从 INSW 服务器删除测试数据。 |
| **定稿（Finalisasi）** | 将期初余额标记为已定稿。 |
| **业务活动（Kegiatan）** | 移动类型：`INCOMING`、`OUTGOING` 或 `SALDO AWAL`。 |