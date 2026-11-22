# Customer Shopping Behavior Analysis

An end-to-end data analysis project exploring customer shopping patterns, purchasing preferences, revenue, product performance, discounts, and subscription behavior. The project uses Python for data preparation, MySQL for querying, and Power BI for dashboard reporting.

## Project Overview

The project works with a customer shopping dataset containing **3,900 records and 18 original columns**. It follows a typical analytics workflow:

1. Load and inspect the raw CSV dataset with Pandas.
2. Handle missing review ratings and prepare the data for analysis.
3. Create additional fields that make customer behavior easier to analyze.
4. Load the prepared data into a MySQL database.
5. Run SQL queries to answer business questions.
6. Explore the results in an interactive Power BI dashboard.

## Dashboard Preview

The Power BI report includes KPI cards, charts, and slicers for exploring customer shopping behavior. The dashboard shown in this project includes:

- KPI cards for Customers, Revenue, Average Amount, and Average Rating.
- Customer distribution by subscription status.
- Customer counts by product category.
- Top 10 products by revenue.
- Revenue by category.
- Revenue by gender.
- Slicers for subscription status, gender, category, shipping type, and payment method.

To display the screenshot in this section, add the dashboard image to the repository (for example, save it as `assets/dashboard.png`) and update the path below if your filename is different:

![Customer Shopping Behavior Dashboard](assets/dashboard.png)

## Repository Files

The following names reflect the uploaded project files. If the filenames in your GitHub repository are different, use your repository's actual names in the commands below.

| File | Description |
|---|---|
| `customer_shopping_behavior.csv` | Original customer shopping dataset. |
| `Customer _Behavior_Analysis.ipynb` | Jupyter Notebook containing data inspection, cleaning, feature engineering, and MySQL loading code. |
| `customer_behavior_analysis.sql` | MySQL database setup and analytical SQL queries. |
| `customer_analysis_dashboard.pbix` | Power BI report file. |
| `assets/dashboard.png` | Optional dashboard screenshot for this README. Add this image to the repository if you want the preview above to render. |

## Tools and Technologies

| Tool / Library | Purpose |
|---|---|
| Python | Data preparation and analysis. |
| Pandas | Reading, inspecting, cleaning, and transforming the dataset. |
| Jupyter Notebook | Running the Python workflow interactively. |
| MySQL Server | Storing the prepared customer data. |
| MySQL Workbench | Optional graphical interface for running and checking SQL queries. |
| SQL | Aggregations, filtering, subqueries, CTEs, and window-function analysis. |
| SQLAlchemy | Connecting Python to MySQL and writing the DataFrame to a table. |
| PyMySQL | MySQL driver used by the PyMySQL SQLAlchemy connection in the notebook. |
| mysql-connector-python | Alternative MySQL driver used by another connection example in the notebook. |
| Power BI Desktop | Building and viewing the interactive dashboard. |

## Dataset Description

The raw CSV contains **3,900 rows and 18 columns**:

| Column | Description |
|---|---|
| `Customer ID` | Customer identifier. |
| `Age` | Customer age. |
| `Gender` | Customer gender. |
| `Item Purchased` | Product purchased. |
| `Category` | Product category. |
| `Purchase Amount (USD)` | Purchase amount in US dollars. |
| `Location` | Customer location. |
| `Size` | Purchased product size. |
| `Color` | Product color. |
| `Season` | Season associated with the purchase. |
| `Review Rating` | Customer review rating. |
| `Subscription Status` | Whether the customer has a subscription. |
| `Shipping Type` | Shipping method selected. |
| `Discount Applied` | Whether a discount was applied. |
| `Promo Code Used` | Whether a promotional code was used. |
| `Previous Purchases` | Number of previous purchases. |
| `Payment Method` | Payment method used. |
| `Frequency of Purchases` | Customer purchase frequency. |

There are **37 missing values** in `Review Rating` in the original CSV. The notebook fills these using the median review rating within the corresponding `Category`.

## Data Preparation in Python

The notebook performs the following steps:

1. Imports Pandas and reads the CSV into a DataFrame.
2. Inspects sample records, dataset dimensions, data types, descriptive statistics, and missing values.
3. Fills missing `Review Rating` values with the median rating of the relevant product category.
4. Converts column names to lowercase and replaces spaces with underscores.
5. Renames `purchase_amount_(usd)` to `purchase_amount`.
6. Creates an `age_group` column using quartile-based grouping with the labels `Young_Adult`, `Adult`, `Middle_aged`, and `Senior`.
7. Creates `purchase_frequency_days` by mapping the frequency categories to approximate day intervals.
8. Drops `promo_code_used` from the prepared DataFrame.
9. Loads the prepared DataFrame into the MySQL table named `customer`.

The frequency mapping used in the notebook is:

| Purchase frequency | Days |
|---|---:|
| `Weekly` | 7 |
| `Fortnightly` | 14 |
| `Bi-Weekly` | 14 |
| `Monthly` | 30 |
| `Quarterly` | 90 |
| `Every 3 Months` | 90 |
| `Annually` | 365 |

**Note:** The notebook's `age_group` feature uses quartiles (`pd.qcut`), while the age-group revenue query in the SQL file uses fixed age ranges (18–25, 26–35, 36–45, 46–55, and 56+). These are two separate grouping approaches in the project.

## SQL Analysis

The SQL file creates and selects the `customer_behavior` database, checks the `customer` table, and contains ten analysis queries:

1. **Revenue by gender** – compares total purchase revenue for male and female customers.
2. **Discount and spending analysis** – finds customers who used a discount and spent at least the overall average purchase amount.
3. **Product rating analysis** – ranks products by average review rating and returns the top five.
4. **Shipping analysis** – compares average purchase amounts for Standard and Express shipping.
5. **Subscription analysis** – compares customer count, average spending, and total revenue for subscribers and non-subscribers.
6. **Discount dependency** – calculates the percentage of purchases with discounts for each product and returns the top five.
7. **Customer segmentation** – groups customers as New, Returning, or Loyal based on previous purchases.
8. **Product popularity by category** – returns the top three purchased products in each category using `ROW_NUMBER()`.
9. **Repeat buyer analysis** – compares subscription status among customers with more than five previous purchases.
10. **Revenue by age group** – compares revenue across fixed age bands.

The customer segments in the SQL query are defined as:

| Segment | Previous purchases |
|---|---:|
| New | 0–1 |
| Returning | 2–10 |
| Loyal | More than 10 |

The SQL script uses common table expressions (CTEs) and a window function, so **MySQL 8.0 or later** is recommended.

## Installation and Requirements

### 1. Install the software

Install the following before running the full project:

- **Python 3** – for the notebook and data preparation.
- **Jupyter Notebook** – to open and run the `.ipynb` file.
- **MySQL Server 8.0 or later** – to create the database and store the customer table.
- **Power BI Desktop** – to open and interact with the `.pbix` report.
- **MySQL Workbench** – optional, but useful for running the SQL file and inspecting tables.

### 2. Install the Python libraries

Open a terminal in the project directory and run:

```bash
python -m pip install --upgrade pip
python -m pip install pandas sqlalchemy pymysql jupyter
```

The notebook also contains an alternative connection using MySQL Connector. Install its driver only if you use that connection block:

```bash
python -m pip install mysql-connector-python
```

`urllib.parse.quote_plus` is part of Python's standard library and does not need a separate installation.

## How to Run the Project

### Step 1: Clone the repository

Replace the repository URL below with the URL of your GitHub repository:

```bash
git clone <your-repository-url>
cd <your-repository-folder>
```

Alternatively, download the repository as a ZIP file and extract it.

### Step 2: Check the project files

Keep the CSV dataset in the same directory as the notebook, or update the CSV path in the notebook.

The notebook currently reads:

```python
df = pd.read_csv("customer_shopping_behavior.csv")
```

Make sure the filename in this line matches the dataset file in your repository.

### Step 3: Start Jupyter Notebook

From the project directory, run:

```bash
jupyter notebook
```

Open the customer behavior analysis notebook in the browser.

Run the notebook cells in order. The early cells inspect and prepare the data; the later cells connect to MySQL and load the DataFrame.

### Step 4: Set up MySQL

1. Start your MySQL Server.
2. Open MySQL Workbench or another MySQL client.
3. Create the database:

```sql
CREATE DATABASE customer_behavior;
```

4. Confirm that the connection details in the notebook match your local MySQL setup.

The notebook uses these connection settings as examples:

```python
username = "root"
password = "YOUR_MYSQL_PASSWORD"
host = "localhost"
port = "3306"
database = "customer_behavior"
```

Replace `YOUR_MYSQL_PASSWORD` with your own MySQL password. Do not commit real passwords or other credentials to GitHub.

### Step 5: Load the prepared data into MySQL

The notebook writes the prepared DataFrame to a table named `customer`.

The PyMySQL connection example is:

```python
from sqlalchemy import create_engine

username = "root"
password = "YOUR_MYSQL_PASSWORD"
host = "localhost"
port = "3306"
database = "customer_behavior"

engine = create_engine(
    f"mysql+pymysql://{username}:{password}@{host}:{port}/{database}"
)

df.to_sql(
    "customer",
    engine,
    if_exists="replace",
    index=False
)
```

This uses the `pymysql` package installed earlier. The `if_exists="replace"` option replaces the table if it already exists, so use it carefully if you have made changes to the database.

The notebook also includes a MySQL Connector connection example. Use one connection method at a time and make sure its matching driver is installed.

### Step 6: Run the SQL analysis

After the notebook has created and populated the `customer` table:

1. Open `customer_behavior_analysis.sql` in MySQL Workbench.
2. Connect to the same MySQL Server instance.
3. Run the script or execute the analysis queries individually.
4. Review the result sets for the ten analysis questions.

The SQL script selects the `customer_behavior` database and expects the `customer` table to exist. Run the notebook's data-loading step before running the analysis queries.

### Step 7: Open the Power BI dashboard

1. Open Power BI Desktop.
2. Open `customer_analysis_dashboard.pbix`.
3. If Power BI asks for data-source credentials or a file/database location, configure the source to match your local setup.
4. Refresh the report if the data source is connected and refresh is required.

The `.pbix` file is the Power BI report. The exact refresh configuration can depend on how the data source was configured when the report was created.

## Dashboard Features

The report provides an overview of customer behavior through the following visuals:

| Dashboard element | What it shows |
|---|---|
| Customers | Customer count KPI. |
| Revenue | Revenue KPI. |
| Avg Amount | Average purchase amount KPI. |
| Avg Rating | Average review rating KPI. |
| Customers Distribution by Subscribers Status | Subscriber and non-subscriber distribution. |
| Customers By Category | Customer distribution across product categories. |
| Top 10 Products by Revenue | Products ranked by revenue. |
| Revenue by Category | Revenue comparison across categories. |
| Revenue by Gender | Revenue split between male and female customers. |

The slicers shown in the report allow filtering by subscription status, gender, category, shipping type, and payment method.

## Important Notes

- The original CSV has 18 columns; the notebook modifies the DataFrame before loading it into MySQL.
- The notebook drops `promo_code_used` from the prepared DataFrame. The original CSV remains unchanged.
- The SQL script expects the prepared table to be named `customer` and the database to be named `customer_behavior`.
- The notebook includes both PyMySQL and MySQL Connector connection examples. They are alternatives, not steps that need to be run simultaneously.
- The notebook includes a `pyodbc` installation command, but the shown MySQL connection code uses SQLAlchemy with MySQL drivers. `pyodbc` is not required for the connection examples described above.
- The README describes the workflow and files provided with the project. It does not assume that the Power BI report can be refreshed on another computer without configuring its data source.

## Author

**Nikhil Rane**

- **GitHub:** [Nikrane033](https://github.com/Nikrane033)
- **LinkedIn:** [Nikhil Rane](https://www.linkedin.com/in/nikhil-rane-4496b5325/)
