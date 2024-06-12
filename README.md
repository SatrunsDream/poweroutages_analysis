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
We explore the relationships between pairs of columns in the dataset and create relevant plots. 


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
