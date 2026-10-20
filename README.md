# 🍔 QSR Waste & Staffing Optimization Project

> **A Data-Driven Operations & Project Management Case Study** designed to optimize inventory replenishment, eliminate food waste, and streamline staff scheduling for quick-service restaurants (QSR).

---

## 📋 Table of Contents
1. [Project Overview & Business Context](#-1-project-overview--business-context)
2. [Project Objectives & OKRs](#-2-project-objectives--okrs)
3. [Database Architecture & Schema](#-3-database-architecture--schema)
4. [Data Analytics & SQL Insights](#-4-data-analytics--sql-insights)
5. [Project Management & Implementation Plan](#-5-project-management--implementation-plan)
6. [Business Impact & Key Takeaways](#-6-business-impact--key-takeaways)

---

## 📌 1. Project Overview & Business Context

Operating a mid-sized quick-service restaurant (Apex QSR & Cafe) often leads to severe financial and operational inefficiencies if management decisions rely solely on intuition rather than empirical data. This project tackles two critical operational bottlenecks:

* **High Food Waste Rate (18–22%):** Perishable inventory (dairy, fresh produce, and bakery goods) is frequently over-ordered, resulting in heavy daily disposal losses.
* **Shift Mismatch & Overstaffing:** Inflexible shift structures lead to severe **Overstaffing** during slow mid-afternoon hours (Off-peak) while causing **Understaffing** and long customer wait times during lunch and dinner rushes (Peak Hours).

---

## 🎯 2. Project Objectives & OKRs

To demonstrate a structured management approach, the project follows clear **Objectives and Key Results (OKRs)**:
* **Objective:** Optimize restaurant operations and reduce revenue leakage using data analytics.
* **Key Result 1:** Reduce monthly food waste cost by at least **40%** (from ~$800 to under $450/month).
* **Key Result 2:** Eliminate redundant off-peak labor costs, saving **$500+/month** without compromising peak service quality.
* **Key Result 3:** Improve order fulfillment speed during peak hours by **25%**.

---

## 🗄️ 3. Database Architecture & Schema

The project utilizes a relational schema containing three core tables stored as CSV datasets (`fact_sales_transactions.csv`, `fact_inventory_waste.csv`, `dim_staff_schedules.csv`):

1. **`fact_sales_transactions`**: Stores hourly point-of-sale transactions, customer volume, and sales channels (Dine-in, Takeaway, Delivery).
2. **`fact_inventory_waste`**: Tracks daily ordered quantities, used quantities, disposal waste, and financial losses per item category.
3. **`dim_staff_schedules`**: Records employee roles, shift hours, hourly wage rates, and daily labor costs.

---

## 💻 4. Data Analytics & SQL Insights

Using **SQLite and Python**, we executed analytical queries to uncover deep operational insights.

### 🔍 Query 1: Peak Hours Analysis
Identified high-revenue rush hours to guide future staff allocation.

```sql
SELECT 
    hour,
    COUNT(transaction_id) AS total_orders,
    SUM(items_sold_qty) AS total_items_sold,
    SUM(total_sales_usd) AS hourly_revenue_usd
FROM fact_sales_transactions
GROUP BY hour
ORDER BY hourly_revenue_usd DESC;

​Analytical Finding: Peak rushes occur from 12:00–1:00 PM and 6:00–7:00 PM, generating roughly 65% of daily revenue. Conversely, the 2:00–3:00 PM window yields minimal returns.

​📦 Query 2: Food Waste Cost Analysis
​Pinpointed which inventory items drive the highest financial losses.
SELECT 
    category,
    item_name,
    SUM(ordered_qty) AS total_ordered,
    SUM(wasted_qty) AS total_wasted,
    ROUND((SUM(wasted_qty) * 100.0 / SUM(ordered_qty)), 2) AS waste_rate_pct,
    SUM(total_waste_cost_usd) AS total_cost_lost_usd
FROM fact_inventory_waste
GROUP BY category, item_name
ORDER BY total_cost_lost_usd DESC;

​Analytical Finding: Prepped Salad (Produce) recorded a staggering 40% waste rate, driving a projected monthly loss of nearly $800+.

​🚀 5. Project Management & Implementation Plan
​As the Project Manager, a structured 4-week rollout plan was designed to implement corrective actions:
