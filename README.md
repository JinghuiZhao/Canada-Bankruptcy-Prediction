
# Description of the Project
This project aims to build a time series model to forecast monthly bankruptcy rates in Canada for the period from January 2015 to December 2017. The dataset we will work on contains historical bankruptcy rates, unemployment rates, population, and housing price index from January 1987 to December 2014. Please see graph below for the historical monthly bankruptcy rates. In this report, we will explore different time series modeling approaches, such as Holt-Winters , SARIMA , SARIMAX and VAR , to find the optimal model with the best predictive accuracy which is measured by the RMSE (Root Mean Squared Error). 

![alt text](https://github.com/JinghuiZhao/Canada-Bankruptcy-Prediction/blob/master/Screen%20Shot%202019-03-10%20at%2012.53.23%20PM.png)

We adopted 2 ways of modelling: univariate modeling and multivariate modeling.

A univariate modeling approach considers only the historical data of the variable being modeled and does not take any external information into consideration. In our case, we will only use only historical bankruptcy rates data to train our univariate models, such as Holt-Winters and SARIMA (under Box-Jenkins framework). Holt-Winters is the simplest approach because it does not rely on any statistical assumptions. Under the Holt-Winters approach, future observations are predicted by performing a smoothing on the previous observations in the time series. SARIMA is the most common type of modeling under Box-Jenkins framework.

On the other hand, a multivariate modeling approach considers external data. There are two common types of multivariate models: SARIMAX, and Vector Autoregression (VAR). If one treats the external information as exogenous, meaning the external variables have a uni-directional influence on the response, then a SARIMAX should be employed. On the contrary, if one treats the external variables as endogenous, meaning the external variable and response have mutual influence, then a Vector Autoregression model should be employed. In our case, with bankruptcy rates as the response variable, we have considered unemployment rates, housing price index, and population for multivariate modeling.
After exploring all of the above models, SARIMAX was found to be the optimal model.


# Modeling Approach
## Data Preprocessing
```(1) Train-Validation Data Split```
In order to find the best model with the lowest RMSE (root mean squared error), a train-validation data split was carried out on the original train.csv dataset. The validation dataset was used to measure and rank the model performance based on its RMSE value. We determined the split to be at the end of year 2013 - thus the training set contains 324 data points, and the validation set contains 12 data points.
```(2) Box-Cox Transformation```
Box-Cox tranformation is useful in adjusting the non-constant variation in data. The bankruptcy data has shown certain degree of inflated variance over time which was mitigated by the Box-Cox transformation on the entire dataset.


## Data Modeling
(1)``` Holt-Winters Model (Exponential Smoothing)```
Triple Exponential Smoothing, also known as the Holt-Winters method, is one of the many methods that can be used to forecast data points in a time series, provided that the series is “seasonal”, i.e. repetitive over some period. The objective is to predict $y_n$+ h given the observed history ${ y_1, y_2, . . ., y_n}$ of the time series. Exponential smoothing is a method of time series modeling by which we model its level, trend, and seasonality components by exponential equations. Since our data exhibits both trend and seasonal components, we employed triple exponential smoothing to model and forecast the data. We modeled the seasonal component either additively or multiplicatively, depending on how the variation in our data changes over time. Since the variation does appear to inflate over time (in a non-linear fashion), we selected multiplicative seasonality for modeling. The parameters α, β, and γ, which represent the model’s sensitivity to its level, trend, and seasonality, were found using a grid search that gave the lowest RMSE value.


(2) ```SARIMA Model Seasonal Autoregressive Integrated Moving Average```
SARIMA is widely used for modeling univariate time series with seasonality with or without trend. One can think of SARIMA model from two dimensions, first a within-season time series which can be modeled by ARIMA(p, q), then between-season time series. Since the data exhibits both trend and seasonality, we considered both trend and seasonality components. With regard to the trend component: p represents trend autoregression order, d represents ordinary (trend) difference order, and q represents trend moving average order. With regard to the seasonal component: P represents seasonal autoregressive order, D represents seasonal difference order, Q represents seasonal moving average order, m represents the number of time steps for a single seasonal period. These are all captured in the SARIMA (p,d,q)×(P, D, Q)m model. Now the question is how to determine the orders for a SARIMA model. First we used ndiffs and nsdiffs to determine the ordinary differencing and seasonal differencing order. Then we found the maximum orders, p, q, P and Q. The Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) plots of the transformed data were examined and iterated to find out the optimal model. auto.arima, a less computationally expensive method, was also employed to optimize the model. Then we compared other models with this the model produced by auto.arima and chose the best model in the end.

(3)``` SARIMAX Model```
A SARIMAX model is a SARIMA model with explanatory variables. SARIMAX model is a popular method for modeling multivariate time series. We considered multivariate time series when there exists other variables that are highly correlated with the response variable. In addition, the data on other variables have to be collected at the same frequency and for the same duration as our response variable. In our case, bankruptcy rates are highly correlated with population and housing price index (according to CCF plot) and is negatively correlated with unemployment rates. By considering a SARIMAX model, we hoped that such a model would provide more accurate forecasts than other univariate models. And SARIMAX was found to be the optimal model.

![alt text](https://github.com/JinghuiZhao/Canada-Bankruptcy-Prediction/blob/master/Screen%20Shot%202018-12-26%20at%205.05.51%20PM.png)


(4)``` VAR Model Vector Autoregression (VAR(p)) model```
An extension of the univariate autoregression model to multivariate time series data, is a system of equations whose variables are treated as endogeneous. The model consists of r equations, one for each variable, that are each autoregressions of order p. We have chosen this model to account for the relationships between predictor variables. For example, house price index and population might be influencing each other, since a larger population will boost the housing price, on the other hand, a high housing price will reduce the population in an area. Their endogeneous relationship was accounted for by the VAR model. In our case, p was chosen to be 3 based on the RMSE values.
