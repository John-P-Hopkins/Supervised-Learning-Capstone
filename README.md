# Building a Regression Model for the Price of Bitcoin

## Overview

Data was obtained from Kaggle and can be obtained at [this link](https://www.kaggle.com/datasets/mczielinski/bitcoin-historical-data).

- The dataset consists of data from select bitcoin exchanges for the time period of Jan 2012 to March 2021, with minute-to-minute updates of OHLC (Open, High, Low, Close), Volume in BTC and indicated currency (USD), and weighted bitcoin price.
- Financial technical analysis models are outside the scope of this notebook.
- The following notebook is structured as follows:
  - **Data Cleaning:** The dataset was explored to identify missing data, outliers, feature distribution, etc.
  - **Data Exploration:** The dataset was explored to discover relationships and features using statistical analysis and data visualization.
  - **Feature Engineering:** Relevant features were selected. The target variable is "Close," and the independent variables are "Gap" (difference between the high & low price), "Volume_BTC," and "Volume_Currency."
  - **Modeling:** Models were built and tested both visually and statistically for goodness of fit.

The initial dataset consisted of 8 columns and 4,857,377 rows.

Approximately 26% of the data was missing, prompting the decision to drop them from the dataset.

Some column names contained special characters, which were renamed to avoid processing issues.

The timestamps were in Unix time, so they were converted to date-time format, and the dataframe was indexed using timestamps.

No duplicate values were found.

Due to the extensive amount of data and resource limitations, a six-month time frame was chosen for the study: from 2020-07-03 00:00:00 to 2021-01-03 00:00:00.

The resulting dataset contained 260,884 rows and 7 columns indexed by date-time.

## Data Cleaning

The initial dataset consisted of 8 columns and 4,857,377 rows.

Approximately 26% of the data was missing, which led to the decision to drop them from the dataset.

Some column names contained special characters, which were renamed to avoid processing issues.

The timestamps were in Unix time, so they were converted to date-time format, and the dataframe was indexed using timestamps.

No duplicate values were found.

Due to the extensive amount of data and resource limitations, a six-month time frame was chosen for the study: from 2020-07-03 00:00:00 to 2021-01-03 00:00:00.

The resulting dataset contained 260,884 rows and 7 columns indexed by date-time.

The data was winsorized (capped) to remove outliers.

All price variables were one-way winsorized by 5% of the highest value, and volume variables were winsorized by 15% of the highest value, reducing the number of outliers to 0% as determined using the IQR method.

## Distribution and Selection of Features

Visually, the features were right-skewed (positively skewed) with a trimodal appearance for the price variable and a bimodal appearance for the volume variables.

None of the indicators were statistically normally distributed (all p-values are < 0.05) based on the Jarque-Bera & Shapiro-Wilk tests.

Several methods of transformation were evaluated; however, none were able to produce a Gaussian (Normal) distribution of data. The decision was made to stay with the winsorized data due to the clear right skew, and for financial data, it's more appropriate to obtain performance estimations based on skewness.

The data showed high multicollinearity between variables (Open, High, Low, and Weighted_Price).

The variable Volume_BTC showed a low-level positive correlation with the target variable at 0.21 and no strong multicollinearity.

The variable Volume_Currency showed a low-level positive correlation with the target at 0.38 and no strong multicollinearity.

Feature engineering was used to create one other variable, "Gap" (High - Low), which had a mid-level positive correlation with the target at 0.56 and no strong multicollinearity.

The target feature is "Close," and the independent features are "Volume_BTC," "Volume_Currency," and "Gap."

## Models and Summary

| Model             | R^2   | MAE   | MSE       | RMSE  | MAPE   |
| ----------------- | ----- | ----- | --------- | ----- | ------ |
| Bagged Regressor  | 0.59  | 2,123 | 7,898,903 | 2,810 | 0.2%   |
| Linear Regression | 0.60  | 2,111 | 7,795,125 | 2,792 | 15.6%  |
| KNN               | 0.90  | 624   | 2,001,152 | 1,415 | 4.5%   |
| Gradient Boost    | 0.91  | N/A   | 1,738,679 | N/A   | 4.7%   |
| Random Forest     | 0.90  | 442   | 1,908,673 | 1,382 | 3.1%   |

- The Random Forest Regression model is the best fit for the data.

- The volume of Bitcoin was the most important feature at approximately 0.6.

- Bitcoin's market is still highly volatile, resulting in large price swings. This is evident from the error values of the model.

- With a Random Forest Regression model, there is no fixed forecasting horizon. Given the 6 months' worth of data on 1-minute timestamps, a short-term horizon would be prudent for Bitcoin forecasting.
