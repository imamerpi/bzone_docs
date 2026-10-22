# Manufacturing & Bill of Materials (MRP)

> **Last updated:** 22 Oktober 2026

The Manufacturing module (MRP) oversees recipe formulas (*Bills of Materials / BoM*), work orders and production orders (*Manufacturing Orders / MO*), shop floor material transfers, and product conversions from raw materials into Work-in-Progress (WIP) or finished goods.

---

## 1. Production Workflow Overview

```text
[ Configure Bill of Materials (BoM) ]
                  │
                  ▼
[ Create Manufacturing Order (MO) ]
                  │
                  ▼
[ Confirm MO (Confirm) ]
                  │
                  ▼
[ Transfer Raw Materials to Production Floor ]
                  │
                  ▼
[ Execute & Validate (Produce All) ]
                  │
                  ▼
[ Raw Materials Consumed & Finished Goods Added ]
```

---

## 2. Defining Bills of Materials (BoM)

A *Bill of Materials* (BoM) defines the precise list of raw materials and quantities required to produce a specific finished item.

### Step-by-Step BoM Setup:

1. Open the **Manufacturing** module.
2. Navigate to **Products → Bills of Materials**.
3. Click **New**.

![Bill of Materials Form View](../assets/images/mrp_bom_form.png)
*Figure 6.1: Bill of Materials (BoM) configuration form.*

4. Complete BoM details:
   * **Product**: Select the target finished product or subassembly.
   * **Quantity**: Reference batch size (e.g., `1.00 Unit` or `100.00 Units`).
   * **Reference**: Engineering formula code or version identifier.
   * **BoM Type**: Select **Manufacture this product**.
   * **Components Tab**: Click **Add a line** to list component items and required quantities per unit.
5. Click **Save**.

---

## 3. Creating & Confirming Manufacturing Orders (MO)

A *Manufacturing Order* (MO) represents an authorized work order to produce products on the production floor.

### Step-by-Step MO Creation:

1. Navigate to **Operations → Manufacturing Orders**.
2. Click **New**.

![Manufacturing Order Creation View](../assets/images/mrp_mo_creation.png)
*Figure 6.2: Manufacturing Order creation form.*

3. Set production parameters:
   * **Product**: Select the product to produce.
   * **Quantity**: Enter quantity to produce.
   * **Bill of Material**: Choose the relevant BoM. Required components automatically populate.
4. Click **Confirm**. Document status transitions to **Confirmed**.

---

## 4. Transferring Raw Materials (Material Transfer)

To preserve physical and digital inventory accuracy:

1. Check component stock availability.
2. If raw materials must move from central storage to the production floor:
   * Access the transfer order or click **Transfer**.
   * Verify **Source Location** (e.g., `WH/Raw Materials`) and **Destination Location** (e.g., `WH/Production Floor`).
   * Click **Validate** to execute the physical transfer.

![Material Transfer to Production Floor](../assets/images/mrp_material_transfer.png)
*Figure 6.3: Validating component transfer to the production floor.*

---

## 5. Completing Production & Stock Verification (Produce All)

1. Return to the Manufacturing Order.
2. Enter the actual finished quantity.
3. Click **Produce All** on the top header.

![Completing Production and Verifying Stock](../assets/images/mrp_produce_all_done.png)
*Figure 6.4: Executing Produce All and verifying On-Hand finished goods inventory.*

4. The Manufacturing Order transitions to **Done**:
   * Raw materials are consumed from warehouse records.
   * Finished goods are added to on-hand inventory.
5. Verify on-hand finished goods:
   * Navigate to **Products → Products**.
   * Open the newly produced product card.
   * The **On Hand** metric reflects updated stock quantities.

> [!NOTE]
> Component consumption and finished product output from Manufacturing Orders directly feed into the official IT Inventory **Raw Material Mutation Report** and **Finished Goods Mutation Report**.
