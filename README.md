# Translated - Data Science Assessment 
This repository contains technical assessment exercises for Data Scientists by Translated.

The assesment consists of 3 exercises and the repo is organized in the following way:

* [`assessment_translated.ipynb`](assessment_translated.ipynb) : notebook with the solutions for exercise 1.
* [`data`](data) : folder with the dataset used in exercise 1.
* `README.md` : README file with description of the repo and answers for exercise 2 and 3.

## Exercise 1 - Customer Churn Prediction
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

## Exercise 2 - Product Sales 
This exercise is about SQL and data management.

You have the following tables organized in a database:
* customers 
  * id 
  * firstname 
  * lastname 
  * data_of_birth 
  * country 
* orders 
  * id 
  * id_customer 
  * order_cost 
* orders_items 
  * id 
  * id_order 
  * id_product 
  * quantity 
* products 
  * id 
  * name 
  * price 

1. Can you explain the purpose of the orders_items table? 

    The orders_items table plays a key role in tracking the products that are sold and it is crucial for retrieving detailed information about customer orders. To be more specific, the purpose of the orders_items table is to be able to store and find information about the  exact products that are associated with each order. In other words, each row in the orders_items table represents a product that is associated with a particular order. Furthermore, it enables us to join /link the orders and products tables  through id_order and id_product columns respectively. Therefore, we can retrieve  information about the products for each order such as the name of the product, price ,and so on.
2. Can you write a SQL query to find the average order cost per country? 

    ```
    SELECT 
        customers.country, 
        AVG(orders.order_cost) AS avg_order_cost
    FROM 
        customers 
        INNER JOIN orders ON customers.id = orders.id_customer
    GROUP BY 
        customers.country;
    ```

    This query performs an inner join between the customers and orders tables on the id and id_customer columns. Then it groups the results by the country column in the customers table and computes the average order cost with AVG() function on the order_cost column in the orders table.
    So this SQL query returns a table with 2 columns, columns being country and avg_order_cost. The avg_order_cost represents the average order cost for each country in the customers table.


3. Can you write a SQL query to find the name of the highest price product sold to an Italian customer? 

    ```
    SELECT 
        products.name 
    FROM 
        customers 
        JOIN orders ON customers.id = orders.id_customer 
        JOIN orders_items ON orders.id = orders_items.id_order 
        JOIN products ON orders_items.id_product = products.id 
    WHERE 
        customers.country = 'Italy' 
    ORDER BY 
        products.price DESC 
    LIMIT 1;
    ```

    This SQL query joins the customers, orders, orders_items, and products tables; filters the results to only include customers from Italy. It then orders the results by product price in descending order, and selects only the top result using the LIMIT 1. It selects the name of the product using products.name column. 


4.  How could you optimize the orders table assuming you have to manage a lot of queries? 

    In order to optimize the orders table for managing a lot of queries, one can consider “indexing”.  It is one of the most effective ways to optimize a table; by creating indexes on the columns that are commonly used in queries. Once frequently used columns are identified, we can choose the appropriate index type ( clustered, non-clustered) based on the usage patterns of the columns being indexed. However, it is important to avoid creating too many indexes, since it might slow down write operations and increase storage requirements.



5. What would you change if the amount of data is too big to run these queries? (suppose to have 500TB of data) 

    If the amount of the data is too big to run these queries, one can consider “ Data Partitioning”  for dividing the data into smaller and more manageable that can be processed independently. This can help to reduce the amount of data that needs to be processed in a single query and can improve the query performance. To illustrate, we could partition the orders table by date range so that orders are stored in separate tables based on the date they were placed. This will help to reduce the amount of the data that needs to be processed for each query and can improve query performance. Alternatively, avoiding the use of  SELECT* and using WHERE clause for limiting the data amount that a query returns.

## Exercise 3 - Machine Translation 

This exercise is about data management and Machine Translation.

You have access to a large amount of translation data (i.e., parallel files containing a sentence in a source language and the corresponding translation in the target
language). Some of this data comes from public datasets, others have been produced by professional translators working for your own company.

1. How would you design a data pipeline for a Machine Translation system? (e.g. necessary steps, main challenges, etc.) 

    The task of Machine Translation requires large amounts of good quality data in order to achieve a good performance. In the context where one has access to parallel corpora containing gold standard data produced by professional translators, the first step would include cleaning the public datasets by leveraging the gold standard one. To be more precise, depending on the size of gold standard data, one could either train a model with encoder-decoder architecture or fine tune a sentence transformers (multilingual) with that aligned corpora for learning multilingual sentence embeddings. With this approach, one can obtain a dense representation of sentences from different languages in one joint space where a given source and target sentence are close to each other in the semantic space. Then this method can be used to filter the public dataset by obtaining embeddings from joint semantic space and calculating cosine distance between source and target sentence. Therefore, the quality of a public dataset can be ensured by removing noise from it with a predefined threshold on the cosine distance for sentence pairs. Note that this process would require good quality of gold standard data; the optimum scenario would be having multiple targets for source sentences for creating a better semantic space. Furthermore, it is important to have parallel data from different domains and different styles to ensure Machine Translation System can handle different text genres.



2. What would you do to collect new and good quality data from the web? Assume you want to use them to train a neural model for Machine Translation.

    Since training a NMT model requires good quality and large amounts of parallel data, the first step would be deciding on the language pair (i.e., source and target language) to narrow the search. A good source for parallel data could be an international organization like the United Nations or community provided translation such as TED talks. Furthermore, data from reputable websites with high quality content can be scraped using web scraping tools or APIs if available. Wikipedia or news collections could be a good source for collecting a comparable corpora with potential mutual translations, which then could be aligned/parallelized using the cosine distance or nearest neighbors from the pre-trained joint embedding space. Additionally, several data augmentation methods can be employed to enrich the training data. In particular, back-translation from source language to target one could be used for generating additional parallel data for the training.

 3. Suppose that low quality translations created by the system are post-edited by professional translators. How would you use this process to monitor the quality of the Machine Translation system?

    In order to integrate the process of post-editing by professionals to evaluate and monitor the system’s quality, a numerical metric such as BLEU (Bilingual Evaluation Understudy) could be used. In this case, the BLEU score that is obtained through the precision of n-gram overlap between the output of the system and the edited version of that output can be used as a way to monitor the system. In addition to that, the edited version of the output can be considered as a valuable way to enrich the training data as it might provide a different version for the target sequence.
