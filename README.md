# E-Commerce Sales Analysis — Target Brazil

SQL analysis of Target's Brazilian e-commerce data across 6 relational tables, 100K+ orders, and 15 queries ranging from basic aggregations to window functions. Sales grew 22.1% YoY — but the 6-month repeat purchase rate was effectively 0%. Basically no one came back for a second order.

> See `DATA_SOURCE.md` for dataset download instructions and table setup guide.

---

## Dataset

Kaggle: [e-Commerce (Target) Sales Dataset](https://www.kaggle.com/datasets/devarajv88/target-dataset)

| Table | What it contains |
|---|---|
| `customers` | Customer ID, city, state |
| `orders` | Order timestamps, status |
| `order_items` | Product-level line items, price, seller |
| `products` | Product category |
| `payments` | Payment value, method, installments |
| `sellers` | Seller ID, location — used for revenue ranking analysis |

---

## Key Findings

**1. Revenue is concentrated in 3 categories.**
Bed/Bath (10.7%), Health & Beauty (10.35%), and Computer Accessories (9.9%) together account for ~31% of total revenue. The bottom 5 categories combined don't match what Bed/Bath alone generates.

**2. São Paulo is the market.**
SP state has 41,746 customers — more than 3x the next largest state (RJ at 12,852). Any national campaign without SP-specific logic is ignoring its biggest lever.

**3. Half of all orders are paid in installments.**
49.4% of transactions use more than 1 installment. This matters for cash flow forecasting and how revenue should be recognized over time — not just a payment stat.

**4. Seller revenue follows a steep long-tail curve.**
The #1 seller generated 507,167 in revenue. By rank 10, that drops to 185,134. The top sellers likely account for a disproportionate share of total GMV.

**5. Growth is real. Retention is broken.**
YoY sales grew 22.1% from 2017 to 2018. But the 6-month repeat purchase retention rate is effectively 0% — almost no customer placed a second order within 6 months of their first. This is either a genuine retention problem or a dataset artifact where each transaction generates a unique customer ID. Worth investigating before drawing conclusions either way.

---

## SQL Concepts Used

| Concept | Where it appears |
|---|---|
| Multi-table JOINs (3 tables) | Queries 3, 8, 9 |
| Subqueries | Query 8 — revenue % share |
| CTEs (`WITH` clause) | Queries 7, 13, 14 |
| Window functions | Queries 10, 11, 12, 13, 15 |
| `DENSE_RANK()` with partitioning | Queries 10, 15 |
| `LAG()` for period-over-period calc | Query 13 |
| Moving average (sliding window frame) | Query 11 |
| `DATE_ADD` for retention logic | Query 14 |
| `CASE WHEN` aggregation | Query 4 |

---

## Project Structure

```
E-commerce-Target-Sales-Analysis/
├── E-commerce (Target) sales analysis.sql   # All 15 queries with inline comments
├── E-commerce Sales Analysis Presentation.pdf  # Slide deck with query outputs
├── csv_to_sql.ipynb                         # Python pipeline — CSV to MySQL import
├── DATA_SOURCE.md                           # Dataset download and setup guide
└── README.md
```

> Dataset CSVs (customers, orders, order_items, products, payments, sellers, geolocation) are not included in this repo. Download them from the Kaggle link in DATA_SOURCE.md.

---

## How to Run

```bash
# 1. Clone the repo
git clone https://github.com/Tisha34/E-commerce-Target-Sales-Analysis.git

# 2. Download the dataset from Kaggle (see DATA_SOURCE.md)

# 3. Set your MySQL password as environment variable
# Windows: setx MYSQL_PASSWORD "yourpassword"

# 4. Run csv_to_sql.ipynb to import CSVs into MySQL

# 5. Run queries from E-commerce_Sales_Analysis.sql in MySQL Workbench
```

Tested on MySQL 8.0 / MySQL Workbench

---

## What I'd Build Next

- Delivery time analysis using the timestamp gap between order placement and delivery
- Cohort retention table to replace the single retention rate figure with month-by-month customer behavior
- Installment usage broken down by category to see which product types drive credit dependency

---

*Dataset: Kaggle — Target Brazil e-commerce data. Used for portfolio and learning purposes only.*
