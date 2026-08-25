# Enterprise Analytics Project

## Overview
A comprehensive end-to-end data analytics project analyzing the Olist E-commerce dataset using modern cloud and BI tools. This project demonstrates data engineering, SQL analysis, and business intelligence capabilities.

## Project Structure

```
enterprise-analytics-project/
|-- documentation/       # Data dictionary and architecture docs
|-- sql/                 # 33+ SQL analytical queries (star schema, Snowflake)
|-- .env.example         # Environment variable template (Azure Blob Storage)
|-- requirements.txt     # Python dependencies
```

## Technologies Used

- **Snowflake** - Cloud data warehouse for data modeling and SQL analysis
- **SQL** - 33+ analytical queries across sales, customer, delivery, and review metrics
- **Azure Blob Storage** - Cloud storage for raw dataset files (config template provided; actual pipeline not included in this repo)

## Dataset

The project uses the **Olist E-commerce Dataset** containing 9 CSV files:
- Customers, Orders, Order Items, Order Payments, Order Reviews
- Products, Sellers, Geolocation, Product Category Translation

## Key Features

- Star Schema data modeling (Dim/Fact tables) — see `documentation/`
- 33+ SQL queries for business metrics (sales, customer, delivery, review analysis)
- Feature engineering and data optimization in SQL

## Installation

1. Clone the repository
2. Copy `.env.example` to `.env` and fill in your own Azure Blob Storage connection string (if using Blob Storage for the raw CSVs)
3. Run the queries in `sql/analysis.sql` against a Snowflake warehouse loaded with the Olist dataset
4. Follow the setup guides in the documentation folder

## Author

**Deepanraj A** - Data Analyst | SQL | Power BI | Python

Portfolio: [deeepanbe.github.io](https://deeepanbe.github.io)
GitHub: [github.com/deeepanbe](https://github.com/deeepanbe)


## SQL Analysis

The `sql/` folder contains comprehensive analytical queries:

- **33+ SQL queries** covering sales, customer, product, delivery, and review metrics
- **final_dashboard view** - A consolidated reporting view for Power BI consumption
- Queries use Snowflake SQL syntax targeting `OLIST_E_COMMERCE.PUBLIC` schema
- Key analysis areas:
  - Revenue and order trends
  - Customer segmentation and lifetime value
  - Product category performance
  - Delivery performance and logistics
  - Customer satisfaction and reviews
  - Geographic market analysis
  - Payment method distribution

## Data Modeling

- **Star Schema** design with `order_items` as the central fact table
- Dimension tables: `DIM_CUSTOMERS`, `DIM_PRODUCTS`, `DIM_SELLERS`, `DIM_GEOLOCATION`, `DIM_ORDERS`
- Refer to `documentation/02_star_schema.md` for detailed schema documentation
- Refer to `documentation/01_views_overview.md` for view definitions

## Power BI Dashboard

The Power BI dashboard connects to Snowflake and includes:
- Executive KPI summary cards
- Revenue trends and forecasting
- Customer acquisition and retention metrics
- Product performance analysis
- Delivery performance tracking
- Geographic sales distribution
- Review sentiment analysis

## Contributing

Feel free to fork this repository and submit pull requests. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is open source and available under the MIT License.

## Author

**Deepanraj A** | Data Analyst
- LinkedIn: [linkedin.com/in/deepanraj-a](https://www.linkedin.com/in/deepanraj-a)
- GitHub: [github.com/deeepanbe](https://github.com/deeepanbe)
