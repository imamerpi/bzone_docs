# Product Master Data & Logistics Routes

Product master data is the core foundation for ERP operations and BZone IT Inventory customs compliance. Every item stored, manufactured, purchased, or sold must have properly configured accounting parameters, logistics routes, and customs classifications.

---

## 1. Creating a New Product

To create a new product master record:

1. Open the **Inventory**, **Sales**, or **Purchase** module.
2. Navigate to **Products → Products**.
3. Click the **New** button in the upper-left corner.

![Product Master Form View](../assets/images/master_produk_general.png)
*Figure 1.1: Product master creation form in Odoo 20.*

---

## 2. General Information Configuration

On the main form and the **General Information** tab, configure the following parameters:

| Parameter / Field | Description & Recommended Values |
| :--- | :--- |
| **Product Name** | Official commercial name or description of the product. |
| **Can be Sold** | Check if this product is sold to Customers. |
| **Can be Purchased** | Check if this product is procured from Vendors. |
| **Can be Used for DJBC** | Automatically checked when classified for customs compliance. |
| **Product Type** | Select **Goods** (Storable) for physical goods whose stock quantities are tracked. |
| **Invoicing Policy** | Select **Ordered quantities** or **Delivered quantities** based on billing policies. |
| **Track Inventory** | Choose tracking method: *By Unique Serial Number*, *By Lots*, or *No Tracking*. |
| **Sales Price** | Base unit selling price (before taxes). |
| **Customer Taxes** | Applicable Value Added Tax (VAT) rate. |
| **Cost** | Acquisition cost or standard cost of the item. |
| **Product Category** | Category defining inventory valuation and automated ledger accounts. |
| **Internal Reference** | Internal part number or SKU. |
| **Barcode** | Barcode / EAN code for warehouse handheld scanners. |

> [!NOTE]
> The **Product Category** automatically determines inventory valuation accounts as well as the default DJBC IT Inventory classification (Raw Material, Finished Goods, or Consumables).

---

## 3. Configuring Logistics Routes (Inventory Tab)

Routes determine how the system triggers procurement and stock movements when stock is requested.

1. Open the **Inventory** tab.
2. In the **Routes** section, activate the appropriate routes:

![Logistics Routes Configuration](../assets/images/master_produk_routes.png)
*Figure 1.2: Logistics route selection (Buy, MTO, Manufacture) on the Inventory tab.*

### Operational Routes Breakdown:

* **Buy**: Instructs the system to automatically generate a draft Purchase Order (RFQ) when inventory falls below the reordering rule threshold.
* **Make to Order (MTO)**: Instructs the system not to pull from regular warehouse stock, but to trigger a dedicated Purchase Order or Manufacturing Order upon Sales Order confirmation.
* **Manufacture**: Marks the product as internally produced at the factory via a Manufacturing Order (MO).
* **Manufacture WIP**: Identifies intermediate Work-in-Progress items between multiple production steps.
* **Resupply Subcontractor on Order**: Used when raw materials are delivered to an external subcontractor for toll processing.
* **Dropship**: Direct shipment from vendor to the end customer without physical transit through internal warehouses.

### Physical & Logistics Parameters:
* **Weight & Volume**: Gross/net weight and volume for shipping manifests and customs declarations.
* **Customer Lead Time**: Estimated business days required from order confirmation to readiness for dispatch.
* **Purchase Request**: Automated internal purchase requisition creation when stock reaches minimum thresholds.

---

## 4. Customs Compliance Parameters (DJBC Information Tab)

For companies operating under customs facility schemes (Bonded Zone / KB, KITE, or KEK):

![Customs Classification Parameters](../assets/images/master_produk_djbc.png)
*Figure 1.3: DJBC classification and HS Code configuration on the product form.*

1. Go to the **DJBC Information** tab.
2. **DJBC Raw Material**: Check this option if the item is classified as raw/auxiliary material subject to mandatory reporting in the IT Inventory Raw Material Mutation Report.
3. **HS Code**: Enter the 8-digit international Harmonized System code for tariff rate determination and CEISA integration.
4. **Origin of Goods**: Specify the Country of Origin for import/export customs declarations.
5. Click **Save** in the top left corner.

> [!IMPORTANT]
> Consistent and accurate setup of **HS Code** and **DJBC Classification** is essential for document submission to CEISA 4.0 and the 7 Mandatory IT Inventory Reports.
