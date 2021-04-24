---
title: " Classification using kNN Algorithm"
date: 2019-07-21
tags: [Data Science]
header:
    image: "/images/projects.jpg"
excerpt: "Train kNN algorithm to distinguish the species from one another"
mathjax: "true"
---
## Data Science Certificate Project 4
### Goal
Training kNN algorithm to distinguish the species from one another.

    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns
    %matplotlib inline
    import numpy as np
    from sklearn.model_selection import train_test_split
    from sklearn.neighbors import KNeighborsClassifier

**1.**

Load the data from the file (iris.data) into the DataFrame. Set the names of columns according to the column definitions given in Data Description.

    with open('iris.data') as iris:
        content = iris.read()
        print(content)


    iris = pd.read_csv('iris.data', sep=',', 
                    header=None,
                    names=['sepal length in cm','sepal width in cm','petal length in cm','petal width in cm','class']
                    )

**2.**

Data inspection. Display the first 5 rows of the dataset and use any relevant functions that can help you to understand the data. Prepare 2 scatter plots - sepal_width vs sepal_length and petal_width vs petal_length. Scatter plots should show each class in different color (seaborn.lmplot is recommended for plotting).

    f, axes = plt.subplots(1, 2, figsize = (8,5))

    sns.swarmplot(x=iris['class'], y=iris['sepal length in cm'], data=iris, ax=axes[0])
    sns.swarmplot(x=iris['class'], y=iris['sepal width in cm'], data=iris, ax=axes[1])

    s, axes2 = plt.subplots(1, 2, figsize = (8,5))
    sns.swarmplot(x=iris['class'], y=iris['petal length in cm'], data=iris, ax=axes2[0])
    sns.swarmplot(x=iris['class'], y=iris['petal width in cm'], data=iris, ax=axes2[1])

    sns.lmplot(x='sepal length in cm', y='sepal width in cm', hue='class', data=iris)
    sns.lmplot(x='petal length in cm', y='petal width in cm', hue='class', data=iris)

    iris.head()


sepal length in cm 	sepal width in cm 	petal length in cm 	petal width in cm 	class<br>
0 	5.1 	3.5 	1.4 	0.2 	Iris-setosa<br>
1 	4.9 	3.0 	1.4 	0.2 	Iris-setosa<br>
2 	4.7 	3.2 	1.3 	0.2 	Iris-setosa<br>
3 	4.6 	3.1 	1.5 	0.2 	Iris-setosa<br>
4 	5.0 	3.6 	1.4 	0.2 	Iris-setosa

<img src="{{ site.url }}{{ site.baseurl }}/images/graph4.png" alt="graph4.png">
<img src="{{ site.url }}{{ site.baseurl }}/images/graph5.png" alt="graph5.png">
<img src="{{ site.url }}{{ site.baseurl }}/images/graph6.png" alt="graph6.png">

**3.**

Prepare the data for classification. Using the pandas operators prepare the feature variables X and the response Y for the fit. Note that sklean expects data as arrays, so convert extracted columns into arrays.

    # Before proceeding with regression, first check if there are any missing values
    iris.isnull().values.any()

    # And the size of dataframe
    iris.shape

    iris['class'] = iris['class'].astype('category').cat.codes


    # Converting extracted columns into arrays
    X= iris.loc[:, 'sepal length in cm':'petal width in cm'].to_numpy()
    Y= iris.loc[:,'class'].to_numpy()

    X
    Y

array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,<br>
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,<br>
       0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,<br>
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,<br>
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,<br>
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,<br>
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2], dtype=int8)

**4.**

Split the data into train and test using sklearn train_test_split function.

    X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size = 0.3) 

    X_train.shape

(105, 4)

**5.**

Run the fit using KNeighborsClassifier from sklearn.neighbors. 
First, instantiate the model,
Then, run the classifier on the training set.

KNN is a non-parametric and lazy learning algorithm. 
 
The model structure determined from the dataset. This will be very helpful in practice where most of the real world datasets do not 
follow mathematical theoretical assumptions.
 
The number of neighbors(K) in KNN is a hyperparameter that you need choose at the time of model building. 
 
Generally, Data scientists choose as an odd number if the number of classes is even. You can also check by generating the model on different values of k and check their performance. 

Even number of K was used since the number of classes is odd.

    neigh = KNeighborsClassifier(n_neighbors=10)

    neigh.fit(X_train, Y_train)

    neigh

KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski', metric_params=None, n_jobs=None, n_neighbors=10, p=2, weights='uniform')

**6.**

Use learning model to predict the class from features, run prediction on X from test part.

Show the accuracy score of the prediction by comparing predicted iris classes and the Y values from the test.

Comparing these two arrays (predicted classes and test Y), count the numbers of correct predictions and predictions that were wrong. 

    Y_pred = neigh.predict(X_test)

    from sklearn import metrics

    print("Accuracy:",metrics.accuracy_score(Y_test, Y_pred))

    Y_test
    Y_pred

    c = Y_test == Y_pred

    np.count_nonzero(c)

Accuracy: 0.9555555555555556<br>
43

**7.**

In this task, we want to see how accuracy score and the number of correct predictions change with the number of neighbors k. We will use the following number of neighbors k: 1, 3, 5, 7, 10, 20, 30, 40, and 50:

Generate 10 random train/test splits for each value of k.<br>
Fit the model for each split and generate predictions.<br>
Average the accuracy score for each k.<br>
Calculate the average number of correct predictions for each k as well.<br>
Plot the accuracy score for different values of k. What conclusion can you make based on the graph?

    k = [1, 3, 5, 7, 10, 20, 30, 40, 50]
    accuracy = []
    subtotal = 0
    acc_avg = 0
    acc_list_avg = []
    count = 0
    count_avg = 0
    count_list_avg = []

    for i in k:
        for j in range(9):
        
            X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size = 0.3, random_state=None)
            model = KNeighborsClassifier(n_neighbors=i)
            model.fit(X_train, Y_train)
        
            Y_pred = model.predict(X_test)
        
            accuracy.append(metrics.accuracy_score(Y_test, Y_pred))
            subtotal += metrics.accuracy_score(Y_test, Y_pred)
            
            c = Y_test == Y_pred
            count += np.count_nonzero(c)

        acc_avg = subtotal/10
        acc_list_avg.append(acc_avg)
        
        count_avg = count/10
        count_list_avg.append(count_avg)
        
        subtotal = 0
        count = 0
        
    print(acc_list_avg)
    print(count_list_avg)

    df = pd.DataFrame(data = acc_list_avg, columns = ['accuracy score'])
    df['k'] = k

    df

    plt.scatter(df['k'], df['accuracy score'], alpha = 0.7, cmap='viridis')
    plt.xlabel(df.columns.values[1])
    plt.ylabel(df.columns.values[0])

[0.868888888888889, 0.8688888888888888, 0.8800000000000001, 0.8622222222222222, 0.8800000000000001, 0.8600000000000001, 0.8333333333333334, 0.8444444444444444, 0.7955555555555558]<br>
[39.1, 39.1, 39.6, 38.8, 39.6, 38.7, 37.5, 38.0, 35.8]

<img src="{{ site.url }}{{ site.baseurl }}/images/graph7.png" alt="graph7.png">

As the result shown above, higher accuracy scores can be achieved from lower values of k which are between 1 to 10.<br>
When the k value is 10, the highest accuracy score was gained.