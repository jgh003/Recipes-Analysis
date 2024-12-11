# Recipes Analysis: Healthy & Delicious

By: Joseph Hernandez

# Introduction

This data science project, conducted at UCSD, focuses on the question:
> "What is the relationship between rating and healthiness of foods?"

Researching this question is relevant because it provides insight on a problem point when it comes to eating healthier. From experience, I've noticed that some people prefer to eat unhealthy foods and I wonder if this is something that happns on a larger scale, or even contributes to obesity in the US. Essentially, I investigate the differences between how people rate healthy foods versus less healthy foods.

In addition, the American population highlights the need for research on topics such as this one, because when it comes to the obesity rates for American citizens, the numbers don't look good... "At least one in five adults (20%) in each U.S. state is living with obesity" ([cdc.gov](https://www.cdc.gov/media/releases/2024/p0912-adult-obesity.html)). To give you some perspective. Take for example, Kansas. According to the CDC, it has one of the highest obesity rates in the country with a population size of roughly 2,937,880 people ([census.gov](https://www.census.gov/quickfacts/fact/table/KS/HSG010223)). Now imagine, if 20% of them were obese... That's 587,576 people (almost 600,000 in just one state)!

| Column    | Description                                                               |
|:----------|:--------------------------------------------------------------------------|
| name      | Name of recipe.                                                           |
| minutes   | The time it takes to prepare the recipe.                                  |
| nutrients | The nutrition facts of the recipe measured in PDV (percent daily values). |
| rating    | The rating of the recipe by a user.                                       |

The original dataframe, upon merging the two dataframes ```RAW_recipes``` and ```RAW_interactions```, resulted in a dataframe with 234,429 rows and 16 columns. However, only 4 columns were relevant and are shown above.

# Data Cleaning & Exploratory Analysis

## Steps for cleaning the dataset

0. Merge ```RAW_recipes``` and ```RAW_interactions```.
1. Replace ```np.nan``` values with 0.
   - Some of the reviews for the recipes are just comments or tweaks, which do not have a rating option on the website this data was gathered from. Thus, by design the rating for the recipe was defaulted to zero.
2. I created a ```average_rating``` column, by grouping by the column ```recipe_id``` that way only existing rows which have a corresponding rating will be counted, and transformed by the pandas mean() function.
3. I created seperate nutritonal columns from ```nutrition```.
   - I created the columns 'calories', 'total_fat', 'sugar', 'sodium', 'protein', 'saturated_fat', and 'carbohydrates' using the respective values in the original ```nutrition``` column which contained a list of PDV's for each recipe.
   - I casted elements for each column to be of type float, since some lists were formatted as strings rather than actual lists.
5. I converted any other columns that looked like lists, but were actually strings, to a list containing their respective values (float or string).
   - These included the columns: 'tags', 'steps', and 'ingredients'.
7. Created ratings for the columns 'protein', 'saturated_fat', and 'sugar'.
   - These ratings range from 1-3
   - Using guidelines from the [U.S Food & Drug Administration](https://www.fda.gov/food/nutrition-facts-label/lows-and-highs-percent-daily-value-nutrition-facts-label), I am determining if a food has a high, middle, or low pdv for the given nutrient depending on how it matches with the FDAs standards. This grade will be measured as a number where low=1 and high=3.
8. Lastly, I create a column called ```high_in_protein```, which is a column of type bool. A recipe is considered high in protein if it recieves a score/rating higher than 2.

| name                                 |   minutes |   protein_rating |   sat_fats_rating |   sugar_rating |   calories |   total_fat |   saturated_fat |   sodium |   protein |   carbohydrates |   sugar |   average_rating | high_in_protein   |
|:-------------------------------------|----------:|-----------------:|------------------:|---------------:|-----------:|------------:|----------------:|---------:|----------:|----------------:|--------:|-----------------:|:------------------|
| 1 brownies in the world    best ever |        40 |                1 |                 2 |              3 |      138.4 |          10 |              19 |        3 |         3 |               6 |      50 |                4 | False             |
| 1 in canada chocolate chip cookies   |        45 |                2 |                 3 |              3 |      595.1 |          46 |              51 |       22 |        13 |              26 |     211 |                5 | False             |
| 412 broccoli casserole               |        40 |                3 |                 3 |              2 |      194.8 |          20 |              36 |       32 |        22 |               3 |       6 |                5 | True              |
| 412 broccoli casserole               |        40 |                3 |                 3 |              2 |      194.8 |          20 |              36 |       32 |        22 |               3 |       6 |                5 | True              |
| 412 broccoli casserole               |        40 |                3 |                 3 |              2 |      194.8 |          20 |              36 |       32 |        22 |               3 |       6 |                5 | True              |

## Univariate Analysis

<iframe src="assets/plot.html" width="800" height="600" frameborder="0"></iframe>
The graph above shows the distribution of average ratings for a sample size of 1000, in which there is a lot of overlap showing little difference in how ratings differed based on 'healthiness'. The x-axis represents ratings (on a 5 point scale), and the y-axis shows the probability of each rating.

## Bivariate Analysis

<iframe src="assets/plot2.html" width="800" height="600" frameborder="0"></iframe>
The graph above shows that foods from the sample (size 1000) that are higher in protein tend to have higher levels of saturated fat.

## Interesting Aggregates

| ingredients                                                           |   ('protein_rating', 'mean') |   ('protein_rating', 'count') |   ('protein_rating', 'median') |
|:----------------------------------------------------------------------|-----------------------------:|------------------------------:|-------------------------------:|
| (' cheese deluxe", ', ', ', ', ', ', ')                               |                            3 |                             1 |                              3 |
| (' cheese deluxe", ', ', ', ', ', ', ', ', ', ', ', ', ')             |                            3 |                             1 |                              3 |
| (' cheese deluxe", ', ', ', ', ', ', ', ', ', ', ', ', ', ', ')       |                            3 |                             1 |                              3 |
| (' cheese deluxe", ', ', ', ', ', ', ', ', ', ', ', ', ', ', ', ', ') |                            3 |                             7 |                              3 |
| (' sugar", ', ', ')                                                   |                            1 |                             5 |                              1 |

Here, I further investigated how common statistics would measure up for recipes when it came to the amount of protein with given ingredients. Some values like shown above, are repeats, and would likely be more useful if I had time to clean it up.

# Assessment of Missingness

The columns ```description```, ```rating```, and ```review``` stood out due to their higher frequency of null values in the dataset.

## NMAR Analysis

I believe the 'Description' column is NMAR because if someone made a recipe that was not original, there wouldn't be much need to explain what it is.

## Missingness Dependency

I examined the missingness of 'rating' in the cleaned dataframe by testing the dependency of its missingness. I investigate if the 'rating' column depends on the column ```recipe_id``` or the column ```minutes```. I took a sample of 10,000 (for computational reasons) and ran a permutation test where I used the absolute difference in means as the test statistic. Here's what I found:
- The observed p-value for the test on 'recipe_id' was 0.991
- The p-value for the test on 'minutes' was 0.007
- The p-values demonstrated a clear dependency of missingness
- The permutation test on 'recipe_id' did show a difference for the missingness of 'rating'

# Hypothesis Testing

- Null Hypothesis: People do not rate healthier recipes different than less healthy recipes.
- Alternative Hypothesis: People rate healthier recipes higher than less healthy recipes.
- Test Statistic: Difference in Means.
- Significance Level: 0.05.

## Conclusion Of The Test

When performing the hypothesis testing, I chose to run permutation tests since I want to see whether the difference between group means were statistically significant. The resulting p-value from the test was ```p = 0.718```, which means reject the null. People rate healthier foods higher than less healthy foods, and this was not randomly due to chance.

# Framing A Prediction Problem

I plan to predict the average rating of a recipe which will utilize a regression model since the data is continuous. Because I have data that supports a difference between how people rate food options (from the previous section), I want to predict the average rating of recipes using that knowledge. To evaluate the model, I'm keeping it simple and using mean squared error to test predicted values against actual values.

# Baseline Model

In this section, I add a new column called ```is_healthy``` which takes recipes that have a high score (3) in ```protein_rating```, and low scores (1) in both ```sat_fats_rating``` (PDV score for the amount of saturated fats in the recipe based on FDA standards) and ```sugar_rating``` (PDV score for the amount of sugar in the recipe based on FDA standards). This column acts as a way to measure the 'healthiness' of a recipe by maximizing protein which is prevalent in dieting for fat loss/muscle gain, and minimzing the most unhealthy macronutrients. These would be sugar and saturated fat. We need carbs, fats, and (some) sodium to function properly as people, and thus the 2 mentioned macros were chosen to be minimized.

My model is a regression model, specifically using the RandomForestRegressor, and the data is separated into testing and training sets. The features in the model are ```is_healthy``` and ```minutes```. The column 'is_healthy' was chosen as a feature since the 'healthiness' affects rating as seen in the previous section 'Hypothesis Testing'. And minutes was chosen because perhaps people would rate a recipe that takes a long time to cook lower. The feature ```is_healthy``` is boolean data that was one-hot-encoded into quantitative data and ```minutes``` remains the same. To measure performance, I used mean squared error.

## Reported Performance

The model was 'fairly poor', with a ```MSE (mean squared error) of 0.502128284338456```. I do not believe this model is good since the MSE is about 50, which is about directly in the middle between absolutely terrible and a perfect fit. This does not make the model particularly useful.

# Final Model

The features that I added are ```sugar``` and ```calories```. I initially chose them because it would make sense that if healthy recipes differed in rating from less healthy recipes, then something like sugar would help increase the model's effectiveness when predicting lower ratings (less healthy recipes were rated lower during hypothesis testing). And I chose calories because perhaps larger meals which may be more filling would be rated higher and thus help with predicting higher ratings. However, upon performing a k-fold cross validation, all the transformed features performed the same. Therefore, I stuck with adding ```sugar``` and ```calories``` as features. The ```sugar``` feature was tranformed using the ```QuantileTransformer``` since the distribution of sugar levels was not a normal distribution, and thus I wanted to see if there would be a difference with the model if I transformed the data to match the distribution. There was no difference. Ultimately for the model, I used linear regression and the model did improve.

## Reported Performance

The model did not perform fantastic, but it did improve significantly. ```The difference in MSE was -0.12960350026396222```, which made the new ```MSE 0.3725247840744938```.

# Fairness Analysis

## Overview

When splitting the groups, I decided to use ```minutes``` and explore how RMSE for recipes with different cooking times differed. The value of 35 minutes as a threshold between groups was derived from the median of the input dataset. I wanted median in order to reduce noise from outliers.
- Group X: Slow preparation times (>35 minutes)
- Group Y: Fast preparation times (<35 minutes)
- Null Hypothesis: Our model is fair. Its RMSE for recipes with a lower cook time and higher cook time are roughly the same, and any differences are due to random chance.
- Alternative Hypothesis: Our model is unfair. RMSE differs between the groups.
- Evaluation Metric: RMSE.
- Test Statistic: Absolute difference in means.
- Significance Level: 0.05.
- p-value: ```0.33```. We reject the null hypothesis.

## Conclusion

At the end of the fairness analysis, the model was unfair.

<iframe src="assets/plot4.html" width="800" height="600" frameborder="0"></iframe>
