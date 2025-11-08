# Relax Data Science Challenge - Predicting User Adoption

This repository presents my complete solution for the **Relax Data Science Challenge**, a real-world product analytics and user retention problem.  
The goal is to identify **which factors predict future user adoption**, defined as users who log in on **three separate days within at least one seven-day period**.

---

## Problem Overview

Relax Inc. wants to understand which users are most likely to become **adopted users** - returning regularly to use the product.  
This insight allows data-driven growth, improved onboarding, and personalized retention strategies.

### Datasets
Two CSVs were provided:

| File | Description |
|------|--------------|
| **takehome_users.csv** | ~12,000 user records — includes account creation info, organization membership, and marketing preferences. |
| **takehome_user_engagement.csv** | Daily login logs — includes `user_id` and `time_stamp` for every product login event. |

---

## Approach & Methodology

### 1. Data Cleaning & Preparation
- Parsed timestamps and merged user data with engagement logs.  
- Removed nulls, standardized time zones, and ensured correct user matching.  
- Created a binary label `adopted_user` based on login frequency within rolling 7-day windows.

### 2. Exploratory Data Analysis (EDA)
- Investigated user signup sources, organization sizes, and adoption rates.  
- Found strong adoption patterns linked to organizational invites and marketing engagement.  
- Checked for class imbalance (≈14% adopted users).

### 3. Feature Engineering
- Created features:
  - `account_age_days`, `recency`, `org_size`, and `creation_source` one-hot encodings.
  - Marketing-related flags (`enabled_for_marketing_drip`, `opted_in_to_mailing_list`).
- Normalized continuous features and handled categorical encodings with pipelines.

### 4. Modeling
Models evaluated:
- **Logistic Regression** (baseline, interpretable)
- **Random Forest** (feature importance)
- **XGBoost** (best overall performance)

Evaluations used:
- Accuracy, Precision, Recall, F1-score, ROC-AUC
- Confusion matrix and ROC curve visualizations

---

## Results

| Metric | Value | Description |
|--------|--------|-------------|
| **Test Accuracy (XGBoost)** | **0.98** | Best-performing model accuracy |
| **Test Samples** | **≈2,400** | Held-out evaluation set |
| **ROC-AUC** | **0.94** | Excellent separation of classes |
| **Precision / Recall** | **0.90 / 0.88** | Strong predictive reliability |
| **Adoption Rate** | **~14%** (≈1,658 users) | Proportion of adopted users |

---

## Feature Importance (Random Forest)

| Rank | Feature | Importance | Insight |
|------|----------|-------------|----------|
| 1 | `last_session_creation_time` | ⭐⭐⭐⭐ | Recent activity predicts retention |
| 2 | `creation_source_ORG_INVITE` | ⭐⭐⭐ | Invited users adopt faster |
| 3 | `enabled_for_marketing_drip` | ⭐⭐ | Email drip campaigns boost engagement |
| 4 | `account_age_days` | ⭐⭐ | Older accounts sustain higher usage |
| 5 | `opted_in_to_mailing_list` | ⭐ | Minor but positive effect |

**Dead ends / notes**
- Email domains didn’t correlate strongly with adoption.  
- Total login count alone underperformed; time-based features were critical.  
- Significant chi-square relationship found between `creation_source` and adoption (p < 0.01).

---

## Key Insights

- **Teamwork drives adoption:** Users invited into organizations are 1.5–2× more likely to become adopted.  
- **Recent activity = retention:** Last login timestamp is the single most predictive signal.  
- **Marketing engagement matters:** Users on marketing drips have 5–10% higher adoption.  
- **Large organizations double adoption:** Teams >50 members retain users significantly better.  
- **Personal projects show lowest stickiness.**

---

## Recommendations

### Business Strategy
1. **Encourage organizational invites** — offer rewards or onboarding perks for inviting colleagues.  
2. **Target large organizations** — prioritize enterprise partnerships and workspace expansion.  
3. **Re-engage inactive users** — trigger campaigns based on inactivity or low recency.  
4. **Onboard early** — guide new users through milestone-based tutorials to form habits.

### Future Work
- Add **in-app behavior metrics** (feature usage, session duration).  
- Run **cohort and time-series analyses** by signup month.  
- Conduct **A/B tests** for invite incentives and onboarding flow variations.  
- Explore **SHAP explainability** and advanced models for interpretability.

---

