# 7 Mandatory Customs Mutation Reports (djbc_kb)

> **Last updated:** 22 Oktober 2026

BZone provides **7 Official Reconciliation Reports** required for real-time compliance inspections by Customs auditors in accordance with Director General of Customs and Excise Regulation **PER-02/BC/2019**.

---

## Overview of the 7 Reports

All reports can be accessed from **Laporan Bea Cukai > Reporting**:

1. **Inbound Goods Report by Customs Document**:
   Recapitulates all receipts grouped by declaration type (BC 2.3, BC 4.0, BC 2.7 In).
2. **Outbound Goods Report by Customs Document**:
   Recapitulates all shipments leaving the Bonded Zone (BC 2.5, BC 4.1, BC 2.7 Out, BC 3.0 PEB).
3. **Raw Materials & Auxiliary Mutation Report**:
   Displays beginning balance, receipts, production usage, adjustments, and ending balance.
4. **Finished Goods Mutation Report**:
   Displays beginning balance, manufacturing receipts, deliveries, and ending balance.
5. **Work in Process (WIP) Position Report**:
   Tracks raw materials undergoing active processing on manufacturing orders on the shopfloor.
6. **Machinery & Equipment Mutation Report**:
   Accounts for production machinery and spare parts granted customs duty exemption or deferment.
7. **Scrap & Waste Goods Mutation Report**:
   Monitors production scrap, waste materials, and rejected parts before destruction or domestic disposal.

---

## Generating Reports (Wizard Reporting)

1. Open **Laporan Bea Cukai > Reporting**.
2. Select the report type (e.g. **Raw Materials Mutation Report**).
3. In the popup wizard:
   * **Start Date**: Beginning date of the audit period.
   * **End Date**: Ending date of the audit period.
   * **Output Format**: Choose between HTML screen view, Py3o official template (ODT/PDF), or Excel (XLSX).
4. Click **Print / Preview**.

![Customs Report Wizard](/assets/images/kb_report_wizard.png)
*Figure 5.1: Customs reporting wizard for date selection and export formatting.*

---

## Reconciliation Formula

> [!NOTE]
> **Reconciliation Formula:**  
> **Ending Balance** = **Beginning Balance** + **Inbound** − **Outbound** ± **Physical Adjustments**

* **Inbound**: Derived from receipts validated by customs declarations.
* **Outbound**: Derived from production usage (MO) or delivery orders.
* **Adjustments**: Derived from physical count variances during annual IT Inventory stock counts.

---

## Data Eligibility Guide: Ensuring Products & Transactions Appear in Reports

To ensure inventory items and movement transactions appear accurately on **Inbound Reports**, **Outbound Reports**, and **Mutation Reports**, the system enforces specific operational validation criteria:

![Customs Data Eligibility Checklist](/assets/images/kb_checklist_transaksi_laporan.png)
*Figure 5.2: Verification workflow and data eligibility checklist for DJBC customs reporting.*

### 1. Requirements for Inbound Goods Reports (Laporan Pemasukan)
Inbound warehouse receipts (*Stock Picking / Receipts*) automatically qualify for the **Inbound Goods Report per Customs Declaration** when 4 conditions are met:

* **Picking Status Must Be "Done"**: The receiving picking must be validated (`done`). Draft, waiting, or ready pickings are excluded.
* **Warehouse Operation Type Configuration**:
  * The Operation Type must have **DJBC Movement Type** set to `Inbound (in)`.
  * Must **not** be flagged as *Scrap* or *Adjustment*.
* **Customs Document Information Fully Populated**:
  * On the **Customs Document** tab of the Receipt, the **Customs Document Type** (`BC 2.3`, `BC 4.0`, or `BC 2.7`), **Registration Number** (6 digits), and **Registration Date** must be entered.

> [!DANGER]
> Receipts without customs document details are treated as internal non-facility transfers and will **not** appear in official customs inbound reporting.

* **Product & Purchasing Valuation**:
  * Product must possess an **Internal Reference / Code**.
  * Delivered quantity must be greater than zero.
  * Linked to a Purchase Order (PO) to automatically aggregate foreign currency valuation, currency codes, and IDR conversion referencing official weekly KMK exchange rates.

### 2. Requirements for Outbound Goods Reports (Laporan Pengeluaran)
Outbound deliveries (*Delivery Orders / Stock Picking*) qualify for the **Outbound Goods Report per Customs Declaration** when:

* **Delivery Order Status Must Be "Done"**: Dispatch must be fully validated.
* **Warehouse Operation Type Configuration**:
  * Operation Type must have **DJBC Movement Type** set to `Outbound (out)`.
  * Must **not** be flagged as *Adjustment*.
* **Customs Outbound Document Entered**:
  * On the **Customs Document** tab of the Delivery Order, the **Customs Document Type** (`BC 2.5`, `BC 4.1`, `BC 2.7`, or `BC 3.0 / PEB`), **Registration Number**, and **Registration Date** must be filled.
* **Product & Commercial Valuation**:
  * Product must possess an Internal Reference.
  * Commercial values are derived from Sales Order (SO) unit prices multiplied by delivered quantities, converted with the applicable KMK customs rate.

### 3. Requirements for 7 IT Inventory Mutation Reports
To ensure products and their stock movement quantities (beginning balance, inbound, outbound, adjustments, and ending balance) appear correctly on the respective mutation reports:

* **Product Master Data Requirements**:
  * **Active Product**: Field `Active` must be `True` on both `product.template` and `product.product`.
  * **Internal Reference (Product Code)**: Mandatory; acts as the primary key identifying goods for Customs audits.
  * **Bonded Facility Flag Active**: In the **Customs / DJBC** tab, the **Bonded Zone (djbc_kb_ok)** checkbox must be enabled (`True`).
  * **Accurate IT Inventory Classification**: Assign the correct category on the product or product category:
    * `Raw Material` (`djbc_rm`) → Appears in Raw Materials & Auxiliary Mutation Report.
    * `Finished Goods` (`djbc_fg`) → Appears in Finished Goods Mutation Report.
    * `Work in Process` (`djbc_wip`) → Appears in WIP Position Report.
    * `Machinery` (`djbc_mesin`) → Appears in Machinery & Equipment Mutation Report.
    * `Scrap` (`djbc_scrap`) → Appears in Scrap & Waste Mutation Report.
* **Stock Location Master Requirements**:
  * Quantities are calculated based on movements to/from internal locations tagged with DJBC location codes:
    * Raw Material Location (`rm`)
    * Finished Goods Location (`fg`)
    * Production / WIP Location (`wip`)
    * Machinery Location (`mesin`)
    * Scrap Location (`scrap`)
  * Configurable under **Inventory > Configuration > Locations**.
* **Balance Calculation & Wizard Period**:
  * **Beginning Balance**: All completed stock moves prior to the wizard's **Start Date**.
  * **Inbound**: Moves entering the designated DJBC location within the report date range.
  * **Outbound**: Moves leaving the designated DJBC location within the report date range.
  * **Adjustments**: Variances from validated **Stock Opname** count sessions within the reporting timeframe.

---

## Diagnostic & Troubleshooting Matrix

| Symptom | Probable Cause | Corrective Action |
| :--- | :--- | :--- |
| **Product is missing entirely from the Mutation Report table** | Bonded Zone flag is disabled or product lacks an Internal Reference. | Open the Product record, ensure **Internal Reference** is filled, and check **Bonded Zone (djbc_kb_ok)** on the Customs tab. |
| **Product appears in the wrong Mutation Report** (e.g. Raw Material shows in Finished Goods) | Incorrect IT Inventory classification assigned on product or category. | Open the Product or Category, review the Customs classification, and select the appropriate category (e.g., change from FG to RM). |
| **Receipt does not appear in Inbound Goods Report** | 1. Picking is not *Done*.<br/>2. Customs Document tab lacks registration number/date.<br/>3. Operation Type movement type is not set to `in`. | 1. Validate the picking.<br/>2. Open the Customs Document tab and enter declaration type (BC 2.3/4.0/2.7), registration number, and date.<br/>3. Verify Operation Type settings under Inventory. |
| **Delivery does not appear in Outbound Goods Report** | 1. Delivery order is not *Done*.<br/>2. Outbound customs declaration details are missing. | Validate the Delivery Order and enter declaration type (BC 2.5/4.1/2.7/3.0) and official customs registration numbers. |
| **Inbound/Outbound quantities show 0 on Mutation Report despite activity** | 1. Transaction dates fall outside wizard date range.<br/>2. Warehouse location is missing DJBC location classification. | 1. Verify and adjust the wizard Start Date and End Date.<br/>2. Go to **Inventory > Configuration > Locations**, open the relevant location, and assign the proper DJBC classification code. |
