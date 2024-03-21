# Power Outages Project
**Authors**: Yeogyeong Choi
## Project Overview
This data science project aims to analyze the severity of power outages. The dataset utilized for this investigation is available [here](https://engineering.purdue.edu/LASCI/research-data/outages), and data dictionary is available in [this source](https://www.sciencedirect.com/science/article/pii/S2352340918307182).

---
## Introduction
In the history of the United States, major power outages have left lasting impacts, causing economic losses and widespread discomfort. Numerous factors contribute to the severity of these outages, including the affected population, duration, and the amount of demand lost. Regional climate patterns, consumption behaviors, and economic indicators further exacerbate the variance in outage severity across different areas.

One critical factor warranting exploration is the cause of these power outages. Incidents such as severe weather events, system operability disruptions, equipment failures, and fuel supply emergencies, among others, can lead to significant disruptions. While many outages stem from unintentional causes, the specter of intentional attacks looms large—a human-created threat intended to disrupt power supply deliberately.

Intentional attacks can pose a distinct and potentially more severe threat due to their malicious intent and the targeted nature of the damage they inflict. However, restoring power after such attacks can be less challenging since it is easier to restore from human attack compared to natural events like tornado and equipment failure. To discern whether intentional attacks indeed result in less time for restore, a thorough investigation and analysis of outage duration relative to the cause are imperative.

By unraveling the nuances of outage severity in relation to different causes, this research aims to inform public awareness and preventative measures, ultimately fostering resilience in the face of potential threats to the power grid.

### Introduction to the Datasets

The data set contains the information of 1534 observations that pertain to the major outages witnessed by different states in the continental U.S. during January 2000 to July 2016.There are 55 variables. There are variables for general information, regional climate information, outage events information, regional electicity consumption information, regional economic characteristics, and regional land-use characterics.

Some of the relevant columns:

|Variable Names	                 |Description|
|:---------|:--------:|
|  YEAR   |  	Indicates the year when the outage event occurred   |
|  MONTH   |  Indicates the month when the outage event occurred   |
|  U.S._STATE   |  U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)   |
|  CLIMATE.REGION  |  Represents all the states in the continental U.S.   |
|  ANOMALY.LEVEL   |  This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W)  |
|  CLIMATE.CATEGORY   |  This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)  |
|  OUTAGE.START.DATE  |  This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region)   |
|  OUTAGE.START.TIME   |  	This variable indicates the time of the day when the outage event started (as reported by the corresponding Utility in the region)  |
|  OUTAGE.RESTORATION.DATE   |  This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region)   |
|  OUTAGE.RESTORATION.TIME   |  This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region)   |
|  CAUSE.CATEGORY   |  Categories of all the events causing the major power outages  |
|  OUTAGE.DURATION  |  Duration of outage events (in minutes)   |

---
## Data Cleaning and Exploratory Data Analysis
### Data Cleaning:
To ensure the dataset is prepared for thorough analysis, the following steps were undertaken to clean the DataFrame:

- **Filtering unnecessary rows and columns:** Upon reading the Excel file, extraneous rows and columns were disregarded. This step aids in streamlining the dataset for easier analysis and eliminates irrelevant missing values. Similarly, the removal of unnecessary columns enhances the overall readability of the dataset.

- **Correcting column data types:** In order to enhance data accuracy, certain columns underwent type conversion. Specifically, the 'YEAR' column was transformed into integers, while 'OUTAGE.DURATION' was converted to floating-point numbers. This adjustment ensures that the information within each column is appropriately represented.

- **Creating new columns for timestamp data:** Additional columns were generated to accommodate timestamp information derived from 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME'(similarly, 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME'). This transformation facilitates data analysis by incorporating a new datetime stamp column, which may prove invaluable in uncovering temporal patterns and trends within the dataset.

After data cleaning, the combined DataFrame looks like the following (only showing the first 5 rows for illustration):

|   | OBS | YEAR | CAUSE.CATEGORY	     | OUTAGE.DURATION |           OUTAGE.START |  OUTAGE.RESTORATION |
|--:|:----|-----:|:--------------------|----------------:|-----------------------:|--------------------:|
| 0 | 1   | 2011 | severe weather      |          3060.0 |    2011-07-01 17:00:00 | 2011-07-03 20:00:00 |
| 1 | 2   | 2014 | intentional attack  |             1.0 |    2014-05-11 18:38:00 | 2014-05-11 18:39:00 |
| 2 | 3   | 2010 | severe weather	     |          3000.0 |    2010-10-26 20:00:00 | 2010-10-28 22:00:00 |
| 3 | 4   | 2012 | severe weather	     |          2550.0 |    2012-06-19 04:30:00 | 2012-06-20 23:00:00 |
| 4 | 5   | 2015 | severe weather      |          1740.0 |   2015-07-18 02:00:00	 | 2015-07-19 07:00:00 |

### Data Cleaning: Univariate Analysis:

In the univariate analysis, two primary variables were examined: 'OUTAGE.DURATION' and 'CAUSE.CATEGORY'.

#### Distribution of 'OUTAGE.DURATION':
The distribution of 'OUTRAGE.DURATION' was analyzed, focusing on a range from 0 to 20,000 minutes, while ignoring extreme outliers. Despite the removal of outliers, a noticeable right-skewed pattern persisted. The majority of outage durations fell within the range of 0 to 999 minutes, indicating that most outages were relatively short-lived.

<iframe src="assets/uni-duration.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of 'CAUSE.CATEGORY':
The distribution of outage causes, as represented by the 'CAUSE.CATEGORY' column, was explored. Seven distinct categories were observed, with 'Severe weather' emerging as the most prevalent cause, accounting for nearly double the occurrences compared to the second most frequent cause, 'Intentional attack'. 'System operability disruption' ranked third, comprising approximately one-fourth of the occurrences of 'Intentional attack'. The remaining causes, including 'Equipment failure', 'Public appeal', 'Fuel supply emergency', and 'Islanding', each appeared in the dataset less frequently, with occurrences numbering less than 100 for each.

<iframe src="assets/uni-cause.html" width=800 height=600 frameBorder=0></iframe>


### Bivariate Analysis:
To uncover potential associations between two variables, a boxplot was utilized to visualize the relationship between outage duration and the categories of outage causes.

<iframe src="assets/bi-du-ca.html" width=800 height=600 frameBorder=0></iframe>

Upon examination of the box plot, distinct patterns in outage duration among different cause categories became evident. Notably, categories such as 'Intentional attack', 'System operability disruption', 'Equipment failure', and 'Islanding' exhibited relatively smaller box sizes, suggesting less variability or spread in data. Conversely, categories such as 'Severe weather', 'Public appeal', and 'Fuel supply emergency' displayed larger box sizes, indicating large spread in data. The variation in box sizes suggests differences in outage durations across various cause categories.

### Interesting Aggregates
The visual analysis conducted above focused on exploring the relationship between outage duration and cause categories. To enhance the reliability of our analysis, the data was grouped by year and cause category. The trends in the median outage duration over time is followed:

<iframe src="assets/interesting_aggregates.html" width=800 height=600 frameBorder=0></iframe>

For better visualization and clarity, only outage durations under 30,000 minutes were considered in this analysis.

As depicted in the graph, the median outage duration exhibits a decreasing trend over time across most cause categories, with the exception of 'Fuel supply emergency'. It's worth noting that 'Fuel supply emergency' displays several spikes in the graph.

Overall, the analysis indicates a general decrease in median outage duration over time, particularly evident after 2009. This trend underscores potential improvements or advancements in outage management and response strategies across various cause categories.

---
## Assessment of Missingness

### NMAR Analysis

One of the columns susceptible to NMAR is 'OUTAGE.START.TIME', which signifies the time of day when the outage event commenced. The missingness in this column may be attributed to instances where recording the time proved challenging or impractical. For instance, data may be absent during late-night hours, such as between 1-4 AM, when it is less likely for individuals to be awake and able to document the outage start time. The likelihood of missingness in outage start time increases during periods where recording is logistically difficult.

However, introducing a new column to denote the availability of recording during specific time ranges could unveil a potential dependency between this new column and the missingness in 'OUTAGE.START.TIME'. Consequently, this might render the missingness mechanism as MAR, as there could be a correlation between the availability of recording and the likelihood of capturing the outage start time.

### Missingness Dependency
Our attention now shifts to the missingness of outage duration time, and our aim is to assess the dependency of this missingness. We intend to investigate the potential dependency of missingness on the year and another variable (which will be determined later). To evaluate this dependency, permutation tests were employed.

#### Year and Outage Duration Time
**Null hypothesis:** The distribution of 'YEAR' when outage duration is missing is identical to the distribution of 'YEAR' when outage duration is not missing.
<br> **Alternative hypothesis:** The distribution of 'YEAR' when outage duration is missing differs from the distribution of 'YEAR' when outage duration is not missing.

<iframe src="assets/missing-year.html" width=800 height=600 frameBorder=0></iframe>


---
## Framing Investigation Problem
---
## Framing Investigation Problem
---
## Framing Investigation Problem
---
## Framing Investigation Problem
---
## Framing Investigation Problem
**Investigation Problem**: In this project, we are going to perform an investigation on predicting the time duration to prepare recipes. Specifically, we will make a <u>multiclass classification model</u> in which the model will help classify the recipes into one of the four categories (light, casual, event, luxury) in terms of preparation duration to help people decide on the recipe that fits their pace of living.

**Response Variable**: `level` column that indicates the level of time consumption of a recipe. Because we want a prediction on the time consumption of a recipe categorized in different categories of timeframe, as shown in the data cleanning process of previous part, `level` is a good indicator.

**Measureing Metrics**: the metrics for the performance of our classification model would be <u>*accuracy*</u>, this is because our distribution of level of time consumption in our observed datasets are not distributed very spread out, so we could use accuracy as a fair metrics that relect the true performance of our model.

**Information At Time of Prediction**: At the time of prediction, the user would normally have the recipe's ingredients, instructions, nutrition table, and the tags where they find the recipe at hand. Through these details on the recipe, people can resonably count the number of steps and ingredients used in the recipe, determine its difficulty based on its tags, and its estimated protein amount in the nutrition table of the recipe. Hence, we would have all four features columns available at the time of prediction.

---
## Baseline Model
**Features Used**:
For the Baseline Model, we decided to use two features as shown below:

* `difficulty` (categorical feature): This feature extracted from the tags indicates the difficulty level of a recipe, and could be used as an useful feature in predicting the time consumption of the recipe. This is because recipes harder to perform tends to take up more time than easier recipes due to more complicated instructions and techniques required.
    * Feature engineering: Since this feature is a categorical feature, we use one hot encoder to convert it into a bag of words matrix that can be useful for our modeling.
* `protein` (numerical feature): This feature extracted from the nutrition table of the recipe indicates the amount of protein in that recipe, and could provide useful information in predicting the time it takes to complete the recipe. This is because recipes that are high in protein most of the time involve meat as the ingredient, which takes longer time to prepare and cook as compared to other recipes low in protein level.
    * Feature Engineering: Since protein is a useful numerical feature that forms a strong correlation to cooking minutes, we decide to standardize it so that the distribution could be standardized and become more obvious during out modeling.

**Model Construction**:
With the above transformers for each of the feature, we used a ColumnTransformer to allocate a OneHotEncoder transformer and a StandardScaler transformer to the two columns separately, and combined with a <u>RandomForestClassifier</u> as our multi-class classifier in one Pipeline object as out baseline model.

**Model Performance**:
In our investigation, we separate the datasets in to a training set and a testing set (8:2 ratio), and fit our baseline model on the training set to test its performance in terms of accuracy on both seen and unseen data.

The accuracy of baseline model on training set (seen data) is 0.5043137464007679;
the accuracy of baseline model on testing set (unseen data) is 0.5002452810067186.

The performance of the baseline model is fine, but not excellent. This is because the accuracy is about 50%, but we have 40 percent of recipes classified as "light meal" in terms of time consumption. So 50% accuracy is not very significant in this case.

---
## Final Model
In the final model, in order to improve accuracy of our model, we decided to add the following two features into our model when classifying the recipes:
* `n_steps` (numerical feature): This is because a recipe with more steps tend to take longer time to cook. For example, baking a cake tends to take longer than than making salads, and it also takes more steps.
    * Feature Engineering: As steps is a valuable numerical feature that exhibits a significant association with cooking minutes, we opt to normalize it in order to standardize the distribution and enhance its clarity in our modeling process.
* `n_ingredients` (numerical feature): This is because a recipe that uses more ingredients tends to take longer time to in terms of preparation, which reveals longer time to cook.
    * Feature Engineering: Given that ingredients is a valuable numerical feature that demonstrates a robust correlation with cooking minutes, we choose to standardize it. This standardization aims to normalize the distribution and increase its clarity in our modeling process.

**Model Construction and Choice of Hyperparameter:**
**Model Construction**:
Building upon the baseline model, we added the above two new features with StandardScaler transformer. We still use the RandomForestClassifer as our model of multiclass classification using pipeline. However, to improve accuracy of our model, we use Grid Search approach to figure out the optimized hyperparameter (maxmium depth of the decision tree inside the random forest).

**Model Performance**:
We fit our final model with the max_depth returned by the Grid Search (max_depth = 12) on the same training set to test its performance in terms of accuracy on both seen and unseen data.

The accuracy of final model on training set (seen data) is 0.639138317159006;
the accuracy of final model on testing set (unseen data) is 0.6193665351391703.

The performance of the final model is better. This is because the accuracy of our final model improved by 10% as compared to our baseline model's accuracy, and we can compare the accuracy from the two models because we used the same training set and testing set for the two models.

We can get a visualization on the performance of our final model using the illustration from the confusion matrix below:

![confusion matrix](confusion_matrix.png)

---
## Fairness Analysis
Going back to our investigation topic, we are investigating whether our model allows people to predict the time consumption of a recipe into one of the four categories we provided, to match with their lifestyle. So far, we have already constructed a final model that allows prediction, but observation the accuracy on the dataset alone cannot be a good indicator as to whether this model is fair when putting into different timings (older recipes and relatively newer recipes). We determined that a good way to test the fairness of our multiclass classification model is to take the difference between our predicted values's precision score on earlier recipes (2008-2012) and more recent recipes (2013-2018) for comparison. Hence, we are going to conduct a permutation test on the precision of our model in predicting the time consumption of recipes people reviewed in older recipes (2008-2012) and the precision of our model in predicting the time consumption of recipes people reviewed in more recent recipes (2013-2018) to see whether there is an actual bias in our model's performance in terms of different years.

**Group A and Group B**: The two groups we are going to use in this permutation test will be extracted from the column `year`, in which we use a Binarizer with a threshold of 2012 to separate the two groups (older recipes and more recent recipes).

*Group A*: Recipes reviewed between 2008 and 2012.

*Group B*: Recipes reviewed between 2013 and 2018.

(Note that our dataset only contains recipes reviewed between 2008 and 2018.)

**Null Hypothesis**: Our model is fair. Its precision for older recipes (2008-2012) and more recent recipes
(2013-2018) are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis**:  Our model is unfair. Its precision for older recipes is different from its
precision for more recent recipes.

**Significance Level**: The significance level we set for this permutation test is 0.01. That being said, we will perform a relatively strong permutation test with confidence level of 99%.

**Evaluation Metrics and Test Statistics**: Since we used accuracy as the performance measurement in the models before, we will continue to use this as the evaluation metrics in our permutations. As for test statistics, because we suspect the accuracy might be different for testing on older recipes as compared to on more recent recipes, we use the absolute difference in accuracy scores of the predictions on two groups using our final model.

**P-Value Result**:
After 100 permutation, we get a p-value of 0.6, which is greater than our test statistics of 0.01. This suggests we fail to reject out null hypothesis.

Below is a graphic visualization of the outcome of our permutation test:

<iframe src="permutation_test_dist.html" width=800 height=600 frameBorder=0></iframe>

**Conclusion**: According to the result from our permutation test above, it shows evidence that suggests our final model on predicting time consumption based on recipe contents is very likely to be a fair model in terms of the time for both old recipes and the relatively recent recipes.