# Star Schema Design - Olist E-Commerce Analysis

## Database: OLIST_E_COMMERCE
## Schema: PUBLIC
## Platform: Snowflake

---

## Star Schema Overview

The star schema is designed with `order_items` as the fact table, surrounded by dimension tables for customers, products, sellers, orders, payments, and reviews.

---

## Schema Diagram

```
                         FACT_ORDER_ITEMS (Fact Table)
                        /    |    |    |    |    \\
                       /     |    |    |     |     \\
          DIM_CUSTOMERS  DIM_PRODUCTS  DIM_SELLERS  V_ORDERS  V_PAYMENTS  V_REVIEWS
```

---

## Fact Table: FACT_ORDER_ITEMS

| Column | Type | Description |
|--------|------|-------------|
| ORDER_ID | VARCHAR(32) | Order identifier |
| ORDER_ITEM_ID | INT | Order item sequence |
| PRODUCT_ID | VARCHAR(32) | Product identifier |
| SELLER_ID | VARCHAR(32) | Seller identifier |
| SHIPPING_LIMIT_DATE | TIMESTAMP | Shipping deadline |
| PRICE | DECIMAL(12,2) | Item price |
| FREIGHT_VALUE | DECIMAL(12,2) | Shipping cost |
| TOTAL_ITEM_VALUE | DECIMAL(12,2) | PRICE + FREIGHT_VALUE |
| CUSTOMER_ID | VARCHAR(32) | Customer identifier |
| ORDER_STATUS | VARCHAR | Order status |
| ORDER_PURCHASE_TIMESTAMP | TIMESTAMP | Order date |
| ORDER_APPROVED_AT | TIMESTAMP | Approval date |
| ORDER_DELIVERED_CARRIER_DATE | TIMESTAMP | Carrier pickup date |
| ORDER_DELIVERED_CUSTOMER_DATE | TIMESTAMP | Delivery date |
| ORDER_ESTIMATED_DELIVERY_DATE | TIMESTAMP | Estimated delivery |
| DELIVERY_DAYS | INT | Actual delivery days |
| DELAY_DAYS | INT | Days late/early vs estimate |
| DELIVERY_STATUS | VARCHAR | Late / On Time / Not Delivered |

**Row Count:** 112,650

---

## Dimension Tables

### DIM_CUSTOMERS

| Column | Type | Description |
|--------|------|-------------|
| CUSTOMER_ID | VARCHAR(32) | Customer identifier |
| CUSTOMER_UNIQUE_ID | VARCHAR(32) | Unique customer ID |
| CUSTOMER_ZIP_CODE_PREFIX | INT | ZIP code prefix |
| CUSTOMER_CITY | VARCHAR | Customer city |
| CUSTOMER_STATE | VARCHAR(2) | Customer state (BR) |

**Row Count:** 99,441

### DIM_PRODUCTS

| Column | Type | Description |
|--------|------|-------------|
| PRODUCT_ID | VARCHAR(32) | Product identifier |
| PRODUCT_CATEGORY_NAME | VARCHAR | Category (Portuguese) |
| PRODUCT_CATEGORY_ENGLISH | VARCHAR | Category (English) |
| PRODUCT_NAME_LENGTH | INT | Name character length |
| PRODUCT_DESCRIPTION_LENGTH | INT | Description length |
| PRODUCT_PHOTOS_QTY | INT | Number of photos |
| PRODUCT_WEIGHT_G | INT | Weight in grams |
| PRODUCT_LENGTH_CM | INT | Length in cm |
| PRODUCT_HEIGHT_CM | INT | Height in cm |
| PRODUCT_WIDTH_CM | INT | Width in cm |

**Row Count:** 32,340

### DIM_SELLERS

| Column | Type | Description |
|--------|------|-------------|
| SELLER_ID | VARCHAR(32) | Seller identifier |
| SELLER_ZIP_CODE_PREFIX | INT | ZIP code prefix |
| SELLER_CITY | VARCHAR | Seller city |
| SELLER_STATE | VARCHAR(2) | Seller state (BR) |

**Row Count:** 3,094

---

## Supporting Views

### V_ORDERS (Base)
Orders table with 8 timestamp fields (purchase, approved, carrier, delivered, estimated).
**Row Count:** 99,441

### V_PAYMENTS (Base)
Payment transactions with type, installments, and value.
**Row Count:** 103,886

### V_REVIEWS (Base)
Customer reviews with scores 1-5 and timestamps.
**Row Count:** 99,224

### V_ORDER_ITEMS (Base)
Order items with product, seller, price, and freight.
**Row Count:** 112,650

### V_PRODUCTS (Base)
Product catalog with dimensions and category.
**Row Count:** 32,340

### V_CUSTOMERS (Base)
Customer records with location data.
**Row Count:** 99,441

### V_SELLERS (Base)
Seller records with location data.
**Row Count:** 3,094

### V_CATEGORY_TRANSLATION (Base)
Portuguese to English category name mapping.
**Row Count:** 70

---

## Relationships

| Fact Column | Dimension | Join Key |
|-------------|-----------|----------|
| CUSTOMER_ID | DIM_CUSTOMERS | CUSTOMER_ID |
| PRODUCT_ID | DIM_PRODUCTS | PRODUCT_ID |
| SELLER_ID | DIM_SELLERS | SELLER_ID |
| ORDER_ID | V_ORDERS | ORDER_ID |
| ORDER_ID | V_PAYMENTS | ORDER_ID |
| ORDER_ID | V_REVIEWS | ORDER_ID |

---

## Key Design Decisions

1. **Denormalized Fact Table**: FACT_ORDER_ITEMS includes customer and order attributes to minimize joins for common BI queries.

2. **English Category Names**: DIM_PRODUCTS includes PRODUCT_CATEGORY_ENGLISH via LEFT JOIN with V_CATEGORY_TRANSLATION for BI readability.

3. **Derived Delivery Metrics**: DELIVERY_DAYS, DELAY_DAYS, and DELIVERY_STATUS are pre-calculated in the fact table for efficient time-based analysis.

4. **Snowflake-Specific Types**: Uses TRY_TO_TIMESTAMP() and TRY_CAST() for robust type conversion from the raw denormalized data.

---

## Power BI Views (Built on Star Schema)

| View | Purpose | Rows |
|------|---------|------|
| PBI_MONTHLY_KPI | Monthly aggregated KPIs with MoM growth | 24 |
| PBI_CUSTOMER_SUMMARY | Customer-level spend, tenure, segmentation | 96,218 |
| PBI_MASTER_DATASET | Fully denormalized dataset with all features | 112,650 |

