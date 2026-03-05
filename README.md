# **Credit Card Fraud Detection**

I was contracted as a Data Analyst for NovaPay, a UK-based fintech startup that processes card payments for small and medium-sized e-commerce merchants. NovaPay has experienced a sharp rise in fraudulent transactions, resulting in significant financial losses and a growing loss of trust among their merchant partners.

Their existing rule-based system flags transactions based on static thresholds and misses a large proportion of fraud — particularly low-value card-testing transactions and online purchases — while generating too many false positives that disrupt legitimate customer payments.

My job is to use data analysis and machine learning to help NovaPay understand how fraud is occurring on their platform, build a predictive model that can detect it more accurately, and communicate findings through an interactive Power BI dashboard accessible to both their fraud operations team and senior leadership.
The analysis includes:

* Exploratory Data Analysis (EDA) to understand transaction patterns and identify fraud signals
* Hypothesis testing to validate key assumptions about fraudulent behaviour
* Predictive modelling to classify transactions as fraudulent or legitimate
* Interactive dashboards in Power BI to communicate insights to both technical and non-technical stakeholders

# ![CI logo](https://codeinstitute.s3.amazonaws.com/fullstack/ci_logo_small.png)


---

**Power BI Interactive Dashboard**

View the interactive dashboard online: [Fraud Detection Dashboard](https://app.powerbi.com/view?r=eyJrIjoiZTRkMjEzOTYtM2Y4Zi00MGJlLWE5MjUtY2JiNjQyYTExMzA2IiwidCI6ImMyMzNjMDcyLTEzNWItNDMxZC1hZjU5LTM1ZTA1YmFiZjk0MSIsImMiOjh9)

---

## Dataset Content

The dataset used in this project is the [Credit Card Transactions Fraud Detection Dataset](https://www.kaggle.com/datasets/kartik2112/fraud-detection) by kartik2112, sourced from Kaggle.

https://www.kaggle.com/datasets/kartik2112/fraud-detection

Dataset overview:

- **Size:** 1.85 million simulated transactions (training file: `fraudTrain.csv`, test file: `fraudTest.csv`)
- **Columns:** 22 features including transaction datetime, merchant category, transaction amount, cardholder location, merchant location, and fraud label
- **Objective:** Identify the transaction patterns that distinguish fraudulent from legitimate activity and build a classifier to detect fraud with high recall
- **Source:** Synthetically generated using the Sparkov Data Generation tool, simulating US cardholder transactions between January 2019 and December 2020
- **Licence:** CC0 Public Domain — no restrictions on use for educational or analytical purposes

The dataset was chosen specifically because it contains **interpretable, real-world column names** — merchant category, transaction hour, geographic location — which allow for meaningful visualisations and business-facing insights accessible to non-technical stakeholders.

**Columns dropped before analysis (PII and low-signal):** `first`, `last`, `street`, `cc_num`, `trans_num`, `unix_time`, `merchant`, `job`

**Engineered features created during preprocessing:**

| Feature | How it was created |
|---|---|
| `trans_hour` | Extracted from `trans_date_trans_time` |
| `trans_dayofweek` | Extracted from `trans_date_trans_time` |
| `trans_month` | Extracted from `trans_date_trans_time` |
| `age` | Calculated from `dob` and transaction date |
| `home_merch_dist` | Haversine distance between cardholder home (`lat/long`) and merchant (`merch_lat/merch_long`) |
| `log_amt` | Log-transformed `amt` to reduce skew before modelling |

---

## Business Requirements

I am contracted as a Data Analyst for NovaPay and my job is to utilise this project to address the following business requirements:

1. Identify which transaction characteristics and patterns are most strongly associated with fraud, so NovaPay's fraud team can understand how fraud is occurring on their platform.
2. Analyse the relationship between transaction timing, merchant category, transaction amount, and geographic distance to fraud likelihood.
3. Build a predictive model that classifies transactions as fraudulent or legitimate with a high fraud recall rate — minimising missed fraud while keeping false positives at an acceptable level.
4. Provide an interactive Power BI dashboard that allows both the fraud operations team and senior leadership to explore fraud patterns, monitor model performance, and extract actionable insights.

---

## Hypothesis and How to Validate

**H1: Time of Day and Fraud**

Research Hypothesis (H1): Fraudulent transactions are significantly more likely to occur during late-night hours (midnight to 4am) than during normal business hours, reflecting fraudsters exploiting periods of reduced monitoring.

Null Hypothesis (H0): There is no statistically significant difference in fraud rate between late-night hours and other times of day.

Rationale: Fraud often occurs when cardholders are less likely to notice unusual activity. If late-night transactions carry a disproportionate fraud rate relative to their transaction volume, time of day becomes a valuable and immediately actionable detection signal for NovaPay — even without a machine learning model.

What I discovered: `[Update with your finding once EDA is complete]`

---

**H2: Merchant Category and Fraud**

Research Hypothesis (H2): Fraud is not evenly distributed across merchant categories — online shopping and miscellaneous online categories will show disproportionately higher fraud rates than in-person categories.

Null Hypothesis (H0): There is no statistically significant association between merchant category and fraud likelihood.

Rationale: Card-not-present transactions (online purchases) carry a higher inherent fraud risk than card-present transactions because they do not require physical possession of the card. Identifying high-risk categories allows NovaPay to apply targeted friction — such as additional verification for online transactions above a certain threshold — as a practical, low-cost risk control.

What I discovered: `[Update with your finding once EDA is complete]`

---

**H3: Transaction Amount and Geographic Distance**

Research Hypothesis (H3): Fraudulent transactions will show a significantly higher average transaction amount and a greater distance between the cardholder's home address and the merchant location compared to legitimate transactions.

Null Hypothesis (H0): There is no statistically significant difference in transaction amount or home-to-merchant distance between fraudulent and legitimate transactions.

Rationale: Large transactions occurring far from the cardholder's home are classic fraud indicators. A fraudster using a stolen card is unlikely to be near the cardholder's address and may attempt high-value purchases before the card is cancelled. The home-to-merchant distance feature was specifically engineered for this project using the haversine formula on cardholder and merchant coordinates.

What I discovered: `[Update with your finding once EDA is complete]`

---

## Project Plan

### High-Level Steps

**Data Collection:**
Dataset downloaded from Kaggle using the Kaggle API. Both `fraudTrain.csv` and `fraudTest.csv` stored locally in `data/raw/` and excluded from version control via `.gitignore`.

**ETL Pipeline:**

*Extract:*
- Load `fraudTrain.csv` using Pandas
- Inspect shape, data types, null values, and class distribution

*Transform:*
- Drop PII columns immediately — `first`, `last`, `street`, `cc_num`, `trans_num` — documented as the first ethical data handling step before any analysis is performed
- Drop low-signal columns: `merchant`, `job`, `unix_time`
- Parse datetime and engineer temporal features: `trans_hour`, `trans_dayofweek`, `trans_month`
- Engineer `age` from date of birth
- Engineer `home_merch_dist` using the haversine formula on cardholder and merchant coordinate pairs
- Apply log transformation to `amt` to reduce right skew
- Encode `category` for modelling
- Exclude `gender` and `age` from model features — retained for EDA visualisation only (see Ethical Considerations)
- Export cleaned dataset to `data/processed/`

*Load:*
- Cleaned data exported as CSV for analysis and Power BI connection

**Exploratory Data Analysis (EDA):**
Descriptive statistics, class imbalance analysis, fraud rate by transaction hour, fraud rate by merchant category, transaction amount and distance distributions, correlation heatmap.

**Hypothesis Testing:**
Chi-squared tests (H1, H2) and Mann-Whitney U tests (H3) to statistically validate all three hypotheses before modelling.

**Machine Learning:**
Predict fraud with Logistic Regression (baseline), and XGBoost (final model). Evaluate using precision, recall, F1-score, and ROC-AUC. Tune classification threshold via Precision-Recall curve to optimise for recall — minimising missed fraud.

**Interactive Dashboard Development:**
Visualisation of fraud patterns, model performance, and business recommendations in Power BI across two pages — an executive summary for leadership and a detailed analysis page for the fraud operations team.

**Interpretation & Recommendations:**
Business insights with narrative storytelling tied back to each business requirement.

### Research Methodology Justification

EDA helps surface fraud patterns in the data before modelling — statistical hypothesis testing prevents over-reliance on visual patterns alone and provides objective validation. Classification algorithms are appropriate for this binary target (fraud vs legitimate). The haversine distance feature was specifically engineered because geographic displacement is a documented real-world fraud signal not available as a raw column. Interactive dashboards allow non-technical stakeholders to explore findings without needing to interpret code or raw model metrics. The classification threshold was tuned deliberately rather than left at the default 0.5 — because in a heavily imbalanced dataset, threshold selection is a business decision about acceptable levels of missed fraud and false positives, not just a technical optimisation.

---

## The Rationale to Map the Business Requirements to the Data Visualisations

To effectively address the business requirements of this project, each visualisation was carefully chosen to provide clear, interpretable insights into transaction fraud patterns. The dashboard uses **at least four distinct chart types**, each selected to answer a specific business question.

To identify which transaction patterns are associated with fraud (Business Requirement 1), a **horizontal bar chart** of fraud rate by merchant category was used, sorted from highest to lowest risk. This allows the fraud team to immediately see which merchant types carry the highest risk without needing to interpret raw numbers. A **correlation heatmap** was used to show relationships between all engineered numerical features and the fraud label, helping identify which variables carry the strongest predictive signal.

To analyse the relationship between timing, category, amount, and distance to fraud (Business Requirement 2), a **dual-axis chart** was used to plot transaction volume and fraud rate by hour of day on the same chart — making the overnight fraud spike visually unmissable. **Box plots** were used to compare transaction amount and home-to-merchant distance distributions between fraud and legitimate classes, validating Hypothesis 3 in a format readable by both technical and non-technical audiences.

To communicate model performance (Business Requirement 3), a **horizontal bar chart** of feature importance shows what the XGBoost model learned and makes the model interpretable rather than a black box. A precision-recall curve illustrates the threshold trade-off, allowing NovaPay to make an informed decision about where to set the detection threshold.

To support the interactive dashboard for two audiences (Business Requirement 4), a **choropleth map** of fraud rate by US state provides an immediately engaging geographic overview for senior leadership. Slicers and drill-through filtering on both pages allow the operations team to explore specific categories, time windows, and transaction amounts dynamically.

The four distinct chart types used across the dashboard are: **bar chart** (class imbalance, fraud by category, feature importance), **dual-axis chart** (fraud rate by hour), **box plot** (amount and distance by class), and **heatmap** (feature correlation matrix). The choropleth map and precision-recall curve are additional chart types further strengthening the analytical depth.

---

## Analysis Techniques Used

**Descriptive Statistics:**
- Mean, median, percentiles for transaction amount, age, and distance
- Fraud rate per group (hour, merchant category, US state)
- Class balance assessment

**Visualisations:**
- Dual-axis bar and line chart (fraud rate vs transaction volume by hour)
- Horizontal bar chart (fraud rate by merchant category, XGBoost feature importance)
- Box plots (transaction amount and home-merchant distance by class)
- Correlation heatmap (engineered feature relationships)
- Choropleth map (fraud rate by US state)
- Precision-recall curve (model threshold trade-off)

**Statistical Tests:**
- Chi-squared tests to assess the association between categorical variables (merchant category, transaction hour bracket) and fraud label — H1 and H2
- Mann-Whitney U tests to assess differences in continuous variables (amount, distance) between fraud and legitimate groups — H3

**Machine Learning Models:**
- Logistic Regression (interpretable baseline)
- XGBoost (final model — strong performance on imbalanced tabular data with built-in feature importance)
- Evaluation metrics: Precision, Recall, F1-score, ROC-AUC
- SMOTE applied to training set only to address class imbalance (~0.58% fraud rate)
- Classification threshold tuned via Precision-Recall curve

**Feature Engineering:**
- Haversine distance calculated from raw latitude/longitude coordinate pairs — engineered specifically for this project as a geographically meaningful fraud signal
- Datetime decomposition into hour, day of week, and month
- Log transformation of transaction amount to reduce right skew

**Generative AI Usage:**
Claude (Anthropic) was used to assist with code templates, hypothesis framing, feature engineering approaches, and README structure. All AI-assisted code was reviewed, tested, and understood before use. AI was not used to generate analysis conclusions — all findings are based on actual data outputs.

**Limitations and Alternative Approaches:**
- The dataset is synthetically generated, meaning patterns may not fully reflect real-world fraud distributions. This is disclosed transparently throughout the project.
- Gender and age were intentionally excluded from modelling due to fairness concerns — an alternative approach would be to include them and perform post-hoc bias auditing, but exclusion was the more conservative and defensible choice given the regulatory context.
- SMOTE was applied to address class imbalance. An alternative was using `scale_pos_weight` in XGBoost directly, but SMOTE produced better recall on the validation set.
- The dataset lacks per-customer transaction history, preventing velocity features (e.g. number of transactions in the last hour per card) which are highly valuable in real fraud detection systems.

---

## Ethical Considerations

Ethics was treated as a first-order concern throughout this project, not an afterthought. The following decisions were made deliberately and are documented here for full transparency.

**Choice of Synthetic Data:**
I deliberately selected a synthetically generated dataset for this project. Working with real cardholder transaction data would introduce significant data privacy obligations outside the scope of an educational project. The synthetic dataset also provides interpretable column names — merchant category, transaction hour, geographic location — enabling clearer visualisations and more meaningful business communication. The use of synthetic data is explicitly disclosed in this README and within the dashboard itself.

**PII Removal:**
Although the dataset is synthetic, it contains simulated personal fields including cardholder names (`first`, `last`), street address (`street`), and card number (`cc_num`). These columns were dropped as the **very first step** in the data cleaning notebook — before any analysis, visualisation, or modelling was performed. This reflects the principle that PII, even synthetic, should be handled with the same discipline as real personal data and establishes responsible practice for working with real datasets in the future.

**Bias and Fairness — Gender and Age Exclusion:**
The dataset contains `gender` and `dob` (from which age is derived). Both were used for EDA visualisation only and were **explicitly excluded from the model feature set**. Using demographic attributes such as gender or age as fraud predictors risks creating a discriminatory model — one that flags transactions based on who the cardholder is rather than what the transaction looks like. This would likely conflict with the FCA's expectations on algorithmic fairness in financial services and could constitute indirect discrimination under the Equality Act 2010. This decision is documented with a comment in the feature engineering notebook.

**Transparency in Model Trade-offs:**
The model's precision-recall trade-off is explicitly documented and visualised. Raising recall (catching more fraud) comes at the cost of more false positives — legitimate customer transactions being declined. This has a real human impact and disproportionately harms certain groups if the model has learned demographic patterns from training data. NovaPay would need to conduct further bias testing before any real-world deployment.

**Responsible Use Disclaimer:**
This project is built for educational and portfolio purposes only. The model is not intended for deployment in a live fraud detection system without further validation, independent bias auditing, and appropriate regulatory review under FCA guidelines.

---

## Dashboard Design

### Planned Dashboard Pages

**Page 1 — Executive Summary (Non-technical audience: NovaPay senior leadership)**

- KPI cards: total transactions analysed, total fraud detected, overall fraud rate (%), estimated financial exposure
- Fraud rate by merchant category — horizontal bar chart, colour-scaled by risk level
- Fraud rate and volume by hour of day — dual-axis chart
- Choropleth map of fraud rate by US state
- Plain-English narrative text boxes summarising key findings and recommendations
- Slicers: merchant category, transaction amount range
- No model metrics or technical terminology on this page

**Page 2 — Fraud Analysis (Technical audience: fraud operations team)**

- Feature importance horizontal bar chart (top 10 XGBoost features — all labelled in plain English)
- Transaction amount by class — box plot with median annotations
- Home-to-merchant distance by class — box plot
- Correlation heatmap of engineered features
- Confusion matrix with estimated cost-per-missed-fraud annotation
- Precision-recall curve with selected threshold highlighted
- Drill-through transaction table: filterable by predicted class, merchant category, transaction hour, and amount

**How data insights were communicated to different audiences:**
Technical metrics (precision, recall, F1-score, ROC-AUC) appear only on Page 2 with annotations explaining what each means in business terms. Page 1 translates these into plain-language summaries. All chart titles are framed as business questions rather than technical labels — for example, "Which merchant types carry the highest fraud risk?" rather than "Fraud rate by category."

---

## Unfixed Bugs

`[Document any bugs here and why they were not resolved. If none, state: No unfixed bugs were identified during development.]`

---

## Development Roadmap

`[Document challenges faced and strategies used to overcome them as you work through the project.]`

**Skills and tools to explore next based on this project:**
- SHAP values for per-transaction model explainability
- Real-time scoring pipelines — serving the model as an API endpoint
- Power BI dataflows for live data source connections

---

## Deployment

### Power BI

The dashboard was built in Power BI Desktop and published to Power BI Service.

`[Add your published dashboard link here once deployed]`

**To view the dashboard locally:**

1. Ensure [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) is installed (free, Windows only)
2. Clone this repository: `git clone https://github.com/[YOUR_USERNAME]/fraud-detection-project`
3. Open `dashboard/fraud_detection_dashboard.pbix` in Power BI Desktop
4. When prompted, update the data source path to point to `data/processed/transactions_processed.csv` on your local machine

**To reproduce the full analysis pipeline:**

1. Install dependencies: `pip install -r requirements.txt`
2. Set up Kaggle API credentials — place `kaggle.json` in `~/.kaggle/` (never commit this file)
3. Run the data download:
```bash
kaggle datasets download -d kartik2112/fraud-detection -p data/raw --unzip
```
4. Run notebooks in order from `notebooks/v1/`

---

## Main Data Analysis Libraries

| Library | Purpose | Example Usage |
|---|---|---|
| Pandas | Data cleaning, transformation, groupby analysis | `df = pd.read_csv('fraudTrain.csv')` |
| NumPy | Numerical operations, haversine distance calculation | `np.radians(df['lat'])` |
| Matplotlib | Static plotting for notebook EDA | `plt.hist(df['trans_hour'])` |
| Seaborn | Advanced statistical plots | `sns.boxplot(x='is_fraud', y='amt', data=df)` |
| Plotly | Interactive charts prototyped in notebooks | `px.bar(cat_fraud, x='fraud_rate', y='category', orientation='h')` |
| Scikit-learn | ML models, evaluation metrics, train/test split | `LogisticRegression().fit(X_train, y_train)` |
| XGBoost | Final fraud classification model | `XGBClassifier(scale_pos_weight=spw).fit(X_train, y_train)` |
| SciPy | Statistical hypothesis testing | `chi2_contingency(pd.crosstab(df['category'], df['is_fraud']))` |
| Imbalanced-learn | SMOTE oversampling | `SMOTE().fit_resample(X_train, y_train)` |
| Power BI | Interactive dashboard, slicers, KPIs, maps | Choropleth map, dual-axis chart, drill-through table |

---

## Credits

### Content

- Credit Card Transactions Fraud Detection Dataset: [kartik2112 on Kaggle](https://www.kaggle.com/datasets/kartik2112/fraud-detection) — CC0 Public Domain
- Code Institute README template: [da-README-template](https://github.com/Code-Institute-Solutions/da-README-template)
- `[Add any tutorials, Stack Overflow answers, or documentation referenced during the project]`

### Media

- Code Institute logo used with permission from Code Institute

### Tools

- [Claude by Anthropic](https://claude.ai) — used for ideation, code templates, hypothesis framing, and README structure. All outputs reviewed and validated before use.

---

## Acknowledgements

`[Thank your facilitator, cohort peers, or anyone who provided support through the project]`