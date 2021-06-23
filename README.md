## Twitter's Effects On Currency Exchange RateTwitters Effects On Currency Exchange Rate

#### Note
Because of github there might be problems with opening the ipynb files. If you face with this problem you can use [Viewer](https://nbviewer.jupyter.org/ "Viewer") to open files.

### Introduction
Predicting the currency rate or predicting that it will go upwards or downwards is a well-known problem.  Nowadays social media is perfectly representing the public sentiment and opinion about current events.

According to Efficient Market Hypothesis all available new and even hidden information is reflected in market prices because investors, being rational, want to maximize profits. 

Because of the lying unpredictability in news and current events, currency rates follow a random pattern. 

According to Twitter there are **500 million** tweets a day in 2020. So if researchers or investors can analyze those data, it will be very valuable information.

### Developed Approach
To forecast USD/TRY data needed find exchange rate history.  Python tools like forex-python is provides historical data day by day. This time interval is too much to make a good prediction because there are too much changes can happen between USD/TRY in a day. So founded a historical data file minute by minute for every year. I resampled it to one hour and added the previous hours rate as a feature and combined all the years into [Sum_Last.csv](https://github.com/aburak256/Currency-with-Twitter/blob/main/Sum_Last.csv "Sum_Last.csv") file. 

Decomposition of dataset starting from 2019:
![image](https://user-images.githubusercontent.com/34773124/123096847-2f321300-d438-11eb-925d-9a94f7f64c32.png)

Stationarity is an important characteristic of time series. Stationary dataset is properties do not depend on the time at which the series is observed. Time series with trends are not stationary but in order to analyse this I made the Augmented Dickey-Fuller test. In the Augmented-Dickey Fuller test, if the p-value is bigger than 0.05, it fails to reject the null hypothesis. But our dataset has 0.9 p value so its not stationary. To get better results with our models, I added a difference feature with a lag to partially fix my dataset's dependence on time. This file is [Last.csv](https://github.com/aburak256/Currency-with-Twitter/blob/main/Last.csv "Last.csv")

### Collecting Twitter Data
Twitter API has limitations in terms of collecting Tweets because they don't allow you to collect Tweets that older than 1 week, so I needed to find another tool. To collect Twitter data, I found a tool called [GetOldTweets3](https://github.com/Jefferson-Henrique/GetOldTweets-python "GetOldTweets3"). It is an open-source project published by [Jefferson Henrique](https://github.com/Jefferson-Henrique "Jefferson Henrique") on GitHub. You can add time, language, location, etc. filters to collect the Tweets with this tool. But on October 12 2020 Twitter released its Twitter API v2 and with this update GetOldTweets3 tool was broken. After this update I decided to use a tool called [Twint](https://github.com/twintproject/twint "Twint"). 

To collect the Tweets with the Twint tool, I wrote a code that reads the USD/TRY CSV file and finds the Tweets with specific keywords in the last selected hour. But Twitter API and twint has some problems here again because Twitter blocks your guest token if you make too many requests in a short time interval. I tried to add delay but it didn't work so I have to collect the Twitter data week by week to prevent the token error. Also, there are some loadout failures while the code works because of twint tool so I wrote a code that eliminates the duplicates and concatenates the CSV files that contain Tweets. 

Twitter data contains number of likes, number of replies and number of tweets that contains specified keyword.

These files are the ones that begins with Twint. For example if you want to open the code that collects Tweets that contains "turkish lira" you can open [Twint_turkish_lira.ipynb](https://github.com/aburak256/Currency-with-Twitter/blob/main/Twint_turkish_lira.ipynb "Twint_turkish_lira.ipynb"). 

You can see the number of Tweets that contain keyword "Dolar" with Turkish Language Filter day by day starting from 2014 to the end of 2019:

![image](https://user-images.githubusercontent.com/34773124/123101092-5c80c000-d43c-11eb-9be1-4e35b58e43c2.png)

### Predictions
Used 6 models and ensembled versions of these models to obtain different results to compare the effects of Twitter data. These methods are:
- Bayesian Regression
- Lasso
- Random Forest
- XGBoost
- Lightgbm
- LSTM

|   | LSTM  |  Lightgbm  | XGBoost  |
| ------------ | ------------ | ------------ | ------------ |
| LSTM| 1   |  0.208 | 0.031  |
|  Lightgbm | 0.208  | 1  |  0.521 |
|  XGBoost |  0.031  | 0.521  |  1 |


In order to obtain the Twitter's effect, used two different datasets: With Tweets data and without one.

Used 4 different error measurement method
Mean Absolute Error
Root Mean Squared Error
Mean Absolute Percentage Error
R^2 coefficient of determination

Results = Errors of Twitter - Errors without Twitter

One of the results, MAE Metric (Negative signed results are the ones that Twitter dataset has more accuracy):
![image](https://user-images.githubusercontent.com/34773124/123103077-57bd0b80-d43e-11eb-932d-ce5eb8a3fbe8.png)

### Evaluating The Results / T-Test
T-test compares two averages and tells you if they are different from each other. In other words, it lets you know if those differences could have happened by chance.
If p-value is smaller than 0.05, we can reject the null hypothesis and say that there is a meaningful difference between the two results

![image](https://user-images.githubusercontent.com/34773124/123103530-c8642800-d43e-11eb-985f-fbc9081ba324.png)

p-values are smaller than 0.05. So we can say that there is a meaningful difference with the results of two datasets







