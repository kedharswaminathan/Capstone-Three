# Customer Churn Prediction Using Machine Learning

## Capstone Three Project

**Author:** Kedhar Swaminathan  
**Program:** Springboard Data Science Career Track

---

## Project Overview

Customer churn is an important challenge for subscription-based businesses. Losing customers can reduce recurring revenue and increase the cost of acquiring new customers.

This project uses machine learning to predict which customers are most likely to cancel their subscription.

The analysis uses customer demographic information, account characteristics, service subscriptions, tenure, and billing information to identify patterns associated with customer churn.

The ultimate goal is to provide businesses with information that can support targeted customer-retention strategies.

---

## Business Problem

The primary business question for this project is:

> **Can machine learning be used to identify customers who are at increased risk of churning?**

If customers at risk of churn can be identified early, businesses can prioritize those customers for retention efforts before they cancel their subscriptions.

---

## Dataset

The project uses the Telco Customer Churn dataset.

The original dataset contains:

- **7,043 customer records**
- **21 variables**
- Demographic information
- Account information
- Service information
- Billing information
- Customer tenure
- Churn status

The target variable is:

- `0` = Customer did not churn
- `1` = Customer churned

The original dataset contains variables including:

- `gender`
- `SeniorCitizen`
- `Partner`
- `Dependents`
- `tenure`
- `PhoneService`
- `MultipleLines`
- `InternetService`
- `OnlineSecurity`
- `OnlineBackup`
- `DeviceProtection`
- `TechSupport`
- `StreamingTV`
- `StreamingMovies`
- `Contract`
- `PaperlessBilling`
- `PaymentMethod`
- `MonthlyCharges`
- `TotalCharges`
- `Churn`

---

# Project Workflow

The project is divided into four Jupyter notebooks.

### 01 - Data Understanding

`01_data_understanding.ipynb`

This notebook explores and understands the customer churn dataset.

The analysis includes:

- Dataset structure
- Data types
- Missing values
- Numerical variables
- Categorical variables
- Target variable
- Exploratory data analysis
- Relationships between customer characteristics and churn

The dataset contains 7,043 customers and 21 variables.

One important data-cleaning issue identified during exploration was that `TotalCharges` was stored as a string rather than a numerical variable.

---

### 02 - Feature Engineering

`02_feature_engineering.ipynb`

This notebook prepares the dataset for machine learning.

The preprocessing steps include:

- Converting `TotalCharges` to numeric
- Handling missing values
- Removing `customerID`
- Separating features and target
- Converting `Churn` into a binary variable
- One-hot encoding categorical variables
- Splitting the data into training and testing datasets
- Standardizing numerical features
- Checking the final dataset for missing and infinite values

An 80/20 train-test split was used.

---

### 03 - Modeling

`03_modeling.ipynb`

Three classification models were developed and compared:

1. Logistic Regression
2. Random Forest
3. Gradient Boosting

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-Score
- ROC-AUC

The modeling stage also includes visual comparisons of model performance, ROC curves, confusion matrices, and feature importance.

---

### 04 - Evaluation

`04_evaluation.ipynb`

The final evaluation examines the performance of the selected model in greater detail.

The evaluation includes:

- Final model performance
- Confusion matrix
- Classification report
- Actual vs. predicted churn
- ROC curve
- Churn probability
- Feature analysis
- Business interpretation
- Recommendations
- Limitations
- Future research

---

# Model Performance

The final reported test-set results are:

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| **Logistic Regression** | **80.38%** | **64.94%** | **56.95%** | **60.68%** | **83.61%** |
| Random Forest | 78.32% | 61.31% | 50.00% | 55.08% | 81.98% |
| Gradient Boosting | 79.18% | 63.46% | 51.07% | 56.59% | 82.00% |

## Final Model

### Logistic Regression

Logistic Regression was selected as the final model based on the reported test-set results.

It achieved:

- **80.38% Accuracy**
- **64.94% Precision**
- **56.95% Recall**
- **60.68% F1-Score**
- **83.61% ROC-AUC**

The model provides a useful balance between predictive performance and interpretability.

---

# Key Findings

The exploratory analysis identified several differences between customers who churned and customers who remained.

### Customer Tenure

Customers who churned generally had shorter tenures than customers who remained with the company.

This suggests that newer customers may represent an important group for retention efforts.

### Monthly Charges

Customers who churned generally had higher monthly charges than customers who remained.

This suggests that monthly pricing and perceived value may be important areas for further investigation.

### Total Charges

Customers who churned generally had lower total charges.

This is consistent with their shorter tenure because customers who leave earlier have less time to accumulate total charges.

### Customer Contracts

Contract type appears to have an important relationship with churn.

Month-to-month customers represent an important segment for further retention analysis.

### Service and Billing Characteristics

The modeling analysis identified customer billing and service characteristics as important predictors of churn.

Important variables include:

- Tenure
- Total charges
- Monthly charges
- Contract type
- Internet service
- Payment method
- Online security
- Technical support

---

# Business Recommendations

## 1. Create a High-Risk Customer Retention Program

Use the churn prediction model to identify customers with a higher probability of leaving.

These customers can be prioritized for proactive retention campaigns.

Possible interventions could include:

- Personalized offers
- Customer-service outreach
- Contract incentives
- Service upgrades
- Loyalty programs

---

## 2. Focus on Early-Customer Retention

The analysis indicates that customer tenure is an important factor associated with churn.

Businesses should consider additional onboarding and engagement strategies during the early stages of the customer relationship.

The objective would be to establish customer value before the customer reaches the point of cancellation.

---

## 3. Investigate Pricing and Service Value

Customers with higher monthly charges showed differences in churn behavior.

The business should investigate whether customers perceive their monthly costs as providing sufficient value.

Possible strategies include:

- Reviewing pricing structures
- Creating personalized service bundles
- Offering targeted upgrades
- Improving communication about service benefits

---

# Important Business Metrics

The machine-learning model provides predictive metrics, but the ultimate business goal is to reduce customer churn.

Future implementation should measure:

- Reduction in churn rate
- Customer retention rate
- Revenue retained
- Customer Lifetime Value (CLV)
- Retention campaign effectiveness

---

# Limitations

This project has several limitations.

### Historical Data

The model is trained on historical customer data and may not perfectly predict future customer behavior.

### Correlation Does Not Equal Causation

The model identifies relationships between customer characteristics and churn.

These relationships should not automatically be interpreted as causal relationships.

### False Positives and False Negatives

The model will not correctly classify every customer.

Some customers who churn will be missed, while some customers who remain may be incorrectly identified as likely to churn.

### Dataset Limitations

The dataset contains customer demographic, account, service, and billing information but does not include every possible factor that may influence churn.

Additional information such as customer satisfaction, support interactions, complaints, and engagement could potentially improve future models.

---

# Future Research

Future work could improve the project in several areas.

## Model Optimization

Future versions could use:

- Hyperparameter tuning
- Cross-validation
- Additional classification algorithms
- Ensemble methods

## Additional Customer Data

Additional variables could include:

- Customer satisfaction scores
- Customer service interactions
- Complaint history
- Usage behavior
- Promotional offers
- Customer engagement

## Customer Segmentation

Customers could be divided into different risk groups to develop more targeted retention strategies.

## Retention Campaign Testing

The model could be incorporated into a customer-retention campaign and evaluated using A/B testing.

This would allow the business to determine whether model-driven interventions actually reduce churn.

---

# Project Structure

```text
Capstone-Three/
│
├── 01_data_understanding.ipynb (Capstone Three).ipynb
├── 02_feature_engineering.ipynb (Capstone Three).ipynb
├── 03_modeling.ipynb (Capstone Three).ipynb
├── 04_evaluation.ipynb (Capstone Three).ipynb
│
├── model_metrics.csv
├── model_metrics.txt
├── README.md
│
└── data/
    └── WA_Fn-UseC_-Telco-Customer-Churn.csv
