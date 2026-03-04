# Enterprise Analytics Project - Complete Setup Guide

## ✅ What's Already Done

Your **enterprise-analytics-project** is now fully set up and accessible from anywhere!

### Repository Structure Created:
```
enterprise-analytics-project/
├─ dataset/           # Your data files
├─ sql/              # SQL scripts
│  └─ test_connection.sql
├─ python/           # Python scripts
│  └─ sql_connection.py
├─ powerbi/          # Power BI files
│  └─ enterprise_dashboard.pbix
├─ etl/              # ETL pipelines
│  └─ load_data_pipeline.py
├─ automation/       # Automation scripts
│  └─ run_daily_pipeline.py
├─ documentation/    # Project docs
│  ├─ project_architecture.md
│  └─ data_dictionary.md
├─ README.md
├─ requirements.txt
├─ .env
└─ .gitignore
```

---

## 🌐 Access Your Project From Anywhere

### Method 1: GitHub Codespaces (Recommended - Free for 60 hours/month)
1. Go to: https://github.com/deeepanbe/enterprise-analytics-project
2. Click the green **"Code"** button
3. Click **"Codespaces"** tab
4. Click **"Create codespace on main"** or select your existing codespace
5. VS Code opens in browser with full terminal access!

### Method 2: VS Code Online (vscode.dev)
1. Go to: https://vscode.dev
2. Click "Open Remote Repository"
3. Sign in to GitHub
4. Select: deeepanbe/enterprise-analytics-project

---

## 🔧 Configure Azure SQL Connection

### Step 1: Get Your Azure SQL Details
1. Go to https://portal.azure.com
2. Navigate to **SQL databases**
3. Click your database name
4. On the Overview page, find:
   - **Server name**: something like `yourserver.database.windows.net`
   - **Database name**: your database name
5. Your username and password are what you set during creation

### Step 2: Update .env File
In your Codespace, edit the `.env` file:
```
AZURE_SQL_SERVER=yourserver.database.windows.net
AZURE_SQL_DATABASE=yourdatabase
AZURE_SQL_USERNAME=yourusername
AZURE_SQL_PASSWORD=yourpassword
```

### Step 3: Test Connection
Run in terminal:
```bash
python python/sql_connection.py
```

You should see: "Connection successful!"

---

## 📦 Install Python Packages

In your Codespace terminal:
```bash
pip install -r requirements.txt
```

Packages included:
- pandas - Data analysis
- numpy - Numerical computing
- matplotlib - Data visualization
- seaborn - Statistical plots
- sqlalchemy - Database toolkit
- pyodbc - Azure SQL connection
- python-dotenv - Environment variables

---

## 🎯 Next Steps for Practice

### 1. SQL Practice
- Edit `sql/test_connection.sql`
- Write queries to analyze your data
- Run queries directly in VS Code with SQL extension

### 2. Python Practice
- Use `python/sql_connection.py` as base
- Create new scripts for data analysis
- Import pandas, connect to Azure SQL, analyze data

### 3. Power BI Practice
- Download Power BI Desktop on your computer
- Open `powerbi/enterprise_dashboard.pbix`
- Connect to your Azure SQL database
- Create visualizations

### 4. ETL Practice
- Edit `etl/load_data_pipeline.py`
- Build data transformation pipelines
- Automate data loading processes

---

## 💡 Pro Tips

1. **Your work auto-saves to GitHub**
   - Every time you commit in Codespace, it syncs to GitHub
   - Access from any computer by opening the Codespace again

2. **Use ChatGPT for help**
   - Ask: "How do I write a SQL query to..."
   - Ask: "Help me create a Python script that..."
   - Ask: "Explain this error message..."

3. **Practice daily**
   - SQL: 30 minutes daily writing queries
   - Python: 30 minutes daily analyzing data
   - Power BI: 1 hour creating dashboards

4. **Build your portfolio**
   - Document your work in markdown files
   - Share your GitHub repo link on resume
   - Showcase dashboards in interviews

---

## 🆘 Troubleshooting

### Can't connect to Azure SQL?
- Check firewall rules in Azure Portal
- Add your IP address to allowed IPs
- Verify credentials in .env file

### Codespace running slow?
- GitHub gives 2-core machines for free
- Upgrade to 4-core if needed (uses more hours)

### Need more practice datasets?
- Upload CSV files to `dataset/` folder
- Import to Azure SQL using Python scripts
- Practice with real-world data

---

## 📚 Learning Resources

### SQL
- W3Schools SQL Tutorial
- HackerRank SQL Practice
- LeetCode Database Problems

### Python
- Real Python tutorials
- Kaggle Learn Python course
- DataCamp Python for Data Science

### Power BI
- Microsoft Power BI Learning Path
- Guy in a Cube YouTube channel
- Power BI Community forums

### Azure
- Microsoft Learn Azure Fundamentals
- Azure SQL Database documentation
- Free Azure credits for students

---

## 🎓 Interview Preparation Tips

1. **Show this project in interviews**
   - Share GitHub link: https://github.com/deeepanbe/enterprise-analytics-project
   - Explain your development workflow
   - Demo your dashboards and analysis

2. **Be ready to explain**
   - Why you chose this tech stack
   - How you use version control (Git/GitHub)
   - Your approach to data analysis problems

3. **Have examples ready**
   - Complex SQL queries you've written
   - Python scripts you've created
   - Insights you've discovered in data

---

## ✨ Your Setup is Complete!

You now have:
- ✅ Professional project structure
- ✅ Cloud-based development environment
- ✅ Access from any computer, anywhere
- ✅ All tools ready for practice
- ✅ Portfolio-ready GitHub repository

**Start practicing today and build your skills!**

Good luck with your interview preparation! 🚀
