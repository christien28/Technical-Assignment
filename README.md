# Technical-Assignment
This project implements a data pipeline that transforms raw sales data into structured, business-ready datasets.

## Architecture
- Bronze: Raw ingestion 
- Silver: Cleaned & validated data 
- Gold: Business-ready datasets

## Data Source
- File : retail.csv
- Format: CSV
- Contains transactional and customer data

## Bronze Layer
- Reads raw CSV data using Spark
- Infers schema and standardizes column names
- Adds metadata (execution_datetime, source_file)
- Stores data in bronze.sales_details

## Silver Layer
- Selects only required columns
- Converts date fields to proper date types
- Cleans categorical data 
- Removes duplicates (order_id)
- Applies business rules (e.g., valid shipment dates)
- Splits customer_name into first and last name
- Partitions data by year, month, day

Output: silver.orders

## Gold Layer
### gold.sales
- Columns: order_id, order_date, ship_date, ship_mode, city, execution_datetime, source_file
- Transactional dataset for reporting
### gold.customer
- Columns: customer_id, customer_first_name, customer_last_name ,customer_segment, country, execution_datetime, source_file, orders_last_1_month, orders_last_6_month,  orders_last_12_month, order_all_time
- Customer-level aggregation
- Metrics:
> - Orders last 1 month
> - Orders last 6 months
> - Orders last 12 months
- Total orders
- Uses dynamic latest date (max(order_date))
- Recomputed each run (overwrite)

## Schema Evolution Strategy
- Bronze Layer: Accepts new columns
- Silver Layer: Keeps only validated columns
- Gold Layer: Strict business schema

## Data Quality & Business Rules
- Remove duplicate orders
- Validate ship_date ≥ order_date
- Drop missing critical fields
- Standardize categorical columns
- Ensure consistent schema

## Logging & Debugging

Structured logging is used to track:
- Row counts
- Transformations
- Pipeline steps
