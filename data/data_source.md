# Data Source

## Dataset

COFINFAD — Colombian Fintech Financial Analytics Dataset

## Scope

- 48,723 customers
- 3,159,157 transactions
- January 4 to December 29, 2023
- Currency: Colombian Peso (COP)

## Files Used

### customer_data.csv
Grain: one row per customer.

### transactions_data.csv
Grain: one row per transaction.

## Join Key

`customer_id`

## Key Fields Used

- `customer_segment`
- `avg_tx_value`
- `total_tx_volume`
- `tx_count`
- `app_logins_frequency`
- `feature_usage_diversity`
- `active_products`
- `transaction_date`
- `amount`

## Data Quality Notes

- Customer IDs are unique in the customer table.
- All transaction customer IDs match a customer record.
- Customer-level transaction summaries reconcile with raw transaction history.
- 102 exact duplicate transaction rows are present.
- No transaction ID is available to determine whether those rows are true duplicates.

## Known Limitations

- The methodology used to create `customer_segment` is not disclosed.
- `active_products` does not fully reconcile with the five visible product flags.
- Churn probability and CLV-related fields appear to be modeled or estimated and are not used as independent validation.
- Findings describe relationships within this dataset and should not be generalized to all digital banking customers.

## Provenance

Source: COFINFAD Colombian Fintech Financial Analytics Dataset  
Creator: To be documented from the original source  
Download date: To be added  
Source URL: To be added  
License: To be documented from the exact downloaded source
