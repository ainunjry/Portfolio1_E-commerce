# Portfolio1_E-commerce
## Customer behavior analysis related to Returns and Churn rate.

### Business understanding
This analysis aims to understand customer purchasing behavior based on transaction
characteristics, demographics, product categories, payment methods, and product returns.
It also identifies patterns related to customer churn and factors that may influence customer retention.
The findings are expected to provide relevant insights
to support business decisions aimed at improving customer retention and value.

### About the dataset
**Data descriptions:**

The "E-commerce Customer Behavior and Purchase Dataset" has been designed for data analysis and predictive
modeling for tasks such as customer churn prediction, market basket analysis, recommendation systems,
and trend analysis.

**Column information:**

The dataset contains the following columns:

1. Transaction ID: A unique identifier for each transaction.
2. Customer ID: A unique identifier for each customer.
3. Customer Name: The name of the customer.
4. Customer Age: The age of the customer.
5. Gender: The gender of the customer.
6. Purchase Date: The date of each purchase made by the customer.
7. Product Category: The category or type of the purchased product.
8. Product Price: The price of the purchased product.
9. Quantity: The quantity of the product purchased.
10. Total Purchase Amount: The total amount spent by the customer in each transaction.
11. Payment Method: The method of payment used by the customer (e.g., credit card, PayPal).
12. Returns: Whether the customer returned any products from the order (binary: 0 for no return, 10 for return).
13. Churn: A binary column indicating whether the customer has churned (0 for retained, 1 for churned).

#### Data sources

The dataset used for this analysis is the "ecommerce_customer_data_large.csv" from Kaggle, consisting of 250.000 rows.
This dataset is not included in this repository due to size consideration. Its analysis was performed
using the original dataset.

### Tools

- Google Colab - Data cleaning and Exploratory data analysis
  - [Download here :]
  - (https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/EDA_EXTRACT_1.ipynb)
- XAMPP PhpMyAdmin - SQL - Data Analysis
- Tableau - Visualization

### Business questions
   1. Which product category has the highest number of purchases?
   2. Are age and gender related to purchase volume and frequency?
   3. What is the average quantity purchased by customers?
   4. What is the highest product price in each product category?
   5. Which product category has the highest return and churn rates?
   6. Is the payment method related to returns and churn?
   7. Is gender related to returns and churn?
   8. What is the correlation between number of customers, returns, and churn?
   9. What is the correlation between returns and churn?
   10. Does purchase frequency by customer age   differ between churned and retained customers?
   11. Does customer behavior change over time?
   12. Does purchasing behavior differ between customer who returns product and those who don't?
   13. Which factors are most  strongly associated with customer churn?

### Customer behavior analysis

***Python Pandas***
1. Data understanding and data cleaning
 - a. info :
     - df.head(10)
     - df.info()
 - b. shape :
     - df.shape
     - print(type(df)
 - c. checking duplicates :
     - df.duplicated().sum()
 - d. checking missing values by creating another DataFrame variable :
     - ex_data = df.copy()
     - def cek(a,b):
      - hasil = a[b].isnull().sum()
      - print(f"The Number of isnull column {b}: {hasil}")

     - cek(ex_data,'Product Category')
     - cek(ex_data,'Product Price')
 - e. checking unique values  :
     - ex_data['Customer Name'].nunique()
     - ex_data['Returns'].value_counts()
   











