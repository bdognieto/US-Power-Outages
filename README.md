# US Power Outages
Final project for DSC 80 at UCSD

By: Brandon Nieto

# Introduction
In this project, I analyzed the major power outages that occured in the United States from January 2000-July 2016. The Data was fetched from Purdue University’s research data (https://engineering.purdue.edu/LASCI/research-data/outages). Acording to the Department of Energy, the major outages in this data refer to those that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300MW. The data set also provides information on geographical location of the outages, date and time of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages. 

The initial research question I chose to center my project around: what are the main causes of the major power outages and what are their associated characteristics? 

Power outages can have serious economic and social consequences, affecting millions of people each year. By analyzing the causes of major outages and their associated characteristics, we can better understand what drives outage severity and identify patterns that may help improve infrastructure resilience. This is particularly important as extreme weather events become more frequent, making it critical to understand how different causes contribute to large-scale outages.

The original raw DataFrame contains 1534 rows, corresponding to 1534 outages, and 57 columns. However, I will only focus on a few of these columns for the sake of my analysis.
|Column                |Description|
|---                |---        |
|`'YEAR'`                |Year an outage occurred|
|`'MONTH'`                |Month an outage occurred|
|`'U.S._STATE'`                |State the outage occurred in|
|`'NERC.REGION'`                |North American Electric Reliability Corporation (NERC) regions involved in the outage event|
|`'CLIMATE.REGION'`                |U.S. Climate regions as specified by National Centers for Environmental Information (9 Regions)|
|`'ANOMALY.LEVEL'`                |Oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season|
|`'OUTAGE.START.DATE'`                |Day of the year when the outage event started|
|`'OUTAGE.START.TIME'`                |Time of the day when the outage event started|
|`'OUTAGE.RESTORATION.DATE'`                |Day of the year when power was restored to all the customers|
|`'OUTAGE.RESTORATION.TIME'`                |Time of the day when power was restored to all the customers|
|`'CAUSE.CATEGORY'`                |Categories of all the events causing the major power outages|
|`'OUTAGE.DURATION'`                |Duration of outage events (in minutes)|
|`'DEMAND.LOSS.MW'`                |Amount of peak demand lost during an outage event (in Megawatt) [but in many cases, total demand is reported]|
|`'CUSTOMERS.AFFECTED'`                |Number of customers affected by the power outage event|
|`'TOTAL.PRICE'`                |Average monthly electricity price in the U.S. state (cents/kilowatt-hour)|
|`'TOTAL.SALES'`                |Total electricity consumption in the U.S. state (megawatt-hour)|
|`'TOTAL.CUSTOMERS'`                |Annual number of total customers served in the U.S. state|
|`'POPPCT_URBAN'`                |Percentage of the total population of the U.S. state represented by the urban population (in %)|
|`'POPDEN_URBAN'`                |Population density of the urban areas (persons per square mile)|
|`PC.REALGSP.CHANGE`                |Percentage change of per capita real GSP from the previous year (in %)|

# Data Cleaning and Exploratory Data Analysis

# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model 

# Fairness Analysis
