# Customs & Tax Exchange Rate Management (erpi_custom_rate)

The **`erpi_custom_rate`** module provides manual currency rate specification (*Custom Currency Rate*) across Purchase Orders, Sales Orders, and Invoicing documents (*Customer Invoices & Vendor Bills*).

---

## Importance of Customs & Fiscal Rates (KMK)

In Indonesian customs compliance, import procurement declarations (e.g. BC 2.0, BC 2.3, BC 2.7) and export sales declarations (BC 3.0) frequently involve foreign currencies such as **USD, EUR, JPY, or SGD**.

1. **Operational vs. Fiscal Exchange Rates**:
   * Odoo by default retrieves standard bank or market spot rates.
   * The Directorate General of Customs and Excise (DJBC) and the Directorate General of Taxes (DJP) require customs valuations (*Customs Value*), import duty, VAT, and Income Tax (PPh 22) calculations to reference the official **Minister of Finance Decree Exchange Rates (*Kurs KMK*)** published weekly.
2. **Operational Value of Custom Rates**:
   * Ensures accounts payable/receivable balances and inventory valuation at inbound clearance align with official customs declaration figures.
   * Eliminates currency discrepancy issues between customs documents and financial accounting books.

---

## Key Features

* **Use Custom Rate**: Flexible checkbox toggle on multi-currency transactions.
* **Custom Rate Precision**: Manual exchange rate input supporting up to 6 decimal places (e.g. `16250.000000`).
* **Multi-Document Flow**: Continuous rate inheritance across operational documents:
  * Inbound Procurement (*Purchase Orders*)
  * Vendor Invoicing (*Vendor Bills / account.move*)
  * Outbound Sales (*Sale Orders*)
  * Customer Invoicing (*Customer Invoices / account.move*)

---

## Application Workflow

### 1. Procurement (Purchase Order)

When creating foreign currency purchase orders for overseas suppliers or other bonded facilities:

1. Navigate to **Purchase > Orders > Purchase Orders** and create a record.
2. Select the vendor and specify the foreign currency in **Currency** (e.g. `USD`).
3. In the order header:
   * Check **Use Custom Rate**.
   * Enter the prevailing official customs rate (*Kurs KMK*) in the **Custom Rate** field (e.g. `16,350.00`).
4. When confirming the order and generating a bill (*Create Bill*), the custom rate automatically transfers to the vendor bill.

```
+-------------------------------------------------------------+
| Currency: [ USD ]        [x] Use Custom Rate                |
| Custom Rate: [ 16350.000000 ]                               |
+-------------------------------------------------------------+
```

---

### 2. Sales (Sale Order)

For export-bound sales or domestic bonded transfers:

1. Navigate to **Sales > Orders > Orders** and create a new order.
2. Choose the transaction currency (*Foreign Currency*).
3. Check **Use Custom Rate** and enter the contract rate or applicable customs rate for the transaction date.
4. The system calculates the company currency equivalent (*IDR*) accurately and proportionally.

---

### 3. Accounting & Invoicing (Account Move)

In the Accounting/Invoicing modules:

1. When an invoice or bill involves a foreign currency:
   * The system detects multi-currency status (`not_company_currency = True`).
   * Users can activate **Use Custom Rate** at any time prior to posting the document.
2. Accounting impact:
   * The `currency_rate` and `invoice_currency_rate` fields on journal items are overridden with the specified *Custom Rate*.
   * Debit and credit amounts in company currency (IDR) convert precisely based on the manual rate.
   * Realized exchange difference calculations during payment reconciliation compute reliably from this custom rate baseline.

> [!TIP]
> If **Use Custom Rate** is unchecked, the document automatically falls back to Odoo's standard daily exchange rate tables (`res.currency.rate`).
