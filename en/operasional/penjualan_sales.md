# Sales Operational Cycle (Sales)

The sales workflow covers quotation preparation, sales order confirmation, automated Make to Order (MTO) execution, outbound delivery, and customer invoice generation with payment registration.

---

## 1. Sales Workflow Overview

```text
[ Create Quotation ]
         │
         ▼
[ Set MTO Route / Standard Stock ]
         │
         ▼
[ Confirm as Sales Order (SO) ]
         │
         ├──────────────────────────────┐
         ▼                              ▼
  [ Trigger Production / PO ]    [ Create Customer Invoice ]
         │                       (Down Payment / Regular)
         ▼                              │
  [ Outbound Delivery (DO) ]            ▼
                                 [ Register Payment ]
```

---

## 2. Creating a Quotation & Configuring MTO Route

1. Open the **Sales** module.
2. Click **New** to create a fresh quotation.
3. Select the buyer under **Customer**.
4. In the **Order Lines** tab, click **Add a product** and specify the product, quantity, and unit price.

![Quotation Creation and MTO Route Setup](../assets/images/sales_quotation_mto.png)
*Figure 3.1: Quotation creation with Make to Order (MTO) route selected.*

5. For custom or made-to-order products:
   * Reveal the **Route** column if hidden by clicking the column options toggle on the right side of the line headers, then check **Route**.
   * Under **Route**, select **Make to Order (MTO)**.

> [!TIP]
> Selecting **Make to Order (MTO)** causes the system to automatically generate a linked *Manufacturing Order* (if the product route is *Manufacture*) or a draft *Purchase Order* (if the route is *Buy*) upon Sales Order confirmation.

---

## 3. Confirming the Sales Order

1. Review quantities, VAT taxes, and delivery addresses.
2. Click **Confirm** in the upper left corner.
3. The document status transitions from **Quotation** to **Sales Order**.
4. A linked **Delivery** record is automatically created and accessible via the smart button on top of the form.

---

## 4. Issuing Customer Invoices (Create Invoice)

1. On the confirmed Sales Order, click **Create Invoice**.
2. Select the billing method:

| Invoice Option | Intended Usage |
| :--- | :--- |
| **Regular invoice** | Full billing based on ordered or delivered quantities. |
| **Down payment (percentage)** | Advance billing by percentage (e.g., 30% of total order). |
| **Down payment (fixed amount)** | Advance billing by a fixed agreed monetary amount. |

![Confirming Sales Order and Creating Invoice](../assets/images/sales_order_confirmed.png)
*Figure 3.2: Customer invoice creation dialog (Down Payment / Regular).*

3. Click **Create Draft** to generate the draft Customer Invoice.
4. Open the draft invoice, verify the **Invoice Date**, and click **Confirm**.

---

## 5. Registering Customer Payment (Register Payment)

1. On the posted customer invoice, click **Register Payment**.

![Registering Customer Payment](../assets/images/sales_register_payment.png)
*Figure 3.3: Register payment popup form.*

2. Complete the payment parameters:
   * **Journal**: Bank or cash receipt account.
   * **Payment Method**: Bank transfer or manual receipt.
   * **Amount**: Confirm received monetary amount.
   * **Payment Date**: Bank transaction posting date.
3. Click **Create Payment**.
4. Invoice status updates to **In Payment** or **Paid**.
