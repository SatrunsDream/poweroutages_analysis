# Major Power Outage Events In The Continental U.S.
By: Sardor Sobirov and Diego Arevelo 

# Introduction
In this project, I analyzed a dataset of significant power outages in the U.S. from January 2000 to July 2016. These major outages, as defined by the Department of Energy, impacted at least 50,000 customers or resulted in an unplanned energy demand loss of at least 300 megawatts. The dataset was obtained from Purdue University's Laboratory for Advancing Sustainable Critical Infrastructure (LASCI), available at https://engineering.purdue.edu/LASCI/research-data/outages.

The dataset includes details about the major outages, as well as geographical, climatic, land-use characteristics, electricity consumption patterns, and economic characteristics of the affected states.

First, I will clean the dataset and perform exploratory data analysis to understand the provided information. Next, I will examine the mechanisms and dependencies of the missing data.

Finally, I will address my research question: what are the most important causes and characteristics of major power outages? I aim to develop a model that predicts the cause of a major power outage in a specific location and time. This is crucial because if energy companies can anticipate the causes of power outages, they can implement preventative measures, such as reinforcing infrastructure against weather events or enhancing security to reduce attacks.

The original raw DataFrame contains 1,534 rows, each representing an outage, and 57 columns. However, for my analysis, I will focus on a subset of these columns.
