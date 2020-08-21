# DataScience
Data Science projects

## Data Science Project 1

This project evaluates the dataset for [Broward County Airbnb data](http://insideairbnb.com/get-the-data.html)

The analysis attempts to answer the questions: 
* What are the main features driving the price of a listing?
* What are the main features driving occupancy, measures with number of reviews? 
* Is there a correlation between price and occupancy (measured by number of reviews)? 

The analysis is done in a Jupyter notebook with the following libraries: 
* numpy
* pandas
* matplotlib.pyplot
* LinearRegression from sklearn.linear_model 
* train_test_split from sklearn.model_selection
* r2_score and mean_squared_error from sklearn.metrics

## Files 
* Data Science Project 1.ipynb - The Jupyter notebook containing the data analysis. The notebook is commented with the steps taken in the data collection, cleaning, analysis, modeling, and observations

## The Data
I imported the listings data into a Jupyter notebook and began to evaluate the data. Examining the date revealed 106 columns and 10858 rows.

## Cleaning the Data
My first step was to drop columns with 0 or 1 unique value as all the values were either all null or all the same. Neither case would provide any useful information.
Next, I noticed that prices were formatted as strings with a dollar sign ($). In order to evaluate price data, I needed to convert these values to floats.
After that, I decided to evaluate columns with nulls and remove columns that are mostly null (more than 50% null). There did not appear to be any high null count columns that should be retained to reduce loss of information. Many columns with high null values were text descriptions which are difficult to quantify. I removed columns with free text descriptions, urls, redundant data, and other data that did not provide much information. The 'weekly_price' and 'monthly_price' columns are mostly null, but we can look at price instead as it is complete. It would be interesting to look at 'square_feet', but there are only 31 values available out of 10839. The 'neighbourhood' column was previously cleaned and values were added to 'neighbourhood_cleansed', so I dropped it and used 'neighbourhood_cleansed' instead.
I noticed that the 'host_response_rate' and 'host_acceptance_rate' had percentages formatted as strings with a percent sign (%). These were converted to floats as a decimal value between 0 and 1.

The 'host_identity_verified', 'host_is_superhost', and 'instant_bookable' columns had t and f values for true and false. These were converted to 1 for true and 0 for false.
The 'host_response_time' and 'cancellation_policy' columns could be represented as ordinal variables, so they were converted to a 1–4 scale for 'host_response_time' and a 1–5 scale for 'cancellation_policy'. Null values were converted to a 0.

There were some null values present for the number of bathrooms, bedrooms, and beds. The column for the number of people a listing accommodates had no null values, however, so I used that to estimate the null values. For each room type, I calculated the mean of the ratio of number of that room type to the number of people accommodated. I then multiplied that mean by the 'accommodates' value for the null row to estimate its value.

I then had to look at the other columns with null values to determine how to replace them. The 'host_identity_verified', 'host_is_superhost', 'cleaning_fee', and 'security_deposit' columns were replaced with 0 as they likely would have a value listed if they were true or non-zero.

The 'host_listings_count' column was a bit different as the fact that a row existed for that column meant that it was at least 1. Null values and 0 values were replaced with 1. Another option could be to cross-reference the 'host_id' with other Airbnb data and record the frequency. This would be a good option if we wanted to look more closely at how many other listings a host may have. It is possible that 0 represents the host not having any other listings and 1 represents the host having 1 other listing; however, looking at the visualized data from Airbnb (http://insideairbnb.com/broward-county/), single listings were represented by the 0–1 bucket.

With the exception of the review columns, all other columns, 'host_acceptance_rate' and 'host_response_rate', were replaced with the mean.

## Analyzing the Data
With the data mostly cleaned, It was time for analysis. Further cleaning was customized to the data to be examined.

### What are the main features driving the price of a listing?
Rows with null values for price were dropped. There should not be any, but this was done just in case. I then set y to the price column and X to all the columns excluding id and price. I first set the null values of the review columns to the mean and later to 0 to see which performed better. It turned out that setting the null values to 0 resulted in a higher r2 score. Dummy values were provided for the categorical variables. I then evaluated the linear regression model for the resulting X and y values using a method provided during the class. This method takes in the data and multiple cutoff values to determine which feature set is the most optimal.

The feature set is reduced based on each provided cutoff value. The data is then split into testing and training data. The training data is fitted to the linear regression model, and predictions are made for the testing data. The r2 score was then evaluated. Once all of the cutoffs are evaluated, a model is fit with the dataset with the highest r2 score value. The method then returns the list of r2 scores for the test data, list of r2 scores for the training data, the optimal linear regression model, the training features, the test features, the training labels, and the testing labels.

As it turned out, there wasn't a good relationship between price and the other features. The highest obtained r2 score values were 0.29697424966673924 for the testing data and 0.3085998646523481 for the training data. Property type, room type, neighborhood, and number of bathrooms were the highest correlated features, but without a good correlation coefficient, it is difficult to make predictions of the price.

### What are the main features driving occupancy, measures with number of reviews?
The process was similar for this question. Rows with null values for 'number_of_reviews' were removed. Again, there shouldn't be any. The values of y and X were set to the 'number_of_reviews' column and the columns excluding 'id' and 'number_of_reviews' respectively. The remaining null values were set to 0. These represent the different reviews for the listing. Null values are in place for listings who have not received any reviews. Dummy variables were created for the categorical variables. The X and y values were evaluated via the same optimization method to find the best linear regression model. This analysis returned a better score, and interestingly, the regression model fit the training data better than the testing data. The r2 score values were 0.6435668299231803 for the testing data and 0.6076545099208543 for the training data. Property type, bed type, and neighborhood were the features with the highest correlation

### Is there a correlation between price and occupancy (measured by number of reviews)?
A scatter plot was created from the 'price' and 'number_of_reviews' columns to visualize the relationship between these two features. Most of the reviews fell under $2000. Most of the listings over that price had a low number of reviews.

## Conclusions
There were not any strong relationships between the price of listings and the other features in the dataset. The relationships were stronger for the number of reviews, but it was still only a moderate relationship. There seem to be a large number of outliers for both price and number of reviews which may be skewing the data. It would be interesting to rerun the analyses with outliers removed or evaluate the differences between outliers and the rest of the dataset. Property type and neighborhood were common highest-correlated features between the two datasets, so these are potential features to explore for Airbnb properties.

## Acknowledgements
This project was completed as part of the Udacity Data Scientist Nanodegree.
A helper function for evaluating the best features for the linear regression model was presented during the course work for the class and was used in the analysis. 
