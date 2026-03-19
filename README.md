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
<iframe
  src="assets/cause_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The plot shows that **severe weather** was by far the most common cause of major outages, followed by intentional attack. This suggests that extreme weather is the dominant driver of large outage events in the dataset.

Then, I also created a `plotly` barchart to see the distribution of the total number of outages by month across all years.
<iframe
  src="assets/month_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This plot shows that outages were most common in the **summer months**, especially around June through August, which may reflect the impact of storms, heat-related failures, and seasonal demand stress on the grid.

### Bivariate Analysis
I also conducted several bivariate analyses; A couple are shown below

I first wanted to see the relation between `OUTAGE.DURATION` and `CUSTOMERS.AFFECTED` by creating a `plotly` scatterplot
<iframe
  src="assets/customer_duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This plot shows a **positive relationship**: in general, outages that last longer tend to affect more customers. While the plot contains several extreme outliers, the overall upward trend suggests that outage scale and outage duration are related

I also created a `plotly` box plot of `CAUSE.CATEGORY` versus `OUTAGE.DURATION`, using a log-scaled y-axis to make the comparison easier to interpret.
<iframe
  src="assets/duration_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This plot shows that **fuel supply emergencies** and **severe weather** tend to be associated with longer outage durations than most other causes of power outages.

### Interesting Aggregates
I also summarized outage severity by cause category using grouped tables. One informative table I created was grouped by `CAUSE.CATEGORY` and calculated the count, mean, median, and maximum of `CUSTOMERS.AFFECTED`.
| CAUSE.CATEGORY                |   count |          mean |   median |              max |
|:------------------------------|--------:|--------------:|---------:|-----------------:|
| system operability disruption |      83 | 211066        |  69000   |      3.12535e+06 |
| severe weather                |     717 | 188575        | 110433   |      3.24144e+06 |
| equipment failure             |      30 | 101936        |  45451.5 | 900000           |
| public appeal                 |      21 |   7618.76     |      0   |  34500           |
| islanding                     |      34 |   6169.09     |   2342.5 |  48400           |
| intentional attack            |     199 |   1790.53     |      0   | 125000           |
| fuel supply emergency         |       7 |      0.142857 |      0   |      1           |

This table shows that **system operability disruption** tends to affect more customers on average, however **severe weather** is more impactful as it is significantly more common and around the same severity. 

# Assessment of Missingness
For the 1534 rows and 57 columns, this dataset contained a lot of missing `NaN` values that I would need to assess. While a column like `HURRICANE.NAMES` can be interpreted as Missing by Design, many other columns seemed to be Missing Not at Random (MNAR).

## MNAR Analysis
I wantd to specifically focus on `CUSTOMERS.AFFECTED`, which has about 29 percent of its entries missing, since I will be using it in my prediction model and will want to impute its missing values.

It is plausible that this column is **MNAR**, since the likelihood of missing data may depend on the value itself. For example, outages affecting extremely large or extremely small numbers of customers may be more difficult to accurately record, leading to missing entries. Additionally, reporting practices may vary across utility companies or regions, further contributing to missingness that depends on the unobserved value.

To better understand this, additional data/variables could help explain whether the missingness is actually **MAR** instead of MNAR.

## Missingness Dependency
To better understand the missingness of `CUSTOMERS.AFFECTED`, I created a binary indicator variable:
- `customers_missing = True` if the value is missing
- `customers_missing = False` otherwise

I then performed permutation tests to determine whether the missingness depends on other variables.

### Dependent Relationship
I found that the missingness of `CUSTOMERS.AFFECTED` **depends on** the variable `CAUSE.CATEGORY`

I first plotted the distribution of outage cuases based on `customer_missing` using a horizontal bar chart
<iframe
  src="assets/cause_missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This plot shows that the distribution of outage causes differs between missing and non-missing observations. Notably, severe weather events are much more common when customer data is recorded, while other causes appear more frequently when values are missing.

This difference was quantified using the **Total Variation Distance (TVD)** and confirmed with a permutation test, where the observed statistic was far from the null distribution.
<iframe
  src="assets/dependent_tvd.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
I calculated a following p-value of 0.0

These findings suggests the missingness is dependent of `CAUSE.CATEGORY` (likely **MAR**).

### Independent Relationship
I found that missingnes of customers affected depended on many variables in the dataset unitl I tested `PC.REALGSP.CHANGE`.

First I created a **kernel density** plot since GSP change is a numerical variable
<iframe
  src="assets/kde_gsp.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The distributions of percent change in real GSP for missing and non-missing groups largely overlap, indicating no meaningful difference between them.

For the permutation test, I used a **Kolmogorov-Smirnov test** instead of difference of absolute means which would've given an unaccurate representation of the distribution plotted above. I obtained a **large p-value**(0.28), indicating no statistically significant difference between the distributions.

This suggests that missingness is independent of this variable

### Conclusion
Overall, we find that the missingness of `CUSTOMERS.AFFECTED` is likely **MAR (Missing At Random)** rather than MCAR, since it depends on observed variables such as outage cause

# Hypothesis Testing
I will be testing whether the outage cause of **severe weather** affects more customers on average than the other causes. I only selected the relevant columns for this test which are `CUSTOMERS.AFFECTED` and `CAUSE.CATEGORY`. I then created a new column for the subset dataframe, that contained boolean values on whether the outage cause equaled `severe weather` or not. 

**Null Hypothesis:** The average number of customers affected is *the same* for severe weather outages and non-severe weather outages

**Alternative Hypothesis:** Outages caused by severe weather *affect more customers* on average

**Test Statistic:**
- Difference of group means
- mean(customers affected | severe weather) - mean(customers affected | other causes)

**Significance Level:** α = 0.05

I performed a permutation test with 500 simulations in order to generate an empirical distribution of the test statisic under the null hypothesis.

The p-value I got was 0.0, so with a standard significance level of 0.05, we reject the null hypothesis because the results are statistically significant. We conclude that on average, a power outage caused by severe weather will most likely affect more customers.

The plot below shows the observed difference against the empirical distribution of differences from the permutation tests.
<iframe
  src="assets/hyp_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

# Framing a Prediction Problem
The model I will create will attempt to predict the severity (in terms of **number of customers**) of a major power outage. This will be a linear regression model since we are predicting a continuous numerical value.

The response variabe for the model (column we are predicting) will be `CUSTOMERS.AFFECTED`. I chose this variable to measure severity compared to it's alternatives, `OUTAGE.DURATION` and `DEMAND.LOSS.MW`, because the data entries it contains are more reliable and we already assesed it's missingness. 

The evaluation metric we will be using for the model will be **R² (coefficient of determination)**. I choose this metric because it helps us compare models easily and provides an intuitive measure of model performance.

# Baseline Model

# Final Model 

# Fairness Analysis
