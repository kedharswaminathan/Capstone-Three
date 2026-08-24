# Customer Churn Prediction

**Author:** Kedhar Swaminathan
**Program:** Springboard Data Analytics Career Track

Predicting which telecom subscribers are likely to cancel, and — more importantly — deciding which of them are worth contacting.

---

## Business problem

Retaining an existing customer is cheaper than acquiring a new one, so a subscription business wants to intervene *before* a customer leaves. That requires answering two separate questions:

1. **Can we rank customers by churn risk?** (a modelling question)
2. **Where do we draw the line between "contact" and "don't contact"?** (a business question)

Most churn analyses answer only the first. This project answers both, and finds that the second question cannot be settled without financial data the dataset does not contain.

---

## Dataset

The IBM Telco Customer Churn dataset: **7,043 customer records, 21 variables** covering demographics, account details, subscribed services, tenure, and billing.

The dataset is committed to `data/` so the notebooks run without modification.

**Class balance: 26.5% of customers churned.** This matters more than it first appears — see below.

---

## Why accuracy is the wrong metric here

Because only 26.5% of customers churn, a model that predicts "no churn" for everyone achieves **73.42% accuracy** while providing zero business value. Any accuracy figure in this project has to be read against that baseline, not against zero.

ROC-AUC is the headline metric instead, because it measures how well the model *ranks* customers by risk — which is the capability a retention program actually uses.

---

## Model performance

All models evaluated on a held-out 20% test set (stratified split, `random_state=42`).

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Baseline (predict "no churn") | 73.42% | — | 0.00% | — | 0.500 |
| **Logistic Regression** (`class_weight='balanced'`) | 72.64% | 49.09% | **79.41%** | 60.67% | 0.8353 |
| Random Forest | 78.68% | 62.25% | 50.27% | 55.62% | 0.8172 |
| Gradient Boosting | [FILL] | [FILL] | [FILL] | [FILL] | 0.841 |

**Logistic Regression is the selected model**, despite Gradient Boosting's marginally higher ROC-AUC (0.841 vs 0.835). The gap is small enough to be within noise on a test set this size, and logistic regression's coefficients show both the direction and relative magnitude of each predictor's effect — which matters when the output has to justify spending money on specific customers.

### What class weighting did, and didn't do

Applying `class_weight='balanced'` to the logistic regression:

| | Default | Balanced |
|---|---|---|
| Recall | 56.95% | **79.41%** |
| Precision | 64.94% | 49.09% |
| Accuracy | 80.38% | 72.64% |
| ROC-AUC | 0.8361 | 0.8353 |

**ROC-AUC did not move.** The model's ability to rank customers by risk was identical before and after. What changed was the operating point — where the cut between "flag" and "don't flag" falls. Class weighting is a threshold adjustment in disguise, not a modelling improvement.

Accuracy now sits slightly below the naive baseline. On this problem that is acceptable: the errors have very different costs, and accuracy weights them equally.

---

## The retention decision: choosing a threshold deliberately

`predict()` uses a 0.5 cutoff because that is scikit-learn's default, not because anyone chose it. Replacing it with an explicit cost model:

- Each retention offer costs **$50**, paid on every customer contacted
- A lost customer costs **$500** in foregone lifetime value
- The offer retains **30%** of the churning customers who receive it

Under these assumptions the cost-minimizing threshold is **0.40**, contacting 440 customers (255 genuine churners, 185 false alarms) at a total cost of $170,750 — against $171,450 at the default cutoff.

**The saving is roughly $700, under 1% of total cost.** Threshold tuning is close to irrelevant here.

### Sensitivity to the cost assumption

The $500 figure is an assumption; the dataset contains no margin or acquisition-cost data. Varying it:

| Lost-customer value | Ratio | Optimal threshold | Customers contacted |
|---|---|---|---|
| $250 | 5:1 | 0.69 | 103 |
| $500 | 10:1 | 0.40 | 440 |
| $1,000 | 20:1 | 0.14 | 817 |

**An eightfold swing in campaign size**, driven entirely by an assumed number. This matches the analytical break-even, `OFFER_COST / (CHURN_LOSS × SAVE_RATE)`, which gives 0.67, 0.33, and 0.17 respectively.

### What this means

1. **No single threshold can be recommended** without real customer-lifetime-value figures from finance. Any project that reports one without stating its cost assumptions is reporting an arbitrary number.
2. **Offer effectiveness dominates threshold choice.** Raising the 30% save rate would change total cost far more than any adjustment to the model or its cutoff. That is where the business should focus.

---

## Key predictors

Contract type, tenure, fiber-optic internet service, payment method, and monthly charges carry the strongest signal. Directionally: shorter-tenure, month-to-month customers on higher monthly charges churn more.

Note that these are associations, not causes. Higher monthly charges correlating with churn does not establish that lowering prices would retain anyone.

---

## Project structure

```
Capstone-Three/
├── data/
│   └── WA_Fn-UseC_-Telco-Customer-Churn.csv
├── 01_data_understanding.ipynb    — EDA, class balance, baseline
├── 02_feature_engineering.ipynb   — cleaning, encoding, train/test split
├── 03_modeling.ipynb              — three classifiers, comparison
├── 04_evaluation.ipynb            — final model, cost analysis, sensitivity
├── final_model_comparison.csv
├── final_model_predictions.csv
├── logistic_regression_coefficients.csv
└── README.md
```

Run the notebooks in order. All paths are relative to the repository root.

---

## Preprocessing notes

- `TotalCharges` arrives as text and contains blanks for customers with zero tenure; converted with `pd.to_numeric(errors='coerce')` and the resulting nulls handled explicitly
- `customerID` dropped (unique identifier, no predictive content)
- Categorical variables one-hot encoded
- Numeric features standardized, fit on the training split only
- 80/20 stratified split, `random_state=42`

---

## Limitations

**No cost data.** The single largest limitation. Without customer lifetime value, the retention threshold cannot be specified — only bounded.

**No hyperparameter tuning or cross-validation.** Results come from a single train/test split with default parameters. Cross-validated performance would be a more reliable estimate.

**Associations, not causes.** The model identifies patterns in historical data. It does not establish that changing any of these variables would change churn behaviour.

**Static snapshot.** The dataset captures one point in time and cannot reflect changes in customer behaviour, competitive pressure, or pricing.

**Missing behavioural signals.** Support interactions, complaints, outages, and satisfaction scores are absent and would likely improve prediction.

---

## Next steps

- Obtain real CLV figures to fix the threshold
- Cross-validate and tune hyperparameters
- Test whether model-driven retention offers actually reduce churn, via a holdout control group
- Measure retained revenue rather than classification metrics
