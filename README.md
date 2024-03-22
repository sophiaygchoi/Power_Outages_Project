# Power Outages Project
**Author**: Yeogyeong Choi
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

|      Variable Names	             |                                                                                                          Description                                                                                                          |
|:--------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|               YEAR               |                                                                                      	Indicates the year when the outage event occurred                                                                                       |
|              MONTH               |                                                                                      Indicates the month when the outage event occurred                                                                                       |
|            U.S._STATE            |                                       U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)                                        |
|          CLIMATE.REGION          |                                                                                       Represents all the states in the continental U.S.                                                                                       |
|          ANOMALY.LEVEL           | This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W) |
|         CLIMATE.CATEGORY         |           This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)            |
|        OUTAGE.START.DATE         |                                              This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region)                                               |
|        OUTAGE.START.TIME         |                                              	This variable indicates the time of the day when the outage event started (as reported by the corresponding Utility in the region)                                              |
|     OUTAGE.RESTORATION.DATE      |                                       This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region)                                       |
|     OUTAGE.RESTORATION.TIME      |                                       This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region)                                       |
|          CAUSE.CATEGORY          |                                                                                 Categories of all the events causing the major power outages                                                                                  |
|         OUTAGE.DURATION          |                                                                                            Duration of outage events (in minutes)                                                                                             |

---
## Data Cleaning and Exploratory Data Analysis
### Data Cleaning:
To ensure the dataset is prepared for thorough analysis, the following steps were undertaken to clean the DataFrame:

- **Filtering unnecessary rows and columns:** Upon reading the Excel file, extraneous rows and columns were disregarded. This step aids in streamlining the dataset for easier analysis and eliminates irrelevant missing values. Similarly, the removal of unnecessary columns enhances the overall readability of the dataset.

- **Correcting column data types:** In order to enhance data accuracy, certain columns underwent type conversion. Specifically, the 'YEAR' column was transformed into integers, while 'OUTAGE.DURATION' was converted to floating-point numbers. This adjustment ensures that the information within each column is appropriately represented.

- **Creating new columns for timestamp data:** Additional columns were generated to accommodate timestamp information derived from 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME'(similarly, 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME'). This transformation facilitates data analysis by incorporating a new datetime stamp column, which may prove invaluable in uncovering temporal patterns and trends within the dataset.

After data cleaning, the combined DataFrame looks like the following (only showing the first 5 rows for illustration):

|   | OBS | YEAR |   CAUSE.CATEGORY	   | OUTAGE.DURATION |      OUTAGE.START      | OUTAGE.RESTORATION  |
|--:|:---:|:----:|:-------------------:|:---------------:|:----------------------:|:-------------------:|
| 0 |  1  | 2011 |   severe weather    |     3060.0      |  2011-07-01 17:00:00   | 2011-07-03 20:00:00 |
| 1 |  2  | 2014 | intentional attack  |       1.0       |  2014-05-11 18:38:00   | 2014-05-11 18:39:00 |
| 2 |  3  | 2010 |   severe weather	   |     3000.0      |  2010-10-26 20:00:00   | 2010-10-28 22:00:00 |
| 3 |  4  | 2012 |   severe weather	   |     2550.0      |  2012-06-19 04:30:00   | 2012-06-20 23:00:00 |
| 4 |  5  | 2015 |   severe weather    |     1740.0      |  2015-07-18 02:00:00	  | 2015-07-19 07:00:00 |

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
Grouping by year and cause category and looking at the pattern in duration mean is meaningful in a way that we can see the median trends over time of different category.

|          |                               | OUTAGE.DURATION	 |
|:--------:|:-----------------------------:|:----------------:|
| **YEAR** |      **CAUSE.CATEGORY**       |                  |
|   2000   |        severe weather         |      2490.0      |
|          | system operability disruption |      375.5       |
|   2001   |       equipment failure       |      494.0       |
|          |         public appeal         |      140.0       |
|          |        severe weather         |     10140.0      |



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
This part aims to assess the dependency of this missingness. I intend to investigate the potential dependency of missingness on the year and another variable (which will be determined later). To evaluate this dependency, permutation tests were employed.

#### Year and Outage Duration Time
**Null hypothesis:** The distribution of 'YEAR' when outage duration is missing is identical to the distribution of 'YEAR' when outage duration is not missing.
<br> **Alternative hypothesis:** The distribution of 'YEAR' when outage duration is missing differs from the distribution of 'YEAR' when outage duration is not missing.

<iframe src="assets/missing-year.html" width=800 height=600 frameBorder=0></iframe>

Upon inspecting the distributions of years, it's observed that while the shapes of the distributions differ, they roughly share the same central tendency. Consequently, the K-S statistic will be employed to further analyze the relationship between the variables. The observed k-s statistic is 0.3948.
The plot below shows the empirical distribution of test statistics in 1000 permutations, the red line indicates the observed test statistics.

<iframe src="assets/missing-year-empirical.html" width=800 height=600 frameBorder=0></iframe>

From the graph above and the result from permutation test, we get p-value of roughly 0.0, which is significantly less than the significance threshold of 0.05. Therefore, we reject the null hypothesis. **Thus, we conclude that it is highly possible that the missingness of duration depends on year column**.

#### Hour and Outage Duration
**Null hypothesis:** The distribution of hour when outage duration is missing is identical to the distribution of hour when outage duration is not missing.
<br> **Alternative hypothesis:** The distribution of hour when outage duration is missing differs from the distribution of hour when outage duration is not missing.

<iframe src="assets/missing-hour.html" width=800 height=600 frameBorder=0></iframe>

The plot below shows the empirical distribution of test statistics in 1000 permutations, the red line indicates the observed test statistics.

<iframe src="assets/missing-hour-empirical.html" width=800 height=600 frameBorder=0></iframe>

The plot below shows the empiritcal distribution of test statistices in 1000 permutations, the red line indicates the observed test statistics.

From the graph above and the result from permutation test, we get p-value of 0.557, which is greater than the significance threshold of 0.05. Therefore, we fail to reject the null hypothesis. **Thus, we conclude that it is highly possible that the missingness of duration does not depend on hour column.**

---
## Hypothesis Testing
The research question under scrutiny is whether power outages caused by intentional attacks are less severe in terms of duration compared to other causes.

To delve deeper into this inquiry, I aim to ascertain whether power outages resulting from intentional attacks, such as vandalism and sabotage, exhibit shorter restoration times and consequently lower severity compared to outages caused by non-intentional factors. This hypothesis will be tested through a permutation test on the distribution of outage durations attributed to intentional and non-intentional causes.

### Setting Up the Testing
**Null Hypothesis H0:**  In the population, the severity (in terms of duration) of power outages caused by intentional attacks and non-intentional causes follows the same distribution.

**Alternative Hypothesis H1:** In the population, outage due to intentional attack have less serverity than power outage due to non-intentional causes, on average.

Under the alternative hypothesis, outage severity resulting from intentional attacks is hypothesized to be lower than that caused by non-intentional factors in terms of duration.

For the analysis, only pertinent columns including 'OBS', 'CAUSE.CATEGORY', and 'OUTAGE.DURATION' will be selected. Additionally, a new binary column named 'is_intentional' will be created, denoting whether the outage event was caused by an intentional attack. To address extreme numeric outliers in the 'OUTAGE.DURATION' column, a new categorical column 'severity' will be generated. This column will categorize outage durations into five brackets, each representing a different level of severity. The severity brackets are as follows:

| duration (min) | severity |
|:---------|:--------:|
|  0 <= duration < 60   |  	1   |
|  60 <= duration < 600   |  	2   |
|  600 <= duration < 1200 |  	3   |
|  1200 <= duration < 3000  |  	4   |
|  3000 <= duration |  	5   |

**Test Statistics:** Utilizing the new scaled severity of duration ('severity'), we have numerical data available. To effectively assess the difference in severity between intentional and non-intentional outage causes, the rescaled severity and the binary column indicating intentional attack status will be employed. Hence, it is appropriate to employ the difference in means as the test statistic.

**Significance Level:** To uphold the accuracy and reliability of our conclusions, a significance level of 0.05 will be adopted for this hypothesis test.

### Permutation Test
According to the observed difference found, which is roughly -1.6622, the mean severity of power outage due to intentional attack is less than the power outage due to non-intentional causes.
I ran permutation test for 10000 times and the graph shows the distribution of permutation test result. The red line marks the observed value.

<iframe src="assets/hypothesis.html" width=800 height=600 frameBorder=0></iframe>

### Hypothesis Testing Conclusion
The obtained p-value for the hypothesis test is nearly 0.0, indicating strong evidence against the null hypothesis at a significance level of 0.05. Consequently, we reject the null hypothesis in favor of the alternative hypothesis.

This result aligns with expectations and holds several implications. Firstly, power outages stemming from intentional attacks may exhibit shorter restoration times, leading to a higher likelihood of lower severity. It stands to reason that intentional attacks may prompt more efficient and targeted response efforts, thereby reducing outage durations compared to events caused by other factors.

---
## Framing a Prediction Problem
### Problem Identification
**Investigation Problem:**

In this research project, the objective is to develop an accurate predictive model for estimating the severity of power outages in terms of duration. This prediction will be based on various features available in a comprehensive power outage dataset, including year, climate category, and cause category.

**Response Variable:**

The 'OUTAGE.DURATION' column serves as the response variable for this prediction task. As it directly indicates the severity of a power outage incident in terms of duration, it is a suitable indicator for our predictive model. Furthermore, since the goal is to predict a continuous outcome (duration), this problem will be framed as a regression task.

**Measuring Metrics:**

The performance of our regression model will be assessed using Root Mean Squared Error (RMSE) and R-squared (R^2) metrics. RMSE is chosen to gauge the average magnitude of prediction errors, offering insights into the model's accuracy while providing interpretable results. R^2, on the other hand, is essential for evaluating how much of the variation in the duration of power outages can be explained by our predictive model.

**Information at Time of Prediction:**

At the time of prediction, information regarding the year, climate category, and cause category will be readily available for each power outage incident. By leveraging these features available at the time of the outage, our predictive model will utilize relevant information to estimate the severity of the outage duration accurately.

**Significance of Prediction:**

The ability to predict the duration of power outages holds significant practical value for both power companies and the general public. Such predictions can aid in understanding the severity of potential outages, enabling proactive measures to mitigate their impact. This predictive tool can assist power companies in optimizing resource allocation and response strategies, while also empowering the public to better prepare for and manage potential disruptions.

---
## Baseline Model
### Baseline Model
**Brief Introduction**
In the Baseline Model, I incorporate the year, climate category, and cause category of power outages as features for our regression model. This entails utilizing one quantitative variable (YEAR) and two nominal variables (CAUSE.CATEGORY and CLIMATE.CATEGORY) to build the baseline regression model.

**Data encoding**
Here are the reasons why I choose these as the features:

`YEAR`: The duration of a power outage may vary depending on the year in which the outage occurred. Advancements in technology over time may lead to shorter outage durations, thereby reducing severity. Since the 'YEAR' variable is a discrete numerical feature representing the year when the power outage occurred, it was passed through without any encoding or transformation. It was directly used as a feature in the model.

`CAUSE.CATEGORY`: The cause of a power outage can significantly impact its duration. For example, outages caused by natural disasters may have longer durations and higher severity compared to those caused by intentional attacks, as restoring power after a natural disaster often requires extensive effort and resources. Cause category is categorical variable. To encode the variable, one-hot encoding was applied. This process involves creating binary columns for each category within the categorical variables. Each binary column indicates the presence or absence of a particular category for each observation. This encoding allows the model to interpret categorical variables as numerical features.

`CLIMATE.CATEGORY`: Climate conditions can also influence the severity of outage durations. For instance, power outages occurring in regions with harsh climates, such as cold climates, may experience more severe impacts and longer durations due to potential facility damage and increased restoration challenges. Climate Category is categorical variable. To encode the variable, one-hot encoding was applied. This process involves creating binary columns for each category within the categorical variables. Each binary column indicates the presence or absence of a particular category for each observation. This encoding allows the model to interpret categorical variables as numerical features.

**Model Descriptions and Performance**
In this analysis, a linear regression model was employed using the scikit-learn library to predict the duration of power outages based on the features of year, climate category, and cause category. However, the performance of the current model, as indicated by the R^2 value, is not particularly strong. The training R^2 value of 0.17036191358779373 and the test R^2 value of 0.16258135998907863 suggest that only approximately 16% of the variability in the outage duration can be explained by these features. This indicates that either these features may not be the most influential factors in predicting outage duration, or that the relationship between these features and outage duration may not be accurately captured by the linear regression model.

Additionally, the RMSE (Root Mean Squared Error) of the model is 7168.365732216755. This large RMSE suggests that, on average, the model's predictions deviate from the actual outage durations by approximately 7168.37 minutes. A larger RMSE value indicates that the model's predictions are less accurate, as they deviate more from the actual values. It's important to note that the wide range of outage durations in the dataset contributes to the large RMSE. Given the variability in outage durations, a larger RMSE may still be acceptable.

To gain further insight into the model's performance, data visualization techniques were employed. By plotting the actual outage durations against the predicted outage durations, we can visually assess how well the model performs across the dataset.

<iframe src="assets/baseline.html" width=800 height=600 frameBorder=0></iframe>

The model can not correctly predict the severity of power outage.

---
## Final Model
In the final model, I aimed to enhance the accuracy by introducing two additional features:

`U.S._STATE`: The inclusion of this feature is crucial because different states may experience varying durations of power outages due to differences in electricity consumption patterns, economic characteristics, and restoration infrastructure. Each state may have a different number of technicians and varying geographic locations, affecting outage restoration times. For feature engineering, I applied one-hot encoding to categorize the states.

`hour`: The time of the outage occurrence can significantly influence restoration efforts. For instance, outages during early morning or late night hours might take longer to resolve due to reduced workforce availability. As such, I included the hour of the outage occurrence as a numeric feature. For feature engineering, I retained the hour information as it is without transformation.

Given the challenges associated with predicting exact outage durations, primarily due to the presence of numerous outliers, I opted to categorize severity based on duration minutes. This approach aligns with the brackets used in hypothesis testing and enables more robust predictions by mitigating the impact of extreme outliers:

| duration (min) | severity |
|:---------|:--------:|
|  0 <= duration < 60   |  	1   |
|  60 <= duration < 600   |  	2   |
|  600 <= duration < 1200 |  	3   |
|  1200 <= duration < 3000  |  	4   |
|  3000 <= duration |  	5   |

Consequently, severity becomes the response variable for the final model, which remains a suitable indicator for predictive modeling while addressing the challenges posed by extreme outliers.

#### Model Construction and Choice of Hyperparameter:
Model Construction: Building upon the baseline model and incorporating the newly defined severity variable, I integrated the aforementioned features. The final model employs RandomForestClassifier, chosen for its ability to handle complex relationships and yield improved accuracy. Utilizing a Grid Search approach, I optimized hyperparameters to enhance model performance.

#### Model Performance:
The final model, trained with the optimized hyperparameters (max_depth = 10) on the same dataset, exhibits promising accuracy on both training and testing data. The accuracy on the training set is 0.702, and on the testing set, it is 0.508. This outperforms the baseline linear regression model, which achieved R^2 scores of approximately 0.17 on both sets. While R^2 scores measure the proportion of variance explained by a regression model, accuracy reflects the proportion of correctly classified instances in classification tasks. The superior accuracy of the final model indicates its effectiveness in accurately predicting severity levels compared to the baseline model's ability to explain variance in the target variable.

---
## Fairness Analysis
The project aims to assess the fairness of our multiclass classification model in predicting the severity of power outages across different time periods. While accuracy alone provides insight into model performance, it may not adequately gauge fairness when applied across various temporal contexts. To address this, I propose evaluating the precision of the model's predictions on earlier (2000-2008) and more recent (2009-2016) power outage incidents.

**Group Formation:**
I segregate the data into two distinct groups based on the year of the power outage incidents:

Group A: Power outage incidents occurring between 2000 and 2008. <br>
Group B: Power outage incidents occurring between 2009 and 2016.

**Null Hypothesis:** The model is fair. Its precision for older incidents (2000-2008) and more recent incidents (2009-2016) are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis:** The model is unfair. Its precision for older incidents is different from its precision for more recent incidents.

**Significance Level:** I set a significance level of 0.05 for this permutation test, ensuring that any observed differences are statistically significant with a 95% confidence level.

**Evaluation Metrics and Test Statistics:** I continue to utilize accuracy as the evaluation metric, consistent with the previous model assessments. Test statistics will be computed as the absolute difference in accuracy scores between predictions on Group A and Group B using the final model. This approach enables us to quantify the disparity in model performance across different time periods accurately.

<iframe src="assets/fairness.html" width=800 height=600 frameBorder=0></iframe>

**P-Value Result:** After conducting 1000 permutations, the resulting p-value is 0.197. This value exceeds our predetermined significance level of 0.05, indicating that we do not have sufficient evidence to reject the null hypothesis.

**Conclusion:** Based on the permutation test results, it appears that our final model for predicting power outage severity levels demonstrates fairness across different time periods. Both older power outage incidents and recent incidents show similar predictive accuracies, suggesting that our model's performance is consistent over time.



