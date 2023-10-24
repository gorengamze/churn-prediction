# Customer Churn Prediction

The objective of this exercise to build a binary classifier that predicts if a bank’s customer is going to churn or not. 

### Dataset
The [Bank Customer Churn Dataset](https://www.kaggle.com/datasets/gauravtopre/bank-customer-churn-dataset?resource=download) contains customer_id, credit_score, country, gender, age, tenure, balance, products_number, credit_card, active_member, and estimated_salary as features and churn is labelled as 1 if the client has left the bank during some period or as 0 if he/she has not.

### Approach
For finding the best model for the task, [LazyClassifier](https://lazypredict.readthedocs.io/en/latest/usage.html) is used to fit all the models to our dataset and compare their performance.

Among all models, RandomForestClassifier is the best performing model. 

Hyperparameters of the RandomForestClassifier are optimized via GridSearch.


### Results

|Model|Accuracy|Precision|Recall|F1 Score|
|-|-|-|-|-|
|RandomForestClassifier|0.90|0.90|0.90|0.90|
