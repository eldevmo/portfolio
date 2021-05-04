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

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify8.PNG" alt="shopify8.PNG">

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

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify9.png" alt="shopify9.png">
<img src="{{ site.url }}{{ site.baseurl }}/images/shopify10.png" alt="shopify10.png">

shop_id 	revenue 	uniqueCustomerCount 	Revenue_Per_Visitor<br>
0 	1 	       13588 	       42 	              323.523810<br>
1 	2 	       9588   	51 	              188.000000<br>
2 	3 	       14652  	48     	       305.250000<br>
3 	4      	13184  	45     	       292.977778<br>
4 	5 	       13064 	       44 	              296.909091<br>
5 	6      	22627   	53            	426.924528<br>
6 	7      	12208  	47 	              259.744681<br>
7 	8      	11088 	       42            	264.000000<br>
8 	9 	       13806  	56            	246.535714<br>
9 	10 	       17612  	48 	              366.916667<br>
10 	11     	17480 	       48     	       364.166667<br>
11 	12 	       18693 	       47            	397.723404<br>
12 	13 	       21760  	56            	388.571429<br>
13 	14 	       14036  	54            	259.925926<br>
14 	15 	       16065        	49 	              327.857143<br>
15 	16 	       11076  	41            	270.146341<br>
16 	17 	       17600   	48            	366.666667<br>
17 	18     	17472  	47            	371.744681<br>
18 	19     	20538  	60 	              342.300000<br>
19 	20     	13081 	       47            	278.319149<br>
20 	21     	14200   	41            	346.341463<br>
21 	22     	13140  	44            	298.636364<br>
22 	23     	17472   	52            	336.000000<br>
23 	24 	       17640 	       52            	339.230769<br>
24 	25 	       11180 	       45            	248.444444<br>
25 	26 	       16720   	47            	355.744681<br>
26 	27 	       18083   	52            	347.750000<br>
27 	28 	       13776 	       43 	              320.372093<br>
28 	29 	       19234  	50            	384.680000<br>
29 	30 	       16524  	48 	              344.250000<br>
30 	31 	       12642  	47            	268.978723<br>
31 	32 	       7979   	39            	204.589744<br>
32 	33     	15051 	       39            	385.923077<br>
33 	34     	11712  	45            	260.266667<br>
34 	35     	17056 	       50            	341.120000<br>
35 	36     	12740   	48            	265.416667<br>
36 	37 	       16330 	       44             	371.136364<br>
37 	38     	13680   	34            	402.352941<br>
38 	39 	       10988  	35            	313.942857<br>
39 	40     	14168 	       46            	308.000000<br>
40 	41 	       14986   	53            	282.754717<br>
41 	42     	22176 	       30 	              739.200000<br>
43 	44     	10224 	       34 	              300.705882<br>
44 	45     	15620  	50 	              312.400000<br>
45 	46           	14940   	43            	347.441860<br>
46 	47 	       12180  	44            	276.818182<br>
47 	48 	       9711 	       37            	262.459459<br>
48 	49     	14835         49            	302.755102<br>
49 	50 	       17756 	       40             	443.900000<br>
50 	51     	16643   	43             	387.046512<br>
51 	52     	12994 	       40            	324.850000<br>
52 	53 	       14560  	60            	242.666667<br>
53 	54 	       13832  	48 	              288.166667<br>
54 	55 	       15732  	45            	349.600000<br>
55 	56 	       8073   	34            	237.441176<br>
56 	57 	       15729   	50            	314.580000<br>
57 	58 	       15042  	51            	294.941176<br>
58 	59 	       21538 	       54            	398.851852<br>
59 	60 	       16461  	43            	382.813953<br>
60 	61     	17222  	44            	391.409091<br>
61 	62 	       13280 	       39            	340.512821<br>
62 	63 	       15368 	       50            	307.360000<br>
63 	64     	11704   	42      	       278.666667<br>
64 	65 	       17864  	49            	364.571429<br>
65 	66     	16583 	       46            	360.500000<br>
66 	67 	       10087 	       35            	288.200000<br>
67 	68 	       11968 	       44            	272.000000<br>
68 	69     	15851  	55            	288.200000<br>
69 	70 	       20241  	56            	361.446429<br>
70 	71 	       21320  	62             	343.870968<br>
71 	72 	       14240  	42            	339.047619<br>
72 	73 	       19470  	50            	389.400000<br>
73 	74     	11628 	       37             	314.270270<br>
74 	75     	10112  	40            	252.800000<br>
75 	76     	13485        	39 	              345.769231<br>
76 	77     	14040  	49            	286.530612<br>
77 	78 	       0 	       0 	              NaN<br>
78 	79     	17738  	50            	354.760000<br>
79 	80 	       13485  	41             	328.902439<br>
80 	81     	22656 	       54            	419.555556<br>
81 	82     	14691  	40            	367.275000<br>
82 	83 	       10449   	39 	              267.923077<br>
83 	84     	20196 	       53 	              381.056604<br>
84 	85     	11524  	34             	338.941176<br>
85 	86     	14430  	46            	313.695652<br>
86 	87 	       15198   	48            	316.625000<br>
87 	88 	       17776 	       45            	395.022222<br>
88 	89     	23128  	56            	413.000000<br>
89 	90 	       19758   	45            	439.066667<br>
90 	91 	       17600  	49            	359.183673<br>
91 	92 	       6840    	40            	171.000000<br>
92 	93 	       12654   	53            	238.754717<br>
93 	94     	13400 	       43 	              311.627907<br>
94 	95 	       12432 	       35 	              355.200000<br>
95 	96 	       16830   	47             	358.085106<br>
96 	97     	15552   	42            	370.285714<br>
97 	98     	14231 	       53 	              268.509434<br>
98 	99 	       18330  	49            	374.081633<br>
99 	100 	       8547 	       36 	              237.416667<br>


### Question 2

a. How many orders were shipped by Speedy Express in total?

       SELECT COUNT(OrderID) AS OrderCount FROM Orders INNER JOIN Shippers ON Shippers.ShipperID = Orders.ShipperID WHERE Shippers.ShipperName = 'Speedy Express';

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify4.PNG" alt="shopify4.PNG">

b. What is the last name of the employee with the most orders?

       SELECT TOP 1 LastName, COUNT(OrderID)
       from Orders
       inner join Employees ON Employees.EmployeeID = Orders.EmployeeID
       GROUP BY Employees.EmployeeID, Employees.LastName
       ORDER BY COUNT(OrderID) DESC

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify5.PNG" alt="shopify5.PNG">

c. What product was ordered the most by customers in Germany?

       SELECT TOP 1 Products.ProductID, Products.ProductName, SUM(OrderDetails.Quantity) AS NumberOfProductsSold
       from Products, OrderDetails, Orders, Customers
       where Products.ProductID = OrderDetails.ProductID
       and OrderDetails.OrderID = Orders.OrderID
       and Customers.CustomerID = Orders.CustomerID
       and Customers.Country = 'Germany'
       GROUP BY Products.ProductID, Products.ProductName
       ORDER BY SUM(OrderDetails.Quantity) DESC

<img src="{{ site.url }}{{ site.baseurl }}/images/shopify6.PNG" alt="shopify6.PNG">

