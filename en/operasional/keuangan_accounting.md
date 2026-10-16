# Accounting & Finance Management

The Accounting & Finance module governs recognition of Accounts Payable (*Vendor Bills*), Accounts Receivable (*Customer Invoices*), bank and cash disbursements, and key financial reporting including the Profit and Loss statement.

---

## 1. Accounting Lifecycle Overview

```text
  [ Vendor Bills (Accounts Payable) ]     [ Customer Invoices (Receivable) ]
                   │                                       │
                   ▼                                       ▼
        [ Posted Status: Approved ]             [ Confirm Draft Invoice ]
                   │                                       │
                   ▼                                       ▼
        [ Click Pay / Register ]                [ Click Pay / Register ]
                   │                                       │
                   ▼                                       ▼
    (Select Bank & Payment Method)          (Select Bank & Payment Method)
                   │                                       │
                   ▼                                       ▼
       [ Payment Status: Paid ]                [ Payment Status: Paid ]
                   │                                       │
                   └───────────────────┬───────────────────┘
                                       ▼
                      [ Financial & Audit Reports ]
                      (Profit & Loss / Balance Sheet)
```

---

## 2. Processing Vendor Bill Payments

Disbursing payments against received vendor invoices:

1. Open the **Accounting** module.
2. Navigate to **Vendors → Bills**.
3. Select an approved bill currently marked **Not Paid**.
4. Click **Pay** (or **Register Payment**) at the top of the bill form.

![Vendor Bill Payment Execution](../assets/images/accounting_vendor_bill_payment.png)
*Figure 7.1: Register payment popup for vendor liabilities.*

5. In the payment dialog:
   * **Journal**: Select the bank or cash account for fund disbursement.
   * **Payment Method**: Select disbursement channel (*Bank Transfer*, *Manual*, *Cheque*).
   * **Amount**: Confirm disbursement amount.
   * **Payment Date**: Value date of the bank debit.
6. Click **Create Payment**.
7. Status updates to **In Payment** or **Paid**.

---

## 3. Confirming and Collecting Customer Invoices

Recognizing sales receivables and customer incoming payments:

1. Open the **Accounting** module.
2. Navigate to **Customers → Invoices**.
3. Select an invoice in **Draft** status.
4. Verify the **Invoice Date** and **Due Date**.
5. Click **Confirm** to post the invoice into receivables.
6. When customer remittance is received, click **Pay** (or **Register Payment**).

![Customer Invoice Confirmation and Receipt](../assets/images/accounting_customer_invoice_confirm.png)
*Figure 7.2: Customer invoice confirmation and receipt registration.*

7. Select the receiving bank **Journal**, payment method, and monetary amount.
8. Click **Create Payment**.
9. Invoice status updates to **In Payment** / **Paid**.

---

## 4. Generating Financial Reports (Profit and Loss)

The *Profit and Loss* (Income Statement) details company revenues, Cost of Goods Sold (COGS), and operating expenses over a specific period.

### Step-by-Step Report Generation:

1. Navigate to **Reporting → Profit and Loss**.
2. Define the reporting window:
   * Select **Start Date** and **End Date**.
   * Optionally configure **Comparison** to benchmark against prior periods.
3. Click **Print** to download the official PDF document, or choose **Export (XLSX)** for spreadsheet analysis.

![Profit and Loss Financial Reporting](../assets/images/accounting_profit_loss_report.png)
*Figure 7.3: Profit and Loss interface with date filters.*

4. Click **Continue** if a confirmation prompt appears.
5. The resulting report is ready for management review and fiscal audit submissions.
