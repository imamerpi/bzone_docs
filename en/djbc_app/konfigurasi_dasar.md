# Basic Configurations & Master Documents (djbc_app)

The **`djbc_app`** module serves as the core foundation for the entire BZone ecosystem. It provides standardized customs master data, customs registration validations, and reporting period rules.

---

## Customs Document Types Master Data

Every inbound and outbound movement of goods at facilities granted customs privileges must be linked to a valid Indonesian customs document type.

Under **Laporan Bea Cukai > Configuration > Document Type**, the system predefines official customs document types:

* **BC 2.3**: Import declaration into Bonded Storage Facilities (Kawasan Berikat).
* **BC 2.5**: Outbound declaration from Bonded Zone to domestic customs territory (TLDDP / Local Market).
* **BC 4.0**: Inbound declaration from domestic territory (TLDDP) into Bonded Zone.
* **BC 4.1**: Outbound declaration of domestic-origin goods from Bonded Zone back to TLDDP.
* **BC 2.7**: Movement of goods between Bonded Zones or other bonded facilities.
* **PPFTZ**: Customs declaration for Free Trade Zones.

![Customs Document Types Configuration](/assets/images/djbc_document_type.png)
*Figure 2.1: Configuration of customs document types in BZone.*

> [!WARNING]
> Do not alter the official code or registration type once the system is in live operation, as it directly affects data reconciliation with customs systems.

---

## DJBC Unit of Measure (UOM) Standardization

Indonesian customs regulations require standard international units of measure (e.g. `KGM` for Kilograms, `PCE` for Pieces, `MTR` for Meters).

Steps for UOM mapping:

1. Navigate to **Inventory > Configuration > Units of Measure**.
2. Open the desired unit of measure.
3. In the **DJBC Mapping** tab, ensure the **DJBC Code** field is populated with the official code.

![UOM Mapping to DJBC Standard](/assets/images/djbc_uom_mapping.png)
*Figure 2.2: Mapping internal units of measure to official DJBC standards.*

---

## Packaging Management

Packaging type specifications (e.g. Pallet, Drum, Box, Bag) are mandatory when creating customs declarations.

* Access **Laporan Bea Cukai > Master Type > Kemasan**.
* Verify that packaging codes correspond to customs standards (e.g. `PX` for Pallet, `BX` for Box).

---

## Master Reporting Periods (Report Period)

Master Reporting Periods provide predefined date ranges to simplify and speed up report generation for periodic reporting (e.g., monthly, quarterly, or annual reports). This feature serves as a shortcut helper to auto-populate date fields in the report wizard (*Date Range Selector*), **not** for locking operational transactions or inventory movements.

* Access **Laporan Bea Cukai > Configuration > Report Period**.
* Create a new period by specifying:
  * **Period**: Period identifier (e.g., `January 2025`, `Q1 2025`).
  * **Code**: Unique period code (e.g., `2025-01`).
  * **Date Start**: Period starting date.
  * **Date End**: Period ending date.

> [!TIP]
> When generating reports via the report wizard (such as the IT Inventory Mutation Reports), selecting a **Report Period** will automatically populate the **Date Start** and **Date End** fields. Users can still manually enter or adjust date ranges freely if custom query windows are needed.
