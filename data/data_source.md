# Data Source

## Dataset

**COFINFAD — Colombian Fintech Financial Analytics Dataset**

An anonymized financial analytics dataset containing customer-level attributes and transaction-level activity from a Colombian fintech platform during 2023.

Raw source files are not redistributed in this repository. This repository contains analysis code, documentation, and derived visual outputs only.

---

## Dataset Scope

| Item | Coverage |
|---|---:|
| Customers | 48,723 |
| Transactions | 3,159,157 |
| Analysis period | January 4 – December 29, 2023 |
| Currency | Colombian Peso (COP) |
| Customer grain | One row per customer |
| Transaction grain | One row per transaction |

---

## Files Used

### `customer_data.csv`

Customer-level table containing demographic, transactional, product, digital engagement, and supplied segmentation variables.

**Grain:** one row per customer  
**Primary key:** `customer_id`

### `transactions_data.csv`

Transaction-level table containing individual customer transaction activity.

**Grain:** one row per transaction  
**Customer join key:** `customer_id`

The two tables are joined using:

```text
customer_data.customer_id
            ↕
transactions_data.customer_id
```

---

## Fields Used in the Analysis

The project intentionally uses a subset of the available variables.

### Customer and Segment

- `customer_id`
- `customer_segment`

### Monetary Activity

- `avg_tx_value`
- `total_tx_volume`
- `tx_count`
- `first_tx`
- `last_tx`

### Digital and Product Engagement

- `app_logins_frequency`
- `feature_usage_diversity`
- `active_products`
- `bill_payment_user`
- `auto_savings_enabled`

### Transaction-Level Fields

- `customer_id`
- `transaction_date`
- `amount`

Additional fields were reviewed during discovery but were not automatically included in the final analysis.

---

## Data Validation

Before analyzing the supplied customer segments, I checked the analytical grain and internal consistency of the two source tables.

### Customer Table

- 48,723 customer records
- 48,723 unique `customer_id` values
- no duplicate customer IDs

### Transaction Table

- 3,159,157 transaction rows
- 48,723 unique customers represented
- no transaction customer IDs without a corresponding customer record
- no customers in the customer table without transaction history
- no missing values in the transaction fields used in the analysis

### Transaction Reconciliation

I independently reconstructed the following customer-level measures from raw transaction history:

- transaction count
- average transaction value
- total transaction volume
- first transaction date
- last transaction date

The reconstructed measures reconciled with the corresponding values in the customer-level table across the full customer base.

This validation was completed before using the supplied transaction summaries in downstream analysis.

---

## Duplicate Transactions

The transaction table contains **102 exact duplicate rows**, representing approximately **0.003%** of transaction records.

These rows were retained.

The dataset does not provide a unique transaction identifier, so an identical row cannot be reliably classified as either:

1. an accidental duplicate record, or
2. a legitimate repeated transaction with identical recorded attributes.

Removing the rows would therefore introduce an assumption that cannot be verified from the available data.

---

## Fields Used With Caution

### `customer_segment`

The dataset provides four customer segments:

- Inactive
- Occasional
- Regular
- Power

The methodology used to construct these segments is not disclosed.

For that reason, the project treats segment membership as an **observed label to audit**, not as a known segmentation model.

The analysis can evaluate what the supplied labels align with, but it cannot establish which variables or rules were originally used to create them.

### `active_products`

The supplied `active_products` field does not fully reconcile with the sum of the five visible product indicator fields.

This may indicate that the aggregate includes products not represented by the visible indicators, but the available documentation does not provide enough information to verify that explanation.

The supplied aggregate is therefore used as documented rather than reconstructed from an unsupported definition.

### Modeled Variables

The dataset includes fields such as:

- churn probability
- estimated customer lifetime value

These appear to be modeled or derived measures rather than directly observed outcomes.

They were not used as independent evidence for validating the customer segmentation because doing so could introduce circular reasoning if the underlying models already incorporate transaction or engagement variables.

---

## Analytical Boundaries

This project supports statements about relationships observed **within this dataset**.

It does not establish:

- how the original customer segmentation algorithm was constructed
- that average transaction value was directly used to assign customer segments
- causal relationships between monetary value and customer engagement
- whether engagement-based targeting improves retention, cross-sell, or product adoption
- that the observed relationships generalize to other fintech platforms or banking populations

These boundaries are maintained throughout the notebooks and dashboard.

---

## Provenance

**Dataset:** COFINFAD — Colombian Fintech Financial Analytics Dataset  
**Creators:** Daniel Muñoz Guerrero, Sebastián Ceballos, and Juan Trejos Rojas  
**Original distribution:** Hugging Face  
**Source URL:** [COFINFAD dataset](EXACT_HUGGING_FACE_URL)  
**Downloaded:** September 2026  
**License:** ODC-By, as listed by the Hugging Face source used for this project

The raw dataset remains subject to the terms specified by its original publisher and is not covered by the MIT License applied to this repository's code.

---

## Reproducing the Project

To reproduce the analysis:

1. Obtain `customer_data.csv` and `transactions_data.csv` from the original dataset source.
2. Store the files locally in the project's raw-data directory.
3. Update the local data path at the beginning of the notebooks if necessary.
4. Run the notebooks in numerical order.

Expected local structure:

```text
data/
├── raw/
│   ├── customer_data.csv
│   └── transactions_data.csv
│
└── data_source.md
```

The `data/raw/` directory is excluded from version control.

This keeps the repository focused on reproducible analytical work while preserving the original dataset's distribution and licensing terms.
