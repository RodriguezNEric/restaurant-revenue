# ![GA Logo](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Predicting Restaurant Revenue

---

**Individual Contributors:** Asia Roy, Eric Rodriguez, Gabrielle Burgos, Jeremy Manning, and Seung Woo Choi

---

## Overview
- Executive Summary
- Introduction
- Data
- Modeling
- Results
- Conclusion

---

### Executive Summary
In this project, we sought to achieve two goals: (1) the first was to develop a mathematical model to predict the annual restaurant sales of 100,000 regional locations and (2) the second was to identify the factors that contributed the most to restaurant revenue. We accomplished both goals by tuning a random forest regression model and diving into its features.

---

### Introduction

**Background** <br>
TFI, the company behind Burger King, Popeyes, and Arby's, spends heavily in developing new restaurant locations. Because the process requires lots of time and capital and could involve operating losses if executed poorly, TFI hired my group of data scientists to uncover insights using their data. <br>

**Problem Statement** <br>
By identifying optimal restaurant locations, TFI hopes to avoid incurring operating losses to fund other crucial business areas like innovation and employee training. The company has tasked us with developing a mathematical model that can incorporate demographic, real estate, and commercial data to predict the annual restaurant sales of 100,000 regional locations. We will use the root mean squared error (RMSE) and coefficient of determination (R-squared) to evaluate our model.

---

### Data

**Data Source** <br>
The two files that were used for this project can be found on [Kaggle](https://www.kaggle.com/c/restaurant-revenue-prediction/data):
- train.csv: This file contains all the training data.
- test.csv: This file contains all the testing data.

**Data Dictionary** <br>

| Feature      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Id           | Restaurant Id                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Open Date    | opening date for a restaurant                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| City         | City that the restaurant is in. Note that there are Unicode in the names                                                                                                                                                                                                                                                                                                                                                                                         |
| City Group   | Type of the city. Big cities, or Other                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Type         | Type of the restaurant. FC: Food Court, IL: Inline, DT: Drive Thru, MB: Mobile                                                                                                                                                                                                                                                                                                                                                                                   |
| P1 - P37 | There are three categories of these obfuscated data.  Demographic data are gathered from third party providers with GIS systems. These include population in any given area, age and gender distribution, development scales.  Real estate data mainly relate to the m2 of the location, front facade of the location, car park availability.  Commercial data mainly include the existence of points of interest including schools, banks, other QSR operators. |
| Revenue      | The revenue column indicates a (transformed) revenue of the restaurant in a given year and is the target of predictive analysis.  Please note that the values are transformed so they don't mean real dollar values.                                                                                                                                                                                                                                             |

**Data Cleaning** <br>

To prepare for preprocessing, features names were lowercased and the ID was set as the index. Missing data was visualized and data types were check to ensure congruity.

---

### Modeling

**Pre-processing**<br>
The feature **open_date** was converted to month, day, year using .str.split. We then created a function that returns days since opening. We then dichotomized feature **city group** as 1 for big cities and as 0 for "other". Features that described which city a restaurant was located in were then dummified.

**Models**<br>
Model 1:<br>
    - Features: 'city_İstanbul','p2', 'city_group', 'days_since_opening', 'p28', 'city_İzmir', 'p6', 'p23', 'p1', 'p7'<br>
    - Utilized train, test, split, and ran a linear regression on the split data

Model 2:<br>
    - created interaction terms based on highly correlated variables to target (p2, city_group, days_opened)<br>
    - Features: 'city_İstanbul', 'p2_x_days_opened','p2_x_city_group','p2', 'city_group','city_Ankara','days_since_opening','year' <br>
    - Utilized train, test, split, and instantiated a random forest regressor and used gridsearch to model our features on revenue <br>
    - Since Model 2 performed the best, we ended up using this as our production model, and ran the unseen Kaggle test dataset on this model.

---

### Results

Null Model:<br>
The null model has a baseline R-squared value of 0.0 and an RMSE of 1,792,868 USD. Our goal for the production model is to achieve an R-squared greater than 0.0 and an RMSE lower than 1,792,868 USD.

Model 1:<br>
A multiple linear regression model with an R-squared of 0.208 and an RMSE of 1,683,384 USD for the testing data. The model performs better than the null model and generalizes well to new data.

Model 2:<br>
A random forest regression model with an R-squared of 0.293 and an RMSE of 1,590,157 USD. The model performs better than the null model and Model 1 but appears to overfit the training and testing data. Since the model has the highest R-squared value and the lowest RMSE score, we designated Model 2 as the production model.

---

### Conclusion
The features that are important in predicting the revenue of a restaurant include: (1) the city where the restaurant is located, (2) the size of the city, (3) the time duration that the restaurant has been open, and (4) an unknown variable called p2. Through our analysis, we determined that the best model for predicting restaurant revenue is the random forest regressor. Our production model achieved an R-squared score of 0.293 and an RMSE of 1,590,157 USD in terms of predicting revenue on unseen data. An R-squared score of 0.293 indicates that 29.3% of the variability in the target variable can be explained by the features we used. An RMSE of 1,590,157 USD means that our predictions for revenue were typically off by 1,590,157 USD from the actual revenue.

**Recommendations** <br>
Our next steps would involve trying to understand the details and relevance of the demographic, real estate, and commercial data (variables P1-P37) and to explore additional interaction terms. Given that the project was completed as part of a hackathon, we were limited on time and could not investigate all the unknown variables.

---
