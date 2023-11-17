## Introduction

**Do people generally prefer high calorie foods or low calorie ones?**  

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

|      |                               name |     id | minutes | contributor_id |  submitted | n_steps |                                       description | n_ingredients |   user_id | recipe_id | rating | avg_rating | calories | total fat | sugar | sodium | protein | saturated fat | carbohydrates | high_calories |
| ---: | ---------------------------------: | -----: | ------: | -------------: | ---------: | ------: | ------------------------------------------------: | ------------: | --------: | --------: | -----: | ---------: | -------: | --------: | ----: | -----: | ------: | ------------: | ------------: | ------------: |
|    0 |  1 brownies in the world best ever | 333281 |      40 |         985201 | 2008-10-27 |      10 | these are the most; chocolatey, moist, rich, d... |             9 |  386585.0 |  333281.0 |    4.0 |        4.0 |    138.4 |      10.0 |  50.0 |    3.0 |     3.0 |          19.0 |           6.0 |         False |
|    1 | 1 in canada chocolate chip cookies | 453467 |      45 |        1848091 | 2011-04-11 |      12 | this is the recipe that we use at my school ca... |            11 |  424680.0 |  453467.0 |    5.0 |        5.0 |    595.1 |      46.0 | 211.0 |   22.0 |    13.0 |          51.0 |          26.0 |          True |
|    2 |             412 broccoli casserole | 306168 |      40 |          50969 | 2008-05-30 |       6 | since there are already 411 recipes for brocco... |             9 |   29782.0 |  306168.0 |    5.0 |        5.0 |    194.8 |      20.0 |   6.0 |   32.0 |    22.0 |          36.0 |           3.0 |         False |
|    3 |             412 broccoli casserole | 306168 |      40 |          50969 | 2008-05-30 |       6 | since there are already 411 recipes for brocco... |             9 | 1196280.0 |  306168.0 |    5.0 |        5.0 |    194.8 |      20.0 |   6.0 |   32.0 |    22.0 |          36.0 |           3.0 |         False |
|    4 |             412 broccoli casserole | 306168 |      40 |          50969 | 2008-05-30 |       6 | since there are already 411 recipes for brocco... |             9 |  768828.0 |  306168.0 |    5.0 |        5.0 |    194.8 |      20.0 |   6.0 |   32.0 |    22.0 |          36.0 |           3.0 |         False |