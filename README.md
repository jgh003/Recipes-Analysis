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

## Steps for cleaning the dataset.
0. Merge ```RAW_recipes``` and ```RAW_interactions```.
1. Replace ```np.nan``` values with 0.
   - Some of the reviews for the recipes are just comments or tweaks, which do not have a rating option on the website this data was gathered from. Thus, by design the rating for the recipe was defaulted to zero.
2. I created a ```average_rating``` column, by grouping by the column ```recipe_id``` that way only existing rows which have a corresponding rating will be counted, and transformed by the pandas mean() function.
3. I created seperate nutritonal columns from ```nutrition```.
   - I created the columns 'calories', 'total_fat', 'sugar', 'sodium', 'protein', 'saturated_fat', and 'carbohydrates' using the respective values in the original ```nutrition``` column which contained a list of PDV's for each recipe.
   - I casted elements for each column to be of type float, since some lists were formatted as strings rather than actual lists.
5. I converted any other columns that looked like lists, but were actually strings.
   - These included the columns: 'tags', 'steps', and 'ingredients'.
7. a

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
