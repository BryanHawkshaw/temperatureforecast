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

#1) MICROSOFT EXCEL'S FORECAST SHEET
This algorithm, often referred to as the Holt Winters method, effectively handles additive trends, seasonality and smoothing of data to provide forecasts based on historical patterns. A confidence interval of 80% was used. The resulting forecast and its statistics are:

<img width="1008" height="484" alt="image" src="https://github.com/user-attachments/assets/7172c700-2fd1-4c6a-8c3f-1745ee404fd5" />

Statistic	Value
Alpha	0.25
Beta	0.00
Gamma	0.25
MASE	0.97
SMAPE	0.21
MAE	2.10
RMSE	2.64
<img width="130" height="161" alt="image" src="https://github.com/user-attachments/assets/dfe04fcd-89f5-4395-bb86-281f1877806d" />

This was a good starting place.

