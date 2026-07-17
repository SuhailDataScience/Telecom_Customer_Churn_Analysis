# Telecom Customer Churn Analysis
End-to-end churn analysis project combining SQL data cleaning, an interactive Power BI dashboard, and a machine learning model to predict which customers are likely to churn. Built to identify at-risk segments, quantify the revenue impact, and flag individual customers before they leave.

---

## Table of Contents
- [Business Problem](#business-problem)
- [Dataset](#dataset)
- [Tools & Tech Stack](#tools--tech-stack)
- [Project Workflow](#project-workflow)
- [Dashboard](#dashboard)
- [Predictive Model](#predictive-model)
- [Key Insights](#key-insights)

---

## Business Problem
Telecom companies lose recurring revenue every time a customer churns. This project answers three questions a retention/growth team would actually ask:
1. **Who is churning?** — which segments (contract type, payment method, internet type, tenure) show the highest churn rates
2. **How much revenue is at risk?** — quantified, not just a percentage
3. **Which individual customers are likely to churn next?** — so retention efforts can be targeted before they leave

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
| Predictive Modeling | Python (pandas, scikit-learn, XGBoost) |

---

## Project Workflow
1. **SQL Data Cleaning** — handled null and empty-string inconsistencies using `NULLIF`, standardized categorical fields
2. **Power BI Data Modeling** — built a star schema, wrote DAX measures for churn rate, revenue lost %, and segment-level breakdowns
3. **Dashboard Design** — two-page report (Overview + Drivers), warm beige and electric blue color theme with semantic color encoding (orange = high risk, blue = baseline)
4. **Predictive Modeling** — cleaned and engineered features, removed leakage columns, then trained and tuned XGBoost, Random Forest, and Decision Tree classifiers to predict individual churn risk, with nested cross-validation on all three models to check the results generalize

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

## Predictive Model
The dashboard above answers *what* is driving churn at a segment level. The model in [`churn_model.ipynb`](./churn_model.ipynb) goes a step further and predicts *which individual customers* are likely to churn, so retention efforts can be targeted rather than blanket.

**Approach**
- Removed post-outcome columns (`churn_reason`, `churn_category`) to avoid leaking the target into the features
- Engineered binary flags for refunds and extra data charges in place of their raw, heavily skewed values
- Trained and tuned three classifiers — XGBoost, Random Forest, and a Decision Tree baseline — optimizing for recall on the churned class, since missing an at-risk customer is costlier than a false alarm
- Checked the tuned results with nested cross-validation on all three models, so the reported scores aren't just an artifact of the hyperparameter search or a lucky train/test split

**Results**
| Model | Test Recall | Test Precision | Nested CV Recall |
|---|---|---|---|
| XGBoost | 0.81 | 0.64 | 0.832 ± 0.017 |
| Random Forest | 0.83 | 0.65 | 0.832 ± 0.017 |
| Decision Tree | 0.85 | 0.60 | 0.794 ± 0.040 |

On the single train/test split, the Decision Tree looks like the best performer on recall — but nested CV tells a different story. XGBoost and Random Forest come out essentially tied (0.832 ± 0.017 each), while the Decision Tree drops to 0.794 ± 0.040, roughly 4 points lower with more than double the fold-to-fold variance. Combined with its weaker precision (0.60 vs. 0.64–0.65), the Decision Tree's single-split recall looks like it was partly an artifact of that particular split rather than a real, stable advantage.

**Model choice:** XGBoost or Random Forest over the Decision Tree. The two are statistically indistinguishable from each other on nested CV, so choosing between them would come down to secondary factors like inference speed or interpretability rather than raw performance.

---

## Key Insights
- **Contract type is the strongest churn driver observed:** month-to-month customers churn at 46.53%, vs. 11.04% for one-year and 2.73% for two-year contracts — a 17x gap between month-to-month and two-year.
- **Fiber optic customers churn more than any other internet type** (41.10% vs. 25.72% cable, 19.37% DSL), despite typically being a premium/higher-revenue service — worth investigating whether this is a pricing or service-quality issue.
- **Mailed check users churn nearly 2.5x more than credit card users** (37.82% vs. 14.80%), suggesting payment friction or an older/lower-engagement customer segment.
- **Revenue at risk:** churned customers represent 17.52% of total revenue.
- Tenure and age group showed only modest variation in this dataset (~1–8 points spread) — weaker signal than contract type or payment method.

---
**Author:** [Suhail](https://github.com/SuhailDataScience)
