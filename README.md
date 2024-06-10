# Major Power Outage Events In The Continental U.S.
By: Sardor Sobirov and Diego Arevelo 

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





