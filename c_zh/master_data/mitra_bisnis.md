# 业务合作伙伴（供应商与客户）

业务合作伙伴主数据（*Partners*）涵盖供应商（*Vendor*）和客户（*Customer*）记录。在 BZone 生态系统中，合作伙伴数据不仅用于商业交易（PO、SO 和发票），还用于 CEISA 海关单证中的官方贸易实体申报（进口商、出口商、发货人、收货人以及报关行 / PPJK）。

---

## 1. 创建供应商主数据

供应商代表原材料、消耗品、机械设备和服务的提供者。

### 操作步骤：

1. 打开 **采购（Purchase）** 模块。
2. 进入 **订单 → 供应商（Orders → Vendors）**。
3. 点击 **新建（New）**。

![供应商主表单视图](../assets/images/master_vendor_form.png)
*图 2.1：供应商主数据创建表单。*

### 字段信息：

1. **实体类型（Entity Type）**：选择 **公司（Company）**（法人实体）或 **个人（Individual）**。
2. **名称（Name）**：输入与税务登记（NPWP）对应的法定注册名称。
3. **地址（Address）**：完整填写街道地址、城市、省份、邮政编码和国家。
4. **联系方式（Contact Details）**：电话、手机以及官方开票邮箱。
5. **开票选项卡（Invoicing Tab）**：
   * 输入纳税人识别号（NPWP 16 位 / NITKU）。
   * 注册供应商银行账户信息以执行电汇付款。
6. **销售与采购选项卡（Sales & Purchase Tab）**：
   * 配置默认 **付款条件（Payment Terms）**（例如 *即时*、*30 天*）。
7. 点击 **保存（Save）**。

---

## 2. 创建客户主数据

客户代表成品或商品的买家。

### 操作步骤：

1. 打开 **销售（Sales）** 模块。
2. 进入 **订单 → 客户（Orders → Customers）**。
3. 点击 **新建（New）**。

![客户主表单视图](../assets/images/master_customer_form.png)
*图 2.2：客户主数据创建表单。*

### 字段信息：

1. **实体类型（Entity Type）**：选择 **公司（Company）** 或 **个人（Individual）**。
2. **名称（Name）**：填写客户完整注册名称。
3. **地址与配送（Address & Delivery）**：定义开票地址和收货地址。
5. **联系方式（Contact Details）**：联系人、电话与开票邮箱。
6. **开票 / 会计选项卡（Invoicing / Accounting Tab）**：
   * 客户税号（NPWP）用于开具电子发票。
   * 如实施信用控制策略，请配置 **信用额度（Credit Limit）**。
7. **销售与采购选项卡（Sales & Purchase Tab）**：
   * 分配默认 **价格表（Pricelist）** 和 **付款条件（Payment Terms）**。
8. 点击 **保存（Save）**。

---

## 3. 业务合作伙伴的海关集成

> [!NOTE]
> 当 **BZone CEISA** 模块启用时，合作伙伴的名称、16 位 NPWP/NITKU、地址和国家代码将自动映射为官方报关实体：
> * **境外供应商**：映射为进口报关 BC 2.0 / BC 2.3 中的 *卖方（Seller）* / *出口商（Exporter）*。
> * **境内供应商**：映射为国内入库报关 BC 4.0 中的 *发货人（Consignor）*。
> * **境外客户**：映射为出口报关 BC 3.0 中的 *收货人（Consignee）* / *买方（Buyer）*。
> * **境内客户**：映射为国内出库报关 BC 2.5 / BC 4.1 中的 *收货人（Consignee）*。