# Procurement Operational Cycle (Purchase)

> **Last updated:** 22 Oktober 2026

The procurement workflow covers request for quotation (RFQ) creation, vendor price confirmation, purchase order (PO) approval, warehouse goods receipt, and vendor bill verification.

---

## 1. Procurement Workflow Overview

```text
[ Create RFQ Requisition ]
           │
           ▼
[ Vendor Selection & Negotiation ]
           │
           ▼
[ Confirm Purchase Order (PO) ]
           │
   ┌───────┴───────┐
   ▼               ▼
[ Goods Receipt ]  [ Create Vendor Bill ]
(Warehouse Stock)  (Accounts Payable)
   │               │
   └───────┬───────┘
           ▼
[ Reconciliation & Payment ]
```

---

## 2. Creating a Request for Quotation (RFQ)

1. Open the **Purchase** module.
2. Click **New** to open a new requisition.
3. Select the supplier in the **Vendor** field.
4. Set the **Order Deadline** and **Expected Arrival** date.
5. In the **Products** tab, click **Add a product** to specify items, quantities, and agreed unit prices.

![New RFQ Creation Form](../assets/images/purchase_rfq_new.png)
*Figure 4.1: Request for Quotation (RFQ) form view.*

---

## 3. Confirming as Purchase Order (PO)

1. After pricing and shipment terms are finalized, click **Confirm Order**.
2. Document status updates from **RFQ** to **Purchase Order**.
3. The system automatically creates a linked warehouse receipt record, accessible via the **Receipt** smart button.

![Purchase Order Confirmation View](../assets/images/purchase_order_confirm.png)
*Figure 4.2: Confirmed Purchase Order with Receipt smart button.*

---

## 4. Receiving Goods in Warehouse (Goods Receipt)

1. Warehouse operators process inbound goods directly via the **Receipt** smart button or through **Inventory → Operations → Receipts**.
2. Verify incoming physical quantities against ordered quantities.
3. Once fully received and validated, the **Receive Products** button disappears and the *Received* quantity updates accordingly.

---

## 5. Issuing and Confirming Vendor Bills (Create Bill)

1. On the Purchase Order, click **Create Bill**.
2. The system generates a draft **Vendor Bill**.
3. Specify the supplier invoice date under **Bill Date**.
4. Enter the supplier invoice reference under **Vendor Reference**.
5. Click **Confirm**.

![Vendor Bill Creation and Posting](../assets/images/purchase_vendor_bill.png)
*Figure 4.3: Vendor bill verification and confirmation form.*

6. The bill transitions to **Posted** status (payment status: *Not Paid*) and is queued for payment disbursement in the Accounting module.

> [!NOTE]
> Accounts Payable liabilities and inventory clearing ledger entries are automatically recognized upon Vendor Bill confirmation.
