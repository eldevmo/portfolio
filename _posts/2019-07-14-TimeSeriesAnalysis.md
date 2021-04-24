---
title: "Financial Data Analysis Using Pandas"
date: 2019-07-14
tags: [Data Science]
header:
    image: "/images/projects.jpg"
excerpt: "Time series analysis of financial data."
mathjax: "true"
---

## Data Science Certificate Project 3
### Goal
Time series analysis of the financial data, FB, MMM, IBM and AMZN, for the last 60 month from Yahoo Finance fix_yahoo_finance library.

    # all imports and env variables
    import pandas as pd
    pd.core.common.is_list_like = pd.api.types.is_list_like
    import datetime
    import pandas_datareader.data as web

    # This line of code should work on Windows and Mac
    #%env QUANDL_API_KEY = "YOUR_API_KEY"

    # If the above line of code does not work on your system,
    # You can use this way of setting Quandl env variable
    import quandl
    quandl.ApiConfig.api_key = "YOUR_API_KEY"

    # Make sure you adjust the start and end date accordingly
    # so that the start date = today date

    start = datetime.datetime(2013, 11, 12)
    end = datetime.datetime(2018, 11, 12)

    amzn = web.DataReader('WIKI/AMZN', 'quandl', start, end)

**1.**

Using Yahoo Finance fix_yahoo_finance library.

    import numpy as np
    import pandas as pd
    import matplotlib.pyplot as plt
    %matplotlib inline

    import warnings
    warnings.filterwarnings("ignore")

    import datetime as dt
    from datetime import timedelta

    import requests

    from pandas_datareader import data as pdr
    import yfinance as yf

**2.**

Download the adjusted close prices for FB, MMM, IBM and AMZN for the last 60 months.<br>

    AMZN = yf.download('AMZN', start = '2014-07-31', end='2019-06-30')
    FB = yf.download('FB', start = '2014-07-31', end='2019-06-30')
    MMM = yf.download('MMM', start = '2014-07-31', end='2019-06-30')
    IBM = yf.download('IBM', start = '2014-07-31', end='2019-06-30')

```[*********************100%***********************]  1 of 1 downloaded```<br>
```[*********************100%***********************]  1 of 1 downloaded```<br>
```[*********************100%***********************]  1 of 1 downloaded```<br>
```[*********************100%***********************]  1 of 1 downloaded```

**3.**

Resample the data to get prices for the end of the business month. Select the Adjusted Close for each stock.

    AMZN_end = AMZN.resample('BM').mean()['Adj Close']
    FB_end = FB.resample('BM').mean()['Adj Close']
    MMM_end = MMM.resample('BM').mean()['Adj Close']
    IBM_end = IBM.resample('BM').mean()['Adj Close']

    AMZN_end

**4.**

Use the pandas autocorrelation_plot() function to plot the autocorrelation of the adjusted month-end close prices 
for each of the stocks.

Are they autocorrelated?<br>
Provide short explanation.

    plt.figure(figsize = (8,7))

    AMZNTimePlot = pd.plotting.autocorrelation_plot(AMZN_end, label = 'AMZN')
    AMZNTimePlot.set_title("An Autocorrelation Graph of the adjusted month-end close prices for AMZN, FB, MMM, and IBM")
    AMZNTimePlot.set_xlabel("Lag (Business Monthes)")

    FBTimePlot = pd.plotting.autocorrelation_plot(FB_end, label = 'FB' )
    FBTimePlot.set_xlabel("Lag (Business Monthes)")

    MMMTimePlot = pd.plotting.autocorrelation_plot(MMM_end, label = 'MMM')
    MMMTimePlot.set_xlabel("Lag (Business Monthes)")

    IBMTimePlot = pd.plotting.autocorrelation_plot(IBM_end, label = 'IBM')
    IBMTimePlot.set_xlabel("Lag (Business Monthes)")

    plt.legend(loc = 'upper right')


    plt.show()

<img src="{{ site.url }}{{ site.baseurl }}/images/graph1.png" alt="graph1.png">

For AMZN, FB, and MMM, the correlation before 15th business month is significant between adjusted closed price and time. 

For AMZN, the time period between 35th and 50th business month shows the significant values again. Other time periods do not
provide strong correlation.

For FB, the time period between 29th and 43th month shows the significant values again. Other time periods do not provide
strong correlation.

For MMM, the time period between 28th and 42th month shows the significant values again. Other time periods do not provide
strong correlation.

For IBM, only first 2 busines time monthes have strong correlation since the data line of the other time periods stays between two 
dotted lines.

In the case of Month End data, From the Autocorrelation graphes, some strong trends are observed from AMZN, FB, and MMM 
that the values are not randomly distributed. However, IBM shows only first 2 monthes have significant trend that the
values are not randomly distributed.

**5.**

Calculate the monthly returns for each stock using the "shift trick" explained in the lecture, using shift() function.
Use pandas autotocorrelation_plot() to plot the autocorrelation of the monthly returns.
Are the returns autocorrelated? Provide short explanation.

    plt.figure(figsize = (8,7))

    AMZN_return = AMZN_end/AMZN_end.shift(1)-1
    AMZN_return = AMZN_return.dropna()

    FB_return = FB_end/FB_end.shift(1)-1
    FB_return = FB_return.dropna()

    MMM_return = MMM_end/MMM_end.shift(1)-1
    MMM_return = MMM_return.dropna()

    IBM_return = IBM_end/IBM_end.shift(1)-1
    IBM_return = IBM_return.dropna()

    pd.plotting.autocorrelation_plot(AMZN_returen, label ='AMZN')
    pd.plotting.autocorrelation_plot(FB_returen, label ='FB')
    pd.plotting.autocorrelation_plot(MMM_returen, label ='MMM')
    pd.plotting.autocorrelation_plot(IBM_returen, label ='IBM')

<img src="{{ site.url }}{{ site.baseurl }}/images/graph2.png" alt="graph2.png">

For the returns, there is no such significant trend for all four companies. Therefore, in the future,
it would be beneficial to observe how stock price data is affected by other company's stock prices.
Additionally, calculating different time interval affecting how stock price, and their returns are 
affected would be nessecery.

**6.**

Combine all 4 time series (returns) into a single DataFrame, 
Visualize the correlation between the returns of all pairs of stocks using a scatter plot matrix 
(use scatter_matrix() function from pandas.plotting). Explain the results. Is there any correlation?

    all_returns = pd.concat([AMZN_return, FB_return, MMM_return, IBM_return], axis=1)

    all_returns.columns = ['AMZN return', 'FB return', 'MMM return', 'IBM return']

    all_returns_scatter = pd.plotting.scatter_matrix(all_returns, figsize = (8,7), diagonal = 'kde')

<img src="{{ site.url }}{{ site.baseurl }}/images/graph3.png" alt="graph3.png">

As it is shown, the correlation between AMZN return and FB return values is positive and steeper 
inclined slope than AMZN return versus MMM return values.

FB return and MMM return values have a positive correlation.

MMM return has a positive correlation with all three other comapnies.

IBM return only has a positive correlation with MMM return values.