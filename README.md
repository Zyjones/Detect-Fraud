<p align="center">
  <img src="Visualizations/Caishen Logo.png" alt="Centered Image" width="200" height="200">
</p>

# <center> Caishen Detect Fraud<center> 

## <center>Client Background<center>

**Caishen**, a Zurich-based international bank, announced in an all-hands meeting that the goal for the next 3 years will be to identify 99% of all fraudulent activity within their customer-facing bank accounts. 

Collaborating with the cybersecurity team, an in depth analysis was conducted and a comprehensive machine learning pipeline to detect if fraudulent activity has occurred for a transaction utilizing a dataset of 1 million bank transactions. This comprehensive review provides valuable insights that will help advance their performance. The key insights and recommendations focus on the following areas:
### Nortstar Metrics
* Initial Insights about Fradualent versus Legitimate Transations
* Report of Current Fraud Flagging System
* Trained Machine Learning Model Detecting Fraud 


## Insights Exploratory Data Analysis (Key Findings)
### Fradualent versus Legitimate Transations
* **Most transactions involve lower amounts of money** because majority of the numeric features in our data set have a **right skewed distribution**. 
* **During fradulent transactions, account is drained to zero. Legitimate transactions have more variance in balance values, likely because real transactions involve a broader range of account types and amounts.** The correlation bewteen new balance and old balance without fraud is 0.999418 thus showing that normal activty will have a strong correlation, and consistent relationship between the old and new balance.
* The most popular transaction type is **cash out** with 351360 transactions. However, the transactively types that consistently involve bigger amoounts of money is **transfer transactions**.
* The only transaction types with **instances of fraud are cash out and transfer bank transactions**. The average amount of money transferred in both of these transaction types is higher in cases where there is fraud. 
* **These fraudulent transitons involve draining the entire balance from the origin account** which is why in cash out transitions we see the average of the old balance accounts go from 1.324727e+06 to 0. **The receiving accounts are likely newly created and that the scammers are transferring money into fresh accounts** which is why we see average of oldbalanceDest is 0 while newbalanceDest has an average of 
5.442266e+02. 

### Report of Current Fraud Flagging System:
* **The system's current fraud flagging (isFlaggedFraud) does not align well with actual fraudulent activity (isFraud)**. 
    * There is only 1 row with isFlaggedFraud being marked 1 out of a million transactions. 
    * The built in fraud flag is suppose to be flag cases where the amount is over 200,000.
    * However, there are 1297 rows with isFraud is detected but there are 852 rows where fraud is detected AND the amount is over 200,000 meaning 852 rows should have been flagged.
    * Of the 1,297 transactions that are confirmed as fraudulent, 445 have amounts below 200,000 and therefore would not be flagged by the current system, even though they are fraudulent.
    * A problem that often occurs in the domain of fraud detection is [class imbalance](https://developers.google.com/machine-learning/crash-course/overfitting/imbalanced-datasets). We can assume that an overwhelming majority of transactions are credible and therefore "achieve" 99% accuracy simply by classifying every new sample as non-fraudulent (0). However, if we do this, we will miss every single fraudulent transaction and subsequently get a sensitivity of 0%. 


## Detecting Fraud Model Evaluation (RandomForestClassifier)
* The model performs extremely well overall, with almost perfect accuracy on legitimate transactions. Importantly, it does not produce any false positives, meaning customers are unlikely to be incorrectly flagged for fraud.
* However, the model still misses some fraud cases due to the highly imbalanced dataset. It captures 75% of fraud cases (recall), identifying 3 out of 4 fraudulent transactions. This suggests we may need to adjust class weights, apply resampling techniques, or change the decision threshold to improve fraud detection recall.

#### **Confusion Matrix**

|                | **Predicted: 0** | **Predicted: 1** |
|----------------|------------------|------------------|
| **Actual: 0**  | 2996             | 0                |
| **Actual: 1**  | 1                | 3                |

---

#### **Classification Report**

| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| **0 (Not Fraud)** | 1.00 | 1.00 | 1.00 | 2996 |
| **1 (Fraud)** | 1.00 | 0.75 | 0.86 | 4 |
| **Accuracy** | — | — | **1.00** | 3000 |
| **Macro Avg** | 1.00 | 0.88 | 0.93 | 3000 |
| **Weighted Avg** | 1.00 | 1.00 | 1.00 | 3000 |

---

## Project Structure
```text
DETECT_FRAUD/
├── README.md
├── data/
│   └── bank_transactions.csv
├── notebooks/
│   ├── eda.ipynb
│   ├── model_train.ipynb
│   └── transform.ipynb
├── Visualizations/
├── documentation/
│   ├── data_dictionary.md
│   └── notebook_overview.md
└── requirements.txt
```



## Installation

1. Clone the repository: https://github.com/Zyjones/Detect-Fraud.git 
2. Create and activate a virtual environment (recommended):
    - python -m venv venv
    - source venv/bin/activate # macOS/Linux
    - venv\Scripts\activate # Windows
3. Install dependencies:
```bash
    pip install -r requirements.txt

