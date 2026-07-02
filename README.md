# Telecom Customer Churn Analysis

End-to-end churn analysis project combining SQL data cleaning, and an interactive Power BI dashboard. Built to identify which customer segments are most likely to churn and quantify the revenue impact.

---

## Table of Contents
- [Business Problem](#business-problem)
- [Dataset](#dataset)
- [Tools & Tech Stack](#tools--tech-stack)
- [Project Workflow](#project-workflow)
- [Dashboard](#dashboard)
- [Key Insights](#key-insights)

---

## Business Problem

Telecom companies lose recurring revenue every time a customer churns. This project answers three questions a retention/growth team would actually ask:

1. **Who is churning?** — which segments (contract type, payment method, internet type, tenure) show the highest churn rates
2. **How much revenue is at risk?** — quantified, not just a percentage
3. **Can we predict churn before it happens?** — using a classification model tuned for recall, since missing an at-risk customer is more costly than a false alarm

---

## Dataset
- **Size:** 6,418 customers
- **Fields:** demographics (age, gender, state), account info (tenure, contract type, payment method), service usage (internet type, add-ons), and churn label

---

## Tools & Tech Stack

| Stage | Tools |
|---|---|
| Data Cleaning | SQL Server (`NULLIF`, null/empty string handling) |
| Data Modeling & Visualization | Power BI (star schema, DAX measures) |

---

## Project Workflow

1. **SQL Data Cleaning** — handled null and empty-string inconsistencies using `NULLIF`, standardized categorical fields
2. **Power BI Data Modeling** — built a star schema, wrote DAX measures for churn rate, revenue lost %, and segment-level breakdowns
3. **Dashboard Design** — two-page report (Overview + Drivers), warm beige and electric blue color theme with semantic color encoding (orange = high risk, blue = baseline)

---

## Dashboard

**Page 1 — Churn Analysis (Overview)**
- Total customers, new joiners, churn count, churn rate KPIs
- Churn by tenure group, gender, age group, and contract type

**Page 2 — Churn Drivers**
- Avg monthly charge / tenure: stayed vs. churned
- Revenue lost %, no-add-ons %
- Churn rate by payment method, internet type, stated reason, and state (top 5)

<img width="683" height="383" alt="Screenshot 2026-07-02 155016" src="https://github.com/user-attachments/assets/a697d74f-a2ce-43d8-ace2-11a7311da3d0" />



<img width="678" height="374" alt="Screenshot 2026-07-02 155045" src="https://github.com/user-attachments/assets/48ece9ae-3f6e-4ba5-9e1c-54c9e318de04" />


---

## Key Insights

- **Contract type is the strongest churn driver observed:** month-to-month customers churn at 46.53%, vs. 11.04% for one-year and 2.73% for two-year contracts — a 17x gap between month-to-month and two-year.
- **Fiber optic customers churn more than any other internet type** (41.10% vs. 25.72% cable, 19.37% DSL), despite typically being a premium/higher-revenue service — worth investigating whether this is a pricing or service-quality issue.
- **Mailed check users churn nearly 2.5x more than credit card users** (37.82% vs. 14.80%), suggesting payment friction or an older/lower-engagement customer segment.
- **Revenue at risk:** churned customers represent 17.52% of total revenue.
- Tenure and age group showed only modest variation in this dataset (~1–8 points spread) — weaker signal than contract type or payment method. *(See Limitations.)*

---


**Author:** [Suhail](https://github.com/SuhailDataScience)
