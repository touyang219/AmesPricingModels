# AmesPricingModels

Introduction:

In this project, our primary objective was to create and evaluate multiple machine learning models to predict Sales Prices of homes in Ames, Iowa (where Iowa State is located). I tackled this project from the perspective of someone who is interested in purchasing a home. Are there any features that may stand out more than the rest?

EDA:

First, I took a look at the distribution of our target variable Sales Price. As shown, it is not normally distributed, but rather, right-skewed. I then performed a log-transformation to see if it was more normally distributed. As you can see, the log transformation is normally distributed, so our target variable will be the log of Sales Price. This will help our linear models perform better.

I then took a look at the multicollinearity amongst our variables. Not only is this to check the multicollinearity assumption for linear models, but also to give us an idea for possible new features we can create in Feature Engineering. As it turns out, there were 5 pairs of variables that were highly correlated with each other. Garage Year Built and Year Built, 1st Floor Square Feet and 2nd Floor Square Feet, Total Rooms above Ground and Above Ground Living Area, Garage Area and Garage Cars, and Garage Living Area and 2nd Floor Square Feet. Amongst these pairs, I dropped the variable that is least correlated with our target variable. However, I did keep 2nd Floor Square Feet for the time being, as I was planning to use it in Feature Engineering.

Next, I took a look to see if our feature variables had a linear relationship with the log of Sale Price. As an example, here is the Overall Quality of House showcasing a strong linear relationship with our target variable. This part really only matters for linear models.

From the heatmap, GrLivArea was one of the variables that were most correlated with our target variable. When compared to the Sale Price (so not our target variable), we can see a linear relationship, but it also clearly shows outliers. I then applied Robust Scaler, a function in sklearn, to tackle this issue.

Finally, missing values. Numerical Features had “NA” values replaced with the sample mean. Specifically for LotFrontage, I replaced missing values with the median of the neighborhood the house is in. For categorical features, I noticed that “NA” doesn’t mean a “missing value” but rather the house is lacking in that feature, so I replaced those values with “none.”

Feature Engineering:

I utilized Standard Scaler, another package in sklearn, to standardize my numerical variables. This helps make variables that have a very large scale more comparable. For Ordinal Variables, I label-encoded them, and then dummified nominal variables. I then created two new variables: totalSqFt and totalBath. totalSqFt is the sum of Total Basement Square Feet, 1st Floor Square Feet and 2nd Floor Square Feet. totalBath is the sum of Full Bath, BsmtFullBath, HalfBath, and Basement HalfBath (I summed up the latter two variables and divided them by 2 to account for “half bathroom”). I then dropped the features the two new variables were composed of. After all this, I had over 200 variables, so I decided to remove 100 of the least correlated features (in relation to our target variable). 
One thing to note: I did create a separate dataset for my tree-based models; prior to label-encoding my ordinal variables, I copied my cleaned dataset and label-encoded all the categorical variables since dummifying variables affect the performance of these non-linear models.

These 8 Models were explored with the goal of finding the Model with the lowest RSME.
As an example, these were the results of my Linear Regression Model. I wanted to highlight that these two assumptions were met. The scatter plots show residual points evenly spread around the diagonal line, so we can assume that there is linear relationship between our independent and dependent variables. The normal distribution of the residuals shows an even distribution of our residuals.

Results: 

It turns out that our Linear Regression had the best RMSE with a r^2 of 0.9234. This means that our model can explain 92.3% of the variance in log of Sales Price. Since we took the RSME of a log transformed variable, it’s unitless, and thus a little hard to interpret. I converted my RMSE such that we can get the average error in dollars. 

Finally, I counted the “top” features in my models. I had to split up how I counted these. For linear models, I just took the coefficients. This can be done since there is a linear relationship between the target variable and predictor variables, so the larger the coefficient, the "more important" the feature. In this case, I set the criteria to be any coefficient over 0.05. For tree-based models, it's a little different. I used the feature_importances_ function, and then divided each feature's value by the feature with the max value. Therefore, these results were in relation to the feature variables, rather than our target variable.

Interestingly enough, Overall Quality appeared in all eight models. The total square footage, total number of baths, year remodeled, and number of fireplaces appeared in all four non-linear models. At the end of the day, this gives an idea for what feature(s) to pay attention to for those that are looking to buy a house, as well as those who want to sell their house. Perhaps their current house is missing some of these features, and by adding or renovating certain features, it will help increase the sale price of their house.

For the Future:

If I had more time, I would probably spend more time improving EDA and Feature Engineering, but this project was able to provide some insight on the most impactful drivers of housing projects in Ames, Iowa. It would be cool to see how accurate these models would be compared to similar cities (in terms of population, crime, education).


