# Data Quality Report

## Dataset
- **Source:** UCI Machine Learning Repository — Online Retail (Chen, 2015), DOI: 10.24432/C5BW33
- **File:** `data/raw/Online Retail.xlsx`
- **SHA-256:** _fill in from your Stage 1 checksum output_
- **Download date:** _fill in_
- **License:** CC BY 4.0

## Stage 0: Raw snapshot (before any cleaning)

Captured from `df_raw`, prior to any row deletion or transformation.

| Metric | Value |
|---|---|
| Rows | 541,909 |
| Columns | 8 |
| Date range | 2010-12-01 08:26:00 to 2011-12-09 12:50:00 |
| Unique invoices | 25,900 |
| Unique products (StockCode) | 4,070 |
| Unique known customers | 4,372 |
| Unique countries | 38 |
| Exact duplicate rows | 5,268 |
| Null `CustomerID` | 135,080 (≈24.9%) |
| Null `Description` | 1,454 |
| Null `InvoiceNo` / `StockCode` / `Quantity` / `InvoiceDate` / `UnitPrice` / `Country` | 0 |
| Cancellation lines (`InvoiceNo` starts with `C`) | 9,288 |
| Non-positive quantity (`Quantity` <= 0) | 10,624 |
| Non-positive price (`UnitPrice` <= 0) | 2,517 |

### Observations

- Non-positive quantity (10,624) exceeds cancellation-flagged lines (9,288) by 1,336 rows — meaning some negative/zero-quantity rows are **not** captured by the `C`-prefix cancellation convention. These need a separate rule before being classified as cancellations, returns, or data errors (see anomaly analysis, Section 15.4 of the blueprint).
- `Description` nulls (1,454) do not block RFM/clustering work since `StockCode` (0 nulls) is the stable item identifier per Section 15.3; `Description` is used for display only.
- `CustomerID` nulls (135,080) will exclude those rows from any customer-level analysis (RFM, segmentation, prediction) but they remain valid for product-level and country-level analyses (e.g. OLAP `FactSales`).
- Exact duplicate rows (5,268) — decision on handling still pending (see "Open decisions" below).

## Open decisions (to be resolved and logged before Stage 1 cleaning)

- [ ] Duplicate-row rule: keep as legitimate repeat line items vs. drop as data-entry duplicates.
- [ ] Rule for non-positive-quantity rows not flagged as cancellations (the 1,336-row gap).
- [ ] Confirm RFM reference date for baseline reproduction (Section 17.1).

## Stage 1: Profiled table (`df_profiled`)

Same row count as raw (541,909 rows, 15 columns after adding flags). No rows deleted.

| Metric | Value |
|---|---|
| `IsValidPurchaseLine` = True | 397,884 |
| `IsValidPurchaseLine` = False | 144,025 |