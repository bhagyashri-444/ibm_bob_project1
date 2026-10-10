# E-Commerce Sales and Customer Behavior Analysis

**AICTE | IBM SkillsBuild — Data Analytics with AI Internship 2026 · Project Submission**

## Project Overview

This project analyses an e-commerce order dataset end-to-end with Python. It first **cleans** the raw data (documenting every cleaning decision), then performs **exploratory data analysis, KPI calculation, statistical testing and visualisation**, and finally turns the evidence into **business recommendations**. The work is delivered as a Jupyter Notebook, an interactive Streamlit dashboard and a project report.

## Problem Statement

An e-commerce company wants to understand its sales performance, customer purchasing behavior, product demand, regional revenue contribution and shipping outcomes. It has historical order data with customers, products, categories, prices, quantities, regions, order dates and shipping status.

The objective is to clean and analyse this data using Python, identify important sales and customer-behavior patterns, evaluate product and regional performance, analyse shipping and return behavior, and generate actionable business recommendations.

## Objectives

- Increase sales
- Identify high-performing products and categories
- Understand valuable customer segments
- Identify strong and weak regions
- Reduce product returns
- Improve shipping performance
- Improve overall customer experience

## Dataset

**Source (Kaggle):** https://www.kaggle.com/datasets/refiaozturk/e-commerce-sales

The uploaded file `realistic_e_commerce_sales_data.csv` is stored in this project as `data/e_commerce_sales.csv` (1,000 orders × 12 columns, 1 Jan 2023 – 3 Jan 2024). The original file is never modified; the notebook writes a separate cleaned file, `data/ecommerce_sales_cleaned.csv`.

## Dataset Description

| Column | Description |
|---|---|
| `Customer ID` | Customer identifier (292 unique customers) |
| `Gender` | Male / Female, as recorded on the order |
| `Region` | North, South, East, West (50 values missing) |
| `Age` | Customer age, 18–69 (100 values missing) |
| `Product Name` | One of 7 products: Laptop, Smartphone, Monitor, Smartwatch, Headphones, Keyboard, Mouse |
| `Category` | Electronics, Accessories, Wearables |
| `Unit Price` | Price per unit (20 rows contained incorrect values — corrected, see below) |
| `Quantity` | Units per order (1–5) |
| `Total Price` | Order value = unit price × quantity (shipping not included) |
| `Shipping Fee` | Shipping charge (5.00–19.98) |
| `Shipping Status` | Delivered / In Transit / Returned (50 values missing) |
| `Order Date` | Order date in day-month-year format |

Each row is treated as one order (the dataset has no order-ID column). **Revenue** = sum of `Total Price`.

### Data cleaning summary (done before any analysis)

| Problem found | How it was handled | Why |
|---|---|---|
| Column names with spaces/capitals | Converted to `snake_case` | Safer code |
| `Order Date` stored as text (`dd-mm-yyyy`) | Parsed with explicit format | Avoids day/month confusion |
| 20 rows with wrong `Unit Price` (≈ 2.07× the list price; unit × quantity ≠ total) | `unit_price = total_price ÷ quantity`, flag column kept | `Total Price` matches the single list price of every product in all rows |
| 100 missing `Age` | Filled from the same customer's other orders (age never changes per customer); 7 remain → "Unknown" group | Real data, no invented ages |
| 50 missing `Region` | Labelled "Unknown"; kept in totals, excluded from regional rankings | Cannot be inferred |
| 50 missing `Shipping Status` | Labelled "Unknown"; return rate uses orders with a recorded status | Avoids counting missing as "not returned" |
| Duplicates | None found (exact and near-duplicate checks) | — |
| Outliers in order value | Kept | Genuine high-priced Laptop/Smartphone orders |
| Same customer ID with different gender/region | Documented; gender and region analysed per order | Cannot be fixed without inventing data |
| Last month (Jan 2024) has only 3 days | Flagged; excluded from highest/lowest-month comparisons | Not comparable with full months |

## Technologies Used

- **Python 3** — primary language
- **pandas, NumPy** — data cleaning, transformation, aggregation
- **Matplotlib, Seaborn** — visualisation
- **SciPy** — Pearson/Spearman correlation and chi-square tests
- **Streamlit** — interactive dashboard
- **Jupyter Notebook** — analysis document

## Project Workflow

Data Collection → Data Cleaning → Data Transformation → EDA → Statistical Analysis → Visualization → Business Insights → Recommendations

## Key Business Questions

1. Which products and categories generate the highest sales?
2. Which regions contribute the most revenue?
3. Which products have the highest quantity sold?
4. How do sales change month by month?
5. Which age groups and genders contribute most to sales?
6. Which regions have the highest returned orders?
7. What percentage of orders are Delivered, In Transit, or Returned?
8. Which products have the highest return rate?
9. How much revenue is generated from each category and region?
10. Is there a relationship between product price and quantity purchased?
11. Which customers contribute the most to total sales?
12. What recommendations can improve sales and shipping performance?

## Installation / Setup

1. Install Python 3.9 or newer.
2. (Optional) create a virtual environment:
   ```bash
   python -m venv venv
   venv\Scripts\activate        # Windows
   source venv/bin/activate     # macOS / Linux
   ```
3. Install the dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Make sure the dataset is at `data/e_commerce_sales.csv`.

## Running the Jupyter Notebook

```bash
jupyter notebook
```
Open `notebooks/YourName_ECommerceSalesCustomerBehaviorAnalysis.ipynb` and choose **Kernel → Restart & Run All**. The notebook finds the dataset automatically (from the project root or the `notebooks/` folder), saves the cleaned dataset to `data/ecommerce_sales_cleaned.csv` and saves all charts to `figures/`.

## Running the Streamlit Dashboard

```bash
streamlit run app.py
```
The dashboard reads `data/ecommerce_sales_cleaned.csv` (if it does not exist yet, it cleans the raw CSV automatically using the same rules). Sidebar filters: region, category, product, gender, age group, shipping status and date range. Sections: Executive Overview, Sales Performance, Regional Performance, Customer Analysis, Shipping & Returns, Price & Quantity, and Business Insights. All numbers and insights are calculated from the filtered data — nothing is hard-coded.

## Project Structure

```
ECommerce_Sales_Customer_Behavior_Analysis/
│
├── data/
│   ├── e_commerce_sales.csv              # original dataset (unchanged)
│   └── ecommerce_sales_cleaned.csv       # cleaned data (created by the notebook)
│
├── notebooks/
│   └── YourName_ECommerceSalesCustomerBehaviorAnalysis.ipynb
│
├── figures/                              # charts saved by the notebook (used in the report)
│
├── app.py                                # Streamlit dashboard
├── requirements.txt
├── README.md
└── YourName_ProjectReport.docx
```

## Key Findings

All values below come from the cleaned dataset.

- **Revenue:** 1,346,600 from 1,000 orders and 3,008 units; average order value ≈ 1,347; 292 customers.
- **Products and categories:** Laptop (51.7% of revenue) and Smartphone (26.3%) generate 77.9% of revenue. **Electronics = 89.2%** of revenue. Accessories are 40.1% of orders but only 5.3% of revenue. Mouse is the weakest product (0.9%).
- **Quantity:** Monitor sold the most units (503), but ranks only 3rd by revenue (11.2%).
- **Regions:** East is strongest (329,950; 24.5%) and South weakest (301,040; 22.4%) — a gap of only 9.6%. 50 orders have no region.
- **Monthly trend:** Peak revenue in Nov 2023 (147,150); lowest in Mar 2023 (67,660). Sales are uneven, not steadily growing. Jan 2024 has only 3 days of data.
- **Customers:** Ages 46+ give 57.8% of revenue (56+ alone 35.8%); 18–25 gives 9.4%. Male orders 53.1%, female 46.9%.
- **Shipping status (orders with a recorded status):** Delivered 32.9%, In Transit 34.6%, Returned 32.4%.
- **Returns:** Returned orders hold 415,640 of revenue (30.9%). **West has the highest return rate (39.9%) and the most returns (95)**, versus 27.5% in South — statistically significant (chi-square p = 0.015). Product return rates (30.2% Monitor – 34.5% Headphones) are **not** significantly different (p = 0.99).
- **Price vs quantity:** Pearson r = +0.004 (p = 0.89) — no meaningful relationship.
- **Customer value:** top customer CUST0204 (29,100). The top 10% of customers generate 29.9% of revenue; 86% of customers are repeat buyers; 41 bought only once.
- **Shipping cost:** shipping fees equal 6.8% of Accessories order value versus 0.5% for Electronics.

## Business Recommendations

1. **Sales growth:** keep Laptop and Smartphone always in stock and promoted; run campaigns for the weakest months (Mar, Feb, Apr 2023); bundle Accessories with Electronics orders to lift order value.
2. **Product strategy:** price and quantity are unrelated, so test discounts on a small scale before relying on them; treat Mouse and Keyboard (2.6% of revenue) as add-ons/bundle items.
3. **Regional strategy:** use East as a benchmark; push higher-value Electronics in South (lowest order value, lowest return rate); fix fulfilment in West first.
4. **Customer retention:** launch a VIP programme for the top 10% of customers; send follow-up offers to the 41 one-time buyers; follow up personally when a high-value customer's order is returned.
5. **Marketing and segmentation:** prioritise customers aged 46+ with Electronics campaigns; build entry-level offers for the 18–25 group; gender-neutral campaigns are appropriate.
6. **Return reduction:** investigate carriers, packaging and delivery time in West; because returns are similar across products, fix processes (order accuracy, packaging, descriptions) rather than removing one product; record a return reason for every return.
7. **Shipping improvement:** track In-Transit order age with delivery-time targets; use bundles or a free-shipping threshold to reduce the shipping-cost burden on Accessories; make region and status mandatory at order entry; add delivery-date fields.
8. **Inventory planning:** prepare stock ahead of peak periods (Nov and Jan); plan Monitor/Laptop/Keyboard for volume and Laptop/Smartphone for value; collect more than one year of data before firm seasonal forecasts.

## Conclusion

Revenue comes mainly from a few high-priced Electronics products and is spread fairly evenly across regions, led by customers aged 46 and above. The most important weakness is fulfilment: roughly one in three recorded orders is returned and only one in three is confirmed as delivered, with the West region significantly worse than the rest. Because price and quantity are unrelated, growth should come from higher order value, customer retention, cross-selling and fixing returns rather than price cuts alone.

**Limitations:** one year of data, no order ID, delivery dates or return reasons, some missing values, and customer IDs with inconsistent gender/region.

## Author

**Name:** Bhagyashri Rajput
**Program:** AICTE | IBM SkillsBuild — Data Analytics with AI Internship 2026
**Institution:** RCPET's Institute Of Management Research And Development
