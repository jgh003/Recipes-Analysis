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

Here, I further investigated how common statistics would measure up for recipes when it came to the amount of protein with given ingredients. Many values were unique, and may be more interesting if I had got the mean() and so forth for the individual ingredients.

# Assessment of Missingness

More words.

# Hypothesis Testing

Lolololo.

# Framing A Prediction Problem

Words.

# Baseline Model

Words.

# Final Model

More words.

# Fairness Analysis

Final words.
