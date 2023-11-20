# NIFTY Stock Selection : Project Overview
* Developed a stock selection model to offer quantitative investors an innovative approach.  
* Implemented model stacking using LSTM and XGBoost to transform back testing performance from losses to profits.  

## Let's get started  
Hello everyone, in this notebook, I will be using LSTM for stock selection, applying LSTM to panel data. After making predictions, I will employ model stacking by adding XGBoost into the mix. Finally, I will practically use the model to implement a simple strategy and backtest its performance. Due to the significant randomness in the stock market, the performance will not be as high as image classification accuracy, so please do not use it as an investment strategy.  

## Part 1: Data Preprocessing  
Data preprocessing is a crucial step because LSTM is a model that demands careful data handling. We need to ensure that all stocks have the same time length and no missing values. To achieve this, I'll start by selecting stocks with the same time length from the entire dataset and then removing any rows with missing values.

## Part 2: Time Series Generator  
In this step, it is quite crucial because our data structure is panel data. If we build a time series model in the usual way, it will combine different stocks into one window. So, we need to create generators for each individual stock and then merge these generators into one large batch. The train-test split should also be done based on stocks.

You might wonder why not initially create separate generators for each stock, make predictions, and then select stocks afterward. This is because I want to build a shared model, which means the model can learn from other stocks' variations during training. The advantage of this approach is that it can capture industry-related patterns, increase the amount of data available, and reduce the risk of overfitting.  

## Part 3: Build LSTM model and backtest  
After creating a large generator, we can now construct the LSTM model. Due to the relatively small amount of data, the LSTM architecture is kept simple. I used a dynamic learning rate adjustment method for optimization.

You can see the results after training and predicting. While the loss looks good, when we randomly select a stock and plot the actual and predicted returns, it's clear that the model struggles to capture the returns.

Based on these predictions, we still create a simple strategy, which is to buy stocks with relatively higher predicted returns for stock selection. As I mentioned earlier, predicting the stock market, which is highly stochastic, is challenging, and this stock selection strategy performs poorly.  
![download](https://github.com/Elvis-YAL/stock_selection_with-LSTM-and-model_stacking/assets/40426433/b4ac5c72-fee6-4b97-9e99-31f9c847f0be)
![download](https://github.com/Elvis-YAL/stock_selection_with-LSTM-and-model_stacking/assets/40426433/1a88cff5-150f-4572-93a5-a563d409a850)
![download](https://github.com/Elvis-YAL/stock_selection_with-LSTM-and-model_stacking/assets/40426433/a43be9c8-4859-4298-8c94-b9b0149c1722)

## Part 4: Model Stacking  
We can make adjustments to the strategy. Here, I'm using XGBoost for model stacking. Based on the predicted returns from LSTM, I'm employing XGBoost's ranking model to calculate which stocks have higher scores. This will be used as the stock selection strategy.  

![download](https://github.com/Elvis-YAL/stock_selection_with-LSTM-and-model_stacking/assets/40426433/c8c08ae4-78ef-4831-b228-d29db0280837)

Through model stacking, we can see an overall improvement in strategy performance.  

Finally, you might wonder if XGBoost is effective when we have only one type of LSTM-predicted returns as a variable. Therefore, we can also input the original features that were used for LSTM into XGBoost. It's important to note that when merging the data, we must ensure that future features are not included in today's prediction results.  
The results are as shown in the figure below. In fact, the overall performance has returned to its original state. Therefore, even with the addition of more variables, to some extent, it may have affected XGBoost's learning
![download](https://github.com/Elvis-YAL/stock_selection_with-LSTM-and-model_stacking/assets/40426433/552cfb80-00fa-4f6b-bf26-430469a21001)

