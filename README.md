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
-   As most of the columns are non-numeric, analyzed unique value in categorial columns and found these columns have limited values and would be good candidate for get_dummies function with linearregression: title_status, fuel, type, transmission, condition, year, model, manufacturer.
### Modeling
I have tried LinearRegression with different combination of features. As the null values exist in many columns, I removed the rows from the dataset with null values for selected features only to keep the database size relatively bigger for each model (compare to removing all rows where any of the columns was null for all remaining columns).
For each model, I split the data into train and test after removing nulls for appropriate features and found mean squared error (mse) for each model.
At the end, I found model linreg8 had least mse to find better model = combination of features which has higher effect on the price.
### Evaluation
Now we know the model linreg7 which has least mse, however the model has 6 features included in the analysis. I used co-efficients to find relationship of the feature with car price prediction. Meaning, if the feature value will increase the price or decrease it. For non-numeric features, the values re pivoted into columns so each feature value has associated co-efficient.
The coefficient values were obtained as below and looked counterintuitive, for ex, the co-efficient for year is negative value. That would mean that as year increases, the car value would go down. Which is opposite to real time scenario - newer car should sell for higher value.
- where I was expecting to see positive coefficient, it has negative (for year)
- where I was expecting higher values, it has value similar to other values of same feature (condition_like new)
- where I was expecting negative values or very small values, it has comparatively higher values (odometer)
Feature	Coefficients
0	year	-1.637639e+03
1	odometer	4.068794e-02
2	fuel_diesel	9.321370e+05
3	fuel_electric	8.592385e+05
4	fuel_gas	8.259532e+05
5	fuel_hybrid	8.174528e+05
6	fuel_other	8.174990e+05
7	transmission_automatic	1.454615e+06
8	transmission_manual	1.388243e+06
9	transmission_other	1.409423e+06
10	condition_excellent	7.469199e+05
11	condition_fair	6.810581e+05
12	condition_good	7.137084e+05
13	condition_like new	7.341433e+05
14	condition_new	7.132277e+05
15	condition_salvage	6.632230e+05
16	type_SUV	3.003477e+05
17	type_bus	2.632800e+05
18	type_convertible	3.040855e+05
19	type_coupe	3.289648e+05
20	type_hatchback	3.353794e+05
21	type_mini-van	2.828656e+05
22	type_offroad	2.979189e+05
23	type_other	3.625477e+05
24	type_pickup	5.671467e+05
25	type_sedan	3.142110e+05
26	type_truck	2.821856e+05
27	type_van	3.006154e+05
28	type_wagon	3.127321e+05

Therefore I started evaluating the features to understand why the coefficient values are completely different than expected.
i) I first checked the values from 'year' column again to ensure the values are in correct 4 digit appropriate format.
- Built a linear regression just with 'year' feature and coefficient came out very high so there was no issue with calling LinearRegression method using this feature. The coefficient is showing 42 meaning increase in each year increases the price by $42 (considering no other factors for a moment) - this means values in 'year' column is not an issue so does this mean there is an issue with combination of features I have used?
ii) Type and Transmission look similar so possible the higher multivariate coefficients are causing the issue. Type has more defined values so keeping 'type' here and removing 'transmission'. So I tried another model with Liner fuel, odometer, condition and type columns. The MSE came out as 19.6 billion - lower than all other models (Except linreg7 but I'm looking for alternate to that)
Somehow the coefficient for year is still negative and other coefficients look flat across the values (For fuel, condition) meaning they are not providing meaning information.
iii) Researching gave a clue that some feature might be highli correlated with one another. Looking at data again, year and mileage might be one of them so trying to remove 'year' as mileage has more granular information/values so keeping mileage. Type and Transmission look similar so possible the higher multivariate coefficients are causing the issue. Type has more defined values so keeping 'type' here and removing 'transmission'. Tried another Liner Regression using fuel, odometer, condition and type columns. The MSE is similar (19.6 Billion), however the odometer has very small positive co-efficient which looks odd as generally the price goes higher with less odometer value.
iv) I tried last Liner Regression using fuel, odometer, condition, manufacturer and type columns to see if manufacturer value would bring more value to prediction. The MSE came out very high (332 Trillion), however, the odometer co-efficient is in negative number which looks reasonable. However, due to much lower value of co-efficient, I will use linreg12 to make recommendations so I added a graphical representation of coefficients to help write recommendations to the business.

### Deployment
From given historical data for used car sales, a linear regression model was developed using fuel, odometer, condition and type.
1. Fuel Type Significantly Impacts Vehicle Value
- Diesel vehicles showed the strongest positive association with price.
- Electric vehicles also commanded a price premium, though substantially smaller than diesel vehicles.
- Hybrid, Other, and Gas-powered vehicles generally showed lower pricing impact relative to the baseline category.
2. Vehicle Condition Is a Major Value Driver
- Vehicles in excellent condition showed a substantially higher expected selling price and then like new, new, good and fair in order. fair seems to make the price go down.
3. Type of the car: 'pickup' trucks has significant effect on pricing. This shows some relation with higher car prices for diesel fuel type vehnicles.
- sedan, van, wagon moderately impact the price while 'truck' type has lowest value impact.
4. Even though odometer has a very small positive coefficient but it should not be interepreted that higher mileage increases ehicle value. Additional analysis including other features show negative -- coefficient meaning the increasing mileage value makes the car price go down.

High level recommendation to dealers:
- Prioritize acquisitions of pick up trucks, diesel vehicles, electric vehicle and vehicles in excellent or like-new condition.
- Take caution with Salvage cars and type 'truck'.
