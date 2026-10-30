# CEISA 4.0 基础集成（djbc_ceisa_core）

**`djbc_ceisa_core`** 模块作为主要集成网关，通过 RESTful OpenAPI 将 Odoo 中的所有 BZone 海关作业与印度尼西亚海关与消费税总局（DJBC）的 **CEISA 4.0（海关-消费税信息系统与自动化）** 生态系统连接。

---

## 架构与核心作用

本模块充当认证、国家海关参考数据同步以及跨所有专业海关服务模块的端到端 API 审计日志的中央枢纽：

```mermaid
graph TD
    subgraph Odoo ERP 与 BZone
        CORE["djbc_ceisa_core（网关与认证）"]
        TPB["djbc_ceisa_tpb（保税设施）"]
        IMP["djbc_ceisa_impor（进口 PIB）"]
        EXP["djbc_ceisa_expor（出口 PEB）"]
        FTZ["djbc_ceisa_ftz（自由贸易区）"]
        TPB --> CORE
        IMP --> CORE
        EXP --> CORE
        FTZ --> CORE
    end
    subgraph 海关门户
        CEISA["CEISA 4.0 OpenAPI 门户"]
        AUTH["SSO 与令牌服务"]
        REF["参考服务（汇率、税则、Lartas）"]
        DOC["海关单证提交端点"]
        CORE --> AUTH
        CORE --> REF
        CORE --> DOC
    end
```

### 依赖服务模块：
* **`djbc_ceisa_tpb`**：保税仓储设施报关（保税区、保税仓库等）。
* **`djbc_ceisa_impor`**：进口货物报关（PIB / BC 2.0）。
* **`djbc_ceisa_expor`**：出口货物报关（PEB / BC 3.0）。
* **`djbc_ceisa_ftz`**：自由贸易区海关报关（PPFTZ）。
* **`djbc_ceisa_barang_kiriman`**：包裹寄送申报。

---

## 凭证配置与认证

要启用与海关 OpenAPI 的通信，管理员必须在 **CEISA 连接器 > 配置 > 设置（CEISA Connector > Configuration > Settings）** 下设置连接参数：

### 1. CEISA OpenAPI 参数
* **用户名（Username）**：海关门户的用户账号标识符。
* **密码（Password）**：海关门户的密码。
* **海关 API 密钥（Bea Cukai API Key）**：在 CEISA OpenAPI 门户（`https://openapi.beacukai.go.id`）的用户配置文件下生成的唯一 API 密钥。
* **CEISA API URL（CEISA API URL）**：官方基础 API 端点（例如生产环境为 `https://openapi.beacukai.go.id`）。
* **公司代码（Kode Perusahaan）**：印度尼西亚海关签发的 6 位公司代码（用于构造 26 位海关申请号 / *Nomor Aju*）。

### 2. OAuth2 令牌生命周期管理
* 认证使用 Bearer 令牌协议（`/v1/openapi-auth/user/login`）。
* 访问令牌有效期为 300 秒（5 分钟），而刷新令牌保持有效期为 24 小时。
* BZone 自主监控令牌过期，并在过期前 30 秒的安全缓冲触发 **自动刷新（Auto-Refresh）**。

---

## 参考数据同步服务

`djbc_ceisa_core` 包含自动化向导，可直接从海关服务器获取官方主数据：

### 1. 每周海关汇率
* 菜单：**CEISA 连接器 > CEISA 服务 > 同步汇率（CEISA Connector > Layanan CEISA > Sinkronisasi Kurs）**。
* 拉取每周财政部长令（KMK）汇率，用于计算海关价值（NDPBM）。
* 可设置为每周三新汇率生效时的自动 cron 作业。

### 2. HS 编码与关税
* 菜单：**CEISA 连接器 > CEISA 服务 > 同步关税（CEISA Connector > Layanan CEISA > Sinkronisasi Tarif）**。
* 从印度尼西亚海关税则（BTKI）中获取特定 **8 位** HS 编码的进口关税率、增值税、奢侈品销售税和第 22 条所得税。
* **HS 编码规范**：必须使用完整的 **8 位**国家税则号（BTKI/AHTN 最末级编码，而非 2/4/6 位分类标题），因为 CEISA 4.0 关税与 Lartas 服务仅在 8 位编码层级验证税率与监管要求。

### 3. 贸易限制与禁令（Lartas）
* 菜单：**CEISA 连接器 > CEISA 服务 > 同步 Lartas（CEISA Connector > Layanan CEISA > Sinkronisasi Lartas）**。
* 根据 8 位 HS 编码检查清关前适用的法规要求（贸易部、检疫局、BPOM 许可证）。

### 4. 舱单集成
* 菜单：**CEISA 连接器 > CEISA 服务 > 同步舱单（CEISA Connector > Layanan CEISA > Sinkronisasi Manifes）**。
* 通过舱单提单和分提单编号从航空港和海港的临时存储设施（TPS）检索货物舱单（BC 1.1）记录。

### 5. 状态与响应独立查询
* 菜单：**CEISA 连接器 > CEISA 服务 > 状态与响应（CEISA Connector > Layanan CEISA > Status & Respon）**。
* 在 **状态与响应（Status & Respon）** 列表或表单顶部点击 **拉取海关状态与响应（Tarik Status & Respon BC）** 按钮，在弹窗中输入 **26 位申请号（Nomor Aju）**。
* 系统自动连接 CEISA 服务器并记录：
    * **状态历史**：海关单证处理状态序列、登记号（Nomor Daftar）、登记日期、状态时间及最新状态徽章。
    * **响应历史**：官方海关响应（SPPB、NPP、SPJM、驳回 Reject 等），包含精确到秒的响应时间戳与海关提示信息。
    * **Chatter 自动附加 PDF 与内嵌预览**：当 CEISA 响应包含 PDF 文件时，系统自动下载并将其附加到对应报关单证（TPB、进口、出口或 FTZ）或 **状态与响应** 记录的 **Chatter（消息栏）** 中，并支持表单侧边内嵌 PDF 预览。

> [!IMPORTANT]
> **状态与响应拉取条件（`Data tidak ditemukan`）：**
> 仅当报关单在海关系统中**已生成登记号（Nomor Pendaftaran / Nomor Daftar）**后，才可拉取到 **状态与响应（Status & Respon）** 数据。若单证尚未取得登记号，拉取时 CEISA 将返回 **`Data tidak ditemukan`**（未找到数据）提示。请待单证获得登记号后再重新拉取。

---

## 主数据对齐

在处理海关申报之前对齐内部 Odoo 主数据：

1. **产品 HS 编码（8 位数字）**：
    * 进入 **库存 > 产品 > 产品（Inventory > Products > Products）**（或 **HS 编码** 主数据）。
    * 在 **CEISA / DJBC 信息** 选项卡上，分配正确的 **8 位税则 / HS 编码（Pos Tarif / HS Code）**（无需录入 2、4 或 6 位上级标题）和标准 CEISA 计量单位。
2. **海关合作伙伴配置文件**：
    * 进入 **联系人（Contacts）**。
    * 在 **CEISA** 选项卡上，指定合作伙伴的 **16 位 NPWP**、**NIB（商业识别号）** 和相关实体角色。
3. **UOM 标准映射**：
    * 将 Odoo 内部计量单位映射到 CEISA 标准国际编码（例如 `KGM` 表示千克、`PCE` 表示件、`LTR` 表示升）。

---

## API 日志与审计追踪

每次出站请求有效负载和来自 CEISA 的入站响应都会被记录以供审计：

* **API 日志菜单（API Log Menu）**：位于 **CEISA 连接器 > 配置 > API 日志（CEISA Connector > Configuration > API Log）**。
* 捕获的指标包括：
    * 访问的端点 URL。
    * HTTP 状态代码（200 OK、400 Bad Request、401 Unauthorized、500 Server Error）。
    * 请求 JSON 正文和响应 JSON 正文。
    * 往返执行延迟。
* 帮助运维和 IT 团队迅速排查提交拒绝（*rejects*）或模式验证错误。

> [!TIP]
> 在进行首次正式单证提交之前，请始终通过单击设置页面上的 **测试连接（Test Connection）** 按钮来验证您的凭证。