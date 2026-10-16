# Business Partners (Vendor & Customer)

Business Partner master data (*Partners*) encompasses supplier (*Vendor*) and client (*Customer*) records. In the BZone ecosystem, partner data serves not only commercial transactions (PO, SO, and Invoices), but also official trade entity declarations (Importer, Exporter, Consignor, Consignee, and Customs Broker / PPJK) for CEISA customs documents.

---

## 1. Creating Vendor Master Data

Vendors represent suppliers of raw materials, consumables, machinery, and services.

### Step-by-Step Guide:

1. Open the **Purchase** module.
2. Navigate to **Orders → Vendors**.
3. Click **New**.

![Vendor Master Form View](../assets/images/master_vendor_form.png)
*Figure 2.1: Vendor master creation form.*

### Field Information:

1. **Entity Type**: Select **Company** (Corporate entity) or **Individual**.
2. **Name**: Enter official registered legal name corresponding to tax registration (NPWP).
3. **Address**: Complete street address, city, state, postal code, and country.
4. **Contact Details**: Phone, mobile, and official invoicing email.
5. **Invoicing Tab**:
   * Enter Tax Identification Number (NPWP 16 digits / NITKU).
   * Register vendor bank account details for wire payment execution.
6. **Sales & Purchase Tab**:
   * Configure default **Payment Terms** (e.g., *Immediate*, *30 Days*).
7. Click **Save**.

---

## 2. Creating Customer Master Data

Customers represent buyers of manufactured goods or commercial merchandise.

### Step-by-Step Guide:

1. Open the **Sales** module.
2. Navigate to **Orders → Customers**.
3. Click **New**.

![Customer Master Form View](../assets/images/master_customer_form.png)
*Figure 2.2: Customer master creation form.*

### Field Information:

1. **Entity Type**: Select **Company** or **Individual**.
2. **Name**: Enter full registered customer name.
3. **Address & Delivery**: Define invoice address and delivery destination address.
4. **Contact Details**: Contact person, phone, and billing email.
5. **Invoicing / Accounting Tab**:
   * Customer Tax ID (NPWP) for electronic tax invoice issuance.
   * Configure **Credit Limit** if credit control policies are enforced.
6. **Sales & Purchase Tab**:
   * Assign default **Pricelist** and **Payment Terms**.
7. Click **Save**.

---

## 3. Customs Integration with Business Partners

> [!NOTE]
> When **BZone CEISA** modules are active, partner names, 16-digit NPWP/NITKU, addresses, and country codes automatically map to official declaration entities:
> * **Foreign Vendors**: Mapped as *Seller* / *Exporter* in import declarations BC 2.0 / BC 2.3.
> * **Domestic Vendors**: Mapped as *Consignor* in domestic inbound declarations BC 4.0.
> * **Overseas Customers**: Mapped as *Consignee* / *Buyer* in export declarations BC 3.0.
> * **Domestic Customers**: Mapped as *Consignee* in domestic outbound declarations BC 2.5 / BC 4.1.
