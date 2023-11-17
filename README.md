## Introduction

Nowadays, more and more people tend to pay more attention to whether foods are healthy rather than their energy provided. One question has been debated frequently: **Do people generally prefer high calorie foods or low calorie ones?**  

To answer this question, appropriate dataset and rigorous hypothesis test are necessary. In this project, research will be conducted on the dataset **Recipes and Ratings**. This dataset contains two tables, one for recipes and one for reviews/ratings. For the purpose of this project, the two tables will be merged and used as one dataset of 234429 rows.  
Some key attributes of the dataset include:  

* recipe_id: Recipe ID
* user_id: User ID
* minutes: Minutes to prepare recipe
* calories: Calories of the recipe
* avg_rating: The average rating of the recipe
  

## Data Cleaning

Before analyzing the dataset, the data needs to be cleaned. For the purpose of this project, several changes are made to the dataset to ensure the analysis work can be conducted smoothly:  

1. Replace all `0.0` values in the `rating` column with `np.NaN`'s. It is reasonable since in most websites, the lowest rating a user can give is **1 star**. This fact infers that all `0.0` ratings are actually produced by users who wish not to rate the recipe at all.
2. The `nutrition` column is consisted of `str`'s that look like a `list` (e.g: `'[138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]'`), which will be messy when dealing with the data. All elements in this column are converted to `list`'s with appropriate values. Also, this column is then expanded to 6 columns, `calories`, `total fat`, `sugar`, `sodium`, `protein`, `saturated fat`, and `carbohydrates`, where each is corresponded to one entry in the original `list`.
3. One additional column `avg_rating` is added to the dataset. This column contains the average rating of each recipe.
4. To better analyze the question "Do people generally prefer high calorie foods or low calorie ones?", the recipes are divided to 'high calory foods' and 'low calory foods'. The column `high_calories` indicate the category of the recipes.

Here is the first 5 rows of the dataset after data cleaning:  

| name                               | id     | n_steps | n_ingredients | rating | avg_rating | calories | total fat | sugar | sodium | protein | saturated fat | carbohydrates | high_calories |
| ---------------------------------- | ------ | ------- | ------------- | ------ | ---------- | -------- | --------- | ----- | ------ | ------- | ------------- | ------------- | ------------- |
| 1 brownies in the world best ever  | 333281 | 10      | 9             | 4.0    | 4.0        | 138.4    | 10.0      | 50.0  | 3.0    | 3.0     | 19.0          | 6.0           | False         |
| 1 in canada chocolate chip cookies | 453467 | 12      | 11            | 5.0    | 5.0        | 595.1    | 46.0      | 211.0 | 22.0   | 13.0    | 51.0          | 26.0          | True          |
| 412 broccoli casserole             | 306168 | 6       | 9             | 5.0    | 5.0        | 194.8    | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          | 3.0           | False         |
| 412 broccoli casserole             | 306168 | 6       | 9             | 5.0    | 5.0        | 194.8    | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          | 3.0           | False         |
| 412 broccoli casserole             | 306168 | 6       | 9             | 5.0    | 5.0        | 194.8    | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          | 3.0           | False         |



## Exploratory Data Analysis

### Univariate Analysis

<iframe src="assets/fig1.html" width=1000 height=600 frameBorder=0></iframe>

This histogram shows the distribution of calories of recipes, which is the main focus of this project. It can be seen that most recipes fall in the range 75-200 calories, and the peak is at 125-175 calories.

### Bivariate Analysis

<iframe src="assets/fig3.html" width=500 height=500 frameBorder=0></iframe>

This scatter plot of rating and calories give the overall trend of rating vs. calories: recipes with extreme high calories (6k+ calories) tend to receive higher ratings rather than lower ratings. 

### Interesting Aggregates

<iframe src="assets/fig4.html" width=500 height=500 frameBorder=0></iframe>

The dataset is grouped by `high_calories` column, and the average rating of each category is plotted in the histogram. The recipes without high calories seem to have a higher rating on average, which is a significant finding for the hypothesis test in the last section of this project.

## Assessment of Missingness

### NMAR Analysis

The `description` column is considered **NMAR**. It is reasonable that the missing of `description` of a recipe could be the inferred by its total number of reviews, since a recipe with a good description should be more likely to be reviewed by users.

### Missingness Dependency

* `n_reviews` column

  As discussed before, the missingness of `description` might depend on the `n_reviews` column. Therefore, the permutation test is proposed with the following setup:

  * Null Hypothesis: The distributions of `n_reviews` where `description` is missing and that where `n_reviews` is not missing are similar.
  * Alternative Hypothesis: The distributions of `n_reviews` where `description` is missing and that where `n_reviews` is not missing are different.
  * Test Statistic: Two-sample Kolmogorov-Smirnov test
  * Significance Level: 1%

  Before performing the test, a plot is produced to help understand the distributions stated above:

  <iframe src="assets/fig5.html" width=800 height=800 frameBorder=0></iframe>

  The two distributions do look like different. Still, a rigorous permutation test is necessary.  

  The observed test statistic for the test is around 0.46, and the p value is 0.0%. Since the p value is lower than the significance level, the null hypothesis is rejected.  

  Therefore, the missingness of `description` **does depend on** the number of reviews of recipes.

* `carbohydrates` column

  The missingness of `description` should be unrelated to the recipe's nutrition. Therefore, a similar permutation test is performed on the `carbohydrates` column.  

  The observed test statistic for the test is around 0.13, and the p value is 2.7%. Since the p value is higher than the significance level, the null hypothesis is not rejected.  

  Therefore, the missingness of `description` **does not depend on** the carbohydrates of recipes.

## Hypothesis Testing

After all the work, it is time to verify the initial problem: Do people generally prefer high calorie foods or low calorie ones?  

Again, a rigorous hypothesis test is proposed with the following setup:

* Null Hypothesis: The calories and average rating of recipes are not related.  
* Alternative Hypothesis: Average ratings for recipes without high calories are higher than the ones with high calories. 
* Test Statistic: Difference between mean of avg ratings of recipes with high calories (more than the 4/5 of the recipes) and that of the other recipes.
* Significance Level: 1%

The observed test statistic for the test is around -0.02, and the p value is 0.0%. Since the p value is lower than the significance level, the null hypothesis is rejected.  It is more likely that people generally prefer food without high calories rather than the ones with high calories.  

A figure is produced to show the test result, the red line is the position of the observed statistic:

<iframe src="assets/fig6.html" width=600 height=600 frameBorder=0></iframe>