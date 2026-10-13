# Portfolio1_E-commerce

### Content

- [Business understanding](https://github.com/ainunjry/Portfolio1_E-commerce/edit/main/README.md#business-understanding)
- [About the dataset](https://github.com/ainunjry/Portfolio1_E-commerce/edit/main/README.md#about-the-dataset)
- [Data sources](https://github.com/ainunjry/Portfolio1_E-commerce/tree/main#data-sources)
- [Tools](https://github.com/ainunjry/Portfolio1_E-commerce/tree/main#tools)
- [Business questions](https://github.com/ainunjry/Portfolio1_E-commerce/tree/main#business-questions)
- [Data understanding and cleaning](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#data-understanding-and-cleaning)
- [EDA](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#2-exploratory-data-analysis-eda)
- [EDA-Outlier](https://github.com/ainunjry/Portfolio1_E-commerce/tree/main#identifying-outliers-by-calculating-q1-q3-iqr-both-with-outliers-and-no-outliers)
- [EDA-Correlation](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#correlation)
- [Customer Behavior Analysis-Business questions No.1-3](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#customer-behavior-analysis)
- [Customer Behavior Analysis-Business questions for Returns & Churn No.4-6](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#returns-and-churn-rates)
- [Customer Behavior Analysis-Business questions for Returns & Churn  No.7-9](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#business-question-7)
- [Key Insight](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#key-insight)

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

The dataset used for this analysis is the "ecommerce_customer_data_large.csv" from Kaggle.
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
   4. Which product category has the highest return and churn rates?
   5. Is the payment method related to returns and churn?
   6. Is gender related to returns and churn?
   7. What is the correlation between number of customers, returns, and churn?
   8. Does purchase frequency by customer age  differ between churned - retained customers and customers who return
      product and those who don't ? 
   9. How many at-risk  customers who had last 45-60 day-transaction to re-engage, grouped by their favourite product?

### Data understanding and cleaning

***Python Pandas***
 1. Data Cleaning
 - a. info :
    ````python
      df.head(10)
      df.info()
    ````
  
 - b. shape :
  ````python
     df.shape
     print(type(df)
  ````
 - c. checking duplicates :
 ````python
     df.duplicated().sum()
````
 - d. checking missing values by creating another DataFrame variable :
````python
     ex_data = df.copy()
     def cek(a,b):
     hasil = a[b].isnull().sum()
     print(f"The Number of isnull column {b}: {hasil}")

     cek(ex_data,'Product Category')
     cek(ex_data,'Product Price')
````
 - e. checking unique values  :
````python
     ex_data['Customer Name'].nunique()
     ex_data['Returns'].value_counts()
````
     
![View result column Returns](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Nmber%20of%20Returns.png)

#### Interpretation :
The dataset has 250.000 rows, 14 columns, column Returns has less 48.000 missing values, some transactions
appeared had no returned items.

- [Back to content](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#content)

### 2. Exploratory Data Analysis (EDA)
   
   ***Creating a categorical atribute which splits the values,a continous data into a specified number groups.***
````python
   bin_x = np.linspace(min(ex_data['Quantity']), max(ex_data['Quantity']),5)
   bin_x
   Output : array([1., 2., 3., 4., 5.])
````
  
   ***Creating group as a label for bin_x variable***
````python
   g_name = ['Low', 'Under_rated', 'Medium', 'High']
   g_name
   Output : ['Low', 'Under_rated', 'Medium', 'High']
````
  
   ***Creating new column to label column of Quantity***
````python
   ex_data['Quantity_bin'] = pd.cut(ex_data['Quantity'], bin_x, labels=g_name, include_lowest=True)
````

   ***To visualize the data range accross the Quantity column.***
````python
   plt.figure(figsize=(6,4))
   plt.bar(g_name, ex_data['Quantity_bin'].value_counts(),color = 'orange')
   plt.xlabel('Level')
   plt.ylabel('Amount')
   plt.title('Quantity Bins')
````

   - view bar of quantity level :
    
     ![view bar of quantity level](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Level%20quantity.png)
#### Interpretation:
From categorical atribute of values, the Low group of Quantity column is The highest frequency, more than 90k.

   ***To visualize the distribution of Product price using kdeplot,and Total Purchase Amount.***
````python
   plt.figure(figsize = (6,4))
   sns.kdeplot(data=ex_data, x='Product Price', fill=True, color='grey')
   plt.title('KDE distribution of Product Price')
   plt.xlabel('Price')
   plt.show()
````
     
   - View KDE Plot:

  ![View KDE Plot](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/KDE%20Plot.png)

  
  ***Creating function to analyze statistic***
````python
   def statistic(c,d):
      mean = c[d].mean()
      median = c[d].median()
      mode = c[d].mode()
      skewness = c[d].skew()
      print(f"For column {d} : mean={mean}, median={median}, mode={mode}, skewness={skewness}")

   statistic(ex_data, 'Product Price')
````

- Output :
       For column Product Price : mean=254.742724, median=255.0, mode=0    290
       Name: Product Price, dtype: int64, skewness=0.0011635058483736365
#### Interpretation:
The histogram visualized bell-shaped curve, symmetrical, and the skewness is 0, it means Product Price data distribution is normal, median and mean are relatively closed, mean could be representative of typical value.

  ***To check skewness Total Purchase Amount & Quantity column***
````python
    statistic(ex_data, 'Quantity')
    print()
    statistic(ex_data, 'Total Purchase Amount')
````

- Output :
       For column Quantity : mean=3.004936, median=3.0, mode=0    4
       Name: Quantity, dtype: int64, skewness=-0.005935270784344358
       For column Total Purchase Amount : mean=2725.385196, median=2725.0, mode=0    2533 1    4456
       Name: Total Purchase Amount, dtype: int64, skewness=-0.00210492340006921

#### Interpretation:
The mean and the median  either for Total Purchase Amount or Quantity columns are closed, nearly similar,
the skewness for both columns are 0, The mean represent typical value of these two columns.

#### **Identifying outliers by Calculating Q1, Q3, IQR, both with outliers and no outliers.**
---
````python
 def stats(e,f):
     Q1 = e[f].quantile(0.25)
     Q3 = e[f].quantile(0.75)
     IQR = Q3 - Q1
     Lower = Q1 - 1.5 * IQR
     Upper = Q3 + 1.5 * IQR


     outliers = e[f][(e[f]<Lower)|(e[f]>Upper)]
     no_outliers = e[f][(e[f]>=Lower) & (e[f]<=Upper)]

     print(f"1.IQR, Lower limit & Upper limit of column {f} are: {IQR}, {Lower}, {Upper}")

     if outliers.empty:
         print(f"2.Outliers of column {f}: Not Found")
     else:
         print(f"2.The Outliers of column {f} : {len(outliers)}")

     print(f"3.No-outliers of column {f} : {len(no_outliers)}")
````

***Identifying outliers column Product Price***
````python
 stats(ex_data, 'Product Price')
````
   Output:
     - 1.IQR, Lower limit & Upper limit of column Quantity are: 2.0, -1.0, 7.0
     - 2.Outliers of column Quantity: Not Found
     - 3.No-outliers of column Quantity : 250000
   

***Identifying outliers column Quantity***
````python
 stats(ex_data, 'Quantity')
````
  - Output:
     - 1.IQR, Lower limit & Upper limit of column Quantity are: 2.0, -1.0, 7.0
     - 2.Outliers of column Quantity: Not Found
     - 3.No-outliers of column Quantity : 250000
   

***Identifying outliers column Total Purchase Amounty***
````python
 stats(ex_data, 'Total Purchase Amount')
````
- Output :
   - 1.IQR, Lower limit & Upper limit of column Total Purchase Amount are: 2499.0, -2272.5, 7723.5
   - 2.Outliers of column Total Purchase Amount: Not Found
   - 3.No-outliers of column Total Purchase Amount : 250000
 
 #### Interpretation:
 It apparently seems that 3 columns [Product Price, Quantity, Total Purchase Amount]
 have no outliers. To ensure them, checking through displaying BOXPLOT chart


 ***Displaying Box plot to identify outliers column Total Purchase Amount***
 ````python
    ex_data['Total Purchase Amount'].plot(kind='box', figsize=(6,4), vert = False, color = 'green')
    plt.title('The Box Plot Total Purchase')
    plt.ylabel('Amount')
    plt.show()
````
 ![Box plot](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Box%20Plot.png)
     
#### Interpretation:
From  Box Plot Chart visualization for [Total Purchase Amount] columns : No outliers found.

  #### **Correlation**
  ---

  ***Finding correlation between columns by Pearson & P-value and common code .corr()***

  Identifying correlation between columns.
````python
   ex_data[['Product Price','Quantity','Total Purchase Amount']].corr()
````
  - Output:
   
       ![Correlation](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Correlation.png)

  By Pearson & P-value.
````python
   from scipy import stats
   pearson_coef,p_value = stats.pearsonr(ex_data['Churn'],ex_data['Product Price'])
   print(f"Pearson_coef : {pearson_coef}")
   print(f"p_value : {p_value}")
````
- Output:
    - Pearson_coef : 0.0011813320220758743
    - p_value : 0.5547460669888156

- Correlation between column Total Purchase Amount and Product Price
````python
   pearson_coef_TP,p_value_TP = stats.pearsonr(ex_data['Total Purchase Amount'],ex_data['Product Price'])
   print(f"Pearson_coef_ : {pearson_coef_TP}")
   print(f"p_value : {p_value_TP}")
````

- Output:
    - Pearson_coef_ : -0.0012972970953191519
    - p_value : 0.516567488744543
   
  #### Interpretation
  Product Price and Total Amount Purchase shows with almost neglible linear correlation
  such as for each: Churn-Price (Pearson : 0.002 & P_value : 0.554),
  and Churn-Total Amount Purchase (Pearson : -0.0013 & P_value : 0,512),
  the relationships show no statistically significat level.
  They suggest that Product Price and Total Amount Purchase not linearly associated
  with Churn in this dataset.

  [Click here to download ipynb format](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/EDA_EXTRACT_1.ipynb)

  - [Back to content](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#content)

### `Customer Behavior Analysis`

**Executive summary: A brief paragraph outlining the overall business problem and analysis**

***Business question 1:***
   -  Which product category has the highest number of purchases?
- `SQL, query:`

#### Query Business questions No. 1 & 4
```sql
WITH CTE1 AS(		
SELECT 		
Product_Category, Transaction_ID,	
COUNT(DISTINCT Transaction_ID) AS Total_Transactions,		
ROUND(AVG(Quantity),2) AS Quantity_On_Average,		
MAX(Product_Price) AS Maximum_Price,		
COUNT(DISTINCT CASE WHEN Returns = 10 THEN Transaction_ID END) AS The_Return,		
COUNT(DISTINCT CASE WHEN Returns = 0 THEN Transaction_ID END) AS Cancel,		
COUNT(DISTINCT CASE WHEN Churn = 1 THEN Transaction_ID END) AS Churn,		
COUNT(DISTINCT CASE WHEN Churn = 0 THEN Transaction_ID END) AS Retain		
FROM all_ext		
GROUP BY Product_Category		
),		
Percentage AS(		
SELECT		
Product_Category, Transaction_ID, Total_Transactions,		
Quantity_On_Average,		
Maximum_Price,		
ROUND(100 * The_Return / SUM(Total_Transactions) OVER(),2) AS Return_rate,		
ROUND(100 * Cancel / SUM(Total_Transactions) OVER(),2) AS Cancel_Return_rate,		
ROUND(100 * Churn / SUM(Total_Transactions) OVER(),2) AS Churn_rate,		
ROUND(100 * Churn / SUM(Total_Transactions) OVER(),2) AS Retain_rate		
FROM CTE1		
)		
SELECT 		
Product_Category, Total_Transactions,
Quantity_On_Average,		
Maximum_Price,		
Return_rate,		
Cancel_Return_rate,		
Churn_rate,		
Retain_rate		
FROM Percentage		
GROUP BY Product_Category		
ORDER BY Total_Transactions DESC;
```
**Output query SQL**
- The highest number of purchases by Product category
![The highest number of purchases by Product category](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/SQL-P.1.png)

### [Back to Business question No.4](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#business-question-4)


  - `Tableau` :
    
![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Tableau-P%2C1C.png)

***Interpretation***
Electronics was the main  contributor to transaction volume, with total 62.630  transactions and 170.1 M Purchase amount.
However the difference was minimal compared with clothing , which recorded 62.581 transactions,  only about 1%  lower.
Given relatively  similar transaction volumes, The level of product price and quantity on average for each product 
category were similar , about $500 and 3.

***Business question 2:***
   - Are age and gender related to purchase volume and frequency?
     
**`Pandas`**

- Creating variable bins dan labels to group age of buyers.
  ````python
     bins = [17,28,39,50,60,70]
     labels =  ['18 - 28 years',
                '29 - 39 years',
                '40 - 50 years',
                '51 - 60 years',
                '61 - 70 years']
     print(bins)
     print(labels)
  ````
- Applying "bins" & "labels" to new column : ex_data['Age_Group]
       ````python
       ex_data['Age_Group'] = pd.cut(ex_data['Age'],
                              bins=bins,
                              labels=labels)
       ````

- Creating function to analyze The number, average, sum Purchase behavior by Age and gender.
   ````python
       def relate (a,b,c,d,e):
         size1 = a.groupby(b)[[c,d,e]].size()
         mean1 = np.round(a.groupby(b)[[c,d,e]].mean(),2)
         sum1 = np.round(a.groupby(b)[[c,d,e]].sum(),2)

         print(f"the number of each column by {b}, {c},{d},{e} on average: {size1}")
         print(f"{b} related to purchase behavior, {c},{d},{e} on average: {mean1}")
         print(f"{b} related to purchase behavior, {c},{d},{e} in amount: {sum1}")
  ````
- Identifying Size,Mean ,Sum,columns relationship :'Product_Price','Quantity','Total_Purchase_Amount' by Age_group.
  
     - relate (ex_data,'Age_Group','Product_Price','Quantity','Total_Purchase_Amount')
- output :
  ![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/PY-P.2B.png)

Identifying Size, Mean, Sum columns relationship :'Product_Price','Quantity','Total_Purchase_Amount' by Gender.
    - relate (ex_data,'Gender','Product_Price','Quantity','Total_Purchase_Amount')
- Output :
  ![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/PY-P.2C.png)


#### **`SQL, query`**

```sql
WITH CTE1 AS(					
    SELECT 					
        Returns, Churn,	Transaction_ID,	Customer_ID,	
        CASE
    			WHEN Age >= 18 AND Age <= 28 THEN '18 - 28 Years'
    			WHEN Age >= 29 AND Age <= 39 THEN '29 - 39 Years'
    		 	WHEN Age >= 40 AND Age <= 50 THEN '40 - 50 Years'
    			WHEN Age >= 51 AND Age <= 60 THEN '51 - 60 Years'
    			WHEN Age >= 61 AND Age <= 70 THEN '61 - 70 Years'	
     			WHEN Age >= 71 THEN 'Above 70 Years'
        ELSE 'Unknown'
        END AS Age_Group,
		COUNT(DISTINCT Transaction_ID)-2 AS Of_Transactions,
        COUNT(DISTINCT CASE WHEN Returns = 10 THEN Transaction_ID END) AS Of_Returns_Trans,	
        COUNT(DISTINCT CASE WHEN Churn = 1 THEN Transaction_ID END) AS Of_Churn_Trans,
        COUNT(DISTINCT CASE WHEN Returns = 10 AND Churn = 1 THEN Transaction_ID END) AS Of_Trans_Returns_Churn,
        COUNT(DISTINCT Customer_ID)-1 AS Of_Customers,
        COUNT(DISTINCT CASE WHEN Returns = 10 THEN Customer_ID END) AS Of_Returns_Cust,
        COUNT(DISTINCT CASE WHEN Churn = 1 THEN Customer_ID END) AS Of_Churn_Cust,		
        COUNT(DISTINCT CASE WHEN Returns = 10 AND Churn = 1 THEN Customer_ID END) AS Of_Cust_Returns_Churn		
FROM all_ext					
GROUP BY Age_Group					
),					
CTE2 AS(					
    SELECT        					
    Returns,Churn,Transaction_ID,Customer_ID,Age_Group,					
    Of_Transactions,Of_Returns_Trans,Of_Churn_Trans, Of_Trans_Returns_Churn,					
    ROUND(Of_Trans_Returns_Churn/SUM(Of_Transactions)OVER() *100,2) AS Percentage_Trans_Returns_Churn,					
    Of_Customers, Of_Returns_Cust,Of_Churn_Cust,Of_Cust_Returns_Churn,					
    ROUND(Of_Cust_Returns_Churn/SUM(Of_Customers) OVER() *100,2) AS Percentage_Cust_Returns_Churn					
FROM CTE1					
)					
SELECT Age_Group,					
       Of_Transactions,
       Of_Returns_Trans,	
       Of_Churn_Trans
       Percentage_Trans_Returns_Churn,	
       Of_Customers,		
       Of_Returns_Cust,
       Of_Churn_Cust,
       Percentage_Cust_Returns_Churn			
FROM CTE2					
GROUP BY Age_Group;					
```
- Output:

![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/SQL-P%2C2D.png)

***Interpretation***

Generating  125.6 K ,342.7 M  and 377K in  total  transactions, purchase amount and quantity respectively, the male 
customers contributed much more.
meanwhile the female customers generated 0.6%,  1.2 % and  0.8 % less with  124.3 K, 338 M and 334K  . 
The customer aged  18 - 28  accounted for the largest share of total  transaction, representing 21 % ( 53.4 K) 
transactions and also made up
21% (10.5 K)  of total customer base, however the highest total purchase amount came from group aged 40 -50 with 140 M , 
slightly higher than the other age group,   which recorded approximately  129 - 139 M.


***Business question 3:***

What is the average quantity purchased by customers?

Picture from Business Question 2,  Identifying Size, Mean, Sum columns relationship :'Quantity' by Gender.

![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/PY-P.2C.png)

***Interpretation***

Purchase quantity was consistent across genders, both male and female customers averaging 3 items per transactions.

- [Back to content](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#content)

#### 'Returns and Churn rates'

#### ***Business question 4:***

- Which product category has the highest return and churn rates?

**`SQL, query is similar with Business question no.1`**

[SQL query](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#query-business-questions-no-1--4)

- Output:
  
![The highest number of purchases by Product category](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/SQL-P.1.png)

***Interpretation***

Interestingly, category with the highest number of  transactions practically had nearly  loyal customers.
for example, electronics generated  a 10.18% and a 5.02 % in Return and Churn rate, meanwhile Clothing  despite having fewer transactions, generating a10.12 %  Return rate and slighly  higher a 5.04% Churn rate.
It suggests  the post-purchase customer experience may play a significant role in customer retention.

***Business question 5:***

Is the payment method related to returns and churn?

**`Pandas`**

 - Creating function to analyze payment method related to returns and churn

````python
   def re_ch (b,c):
         percentage = np.round(pd.crosstab(ex_data[b], ex_data[c],normalize='index')* 100,2)
         print(f"Percentage '%' of related {b} to {c}:\n {percentage} ")
  
   re_ch('Churn','Payment_Method')
   re_ch('Returns','Payment_Method')
````


- Output:

![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/PY-P.6.png)
![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/PY-P.6B.png)

***Interpretation: ***
Paypal and Credit Card transactions  suggested  higher Return  and Churn rates than Cash payments.
Flexible payment option may encourage  impulse purchases  potentially leading  to more Returns.


***Business question 6:***

Is gender related to returns and churn?

- Identifying gender related to returns and churn.

```` python
  re_ch('Returns','Gender')
  re_ch('Churn','Gender')
````

- Output:

  ![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/PY-P.7.png)

  ![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/PY-P.7B.png)

  
***Interpretation: ***
The male customers had slightly higher return rates than the female customers,
they   may be somewhat more likely to returns, following  an unsatisfactory experience.
Despites both  groups  showed a relatively similar Churn rate of around 50%.

- [Back to content](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#content)

#### ***Business question 7:***

 What is the correlation between number of customers, returns, and churn?

 ***SQL, query-1***

 ```sql
  WITH CustomerSummary AS (
    SELECT 
        Customer_ID,
        COUNT(CASE WHEN Returns = 10 THEN 1 END) AS total_return,
        MAX(CASE WHEN Churn = 1 THEN 1 ELSE 0 END) AS is_churn
    FROM all_ext
    GROUP BY Customer_ID
)
SELECT 
    COUNT(CASE WHEN total_return = 2 AND is_churn = 1 THEN 1 END) AS customer_2x_return_churn,
    COUNT(CASE WHEN total_return = 3 AND is_churn = 1 THEN 1 END) AS customer_3x_return_churn,
    COUNT(CASE WHEN total_return > 3 AND is_churn = 1 THEN 1 END) AS customer_more_than_3x_return_churn
FROM CustomerSummary;
```


#### `SQL, query-2 for Business questions No. 7 & 8`

```sql
WITH CTE1 AS(					
            SELECT 					
            Returns, Churn, Transaction_ID, Customer_ID,		
    		CASE			
    			WHEN Age >= 18 AND Age <= 28 THEN '18 - 28 Years'		
    			WHEN Age >= 29 AND Age <= 39 THEN '29 - 39 Years'		
    		 	WHEN Age >= 40 AND Age <= 50 THEN '40 - 50 Years'		
    			WHEN Age >= 51 AND Age <= 60 THEN '51 - 60 Years'		
    			WHEN Age >= 61 AND Age <= 70 THEN '61 - 70 Years'		
     			WHEN Age >= 71 THEN 'Above 70 Years'		
       			ELSE 'Unknown'		
    		END AS Age_Group,			
    	    COUNT(DISTINCT Transaction_ID)-2 AS Of_Transactions,				
	    COUNT(DISTINCT CASE WHEN Returns = 10 THEN Transaction_ID END) AS Of_Returns,			
	    COUNT(DISTINCT CASE WHEN Returns = 0 AND Returns <> '' THEN Transaction_ID END) AS Of_Cancel_Returns,			
    	    COUNT(DISTINCT CASE WHEN Churn = 1 THEN Transaction_ID END) AS Of_Churn,			
            COUNT(DISTINCT CASE WHEN Churn = 0 THEN Transaction_ID END) AS Of_Retained,					
            COUNT(DISTINCT CASE WHEN Returns = 10 AND Churn = 1 THEN Transaction_ID END) AS Of_Returns_Churn					
    					
FROM all_ext			
GROUP BY Age_Group			
),			
CTE2 AS(		
    SELECT			
    Returns,Churn,Transaction_ID,Age_Group,					
    Of_Transactions,Of_Returns,			
    ROUND(Of_Returns / SUM(Of_Transactions) OVER() *100,2) AS Of_Returns_rate,			
    Of_Cancel_Returns,					
    ROUND(Of_Cancel_Returns / SUM(Of_Transactions) OVER() *100,2) AS Cancel_Return_rate,				
    Of_Churn,	
    ROUND(Of_Churn / SUM(Of_Transactions) OVER() *100,2) AS Of_Churn_rate,					
    Of_Retained,			
    ROUND(Of_Retained / SUM(Of_Transactions) OVER() *100,2) AS Retained_rate,					
    Of_Returns_Churn,	
    ROUND(Of_Returns_Churn/SUM(Of_Transactions)OVER() *100,2) AS Returns_Churn_rate					
FROM CTE1				
)				
SELECT  
        Age_Group,					
	Of_Transactions,			
        Of_Returns,				
        Of_Returns_rate,				
        Of_Cancel_Returns,				
        Cancel_Return_rate,				
	Of_Churn,	
        Of_Churn_rate,		
        Of_Retained,			
        Retained_rate,		
        Of_Returns_Churn,			
        Returns_Churn_rate			
FROM CTE2
GROUP BY Age_Group;
```

- Output of SQL, query-1 :

  ![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/SQL-P.8.png)

  - Output of SQL, query-2 :
 
  ![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/SQL-P.8B.png)

***Interpretation: ***
  
Customers with  two returns   that subsequently  led to churn had a nearly 30% higher frequency than 3 returns,
and  had 49% higher than those with 4 returns or more.
Furthermore, customers aged 18-28 had highest returns and churn rate at 8.73 % and 4.43 % respectively across all age 
groups, meanwhile  customers aged  51 - 60 had the lowest, at 7.48% and 3.65% respectively.


***Business question 8:***

Does purchase frequency by customer age  differ between churned - retained customers and customers who return product 
and those who don't ? 

***The query and the table from business question No.7  needed for analysis***
---
[SQL, query is from Business question No.7-query part-2](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#sql-query-2-for-business-questions-no-7--8)

- Output of SQL, query-2 :
 
  ![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/SQL-P.8B.png)

***Analysis with Tableau***

![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/P.8%20Tableau.png)


  ***Interpretation: ***

Customer aged 18 - 28 recorded the highest returns at 8.73% and 8.56% ,
while aged 51 - 60  had the lowest at 7.48% and 7,52% , returns  and  cancel-returns rate respectively.
But comparisons of customer who returns product and those who don't were not far,
under 50 had an average 4% churn rate , with a 12% percentage points higher of retention rate, nearly 17%
In contrast, customers over  50 had  a nearly 1% percentage points lower churn rate and a  nearly 15% retention rate,
Over 80% customers ever returned, were it not handled, unsatisfaction experience would have been worse.

[View Tableau visualization GIF Format-download here](https://github.com/ainunjry/Tools-project-1_E-Commerce/blob/main/Project_1.gif)

[View Tableau visualization-download here](https://github.com/ainunjry/Tools-project-1_E-Commerce/blob/main/Tableau_E-Commerce%20.pdf)

[View G-Colab-download here](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/EDA_EXTRACT_1.ipynb)


***Business question 9:***

How many at-risk  customers who had last 45-60 day-transaction to re-engage, grouped by their favourite product?

***`SQL, query'***

```sql
WITH Max_p AS(	
SELECT 	
    Customer_ID, Customer_name, Purchase_date,	
    DATE_ADD(MAX(Purchase_date), INTERVAL 1 DAY) AS A_day_after_last_PD,	
    Product_Category, 	
    COUNT(Transaction_ID) AS Total_transactions	
	
FROM all_ext	
WHERE YEAR(Purchase_date) = 2023 AND MONTH(Purchase_date) >= 7	
GROUP BY Product_Category	
),	
INTV AS(	
SELECT	
    Customer_ID, Customer_name, Purchase_date,	
    A_day_after_last_PD,	
    DATEDIFF('2023-09-13', A_day_after_last_PD) = 45 AS Int_45_days,	
    DATEDIFF('2023-09-13', A_day_after_last_PD) = 60 AS Int_60_days,	
    Product_Category, 	
    Total_transactions	
FROM Max_p	
)	
	
SELECT	
    Product_Category,Total_transactions,	
    COUNT(DISTINCT CASE WHEN Int_45_days THEN Customer_ID END) AS of_Customers_last_45_days,	
    COUNT(DISTINCT CASE WHEN Int_60_days THEN Customer_ID END) AS of_Customers_last_60_days	
FROM INTV	
GROUP BY Product_Category	
HAVING Total_transactions < (SELECT AVG(Total_transactions) FROM Max_p)	
ORDER BY of_Customers_last_60_days DESC;
```

- Output :

  ![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/P.9-SQL.png)

  
***Interpretation: ***

From this table,  42  at-risk customers who potentially led to churn  in clothing category with 1 transaction  for the last 60 days recorded ( the maximum purchase date was on Sep 13 2023),
and over  at-risk 45 customers  in books and electronics products for the last 45 days.



- [Back to content](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#content)

  
	
#### Key Insight

Satisfaction customer  rate zero in on to accumulative qualified transactions for loyal customer.
The highly Returns customers  represented   87 %  rate based the overall customers.
Customers satisfaction  appeared to be more  easily eroded, with  2 returns , potentially leading to churn.
The young adult customers may require more attention,  making  it important to prioritise their higher returns and churn-
related to complaints.

Intensifying enhancement and dive-in more information,
such as fabric qualification,  competitive price, clarity and alignment rate to item description on store front or whistlist
compared with received items and delivery, reducing Returns rate  that led to churn.
For those at risk customers with 45 - 60-day-transactions, re-engagement highly needed, by cashback promo,
win -  back voucher,  a whist list - discount promo, and etc.

- [Back to content](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#content)


  











  
    












        
  









 

  

  





   


              
       
   











