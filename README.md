# Enterprise BI, Forecasting & Predictive Campaign Optimization (SAS Viya)

An end-to-end Business Intelligence and Machine Learning portfolio project built in **SAS Viya (Visual Analytics & Visual Data Mining and Machine Learning)** with **Excel ROI modeling**. The repository covers three business cases:

1. **Global Sales BI, Forecasting & Operational Analytics** — Multi-page executive dashboards analyzing `$32M` in global product sales, regional performance, time-series forecasting, and facility efficiency.
2. **Non-Profit Donor Propensity Modeling (`PVA` Dataset, 106.5K records)** — Benchmarking 4 classification algorithms to target likely donors and maximize campaign return.
3. **Banking Cross-Sell Prediction & ROI Optimization (`VS_BANK` Dataset, 1.06M records)** — Benchmarking 6 machine learning models to predict new-product adoption (`tgt Binary New Product` / `B_TGT`) and identify the profit-maximizing targeting cutoff.

---

## Tech Stack

* **BI & Dashboards:** SAS Visual Analytics (bar charts, treemaps, donut charts, geo bubble maps, time-series forecasting, gauges, crosstabs, interactive filters)
* **Machine Learning:** SAS Visual Data Mining and Machine Learning (Decision Tree, Logistic Regression, Gradient Boosting, Neural Network, Support Vector Machine, Random Forest)
* **Business Valuation:** Microsoft Excel (Lift / Cumulative Lift tables and campaign ROI optimization curves)

---

## Part 1 — Global Sales BI, Forecasting & Operational Performance

**Report:** [`reports/01_global_sales_bi_forecasting.pdf`](reports/01_global_sales_bi_forecasting.pdf)

Built interactive dashboards designed for non-technical stakeholders to evaluate global revenue, European market concentration, monthly seasonality, and product-line operations:

* **Global Overview & Forecasting (1998–2006):** Total global product sales reached **$32M**, led heavily by **North America** followed by **Europe ($5.7M)**, **South America**, and **Asia**, paired with a time-series forecast model projecting 2006 sales with confidence bands.
* **European Regional Breakdown (`$5.7M` Total):**
  * **Spain ($1.6M / 27.2%)** and the **United Kingdom ($1.5M / 26.1%)** generate over half of European sales, followed by **Germany ($690K / 12.0%)**, **Sweden ($608K / 10.6%)**, **Italy ($446K / 7.8%)**, **Norway ($410K / 7.1%)**, **France ($296K / 5.2%)**, and **Denmark ($235K / 4.1%)**.
  * Compared four visual encodings (Bar Chart, Treemap, Donut Chart, Geo Map), demonstrating why horizontal bar charts provide the clearest direct country comparison while maps and donuts add spatial/share context.
* **Monthly Seasonality (France, 2004):** Combined line chart and crosstab pinpointed **January (3,174)** and **February (3,247)** as the lowest-performing months, with sales climbing to a **November peak (8,575)** (2004 subtotal: `65,461`).
* **Operational & Product Performance (Europe):** Monitored **Average Facility Efficiency (84%)** against target thresholds (`<70%` / `70–130%` / `>130%`) and compared Product Quality vs. Customer Satisfaction across 8 product lines (`Kiosk`, `Figurine`, `Store`, `Plush`, `Game`, `Gift`, `Bead`, `Promo`).

---

## Part 2 — Non-Profit Donor Propensity Modeling (`PVA` Dataset)

**Report:** [`reports/02_pva_donor_propensity_modeling.pdf`](reports/02_pva_donor_propensity_modeling.pdf) | **Assessment & ROI:** [`analysis/pva_gradient_boosting_roi.xlsx`](analysis/pva_gradient_boosting_roi.xlsx)

* **Objective:** Predict `Target Gift Flag` (binary donation indicator) on **106,546 records** (balanced 50% event rate: `53,273` donors / `53,273` non-donors) to optimize direct-mail fundraising for a national veterans' organization.
* **Model Comparison (Event = 1):**

| Model | Observations Used | KS (Youden) | Selected |
| :--- | :---: | :---: | :---: |
| **Gradient Boosting (50 trees)** | **106,546** | **0.3452** | **Yes** |
| Decision Tree | 106,546 | 0.1993 | — |
| Logistic Regression | 65K *(complete cases)* | 0.1863 | — |
| Neural Network | 65K *(complete cases)* | 0.1310 | — |

* **Champion Model Diagnostics (Gradient Boosting):**
  * **AUC (C-Statistic):** `0.7373` | **Gini:** `0.4745` | **Misclassification Rate:** `32.74%` (`0.50` cutoff) | **F1 Score:** `0.6643`
  * **Lift:** `1.84x` in the top 5% (`92.15%` event rate) and `1.73x` cumulative lift in the top 10%.
  * **Top Predictors:** `Demographic Cluster` (115.45), `Gift Count 36 Months` (42.36), `Age` (33.63), `Time Since Last Gift` (32.95), and `Gift Amount Last` (31.24).
  * **Campaign ROI Curve:** Under `100,000 × (Percentile/100) × (-2 + 3 × CumLift)`, return peaks at **80% of ranked donors ($114,726)** compared to `$100,000` when mailing 100% of the list.

---

## Part 3 — Retail Banking Cross-Sell Prediction & ROI Optimization (`VS_BANK` Dataset)

**Report:** [`reports/03_vs_bank_campaign_modeling.pdf`](reports/03_vs_bank_campaign_modeling.pdf) | **Assessment & ROI:** [`analysis/vs_bank_gradient_boosting_roi.xlsx`](analysis/vs_bank_gradient_boosting_roi.xlsx)

* **Objective:** Predict `tgt Binary New Product` (`B_TGT`) across **1,060,038 banking customers** (`19.95%` baseline purchase rate: `211,509` buyers / `848,529` non-buyers) to maximize net campaign profit.
* **Model Comparison (Event = 1, 1.06M Observations):**

| Model | KS (Youden) | True Positives | False Positives | Selected |
| :--- | :---: | :---: | :---: | :---: |
| **Gradient Boosting (50 trees)** | **0.6007** | **104,877** | **46,865** | **Yes** |
| Logistic Regression | 0.5855 | ~93K | ~41K | — |
| Support Vector Machine (SVM) | 0.5855 | ~86K | ~36K | — |
| Neural Network | 0.5754 | ~73K | ~31K | — |
| Decision Tree | 0.5088 | ~95K | ~49K | — |
| Random Forest (100 trees) | 0.5011 | ~73K | ~41K | — |

* **Champion Model Diagnostics (Gradient Boosting):**
  * **AUC (C-Statistic):** `0.8776` | **Gini:** `0.7553` | **Misclassification Rate:** `14.48%` (`0.50` cutoff; `94.48%` specificity / `49.59%` sensitivity)
  * **Optimal KS Cutoff (`0.16`):** Achieves `85.79%` sensitivity at `25.72%` false-positive rate (`KS = 0.6007`).
  * **Lift:** `4.17x` in the top 5% (`83.23%` conversion rate, capturing `20.86%` of all buyers) and `3.79x` cumulative lift in the top 10%.
  * **Top Predictors:** `logi_rfm5 Count Purchased Past 3 Years` (3,491.68), `demog_ri Home Value` (1,803.92), `logi_rfm9 Months Since Last Purchase` (1,461.92), `logi_rfm2 Average Sales Lifetime` (1,308.30), and `logi_rfm3 Avg Sales Past 3 Years Dir Promo Resp` (638.94).

### Campaign ROI Analysis (`VS_BANK` — 100,000 Targetable Customers)

Evaluating expected net profit with a **$2 contact cost**, **$5 margin per conversion**, and **25% reference conversion rate** (`100,000 × p × (-2 + 5 × CumLift × 0.25)`):

| Targeted Top % | Cumulative Lift | Expected Net Profit ($) | Decision Impact |
| :---: | :---: | :---: | :--- |
| 5% | 4.17x | +$16,070 | Highest conversion density (`83.2%`) |
| 15% | 3.42x | +$34,113 | Strong marginal return |
| **25% (Optimal)** | **2.83x** | **+$38,464** | **Maximum campaign profit** |
| 55% | 1.72x | +$7,932 | Last profitable decile |
| 60% | 1.59x | -$669 | Breakeven crossed |
| 100% (No Model) | 1.00x | -$75,000 | Mass mailing results in heavy loss |

> **Business Recommendation:** Deploy the **Gradient Boosting** propensity model to rank customers and restrict outreach to the **top 25% of predicted buyers**, turning an unsegmented **`-$75,000` loss** into a **`+$38,464` net profit** (a `+$113.5K` swing per 100K customers).

---
