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

### Data understanding and cleaning

***Python Pandas***
 1. Data Cleaning
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
     
       ![View result column Returns](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Nmber%20of%20Returns.png)
#### Interpretation :
The dataset has 250.000 rows, 14 columns, column Returns has less 48.000 missing values, some transactions
appeared had no returned items.

- [Back to content](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/README.md#content)

### 2. Exploratory Data Analysis (EDA)
   
   ***Creating a categorical atribute which splits the values,a continous data into a specified number groups.***
   - bin_x = np.linspace(min(ex_data['Quantity']), max(ex_data['Quantity']),5)
   - bin_x
   - Output : array([1., 2., 3., 4., 5.])
  
   ***Creating group as a label for bin_x variable***
   - g_name = ['Low', 'Under_rated', 'Medium', 'High']
   - g_name
   - Output : ['Low', 'Under_rated', 'Medium', 'High']
  
   ***Creating new column to label column of Quantity***
   - ex_data['Quantity_bin'] = pd.cut(ex_data['Quantity'], bin_x, labels=g_name, include_lowest=True)

   ***To visualize the data range accross the Quantity column.***
   - plt.figure(figsize=(6,4))
   - plt.bar(g_name, ex_data['Quantity_bin'].value_counts(),color = 'orange')
   - plt.xlabel('Level')
   - plt.ylabel('Amount')
   - plt.title('Quantity Bins')

   - view bar of quantity level :
    
     ![view bar of quantity level](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Level%20quantity.png)
#### Interpretation:
From categorical atribute of values, the Low group of Quantity column is The highest frequency, more than 90k.

   ***To visualize the distribution of Product price using kdeplot,and Total Purchase Amount.***
   - plt.figure(figsize = (6,4))
   - sns.kdeplot(data=ex_data, x='Product Price', fill=True, color='grey')
   - plt.title('KDE distribution of Product Price')
   - plt.xlabel('Price')
   - plt.show()
     
   - View KDE Plot:

  ![View KDE Plot](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/KDE%20Plot.png)

  
  ***Creating function to analyze statistic***
   - def statistic(c,d):
     - mean = c[d].mean()
     - median = c[d].median()
     - mode = c[d].mode()
     - skewness = c[d].skew()
     - print(f"For column {d} : mean={mean}, median={median}, mode={mode}, skewness={skewness}")

   - statistic(ex_data, 'Product Price')

     - Output :
       For column Product Price : mean=254.742724, median=255.0, mode=0    290
       Name: Product Price, dtype: int64, skewness=0.0011635058483736365
#### Interpretation:
The histogram visualized bell-shaped curve, symmetrical, and the skewness is 0, it means Product Price data distribution is normal, median and mean are relatively closed, mean could be representative of typical value.

  ***To check skewness Total Purchase Amount & Quantity column***
   - statistic(ex_data, 'Quantity')
   - print()
   - statistic(ex_data, 'Total Purchase Amount')

     - Output :
       For column Quantity : mean=3.004936, median=3.0, mode=0    4
       Name: Quantity, dtype: int64, skewness=-0.005935270784344358

       For column Total Purchase Amount : mean=2725.385196, median=2725.0, mode=0    2533
       1    4456
       Name: Total Purchase Amount, dtype: int64, skewness=-0.00210492340006921

#### Interpretation:
The mean and the median  either for Total Purchase Amount or Quantity columns are closed, nearly similar,
the skewness for both columns are 0, The mean represent typical value of these two columns.

#### **Identifying outliers by Calculating Q1, Q3, IQR, both with outliers and no outliers.**
---

- def stats(e,f):
    - Q1 = e[f].quantile(0.25)
    - Q3 = e[f].quantile(0.75)
    - IQR = Q3 - Q1
    - Lower = Q1 - 1.5 * IQR
    - Upper = Q3 + 1.5 * IQR


    - outliers = e[f][(e[f]<Lower)|(e[f]>Upper)]
    - no_outliers = e[f][(e[f]>=Lower) & (e[f]<=Upper)]

    - print(f"1.IQR, Lower limit & Upper limit of column {f} are: {IQR}, {Lower}, {Upper}")

    - if outliers.empty:
        - print(f"2.Outliers of column {f}: Not Found")
    - else:
        - print(f"2.The Outliers of column {f} : {len(outliers)}")

    - print(f"3.No-outliers of column {f} : {len(no_outliers)}")


***Identifying outliers column Product Price***
- stats(ex_data, 'Product Price')
  - Output:
     - 1.IQR, Lower limit & Upper limit of column Quantity are: 2.0, -1.0, 7.0
     - 2.Outliers of column Quantity: Not Found
     - 3.No-outliers of column Quantity : 250000
   

***Identifying outliers column Quantity***
- stats(ex_data, 'Quantity')
  - Output:
     - 1.IQR, Lower limit & Upper limit of column Quantity are: 2.0, -1.0, 7.0
     - 2.Outliers of column Quantity: Not Found
     - 3.No-outliers of column Quantity : 250000
   

***Identifying outliers column Total Purchase Amounty***
- stats(ex_data, 'Total Purchase Amount')
- Output :
   - 1.IQR, Lower limit & Upper limit of column Total Purchase Amount are: 2499.0, -2272.5, 7723.5
   - 2.Outliers of column Total Purchase Amount: Not Found
   - 3.No-outliers of column Total Purchase Amount : 250000
 
 #### Interpretation:
 It apparently seems that 3 columns [Product Price, Quantity, Total Purchase Amount]
 have no outliers. To ensure them, checking through displaying BOXPLOT chart


 ***Displaying Box plot to identify outliers column Total Purchase Amount***
 
   - ex_data['Total Purchase Amount'].plot(kind='box', figsize=(6,4), vert = False, color = 'green')
   - plt.title('The Box Plot Total Purchase')
   - plt.ylabel('Amount')
   - plt.show()

 ![Box plot](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Box%20Plot.png)
     
#### Interpretation:
From  Box Plot Chart visualization for [Total Purchase Amount] columns : No outliers found.

  #### **Correlation**
  ---

  ***Finding correlation between columns by Pearson & P-value and common code .corr()***

  Identifying correlation between columns.
  - ex_data[['Product Price','Quantity','Total Purchase Amount']].corr()
     - Output:
   
       ![Correlation](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Correlation.png)

  By Pearson & P-value.
  - from scipy import stats
  - pearson_coef,p_value = stats.pearsonr(ex_data['Churn'],ex_data['Product Price'])
  - print(f"Pearson_coef : {pearson_coef}")
  - print(f"p_value : {p_value}")

    - Output:
    - Pearson_coef : 0.0011813320220758743
    - p_value : 0.5547460669888156

  - pearson_coef_TP,p_value_TP = stats.pearsonr(ex_data['Total Purchase Amount'],ex_data['Product Price'])
  - print(f"Pearson_coef_ : {pearson_coef_TP}")
  - print(f"p_value : {p_value_TP}")

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
- SQL, query:
  
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


  - Tableau :
    
![](https://github.com/ainunjry/Portfolio1_E-commerce/blob/main/Tableau-P%2C1C.png)

***Interpretation***
Electronics was the main  contributor to transaction volume, with total 62.630  transactions and 170.1 M Purchase amount.
However the difference was minimal compared with clothing , which recorded 62.581 transactions,  only about 1%  lower.
Given relatively  similar transaction volumes, this analysis would like look into the Returns and Churn factors,
should these factors have indicated meaningful differences across categories.









 

  

  





   


              
       
   











