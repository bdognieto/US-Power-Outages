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
The first step is to clean the data to make sure it is suitable for effective analysis. 
## Data Cleaning
1. For cleaning my data I first converted all the columns to the correct data types, as I found that most were stored as string objects when they truly represented numeric values, such as `ANOMALY.LEVEL`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`. 
2. Next, I combined the `OUTAGE.START.DATE` and `OUTAGE.START.TIME` columns into one Timestamp object in an `OUTAGE.START` column. I did the same for `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME`.
3. I then checked that `ANOMALY.LEVEL` and `CLIMATE.CATEGORY` were in agreement as these columns were most likely going to be important for my analysis later. In this dataset, anomaly levels below `-0.5` should correspond to `"cold"`, levels above `0.5`should correspond to `"warm"`, and values in between should correspond to `"normal"`.
4. Finally, I cleaned the `OUTAGE.DURATION` column by treating outages recorded as `0` or `1` minute as missing values. In the context of major power outages, these entries are likely recording issues rather than meaningful durations, so replacing them with `NaN` better reflects the data generating process.

The first few rows of this cleaned DataFrame are shown below, with a portion of columns selected.
|   YEAR |   MONTH | STATE     | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     |   OUTAGE.DURATION | OUTAGE.START        |
|-------:|--------:|:----------|:-------------------|----------------:|:-------------------|:-------------------|------------------:|:--------------------|
|   2011 |       7 | Minnesota | East North Central |            -0.3 | normal             | severe weather     |              3060 | 2011-07-01 17:00:00 |
|   2014 |       5 | Minnesota | East North Central |            -0.1 | normal             | intentional attack |               nan | 2014-05-11 18:38:00 |
|   2010 |      10 | Minnesota | East North Central |            -1.5 | cold               | severe weather     |              3000 | 2010-10-26 20:00:00 |
|   2012 |       6 | Minnesota | East North Central |            -0.1 | normal             | severe weather     |              2550 | 2012-06-19 04:30:00 |
|   2015 |       7 | Minnesota | East North Central |             1.2 | warm               | severe weather     |              1740 | 2015-07-18 02:00:00 |

## Expolratory Data Analysis

### Univariate Analysis
In my exploratory data analysis, I first perform univariate analysis to examine the distribution of single variables.

First, I created a `plotly` histogram to see the distribution of outage causes using the `CAUSE.CATEGORY` column

The plot shows that **severe weather** was by far the most common cause of major outages, followed by intentional attack. This suggests that extreme weather is the dominant driver of large outage events in the dataset.


# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model 

# Fairness Analysis
