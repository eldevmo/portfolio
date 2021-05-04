---
title: "Shopify Data Science Intern Challenge"
date: 2021-05-02
tags: 
header:
    image: "/images/projects.jpg"
excerpt: "Shopify Data Science Intern Challenge"
mathjax: "true"
hidden: "true"
---
## Fall 2021 Data Science Intern Challenge 
### Question 1
       import pandas as pd
       import numpy as np
       import statsmodels.api as sm
       import scipy, scipy.stats
       import matplotlib.pyplot as plt
       import seaborn as sns
       from matplotlib.pyplot import scatter


**a. Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.**

       df = pd.read_excel(r'C:\Users\Lim\Desktop\resume\2021 Winter Data Science Intern Challenge Data Set.xlsx', encoding="ISO-8859-1")
       df.describe()

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify7.png" alt="shopify7.png">

**As it is mentioned in the given file, the AOV is $3145.13**

**To check any outliers, columns are plotted**
       
       sns.set(style = "ticks")
       sns.pairplot(df)

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify1.png" alt="shopify1.png">

**It is found that there is a distinct dot where total_times is greater than 1500 and order_amount is lower than 500000**

       print(np.where((df['total_items'] > 1500) & (df['order_amount'] > 500000)))

(array([  15,   60,  520, 1104, 1362, 1436, 1562, 1602, 2153, 2297, 2835, 2969, 3332, 4056, 4646, 4868, 4882], dtype=int64),)

**It is found that these indexes are outliers**

       df.iloc[[  15,   60,  520, 1104, 1362, 1436, 1562, 1602, 2153, 2297, 2835, 2969, 3332, 4056, 4646, 4868, 4882]]

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify2.PNG" alt="shopify2.PNG">

**Drop the outliers to verify the data properly**

       df2 = df.drop([15, 60, 520, 1104, 1362, 1436, 1562, 1602, 2153, 2297, 2835, 2969, 3332, 4056, 4646, 4868, 4882])

**b. What metric would you report for this dataset?**

**I chose to report RPV (Revenue_Per_Visitor) since this gives a better idea of the overall health of a eCommerce store**
**RPV is a metric where Conversion rate and Average order value are combined, RPV = Total Revenue / Total Unique Visitors**

**Calculated Revenues for each shop to get RPV**

       revenue_list = []
       uniqueShopidCount = len(df2['shop_id'].unique())

       for i in range(1, uniqueShopidCount+1):
       revenue = df2.loc[df2['shop_id'] == i, 'order_amount'].sum()
       revenue_list.append(revenue)
       print('shop_id is', i, 'and Revenue is', revenue)

shop_id is 1 and Revenue is 13588<br>
shop_id is 2 and Revenue is 9588<br>
shop_id is 3 and Revenue is 14652<br>
shop_id is 4 and Revenue is 13184<br>
shop_id is 5 and Revenue is 13064<br>
shop_id is 6 and Revenue is 22627<br>
shop_id is 7 and Revenue is 12208<br>
shop_id is 8 and Revenue is 11088<br>
shop_id is 9 and Revenue is 13806<br>
shop_id is 10 and Revenue is 17612<br>
shop_id is 11 and Revenue is 17480<br>
shop_id is 12 and Revenue is 18693<br>
shop_id is 13 and Revenue is 21760<br>
shop_id is 14 and Revenue is 14036<br>
shop_id is 15 and Revenue is 16065<br>
shop_id is 16 and Revenue is 11076<br>
shop_id is 17 and Revenue is 17600<br>
shop_id is 18 and Revenue is 17472<br>
shop_id is 19 and Revenue is 20538<br>
shop_id is 20 and Revenue is 13081<br>
shop_id is 21 and Revenue is 14200<br>
shop_id is 22 and Revenue is 13140<br>
shop_id is 23 and Revenue is 17472<br>
shop_id is 24 and Revenue is 17640<br>
shop_id is 25 and Revenue is 11180<br>
shop_id is 26 and Revenue is 16720<br>
shop_id is 27 and Revenue is 18083<br>
shop_id is 28 and Revenue is 13776<br>
shop_id is 29 and Revenue is 19234<br>
shop_id is 30 and Revenue is 16524<br>
shop_id is 31 and Revenue is 12642<br>
shop_id is 32 and Revenue is 7979<br>
shop_id is 33 and Revenue is 15051<br>
shop_id is 34 and Revenue is 11712<br>
shop_id is 35 and Revenue is 17056<br>
shop_id is 36 and Revenue is 12740<br>
shop_id is 37 and Revenue is 16330<br>
shop_id is 38 and Revenue is 13680<br>
shop_id is 39 and Revenue is 10988<br>
shop_id is 40 and Revenue is 14168<br>
shop_id is 41 and Revenue is 14986<br>
shop_id is 42 and Revenue is 22176<br>
shop_id is 43 and Revenue is 19367<br>
shop_id is 44 and Revenue is 10224<br>
shop_id is 45 and Revenue is 15620<br>
shop_id is 46 and Revenue is 14940<br>
shop_id is 47 and Revenue is 12180<br>
shop_id is 48 and Revenue is 9711<br>
shop_id is 49 and Revenue is 14835<br>
shop_id is 50 and Revenue is 17756<br>
shop_id is 51 and Revenue is 16643<br>
shop_id is 52 and Revenue is 12994<br>
shop_id is 53 and Revenue is 14560<br>
shop_id is 54 and Revenue is 13832<br>
shop_id is 55 and Revenue is 15732<br>
shop_id is 56 and Revenue is 8073<br>
shop_id is 57 and Revenue is 15729<br>
shop_id is 58 and Revenue is 15042<br>
shop_id is 59 and Revenue is 21538<br>
shop_id is 60 and Revenue is 16461<br>
shop_id is 61 and Revenue is 17222<br>
shop_id is 62 and Revenue is 13280<br>
shop_id is 63 and Revenue is 15368<br>
shop_id is 64 and Revenue is 11704<br>
shop_id is 65 and Revenue is 17864<br>
shop_id is 66 and Revenue is 16583<br>
shop_id is 67 and Revenue is 10087<br>
shop_id is 68 and Revenue is 11968<br>
shop_id is 69 and Revenue is 15851<br>
shop_id is 70 and Revenue is 20241<br>
shop_id is 71 and Revenue is 21320<br>
shop_id is 72 and Revenue is 14240<br>
shop_id is 73 and Revenue is 19470<br>
shop_id is 74 and Revenue is 11628<br>
shop_id is 75 and Revenue is 10112<br>
shop_id is 76 and Revenue is 13485<br>
shop_id is 77 and Revenue is 14040<br>
**shop_id is 78 and Revenue is 2263800**<br>
shop_id is 79 and Revenue is 17738<br>
shop_id is 80 and Revenue is 13485<br>
shop_id is 81 and Revenue is 22656<br>
shop_id is 82 and Revenue is 14691<br>
shop_id is 83 and Revenue is 10449<br>
shop_id is 84 and Revenue is 20196<br>
shop_id is 85 and Revenue is 11524<br>
shop_id is 86 and Revenue is 14430<br>
shop_id is 87 and Revenue is 15198<br>
shop_id is 88 and Revenue is 17776<br>
shop_id is 89 and Revenue is 23128<br>
shop_id is 90 and Revenue is 19758<br>
shop_id is 91 and Revenue is 17600<br>
shop_id is 92 and Revenue is 6840<br>
shop_id is 93 and Revenue is 12654<br>
shop_id is 94 and Revenue is 13400<br>
shop_id is 95 and Revenue is 12432<br>
shop_id is 96 and Revenue is 16830<br>
shop_id is 97 and Revenue is 15552<br>
shop_id is 98 and Revenue is 14231<br>
shop_id is 99 and Revenue is 18330<br>
shop_id is 100 and Revenue is 8547<br>

       len(range(uniqueShopidCount))
       revenue_max = np.max(revenue_list)
       print(revenue_max) 
**Max. revenue is 2263800 - shop id 78 has the max. revenue**

       uniqueCustomer_78shopId = df2.loc[df2['shop_id'] == 78, 'user_id'].unique()
       print(uniqueCustomer_78shopId) 
**Unique customers are printed for shop id 78**

       countCustomer_78shopId = len(uniqueCustomer_78shopId)
       print(countCustomer_78shopId) 
**Counted number of unique customers**

       Revenue_Per_Visitor_78shopId = revenue_max/countCustomer_78shopId
       print(Revenue_Per_Visitor_78shopId) 
**RPV found is over $50306.67 which seems not right**

       df2.loc[df['shop_id'] == 78] 
**Checked the order_amount for shop id 78; it turns out that per pair of shoes, the price is over $25725 so I decided to drop shop id 78**

2263800<br>
[990 936 983 967 760 878 800 944 970 775 867 912 812 810 855 709 834 707<br>
 935 861 915 962 890 869 814 817 740 910 745 927 928 982 828 766 889 852<br>
 946 787 960 756 969 866 997 818 823]<br>
45<br>
50306.666666666664<br>

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify11.PNG" alt="shopify11.PNG">
<img src="{{ site.url }}{{ site.baseurl }}/images/shopify12.PNG" alt="shopify12.PNG">

       df3 = df2[df2.shop_id != 78] 

**Dropped shop id 78 values in the dataframe**

       shop_id_list = []
       revenue_list2 = []
       uniqueCustomer_list = []
       uniqueShopidCount2 = len(df3['shop_id'].unique())

       for i in range(1, uniqueShopidCount2+2):
       revenue = df3.loc[df3['shop_id'] == i, 'order_amount'].sum()
       uniqueCustomer = len(df3.loc[df3['shop_id'] == i, 'user_id'].unique())
       revenue_list2.append(revenue)
       shop_id_list.append(i)
       uniqueCustomer_list.append(uniqueCustomer)

       df4 = pd.DataFrame({"shop_id":shop_id_list,
                     "revenue":revenue_list2,
                     "uniqueCustomerCount":uniqueCustomer_list})

       df4 

**Created a new dataframe to calculate RPV**

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify3.PNG" alt="shopify3.PNG">

**c. What is its value?**

       Revenue_Per_Visitor = df4['revenue']/df4['uniqueCustomerCount']
       pd.set_option("max_rows", None)
       df4['Revenue_Per_Visitor'] = Revenue_Per_Visitor
       df4

**Added RPV column in the dataframe. Finally, there is no distinct data. The RPV values are found for each shop id**
**I chose to show RPV for each shop id since having this for each shop id is more accurate and able to check any outliers**

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify13.PNG" alt="shopify13.PNG">
<img src="{{ site.url }}{{ site.baseurl }}/images/shopify14.PNG" alt="shopify14.PNG">
<img src="{{ site.url }}{{ site.baseurl }}/images/shopify15.PNG" alt="shopify15.PNG">
<img src="{{ site.url }}{{ site.baseurl }}/images/shopify16.PNG" alt="shopify16.PNG">


### Question 2

a. How many orders were shipped by Speedy Express in total? - **54**

       SELECT COUNT(OrderID) AS OrderCount FROM Orders INNER JOIN Shippers ON Shippers.ShipperID = Orders.ShipperID WHERE Shippers.ShipperName = 'Speedy Express';

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify4.PNG" alt="shopify4.PNG">

b. What is the last name of the employee with the most orders? - **Peacook with 40 orders**

       SELECT TOP 1 LastName, COUNT(OrderID)
       from Orders
       inner join Employees ON Employees.EmployeeID = Orders.EmployeeID
       GROUP BY Employees.EmployeeID, Employees.LastName
       ORDER BY COUNT(OrderID) DESC

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify5.PNG" alt="shopify5.PNG">

c. What product was ordered the most by customers in Germany? **Boston Crab Meat was ordered the most in Germany with 160 sold**

       SELECT TOP 1 Products.ProductID, Products.ProductName, SUM(OrderDetails.Quantity) AS NumberOfProductsSold
       from Products, OrderDetails, Orders, Customers
       where Products.ProductID = OrderDetails.ProductID
       and OrderDetails.OrderID = Orders.OrderID
       and Customers.CustomerID = Orders.CustomerID
       and Customers.Country = 'Germany'
       GROUP BY Products.ProductID, Products.ProductName
       ORDER BY SUM(OrderDetails.Quantity) DESC

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify6.PNG" alt="shopify6.PNG">

