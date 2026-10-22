# CEISA 进口模块（djbc_ceisa_impor）

> **最后更新：** 22 Oktober 2026

**`djbc_ceisa_impor`** 模块管理 **进口报关单（BC 2.0 / PIB）** 并将其提交到印度尼西亚海关与消费税总局（DJBC）的 CEISA 4.0 门户。本文档是进出口运营、后勤、保税报关行（PPJK）和公司海关工作人员的用户指南。

> [!IMPORTANT]
> 根据 `__manifest__.py` 中的实现和 `ceisa.import.document` 模型中的 `_check_user_kode_dokumen_permission` 验证，**本模块仅支持 BC 2.0 单证**（参考代码 `20`）。对于 BC 2.3（进口到保税区）和 BC 2.7（TPB 间），请使用 [`djbc_ceisa_tpb`](../djbc_ceisa_tpb/pengelolaan_dokumen_tpb.md) 模块。

---

## 1. 模块范围与单证类型

| BC 编码 | 单证类型 | 用途 |
|---|---|---|
| **BC 2.0** | 进口报关（PIB） | 从海关区域外（LDP）到海港或机场临时存储设施（TPS）的一般进口，有义务支付进口关税、增值税、第 22 条所得税和其他适用税款（消费税、第 23 条等）。 |

> [!TIP]
> 本模块生成相当于 DJBC 在 CEISA 4.0 中主机到主机应用程序的 PIB 单证。在开始数据输入之前，请确保您具有有效的 **DJBC 公司代码** 和 **公司 NPWP**。

---

## 2. 进口单证表单解析

BC 2.0 单证按 CEISA 标准组织成多个选项卡：

```mermaid
graph TD
    A["标题与 26 位申请号"] --> B["海关实体（进口商、货主、发货人、卖方）"]
    A --> C["支持性单证（发票、B/L、AWB 等）"]
    A --> D["运输方式与港口"]
    A --> E["包装与集装箱"]
    A --> F["货物、关税与进口条件"]
    A --> G["授权签字人声明"]
```

---

## 3. 单证生命周期

| 状态 | 描述 | 可用操作 |
|---|---|---|
| **草稿（Draft）** | 新单证 / 正在准备。所有字段可自由编辑。 | 编辑所有字段、添加商品、自动填充 |
| **确认（Confirm）** | 数据内部有效。状态锁定以供提交。 | **发送到 CEISA**、设置为草稿 |
| **已发送（Sent）** | 单证已成功传输到 CEISA 并收到 NPP。 | **检查状态与响应**、**开始修订** |

> [!WARNING]
> **确认 → 已发送** 转换仅当所有 `check_*` 验证（请参见第 5 节）通过时才会发生。否则，系统将引发 **UserError**，列出不完整的字段。

---

## 4. 分步填写流程

打开 **DJBC CEISA > 进口 > 进口单证（DJBC CEISA > Import > Import Documents）** 菜单，然后单击 **创建（Create）**。

### 步骤 1：标题选项卡

* **26 位申请号（Nomor Aju）** - 当您选择 **海关办公室（Customs Office）**（`kode_kantor`）和 **单证代码（Document Code）**（`kode_dokumen`）时自动生成。模式：`[4 位办公室][2 位 BC][6 位公司代码][14 位序列]`。示例：`0403002000012325101000012345`。
* **海关办公室** - 根据 TPS 所在地的监管办公室选择。
* **装货港、目的地港、TPS** - 全部必填。
* **Tutup PU 代码** - 必须为以下之一：`11`、`12` 或 `14`。其他代码将被 `check_detail_dokumen` 拒绝。
* **货币代码与 NDPBM** - 选择货币时自动填充每周 KMK 税率。请参阅 [汇率指南](../erpi_custom_rate/manajemen_kurs_pabean.md)。

  ![进口标题](/assets/images/ceisa_impor_header.png)

### 步骤 2：实体选项卡

创建单证时，系统 **自动生成 4 个实体行**，遵循 BC 2.0 标准：

| 系列 | 代码 | 角色 | 自动填充 |
|---|---|---|---|
| 1 | `1` | **进口商（Importer）** | 是，来自 `company.partner_id` |
| 2 | `7` | **货主（Cargo Owner）** | 否 |
| 3 | `9` | **发货人（Shipper）** | 否 |
| 4 | `10` | **卖方（Seller）** | 否 |

进口商实体的 **必填** 字段：名称、地址、16 位 NPWP、NIB、**API 类型**（`01` APIU 或 `02` APIP）以及 **身份类型**（代码 2/3/4/5/6）。

货主的 **必填** 字段：名称、地址、身份编号和 **关联（Affiliation）**（`AFL`、`CTR`、`FAM` 或 `TAH`）。

  ![进口实体选项卡](/assets/images/ceisa_impor_entitas.png)

### 步骤 3：支持性单证选项卡

根据 CEISA 参考代码选择支持性单证类型：

| 代码 | 单证 |
|---|---|
| `380` | 商业发票 |
| `705` | 提单（B/L） |
| `740` | 空运单（AWB） |
| `217` | 装箱单 |
| `861` | 原产地证书（SKA） |
| `261` | 销售合同 |
| `731` | 支持性海关单证（例如，来自 TPS 的 BC 1.6） |

  ![进口支持性单证](/assets/images/ceisa_impor_dokumen_pelengkap.png)

### 步骤 4：运输、包装、集装箱选项卡

* **运输方式** - 选择方式（海运 / 空运 / 陆运）以及车辆 / 船舶名称。
* **国家旗帜代码** - 2 个大写 ISO 字母（例如 `SG`、`CN`、`JP`、`US`）。
* **集装箱编号** - 11 位（前缀 4 个字母 + 7 位数字），包括尺寸（`20` / `40` / `45` 英尺）。
* **包装数量和类型** - CEISA 单位，例如 `CTN`（纸箱）、`PLT`（托盘）、`BG`（袋）、`DR`（桶）。

  ![进口运输与集装箱](/assets/images/ceisa_impor_pengangkut.png)

### 步骤 5：货物选项卡（商品详情）

* 从主数据选择 **产品** → 系统从 `product.product` 自动填充 **HS 编码**（`pos_tarif`）和 **CEISA 单位**。
* 输入每个行的 **数量** 和 **CIF 值**。
* 选择 **海关估值方法**：方法 1 到 6。如果不是方法 1，请在 `alasan_metode_penentuan_nilai` 中填写 **原因**。
* 填写 HS 编码后，**货物关税** 选项卡自动填充 —— 请参阅下方 [从 HS 编码自动获取关税](#51-从-hs-编码自动获取关税)。
* 如需要，可填写 **特殊规格（Special Specification）** 选项卡（用于某些 HS 类别）和 **自愿声明（Voluntary Declaration）**。

  ![进口货物详情](/assets/images/ceisa_impor_barang.png)

#### 5.1 从 HS 编码自动获取关税

每次选择 **产品** 或 **Pos Tarif / HS** 时，系统会从 [CEISA Core](../djbc_ceisa_core/integrasi_ceisa_core.md) 拉取官方关税，并自动填充 **货物关税** 选项卡的 3 个必填行：**BM** → **PPH** → **PPN**。

##### 自动填充触发条件

| 操作 | 结果 |
| :--- | :--- |
| 在货物选项卡选择 **产品** | HS 编码跟随 `product.hs_code_id`，立即拉取关税 |
| 选择 / 更改 **Pos Tarif / HS** | Pos Tarif 同步更新，重新拉取关税 |
| 清空 **Pos Tarif / HS** | **删除所有货物关税行** |
| 点击 **Tarik / Refresh Tarif CEISA** | 从 CEISA 重新拉取，绕过本地缓存 |

> [!IMPORTANT]
> 在此行手动设置 **Pos Tarif / HS** **不会**修改产品主数据中的 HS 编码。该字段链接到
> `product.hs_code_id` —— 请从 **产品** 字段修改，以确保主数据保持一致。

##### 计算公式

##### 海关估值与税费计算公式

| 组成部分 | 计算公式 | 说明 |
| :--- | :--- | :--- |
| **货物 CIF 金额** | = FOB + 运费 + 保险费 *(或 单价 x 数量)* | 外币海关计税基础 |
| **海关完税价格（DPP BM）** | = CIF x NDPBM（税收汇率） | 折算印尼盾值（四舍五入） |
| **进口关税（BM）** | = DPP BM x 关税税率 (%) | 从价税；或 若为从量税则 = 数量 x 从量单价 |
| **进口计税基础（DPP Impor）** | = 海关完税价格（CIF 印尼盾） + 进口关税 | 所得税与增值税的计税基础 |
| **第 22 条所得税（PPh）** | = DPP Impor x PPh 税率 (%) | 进口商有 API/NIB 时为 2.5%；无 API 时为 7.5% |
| **进口增值税（PPN）** | = DPP Impor x 增值税率 (%) | 法定现行税率（11% 或根据 HS 编码） |

> [!NOTE]
> 当 `kode_jenis_tarif` = `2`（从量税）时，计算采用 **数量 x 从量单价**。

##### PPh 第 22 条税率根据 API 状态自动确定

PPh 税率按公司资质自动判定：

1. 公司联系人主数据中的 `partner.kode_jenis_api` 或 `partner.nib`
2. `company.nib`
3. 单证中具有进口商（代码 `1`）、企业家（代码 `3`）或货主（代码 `7`）角色的实体

| 公司资质情况 | PPh 第 22 条税率 |
| :--- | :--- |
| 持有 NIB 或 API 类型（API-U / API-P） | **2.5%** |
| 既无 NIB 也无 API | **7.5%** |

> [!TIP]
> 持有免税证明（**SKB**）的企业适用 **0%**。若持有 SKB，可手动将该行税率改为 0%。

##### 便利化与应缴金额划分

* **已缴纳便利化（`kode_fasilitas_tarif` = `1`）**：税额计入 **Nilai Bayar**（应付金额），`Nilai Fasilitas` = 0。
* **免征 / 暂停征收便利化（`kode_fasilitas_tarif` 非 `1`）**：税额计入 **Nilai Fasilitas**（免税金额），`Nilai Bayar` = 0，且便利化税率设为 100%。
* **Total Pungutan（总征收）**：该货物各税种行的（应缴金额 + 免税金额）合计。

---

#### 5.2 汇率（NDPBM）更新与重新计算机制

财政部每周定期发布海关基准汇率（NDPBM）。系统提供全自动级联同步：

1. **从 CEISA 自动拉取汇率**：
   - 点击交易选项卡中 NDPBM 字段旁的刷新按钮（**Tarik Kurs Terkini dari CEISA**）。
   - 系统直接从 CEISA 4.0 服务器获取官方生效的税收汇率。
2. **级联联动重算（Cascading Recompute）**：
   - 每当汇率 / NDPBM 发生变动（无论是从 CEISA 拉取还是手动修改）：
     - 货物选项卡下的所有明细行自动更新汇率（`ndpbm`）。
     - **海关完税价格（CIF 印尼盾）** 自动根据新汇率重新计算。
     - 所有税费（**进口关税、PPh 22、增值税等**）基于新的完税价格自动重算。
     - 未填充税费的行将自动从 HS 编码关联标准提取。
3. **单证级总额汇总（Header Summary）**：
   - 单证总 **CIF** 自动汇总所有货物明细行的 CIF 金额。
   - 在货物明细列表下方，实时显示应缴税费汇总卡片：
     - **Total BM**（关税总额）
     - **Total PPN**（进口增值税总额）
     - **Total PPh**（所得税总额）
     - **Total Bayar Pungutan**（需向国库缴纳的税费总额）
     - **Total Pungutan Fasilitas**（享受免税/暂缓的税费总额）

---

#### 5.3 海关估值方法（BC 2.0 报关单 D.36 列）

根据**印尼海关法 2006 年第 17 号第 15 条**及**财政部 PMK 144/PMK.04/2022 号规程**，每项进口商品均须申报海关估值方法：

##### 1. 估值方法层级

* **方法 1（进口货物成交价格法）** *(系统默认)*：
  - 适用于绝大部分正常商业买卖，依据商业发票（*Commercial Invoice*）上的实际支付或应付价格。
* **方法 2（相同货物成交价格法）**：
  - 适用于无正常商业买卖时（如赠品/样品），参考同时期进口的相同货物的成交价格。
* **方法 3（类似货物成交价格法）**：
  - 参考同时期进口的类似货物（*similar goods*）的成交价格。
* **方法 4（倒扣价格法/演绎法）**：
  - 以该货物在印尼国内市场的转售价格扣除利润、国内运输费用及税费倒推计算 CIF。
* **方法 5（计算价格法）**：
  - 以境外制造原料成本、生产加工费加上正常利润正向汇总计算 CIF。
* **方法 6（合理方法/推演法）**：
  - 基于印尼境内可获得的数据，合理灵活运用上述方法估算。

##### 2. 非方法 1 强制填报原因规则

* 选择 **方法 1** 时：海关估值原因字段自动隐藏且无需填写。
* 选择其他方法（**方法 2 至 6**）时：**Alasan Penentuan Nilai** 变为**必填项（Required）**。若未填写原因，系统在保存和发送时将进行验证拦截，以防止报关单被 CEISA 系统驳回。

支持的原因代码：
* `KON`：非买卖寄售货物（Consignment）。
* `CMA`：非买卖礼品、促销品或样品。
* `ITM`：非买卖由不购买货物的中介代理商进口。
* `LES`：非买卖租赁货物（Leasing）。
* `HBH`：非买卖援助品或捐赠品。
* `BTR`：其他非买卖交易。
* `TTS`：买卖交易但不符合成交价格条件（例如存在特殊限制条件或关联方定价影响）。

---

### 步骤 6：签字人选项卡

* **签字城市（Signature City）** - 从 `company.partner_id.city_id` 或 `company.city_id` 自动填充。
* **签字人姓名（Signatory Name）** - 从 `hr.employee` 中选择（自动填充 **职位**）。
* **免责声明（Disclaimer）** - 设置为 `1`（是）以声明批准。

---

## 5. 自动提交前验证

在状态从 **草稿 → 确认 → 已发送** 改变之前，系统运行 **5 个验证函数**（`ceisa.import.document` 中的 `check_*`）：

| 函数 | 执行的验证 |
|---|---|
| `check_detail_dokumen` | 26 位申请号、海关办公室、装货 / 目的地港 / TPS、货币、付款方式、Tutup PU 代码（11/12/14）、进口类型、到达日期 / 签字日期、签字城市 / 姓名 / 职位。 |
| `check_entitas` | 进口商（1）和货主（7）实体是必填的。进口商必须具有名称、地址、NPWP、NIB、API（01/02）和身份类型。 |
| `check_dokumen_pelengkap` | 最少支持性单证（发票和 B/L/AWB）。 |
| `check_kemasan_pengangkut` | 已填写包装和运输数据。 |
| `check_barang` | 每个项目都有 HS 编码、单位、价值和 `jenis_kemasan`。 |

如果其中任何一个失败，单证将不会过渡到 `确认`。使用 **确认（Confirm）** 按钮触发验证，然后更正标记的字段。

---

## 6. 将单证发送到 CEISA（发送单证）

1. 单击 **确认（Confirm）** —— 状态变为 `Confirm`。
2. 单击 **发送单证（Send Document）** —— 系统 POST 到端点：
   ```
   POST {CEISA_BASE_URL}/openapi/document?isFinal=false&isRevision=false
   ```
3. 正文以 **JSON** 形式发送，由 `_prepare_ceisa_import_data()` 组装（包含标题、实体、货物、goodsTarif、goodsDokumen、goodsPemilik 等）。
4. 如果服务器返回 **401 Unauthorized**，系统会自动通过 `ceisa.auth.get_auth_headers(force=True)` 刷新令牌并重试。
5. 成功响应 → 状态变为 `Sent`，JSON 有效负载记录在 `ir.api.log` 中（通过 **API 日志（Api Log）** 按钮），并将通知电子邮件发送给用户（如果 `ceisa.send_email_on_success=True`）。

> [!TIP]
> `isFinal` 参数由系统参数 `ceisa.is_final_dokumen` 控制。默认为 `false`（草稿）。如果要单证在第一次提交时定稿，请将其设置为 `true`。

---

## 7. 修订已发送的单证

**开始修订（Start Revision）** 按钮仅适用于 `Sent` 状态的单证。系统将：

1. 增加标题中的 `revision_count` 和 `seri`。
2. 将状态恢复为 `Draft`。
3. 设置 `is_revision = True`，以便服务器在重新提交时收到 `isRevision=true`。

---

## 8. 关键术语

| 术语 | 描述 |
|---|---|
| **PIB (BC 2.0)** | Pemberitahuan Impor Barang —— 一般进口的官方海关申报。 |
| **NPP** | Nomor Pendaftaran Pabean —— 单证已在 CEISA 中登记的证明。 |
| **SPPB** | Surat Persetujuan Pengeluaran Barang —— 来自 TPS 的放行许可。 |
| **SPJM** | Surat Pemberitahuan Jalur Merah —— 实物检查通知。 |
| **NDPBM** | Nilai Dasar Perhitungan Bea Masuk —— 每周 KMK 税务汇率。 |
| **FOB / CIF / 运费 / 保险** | 海关价值组成部分（请参阅 TPB 指南以获取完整公式）。 |
| **HS 编码** | 基于 BTKI（印度尼西亚海关税则）的 8 位编码。 |

---

> 本文档是 **`djbc_ceisa_impor`** 模块的非技术指南。对于出口，请参阅 [CEISA 出口文档](../djbc_ceisa_expor/dokumentasi_expor.md)。对于 TPB/BC 2.3/BC 4.0/BC 2.5 交易，请使用 [`djbc_ceisa_tpb`](../djbc_ceisa_tpb/pengelolaan_dokumen_tpb.md) 模块。