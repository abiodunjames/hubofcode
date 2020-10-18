---
title: Sales Demand Forecasting with Amazon Forecast
date: 2020-10-17 07:36:00 Z
categories:
- AWS
- Forecast
tags:
- retail
- ecommerce
canonical_url: https://medium.com/@samuelabiodun/sales-demand-forecasting-with-amazon-forecast-4ff81e6db807
image: "/uploads/forecasting.png"
author: samuel
comment: true
---

> Forecasting is a process of making predictions of the future based on past and present data and, most commonly, by analyzing trends.
> \- [Wikipedia](https://en.wikipedia.org/wiki/Forecasting#:~:text=Forecasting is the process of,at some specified future date.)

History is filled with people trying to predict the future by looking at trends and patterns. We don’t have the power to change the past, but we can control future outcomes if we know the future.

For businesses, the ability to predict the future and make informed decisions is critical to their survival.

The traditional method of generating forecasts from time series data often struggles to generate accurate predictions, especially when dealing with extensive data with irregular trends.

The ability to predict demand accurately is a critical need for retailers. They need to know how many inventory store units to have at hand to be at full stock for each product at a given time.

A low inventory level increases the risk of having a stock out, and a too-high inventory level increases the cost related to handling inventory.

Recent research shows that [43% of retailers](https://www.veeqo.com/inventory-management) admitted that they have inventory problems. Retailers have constant pressure to meet market demand. This pressure is exacerbated by the fact that consumers have many options. A customer with unmet needs will not likely return to the same store. It’s estimated that [online out-of-stocks problems cost retailers $22B in sales](https://www.retaildive.com/news/online-out-of-stocks-cost-22-billion-in-sales/528878/).

The question is, can recent advances in AI help retailers forecast demand and keep customers happy at all times?

The rapid democratization of machine learning solutions (AutoML) is empowering individuals and businesses with little to no machine learning skills to solve complex problems that previously required strong expertise in AI. With AutoML, businesses can apply machine learning to complex problems without getting buried in the complexity associated with building, training, and serving ML models at scale.

One of such offerings is Amazon Forecast.

# Amazon Forecast

Amazon forecast is a managed service that uses ML to deliver highly accurate predictions. It looks at historical data (time series data) to build forecasts. Amazon Forecast abstracts the complexity of training, building, or deploying a machine learning model. It automatically examines your data, engineers features, and generates a forecasting model.

Amazon forecast is based on the same technology used at Amazon.com. It has successful use cases from companies like [More Retail](https://www.moreretail.in/), [Anaplan](https://www.anaplan.com/), [AxiomTelecom](https://www.axiomtelecom.com/), [Omnys](https://www.omnys.com/), etc.

In this post, accompanied by a Jupyter Notebook, you’ll learn how to predict sales demand using Amazon Forecast. The workflow demonstrated in the notebook, from data preprocessing to training a predictor, can be fully automated.

# Required Steps:

- Download Jupyter Notebook
- Pre-processed dataset and upload to an s3
- Import training data
- Create a predictor
- Create a forecast
- Retrieve forecast

# Download Jupyter Notebook

To follow this tutorial, you’ll need to download my [Jupyter Notebook here](https://github.com/abiodunjames/Sales-demand-forecast/blob/master/Sales_demand_forecast.ipynb) which you can run on Amazon Sagemaker Notebook Instance. Please ensure that your notebook instance has an IAM role with AmazonForecastFullAccess policy, AmazonSageMakerFullAccess policy, and S3 Policy with a read and write access to an S3 bucket that contains the preprocessed dataset.

# Pre-processed Dataset

For this demo, I used the public [Olist public e-commerce dataset](https://www.kaggle.com/olistbr/brazilian-ecommerce) to train a predictor. The dataset can be found in this [repository](https://github.com/abiodunjames/Predicting-ecommerce-sales-forecast).

It's important to note that you can’t just feed any datasets into AWS Forecast. A dataset domain must be specified, and the dataset must conform to the structure of the domain. You can think of a dataset domain as a predefined dataset schema or format for a use case. For this example, the [RETAIL Domain](https://docs.aws.amazon.com/forecast/latest/dg/retail-domain.html) is a perfect choice.

The [RETAIL Domain](https://docs.aws.amazon.com/forecast/latest/dg/retail-domain.html) defines three required fields, the item_id (string), timestamp (timestamp) and the demand (float). This means we have to do some data pre-processing. The following code already.

- *item_id*: (string) — A unique identifier for the item or product you want to predict the demand.
- *timestamp*: (timestamp)
- *demand*: (float) — The number of sales for that item at the timestamp. This is also the target field for which Amazon Forecast generates a forecast.

The dataset has to be pre-processed to conform to our domain: RETAIL domain.

![Image for post](https://miro.medium.com/max/60/0*rnpkCdf3mce27fSX?q=20)

![Image for post](https://miro.medium.com/max/3200/0*rnpkCdf3mce27fSX)

![Image for post](https://miro.medium.com/max/60/0*hFc4k5Ou1y60i5DM?q=20)

![Image for post](https://miro.medium.com/max/3200/0*hFc4k5Ou1y60i5DM)

# Import Training Dataset

To import the training dataset, we first need to create a forecast dataset. This is where we specify information about the dataset so that AWS Forecast understands how to consume the dataset. We can use Forecast [*CreateDataset*](https://docs.aws.amazon.com/forecast/latest/dg/API_CreateDataset.html) API to achieve this as follows:

![Image for post](https://miro.medium.com/max/60/0*aHTcP-PO-FpEHRlJ?q=20)

![Image for post](https://miro.medium.com/max/3200/0*aHTcP-PO-FpEHRlJ)

Once the import job succeeded, the next step is to create a forecast using the [*CreateForecast*](https://docs.aws.amazon.com/forecast/latest/dg/API_CreateDataset.html) API.

# Create a Predictor

To create a predictor, we specify the predictor’s name, the dataset group and some other parameters. We set PerformAutoML to true which means Amazon Forecast will evaluate each algorithm and choose the one that minimizes the objective function.

![Image for post](https://miro.medium.com/max/60/0*cSsuPgSCFFoVNaOw?q=20)

![Image for post](https://miro.medium.com/max/3200/0*cSsuPgSCFFoVNaOw)

Note: The predictor will not be available for use until it’s inactive. You can check for this status using forecast [DescribePredictor](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/forecast.html#ForecastService.Client.describe_predictor) API.

# Create Forecasts

Once the predictor is active, we can create a forecast for each item used to train a predictor. In this example, I specified 3 quantiles (0.1, 0.5 and 0.9) per forecast.

![Image for post](https://miro.medium.com/max/60/1*MnF61ZIRdljxhGXKSS4K9g.png?q=20)

![Image for post](https://miro.medium.com/max/5816/1*MnF61ZIRdljxhGXKSS4K9g.png)

# Retrieve Forecasts

You can either retrieve a forecast through the console or by using the [CreateForecastExportJob](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/forecast.html#ForecastService.Client.create_forecast_export_job) API to export the complete forecast into an S3 bucket.

![Image for post](https://miro.medium.com/max/60/0*FtHBSEGgJmeACtdt?q=20)

![Image for post](https://miro.medium.com/max/3200/0*FtHBSEGgJmeACtdt)

Demand forecast for product `moveis_decoracao` from September 1st to September 17th

![Image for post](https://miro.medium.com/max/60/0*QTukwVvF8Lnqn6H2?q=20)

![Image for post](https://miro.medium.com/max/3200/0*QTukwVvF8Lnqn6H2)

Demand forecast for product `pet_shop` from September 1st to September 17th

# Conclusion

We have seen one use-case of Amazon Forecast in retail and how businesses can apply it to save costs derived from keeping too high or too low inventory. I hope this sparks new ideas as you embark on building your own solution to solve demand problems in Retail.

There’re rooms for improvement, and if you’re looking to develop a fully automated and large scale forecasting solution, I encourage you to look at the [Forecast process by applying MLOps](https://aws.amazon.com/blogs/machine-learning/building-ai-powered-forecasting-automation-with-amazon-forecast-by-applying-mlops/)

[**Jupyter Notebook**](https://github.com/abiodunjames/Sales-demand-forecast/blob/master/Sales_demand_forecast.ipynb)