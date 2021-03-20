# tayara_cars_price_prediction
scraping cars dataset from tayara.tn , cleaning it, visualizing and then implementing models to predict car prices


We will scrape our data at first, do some EDA(Exploratory Data Analysis), then feature selection then modeling and finally evaluating our model using diffirent metrics and techniques.

![car price prediction](/tayara_logo.png)

## Part 1 – Data Scraping
i used selenium to browse using google chrome
and beautifulSoup as a web scraper
we'll scrape : location, kilometers,year,brand,model,price,carburant,horse power, design,cylinder size


## Part 2 – Data Cleaning and EDA
### Data Cleaning
We need to preprocess the data to transform it to a useful one and also to reduce its size, so that it
becomes easier to analyze.
In our case we need to:
• nb_year: Remove unwanted string from year, convert it to int type, and extract the number of years from it 
nb year will have a smaller number range
• delete “NULL" from the columns ,there'snt a lot of null values.

#### Unwanted columns
• boite and cylindre : drop these columns they were too "incoherent" the web scraper didn't perform well regarding gathering the right columns
### EDA
After the data preprocessing step, we should visualize the data to better understand the business,
how the data is distributed and have more insights about what is happening under the hoods. (please
find all analysis in the notebook)

## Part 2 - Feature selection & removing outliers

In the analysis made in the notebook we can see that the “nb of years” , “carburant type“,
”kilometers driven”, “horsepower” are strongly correlated with the price. So
"boite" and "cylindre" are not the only relevant features to work with.

we'll remove some outliers present in kilometers driven and selling price. We need to remove these outliers by using IQR method.
 ### encoding
  one hot encoder resulted in an explostion of number of columns , label encoder wil create a comparison between the labels and we don't need that, so i used frequency encoder on the brand , model and city columns.


## Modeling
Normally for model selection we should use cross validation to test the data on multiple models,
for example :
linear regression
XGBOOST
Random forest classifier
And then calculate the cross validation score for each model ( use shuffle for automatic split)
In our example, i arbitrary tried with the most 2 famous regression models: Linear regression and XGBOOST, calculated the cross validation score, the root mean square error , the mean score and the variance score for both model (Please see banchmarking table in the notebook)
After selecting the best model, which was gradient boosting regressor, I used gridsearchcv for the
parameter fine tuning, with param grids:
{'n_estimators':[100],
 'learning_rate': [0.1],# 0.05, 0.02, 0.01],
 'max_depth':[6],#4,6],
 'min_samples_leaf':[3],#,5,9,17],
 'max_features':[1.0],#,0.3]#,0.1] }
and then we will have the best estimator that was found by gridsearchCV.

There is a lot of metrics to evaluate the models but we can also see the plot where we show the real against the predicted prices. We can easily see that both curves share the same pace. 

# Linear Regression result:
MAE: 0.1757249618849295
\nMSE: 0.12621292095037118
\nRMSE: 0.35526457880060486


# Xgboost result :
\NMAE: 0.15196680668587836
\nMSE: 0.1012960862550101
\nRMSE: 0.31827046085838706

# Decision Tree Regressor result :
\nMAE: 0.20269131288825087
\nMSE: 0.2037024937092359
\nRMSE: 0.45133412646202137


