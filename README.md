# Bike Store: Revenue and Discount Analysis

Migration of a relational sales database from MySQL to SQLite, followed by analysis of revenue mix, store performance and discount behaviour across 1,615 orders and 4,722 order lines.

**[→ View the interactive dashboard](https://danielgalindo5.github.io/bike_store_sales_analysis/)**

---

## Key Metrics

- Total Revenue: **$7,689,116.56**
- Total Orders: **1,615**
- Total Customers: **1,445**
- Average Order Value: **$4,761.06**
- Total Products Sold: **7,078 units**

## Findings

**Discounting is flat and expensive.** Customers paid $7.69M against a list value of $8.58M a gap of **$889,872**, or 10.4% of list. Only four discount rates appear anywhere in the data (5%, 7%, 10%, 20%), and average discount depth barely moves across the catalogue: 10.41%–10.63% across four price tiers, 10.21%–11.13% across all seven categories. A $200 children's bike and a $6,000 road bike are treated the same. The 20% rate alone covers 25.5% of order lines but produces 47.2% of the total discount given.

**Revenue is concentrated in one store and one brand.** Baldwin Bikes (NY) is 67.8% of revenue, two sales reps there account for the store's entire $5.22M. Trek is 59.9% of revenue, with the next brand at 15.7%.

**Volume and value disagree by category.** Cruisers move the most units (2,063) but rank third in revenue. Road Bikes sell a quarter of that volume and out-earn them by $670K.

## Data coverage: read before using these numbers

The database advertises three calendar years. It contains roughly 28 usable months.

| Window | Orders | Revenue |
|---|---:|---:|
| Jan 2016 – Apr 2018 (dense) | 1,603 | $7,638,588 |
| May 2018 (no rows at all) | 0 | $0 |
| Jun – Dec 2018 (residual) | 12 | $50,529 |

April 2018 is the largest month in the file ($817,922) and sits immediately before the data thins out, so it reads as a boundary artifact rather than a demand spike. Additionally, 170 orders (10.5%) have no shipped date, and 14 of 321 catalogue products were never ordered.

**Rule applied throughout:** totals use all 1,615 orders, but any statement about growth, seasonality or year-over-year change is restricted to Jan 2016 – Apr 2018. A 2018 full-year figure would be wrong by roughly a factor of two.

## Visualizations

Included in the notebook and the interactive dashboard:

- Top 10 most sold products by quantity
- Top 10 products by total revenue contribution
- Top 5 best customers
- Sales percentage by category (units sold)
- Sales percentage by category (revenue)
- Monthly revenue trend
- Top products by revenue

## Repository

```
00_create_db.sql        Database creation
01_schema.sql           Table definitions and keys
02_seed_data.sql        Reference data
03_load_data.sql        Bulk load from CSV
04_queries.sql          Analysis queries
migrate.py              MySQL → SQLite migration script
bike_store.db           Resulting SQLite database
bike_store_queries.ipynb  Analysis notebook
index.html              Interactive dashboard (served via GitHub Pages)
*.csv                   Source tables (9)
```

## Method

`migrate.py` connects to a local MySQL instance, maps column types (`decimal`, `varchar`, `int`) to SQLite equivalents, automatically detects tables and primary keys, converts `Decimal` objects to floats, and copies all records. The result is a portable `.db` file that runs analysis without a server connection.

Revenue is computed per order line as `quantity × list_price × (1 - discount)` and rolled up with pandas. Analysis joins nine tables: `orders`, `order_items`, `products`, `brands`, `categories`, `stores`, `staffs`, `stocks`, `customers`.

## How to Run

**Migration (optional only needed to rebuild the database from source):**
- Ensure you have MySQL running with the `bike_store` database, loaded with `00_`–`03_`.
- Update your credentials in `migrate.py`.
- Run `python migrate.py`.

**Analysis:**

```bash
pip install pandas matplotlib jupyter
jupyter notebook bike_store_queries.ipynb
```

`bike_store.db` is committed, so the notebook runs without MySQL and connects to it automatically.

## Stack

SQL (MySQL, SQLite) · Python · pandas · Matplotlib · Jupyter · SVG/JavaScript

---

Built by **Daniel Galindo** — [LinkedIn](https://www.linkedin.com/in/danielgalindo5) · [GitHub](https://github.com/danielgalindo5)
