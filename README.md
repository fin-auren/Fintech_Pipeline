# Fintech_Pipeline

# Financial Data Reconciliation & JSON Normalization Report

This notebook processes, reconciles, and reports discrepancies between a company's internal **Ledger** records and external **Payment Gateway** records, followed by normalising raw **API JSON** settlement data.

---

## 1. Input Datasets
* **Ledger Dataset (`ledger.csv`):** 10 rows, 6 columns
* **Gateway Dataset (`gateway.csv`):** 9 rows, 6 columns
* **API Response (`api_response_sample.json`):** Nested JSON containing batch and settlement details.

---

## 2. Preprocessing & Quality Checks
* **Column Renaming:** Columns such as `transaction_date`, `amount_usd`, and `status` were renamed with prefixes (`ledger_` and `gateway_`) to avoid overlaps during data merging.
* **Duplicates & Nulls:** 
  * No duplicate transactions detected in either dataset.
  * No missing (null) values present in either dataset.

---

## 3. Reconciliation Findings & Mismatches

| Issue Type | Count | Description | Severity |
| :--- | :---: | :--- | :---: |
| **Missing in Gateway** | 2 | Transactions present in Ledger but not in Gateway. | **High** |
| **Missing in Ledger** | 1 | Transactions present in Gateway but not in Ledger. | **High** |
| **Amount Mismatch** | 2 | Shared Transaction IDs with differing amounts. | **Medium** |
| **Status Mismatch** | 1 | Shared Transaction IDs with conflicting statuses. | **Medium** |
| **Total Issues** | **6** | Combined reconciling items. | - |

---

## 4. Generated Artifacts & Reports
The following structured reports were exported to the environment:
* `reconciliation_report.csv` / `final_reconciliation_report.csv`: Complete consolidated list of all differences with mapped severities.
* `missing_in_gateway.csv`: Detailed details of transactions missing from the gateway.
* `missing_in_ledger.csv`: Detailed details of transactions missing from the ledger.
* `amount_mismatches.csv`: Discrepant amounts with calculated variances (`amount_difference`).
* `status_mismatches.csv`: Mismatched statuses for investigation.
* `summary_metrics.csv` / `summary_metrics.json`: Overall execution and discrepancy metrics.

---

## 5. JSON Normalization
* Extracted nested settlements using `pd.json_normalize` with metadata keys (`batch_id`, `merchant_id`, `merchant_name`, `region`).
* Cleaned and standardized headers (lowercased, dot-to-underscore replacement).
* Standardized date/time fields (`processed_at`, `generated_at`) to datetime formats.
* Exported the flattened tabular output to `api_normalized.csv`.
