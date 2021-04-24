---
title: "Data Analysis using Pandas"
date: 2019-06-23
tags: [Data Science]
header:
    image: "/images/projects.jpg"
excerpt: "Read the data from the file into pandas DataFrame. Analyze, clean and transform the data to answer the following question:
What categories of passengers were most likely to survive the Titanic disaster?"
mathjax: "true"
---

## Data Science Certificate Project 2
### Goal
Read the data from the file into pandas DataFrame. Analyze, clean and transform the data from Kaggle website to answer the following question:

What categories of passengers were most likely to survive the **Titanic disaster**?

**1.**

What categories of passengers were most likely to survive the Titanic disaster?
The detailed explanation of the logic of the analysis

**Answer:**

People who are female from the first class and 1~9 years old were most likely to have survived.

63% of the people from the first class survived.<br>
74% of females survived.<br>
The age groups between 20~29 and 30~39 had highest number of survivors which is 43% of the TOTAL number of the people but ratio wise,61% of people between ages 1~9 had highest possibility of surviving.<br>
SibSp and Parch were analyzed but the percentages between data are not significant so these attributes were not included in the result.

**2.**

What other attributes did you use for the analysis? Explain how you used them and why you decided to use them.
Provide a complete list of all attributes used.

**Answer:**

I used Pclass, Sex, Age, SibSp, Parch.

To analyze data, we need to know which attributes(columns) are the most affecting for surviving. If the percent differences of the ratio of the data is big from a certain attribute(column) then this attribute(column) affects the death or surviving results more clearly. From this logic, the Sex, Pclass, and Age attributes(columns) affect the result the most.

**3.**

Did you engineer any attributes (created new attributes)? If yes, explain the rationale and how the new attributes were used in the analysis?
If you have excluded any attributes from the analysis, provide an explanation why you believe they can be excluded.

**Answer:**

I included the 'possibility_by_age' attribute to analyze the ratio of survivors depending on the age group. This attribute was used because we need to consider that every age group can have different physical abilities but having similar physical abilities depending on the group of the age. For example, younger people are better at running or swimming or being helped.

**4.**

How did you treat missing values for those attributes that you included in the analysis (for example, age attribute)? Provide a detailed explanation in the comments.

**Answer:**

The age, Cabin, and Embarked attributes have some missing values but only age was used in my analysis because 80% was already filled in. I didn't consider missing values for the age because I didn't think 20% was enough to change the result in my analysis.

For the Cabin attribute, only 23% of the data was filled in. This is not enough data to analyze survivors exactly.

For the Embarked attribute, I don't think it would affect the surviving rate because the ship was crashed in the middle of the Atlantic Ocean far from the ports.

**Code**

    import pandas as pd
    import numpy as np

    df = pd.read_csv('train.csv')

    df.replace(u'\xa0', np.nan, regex=True, inplace=True)

    df.info()

RangeIndex: 891 entries, 0 to 890<br>
Data columns (total 12 columns):<br>
PassengerId    891 non-null int64<br>
Survived       891 non-null int64<br>
Pclass         891 non-null int64<br>
Name           891 non-null object<br>
Sex            891 non-null object<br>
Age            714 non-null float64<br>
SibSp          891 non-null int64<br>
Parch          891 non-null int64<br>
Ticket         891 non-null object<br>
Fare           891 non-null float64<br>
Cabin          204 non-null object<br>
Embarked       889 non-null object<br>
dtypes: float64(2), int64(5), object(5)<br>
memory usage: 83.6+ KB

    # The possibility of surviving depending on the passengerclass

    df_original_pclass = df.groupby('Pclass')
    a = df_original_pclass.count()['Survived']

    g = df.groupby('Survived')

    df_survived_group = g.get_group(1)
    df_survived_group

    df_pclass = df_survived_group.groupby('Pclass')

    b = df_pclass.count()['Survived']

    c = b/a

    print(c)

    possibility_by_pclass = df[['Pclass', 'Survived']].groupby(['Pclass'], as_index=False).mean().sort_values(by='Survived', ascending=False)
    possibility_by_pclass

    # As the data shows, the first class has the highest possibility of surviving

Pclass<br>
1    0.629630<br>
2    0.472826<br>
3    0.242363<br>
Name: Survived, dtype: float64

Pclass 	Survived<br>
0 	1 	0.629630<br>
1 	2 	0.472826<br>
2 	3 	0.242363

    # The possibility depending on the gender

    df_sex = df_survived_group.groupby('Sex')
    df_sex
    df_sex.count()['Survived']

    possibility_by_sex = df[['Sex', 'Survived']].groupby(['Sex'], as_index=False).mean().sort_values(by='Survived', ascending=False)
    possibility_by_sex

    # As the data shows, the female has the higher possibility of surviving than the male

Sex 	Survived<br>
0 	female 	0.742038<br>
1 	male 	0.188908

    # The possibility of surviving depending on the Age group

    # The Age group: 1~9 years old, 10~19 years old, 20~29 years old, 
    # 30~39 years old, 40~49 years old, 50~59 years old, 60~69 years old, over 70 years old 

    count00 = 0
    count11 = 0
    count22 = 0
    count33 = 0
    count44 = 0
    count55 = 0
    count66 = 0
    count77 = 0

    count0 = 0
    count1 = 0
    count2 = 0
    count3 = 0
    count4 = 0
    count5 = 0
    count6 = 0
    count7 = 0

    for i in df['Age']:
        
        if 0 < i < 10 :
            count00 +=1
        elif 10 <= i < 20:
            count11 +=1
        elif 20 <= i < 30:
            count22 +=1
        elif 30 <= i < 40:
            count33 +=1
        elif 40 <= i < 50:
            count44 +=1
        elif 50 <= i < 60:
            count55 +=1
        elif 60 <= i < 70:
            count66 +=1
        elif 70 <= i:
            count77 +=1

    for i in df_survived_group['Age']:
        
        if 0 < i < 10 :
            count0 +=1
        elif 10 <= i < 20:
            count1 +=1  
        elif 20 <= i < 30:
            count2 +=1  
        elif 30 <= i < 40:
            count3 +=1
        elif 40 <= i < 50:
            count4 +=1
        elif 50 <= i < 60:
            count5 +=1
        elif 60 <= i < 70:
            count6 +=1
        elif 70 <= i:
            count7 +=1
            
        
    possibility_by_age = [count0/count00, count1/count11, count2/count22, count3/count33, count4/count44, count5/count55, count6/count66, count7/count77]


    print(count00, count11, count22, count33, count44, count55, count66, count77)           
    print(count0, count1, count2, count3, count4, count5, count6, count7)

    print(possibility_by_age)

    # As the data shows, the age group 1~9 years old has the highest possibility of surviving

62 102 220 167 89 48 19 7<br>
38 41 77 73 34 20 6 1<br>
[0.6129032258064516, 0.4019607843137255, 0.35, 0.437125748502994, 0.38202247191011235, 0.4166666666666667, 0.3157894736842105, 0.14285714285714285]

    # The number of survivors depending on if they have SibSp

    df_sibsp = df_survived_group.groupby('SibSp')['Survived']
    df_sibsp.count()

    possibility_by_sibsp = df[['SibSp', 'Survived']].groupby(['SibSp'], as_index=False).mean().sort_values(by='Survived', ascending=False)
    possibility_by_sibsp

    # As the data shows, the people who have 1 SibSp has the highest possibility of surviving

Parch 	Survived<br>
3 	3 	0.600000<br>
1 	1 	0.550847<br>
2 	2 	0.500000<br>
0 	0 	0.343658<br>
5 	5 	0.200000<br>
4 	4 	0.000000<br>
6 	6 	0.000000