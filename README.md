# Financial Fraud Detection 

Analyzing a dataset of bank transactions and capturing as many fraudulent transactions as possible while minimizing false positives. 

## Dataset Description 

With over 6,000,000 transactions totaling up to 1,144 billion and consist of 11 columns:  

- **Step**: Represents a unit of time in hours. This is essentially the timestamp of each transaction (e.g., hour 1, hour 2, ..., hour 534, etc.).
- **Type**: Specifies the type of transaction.
- **Amount**: The amount of money transferred in the transaction.
- **NameOrig**: The identifier for the origin account from which the transaction was initiated.
- **OldBalanceOrg**: The balance of the origin account before the transaction took place.
- **NewBalanceOrg**: The balance of the origin account after the transaction has been processed.
- **NameDest**: The identifier for the destination account to which the transaction was directed.
- **OldbalanceDest**: The balance of the destination account before receiving the transaction.
- **NewbalanceDest**: The balance of the destination account after receiving the transaction.
- **IsFlaggedFraud**: A simplistic model flag that indicates a transaction as fraudulent if the amount transferred is greater than 200,000 (note: the currency is unspecified).
- **IsFraud**: Indicates whether a transaction was actually fraudulent, defined as a malicious attempt to transfer funds out of a victim’s account without their consent.

The types of transactions 

![Transaction Types](https://github.com/myCanaless/financial_fruad/assets/96447448/c02921f8-8b5a-49b9-b216-73e1bf9f2ffd)

![Bar plot](https://github.com/myCanaless/financial_fruad/assets/96447448/f5954283-66fd-4c86-998e-83d4769bb29e)


The total amount 
<pre>
total_transaction_amount = df['amount'].sum()
total = total_transaction_amount / 1e9
</pre>
<pre>
The total amount of transactions made is: 1144.39 billion.
</pre>

### Fraud occurred in 'cash_out' and 'transfer'    
<pre>
# fraudulent transactions
fraudulent_transactions = df[df['isFraud'] == 1]

# the distribution of fraudulent transactions by transaction type
fraudulent_transaction_distribution = fraudulent_transactions['type'].value_counts()

</pre>

<pre>
Distribution of Fraudulent Transactions by Transaction Type: type
CASH_OUT    4116
TRANSFER    4097
</pre>

Fraudulent Transactions 

![Fraud Transactions](https://github.com/myCanaless/financial_fruad/assets/96447448/e96e8e6d-d724-42ce-ae4e-b4c0882b36f3)


## Fraud Detection Analysis 
- Created a random sample of 10% of the dataset due to the large size of the dataset.
- Dropped columns: 'nameDest', 'nameOrig','isFlaggedFraud'
- 'isFlaggedFraud' only flags a transaction as fraudulent if it is greater than 200,000. 
- 'nameDest' and 'nameOrig' may have a large number of unique categorical values which can be computationally expensive. 
- converted 'step' to 'HourOfDay' assuming 'step' starts from hour 1

Hourly Transactions 

![Hourly Transaction Activity](https://github.com/myCanaless/financial_fruad/assets/96447448/2c947748-d1b2-4056-a79a-b8191f451836)

Box Plot of Hourly Transactions

![Box Plot](https://github.com/myCanaless/financial_fruad/assets/96447448/02765d6e-2ae4-4171-a01e-a5ecd31b953a)


## Model Performance 

- Ran a Gradient Boosting Classifier to received an accuracy of 0.9995725044949895

- Ran a RandomizedSearchCV as the dataset was too large for a GridSearchCV
- Using it to find the best parameters: 'n_estimators','learning_rate, and 'max_depth'

<pre>
param_dist = {
    'n_estimators': randint(50, 300),  # number of trees between 50 and 500
    'learning_rate': uniform(0.01, 0.1),  # prevents overfitting
    'max_depth': randint(3, 10)  # max depth of each tree between 3 and 10 
}

random_search = RandomizedSearchCV(
    estimator=gb_class, 
    param_distributions=param_dist, 
    n_iter=10, 
    cv=5, 
    random_state=42, 
    scoring='accuracy', 
    n_jobs=-1
)
</pre>
Received a best score of 0.9995389733717996 & the best parameters:  
<pre>
best_params = {
    'n_estimators': 199,
    'learning_rate': 0.02428668179219408,
    'max_depth': 5
}
</pre>

# f1 Score

<pre>
Optimized f1_score 
</pre>
<pre>
0.8169761273209549
</pre>
