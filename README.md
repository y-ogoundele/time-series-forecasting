# time-series-forecasting
Times series model to forecast daily orders of a delivery company

## TL;DR
To efficiently allocate supply to demand, we generate a 2 weeks ahead order forecast.

Using a prophet model that takes temperature and media spend as extra regressors, we get an out-of-sample root mean square error of 6.7 orders, with 66.4% of observed values within the prediction interval. This is a 26% improvement on the baseline model.

Since temperature values are only available for past days in production, not future days, we use them to explain the observed variance in the training set, but we set a predicted temperature in the prediction dataset (the median of the last week in the training set).

## Why are we forecasting the daily number of orders in Berlin?
On-time delivery is primarily driven by how many couriers are available to handle the number of orders. Similarly, excellent customer support means low response times. Hence, we want to schedule sufficient staff answering customer requests. Ultimately, both are caused by the number of orders. We want to inform couriers if we’re expecting higher demand and simultaneously, we schedule sufficient support agents. When the provided demand forecast isn’t accurate both over-supplying and under-supplying can have costly consequences, e.g.:

We might staff too many support agents
Or we staff too few support agents and lose valuable customers through bad customer
experience.

## What does this notebook contain?
I broke this notebook down into several sections to make it easier to follow:

1. Inspecting the dataset
2. Exploring the dataset: univariate and bivariate distributions
3. Backtesting the models: baseline, exponential smoothing and Prophet models
4. Designing and deploying the model: model architecture and how I would deploy it

## How do we forecast the number of orders?
We create 14-days ahead forecasts, starting on each Monday in the dataset. Our models return the point prediction and the 95% prediction interval. We evaluate model out-of-sample performance using forward chained validation (or backtesting), with 4 evaluation metrics: mean absolute error, root mean squared error, bias and coverage.
