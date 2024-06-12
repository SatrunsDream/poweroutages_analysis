<style>
    body {
        text-align: center;
    }
</style>

# Major Power Outage Events In The Continental U.S.
By: Sardor Sobirov and Diego Arevalo

# Introduction
In this project, we analyzed a dataset of significant power outages in the U.S. from January 2000 to July 2016. These major outages, as defined by the Department of Energy, impacted at least 50,000 customers or resulted in an unplanned energy demand loss of at least 300 megawatts. The dataset was obtained from Purdue University's Laboratory for Advancing Sustainable Critical Infrastructure (LASCI), available at https://engineering.purdue.edu/LASCI/research-data/outages.

The dataset includes details about the major outages, as well as geographical, climatic, land-use characteristics, electricity consumption patterns, and economic characteristics of the affected states.

First, we will clean the dataset and perform exploratory data analysis to understand the provided information. Next, we will examine the mechanisms and dependencies of the missing data.

Finally, we will address our research question: what are the most important causes and characteristics of major power outages? We aim to develop a model that predicts the cause of a major power outage in a specific location and time. This is crucial because if energy companies can anticipate the causes of power outages, they can implement preventative measures, such as reinforcing infrastructure against weather events or enhancing security to reduce attacks.

The original raw DataFrame contains 1,534 rows, each representing an outage, and 57 columns. However, for our analysis, we will focus on a subset of these columns.

| Column Name                | Description                                                                                                   |
|----------------------------|---------------------------------------------------------------------------------------------------------------|
| `MONTH`                    | Indicates the month when the outage event occurred                                                            |
| `U.S._STATE`               | Represents all the states in the continental U.S.                                                             |
| `OUTAGE.START.DATE`        | This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region) |
| `OUTAGE.START.TIME`        | This variable indicates the time of the day when the outage event started (as reported by the corresponding Utility in the region) |
| `OUTAGE.RESTORATION.DATE`  | This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region) |
| `OUTAGE.RESTORATION.TIME`  | This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region) |
| `OUTAGE.DURATION`          | Duration of outage events (in minutes)                                                                        |
| `DEMAND.LOSS.MW`           | Amount of peak demand lost during an outage event (in Megawatt)                                               |
| `CAUSE.CATEGORY`           | Categories of all the events causing the major power outages                                                  |
| `CUSTOMERS.AFFECTED`       | Number of customers affected by the power outage event                                                        |
| `CLIMATE.REGION`           | U.S. Climate regions as specified by National Centers for Environmental Information                           |
| `CLIMATE.CATEGORY`         | Represents the climate episodes corresponding to the years ("Warm", "Cold", or "Normal" episodes)             |
| `TOTAL.PRICE`              | Average monthly electricity price in the U.S. state (cents/kilowatt-hour)                                     |
| `TOTAL.SALES`              | Total electricity consumption in the U.S. state (megawatt-hour)                                               |
| `TOTAL.CUSTOMERS`          | Annual number of total customers served in the U.S. state                                                     |
| `POPULATION`               | Population in the U.S. state in a year                                                                        |
| `POPPCT_URBAN`             | Percentage of the total population of the U.S. state represented by the urban population                       |
| `POPPCT_UC`                | Percentage of the total population of the U.S. state represented by the population of the urban clusters       |
| `POPDEN_URBAN`             | Population density of the urban areas (persons per square mile)                                               |
| `POPDEN_UC`                | Population density of the urban clusters (persons per square mile)                                            |
| `AREAPCT_URBAN`            | Percentage of the land area of the U.S. state represented by the land area of the urban areas                  |
| `AREAPCT_UC`               | Percentage of the land area of the U.S. state represented by the land area of the urban clusters               |
| `PCT_LAND`                 | Percentage of land area in the U.S. state as compared to the overall land area in the continental U.S.         |
| `PCT_WATER_TOT`            | Percentage of water area in the U.S. state as compared to the overall water area in the continental U.S.       |
| `PCT_WATER_INLAND`         | Percentage of inland water area in the U.S. state as compared to the overall inland water area in the continental U.S. |

# Data Cleaning and Exploration
The first step is to clean the data to ensure it is suitable for effective analysis. Data cleaning is crucial because it helps remove inconsistencies, errors, and inaccuracies that can skew results and lead to incorrect conclusions. As data scientists, cleaning the data allows us to work with reliable and accurate datasets, ensuring that our analyses are robust and our insights are valid. Proper data cleaning enhances the quality of our models and improves the overall integrity of our research.

## Cleaning
1.  We begin by dropping irrelevant columns and retaining only the features relevant to our analysis. The columns we keep are: MONTH, U.S._STATE, OUTAGE.START.DATE, OUTAGE.START.TIME, OUTAGE.RESTORATION.DATE, OUTAGE.RESTORATION.TIME, OUTAGE.DURATION, DEMAND.LOSS.MW, CAUSE.CATEGORY, CUSTOMERS.AFFECTED, CLIMATE.REGION, CLIMATE.CATEGORY, TOTAL.PRICE, TOTAL.SALES, TOTAL.CUSTOMERS, POPULATION, POPPCT_URBAN, POPPCT_UC, POPDEN_URBAN, POPDEN_UC, AREAPCT_URBAN, AREAPCT_UC, PCT_LAND, PCT_WATER_TOT, PCT_WATER_INLAND.

2.  Next, we combine the OUTAGE.START.DATE and OUTAGE.START.TIME columns into a single OUTAGE.START column as a Timestamp object. We do the same for OUTAGE.RESTORATION.DATE and OUTAGE.RESTORATION.TIME, creating a OUTAGE.RESTORATION column. We then drop the old columns since all the relevant information is now contained in OUTAGE.START and OUTAGE.RESTORATION.
   
3.  Next, we check our outcomes of interest, OUTAGE.DURATION, CUSTOMERS.AFFECTED, and DEMAND.LOSS.MW, for values of 0, which are likely indicative of missing values since major outages wouldn't have a duration of 0 minutes, 0 customers affected, or 0 MW of energy lost. We replace 0 values in these columns with np.nan.

4.  We observed missing values in several columns: MONTH (9), OUTAGE.START.DATE (9), OUTAGE.START.TIME (9), OUTAGE.RESTORATION.DATE (58), OUTAGE.RESTORATION.TIME (58), CLIMATE.REGION (6), CLIMATE.CATEGORY (9), TOTAL.PRICE (22), TOTAL.SALES (22), and POPDEN_UC (10); To address these missing values, we first need to understand the missing mechanism behind them. This will involve determining if the missing values are missing completely at random (MCAR), missing at random (MAR), or missing not at random (MNAR). Once we understand the missing mechanism, we can use imputation techniques such as mean imputation, mode imputation, or more advanced methods like multiple imputation or predictive imputation to replace the missing values. We will explain these steps in more depth later in our analysis.  

## Exploring Data
In this step, we perform both univariate and bivariate analyses to understand the distribution and relationships of our data, as well as examine interesting aggregates

### Univariate Analysis

We examined the distributions of individual columns by using DataFrame operations and creating relevant plots. 

<iframe
  src="assets/monthly_outages.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>
**The graph above shows the number of outages that occurred in each month provides valuable insights into the seasonal trends and patterns of power outages. By visualizing the monthly distribution of outages, we can identify whether certain times of the year are more prone to power disruptions. The observation that summer months (specifically June to August) have more power outages is coulod be due to Infrastructure Strain, Weather Sensitivity, Resource Planning further analysis is required**


<iframe
  src="assets/cause_outages.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>
**The graph above shows the number of outages per cause category provides insights into the different reasons behind power outages and their frequency. Each bar represents the total number of outages caused by a specific category. By examining the height of the bars, we can identify which causes are more frequent. The graph shows that severe weather is the highest cause of power outages. The high frequency of severe weather-related outages, as indicated by the graph, underscores the vulnerability of power infrastructure to natural disasters. This insight provides context for the higher distribution of power outages during the summer, as severe weather events are more common during this season.**


### Bivariate Analysis

We explored the relationships between pairs of columns in the dataset and create relevant plots. 


<iframe
  src="assets/outagebycause.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>
**The plot above visualizes the relationship between outage duration (the time power is unavailable) and the cause category of the outage events. The plot indicates that outages categorized as "Fuel Supply Emergency" tend to have longer durations compared to other categories. This suggests that issues related to fuel supply emergencies are more complex or severe, leading to extended outages.**


<iframe
  src="assets/outagebyregion.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>
**The plot above visualizes the relationship between outage duration across different climate regions. It helps identify how outage durations vary geographically, providing insights into regional infrastructure resilience and vulnerability. The East North Central and Northeast West regions have the largest bins, indicating that these regions experience a wide range of outage durations. The East North Central region, in particular, shows a significant spread of outliers, suggesting that this region occasionally experiences very long outage durations compared to other regions. This could be due to various factors such as infrastructure vulnerabilities, weather patterns, or other region-specific conditions.**


### Grouping and Aggregates
We utilized pivot tables to extract insights on relationships between pairs of columns in the dataset, aiming to uncover correlations and dependencies.

| CAUSE.CATEGORY     | equipment failure | fuel supply emergency | intentional attack | islanding | public appeal | severe weather | system operability disruption |
|--------------------|-------------------|-----------------------|--------------------|-----------|---------------|----------------|--------------------------------|
| **CLIMATE.REGION** |                   |                       |                    |           |               |                |                                |
| Central            | 175500.0          | 0.0                   | 2325.0             | 29000.0   | 0.0           | 18439625.0     | 1262700.0                      |
| East North Central | 0.0               | 0.0                   | 5941.0             | 0.0       | 7600.0        | 14037140.0     | 2279213.0                      |
| Northeast          | 114303.0          | 1.0                   | 85502.0            | 0.0       | 55800.0       | 28470445.0     | 3715312.0                      |
| Northwest          | 93303.0           | 0.0                   | 2500.0             | 0.0       | 8000.0        | 4909237.0      | 35000.0                        |
| South              | 376330.0          | 0.0                   | 12514.0            | 14500.0   | 54094.0       | 23216039.0     | 4769143.0                      |
| Southeast          | 727101.0          | NaN                   | 0.0                | NaN       | 0.0           | 23956720.0     | 1133333.0                      |
| Southwest          | 167000.0          | 0.0                   | 8513.0             | 35230.0   | 0.0           | 595969.0       | 949589.0                       |
| West               | 1390257.0         | 0.0                   | 239020.0           | 131019.0  | 0.0           | 20579360.0     | 3344890.0                      |
| West North Central | 0.0               | 0.0                   | 0.0                | 0.0       | 34500.0       | 296712.0       | NaN                            |
| **Total**          | 3043794.0         | 1.0                   | 356315.0           | 209749.0  | 159994.0      | 134501247.0    | 17489180.0                     |


**The table provides a comprehensive overview of the number of outages categorized by their causes and climate regions. It allows us to quickly compare the frequency of different outage causes across various climate regions. For instance, we can see that severe weather is the most common cause in most regions, except for the Southeast where it is intentional attack. This highlights the regional variations in outage causes, which can be crucial for infrastructure planning and risk management.**

| CAUSE.CATEGORY               | Central     | East North Central | Northeast   | Northwest  | South      | Southeast  | Southwest  | West       | West North Central |
|------------------------------|-------------|--------------------|-------------|------------|------------|------------|------------|------------|--------------------|
| equipment failure            | 175500.0    | 0.0                | 114303.0    | 93303.0    | 376330.0   | 727101.0   | 167000.0   | 1390257.0  | 0.0                |
| fuel supply emergency        | 0.0         | 0.0                | 1.0         | 0.0        | 0.0        | NaN        | 0.0        | 0.0        | 0.0                |
| intentional attack           | 2325.0      | 5941.0             | 85502.0     | 2500.0     | 12514.0    | 0.0        | 8513.0     | 239020.0   | 0.0                |
| islanding                    | 29000.0     | 0.0                | 0.0         | 0.0        | 14500.0    | NaN        | 35230.0    | 131019.0   | 0.0                |
| public appeal                | 0.0         | 7600.0             | 55800.0     | 8000.0     | 54094.0    | 0.0        | 0.0        | 0.0        | 34500.0            |
| severe weather               | 18439625.0  | 14037140.0         | 28470445.0  | 4909237.0  | 23216039.0 | 23956720.0 | 595969.0   | 20579360.0 | 296712.0           |
| system operability disruption| 1262700.0   | 2279213.0          | 3715312.0   | 35000.0    | 4769143.0  | 1133333.0  | 949589.0   | 3344890.0  | NaN                |


**The table provides a breakdown of the number of customers affected by different causes of outages across various climate regions. It can help identify patterns and trends in outage causes and impacts across different regions. It offers a quick comparison of outage causes across different climate regions. Severe weather emerges as the predominant cause in several regions, such as Central, Northeast, South, and West. Interestingly, intentional attacks appear as the primary cause in the Southeast region, deviating from the pattern observed in other regions. This discrepancy underscores the regional variations in outage causes, emphasizing the importance of region-specific infrastructure planning and risk management strategies.**

# Assessment of Missingness

## NMAR Analysis
Several columns in the dataset contain missing data, and one such column that is likely NMAR (Not Missing at Random) is CUSTOMERS.AFFECTED. The missingness in this column may be due to various reasons, including inaccurate reporting or recording of the number of affected customers. Large-scale outages, in particular, may pose challenges in accurately determining the exact number of affected customers. Additionally, the missingness could be influenced by the severity of the outage or the size of the affected area, as larger outages may be more challenging to assess accurately in terms of customer impact.

Similarly, the column DEMAND.LOSS.MW may also have missing values, as measuring demand loss during outages can be complex, especially for outages affecting multiple regions or a large number of customers. CLIMATE.CATEGORY might have missing values if climate data for certain regions or periods is not available or not applicable. For example, some regions might not have well-defined climate categories due to geographical or climatic peculiarities, leading to missing values in this column.

TOTAL.PRICE and TOTAL.SALES could have missing values if cost and sales data, respectively, are not calculated or documented for certain outages. Factors such as incomplete record-keeping, reporting errors, or the absence of standardized procedures for calculating these values could contribute to missingness in these columns.

TOTAL.CUSTOMERS, representing the total number of customers served by the affected area, might have missing values if the data is not recorded or not applicable. In some cases, it may be challenging to accurately determine the total number of customers served, especially in areas with fluctuating populations or complex utility infrastructure.

Understanding the reasons behind missing values in these columns is crucial for accurate data analysis and interpretation, as it can impact the reliability and completeness of the analysis results.

## Missingness Dependency
To test missingness dependency, I examined the missingness of CAUSE.CATEGORY.DETAIL with respect to CLIMATE.REGION, as well as the day of the week of OUTAGE.START.

Examining the missingness of CAUSE.CATEGORY.DETAIL with respect to CLIMATE.REGION and the day of the week of OUTAGE.START is crucial for understanding data quality and patterns. Identifying systematic missingness helps pinpoint regions prone to incomplete data, informing improvements in data collection processes and reducing biases in analysis. This knowledge enhances imputation strategies and resource allocation, ensuring more accurate and representative results. 

### CAUSE.CATEGORY.DETAIL with respect to CLIMATE.REGION
First, we examine the distribution of Cause Category Detail when the Cause Category is missing and not missing.

**Null Hypothesis:** The distribution of CLIMATE.REGION is the same when CAUSE.CATEGORY.DETAIL is missing vs not missing.

**Alternate Hypothesis:** The distribution of CLIMATE.REGION is different when CAUSE.CATEGORY.DETAIL is missing vs not missing.

<iframe
  src="assets/climateregionbymissingness.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

To assess the significance of the observed TVD, a permutation test was conducted. This involved shuffling the CLIMATE.REGION labels 10,000 times and recalculating the TVD for each shuffle. The proportion of permuted TVDs greater than or equal to the observed TVD gave the p-value. The results of the permutation test yielded a p-value of 0.0, indicating that the observed difference in the distribution of CLIMATE.REGION when CAUSE.CATEGORY.DETAIL is missing versus not missing is highly unlikely to have occurred by chance. Given the p-value of 0.0, we reject the null hypothesis. This suggests that the distribution of CLIMATE.REGION is indeed different when CAUSE.CATEGORY.DETAIL is missing compared to when it is not missing, implying that the missingness of CAUSE.CATEGORY.DETAIL is not completely at random but is related to CLIMATE.REGION.

<iframe
  src="assets/climateregionbymissingnesstvd.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>


### CAUSE.CATEGORY.DETAIL with respect to OUTAGE.START
Next, we examine the distribution of OUTAGE.START when the Cause Category is missing and not missing.

**Null Hypothesis:** The distribution of OUTAGE.START is the same when CAUSE.CATEGORY.DETAIL is missing vs not missing.

**Alternate Hypothesis:** The distribution of OUTAGE.START is different when CAUSE.CATEGORY.DETAIL is missing vs not missing.

<iframe
  src="assets/dayofweekmissignesscause.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

To test the dependency of missingness in CAUSE.CATEGORY.DETAIL with respect to OUTAGE.START, we focused on the distribution of OUTAGE.START across different days of the week, comparing when the Cause Category Detail is missing and not missing. We first assigned the day of the week to each OUTAGE.START entry and then calculated the observed Total Variation Distance (TVD) by comparing the distributions of days when CAUSE.CATEGORY.DETAIL is missing and not missing. We then performed a permutation test, shuffling the days of the week 10,000 times and recalculating the TVD for each permutation to generate a distribution of TVDs under the null hypothesis. The observed TVD was then compared to this distribution to calculate the p-value. The result of the test showed a p-value of 0.6, indicating that there is no statistically significant difference between the distributions of OUTAGE.START when CAUSE.CATEGORY.DETAIL is missing vs not missing. Therefore, we fail to reject the null hypothesis, suggesting that the missingness of CAUSE.CATEGORY.DETAIL is not dependent on the day of the week of OUTAGE.START.

<iframe
  src="assets/dayofweekmissignesscausetvd.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>


# Hypothesis Testing

## Test I
We aim to determine if there is a significant difference in the duration of power outages caused by severe weather compared to those not caused by severe weather. Specifically, whether the average duration of outages due to severe weather is significantly longer than those caused by other factors.

**Null Hypothesis:** The distribution of outage durations is the same regardless of whether the cause is severe weather or not. In other words, outages caused by severe weather and those not caused by severe weather come from the same distribution.

**Alternate Hypothesis:** The distribution of outage durations differs based on the cause, with outages caused by severe weather lasting longer on average than those not caused by severe weather.

**Test Statistic:** The difference in means between the duration of outages caused by severe weather and those not caused by severe weather.

To test if there's a difference in outage duration between severe weather-related outages and those not caused by severe weather, we first created a DataFrame with columns 'CAUSE.CATEGORY' and 'OUTAGE.DURATION'. We then added a column 'is_weather' indicating if the outage was caused by severe weather. The test statistic used was the difference in mean outage duration between severe weather and non-severe weather outages. We shuffled the 'is_weather' column 10,000 times and calculated the difference in mean outage duration each time. The p-value, which indicates the likelihood of observing a difference as extreme as the one in our data under the null hypothesis, was 0.0. This suggests strong evidence to reject the null hypothesis and conclude that severe weather-related outages have a significantly longer duration on average compared to those not caused by severe weather.

<iframe
  src="assets/differencesinOUTAGE.DURATION.html"
  width="700"
  height="600"
  frameborder="0"
></iframe>

## Test II

# Framing a Prediction Problem
Our model aims to predict the duration of a power outage. This is a regression task, as we are predicting a continuous numerical value. The metric we will use to evaluate our model's performance is the Mean Absolute Error (MAE), as it provides a clear interpretation of the average error magnitude.

At the time of prediction, we will have access to the following features: 
These features will be used to predict the duration of the outage, providing valuable insights for outage management and planning.
