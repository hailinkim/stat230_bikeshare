# Bike Share Demand Prediction

# Introduction

- With rising gas prices and environmental concerns, bike share systems have become a popular alternative to automobiles for commuters.
- To keep up with the growing market, it is integral for bike share companies to collect data and be able to project their rental counts on any given day, as well as distinguish between registered users and casual, "one-time" users.
- We used data generated by Washington DC-based firm Capital Rideshare to see if we could predict rentals on a given day based on certain factors.
- **Research Question: What day-to-day factors can predict bike rental numbers on a given day, and do the factors change between casual and registered users?**

---

# Data

There were 731 observations, each matching a day between 2011 and 2012 in the Capital Bike Share system, with weather and seasonal data from the Washington National weather station in DC, all taken from the UCI Machine Learning Repository.

## Variables(used in final model)

### Categorical variables

1. **Season**: The season of the day.
2. **Holiday**: Whether the day is a holiday or not.
3. **Workingday**: Whether the day is a holiday/weekend or not.
4. **Weathersit**: The type of weather on the day in question.

### Numerical variables

1. **atemp**: The normalized feeling temperature of the day.
2. **hum**: The normalized humidity of the day.
3. **casual**: The number of casual bikesharers on that day.
4. **registered**: The number of registered bikesharers on that day.
5. **cnt**: The number of total bikesharers on that day.

---

# Exploratory Data Analysis

## Univariate Analysis

![Image](https://github.com/hailinkim/stat230_bikeshare/blob/a1fe82b660da02b0843eea93e716bb5bfcc68617/plots/histogram.png)

- Data takes normal form with response variable, count.

---

- Adjusted Temperature and Humidity found to be most relevant numerical variables.
- WorkingDay, WeatherSit, and Season most relevant categorical variables.
- Casual and Registered not included as predictors - will later be used to make separate models.
- Normalized feeling temperature between Q1: 0.338 and Q3: 0.608.

---

![Image](https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/favstats.png)

- More than double working days than non-working days.
- Most days had no or low precipitation.

---

## Bivariate Analysis

![Image](https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/scatterplot.png)

- Adjusted Temperature and Count are the most correlated, but the relationship is not linear; it curves.

---

![Image](https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/scatterplot2.png)

- Humidity vs. Count, on the other hand, is difficult to discern any relationship from.

---

![Image](https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/boxplot.png)

- Every level of the weather variable had a different median count.

---

![Image](https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/boxplot2.png)

- There was a major drop in median count in Spring compared to other months.

- Very little difference in medians between working days and non working days.

---

# Model Fitting

We fitted two **multiple linear regression** models for casual and registered users. We used **best subset selection** to decide on variables.

## Model 1 - for Casual

- We used the five-predictor model
  ![Image](https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/summary.png)

  We used the five-predictor model that contained **_season, working day, feeling temperature, humidity and wind speed._** All predictors excpt the indicator variable seasonFall were significant at any reasonable significance level. This simply means that the number of casual users in fall is not significantly different from the number of bikes rented in spring, the reference level, but we decided to use all categories of season.

---

## Model 2 - for Registered

![Image](https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/summary2.png)

For the registered model, the full model has the lowest Cp and highest adjusted R-squared. Although the adjusted R-squared does not increase by substantial amount starting from the five predictor model, we use the **_kitchen sink model._** The sumary output shows that only the indicator variable weathersitLow Precip has large p-value, which means the difference in the mean number of registered users between low and high precipitation is not significant. However, we kept all categories for weather situation in our model.

---

## Conditions

The conditions were not met in either model. These unmet conditions were not helped by transforming variables.

<figure>
  <img src="https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/residual.png" alt="residual plot"/>
  <figcaption> Residuals vs Fitted Plot for Casual</figcaption>
</figure>

<figure>
  <img src="https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/residual2.png" alt="residual plot"/>
  <figcaption> Residuals vs Fitted Plot for Registered</figcaption>
</figure>

---

# Randomization Procedure

We ran permutation tests to test the statistical significance of our models.

## Randomization for Casual

![Image](https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/permutation.png)

We shuffled the response variable to test the significance of the entire model for casual bikers using R-squared.The original R-squared of 65% falls at an extreme point in the right tail of the distribution. Thus, there is an evidence to conclude that our casual model is significant.

## Randomization for Registered

![Image](https://github.com/hailinkim/stat230_bikeshare/blob/main/plots/permutation2.png)

We shuffled the response variable to test the significance of the entire model for registered users using R-squared.The original R-squared of 52% falls at an extreme point in the right tail of the distribution. Thus, there is an evidence to conclude that our registered model is also significant.

# Conclusion

- The two MLR models seem to predict the number of bike rentals for casual and registered users well.
- The predictors of the model for casual bikers and the model for registered users differ.
- Can understand how the rental behaviors of different users are affected by weather and temporal factors
- Useful for optimizing the service for casual and registered users
