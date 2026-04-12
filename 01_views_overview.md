
# SQL Views Overview - Olist E-Commerce Analysis

**Database:** OLIST_E_COMMERCE  
**Schema:** PUBLIC  
**Platform:** Snowflake  
**Data Source:** Olist Brazilian E-Commerce Dataset (Sep 2016 - Oct 2018)

---

## Data Validation Summary

| View | Row Count | Type |
|------|-----------|------|
| V_ORDERS | 99,441 | Base |
| V_CUSTOMERS | 99,441 | Base |
| V_ORDER_ITEMS | 112,650 | Base |
| V_PRODUCTS | 32,340 | Base |
| V_PAYMENTS | 103,886 | Base |
| V_REVIEWS | 99,224 | Base |
| V_SELLERS | 3,094 | Base |
| V_CATEGORY_TRANSLATION | 70 | Base |

**Null Analysis (Orders):**  
- 160 null `order_approved_at`
- 1,783 null `carrier_date`
- 2,965 null `delivered_customer_date` (non-delivered orders)

**Data Period:** Sep 2016 through Oct 2018

---

## Base Entity Views (Step 1)

### V_ORDERS
Extracts order records from denormalized source table.
- **Columns:** ORDER_ID, CUSTOMER_ID, ORDER_STATUS, ORDER_PURCHASE_TIMESTAMP, ORDER_APPROVED_AT, ORDER_DELIVERED_CARRIER_DATE, ORDER_DELIVERED_CUSTOMER_DATE, ORDER_ESTIMATED_DELIVERY_DATE
- **Filter:** Status values include delivered, shipped, canceled, unavailable, invoiced, processing, created, approved

### V_CUSTOMERS
Extracts customer records with unique identifiers.
- **Columns:** CUSTOMER_ID, CUSTOMER_UNIQUE_ID, CUSTOMER_ZIP_CODE_PREFIX, CUSTOMER_CITY, CUSTOMER_STATE
- **Filter:** 32-char IDs, 2-char state codes

### V_ORDER_ITEMS
Extracts line-item level order data.
- **Columns:** ORDER_ID, ORDER_ITEM_ID, PRODUCT_ID, SELLER_ID, SHIPPING_LIMIT_DATE, PRICE, FREIGHT_VALUE
- **Filter:** Numeric price/freight values with valid timestamps

### V_PRODUCTS
Extracts product catalog with dimensions.
- **Columns:** PRODUCT_ID, PRODUCT_CATEGORY_NAME, PRODUCT_NAME_LENGTH, PRODUCT_DESCRIPTION_LENGTH, PRODUCT_PHOTOS_QTY, PRODUCT_WEIGHT_G, PRODUCT_LENGTH_CM, PRODUCT_HEIGHT_CM, PRODUCT_WIDTH_CM
- **Filter:** 32-char product IDs with numeric dimension fields

### V_PAYMENTS
Extracts payment transactions per order.
- **Columns:** ORDER_ID, PAYMENT_SEQUENTIAL, PAYMENT_TYPE, PAYMENT_INSTALLMENTS, PAYMENT_VALUE
- **Filter:** Payment types: credit_card, boleto, voucher, debit_card, not_defined

### V_REVIEWS
Extracts customer review data.
- **Columns:** REVIEW_ID, ORDER_ID, REVIEW_SCORE, REVIEW_COMMENT_TITLE, REVIEW_COMMENT_MESSAGE, REVIEW_CREATION_DATE, REVIEW_ANSWER_TIMESTAMP
- **Filter:** Scores between 1-5 with valid timestamps

### V_SELLERS
Extracts seller registry information.
- **Columns:** SELLER_ID, SELLER_ZIP_CODE_PREFIX, SELLER_CITY, SELLER_STATE
- **Filter:** 32-char seller IDs, 2-char state codes

### V_CATEGORY_TRANSLATION
Provides Portuguese-to-English category name mapping.
- **Columns:** PRODUCT_CATEGORY_NAME, PRODUCT_CATEGORY_NAME_ENGLISH
- **Rows:** 70 category translations

---

## Dimension Views (Step 2 - Star Schema)

### DIM_CUSTOMERS
Customer dimension for star schema.
- **Source:** V_CUSTOMERS
- **Columns:** CUSTOMER_ID, CUSTOMER_UNIQUE_ID, CUSTOMER_ZIP_CODE_PREFIX, CUSTOMER_CITY, CUSTOMER_STATE

### DIM_PRODUCTS
Product dimension with English category names.
- **Source:** V_PRODUCTS LEFT JOIN V_CATEGORY_TRANSLATION
- **Columns:** PRODUCT_ID, PRODUCT_CATEGORY_NAME, PRODUCT_CATEGORY_ENGLISH, PRODUCT_NAME_LENGTH, PRODUCT_DESCRIPTION_LENGTH, PRODUCT_PHOTOS_QTY, PRODUCT_WEIGHT_G, PRODUCT_LENGTH_CM, PRODUCT_HEIGHT_CM, PRODUCT_WIDTH_CM

### DIM_SELLERS
Seller dimension for star schema.
- **Source:** V_SELLERS
- **Columns:** SELLER_ID, SELLER_ZIP_CODE_PREFIX, SELLER_CITY, SELLER_STATE

---

## Fact Table (Step 2 - Star Schema)

### FACT_ORDER_ITEMS
Central fact table with enriched delivery metrics.
- **Source:** V_ORDER_ITEMS LEFT JOIN V_ORDERS
- **Key Columns:** ORDER_ID, ORDER_ITEM_ID, PRODUCT_ID, SELLER_ID, PRICE, FREIGHT_VALUE, TOTAL_ITEM_VALUE, CUSTOMER_ID, ORDER_STATUS, ORDER_PURCHASE_TIMESTAMP
- **Calculated Fields:**
  - `DELIVERY_DAYS` = DATEDIFF between purchase and delivery
  - `DELAY_DAYS` = DATEDIFF between estimated and actual delivery
  - `DELIVERY_STATUS` = Late / On Time / Not Delivered

---

## Star Schema Diagram

```
                    DIM_CUSTOMERS
                    (customer_id)
                          |
                          v
    DIM_PRODUCTS <-- FACT_ORDER_ITEMS --> DIM_SELLERS
   (product_id)    (order_id)          (seller_id)
                          |
                          v
                   V_PAYMENTS    V_REVIEWS
                  (order_id)    (order_id)
```

**Relationships:**
- FACT_ORDER_ITEMS.ORDER_ID -> V_ORDERS.ORDER_ID (Many-to-One)
- FACT_ORDER_ITEMS.CUSTOMER_ID -> DIM_CUSTOMERS.CUSTOMER_ID (Many-to-One)
- FACT_ORDER_ITEMS.PRODUCT_ID -> DIM_PRODUCTS.PRODUCT_ID (Many-to-One)
- FACT_ORDER_ITEMS.SELLER_ID -> DIM_SELLERS.SELLER_ID (Many-to-One)
- FACT_ORDER_ITEMS.ORDER_ID -> V_PAYMENTS.ORDER_ID (One-to-Many)
- FACT_ORDER_ITEMS.ORDER_ID -> V_REVIEWS.ORDER_ID (One-to-Many)

---

## View Creation Summary

| # | View Name | Type | Purpose | Rows |
|---|-----------|------|---------|------|
| 1 | V_ORDERS | Base | Raw orders data | 99,441 |
| 2 | V_CUSTOMERS | Base | Raw customers data | 99,441 |
| 3 | V_ORDER_ITEMS | Base | Raw order items data | 112,650 |
| 4 | V_PRODUCTS | Base | Raw products catalog | 32,340 |
| 5 | V_PAYMENTS | Base | Raw payment records | 103,886 |
| 6 | V_REVIEWS | Base | Raw review data | 99,224 |
| 7 | V_SELLERS | Base | Raw seller registry | 3,094 |
| 8 | V_CATEGORY_TRANSLATION | Base | Category name mapping | 70 |
| 9 | DIM_CUSTOMERS | Dimension | Customer dimension | 99,441 |
| 10 | DIM_PRODUCTS | Dimension | Product dimension (EN) | 32,340 |
| 11 | DIM_SELLERS | Dimension | Seller dimension | 3,094 |
| 12 | FACT_ORDER_ITEMS | Fact | Central fact table | 112,650 |
| 13 | PBI_MONTHLY_KPI | Power BI | Monthly KPI dashboard | 24 months |
| 14 | PBI_CUSTOMER_SUMMARY | Power BI | Customer segmentation | 96,218 |
| 15 | PBI_MASTER_DATASET | Power BI | Full denormalized dataset | 112,650 |

---

**Total Views Created:** 15  
**Total Data Rows Processed:** ~1,550,931  
**Execution Environment:** Snowflake Cortex Code  
**Created by:** Deepanraj A (ACCOUNTADMIN)