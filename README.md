# IBM Telco Customer Churn Prediction

An end-to-end machine-learning project that identifies telecom customers who may be at risk of churn. The goal is to support retention teams with a prioritised outreach list—not to make automatic decisions about customers.

## Business problem

Customer churn directly affects recurring revenue. This project uses customer account, service, and billing attributes to estimate churn risk so that retention efforts can focus on customers who are more likely to leave.

## Dataset

The analysis uses the IBM Telco Customer Churn dataset, containing 7,043 customer records. The target is `Churn`; approximately 26% of customers churned. The source file is commonly distributed as `WA_Fn-UseC_-Telco-Customer-Churn.csv` through the [IBM Telco Customer Churn dataset on Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn).

The dataset is not included in this repository. Download it and place it in a local `data/` folder before running the notebook.

## Project workflow

1. Clean and prepare customer data.
2. Explore churn patterns across customer, contract, service, and billing variables.
3. Preprocess numeric and categorical features.
4. Train and compare baseline classifiers.
5. Tune the selected model and choose a decision threshold using validation data.
6. Evaluate final test performance, inspect feature importance, and review false negatives.

## EDA highlights

The notebook investigates how churn rates vary across tenure, contract type, service choices, and billing-related attributes. These are observed associations in this dataset; they should not be interpreted as causal effects.

## Models evaluated

- Logistic Regression
- Decision Tree
- Random Forest
- Gradient Boosting

## Final model and performance

The final model is a **Random Forest** with `class_weight="balanced"` and a classification threshold of **0.40**.

| Test metric at threshold 0.40 | Result |
| --- | ---: |
| Accuracy | ~72.1% |
| Precision | ~48.5% |
| Recall | ~83.7% |
| F1 score | ~61.4% |
| ROC-AUC | ~84.0% |

## Why use a 0.40 threshold?

A threshold of `0.40` was selected using validation data because the retention use case prioritises recall. This helps identify more actual churners, while accepting lower precision and a higher number of false positives. It was not selected because it maximises F1.

## Feature importance and error analysis

Random Forest feature importance is used to identify which inputs most influenced the model’s predictions. Importance describes predictive usefulness in this model, not the percentage of churn caused by a feature.

The notebook also reviews false negatives: customers who churned but were not flagged. This matters because missed customers represent potential lost retention opportunities.

## Business recommendations

- Use model scores to prioritise proactive, human-led retention outreach.
- Review high-risk customers alongside business context before taking action.
- Tailor offers and communications to the customer’s plan, tenure, and service profile.
- Track outcomes of retention campaigns and retrain or recalibrate the model as customer behaviour changes.

## Limitations

- The dataset represents historical observations and may not match every current customer population.
- Model performance can change over time as products, pricing, and customer behaviour evolve.
- Associations and feature importance do not establish causation.
- Predictions should support—not replace—business judgment and fair customer treatment.

## Technologies used

Python, pandas, NumPy, Matplotlib, Seaborn, scikit-learn, and Jupyter Notebook.

## Project structure

```text
IBM-Telco-customer-churn-prediction/
├── customer_churn.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## How to run

1. Clone this repository.
2. Create and activate a virtual environment.
3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Download the dataset and place `WA_Fn-UseC_-Telco-Customer-Churn.csv` in `data/`.
5. Start Jupyter and open the notebook:

   ```bash
   jupyter notebook customer_churn.ipynb
   ```

## Future improvements

- Validate the approach on more recent or production customer data.
- Monitor performance and fairness after deployment.
- Evaluate the retention impact and cost of different outreach strategies.
- Add a lightweight dashboard for business users.

## Conclusion

This project demonstrates an end-to-end churn-prediction workflow, from exploration and preprocessing through model evaluation and business-oriented threshold selection. The final model is designed to help retention teams focus their attention on customers who may be at risk of leaving.
