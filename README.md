# Recipes and Reviews Reserach Project
<p>The recipes and reviews research project is a course project for the course DSC 80 
(Practical Applications of Data Science) at UCSD.</p>
<p>Author: Diya Lakhani</p>

## Introduction

<p>In a health-conscious, fast-paced world, the new workforce is constantly searching for meal options that are quick, but at the same time healthy. There has been a rise in food content creators that actively share these ready-to-go health-friendly recipes and companies that simplify meal-preparating are growing in popularity. Essentially, the trend we observe is that health and diet recipes are designed to be uncomplicated and quick. This directs us to the <b>goal of the this project, which is to examine if health and diet focused recipes are more convenient to prepare.</b></p>
<p>To address the question above, the project will work with two datasets. The datasets have been created by scraping information about recipes and reviews from <a href="https://www.food.com/">food.com</a>.</p>
<p>The first dataset called recipes contains information on recipes that were posted by users to food.com. The recipes dataset contains 83782 observations or recipes and 12 columns. The main columns that are relevant to our analysis include the time taken to prepare each recipe (minutes), the number of ingredients (n_ingredients), the number of calories, total_fat and other nutrition statistics (nutrition), and the (tags) column which has a list of short tags associated with each recipe. For instance, tags can include 'healthy', 'low-cholestrol', etc. which are a great tool to deduce which recipes are health and diet based recipes.</p>
<p>The second dataset is called ratings and contains information about reviews given to recipes on food.com. One recipe can have multiple reviews. The dataset contains 731927 observations or reviews and 5 columns. The main columns relevant to our analysis is the (ratings) column which is the rating given by a user to the recipe measured on a scale from 1 to 5 and the (recipe_id) column which will be useful while merging the datasets together.</p>

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

<p>The first step of the project is to clean the data and extract necessary features. The following dataset cleaning steps were taken:</p>
<ol>
<li><strong>Left merge the recipes and ratings dataset:</strong> The first piece of cleaning involved merging our datasets together to obtain a larger dataset. To perform the merge we used the 'id' column of the recipes dataset and the 'recipe_id' of the ratings dataset. By performing a left merge, every recipe is matched with its corresponding review. If a recipe has multiple reviews, the recipe will have that many entries in the merged dataframe, each corresponding to one of its reviews. The resulting dataset is stored in the variable merged and we get has 234429 rows and 18 columns.</li>
<li><strong>Replace zero value ratings of the rating column with NaN:</strong> The reason we do this is because a rating of zero is equivalent to being unrated. If we do not replace these values, during analysis it might seem like certain recipes have an extremely low rating, however, this interpretation is incorrect zero means it has not been rated.</li>
<li><strong>Creating an average rating column:</strong> To work with a more uniform distribution of ratings per recipe we compute the average rating per recipe and merge this series onto the dataset merged. The column is called 'avg_rating'.</li>
<li><strong>Creating a column indicating which recipes are health and diet recipes:</strong> For the rest of this project, we will define a health and diet recipe through the tags for each recipe. Some of the most common tags associated with health and diet recipes are 'low-cholestrol', 'low_sodium', 'low-calories', and are essentially of the form 'low-something'. <b>A health and diet recipe is a recipe that contains a 'low-something' tag.</b> We filter the tags through regex and create the column is_low which is a column of booleans indcating True for a health and diet recipe and false otherwise.</li>
<li><strong>Extracting details from the nutrition column:</strong> Since the nutrition content of every recipe is also crucial to classifying health and diet recepies we will extract information from the list provided in the nutrition column. The nutrition column is list formatted as [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]. We create new sub-columns 'calories', 'total_fat', 'saturated_fat', 'sodium', 'sugar', 'protein', and 'carbohydrates' corresponding to each of these values. In addition, the type of each these columns is float.</li>
</ol>

<p>The final dataset is stored in a variable called cleaned and will be utilized throughout the rest of our analysis. The first five rows along with some relevant columns of our dataset are illustrated below:</p>

|     id | name                                 |   n_ingredients |   avg_rating |   calories | is_low   |
|-------:|:-------------------------------------|----------------:|-------------:|-----------:|:---------|
| 333281 | 1 brownies in the world    best ever |               9 |            4 |      138.4 | False    |
| 453467 | 1 in canada chocolate chip cookies   |              11 |            5 |      595.1 | False    |
| 306168 | 412 broccoli casserole               |               9 |            5 |      194.8 | False    |
| 306168 | 412 broccoli casserole               |               9 |            5 |      194.8 | False    |
| 306168 | 412 broccoli casserole               |               9 |            5 |      194.8 | False    |


### Univariate Analysis

<p>In the univariate analysis, we have plotted a distribution of the number of calories per recipe.</p>

<iframe
  src="assets/calories-dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<p> From the plot it seems like the distribution of the number of calories across health and diet v/s non-health and diet recipes is quite similar. This suggests that it would not be suitable for us to classify health and diet recipes based on a calorie cut-off. The plot also indicates that most recipes in our dataset have roughly between 100 and 400 calories.</p>

### Bivariate Analysis

<p>In the bivariate analysis section, we have plotted a box-plot for the number steps and its distribution across health and diet recipes and a sctterplot for to examine the distribution of minutes against calories.</p>

<iframe
  src="assets/steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<p>From the plot above, it is evident that the number of steps on average for health and diet recipes is slightly lesser than non-health and diet recipes. The plot hints at a direction to our question, which is that health and diet recipes are less complicated than its counterpart.</p>

<iframe
  src="assets/calorie-min.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<p>The motivation behind the plot above was to observe if recipes that take longer to prepare are more calorie intensive. We have only considered It is evident that this is infact, not the case. The number of calories in every recipe seems to be distributed almost uniformly along the range of the cooking time. There is no correlation between these two features.</p>

### Interesting Aggregates 

<p>As a part of the interesting aggregates section we have created a pivot_table as shown below. The index of the pivot_table is the category of the rating. The category is determined by the first digit of the rating is on the scale 0 to 5. The columns indicate a health and diet recipe v/s a non-health and diet recipe. The values are the average calories.</p>

|   rating_cate |   False |    True |
|--------------:|--------:|--------:|
|             0 | 522.541 | 493.995 |
|             1 | 459.956 | 518.001 |
|             2 | 498.091 | 455.559 |
|             3 | 489.967 | 482.287 |
|             4 | 408.075 | 370.187 |
|             5 | 437.032 | 392.845 |

<p>The reason we chose to create this pivot table was to analyze if the average number of calories across the average ratings of the recipes would differ. It is quite interesting to notice that the calories averages decrease as the ratings increase, suggesting that people enjoy low-calorie recipes or that low-calorie recipes have better results when created. Another interesting fact is that although we might connect low calorie with being healthy, it is clear that the average of the calories is not consistently lower for the health and diet recipes. This connects to univariate analysis scatter plot.</p>

## Assessment of Missingness

### NMAR

<p>One column that could potentially be NMAR is the review column. The review columns contain written reviews. It is possible that some users simply rated the recipe, however, did not write a review, which means the missingness is dependent on the column itself. Perhaps to learn more about the missingness of the column we could ask if they would remake the recipe or recommend it to someone else. Provided this information we would be able to analyze if the missingness is MAR because maybe they did not write a review because they do not recommend it.</p>

### MAR

<p>We hypothesize that the missingness of the rating column is dependent on the number of ingredients. The reason we want to work with the number of ingredients column is that the complexity of a recipe can be measured by the number of ingredients it takes to make and our quetion suggests the convience of health and diet recipes. To analyze the missingness, we conducted the following permutation test:</p>

<p>Null Hypothesis: The missingness of rating does not depend on the number of ingredients.</p>
<p>Alternative Hypothesis: The missingness of rating does depend on the number of ingredients.</p>
<p>Test Statistic: Absolute differnce in group means.</p>

<p>To conduct the permutation test we created a column called has_rating which indicates whether a rating is missing or not. This column was shuffles every iteration to compute the test statistic of the absolute difference in the average number of ingredients of rating v/s rating missing columns. The observed statistic was rougly 0.16. The following distribution of test statistics was obtained:</p>

<iframe
  src="assets/perm-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<p>We obtain a p-value of 0.0 and this is also reflected by the plot above. We can reject the null hypothesis at a 0.05 significance level and conclude that the missingness of the rating column is MAR with respect to the number of ingredients column. It is possible that recipes extremely complex, that have too many ingredients, go unrrated.</p>

### Not MAR

<p>To analyze the missingness dependency on another column, we hypothesize that the missingness of the rating column is dependent on the minutes. For the same reason as above, the number of minutes can also be used to describe the complexity of a recipe. To analyze the missingness, we conducted the following permutation test:</p>

<p>Null Hypothesis: The missingness of rating does not depend on the minutes.</p>
<p>Alternative Hypothesis: The missingness of rating does depend on the minutes.</p>
<p>Test Statistic: Absolute differnce in group means.</p>

<p>To conduct the permutation test we created a column called has_rating which indicates whether a rating is missing or not. The observed statistic was rougly 51.45. The following distribution of test statistics was obtained:</p>

<iframe
  src="assets/perm-test2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<p>We obtain a p-value of 0.129 which means that at a 0.05 significance level we fail to reject null hypothesis. We lack sufficient evidence to conclude that the missingness of the rating column is dependent on the minutes column.</p>

## Hypothesis Testing

<p>We answer the question proposed in the introduction: Are health and diet recipes more convenient to make? Given our data, this question translates to asking if recipes with the tag 'low-something' take lesser number of steps to make compared to recipes that do not contain a 'low-something' tag? We will conduct a permutation test for this analysis.</p>

<p>To reiterate what has been mentioned before, a health and diet recipe is one that contatins a 'low-something' tag. The various low-something tags in our dataset across all our recipes with their counts are listed below.</p>
| index           |   tags |
|:----------------|-------:|
| low-in          |  88247 |
| low-carb        |  44903 |
| low-sodium      |  41317 |
| low-cholesterol |  38836 |
| low-calorie     |  38473 |
| low-protein     |  33914 |
| low-saturated   |  32576 |
| low-fat         |  22071 |
| low-carbs       |  10135 |
| low-cooker      |   7958 |
| low-beans       |   1402 |

<p>Null Hypothesis: Health and diet recipes take the same number of steps as non-health and diet recipes.</p>
<p>Alternative Hypothesis: Health and diet recipes take less number of steps to make than non-health and diet recipes.</p>
<p>Test Statistic: Differnce in group means.</p>
<p>Significance Level: We will perform our analysis at a 5% significance level.</p>

<p>The reason we have chosen difference in group means is because our alternative hypothesis has a direction to it, that is health and diet recipes take lesser steps to make. We have chosen to conduct a permutation test since we are analyzing two samples, health and diet and non-health and diet recipes, and if they come from the same population, this implies that health and diet recipes take the same number of steps to makes.</p>

<p>The observed test statistic is the average number of steps for a health recipe - average number of steps for a non-health recipe. The statistic observed is roughly -1.38. We shuffle the column 'is_low' which indicates which recipes are health recipes every iteration. The following distribution of test statistics was observed. The line indicates the observed statistic:</p>

<iframe
  src="assets/hypo-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<p>The p-value we obtain is 0.0. At a 0.05 significance level, we can reject the null hypothesis, suggsting that there is a possibility that health and diet recipes do take lesser steps to make, and hence, are more convenient.</p>

## Framing a Prediction Problem

<p>We plan to predict the number of calories present in a recipe. The response variable is the number of calories. We will build a regression model to make this prediction. The model will be evaluated through the RMSE score since it indicates how much our predictions vary from the actual value. We also plan to observe the R2 values since it is indicative of the quality of linear fit. The reason we have chosen this question is since it complements the hypothesis test that works with diet and health recipes.</p>

## Baseline Model

<p>To create the baseline model, we have used the features 'carbohydrates', 'sugar', 'total_fat' and 'is_low' (the column that indicates which recipes are health and diet recipes) to predict the number of calories in the recipe. The features 'carbohydrates', 'sugar', and 'total_fat' are quanitative and are all measured in PDV. The column 'is_low' is a nominal categorical column of booleans. Since the three quanitative variables were measured on the same scale, that is of PDV, we did not perform any kind of transformations, like standard scaling, since they are already compareable. For the 'is_low' column we performed one-hot-encoding, dropping the first column to obtain the most optimal coefficients.</p>

<p>After splitting the training and testing data 80-20, we trained our model and computed the training and test RMSE and the training and test R2. The RMSE of the training and the test models was quite high. The training RMSE was roughly 93.12 while the test RMSE was roughly 96.85. This suggests that we must add features to our model to lower the RMSE since the ingredients 'carbohydrates', 'sugar', 'total_fat' cannot alone predict the number of calories. On a better note, since the training and test RMSE are quite similar, it suggests that model is able to generalize to unseen data quite well. The same is reflected in the training and test R2 score which is roughly 97% for both sets. While this suggests that the quality of the linear fit is great, it does not reflect the strength / weakness of out model's predictions. For the final model will attempt to discover the optimal combination of features.</p>

## Final Model

<p>To improve on the baseline, I added the following features: 'n_ingredients', 'sodium', 'saturated_fat', and 'protein'. The reason for choosing these new parameters is that firstly it is possible that recipes with more ingredients can contain more calories and hence it could improve our prediction. The next three features come from the nutrition column that we expanded earlier. The reason we decided to add them to the final model is because all the nutrition components would ideally be used to compute the number of calories present in the recipe. Depending on the various combinations of hyperparameters, we have performed different kinds of column transformations, these are detailed below:</p>

<ol>
<li>low + sugar + total + carbs: Same as the baseline, same transformations.</li>
<li>low + sugar + total + carbs + pro: The difference between this model and baseline is that we also consider how protein can factor into computing the number of calories. The tranformations for this model are the same as mentioned in the baseline. The reason we have isolated protein from the other features is becasue the protein may have a stronger impact on the number of calories compared to sodium which is almost some small amount of salt in every recipe and saturated_fats, whose calorie contribution might be proportional to that of total fat.</li>
<li>low + sugar + total + carbs + pro3: The difference between this model and the one above is that we tranform the protein column by taking it to the power three. The reason we performed this transformation is because on a scatter plot of protein v/s calories, the plot seemed slightly exponenial. So by the Tukey Mosteller Bulge Diagram we decided to take it to the power 3.</li>
<li>low + sugar + total + carbs + ingre + pro + sodium + satfat: In this model we consider all our features and standaridze them. The reason we standardize them is since in case this happens to be the optimal model, we can also analyze the coefficients to see which component more strongly determined the number of calories.</li>
<li>low + sugar + total + carbs + pro + sodium + satfat: The final combination of hyperparameters invovles all except the number of ingredients. We perform the same transformations as the baseline model.</li>
</ol>

<p>The final model we settled with was a multi-feature linear regression model. The best combination of hyperparatmeters obtained was low + sugar + total + carbs + pro and we determined this through a manual iterative form of k-fold cross-validation. We took k to be 5 and trained each hyperparameter combination five times while comparing against a new validation set each time. We observed a significant improvement from the model's baseline performance. The RMSE of the training and the test reduced to roughly 37 and 39 respectively. We analyzing the coefficients of our model and the various validation statistics, it is evident that the protein feature was the key to improving model performance. In addition, the R2 score for the training and the test set increased by roughly 2%.</p>

## Fairness Analysis

<p>To conduct the fairness analysis, we decided on two quite intriguing groups. The two groups we have considered are groups with a high rating and ang groups with a non-high rating. We define a group as one with a high rating if it have a rating above 4. We conduct a permutation test to analyze if our model performs better on groups with a lower rating. The evaluation metric will be the RMSE.</p>

<p>Null Hypothesis: The model has the same performance on lower rating and high rating groups.</p>
<p>Alternative Hypothesis: The model performs better on lower rating groups.</p>
<p>Test Statistic: Difference in RMSE of the the each of the groups.</p>
<p>Significance Level: We will evaluate the test at a 5% significance level.</p>

<p>The p-value obtained was roughly 0.3 suggesting that we fail to reject the null hypothesis at 0.05 significance level. What this suggests that we do not have enough evidence to conclude that our model performs differently on higher rated v/s lower rated groups.</p>

