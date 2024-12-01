<h1>
  Outage Analysis
  <img id="theme-toggle" 
       src="https://img.icons8.com/ios-filled/50/000000/light-on.png" 
       alt="Toggle Light/Dark Mode" 
       style="cursor: pointer; width: 30px; margin-left: 10px; background-color: white; border-radius: 50%;">
  <span id="toggle-text" style="font-size: small; margin-left: 5px;">Turn me on/off</span>
</h1>

<!-- Add the script for light/dark mode functionality -->
<script>
    const toggleButton = document.getElementById('theme-toggle');
    const savedTheme = localStorage.getItem('theme');

    // Apply the saved theme if it exists
    if (savedTheme) {
        document.body.classList.toggle('dark-mode', savedTheme === 'dark');
        toggleButton.src = savedTheme === 'dark' 
            ? 'https://img.icons8.com/ios/50/ffffff/light-on.png' 
            : 'https://img.icons8.com/ios-filled/50/000000/light-on.png';
        toggleButton.style.backgroundColor = savedTheme === 'dark' ? 'black' : 'white';
    }

    // Add event listener to toggle button
    toggleButton.addEventListener('click', () => {
        const isDarkMode = document.body.classList.toggle('dark-mode');
        toggleButton.src = isDarkMode 
            ? 'https://img.icons8.com/ios/50/ffffff/light-on.png' 
            : 'https://img.icons8.com/ios-filled/50/000000/light-on.png';
        toggleButton.style.backgroundColor = isDarkMode ? 'black' : 'white';

        // Save the current theme in localStorage
        localStorage.setItem('theme', isDarkMode ? 'dark' : 'light');
    });
</script>

<!-- Add CSS for light and dark mode -->
<style>
    /* Light mode (default) styles */
    body {
        background-color: white;
        color: black;
        transition: background-color 0.3s, color 0.3s;
    }

    /* Dark mode styles */
    body.dark-mode {
        background-color: black;
        color: white;
    }

    /* Light bulb styling */
    #theme-toggle {
        border-radius: 50%;
        padding: 5px;
        transition: background-color 0.3s;
    }
</style>

Authors: Claire Wang and Emily Yip

## Overview
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
<iframe
  src=""
  width="800"
  height="600"
  frameborder="0"
></iframe>

We discovered that ...

### Bivariate Analysis

We looked at the number of outages for each cause category in each month to determine which categories were the most common in each month.

<iframe
  src=""
  width="800"
  height="600"
  frameborder="0"
></iframe>

We determined that ...

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

<iframe
  src=""
  width="800"
  height="600"
  frameborder="0"
></iframe>

After the permutation test, we found that the observed test statistic was **0** p-value obtained was **0**. Therefore, we can reject the null hypothesis and conclude that the missingness of values in the `customers affected` column is dependent on the outage duration.

## Hypothesis Testing

We performed a hypothesis test to determine whether outages caused by fuel supply emergencies tend to last longer.

Null hypothesis: The median outage duration caused by fuel supply emergencies is the same as the population median outage duration.

Alternative hypothesis: The median outage duration caused by fuel supply emergencies is more than the population median outage duration.

To test this hypothesis, we used a bootstrap resampling approach. Specifically, we generated a bootstrap distribution of the median outage durations by resampling from the population data and compared the observed median outage duration for outages caused by fuel supply emergencies to this bootstraped distribution.

Significance Level: 0.05

Below is the empirical distribution of the test statistics:

<iframe
  src=""
  width="800"
  height="600"
  frameborder="0"
></iframe>

After doing the permutation test, we discovered that observed test statistic was **13484.026315789473** and the p value obtained was **0**. Given that the p-value of 0 is less than the significance level of 0.05, we reject the null hypothesis. Therefore, we conclude that outages caused by fuel supply emergencies tend to have a longer median duration than the population median outage duration. 

## Framing a Prediction Problem

Our prediction problem will be predicting outage duration using other columns in our dataset, which extends our project theme, and we suspect it can be predicted from other variables.

## Baseline Model

We selected a subset of columns from the dataset and focused on features we deemed as relevant: `cause_category`, `month`, `climate_region`, `postal_code`, `customers_affected`, `res_price`, `demand_loss_mw`, and `outage_duration`.

We noticed that `demand_loss_mw` and `customers_affected` had lots of missing values, so we imputed them based on their respective cause category. We dropped the remaining missing values in our subset of data.

We will use a linear regression model to make our predictions, and use root mean squared error to evaluate it.

We transformed the following features:

- `postal_code`: This is a nominal categorical variable that was transformed using **OneHotEncoding**.
- `climate_region`: This is a nominal categorical variable that was transformed using **OneHotEncoding**.
- `cause_category`: This is a nominal categorical variable that was transformed using **OneHotEncoding**.

The remaining features were passed into the model without changes.

After fitting the model, its root mean squared error was **224828.4913697597**, and its coefficient of determination was 0.028334274869406983, meaning that our features were not very effective in predicting the outage duration.

## Final Model












