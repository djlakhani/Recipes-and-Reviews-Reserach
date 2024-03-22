# Recipes and Reviews Reserach Project
<p>The recipes and reviews research project is a course project for the course DSC 80 
(Practical Applications of Data Science) at UCSD.</p>
<p>Author: Diya Lakhani</p>

## Introduction

<p>. <b>The goal of the this project is to examine if health and diet focused recipes are more convenient to prepare.</b></p>
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

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis