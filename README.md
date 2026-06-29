# Winning Back The Plate: Predicting & Preventing Churn at PlatefulNZ

> **R-based machine learning project** comparing Logistic Regression, Neural Network, and LightGBM to predict customer churn for a NZ meal-kit subscription service — with actionable business recommendations.

---

## Business Problem

PlatefulNZ is a premium meal-kit subscription service in New Zealand. Rising customer attrition was threatening revenue and growth. This project identifies the key drivers of churn across 200,000 customer records and builds predictive models to flag at-risk customers before they cancel.

> Retaining an existing customer is 5–7x cheaper than acquiring a new one.

---

## Dataset

| Property | Detail |
|---|---|
| Records | 200,000 customers |
| Variables | 26 features |
| Target | `retained_binary` (Churned / Retained) |
| Class split | 76.8% Retained / 23.2% Churned |
| Source | BUSINFO 704 course dataset, University of Auckland |

**Key variables:** `satisfaction_survey`, `weeks_since_last_purchase`, `engagement_score`, `avg_AddOnpurchase_value`, `subscription_payment_problem_last4Weeks`

---

## Feature Engineering

Two custom features were engineered from raw variables:

**Social Media Score** — weighted composite of likes, comments, shares, and posts:
```
social_media_score = (1 × likes) + (2 × comments) + (3 × shares) + (4 × posts)
```

**Engagement Score** — composite behavioural metric:
```
engagement_score = (app_visits × 3) + (website_visits × 2) +
                   (opened_last_email × 5) + (social_media_score × 1.5) +
                   (num_purchases × 4)
```

Customers were then banded into Low / Medium / High / Very High engagement tiers — a key churn predictor.

---

## Methodology

- 80/20 train/test split with `set.seed(123)` for reproducibility
- Categorical variables dummy-encoded; numeric variables normalised
- Class imbalance handled via **upsampling**
- Three models compared using `tidymodels` framework:

| Model | Purpose |
|---|---|
| Logistic Regression (GLM) | Interpretable baseline |
| Neural Network (MLP/nnet) | Captures non-linear relationships |
| LightGBM (Gradient Boosting) | High-performance, imbalance-robust |

- Models evaluated on: Accuracy, Sensitivity, Specificity, AUC (ROC)

---

## Results

| Model | Accuracy | Sensitivity | Specificity | AUC |
|---|---|---|---|---|
| Logistic Regression | 88.3% | 59.5% | 96.3% | 0.906 |
| Neural Network | 90.8% | 69.8% | 96.6% | 0.942 |
| **LightGBM** | **91.7%** | **72.5%** | **97.0%** | **0.953** |

**LightGBM** was selected as the best model — highest accuracy, AUC, and sensitivity across all thresholds.

### Confusion Matrix (LightGBM)

| | Predicted Churned | Predicted Retained |
|---|---|---|
| **Actual Churned** | 6,174 ✅ | 919 ❌ |
| **Actual Retained** | 2,339 ❌ | 29,595 ✅ |

72.5% of actual churners correctly identified.

---

## Statistical Testing

All key predictors were validated before modelling:

| Variable | Test | Result |
|---|---|---|
| Engagement score | t-test + Levene | p < 0.001 ✅ |
| Satisfaction survey | Chi-square | p < 0.001 ✅ |
| Payment problems | Chi-square | p < 0.001 ✅ |
| Gender | Chi-square | p = 0.47 ❌ Not significant |
| Age | t-test | p < 0.001 ✅ |

---

## Key Findings

1. **Engagement is the #1 retention driver** — disengaged customers (low app/website visits, no email opens) were far more likely to churn
2. **Satisfaction drives loyalty** — low satisfaction scores strongly predicted cancellation across all models
3. **Payment friction accelerates churn** — billing failures increased churn even among otherwise engaged customers
4. **Demographics are secondary** — age showed minor effect; gender showed no significant effect

---

## Business Recommendations

| # | Recommendation | Rationale |
|---|---|---|
| 1 | **Boost digital engagement** — personalised recommendations, gamified challenges, loyalty rewards | Engagement is the strongest churn predictor |
| 2 | **Proactive payment support** — automated reminders, retry logic, flexible options | Payment failures are a key avoidable churn trigger |
| 3 | **Satisfaction recovery program** — real-time monitoring, targeted offers when scores drop | Dissatisfied customers cancel before being contacted |
| 4 | **Promote add-on purchases** — seasonal bundles, upsell campaigns | Add-on buyers show significantly higher retention |

---

## Tools & Libraries

![R](https://img.shields.io/badge/R-276DC3?style=flat&logo=r&logoColor=white)
![tidymodels](https://img.shields.io/badge/tidymodels-1a162d?style=flat)
![LightGBM](https://img.shields.io/badge/LightGBM-02569B?style=flat)

- **Language:** R
- **Framework:** `tidymodels`, `bonsai`, `themis`
- **Models:** `lightgbm`, `nnet`, GLM
- **Visualisation:** `ggplot2`, `GGally`, `corrplot`, `vip`
- **Reporting:** Quarto (`.qmd`) → PDF

---

## Repository Structure

```
.
├── ProjectRFinalBYSK.qmd          # Full R/Quarto source code
├── ProjectRFinalBYSK.pdf          # Rendered analysis report
├── group24poster.pdf              # Project poster (presented at UoA)
├── contribution_summary.pdf       # Contribution breakdown
└── README.md
```

---

## How To Run

1. Open `ProjectRFinalBYSK.qmd` in **RStudio**
2. Place `platefulnz_customers.csv` in the same directory
3. Click **"Render"** — all packages install automatically on first run
4. Output: full PDF report with all figures and model results

---

## Academic Context

- **Course:** BUSINFO 704 — Business Analytics, University of Auckland
- **Quarter:** Q3 2025
- **Role:** Primary analyst — data pipeline, feature engineering, all model development, statistical testing, and visualisation

---

*Project by Sai Krishna Sudarsan | Master of Business Analytics, University of Auckland*
