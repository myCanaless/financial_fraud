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
![Transaction Types](../docs/Transactions Types.png)

<pre>
```python
# fraudulent transactions
fraudulent_transactions = df[df['isFraud'] == 1]

# the distribution of fraudulent transactions by transaction type
fraudulent_transaction_distribution = fraudulent_transactions['type'].value_counts()

print(f"Distribution of Fraudulent Transactions by Transaction Type: {fraudulent_transaction_distribution}")
```
</pre>



## Fraud Detection Analysis 
