# BZone Ecosystem Changelog

Release history, feature additions, system improvements, and fixes across the BZone product ecosystem.

---

## [v19.0.1.0.2] - 2026-09-08

### Security & System Protection
- [New] Telegram Security Alerts: Automated instant alerts sent directly to administrator Telegram groups/accounts upon account lockouts caused by repeated unauthorized login attempts.
- [Improvement] Enhanced Access Security: Strengthened brute-force protection mechanisms with configurable threshold parameters.

### Customs & Pabean Documents
- [Improvement] Customs Document & Logistics Integration: Automatic filtering of customs documents based on Sales Order (SO) and Purchase Order (PO) references during stock picking operations.
- [Improvement] Free Trade Zone (FTZ) Form Enhancements: Refined data layout and visual form structure for FTZ documents to facilitate efficient customs inspection.
- [Improvement] Entity Autocomplete: Instant auto-completion for carrier and consignee entity details on customs documents to accelerate data entry.
- [Improvement] Standardized Document Numbering: Optimized automatic sequencing algorithms to prevent duplicate customs document numbers.

### IT Inventory & Stock Movements
- [Improvement] Inventory Mutation Reconciliation: Adjusted mutation report calculations so that sales returns are automatically and accurately credited under incoming stock.
- [Fix] KITE Facility Reporting Format: Fixed delimiter and calculation validations for import and export realization reports under the KITE facility.

### Dynamic Financial & Accounting Reports
- [Improvement] Interactive Financial Report Interface: Redesigned financial reports (Balance Sheet, Profit & Loss, General Ledger, Trial Balance) with an interactive modern Date Picker and intuitive navigation.
- [Fix] Financial Report Print Filter: Corrected date range filtering when printing financial report documents.

### Manufacturing & User Experience
- [Improvement] Flexible Shopfloor Interface: Production workcenter lists now feature smooth, responsive scrolling across diverse screen sizes.

---

## [v19.0.1.0.1] - 2026-08-20

### Security & User Access
- [New] Brute-Force Login Protection: Implemented login attempt limits to safeguard user accounts against unauthorized access attempts.

### Customs Service Integration
- [Improvement] Customs Service Compatibility: Upgraded connection and authentication protocols for more stable and secure synchronization with customs services.
- [Improvement] Connection Audit Logging: Enhanced interaction logs with customs systems to facilitate compliance auditing and transmission tracking.
- [Improvement] Customs Data Retrieval Menu: Added a unified dashboard for querying and fetching official customs reference data on demand.
- [Improvement] Customs Document Search: Introduced comprehensive filtering and multi-criteria search capabilities for customs documents.

### Financials & Multi-Currency
- [Improvement] Dual Currency Presentation: Display transaction amounts in both company functional currency and foreign currency side-by-side on transaction forms.
- [Improvement] Initial Balance Visibility: Added Initial Balance presentation and quick account search filters on Balance Sheet and Trial Balance reports.
- [Fix] Transaction Reference Handling: Ensured source transaction references remain clearly visible even when line descriptions are omitted.

---

## [v19.0.1.0.0] - 2026-07-20

### IT Inventory & Customs Ecosystem Foundation
- [New] Official BZone Release for Odoo 19: Core integrated customs IT Inventory foundation, supporting full Indonesian customs documentation (BC 2.0, BC 2.3, BC 2.5, BC 4.0, PPFTZ).
- [New] Packaging & Unit of Measure (UOM) Management: Standardized units of measure and cargo packaging in compliance with Indonesian customs regulations.
- [New] Traceability & Batch/Lot Tracking: Complete tracking of raw materials, work-in-progress, and finished goods from receipt through delivery.
- [New] Standardized Reporting Periods: Date range selector wizard and period standardization for regulatory customs audits.
- [New] Comprehensive Audit Trail: Activity logging across all customs transaction records for compliance assurance.
