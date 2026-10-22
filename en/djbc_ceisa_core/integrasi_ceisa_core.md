# CEISA 4.0 Foundation Integration (djbc_ceisa_core)

> **Last updated:** 22 Oktober 2026

The **`djbc_ceisa_core`** module serves as the primary integration gateway connecting all BZone customs operations in Odoo with the Indonesian Directorate General of Customs and Excise (DJBC) **CEISA 4.0 (Customs-Excise Information System and Automation)** ecosystem via RESTful OpenAPI.

---

## Architecture & Core Role

This module acts as the central hub for authentication, national customs reference data synchronization, and end-to-end API audit logging across all specialized customs service modules:

```mermaid
graph TD
    subgraph Odoo ERP & BZone
        CORE["djbc_ceisa_core (Gateway & Auth)"]
        TPB["djbc_ceisa_tpb (Bonded Facilities)"]
        IMP["djbc_ceisa_impor (Import PIB)"]
        EXP["djbc_ceisa_expor (Export PEB)"]
        FTZ["djbc_ceisa_ftz (Free Trade Zone)"]
        TPB --> CORE
        IMP --> CORE
        EXP --> CORE
        FTZ --> CORE
    end
    subgraph Customs Portal
        CEISA["CEISA 4.0 OpenAPI Portal"]
        AUTH["SSO & Token Service"]
        REF["Reference Services (Rates, Tariffs, Lartas)"]
        DOC["Customs Document Submission Endpoints"]
        CORE --> AUTH
        CORE --> REF
        CORE --> DOC
    end
```

### Dependent Service Modules:
* **`djbc_ceisa_tpb`**: Bonded Storage Facilities declarations (Bonded Zones, Bonded Warehouses, etc.).
* **`djbc_ceisa_impor`**: Import Goods Declarations (PIB / BC 2.0).
* **`djbc_ceisa_expor`**: Export Goods Declarations (PEB / BC 3.0).
* **`djbc_ceisa_ftz`**: Free Trade Zone Customs Declarations (PPFTZ).
* **`djbc_ceisa_barang_kiriman`**: Consignment Note Declarations.

---

## Credentials Configuration & Authentication

To enable communication with the Customs OpenAPI, administrators must set connection parameters under **CEISA Connector > Configuration > Settings**:

### 1. CEISA OpenAPI Parameters
* **Username**: User account identifier for the customs portal.
* **Password**: Password for the customs portal.
* **Bea Cukai API Key**: Unique API key generated under the user profile in the CEISA OpenAPI portal (`https://openapi.beacukai.go.id`).
* **CEISA API URL**: Official base API endpoint (e.g. `https://openapi.beacukai.go.id` for production).
* **Company Code (Kode Perusahaan)**: 6-digit company code issued by Indonesian Customs (used to construct the 26-digit Customs Submission Number / *Nomor Aju*).

### 2. OAuth2 Token Lifecycle Management
* Authentication uses the Bearer token protocol (`/v1/openapi-auth/user/login`).
* Access tokens are valid for 300 seconds (5 minutes) while refresh tokens remain valid for 24 hours.
* BZone autonomously monitors token expiration and triggers **Auto-Refresh** with a 30-second pre-expiration safety buffer.

---

## Reference Data Synchronization Services

`djbc_ceisa_core` includes automated wizards to fetch official master data directly from Customs servers:

### 1. Weekly Customs Exchange Rates (`ceisa.currency.rate`)
* Menu: **CEISA Connector > Layanan CEISA > Sinkronisasi Kurs**.
* Pulls weekly Ministry of Finance Decree (KMK) exchange rates for calculating Customs Value (NDPBM).
* Can be scheduled as an automated cron job every Wednesday when new rates take effect.

### 2. HS Codes & Duty Tariffs (`ceisa.tarif`)
* Menu: **CEISA Connector > Layanan CEISA > Sinkronisasi Tarif**.
* Fetches import duty rates, VAT, Luxury Goods Sales Tax, and Article 22 Income Tax for specific 8-digit or 10-digit HS codes from the Indonesian Customs Tariff Book (BTKI).

### 3. Trade Restrictions & Prohibitions / Lartas (`ceisa.lartas`)
* Menu: **CEISA Connector > Layanan CEISA > Sinkronisasi Lartas**.
* Checks regulatory requirements (Ministry of Trade, Quarantine, BPOM permits) applicable prior to clearance.

### 4. Manifest Integration (`ceisa.manifes`)
* Menu: **CEISA Connector > Layanan CEISA > Sinkronisasi Manifes**.
* Retrieves cargo manifest (BC 1.1) records from Temporary Storage Facilities (TPS) at air and sea ports by manifest bill and sub-bill numbers.

---

## Master Data Alignment

Align internal Odoo master data prior to processing customs declarations:

1. **Product HS Codes**:
    * Navigate to **Inventory > Products > Products**.
    * On the **CEISA** tab, assign the correct **Pos Tarif / HS Code** and standard CEISA UoM.
2. **Customs Partner Profiles**:
    * Navigate to **Contacts**.
    * On the **CEISA** tab, specify the partner's **16-digit NPWP**, **NIB (Business Identification Number)**, and relevant entity roles.
3. **UOM Standard Mapping**:
    * Map Odoo internal UoMs to CEISA standard international codes (e.g. `KGM` for kilograms, `PCE` for pieces, `LTR` for liters).

---

## API Logging & Audit Trails

Every outbound request payload and inbound response from CEISA is recorded for auditability:

* **API Log Menu**: Located at **CEISA Connector > Configuration > API Log**.
* Captured metrics include:
    * Endpoint URL accessed.
    * HTTP Status Code (200 OK, 400 Bad Request, 401 Unauthorized, 500 Server Error).
    * Request JSON Body and Response JSON Body.
    * Round-trip execution latency.
* Empowers operations and IT teams to swiftly troubleshoot submission rejections (*rejects*) or schema validation errors.

> [!TIP]
> Always verify your credentials by clicking the **Test Connection** button on the settings page before initiating your first live document submission.
