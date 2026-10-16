# Goods Receipt & Delivery (Inventory)

The Inventory module controls physical movements of all inbound and outbound items. In the BZone ecosystem, every warehouse movement can be directly linked to official customs declaration registration numbers (BC 2.3, BC 4.0, BC 2.5, BC 4.1, or PEB) to ensure real-time IT Inventory audit compliance.

---

## 1. Inventory Movement Overview

```text
    [ Procurement Inbound (PO) ]                 [ Sales Outbound (SO) ]
                 │                                          │
                 ▼                                          ▼
   [ Inbound Receipts (Receipts) ]            [ Outbound Delivery Orders ]
                 │                                          │
                 ├──────────────────────────────────────────┤
                 ▼                                          ▼
   [ Verify Destination Location ]             [ Verify Delivery Address ]
                 │                                          │
                 ├──────────────────────────────────────────┤
                 ▼                                          ▼
   [ Select Customs Document Link ]           [ Select Customs Document Link ]
                 │                                          │
                 ▼                                          ▼
          [ Click Validate ]                         [ Click Validate ]
                 │                                          │
                 ▼                                          ▼
    (Done / Stock Added to WH)                (Done / Stock Shipped to Client)
```

---

## 2. Inbound Goods Receipts (Incoming Receipts)

Inbound receipts document the physical arrival of raw materials, consumables, or finished products from vendors.

### Step-by-Step Receipt Validation:

1. Open the **Inventory** module.
2. In the **Receipts** operational card, click the waiting task counter (e.g., **X to Receive**).
3. Open the target receipt document (searchable by Purchase Order source number, e.g., `P00003`).

![Inbound Goods Receipt Form](../assets/images/inventory_receipt_incoming.png)
*Figure 5.1: Inbound goods receipt form with warehouse location and customs document reference.*

4. Verify core fields:
   * **Receive From**: Supplier name.
   * **Destination Location**: Receiving warehouse zone (e.g., `WH/Stock` or `WH/Raw Materials`).
   * **Customs Document**: Select the associated customs document (e.g., BC 2.3 for bonded import or BC 4.0 for domestic inbound).
5. Verify physical quantities in the **Quantity Done** column.
6. Click **Validate**.
7. Status updates to **Done**. Stock balances increase immediately and inbound mutation records log into IT Inventory.

---

## 3. Outbound Delivery Orders (Outgoing Deliveries)

Delivery orders record the dispatch of manufactured goods or commercial merchandise to customers.

### Step-by-Step Delivery Validation:

1. Open the **Inventory** module.
2. In the **Delivery Orders** card, click the dispatch queue (e.g., **X to Deliver**).
3. Select an order in **Ready** status (confirming warehouse stock has been reserved).

![Outbound Delivery Order Form](../assets/images/inventory_delivery_outgoing.png)
*Figure 5.2: Outbound delivery order form for customer dispatch.*

4. Verify dispatch details:
   * **Delivery Address**: Destination customer address.
   * **Source Location**: Picking warehouse location (e.g., `WH/Stock` or `WH/Finished Goods`).
   * **Customs Document**: Select the official outbound customs declaration (e.g., BC 2.5 for local sales or BC 3.0 PEB for export).
5. Verify loaded items in the **Quantity Done** column.
6. Click **Validate**.
7. Status updates to **Done**. Warehouse inventory decreases, and the outbound entry is logged in IT Inventory.

> [!IMPORTANT]
> Selecting the appropriate **Customs Document** prior to clicking **Validate** maintains an unbreakable digital audit trail for customs audits.
