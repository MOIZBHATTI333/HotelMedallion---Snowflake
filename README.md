# HotelMedallion

A medallion architecture (Bronze → Silver → Gold) data pipeline built on Snowflake that ingests, cleans, and aggregates hotel booking data.

## Architecture

```
CSV File → Stage → Bronze (raw) → Silver (cleaned) → Gold (aggregated)
```

## Pipeline Stages

### Bronze Layer
- Creates a Snowflake database, file format, and stage
- Loads raw CSV booking data into `BRONZE_HOTEL_BOOKINGS` with all columns as STRING for flexible ingestion

### Silver Layer
- Validates and cleans data:
  - Normalizes city and customer names (INITCAP, TRIM)
  - Validates email format
  - Casts dates and amounts to proper types
  - Fixes negative amounts and typos in booking status
  - Filters out records with invalid or conflicting dates
- Outputs to `SILVER_HOTEL_BOOKINGS` with enforced data types

### Gold Layer
- **GOLD_AGG_DAILY_BOOKING** — Daily booking counts and revenue
- **GOLD_AGG_HOTEL_CITY_SALES** — Total revenue by city
- **GOLD_BOOKING_CLEAN** — Finalized clean booking records

## Tech Stack

- **Snowflake** — Cloud data warehouse
- **SQL** — All transformations
- **Snowflake WorkSpace** — Development environment

## Data Quality Checks

- Invalid email detection
- Negative amount detection
- Check-in/check-out date conflict detection
- Booking status standardization

## How to Run

1. Create a Snowflake account and warehouse
2. Upload your hotel bookings CSV to the stage (`@STG_HOTEL_BOOKINGS`)
3. Run all cells in `Hotel_Data_Project.ipynb` sequentially
