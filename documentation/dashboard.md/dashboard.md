# Enterprise Analytics Dashboard Documentation

## Dashboard Overview

This document describes the enterprise analytics dashboard built for the Olist E-commerce dataset. The dashboard provides comprehensive business intelligence insights across sales, customers, products, delivery, and customer satisfaction metrics.

## Data Source

- **Database**: Snowflake (OLIST_E_COMMERCE.PUBLIC schema)
- **Primary View**: `final_dashboard` - consolidated reporting view
- **Source Tables**: Customers, Orders, Order Items, Payments, Reviews, Products, Sellers, Geolocation

## Dashboard Pages

### 1. Executive Summary

High-level KPIs for leadership review:
- Total Revenue
- Total Orders
- Average Order Value (AOV)
- Total Customers
- Customer Satisfaction Score (Avg Rating)
- On-Time Delivery Rate

### 2. Sales Performance

- Monthly revenue trends with YoY comparison
- Order volume trends
- Revenue by product category
- Top-selling products
- Sales funnel analysis

### 3. Customer Analytics

- Customer acquisition trends
- Customer segmentation (New vs Returning)
- Customer Lifetime Value (CLV) distribution
- Geographic customer distribution
- Customer loyalty status (Active, At Risk, Dormant, Lost)

### 4. Product Analysis

- Revenue by product category
- Top performing products
- Product pricing analysis
- Category-wise order distribution
- Product-seller relationship analysis

### 5. Delivery & Logistics

- Average delivery time trends
- On-time vs late delivery breakdown
- Delivery performance by state/region
- Seller delivery performance
- Freight cost analysis

### 6. Customer Satisfaction

- Review score distribution
- Rating trends over time
- Review volume by month
- Correlation between delivery time and ratings
- Negative review analysis

### 7. Payment Analysis

- Payment method distribution
- Payment value by type
- Credit card vs installment analysis
- Payment success rate

## Key Metrics Definitions

| Metric | Formula | Description |
|--------|---------|-------------|
| Total Revenue | SUM(Price) | Total sales revenue |
| AOV | SUM(Price) / COUNT(Orders) | Average Order Value |
| On-Time Rate | On-Time Orders / Total Orders * 100 | Percentage of on-time deliveries |
| Customer Retention | Returning Customers / Total Customers * 100 | Customer retention rate |
| Avg Rating | AVG(Review Score) | Average customer rating (1-5) |
| CLV | Total Revenue per Customer | Customer Lifetime Value |

## Filters & Interactions

The dashboard supports the following filters:
- **Date Range**: Select custom time periods
- **State**: Filter by customer state
- **Product Category**: Filter by product category
- **Payment Type**: Filter by payment method
- **Seller**: Filter by seller

## Refresh Schedule

- **Data Source**: Snowflake (direct query)
- **Refresh Frequency**: Daily (automated via ETL pipeline)
- **Last Updated**: Check dashboard footer for timestamp

## Access & Permissions

- **Viewers**: Read-only access to dashboards
- **Editors**: Can modify visualizations and add pages
- **Admins**: Full access including data source management

## Technical Details

- **Platform**: Microsoft Power BI / Fabric
- **Data Connection**: DirectQuery to Snowflake
- **Row-Level Security**: Implemented for multi-tenant access
- **Mobile Optimized**: Responsive layout for mobile viewing

## Contact

For questions or feature requests, contact:
- **Deepanraj A** | Data Analyst
- LinkedIn: linkedin.com/in/deepanraj-a
