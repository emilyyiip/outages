#Outage Analysis
Authors: Claire Wang and Emily Yip

##Overview
This is a comprehensive data science project created for DSC80 at UCSD that analyzes powers outages across the United States. The project encompasses all components of statistical analysis, starting with exploratory data analysis, followed by hypothesis testing and prediction models. The primary focus of this project is to analyze the relationships between outage duration and context of the outage.

## Introduction
Data 

The data we used contains information about major outages in different states within the United States from January 2000 to July 2016. The dataset contains the geographical location of the outages, causes of the outages, date and time of the outages, regional climates, land-use characteristics, electricity consumption patterns, and economic characteristics of the states affected.

Columns

The columns that are relevant to the analysis are as follows:

- `YEAR`: the year when the outage happened
- `MONTH`: the month when the outage event occurred
- `U.S._STATE`: the state the outage occured in
- `POSTAL.CODE`: the postal code in which the outage occured
- `OUTAGE.START.DATE`: the date the outage started
- `OUTAGE.START.TIME`: the time the outage started
- `OUTAGE.RESTORATION.DATE`: the date when the outage ended
- `OUTAGE.RESTORATION.TIME`: the time the outage ended
- `CAUSE.CATEGORY`: the cause of the outage
- `CAUSE.CATEGORY.DETAIL`: a detailed description of the category of the cause of the outage
- `OUTAGE.DURATION`: duration of outage in minutes
- `CUSTOMERS.AFFECTED`: number of customers affected by the outage event

## Data Cleaning and Exploratory Analysis

### Data Cleaning

The following outlines my data cleaning process:
1. We dropped rows which had missing start dates and restoration dates. We also converted times to a pandas datetime object, which allowed us to combine them into a column that included a date and a time.
2. We standardized column names by setting all of them to lowercase and replacing all the periods (.) with underscores (_).
3. We converted the month and outage duration to integers to simplify future numerical operations
4. We dropped the state column, since we decided to work with postal codes instead.

The head of the dataframe is displayed below:

### Univariate Analysis

In the first part of our exploratory data analysis, I want to look at the distributions of single variables.

We wanted to look at the distribution of outages by month to see what month/season seemed to have the most outages.
<><iframe
  src=""
  width="800"
  height="600"
  frameborder="0"
></iframe>
<>

We discovered that ...

### Bivariate Analysis

We looked at the number of outages for each cause category in each month to determine which categories were the most common in each month.

<><iframe
  src=""
  width="800"
  height="600"
  frameborder="0"
></iframe>
<>

We determined that ...

### Missingness Dependency

## Assessment of Missingness

### MAR Analysis
In this part, we tested the missingness of the `customers affected` column. We suspected that it would be missing based on the outage duration column due to 2 reasons:

1. Incomplete Reporting for Short Outages
For outages with a shorter duration, utility companies or reporting agencies might fail to record detailed information, such as the number of customers affected. The focus could be on resolving the issue quickly rather than documenting every aspect of the outage.

2. Longer Outages Have Better Documentation
For longer outages, there might be more investigation and reporting, as these outages have a greater impact and require comprehensive recording. This could lead to a bias where data for customers affected is more likely to be recorded when outage duration is longer.


**Null hypothesis**: The missingness of the values in the `customers affected` column is not dependent on the `outage duration` 

**Alternative hypothesis**: The missingness of the values in the `customers affected` column is dependent on the `outage duration` 

The test statistic used was the difference in mean outage durations and the significance level chosen was 0.05.

Below is the empirical distribution of the test statistics.

<> <iframe
  src="assets/empirical-dist-league.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<>

After the permutation test, we found that the p-value obtained was **0**. Therefore, we can reject the null hypothesis and conclude that the missingness of values in the `customers affected` column is dependent on the outage duration.

## Hypothesis Testing







