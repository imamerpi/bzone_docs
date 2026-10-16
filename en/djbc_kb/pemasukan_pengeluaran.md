# Inbound & Outbound Workflows (djbc_kb)

This document details the operational workflow for physical inventory movements integrated with Indonesian customs documentation.

---

## Inbound Operations (Incoming Cargo)

Cargo entering a Bonded Zone may originate from overseas imports, other bonded facilities, or the domestic market (TLDDP).

### Recording Customs Documents on Inbound Stock Picking

1. Navigate to **Inventory > Operations > Receipts** or through **Purchase Orders**.
2. Open the receiving document (*Stock Picking*).
3. On the **Customs Document** tab:
   * **Customs Document Type**: Select the appropriate declaration (`BC 2.3`, `BC 4.0`, or `BC 2.7`).
   * **Registration Number**: Enter the official registration number (6 digits) issued by Customs.
   * **Registration Date**: Enter the official registration date.
   * **Submission Number (CAR)**: Enter the 26-digit customs submission number.
   * **Supplier / Partner**: Ensure the entity matches the customs declaration.
4. Click **Validate** to complete the receipt.

![Inbound Customs Document Picking](/assets/images/kb_incoming_picking.png)
*Figure 4.1: Entering customs document details during inbound receiving.*

> [!IMPORTANT]
> **DJBC Inbound Report Eligibility Criteria:**  
> Inbound receipts will only be compiled into the **Inbound Goods Report per Customs Declaration** if:
> 1. Picking status is validated to **Done**.
> 2. The **Customs Document** tab is populated with Declaration Type (BC 2.3/4.0/2.7), Registration Number (6 digits), and Registration Date.
> 3. Warehouse Operation Type has **DJBC Movement Type** set to `in`.  
> Pickings without customs document details are treated as internal non-facility stock moves.

---

## Outbound Operations (Outgoing Cargo)

Outbound dispatches from a Bonded Zone may include domestic sales (TLDDP), exports, or subcontract movements.

### Recording Customs Documents on Delivery Orders

1. Navigate to **Inventory > Operations > Delivery Orders** or through **Sales Orders**.
2. Open the delivery order.
3. On the **Customs Document** tab:
   * Specify the outbound customs declaration type:
     * **BC 2.5**: For deliveries into the domestic market (TLDDP) subject to import duty and VAT.
     * **BC 4.1**: For outbound returns of domestic origin cargo (BC 4.0).
     * **BC 2.7**: For transfers to another Bonded Zone or subcontracting.
     * **BC 3.0 / PEB**: For export shipments overseas.
   * Complete registration numbers and dates.
4. Click **Validate** to confirm stock dispatch.

![Outbound Customs Document Picking](/assets/images/kb_outgoing_picking.png)
*Figure 4.2: Entering outbound declarations on delivery orders.*

> [!IMPORTANT]
> **DJBC Outbound Report Eligibility Criteria:**  
> Deliveries will only populate the **Outbound Goods Report per Customs Declaration** when the Delivery Order is **Done**, Operation Type is set to `out`, and outbound customs declaration credentials (BC 2.5/4.1/2.7/3.0) are fully recorded.

---

## Returns Handling

* **Sales Returns**: Returned goods re-entering the Bonded Zone are tracked and credited to incoming inventory movements based on original declaration references.
* **Vendor Returns**: Materials returned to overseas or domestic suppliers require corresponding outbound declarations approved by Customs.
