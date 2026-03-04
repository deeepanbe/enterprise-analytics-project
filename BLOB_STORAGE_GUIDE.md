# Azure Blob Storage Connection Guide

## Your Olist Dataset

Your **Olist e-commerce dataset** is stored in Azure Blob Storage:
- **Storage Account**: deepanstorage01
- **Container**: olist-data
- **Location**: Central India

### Dataset Files (9 CSV files):
1. olist_customers_dataset.csv
2. olist_geolocation_dataset.csv
3. olist_order_items_dataset.csv
4. olist_order_payments_dataset.csv
5. olist_order_reviews_dataset.csv
6. olist_orders_dataset.csv
7. olist_products_dataset.csv
8. olist_sellers_dataset.csv
9. product_category_name_translation.csv

---

## Step 1: Get Your Connection String

1. Go to Azure Portal: https://portal.azure.com
2. Navigate to **Storage accounts** > **deepanstorage01**
3. Click **Security + networking** > **Access keys**
4. Click **Show** on "Connection string" for key1
5. Copy the entire connection string

---

## Step 2: Configure Environment

In your Codespace, edit the `.env` file:

```bash
# Open .env file
code .env
```

Add your connection string:
```
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=deepanstorage01;AccountKey=YOUR_KEY_HERE;EndpointSuffix=core.windows.net
```

Replace `YOUR_KEY_HERE` with the actual key from Azure Portal.

---

## Step 3: Install Required Packages

```bash
pip install -r requirements.txt
```

This installs:
- azure-storage-blob (for Blob Storage access)
- pandas (for data analysis)
- Other Python libraries

---

## Step 4: Test Connection

Run the connection test:

```bash
python python/blob_storage_connection.py
```

**Expected output:**
```
=== Azure Blob Storage Connection Test ===

Containers in storage account:
  - $logs
  - olist-data

Blobs in 'olist-data' container:
  - olist_customers_dataset.csv (8626 bytes)
  - olist_geolocation_dataset.csv (xxxxx bytes)
  ... (all 9 files listed)

✓ Connection successful!
```

---

## Step 5: Download Dataset Files

Create a script to download files from Blob Storage:

```python
from python.blob_storage_connection import download_blob
import os

# Create dataset folder
os.makedirs("dataset", exist_ok=True)

# Download all CSV files
files = [
    "olist_customers_dataset.csv",
    "olist_geolocation_dataset.csv",
    "olist_order_items_dataset.csv",
    "olist_order_payments_dataset.csv",
    "olist_order_reviews_dataset.csv",
    "olist_orders_dataset.csv",
    "olist_products_dataset.csv",
    "olist_sellers_dataset.csv",
    "product_category_name_translation.csv"
]

for file in files:
    download_blob("olist-data", file, f"dataset/{file}")
    print(f"Downloaded: {file}")
```

---

## Step 6: Analyze Data with Pandas

Once downloaded, analyze with Python:

```python
import pandas as pd

# Load customer data
customers = pd.read_csv("dataset/olist_customers_dataset.csv")
print(customers.head())
print(f"Total customers: {len(customers)}")

# Load orders
orders = pd.read_csv("dataset/olist_orders_dataset.csv")
print(f"Total orders: {len(orders)}")

# Join and analyze
merged = customers.merge(orders, on="customer_id")
print(merged.describe())
```

---

## Use Cases

### 1. SQL Practice
- Upload dataset to Azure SQL Database
- Write queries to analyze sales patterns
- Practice JOINs, aggregations, window functions

### 2. Python Analysis
- Data cleaning and transformation
- Statistical analysis
- Machine learning models
- Visualization with matplotlib/seaborn

### 3. Power BI Dashboards
- Connect Power BI to Blob Storage
- Create interactive visualizations
- Sales trends, customer segmentation
- Geographic analysis

---

## Python Functions Available

In `python/blob_storage_connection.py`:

```python
# List all containers
list_containers()

# List files in a container
list_blobs_in_container("olist-data")

# Download a file
download_blob("olist-data", "olist_customers_dataset.csv", "local_path.csv")
```

---

## Troubleshooting

### Connection Failed
- Verify connection string is correct in .env
- Check Azure firewall rules (allow your IP)
- Ensure storage account key is not expired

### Import Error
- Run: `pip install azure-storage-blob`
- Restart terminal

### File Not Found
- Verify container name: "olist-data" (case-sensitive)
- Check file names match exactly

---

## Best Practices

1. **Never commit .env file to GitHub** (already in .gitignore)
2. **Download files locally for faster analysis**
3. **Use pandas chunks for large files**
4. **Store processed data in Azure SQL for querying**
5. **Create backup of original dataset**

---

## Next Steps

1. ✅ Connect to Blob Storage
2. Download all CSV files to `dataset/` folder
3. Upload data to Azure SQL Database
4. Write SQL queries for analysis
5. Create Python analysis scripts
6. Build Power BI dashboard
7. Document insights in `documentation/`

---

## Resources

- [Azure Blob Storage Python SDK](https://docs.microsoft.com/en-us/python/api/overview/azure/storage-blob-readme)
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Olist Dataset on Kaggle](https://www.kaggle.com/olistbr/brazilian-ecommerce)

**Your dataset is ready to use! Start analyzing!** 🚀
