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
- By looking at beginning of the dataset, I see many rows have NaN values for these columns so I have removed them first using "how = 'all'" parameter to ensure the row gets removed only when all of these column values are NaN.: 'year','manufacturer','model','condition','cylinders','fuel','odometer','title_status','transmission','drive','size','type','paint_color'
- There are 426812 rows remaining now. I have performed following tasks for cleaning and analysis:
-   Dropped columns which are not useful in prediction: 'id' and 'VIN'
-   Analyzed the total null values in each column
-     'size' value is missing for 306293 records so this won't provide much value in prediction.
-   Analyzed if region and state column represent the same value and confirmed they don't.
-   'year' had float value so converted into int.
-   'odometer' values range from 0.0 to 10000000.0. To standardize the value, I used scaler.fit_transform.
-   As most of the columns are non-numeric, analyzed unique value in categorial columns and found these columns have limited values and would be good candidate for get_dummies function with linearregression: title_status, fuel, type, transmission, condition, year, model, manufacturer.
### Modeling
I have tried LinearRegression with different combination of features. As the null values exist in many columns, I removed the rows from the dataset with null values for selected features only to keep the database size relatively bigger for each model (compare to removing all rows where any of the columns was null for all remaining columns).
For each model, I split the data into train and test after removing nulls for appropriate features and found mean squared error (mse) for each model.
At the end, I found which model had least mse to find better model = combination of features which has higher effect on the price.
### Evaluation

### Deployment
