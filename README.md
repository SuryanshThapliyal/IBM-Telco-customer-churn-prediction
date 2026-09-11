# IBM Telco Customer Churn Prediction

An end-to-end machine-learning project that identifies telecom customers who may be at risk of churn. The goal is to support retention teams with a prioritised outreach list rather than make automatic decisions about customers.

## Business Problem

Customer churn directly affects recurring revenue. This project uses customer account, service, contract, and billing attributes to estimate churn risk so that retention efforts can focus on customers who are more likely to leave.

The key business objective is to identify potential churners while minimizing the number of customers who are missed by the model.

## Dataset

The analysis uses the IBM Telco Customer Churn dataset, containing 7,043 customer records.

The target variable is `Churn`:

- `Yes` → Customer churned
- `No` → Customer did not churn

Approximately 26% of customers in the dataset churned.

The source dataset is commonly distributed as `WA_Fn-UseC_-Telco-Customer-Churn.csv` through the IBM Telco Customer Churn dataset on Kaggle.

The dataset is not included in this repository. Download it and place it in a local `data/` folder before running the notebook.

## Project Workflow

1. Data cleaning and preparation
2. Exploratory data analysis
3. Feature preprocessing
4. Train/test splitting
5. Baseline model training
6. Hyperparameter tuning
7. Validation-based threshold selection
8. Final test evaluation
9. Feature importance analysis
10. False-negative analysis
11. Business recommendations

## Exploratory Data Analysis

The analysis examined customer demographics, tenure, contracts, services, billing characteristics, and payment methods to identify patterns associated with churn.

### Key Findings

**Contract type**

Month-to-month customers showed substantially higher churn than customers on one-year and two-year contracts. Contract type was also the most important grouped feature in the final Random Forest model.

**Customer tenure**

Customers with shorter tenure were considerably more likely to churn. The churn rate was highest among customers in the earliest tenure group and decreased as customer tenure increased.

**Monthly charges**

Churned customers generally had higher monthly charges than customers who remained. Within tenure groups, churned customers tended to have higher monthly-charge distributions.

**Payment method**

Customers using electronic checks had substantially higher churn rates than customers using automatic payment methods.

**Online Security and Technical Support**

Customers without OnlineSecurity and TechSupport showed substantially higher churn rates than customers who had these services.

These findings represent associations observed in the dataset and should not be interpreted as evidence that any individual factor directly causes churn.

## Models Evaluated

Four classification algorithms were evaluated:

- Logistic Regression
- Decision Tree
- Random Forest
- Gradient Boosting

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC

Because the primary business objective is identifying potential churners, recall for the churn class was given particular importance.

## Final Model and Performance

The final model is a **Random Forest** with `class_weight="balanced"` and a classification threshold of **0.40**.

| Test Metric at Threshold 0.40 | Result |
| --- | ---: |
| Accuracy | ~72.1% |
| Precision | ~48.5% |
| Recall | ~83.7% |
| F1 Score | ~61.4% |
| ROC-AUC | ~84.0% |

The model achieves high recall for the churn class, allowing it to identify a large proportion of customers who actually churned.

The trade-off is lower precision, meaning more customers who are predicted as high-risk will ultimately remain.

For this reason, the model is intended as a **customer-prioritization tool** rather than an automatic decision-making system.

## Threshold Selection

A classification threshold of `0.40` was selected using validation data.

The threshold was chosen based on the business objective of prioritizing recall for customer retention.

Lowering the threshold allows more potential churners to be identified, while increasing the number of false positives.

The `0.40` threshold was therefore selected as a business-oriented trade-off between identifying churners and limiting unnecessary outreach.

It was **not** selected because it maximizes F1 score.

## Feature Importance

Random Forest feature importance was used to understand which variables contributed most to the model's predictions.

The most influential feature groups included:

- Contract
- Tenure
- TotalCharges
- OnlineSecurity
- MonthlyCharges
- InternetService
- TechSupport
- PaymentMethod
- OnlineBackup

Contract type was the strongest grouped feature, followed by customer tenure and billing-related variables.

Feature importance represents predictive usefulness within the trained model. It does not represent the percentage of churn caused by a feature and does not establish causation.

## Error Analysis

False-negative analysis was performed to understand customers who actually churned but were not flagged by the final model.

The analysis showed that false negatives tended to have higher tenure and higher TotalCharges than customers who were correctly identified as churners.

This suggests that the model is particularly effective at identifying more typical high-risk churn profiles, especially shorter-tenure customers, but can have difficulty identifying customers who churn despite having characteristics generally associated with retention.

Model performance also varied across contract types.

The model identified approximately:

- 92% of month-to-month churners
- 25% of one-year contract churners
- 0% of two-year contract churners in the test set

The two-year result should not be generalized because the number of churn cases in this segment was very small.

## Business Recommendations

### 1. Prioritize Month-to-Month Customers

Month-to-month customers showed substantially higher churn than customers on longer-term contracts.

Recommended actions:

- Use model predictions to identify high-risk month-to-month customers.
- Prioritize these customers for proactive retention outreach.
- Consider targeted incentives for customers who may benefit from longer-term plans.

### 2. Focus on Early-Tenure Customers

Customers in the early stages of their relationship showed substantially higher churn.

Recommended actions:

- Strengthen onboarding and early customer support.
- Monitor new customers for signs of dissatisfaction.
- Conduct proactive satisfaction checks.
- Prioritize high-risk early-tenure customers for retention efforts.

### 3. Target Customers Without Online Security or Technical Support

Customers without OnlineSecurity and TechSupport showed substantially higher churn rates.

Recommended actions:

- Identify high-risk customers who do not use these services.
- Consider targeted trials, bundles, or upgrades.
- Provide proactive technical support to customers showing signs of churn.

These recommendations are based on observed associations and do not establish that adding these services directly causes lower churn.

### 4. Monitor Electronic-Check Customers

Electronic-check customers showed substantially higher churn rates than customers using automatic payment methods.

Recommended actions:

- Include payment method as one factor in retention prioritization.
- Monitor high-risk electronic-check customers more closely.
- Consider convenient payment options as part of broader customer-engagement strategies.

Payment method should be treated as a risk signal rather than evidence that changing payment method itself causes lower churn.

## Limitations

- The dataset represents historical customer observations and may not perfectly represent a current customer population.
- Model performance may change as products, pricing, and customer behavior change.
- The model produces false positives as well as false negatives.
- Performance varies across customer segments, particularly contract types.
- The two-year contract segment contains very few churn cases in the test set.
- Feature importance and observed relationships do not establish causation.
- The classification threshold was selected using validation data.
- Real business costs were not available for formal cost-sensitive threshold optimization.

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

## Project Structure

```text
IBM-Telco-customer-churn-prediction/
├── customer_churn.ipynb
├── README.md
├── requirements.txt
└── .gitignore
