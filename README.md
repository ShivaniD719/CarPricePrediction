# CarPricePrediction
## Problem Statement:
Given a car dataset, understand what factors make a car more or less expensive.
### Business Understanding
There are features given related to car sales and the target variable 'price' is one of them. The task is to find what combination of features will impact the target variable 'price'. This is linear regression problem. Here are the steps I have taken here:
- select different combination of features
- split the dataset into train and test
- try linear regression
- find one with lowest mean squared error on test data.
### Data Understanding
Here are the steps I have taken to understand the data:
- read through column headers and data within each column for few rows including type of the data (text or numeric)
- remove the rows with most of the data missing as this will be redundant data
- analyze total null values for each column. This gives me an idea for what columns have more null values, need more cleaning and which columns can not be good candidate for feature selection.
- 'id' and 'VIN' columns would represent each car and should be unique and won't provide any information for prediction so I have removed them first under Data Preparation.
### Data Preparation
- There are total of 426880 records in the given dataset.
- by looking at beginning of the dataset, I see many rows have NaN values for these columns so I have removed them first using "how = 'all'" parameter to ensure the row gets removed only when all of these column values are NaN.: 'year','manufacturer','model','condition','cylinders','fuel','odometer','title_status','transmission','drive','size','type','paint_color'
- There are 426812 rows remaining now.
- Drop columns which are not useful in prediction: 'id' and 'VIN'
- Analyse the total null values in each column
-   'size' value is missing for 306293 records so this won't provide much value in prediction.
-   
### Modeling
### Evaluation
### Deployment
