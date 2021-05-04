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

 	order_id 	shop_id 	    user_id 	           order_amount 	       total_items
count 	5000.000000 	5000.000000 	    5000.000000 	    5000.000000 	       5000.00000
mean 	2500.500000 	50.078800 	    849.092400 	    3145.128000 	       8.78720
std 	1443.520003 	29.006118 	    87.798982 	    41282.539349 	       116.32032
min 	1.000000 	1.000000 	    607.000000 	    90.000000 	       1.00000
25% 	1250.750000 	24.000000 	    775.000000 	    163.000000 	       1.00000
50% 	2500.500000 	50.000000 	    849.000000 	    284.000000 	       2.00000
75% 	3750.250000 	75.000000 	    925.000000 	    390.000000 	       3.00000
max 	5000.000000 	100.000000 	    999.000000 	    704000.000000 	       2000.00000

**As it is mentioned in the given file, the AOV is $3145.13**

**To check any outliers, columns are plotted**
       sns.set(style = "ticks")
       sns.pairplot(df)

<img src="{{ site.url }}{{ site.baseurl }}/images/graph6.png" alt="shopify1.png">

**It is found that there is a distinct dot where total_times is greater than 1500 and order_amount is lower than 500000**
       print(np.where((df['total_items'] > 1500) & (df['order_amount'] > 500000)))

(array([  15,   60,  520, 1104, 1362, 1436, 1562, 1602, 2153, 2297, 2835,
       2969, 3332, 4056, 4646, 4868, 4882], dtype=int64),)

**It is found that these indexes are outliers**
       df.iloc[[  15,   60,  520, 1104, 1362, 1436, 1562, 1602, 2153, 2297, 2835,
              2969, 3332, 4056, 4646, 4868, 4882]]

<img src="{{ site.url }}{{ site.baseurl }}/images/graph6.png" alt="shopify2.png">

**Drop the outliers to verify the data properly**
       df2 = df.drop([15, 60, 520, 1104, 1362, 1436, 1562, 1602, 2153, 2297, 2835,
              2969, 3332, 4056, 4646, 4868, 4882])

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

shop_id is 1 and Revenue is 13588
shop_id is 2 and Revenue is 9588
shop_id is 3 and Revenue is 14652
shop_id is 4 and Revenue is 13184
shop_id is 5 and Revenue is 13064
shop_id is 6 and Revenue is 22627
shop_id is 7 and Revenue is 12208
shop_id is 8 and Revenue is 11088
shop_id is 9 and Revenue is 13806
shop_id is 10 and Revenue is 17612
shop_id is 11 and Revenue is 17480
shop_id is 12 and Revenue is 18693
shop_id is 13 and Revenue is 21760
shop_id is 14 and Revenue is 14036
shop_id is 15 and Revenue is 16065
shop_id is 16 and Revenue is 11076
shop_id is 17 and Revenue is 17600
shop_id is 18 and Revenue is 17472
shop_id is 19 and Revenue is 20538
shop_id is 20 and Revenue is 13081
shop_id is 21 and Revenue is 14200
shop_id is 22 and Revenue is 13140
shop_id is 23 and Revenue is 17472
shop_id is 24 and Revenue is 17640
shop_id is 25 and Revenue is 11180
shop_id is 26 and Revenue is 16720
shop_id is 27 and Revenue is 18083
shop_id is 28 and Revenue is 13776
shop_id is 29 and Revenue is 19234
shop_id is 30 and Revenue is 16524
shop_id is 31 and Revenue is 12642
shop_id is 32 and Revenue is 7979
shop_id is 33 and Revenue is 15051
shop_id is 34 and Revenue is 11712
shop_id is 35 and Revenue is 17056
shop_id is 36 and Revenue is 12740
shop_id is 37 and Revenue is 16330
shop_id is 38 and Revenue is 13680
shop_id is 39 and Revenue is 10988
shop_id is 40 and Revenue is 14168
shop_id is 41 and Revenue is 14986
shop_id is 42 and Revenue is 22176
shop_id is 43 and Revenue is 19367
shop_id is 44 and Revenue is 10224
shop_id is 45 and Revenue is 15620
shop_id is 46 and Revenue is 14940
shop_id is 47 and Revenue is 12180
shop_id is 48 and Revenue is 9711
shop_id is 49 and Revenue is 14835
shop_id is 50 and Revenue is 17756
shop_id is 51 and Revenue is 16643
shop_id is 52 and Revenue is 12994
shop_id is 53 and Revenue is 14560
shop_id is 54 and Revenue is 13832
shop_id is 55 and Revenue is 15732
shop_id is 56 and Revenue is 8073
shop_id is 57 and Revenue is 15729
shop_id is 58 and Revenue is 15042
shop_id is 59 and Revenue is 21538
shop_id is 60 and Revenue is 16461
shop_id is 61 and Revenue is 17222
shop_id is 62 and Revenue is 13280
shop_id is 63 and Revenue is 15368
shop_id is 64 and Revenue is 11704
shop_id is 65 and Revenue is 17864
shop_id is 66 and Revenue is 16583
shop_id is 67 and Revenue is 10087
shop_id is 68 and Revenue is 11968
shop_id is 69 and Revenue is 15851
shop_id is 70 and Revenue is 20241
shop_id is 71 and Revenue is 21320
shop_id is 72 and Revenue is 14240
shop_id is 73 and Revenue is 19470
shop_id is 74 and Revenue is 11628
shop_id is 75 and Revenue is 10112
shop_id is 76 and Revenue is 13485
shop_id is 77 and Revenue is 14040
**shop_id is 78 and Revenue is 2263800**
shop_id is 79 and Revenue is 17738
shop_id is 80 and Revenue is 13485
shop_id is 81 and Revenue is 22656
shop_id is 82 and Revenue is 14691
shop_id is 83 and Revenue is 10449
shop_id is 84 and Revenue is 20196
shop_id is 85 and Revenue is 11524
shop_id is 86 and Revenue is 14430
shop_id is 87 and Revenue is 15198
shop_id is 88 and Revenue is 17776
shop_id is 89 and Revenue is 23128
shop_id is 90 and Revenue is 19758
shop_id is 91 and Revenue is 17600
shop_id is 92 and Revenue is 6840
shop_id is 93 and Revenue is 12654
shop_id is 94 and Revenue is 13400
shop_id is 95 and Revenue is 12432
shop_id is 96 and Revenue is 16830
shop_id is 97 and Revenue is 15552
shop_id is 98 and Revenue is 14231
shop_id is 99 and Revenue is 18330
shop_id is 100 and Revenue is 8547

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

2263800
[990 936 983 967 760 878 800 944 970 775 867 912 812 810 855 709 834 707
 935 861 915 962 890 869 814 817 740 910 745 927 928 982 828 766 889 852
 946 787 960 756 969 866 997 818 823]
45
50306.666666666664

	order_id 	shop_id 	user_id 	order_amount 	total_items 	payment_method 	created_at
160 	161 	       78 	       990 	       25725 	       1 	       credit_card 	       2017-03-12 05:56:56.834
490 	491 	       78 	       936 	       51450 	       2 	       debit 	              2017-03-26 17:08:18.911
493 	494 	       78 	       983 	       51450 	       2 	       cash 	              2017-03-16 21:39:35.400
511 	512 	       78 	       967 	       51450 	       2 	       cash 	              2017-03-09 07:23:13.640
617 	618 	       78 	       760 	       51450 	       2 	       cash 	              2017-03-18 11:18:41.848
691 	692 	       78 	       878 	       154350 	6 	       debit 	              017-03-27 22:51:43.203
1056 	1057 	       78 	       800 	       25725 	       1 	       debit 	              2017-03-15 10:16:44.830
1193 	1194 	       78 	       944 	       25725 	       1 	       debit 	              2017-03-16 16:38:25.551
1204 	1205 	       78 	       970 	       25725 	       1 	       credit_card 	       2017-03-17 22:32:21.438
1259 	1260 	       78 	       775 	       77175 	       3 	       credit_card 	       2017-03-27 09:27:19.843
1384 	1385 	       78 	       867 	       25725 	       1 	       cash 	              2017-03-17 16:38:06.279
1419 	1420 	       78 	       912 	       25725 	       1 	       cash 	              2017-03-30 12:23:42.551
1452 	1453 	       78 	       812 	       25725 	       1 	       credit_card 	       2017-03-17 18:09:54.089
1529 	1530 	       78 	       810 	       51450 	       2 	       cash 	              2017-03-29 07:12:01.466
2270 	2271 	       78 	       855 	       25725  	1 	       credit_card   	2017-03-14 23:58:21.635
2452 	2453   	78 	       709 	       51450  	2 	       cash          	2017-03-27 11:04:04.363
2492 	2493   	78 	       834    	102900 	4 	       debit          	2017-03-04 04:37:33.848
2495 	2496   	78     	707    	51450  	2 	       cash 	              2017-03-26 04:38:52.497
2512 	2513   	78     	935    	51450 	       2 	       debit         	2017-03-18 18:57:13.421
2548 	2549   	78     	861    	25725  	1 	       cash           	2017-03-17 19:35:59.663
2564 	2565 	       78 	       915    	77175   	3      	debit          	2017-03-25 01:19:35.410
2690 	2691   	78     	962 	       77175  	3 	       debit          	2017-03-22 07:33:25.104
2773 	2774 	       78     	890 	       25725  	1 	       cash          	2017-03-26 10:36:43.445
2818 	2819   	78 	       869    	51450   	2 	       debit          	2017-03-17 06:25:50.921
2821 	2822   	78 	       814 	       51450  	2 	       cash 	              2017-03-02 17:13:25.271
2906 	2907 	       78 	       817 	       77175  	3      	debit  	       2017-03-16 03:45:46.089
2922 	2923   	78 	       740 	       25725   	1      	debit 	              2017-03-12 20:10:58.008
3085 	3086   	78     	910     	25725   	1 	       cash          	2017-03-26 01:59:26.748
3101 	3102   	78     	855    	51450  	2 	       credit_card 	       2017-03-21 05:10:34.147
3151 	3152 	       78 	       745 	       25725 	       1 	       credit_card    	2017-03-18 13:13:07.198
3167 	3168   	78 	       927     	51450 	       2      	cash 	              2017-03-12 12:23:07.516
3403 	3404 	       78 	       928 	       77175 	       3      	debit         	2017-03-16 09:45:04.544
3440 	3441 	       78     	982    	25725   	1 	       debit 	              2017-03-19 19:02:53.732
3705 	3706   	78     	828    	51450  	2 	       credit_card 	       2017-03-14 20:43:14.502
3724 	3725 	       78 	       766 	       77175  	3      	credit_card 	       2017-03-16 14:13:25.868
3780 	3781 	       78     	889    	25725  	1      	cash          	2017-03-11 21:14:49.542
4040 	4041 	       78     	852    	25725  	1      	cash           	2017-03-02 14:31:11.566
4079 	4080   	78 	       946 	       51450 	       2      	cash 	              2017-03-20 21:13:59.919
4192 	4193   	78     	787     	77175  	3 	       credit_card   	2017-03-18 09:25:31.863
4311 	4312 	       78     	960 	       51450  	2      	debit 	              2017-03-01 03:02:10.223
4412 	4413 	       78 	       756    	51450  	2 	       debit         	2017-03-02 04:13:38.530
4420 	4421   	78     	969 	       77175  	3 	       debit 	              2017-03-09 15:21:34.551
4505 	4506   	78     	866 	       25725   	1 	       debit 	              2017-03-22 22:06:00.804
4584 	4585   	78     	997 	       25725 	       1      	cash          	2017-03-25 21:48:43.570
4715 	4716 	       78 	       818 	       77175   	3 	       debit 	              2017-03-05 05:10:43.633
4918 	4919 	       78     	823    	25725  	1      	cash          	2017-03-15 13:26:46.262

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

<img src="{{ site.url }}{{ site.baseurl }}/images/graph6.png" alt="shopify3.png">

**c. What is its value?**

       Revenue_Per_Visitor = df4['revenue']/df4['uniqueCustomerCount']
       pd.set_option("max_rows", None)
       df4['Revenue_Per_Visitor'] = Revenue_Per_Visitor
       df4
**Added RPV column in the dataframe. Finally, there is no distinct data. The RPV values are found for each shop id**
**I chose to show RPV for each shop id since having this for each shop id is more accurate and able to check any outliers**

       shop_id 	revenue 	uniqueCustomerCount 	Revenue_Per_Visitor
0 	1 	       13588 	       42 	              323.523810
1 	2 	       9588   	51 	              188.000000
2 	3 	       14652  	48     	       305.250000
3 	4      	13184  	45     	       292.977778
4 	5 	       13064 	       44 	              296.909091
5 	6      	22627   	53            	426.924528
6 	7      	12208  	47 	              259.744681
7 	8      	11088 	       42            	264.000000
8 	9 	       13806  	56            	246.535714
9 	10 	       17612  	48 	              366.916667
10 	11     	17480 	       48     	       364.166667
11 	12 	       18693 	       47            	397.723404
12 	13 	       21760  	56            	388.571429
13 	14 	       14036  	54            	259.925926
14 	15 	       16065        	49 	              327.857143
15 	16 	       11076  	41            	270.146341
16 	17 	       17600   	48            	366.666667
17 	18     	17472  	47            	371.744681
18 	19     	20538  	60 	              342.300000
19 	20     	13081 	       47            	278.319149
20 	21     	14200   	41            	346.341463
21 	22     	13140  	44            	298.636364
22 	23     	17472   	52            	336.000000
23 	24 	       17640 	       52            	339.230769
24 	25 	       11180 	       45            	248.444444
25 	26 	       16720   	47            	355.744681
26 	27 	       18083   	52            	347.750000
27 	28 	       13776 	       43 	              320.372093
28 	29 	       19234  	50            	384.680000
29 	30 	       16524  	48 	              344.250000
30 	31 	       12642  	47            	268.978723
31 	32 	       7979   	39            	204.589744
32 	33     	15051 	       39            	385.923077
33 	34     	11712  	45            	260.266667
34 	35     	17056 	       50            	341.120000
35 	36     	12740   	48            	265.416667
36 	37 	       16330 	       44             	371.136364
37 	38     	13680   	34            	402.352941
38 	39 	       10988  	35            	313.942857
39 	40     	14168 	       46            	308.000000
40 	41 	       14986   	53            	282.754717
41 	42     	22176 	       30 	              739.200000
42 	43     	19367  	52            	372.442308
43 	44     	10224 	       34 	              300.705882
44 	45     	15620  	50 	              312.400000
45 	46           	14940   	43            	347.441860
46 	47 	       12180  	44            	276.818182
47 	48 	       9711 	       37            	262.459459
48 	49     	14835         49            	302.755102
49 	50 	       17756 	       40             	443.900000
50 	51     	16643   	43             	387.046512
51 	52     	12994 	       40            	324.850000
52 	53 	       14560  	60            	242.666667
53 	54 	       13832  	48 	              288.166667
54 	55 	       15732  	45            	349.600000
55 	56 	       8073   	34            	237.441176
56 	57 	       15729   	50            	314.580000
57 	58 	       15042  	51            	294.941176
58 	59 	       21538 	       54            	398.851852
59 	60 	       16461  	43            	382.813953
60 	61     	17222  	44            	391.409091
61 	62 	       13280 	       39            	340.512821
62 	63 	       15368 	       50            	307.360000
63 	64     	11704   	42      	       278.666667
64 	65 	       17864  	49            	364.571429
65 	66     	16583 	       46            	360.500000
66 	67 	       10087 	       35            	288.200000
67 	68 	       11968 	       44            	272.000000
68 	69     	15851  	55            	288.200000
69 	70 	       20241  	56            	361.446429
70 	71 	       21320  	62             	343.870968
71 	72 	       14240  	42            	339.047619
72 	73 	       19470  	50            	389.400000
73 	74     	11628 	       37             	314.270270
74 	75     	10112  	40            	252.800000
75 	76     	13485        	39 	              345.769231
76 	77     	14040  	49            	286.530612
77 	78 	       0 	       0 	              NaN
78 	79     	17738  	50            	354.760000
79 	80 	       13485  	41             	328.902439
80 	81     	22656 	       54            	419.555556
81 	82     	14691  	40            	367.275000
82 	83 	       10449   	39 	              267.923077
83 	84     	20196 	       53 	              381.056604
84 	85     	11524  	34             	338.941176
85 	86     	14430  	46            	313.695652
86 	87 	       15198   	48            	316.625000
87 	88 	       17776 	       45            	395.022222
88 	89     	23128  	56            	413.000000
89 	90 	       19758   	45            	439.066667
90 	91 	       17600  	49            	359.183673
91 	92 	       6840    	40            	171.000000
92 	93 	       12654   	53            	238.754717
93 	94     	13400 	       43 	              311.627907
94 	95 	       12432 	       35 	              355.200000
95 	96 	       16830   	47             	358.085106
96 	97     	15552   	42            	370.285714
97 	98     	14231 	       53 	              268.509434
98 	99 	       18330  	49            	374.081633
99 	100 	       8547 	       36 	              237.416667


### Question 2

a. How many orders were shipped by Speedy Express in total?

       SELECT COUNT(OrderID) AS OrderCount FROM Orders INNER JOIN Shippers ON Shippers.ShipperID = Orders.ShipperID WHERE Shippers.ShipperName = 'Speedy Express';

<img src="{{ site.url }}{{ site.baseurl }}/images/graph6.png" alt="shopify4.png">

b. What is the last name of the employee with the most orders?

       SELECT TOP 1 LastName, COUNT(OrderID)
       from Orders
       inner join Employees ON Employees.EmployeeID = Orders.EmployeeID
       GROUP BY Employees.EmployeeID, Employees.LastName
       ORDER BY COUNT(OrderID) DESC

<img src="{{ site.url }}{{ site.baseurl }}/images/graph6.png" alt="shopify5.png">

c. What product was ordered the most by customers in Germany?

       SELECT TOP 1 Products.ProductID, Products.ProductName, SUM(OrderDetails.Quantity) AS NumberOfProductsSold
       from Products, OrderDetails, Orders, Customers
       where Products.ProductID = OrderDetails.ProductID
       and OrderDetails.OrderID = Orders.OrderID
       and Customers.CustomerID = Orders.CustomerID
       and Customers.Country = 'Germany'
       GROUP BY Products.ProductID, Products.ProductName
       ORDER BY SUM(OrderDetails.Quantity) DESC

<img src="{{ site.url }}{{ site.baseurl }}/images/graph6.png" alt="shopify6.png">

