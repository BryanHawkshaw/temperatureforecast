# Temperature Forecast
Temperature Forecast is a time series forecasting task aimed at predicting the daily temperatures using 10 years worth of recorded daily temperatures. The dataset used for this project contains two columns: 1) every day from 1981-01-01 to 1990-12-31 
2) temperature for respective day.

These are time series column and target column respectively. 

A preview of the dataset:

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ds</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1981-01-01</td>
      <td>20.7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1981-01-02</td>
      <td>17.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1981-01-03</td>
      <td>18.8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1981-01-04</td>
      <td>14.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1981-01-05</td>
      <td>15.8</td>
    </tr>
  </tbody>
</table>
</div>


For this project different tools and time series forecast models were used. 

# 1) MICROSOFT EXCEL'S FORECAST SHEET
Microsoft Excel has this feature called Forecast Sheet. This forecast sheet uses the AAA version of the Exponential Smoothing algorithm (placing more weight on last values in dataset to make predictions). This algorithm, often referred to as the Holt Winters method, effectively handles additive trends, seasonality and smoothing of data to provide forecasts based on historical patterns. A confidence interval of 80% was used. The resulting forecast and its statistics are:

<img width="1008" height="484" alt="image" src="https://github.com/user-attachments/assets/7172c700-2fd1-4c6a-8c3f-1745ee404fd5" />

<img width="130" height="161" alt="image" src="https://github.com/user-attachments/assets/dfe04fcd-89f5-4395-bb86-281f1877806d" />

This was a good starting place. Next, Auto Regressive Intergrated Moving Average (ARIMA), Seasonal Auto Regressive Intergrated Moving Average (SARIMA) and Prophet from Facebook models over at Python

# 2) ARIMA and SARIMA
ARIMA (Autoregressive Integrated Moving Average) models are statistical tools for time series forecasting that analyze past values, but they are best for non-seasonal data. SARIMA (Seasonal Autoregressive Integrated Moving Average) models extend ARIMA by incorporating an additional seasonal component to account for repeating patterns like monthly or yearly cycles, making them suitable for data with seasonality, such as hotel occupancy rates or monthly temperature data. For this dataset, it woould be best to use SARIMA due the daily seasonality being present but to highlight differences both models will be utilized. This dataset contains only two columns hence there's no need to worry about exogenous factors or correlation of variables. 

<img width="1189" height="790" alt="image" src="https://github.com/user-attachments/assets/1018630e-ccdc-4c1b-9144-9928d3e58269" />

Before introducing ARIMA and SARIMA, it would be a good idea to see how our baseline models perform. These models include: 
a) Naive

b) SeasonalNaive

c) Historical Average

d) Moving Average

A simple plot reveals : 

<img width="1800" height="361" alt="image" src="https://github.com/user-attachments/assets/8259ccac-7d0e-433f-ab54-68847adf3368" />


The MAE (mean absolute error) of these models are:
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>metric</th>
      <th>Naive</th>
      <th>HistoricAverage</th>
      <th>WindowAverage</th>
      <th>SeasonalNaive</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>mae</td>
      <td>3.9</td>
      <td>2.727477</td>
      <td>0.746939</td>
      <td>1.628571</td>
    </tr>
  </tbody>
</table>
</div>

