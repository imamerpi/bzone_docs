# IT Inventory Classification (djbc_kb)

> **Last updated:** 22 Oktober 2026

In bonded zone facilities, all goods inside factory premises must be classified into standard IT Inventory categories compliant with Director General of Customs and Excise Regulation **PER-02/BC/2019**.

---

## 6 Mandatory IT Inventory Categories

BZone classifies all products into 6 distinct mutation reporting categories:

| Category | DJBC Code | Operational Description | Relevant Report |
| :--- | :--- | :--- | :--- |
| **Raw Materials** | `RM` | Primary materials processed into finished goods | Raw Materials & Auxiliary Mutation Report |
| **Auxiliary Materials** | `AUX` | Supporting materials used during production | Raw Materials & Auxiliary Mutation Report |
| **Finished Goods** | `FG` | Final manufactured output ready for export/delivery | Finished Goods Mutation Report |
| **Work in Process** | `WIP` | Semi-finished goods currently on the shopfloor | Work in Process (WIP) Position Report |
| **Machinery & Equipment** | `MAC` | Factory production assets & spare parts | Machinery & Equipment Mutation Report |
| **Scrap & Waste** | `SCR` | Manufacturing scrap, industrial waste, or rejected goods | Scrap & Reject Goods Mutation Report |

---

## Setting Categories on Product Masters

IT Inventory classification can be defined at the **Product Category** level or directly overridden on specific **Product Templates**.

### 1. Configuration on Product Category
1. Navigate to **Inventory > Configuration > Product Categories**.
2. Open a category (e.g. *Raw Materials*).
3. Under **DJBC Information**, set the **IT Inventory Category**:
   * Choose between: `Raw Material`, `Auxiliary`, `Finished Goods`, `Work in Process`, `Machinery`, or `Scrap`.

![Product Category Classification](/assets/images/kb_product_category.png)
*Figure 3.1: Defining IT Inventory classification on product categories.*

### 2. Override on Product Template
If a specific item requires a different classification from its parent category:
1. Navigate to **Master Data > Products**.
2. Go to the **Customs / DJBC** tab.
3. Select the appropriate classification override.

> [!NOTE]
> If left blank on the product template, the system automatically inherits the classification from its parent product category.
