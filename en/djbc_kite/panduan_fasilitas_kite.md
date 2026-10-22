# Import Facility for Export Purposes (djbc_kite)

> **Last updated:** 22 Oktober 2026

The **`djbc_kite`** module manages customs operations and statutory inventory accountability reporting for manufacturing enterprises operating under the **Kemudahan Impor Tujuan Ekspor (KITE)** facility, in full compliance with Directorate General of Customs and Excise (DJBC) regulations.

---

## Regulatory Framework & KITE Concept

The KITE facility (governed under **PMK No. 145/PMK.04/2022** and **PER-14/BC/2022**) grants fiscal relief to export-oriented manufacturing companies that import raw materials for processing, assembly, or installation into goods destined for overseas markets:

1. **KITE Exemption (*Pembebasan*)**: Exemption from Import Duty, Value Added Tax (VAT), and Luxury Goods Sales Tax on raw material imports utilized for export manufacturing.
2. **KITE Drawback (*Pengembalian*)**: Reimbursement of paid import duty on imported materials incorporated into finished goods that have successfully been exported.
3. **IT Inventory Compliance for KITE**:
   * Internal inventory systems must segregate facility and non-facility materials accountably.
   * Material usage conversion into finished goods must be tracked precisely via Bill of Materials (BoM).
   * Official mutation reports must remain accessible to customs inspection officers via real-time electronic channels.

---

## Master Data Configuration

Configure the following foundational records before initiating transactions:

### 1. KITE Product Flagging (`product.template`)
* Open **Inventory > Products > Products**.
* Under the customs tab, enable **Can be Used for DJBC KITE** (`djbc_kite_ok`).
* This setting can also be governed globally via **Product Categories** (`product.category`).

### 2. KITE Operation Types (`stock.picking.type`)
* Open **Inventory > Configuration > Operations Types**.
* Open the relevant transfer type and configure:
  * **DJBC's KITE Movement Type**: Specify direction as **In** (Inbound) or **Out** (Outbound).
  * **DJBC's KITE Scrap?**: Enable if this operation records production scrap, reject, or waste movements.
  * **DJBC's KITE Adjustment?**: Enable if this operation records physical variance adjustments.
  * **DJBC KITE Doc Required**: Enable if the transfer must validate official customs registration details.

### 3. Warehouse Location Categorization (`stock.location`)
* Designate internal storage locations:
  * Raw Material Storage Location
  * Work in Process (WIP) Location
  * Finished Goods Storage Location
  * Scrap / Reject Waste Location

---

## Manufacturing & Operational Workflow

Manufacturing operations under the KITE scheme integrate directly with customs filings:

1. **Raw Material Inbound**:
   * Raw materials are registered under customs declaration forms **BC 2.0 / BC 2.3** or **BC 2.4** for imports, and **BC 4.0** for domestic supplies.
   * Validated receipt transfers update inventory balances and record into the **KITE Inbound Report**.
2. **Production Consumption (Manufacturing)**:
   * Shopfloor material issuance references Bills of Materials (BoM) in Odoo's Manufacturing module.
   * Actual material consumption per Manufacturing Order (MO) is tracked in the **Raw Material & Auxiliary Usage Report**.
3. **Production Output Receipts**:
   * Completion of production converts WIP into Finished Goods and Scrap.
   * Finished goods transfer to warehouse storage and record in the **Production Output Inbound Report**.
4. **Export Realization**:
   * Overseas shipments are declared under customs document **BC 3.0 (Export Declaration / PEB)**.
   * Successful exports serve as the statutory foundation to discharge KITE import duty guarantees.

---

## 8 Mandatory Statutory KITE Reports

BZone provides **8 Statutory IT Inventory KITE Reports** under **Laporan Bea Cukai > Reporting > Kemudahan Impor Tujuan Expor**:

| No | Statutory Report | Description & Reference Documents |
|---|---|---|
| **1** | **Inbound Goods Report** | Itemized summary of all received materials (BC 2.0, BC 2.3, BC 2.4, BC 4.0). |
| **2** | **Outbound Goods Report** | Itemized summary of export shipments (BC 3.0), subcontracts, and domestic sales. |
| **3** | **Raw Material Usage Report** | Material consumption breakdown per manufacturing order (MO/SPK). |
| **4** | **Production Output Report** | Summary of finished goods received from manufacturing lines into warehouse stock. |
| **5** | **Raw Material Mutation Report** | Beginning balance, receipts, production usage, adjustments, and ending stock reconciliation. |
| **6** | **Finished Goods Mutation Report** | Beginning balance, production additions, export dispatches, and ending balance reconciliation. |
| **7** | **Scrap / Reject Mutation Report** | Accountability for scrap generation, local disposals, destructions, and scrap ending balance. |
| **8** | **Work in Process (WIP) Mutation Report** | Reconciliation of raw materials currently committed inside active production orders. |

---

## Generating Reports via Wizard

1. Open **Laporan Bea Cukai > Reporting > Kemudahan Impor Tujuan Expor**.
2. Select the desired report (e.g. **Raw Material Usage Report**).
3. In the popup wizard:
   * Select a predefined **Report Period** or enter **Date Start** and **Date End** manually.
   * Choose the export format: **Screen View** (*HTML*), official **Py3o ODT/PDF**, or **Excel Spreadsheet (XLSX)**.
4. Click **Print / Preview**.

> [!TIP]
> All KITE mutation reports follow the standardized audit reconciliation formula:
> **Ending Balance** = **Beginning Balance** + **Inbound** − **Outbound / Usage** ± **Adjustments**
