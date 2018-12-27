# Canada-Bankruptcy-Prediction
time series project

# Modeling & Forecasting Canadian National Bankruptcy Rates

## Problem Description :
Understanding how national bankruptcy rates change over time is important for risk management and it has always been of interest to national banks, insurance companies, credit-lenders, and politicians etc. The goal of this report is to precisely and accurately forecast monthly bankruptcy rates for Canada.

In this report, we will exploit the monthly statistics of Canadian bankruptcy rate, unemployment rate, population as well as house price index, for the period of January 1987 to December 2014, and construct a model for bankruptcy rates. We will use the constructed model to predict the monthly data for bankruptcy rates from January 2015 to  December 2017.

## Data Preprocessing
(1) Train-Validation Data Split
In order to find the best model with the lowest RMSE (root mean squared error), a train-validation data
split was carried out on the original train.csv dataset. The validation dataset was used to measure and
rank the model performance based on its RMSE value. We determined the split to be at the end of year 2013
- thus the training set contains 324 data points, and the validation set contains 12 data points.
(2) Box-Cox Transformation
Box-Cox tranformation is useful in adjusting the non-constant variation in data. The bankruptcy data has
shown certain degree of inflated variance over time which was mitigated by the Box-Cox transformation on
the entire dataset.

## Models
We tried variety of modeling approaches including:

Univariate Time Series Model
<ul>
<li> ARIMA/SARIMA (Box-Jenkins Approach) </li>
<li> Exponential Smoothing (Holt-Winters Approach) </li>
</ul>
Multivariate Time Series Model
<ul>
<li> ARIMAX/SARIMAX </li>
<li> VAR/VARX (Vector Autoregression) </li>
</ul>


## Evaluation Metrics
Since our goal is to have as accurate and precise forecasting results as possible, we choose to use the minimum RMSE as our model selection metrics. 

In order to test our models, we split the original data (from January 1987 to December 2014) into two parts, one set is the training part (from January 1987 to December 2012) to train our models, and the other set is the validation part (from January 2013 to December 2014) to test the predictive accuracy (RMSE) of the models.

## Forecasting 
Based on the minimum RMSE value, we selected the optimal model and used this model to predict the following 3 years' bankruptcy rate. Our forecasting plot shows that, despite some seasonal fluctuations, there will be a general decreasing trend of the bankruptcy rate for the year 2015 to 2017.

 ![alt text](https://github.com/JinghuiZhao/Canada-Bankruptcy-Prediction/blob/master/Screen%20Shot%202018-12-26%20at%205.05.51%20PM.png)
 

