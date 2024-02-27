# Financial Fraud Detection 

Analyzing a dataset of bank transactions and capturing as many fraudulent transactions as possible while minimizing false positives. 

## Dataset Description 

With over 6,000,000 transactions and consist of 11 columns.  

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

![Transaction Types](docs\Transactions Types.png)

<pre>
# fraudulent transactions
fraudulent_transactions = df[df['isFraud'] == 1]

# the distribution of fraudulent transactions by transaction type
fraudulent_transaction_distribution = fraudulent_transactions['type'].value_counts()

print(f"Distribution of Fraudulent Transactions by Transaction Type: {fraudulent_transaction_distribution}")
</pre>

# Fraud occured in 'cash_out' and 'transfer'  

<pre>
Distribution of Fraudulent Transactions by Transaction Type: type
CASH_OUT    4116
TRANSFER    4097
Name: count, dtype: int64
</pre>


## Fraud Detection Analysis 
- Created a random sample of 10% of the dataset due to the large size of the dataset.
- Dropped columns: 'nameDest', 'nameOrig','isFlaggedFraud'
- 'isFlaggedFraud' only flags a transaction as fraudulent if it is greater than 200,000. 
- 'nameDest' and 'nameOrig' may have a large number of unique categorical values which can be computationally expensive. 
- converted 'step' to 'HourOfDay' assuming 'step' starts from hour 1


## Model Performance 

- Ran a Gradient Boosting Classifier to received an accuracy of 0.9995725044949895

- Ran a RandomizedSearchCV as the dataset was too large for a GridSearchCV
- Using it to find the best parameters: 'n_estimators','learning_rate, and 'max_depth'

<pre>
# best parameters
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
