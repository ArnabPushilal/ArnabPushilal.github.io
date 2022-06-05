---
title: "Stock Prediction- Hero MotoCorp"
excerpt: "<br/><img src='/images/MTAN.jpeg'>"
collection: portfolio
---

## Motivation 
I wanted to explore time-series data predction from the stock prices of the company I work in.

![](/images/StockData.jpg)

It's about 20 years of data, I chose the closing price for prediction.

## Moving Average

 ### Moving Average + Window
 ![](/images/MovingAvg.jpg)
 
 We can see this just smoothenes out the data, large errors are still present
 
 ###  Difference series to account for periodic data
 ![](/images/DifferenceSeries.jpg)
 
 * Some data might be dependant on Quarter results, so I chose a period of 90 days to see if it improved my model.
 ### Moving Average + Difference 
 ![](/images/MovingAvgpluspast.jpg)
 
 ### Moving Average + Difference + Smoothening
 ![](/images/MovingAvgSmooth.jpg)
 
 ## DNN
 ![](/images/DenseNetwork.jpg)
 
 
 ## LSTM
 
  ### Picking LearningRate
  
 ![](/images/LearningRate%20(2).jpg)
   
 
 
  ### Prediction
  ![](/main/images/LSTM.jpg)

 ## Final Accuracy Table
  
 |Model |MSE| MAE |
|--- | --- | --- |
| Moving Average | 32488.284 | 143.392 |
| Difference Series |  50870.056|180.291|
| Moving Average + Difference + Smoothening|  37266.263 | 156.276|
| Dense Network | 11013.445 | 82.419 |
| LSTM | 10364.572| 80.46758|




