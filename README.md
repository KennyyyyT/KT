# Do food recipes before 2014 and after 2014 require the same number of steps?

by Kenny Tran (kat009@ucsd.edu)


---

## Introduction

The following datasets contain recipes, ratings, reviews, etc. from food.com starting from 2008. Ultimately, we are trying to find **if the distributions of the number of steps for recipes before 2014 and after 2014 are the same.** Have recipes required a significantly different amount steps to complete them during these two time periods?

This dataset contains **234429 rows**.

The key columns are listed below with descriptions:

| Column | Description|
| --------- | --------- |
| 'name' | Recipe name |
| 'id' | Recipe ID |
| 'minutes' | Minutes to prepare recipe |
| 'n_steps' | Number of steps in recipe |
| 'n_ingredients' | Number of ingredients in recipe |
| 'description' | User-provided description of recipe |
| 'review' | Text for review of recipe |
| 'rating' | Rating given |
| 'average_rating' | Average rating given |
| 'year' | Year of submission |

---

## Cleaning and EDA

In order to clean the data, I first had to load in the two raw datasets by using pd.read_csv(). These raw datasets were split up with one having columns about the recipes and the other having columns about the ratings and reviews of the recipes. 

I was able to merge both of them by using the 'id' column (in the ratings and reviews DataFrame it was labeled as 'recipe_id'), and I kept the left values since I needed to keep all of the recipes in order to perform tests. 

I replaced all of the 0 values in ratings with NaN values since they represent missing ratings in our dataset (there cannot be a rating of 0). 

Next, I needed to find the average rating per recipe because it was a key column to test the research question. I did this by grouping by 'id', selecting the 'rating' column, and aggregating with the mean function. 

Finally, I added the column to the merged dataset and dropped unnecessary columns.

Here's what the head of the final DataFrame looks like.

| name                                 |     id |   minutes |   n_steps |   n_ingredients | description                                                                                                                                                                                                                                                                                                                                                                       | review                                                                                                                                                                                                                                                                                                                                           |   rating |   average_rating |   year |
|:-------------------------------------|-------:|----------:|----------:|----------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------:|-----------------:|-------:|
| 1 brownies in the world    best ever | 333281 |        40 |        10 |               9 | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |        4 |                4 |   2008 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |        12 |              11 | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |        5 |                5 |   2011 |
| 412 broccoli casserole               | 306168 |        40 |         6 |               9 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!                                                                                                                                  |        5 |                5 |   2008 |
|                                      |        |           |           |                 |                                                                                                                                                                                                                                                                                                                                                                                   | The photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.                                                                                                                                                                                                        |          |                  |        |
|                                      |        |           |           |                 |                                                                                                                                                                                                                                                                                                                                                                                   | Thanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family's favorite Zaar cookbook :)                                                                                                                                                                                                                          |          |                  |        |
| 412 broccoli casserole               | 306168 |        40 |         6 |               9 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.                                                                                                                                                                               |        5 |                5 |   2008 |
| 412 broccoli casserole               | 306168 |        40 |         6 |               9 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | Loved this.  Be sure to completely thaw the broccoli.  I didn&#039;t and it didn&#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.                                                                                                                                                     |        5 |                5 |   2008 |



In this histogram below which shows the **distribution of the number of steps recipes take**, you can see that the majority of recipes have around 5-10 steps to complete them. There are some outliers that are much more than that, which skew the graph to the right, thus increasing the average amount of steps.

<iframe src="assets/univariate.html" width=800 height=600 frameBorder=0></iframe>


In this scatter plot below which shows the **relationship between the number of steps and the number of ingredients in all recipes**, it seems that the data points are very spread out with no clear pattern. This means that there is a good chance that there is no correlation between these two variables.

<iframe src="assets/bivariate.html" width=800 height=600 frameBorder=0></iframe>


In this grouped table below which shows the **number of ingredients and the average ratings of recipes**, it is interesting to see that the average rating when the number of ingredients are 31, 32, 33, and 37 is 5 out of 5 stars. This could mean that more ingredients gives a recipe a higher rating, but that doesn't hold true throughout the whole table. The range of average ratings when grouped by number of ingredients is from about 4.65 to 5, which may not seem like a lot even on a five star scale.

|   n_ingredients |   rating |
|----------------:|---------:|
|               1 |  4.85882 |
|               2 |  4.74074 |
|               3 |  4.72307 |
|               4 |  4.70089 |
|               5 |  4.71039 |
|               6 |  4.67856 |
|               7 |  4.66868 |
|               8 |  4.66239 |
|               9 |  4.66385 |
|              10 |  4.6622  |
|              11 |  4.68243 |
|              12 |  4.67654 |
|              13 |  4.68833 |
|              14 |  4.65852 |
|              15 |  4.67836 |
|              16 |  4.6723  |
|              17 |  4.6733  |
|              18 |  4.72986 |
|              19 |  4.68314 |
|              20 |  4.69367 |
|              21 |  4.69412 |
|              22 |  4.83926 |
|              23 |  4.80427 |
|              24 |  4.74847 |
|              25 |  4.65385 |
|              26 |  4.82796 |
|              27 |  4.675   |
|              28 |  4.84211 |
|              29 |  4.92857 |
|              30 |  4.78125 |
|              31 |  5       |
|              32 |  5       |
|              33 |  5       |
|              37 |  5       |



---

## Assessment of Missingness

**NMAR Analysis:** 

The three columns that did have a significant amount of missing values in them were 'rating', 'review', and 'description'. Not missing at random means the missingness is due to the values themselves. While I don't think 'review' or 'description' are NMAR, I do think 'rating' is. This is because during the data cleaning process, we replaced all 0 values with NaN, making them missing. This means that the missingness of rating is dependent on whether the rating is 0 or not, so it is dependent on the values themselves, or NMAR.

**Test if "n_ingredients" has an effect on the missingness of "review":**

To perform this missingness test, I first chose the absolute difference of means for my test statistic with a 5% significance level.

I then grabbed the "n_ingredients" and "review" columns from my DataFrame and calculated the observed absolute difference of means using the values of "n_ingredients" where "review" is null and nonnull. The difference turned out to be about 1 ingredient.

Next, I generated 500 samples using random.permutation to shuffle the "n_ingredients" column in order to calculate 500 test statistics under the assumption that missingness is not dependent on "n_ingredients".

After comparing how many statistics from the 500 samples were equal to or more extreme to the observed statistic, I got a p-value of 0.026. Since I'm using a 5% significance level, we **reject the null hypothesis**. Meaning, **the missingness in the "review" column is dependent on "n_ingredients"**.



**Test if "minutes" has an effect on the missingness of "review":**

To perform this missingness test, I first chose the absolute difference of means for my test statistic with a 5% significance level.

I then grabbed the "minutes" and "review" columns from my DataFrame and calculated the observed absolute difference of means using the values of "minutes" where "review" is null and nonnull. The difference turned out to be about 33.5 minutes.

Next, I generated 500 samples using random.permutation to shuffle the "minutes" column in order to calculate 500 test statistics under the assumption that missingness is not dependent on "minutes".

After comparing how many statistics from the 500 samples were equal to or more extreme to the observed statistic, I got a p-value of 0.98. Since I'm using a 5% significance level, we **fail to reject the null hypothesis**. Meaning, **the missingness in the "review" column is not dependent on "minutes"**.

This histogram below shows just how many sampled test statistics were equal or more extreme than the observed absolute difference of means of 33.5!

<iframe src="assets/missingness.html" width=800 height=600 frameBorder=0></iframe>


---

## Hypothesis Testing

**Question:** Are the distributions of the number of steps the same for recipes before 2014 and after 2014?

**Null Hypothesis:** Recipes before 2014 and after 2014 do have the same distribution of the number of steps

**Alternative Hypothesis:** Recipes before 2014 and after 2014 have different distributions of the number of steps

The test statistic is the **absolute difference in means** since I'm comparing two numerical distributions, and I'm using a **1% significance** level in order to ensure that I need strong evidence to reject the null hypothesis.

The process is very similar to the missingness tests as described previously since it's the same type of test.

I first grabbed the "year" and "n_steps" columns from my DataFrame and calculated the observed absolute difference of means using the values of "n_steps" where "year" is greater than or equal to 2014 and less than 2014. The difference turned out to be about 3 steps.

Next, I generated 500 samples using random.permutation to shuffle the "n_steps" column in order to calculate 500 test statistics under the assumption that recipes before 2014 and after 2014 do have the same distribution of the number of steps.

After comparing how many statistics from the 500 samples were equal to or more extreme to the observed statistic, I got a p-value of 0.00. Since I'm using a 1% significance level, we **reject the null hypothesis**. Meaning, **recipes before 2014 and after 2014 have different distributions of the number of steps**.

---
