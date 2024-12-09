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
  
- `RED.PRICE`: monthly electricity price in the residential sector (cents/kilowatt-hour)

- `DEMAND.LOSS.MW`: amount of peak demand lost during an outage event (in Megawatt)

## Data Cleaning and Exploratory Analysis

### Data Cleaning

The following outlines my data cleaning process:
1. We dropped rows which had missing start dates and restoration dates. We also converted times to a pandas datetime object, which allowed us to combine them into a column that included a date and a time.
2. We standardized column names by setting all of them to lowercase and replacing all the periods (.) with underscores (_).
3. We converted the month and outage duration to integers to simplify future numerical operations
4. We dropped the state column, since we decided to work with postal codes instead.

The head of the dataframe is displayed below:

### Univariate Analysis

In the first part of our exploratory data analysis, we wanted want to look at the distributions of single variables to determine if there were any relationships that we wanted to study further.

We wanted to look at the distribution of outages by month to see what month/season seemed to have the most outages.
<iframe
  src="assets/distribution_outage_month.html"
  width="100"
  height="800"
  frameborder="0"
></iframe>
We noticed that there were more outages in the summer and the winter compared to the fall and spring.

### Bivariate Analysis

We also wanted want to look at relationships between multiple variables.

We looked at the number of outages for each cause category in each month to determine which categories were the most common in each month.

<iframe
  src="assets/causes_by_month.html"
  width="100"
  height="800"
  frameborder="0"
></iframe>

The distributions of the cause category by month did not vary as much as we expected to, with the majority of outages were caused by severe weather every month.

We also looked at the distribution of outage duration by cause category, since this relates to the theme of our analysis.

<iframe
  src="assets/outage_duration_by_cause_category.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The category that had the highest median number of outage durations is fuel supply emergencies, which makes sense since they are more likely to be triggered by natural disasters, which take longer to restore power.

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
  src="assets/empirical_missing.html"
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
  src="assets/empirical_hypo.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After doing the permutation test, we discovered that observed test statistic was **13484.026315789473** and the p value obtained was **0**. Given that the p-value of 0 is less than the significance level of 0.05, we reject the null hypothesis. Therefore, we conclude that outages caused by fuel supply emergencies tend to have a longer median duration than the population median outage duration. 

## Framing a Prediction Problem

Our prediction problem will be predicting outage duration using other columns in our dataset. We chose this problem since we suspect outage duration to be predictable from other variables, and it fits into our project theme.

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

We decided not do predict outage duration this time because, as you can see from the plot, there were many outliers, which made the regression difficult. Outliers make it hard to fit the line and to evaluate error. 

<iframe
  src="assets/outage_durations.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As you can see, there are many outliers in the data. Instead, for our final model we decided to predict the cause category of an outage. We chose to use a random forest classifier and GridSearchCV to find the best hyperparameters for our data. We used these features:  `'month'`, `'climate_region'`, `'postal_code'`, `'res_price'`, and `'outage_duration'`. 

There were 7 different cause categories- severe weather, intentional attack, system operability disruption, equipment failure, public appeal, fuel supply emergency, and islanding.  We chose to assign these numbers to them so it could processed by our random forest model: 

`mapping = {'severe weather': 1, 'intentional attack': 2, 'system operability disruption': 3, 
           'equipment failure': 4, 'public appeal': 5, 'fuel supply emergency': 6, 
           'islanding': 7}`


For missing values, we decided to simply drop rows with them because there wasn't a significant amount (5). We then one-hot encoded `postal_code` and `climate_region`. 

For hyperparameters, we had GridSearchCV iterate through these combinations of hyperparameters: 

- `max_depth`: \[2, 4, 5, 7, 10, 15, 18\]
- `min_samples_split`: \[2, 5, 10, 20, 50, 100, 200\]
- `criterion`: \['gini', 'entropy'\]

After 5 folds of cross-validation, our best model had an accuracy of 0.94 and used these hyperparameters: `gini` as `criterion`, 18 as `max_depth`, and 2 as `min_samples_split`. 


## Model Evaluation

We decided to check for accuracy parity at different times of the year. To do this, we compared the model accuracy between June and December. Our model achieved 96% accuracy for June and 99% for December. Since both of these are close and fairly accurate, we were satisfied with the performance of our model. 




