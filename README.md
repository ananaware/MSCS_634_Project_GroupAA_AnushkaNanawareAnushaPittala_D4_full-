# Customer Churn Analysis for a Telecom Company

**Course:** MSCS-634-M50, Advanced Big Data and Data Mining (Fall 2026)
**Group AA:** Anushka Nanaware and Anusha Pittala
**Assignment:** Residency Day 3, Project Deliverable 4: Final Insights, Recommendations, and Presentation

## Project Overview

This project asks one question: **which telecom customers are likely to leave, and what can the company do about it?** We used the same dataset for the whole course and applied four data mining techniques: regression, classification, clustering and association rule mining. This repository brings all the work together in one notebook, one written report and one presentation.

## Dataset

- **Name:** Telco Customer Churn (IBM sample dataset).
- **Source:** https://github.com/IBM/telco-customer-churn-on-icp4d
- **Size:** 7,043 customers and 21 columns
- **Target for classification:** `Churn` (Yes or No). 5,174 customers stayed and 1,869 left, so the churn rate is **26.5%**.
- **Why we chose it:** it is a real business problem, it fits all four techniques, it mixes numbers and categories, and it is public and has no names or addresses.

The notebook loads the data directly from the IBM link, so you do not need to download anything.

## Project Steps

| Part | What we did |
|---|---|
| **1. Data collection, cleaning and EDA** | Loaded the data, fixed 11 blank `TotalCharges` values, changed "No internet service" and "No phone service" to "No", changed `SeniorCitizen` from 0/1 to No/Yes, checked for duplicates and outliers, and explored churn patterns with charts. |
| **2. Feature engineering and regression** | Created five new features (`expected_total`, `num_addons`, `has_support`, `contract_months`, `tenure_band`). Predicted `TotalCharges` with Linear, Ridge and Lasso regression, using 5-fold cross-validation. |
| **3. Classification, clustering and association rules** | Predicted churn with a Decision Tree, KNN and a tuned Decision Tree. Grouped customers with K-Means. Found churn patterns with Apriori. |
| **4. Final insights, fairness and recommendations** | Combined all results, checked the model for fairness by gender and senior-citizen status, and wrote ethical considerations and recommendations. |

## Major Findings

**Data exploration**
- Month-to-month customers churn at **42.7%**, compared with 11.3% for one-year and 2.8% for two-year contracts.
- Fiber optic customers churn at 41.9% (DSL: 19.0%). Electronic check payers churn at 45.3%.
- Customers who left had a median tenure of 10 months, against 38 months for customers who stayed.
- Gender has no real effect on churn (26.9% against 26.2%).

**Regression (target: `TotalCharges`)**
- Baseline R² is 0.906. Adding four new features gives 0.909.
- Adding `expected_total` (tenure x monthly charges) gives **R² 0.999 and RMSE about 68** with Lasso.
- Caution: `expected_total` is almost the same as the target (correlation 0.9996), so the 0.909 result is the more honest sign of learning.

**Classification (target: `Churn`)**

| Model | Accuracy | F1 (churn) | ROC-AUC |
|---|---|---|---|
| Decision Tree | 0.7303 | 0.4974 | 0.6571 |
| KNN (k = 5) | 0.7587 | 0.5491 | 0.7865 |
| **Tuned Decision Tree** | 0.7459 | **0.6208** | **0.8162** |

The tuned Decision Tree (depth 3, balanced class weights) is our best model.

**Clustering (K-Means, k = 6)**
- Churn rates range from 1.6% to **58.3%**.
- The highest-risk group (cluster 2) is new customers with high monthly bills on month-to-month contracts.

**Association rules (Apriori)**
- Month-to-month contract plus fiber optic gives 54.6% churn (lift 2.06).
- Electronic check appears in four of the top five rules.

**Fairness check**
- Women and men are flagged at the same rate (about 40%).
- Seniors are flagged more often (63.1% against 36.2%) because they really do churn more (44.1% against 23.3%). We recommend using the flag only to offer extra help.

## Recommendations

1. Offer one- and two-year plans to month-to-month customers.
2. Give extra care in the first year (welcome call, check-ins at months 3 and 6).
3. Review fiber optic price and service quality.
4. Offer Online Security and Tech Support as a free trial.
5. Encourage automatic payment with a small bill credit.
6. Use the tuned Decision Tree to rank customers by risk, and use predictions only to help customers.

## Repository Contents

| File | Description |
|---|---|
| `MSCS_634_Project.ipynb` | Consolidated Jupyter notebook with all code, comments, charts and results (Parts 1 to 4) |
| `README.md` | This file |
| `MSCS_634_Final_Report.pdf` | Written report (PDF) |
| `MSCS_634_Final_Report.docx` | Written report (Word) |
| `MSCS_634_Presentation_Slides.pdf` | Presentation slides |
| Video | See the link below |

## Video Presentation

Watch our 5 to 7 minute video presentation here: **[ADD YOUR SCREENPAL LINK HERE]**

## How to Run the Notebook

1. Open `MSCS_634_Project.ipynb` in [Google Colab](https://colab.research.google.com/) (File, then Upload notebook).
2. Choose **Runtime, then Run all**.
3. The data loads from the IBM link automatically. Libraries used: pandas, numpy, matplotlib, seaborn, scikit-learn and mlxtend.

## Ethical Considerations

We removed the customer ID before modeling, checked the model's results by gender and senior-citizen status, used F1 score and ROC-AUC because the classes are imbalanced, and reported the regression results with and without the strongest engineered feature. Association rules and clusters show patterns, not causes, and predictions should support people, not replace them. See Section 6 of the report for details.

## References

- Agrawal, R., & Srikant, R. (1994). Fast algorithms for mining association rules. *Proceedings of the 20th International Conference on Very Large Data Bases*, 487-499.
- Ahmad, A. K., Jafar, A., & Aljoumaa, K. (2019). Customer churn prediction in telecom using machine learning in big data platform. *Journal of Big Data, 6*(1), Article 28.
- Mehrabi, N., et al. (2021). A survey on bias and fairness in machine learning. *ACM Computing Surveys, 54*(6), Article 115.
- Pedregosa, F., et al. (2011). Scikit-learn: Machine learning in Python. *Journal of Machine Learning Research, 12*, 2825-2830.
- Vafeiadis, T., et al. (2015). A comparison of machine learning techniques for customer churn prediction. *Simulation Modelling Practice and Theory, 55*, 1-9.

The full reference list is in the written report.
