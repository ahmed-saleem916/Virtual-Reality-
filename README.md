# Virtual-Reality-

## Group 2: Mary-Elizabeth Vogel, Vidul Dasan, Ariel Lin, Mirian Ruanova, Ahmed Saleem, Blaze Hickey

## Introduction: 
Our group is testing the **hypothesis:** A high crime rate for violent crime in San Francisco neighborhoods negatively affects the price of home values for a given neighborhood.

### Data We Used: 
We needed housing and crime data, and preferred free online resources for this. We got our housing data from Zillows online [Housing Data portal](https://www.zillow.com/research/data/), specifically the **Zillow Home Value Index**, and got our crime data from the [San Francisco Open Data Online Portal](https://datasf.org/opendata/). We used [Police Department Incident Reports: 2018 to Present](https://data.sfgov.org/Public-Safety/Police-Department-Incident-Reports-2018-to-Present/wg3w-h783). 

## Cleaning The Data: 

### Housing Data: 
To get the housing data into a format we can use we had to isolate the geography to neighborhoods in San Francisco and making sure the timeframe starts on January 2018 and ends on December 2022. In order to use this data for a linear regression, the csv needed to be transposed so that the timestamp in the rows became the index of the columns. We later used the pd.melt() function to add the neighborhood column back into the final dataset. In the end we had average house values per month per neighborhood in a format that could merge with the crime data

### Crime Data: 
After removing any null values and irrelevent columnsfrom the SF crime csv, it became apparent that there was a fair amount of missing data in string format. Any data with the word "False" where the column was not a boolean was dropped from the dataset. In addition, incident categories with spelling inconcistencies were fixed. Any row where the Incident Datetime matched the Incident Reporttime was also dropped ensuring that every incident recorded in the dataset was unique. Finally, any incident that was reported by a member of the public rather than a member of law enforcement was also removed. We then categorized crime into violent, non-violent, and removal as not all police reports filed do not fall into criminal activity. 

### Preparing for statistical analysis: 
Once we had the data cleaned we had to join the tables to make sure we can run a meaningful statistical test. 

## Running our analysis: 
### Null Hypothesis: 
A high crime rate for violent crime in San Francisco neighborhoods negatively affects 	the price of home values for a given neighborhood.

### Prepare the cleaned Zillow housing data to perform the statistical test.¶
#### Python File: [Population by neighborhood in San Francisco](https://github.com/ahmed-saleem916/Virtual-Reality-/blob/main/population_sf_neighborhoods.ipynb)
1. We found certain difficulties in obtaining the population of each of the neighborhoods of San Francisco in the period of time between 2018 and 2022 so,  we used the following csv that contains an estimate of the inhabitants of San Francisco during the period studied. It should be noted that for this analysis we assumed that the population of each neighborhood remained constant throughout the period under study.
   * [Estimated population in San Francisco by neighborhood](https://data.sfgov.org/widgets/ejrc-vnwu?mobile_redirect=true)
#### File: [Prepare the cleaned Zillow housing data to perform the statistical test.](https://github.com/ahmed-saleem916/Virtual-Reality-/blob/main/zillow_df_regression.ipynb)
1. We prepared the house value to merge with the crime data. For that, we needED to turn the columns into rows since each month from January 2018 until December 2022 is a column in our house value data frame. 
    * To turn the DataFrame from wide into long we used the function `pd.melt()`.
       ```python
        df_melted = pd.melt(incident_report, id_vars=['Neighborhood'], value_vars=['2018-01', '2018-02', '2018-03', '2018-04', '2018-05',".."], var_name='Date', value_name='Price')
        ```
    * In order to get the the median house value per period (year-month) and neighborhood, we grouped by `Neighborhood` and `Date` columns and we used the function `.sum()`. Since there is only a single average value for each neighborhood in each period determined by our columns. We had 37 rows (36 neighborhood) and 61 columns (60 months from January 2018 until December 2020).
      ```python
        gruopbymelted = df_melted.groupby(["Neighborhood","Date"]).sum()
        ```
2. Finally, we saved our data frame as a csv file for use in our future statistical analysis usisng the function `.to_csv(zillow_regression.csv)`.

### Calculation of the correlation between the value of housing and the crime rate in each neighborhood.
#### File: Statistical_analysis_by_neighborhood_total.ipynb
1. We have cleaned the crime data set in the previous steps so we are ready to shape our data frame so that we can work with it and test our hypothesis.
    * 1.1. We get rid of the columns we wont need for running our analysis usisng the function `.drop(columns=[])`.
    * 1.2. We will merge our data frames using the comon index `Date` so we nedded to get the `year-month`from the `Incident Datetime`column. To performn that we implemented the following function:

      ```python
        def extract_year_month(date_str):
          return "-".join(date_str.split("-")[:2])
          incident_reports["Incident Year-Month"] = incident_reports["Incident Date"].apply(extract_year_month)
        ```
    * 1.3 We reduced the number of columns to those we will need: `Analysis Neighborhood`, `Incident Year-Month` and `Incident Category`.
2. To get our final data frame ready to merge with the Zillow data frame, we grouped by `Analysis Neighborhood` and `Incident Year-Month` using the function `.count()`, because we wanted to know how many times `neighborhood per month per year` was repeated throughout our data frame so we would get the total number of crimes for each neighborhood in each time period.
    * 2.1. Reset the Index from the new crime data frema to merge with the Zillow data ser usisng `df_reset_idex()`.
    * 2.2. Renamed the columns to match with the Zillow data frame columns with the `.rename(columns={})`function.

      ```python
        grouped_index = grouped_index.rename(columns={"Analysis Neighborhood":"Neighborhood","Incident Year-Month":"Date","Incident Category":"Number of Crimes"})
      ````
    * 2.3. We merged the  Zillow data frame with the crime data frame with the function `pd.merge(crime_regression_rename,zillow_df,on=["Neighborhood","Date"])`.

3. We decided to set crime rates as our variable instead of the number of crimes. Otherwise, our analysis may not provide an accurate representation of the crime in each neighborhood because it would not take into account the size of the population in each neighborhood.  
      ```python
        crimes_price_population["Crime Rate"] =(crimes_price_population["Number of Crimes"]/crimes_price_population["Population"])*10000
      ```

4. We decided the most appropriate statistical analysis to probe our hypothesis was the *Pearson Correlation Coefficient*. To performn the analysis we would use the following functions:
    * 4.1 `groupby("Neighborhood")` because we want to know the correlation between crime and housing prices in each neighborhood, not in the whole city.
    * 4.2 `.corr()` to calculate the correlation coeffcient between the two variables. 
    * 4.3 `.iloc[]` to filter and `.reset_index()`to reset the index.

      The function`.corr()` returns a *matrix** as the *below table* (). To avoid that has to filter by index using the function .iloc[0::2,-1], which means: from the row **0** up to the end of the data frame **:**, select the step **:2** on the last column of the data fram **-1**.


      | Neighborhood          | Pearson Coefficient|
      | --------------------- |:-------------:| 
      | Bayview Hunters Point | 0.320309      | 
      | Bayview Hunters Point | 1.000000      |  
      | Bernal Heights        | 0.182127      |
      | Bernal Heights        | 1.000000      |   

  ```python
    corr_crimes_prices =crimes_price_population_correlation.groupby("Neighborhood")[["Price","Crime Rate"]].corr().iloc[0::2,-1].reset_index()
    corr_crimes_prices=corr_crimes_prices.rename(columns={"Crime Rate":"Pearson Coefficient"})
    corr_crimes_prices=corr_crimes_prices[["Neighborhood","Pearson Coefficient"]]
  ```
## Major Findings: 
Neighborhoods with the highest average home values tended to have fewer crimes and lower crime rates, however, there is no statistically significant relationship between crime rate or housing value. 

## Shortcomings and Limitations: 
### Home Value:
Housing values are estimates from [Zillow (ZHVI)](https://www.zillow.com/research/data/). This is a companies estimate for what a home should be worth. This is a good resource but compaing against other available sources will make sure we have the most accurate measure for San Francisco home values by neighborhood.

### Crime Data:
The police need to file a report for that crime to show up in the dataset. This means that all crimes that are commited do not show up in this report because [not all crime that is committed is reported](https://www.pewresearch.org/fact-tank/2020/11/20/facts-about-crime-in-the-u-s/). Furthermore, filing a police report does not automatically create public awareness as this does not always reach the news or other public facing information sources.  

### Population Data:
Population data over time is an estimate. 2020 data from US Census is the most accurate, however, the rest of the population data has to be interpolated. We used the [American Community Survey](https://data.census.gov/table?q=All+5-digit+ZIP+Code+Tabulation+Areas+fully/partially+contained+within+San+Francisco-Redwood+City-South+San+Francisco,+CA+Metro+Division+within+San+Francisco-Oakland-Hayward,+CA+Metro+Area&g=050XX00US06075&tid=ACSST5Y2021.S0101&moe=false&tp=true) data available on the [US Census website](https://data.census.gov/). 

## Professional Implications: 
Create an app/program that forecasts home values based on variables we can input (Inflation rate, local weather, unemployment rate, school districts) Anyone with an interest in housing can get value from this as home values can be volatile and reliably predicting which way it might go in a neighborhood can provide economic opprotunities for residents or businesses. 

## Moving Forward: 
### Improving Our Analysis:
We need to determine additional variables which impact housing value and find reliable sources for this information. This can come from [academic research](https://www.sciencedirect.com/science/article/pii/S1877050922001922) which looks at what drives home value changes as well as finding resources that provide reliable data we can use for analyzing each variable. We would then use multiple linear regression and additional statistical analysis methods to determine which variables have a stronger impact on home value, and continue building out a model which would eventually determine housing value trends in a specified geographic area. 
