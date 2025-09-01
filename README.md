# Temperature Forecast
Temperature Forecast is a time series forecasting task aimed at predicting the daily temperatures using 10 years worth of recorded daily temperatures. The dataset used for this project contains two columns:

1) every day from 1981-01-01 to 1990-12-31
   
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

This was a good starting place. Next, Auto Regressive Intergrated Moving Average (ARIMA), Seasonal Auto Regressive Intergrated Moving Average (SARIMA) and Prophet from Meta models over at Python

# 2) ARIMA and SARIMA
ARIMA (Autoregressive Integrated Moving Average) models are statistical tools for time series forecasting that analyze past values, but they are best for non-seasonal data. SARIMA (Seasonal Autoregressive Integrated Moving Average) models extend ARIMA by incorporating an additional seasonal component to account for repeating patterns like monthly or yearly cycles, making them suitable for data with seasonality, such as hotel occupancy rates or monthly temperature data. For this dataset, it woould be best to use SARIMA due the daily seasonality being present but to highlight differences both models will be utilized. This dataset contains only two columns hence there's no need to worry about exogenous factors or correlation of variables. 

<img width="1189" height="790" alt="image" src="https://github.com/user-attachments/assets/1018630e-ccdc-4c1b-9144-9928d3e58269" />

Before introducing ARIMA and SARIMA, it would be a good idea to see how our baseline models perform. These models include: 

a) Naive

b) SeasonalNaive

c) Historical Average

d) Moving Average

A simple plot reveals: 

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

The SeasonalNaive baseline model has a lower mean absolute error than Excel's forecast meaning its more accurate. This is great as another benchmark to use for our next model. 

Now onto to ARIMA and SARIMA.

The ARIMA and SARIMA (by adding seasonality to the ARIMA model) is gotten from the StatsForecast library.

<img width="1366" height="503" alt="image" src="https://github.com/user-attachments/assets/9c9f9017-fe29-4d42-97f6-3c1a481c6814" />

The mean absolute error would be the only metric used to measure the model's accuracy. 

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>metric</th>
      <th>ARIMA</th>
      <th>SARIMA</th>
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
      <td>2.284992</td>
      <td>2.10385</td>
      <td>3.9</td>
      <td>2.727477</td>
      <td>0.746939</td>
      <td>1.628571</td>
    </tr>
  </tbody>
</table>
</div>

<img width="1742" height="361" alt="image" src="https://github.com/user-attachments/assets/7eab8157-e853-4d3c-bf27-62f2954feab1" />

So the SARIMA model has the same MAE score as the Excel Forecast Sheet model. 

Cross Validation will now be applied to further ensure these models work on newer data. 

# Cross Validation 

Cross-validation is a statistical technique used to assess a model's performance and its ability to generalize to new, unseen data. It works by repeatedly splitting the dataset into training and testing sets, allowing the model to be trained on one part and validated on another, a process that is iterated to provide a more reliable performance estimate. The primary goal of cross-validation is to prevent overfitting, ensuring the model doesn't just "memorize" the training data but can adapt to real-world data. In time series forecasting, cross validation can be achieved introducing a cutoff on the time series and data before this cutoff is used an the training section and after the cut off is what the model is validated on. The cutoff will be set on 1990-11-05. The result of the cross validation technique is:

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>unique_id</th>
      <th>ds</th>
      <th>cutoff</th>
      <th>y</th>
      <th>SeasonalNaive</th>
      <th>ARIMA</th>
      <th>SARIMA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>temp_series</td>
      <td>1990-11-06</td>
      <td>1990-11-05</td>
      <td>18.3</td>
      <td>14.9</td>
      <td>12.798342</td>
      <td>13.048451</td>
    </tr>
    <tr>
      <th>1</th>
      <td>temp_series</td>
      <td>1990-11-07</td>
      <td>1990-11-05</td>
      <td>19.2</td>
      <td>14.8</td>
      <td>12.347443</td>
      <td>12.453626</td>
    </tr>
    <tr>
      <th>2</th>
      <td>temp_series</td>
      <td>1990-11-08</td>
      <td>1990-11-05</td>
      <td>15.4</td>
      <td>15.4</td>
      <td>12.191058</td>
      <td>12.319272</td>
    </tr>
    <tr>
      <th>3</th>
      <td>temp_series</td>
      <td>1990-11-09</td>
      <td>1990-11-05</td>
      <td>13.1</td>
      <td>11.8</td>
      <td>11.990521</td>
      <td>11.966940</td>
    </tr>
    <tr>
      <th>4</th>
      <td>temp_series</td>
      <td>1990-11-10</td>
      <td>1990-11-05</td>
      <td>11.5</td>
      <td>13.0</td>
      <td>12.031094</td>
      <td>11.730668</td>
    </tr>
  </tbody>
</table>
</div>

The cross validation plot results to:

<img width="1791" height="361" alt="image" src="https://github.com/user-attachments/assets/8e7da855-e2b0-48ab-b088-6ab679fd3f48" />

The Mean Absolute Error (MAE), Mean Absolute Percentage Error (MAPE), Mean Squared Error (MSE), Root Mean Squared Error (RMSE) and Symmetric Mean Absolute Percentage Error (SMAPE) will be the metrics used to measure the performance of the cross validation:

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>metric</th>
      <th>SeasonalNaive</th>
      <th>ARIMA</th>
      <th>SARIMA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>mae</td>
      <td>2.789286</td>
      <td>2.235611</td>
      <td>2.183218</td>
    </tr>
    <tr>
      <th>1</th>
      <td>mape</td>
      <td>0.209902</td>
      <td>0.160493</td>
      <td>0.157526</td>
    </tr>
    <tr>
      <th>2</th>
      <td>mse</td>
      <td>12.917857</td>
      <td>7.764852</td>
      <td>7.333071</td>
    </tr>
    <tr>
      <th>3</th>
      <td>rmse</td>
      <td>3.594142</td>
      <td>2.786548</td>
      <td>2.707964</td>
    </tr>
    <tr>
      <th>4</th>
      <td>smape</td>
      <td>0.101092</td>
      <td>0.084093</td>
      <td>0.081926</td>
    </tr>
  </tbody>
</table>
</div>

# 3) PROPHET
The Prophet time series model, from Meta, decomposes a time series into trend, yearly, weekly, and daily seasonality, and holiday effects, and is particularly useful for data with strong seasonal patterns, missing data, and outliers. When future values for a year are predicted, we get a plot like so:

<img width="990" height="590" alt="image" src="https://github.com/user-attachments/assets/31224ebd-b4b6-43f6-b202-7cd6078ff763" />

<img width="886" height="889" alt="image" src="https://github.com/user-attachments/assets/dcf4e522-fce7-45c5-a5ad-29206b185091" />

# Cross Validation

When cross validation is then applied using the same cutoff used earlier, the resulting plot is like so: 

<img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/b1891f9c-8ec4-472b-912c-ce0e6a5f23e1" />

The perfomance metrics of the first five rows of this cross validation are:

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>horizon</th>
      <th>mse</th>
      <th>rmse</th>
      <th>mae</th>
      <th>mape</th>
      <th>mdape</th>
      <th>smape</th>
      <th>coverage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5 days</td>
      <td>17.844728</td>
      <td>4.224302</td>
      <td>3.426022</td>
      <td>0.199002</td>
      <td>0.192404</td>
      <td>0.229538</td>
      <td>0.6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6 days</td>
      <td>13.879526</td>
      <td>3.725524</td>
      <td>3.017434</td>
      <td>0.223897</td>
      <td>0.192404</td>
      <td>0.225933</td>
      <td>0.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7 days</td>
      <td>4.965177</td>
      <td>2.228268</td>
      <td>1.685708</td>
      <td>0.154633</td>
      <td>0.085757</td>
      <td>0.142033</td>
      <td>0.8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8 days</td>
      <td>3.433108</td>
      <td>1.852865</td>
      <td>1.304682</td>
      <td>0.131484</td>
      <td>0.076658</td>
      <td>0.115399</td>
      <td>0.8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9 days</td>
      <td>3.974670</td>
      <td>1.993657</td>
      <td>1.530976</td>
      <td>0.145994</td>
      <td>0.085757</td>
      <td>0.131222</td>
      <td>0.8</td>
    </tr>
  </tbody>
</table>
</div>

The last five rows are:

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>horizon</th>
      <th>mse</th>
      <th>rmse</th>
      <th>mae</th>
      <th>mape</th>
      <th>mdape</th>
      <th>smape</th>
      <th>coverage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>47</th>
      <td>52 days</td>
      <td>4.674090</td>
      <td>2.161964</td>
      <td>1.452697</td>
      <td>0.131337</td>
      <td>0.044491</td>
      <td>0.113133</td>
      <td>0.8</td>
    </tr>
    <tr>
      <th>48</th>
      <td>53 days</td>
      <td>4.875450</td>
      <td>2.208042</td>
      <td>1.591150</td>
      <td>0.141638</td>
      <td>0.078920</td>
      <td>0.122909</td>
      <td>0.8</td>
    </tr>
    <tr>
      <th>49</th>
      <td>54 days</td>
      <td>1.178939</td>
      <td>1.085790</td>
      <td>0.940395</td>
      <td>0.070272</td>
      <td>0.078920</td>
      <td>0.067081</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>50</th>
      <td>55 days</td>
      <td>0.810778</td>
      <td>0.900432</td>
      <td>0.804597</td>
      <td>0.056935</td>
      <td>0.064717</td>
      <td>0.055798</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>51</th>
      <td>56 days</td>
      <td>1.529449</td>
      <td>1.236709</td>
      <td>1.164728</td>
      <td>0.084802</td>
      <td>0.078920</td>
      <td>0.081682</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>

The Mean Absolute Squared Error will also be included:
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mase</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.403013</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.429223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.429314</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.438160</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.501666</td>
    </tr>
  </tbody>
</table>
</div>

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mase</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>47</th>
      <td>1.827655</td>
    </tr>
    <tr>
      <th>48</th>
      <td>1.974522</td>
    </tr>
    <tr>
      <th>49</th>
      <td>2.056712</td>
    </tr>
    <tr>
      <th>50</th>
      <td>2.392583</td>
    </tr>
    <tr>
      <th>51</th>
      <td>2.483410</td>
    </tr>
  </tbody>
</table>
</div>

Plotting this metric results in:
<img width="846" height="525" alt="image" src="https://github.com/user-attachments/assets/143159d4-0f8a-4f91-aada-8d62b590a32e" />




