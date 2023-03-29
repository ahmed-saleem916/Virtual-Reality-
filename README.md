# Virtual-Reality-

## Statistical Analysis
#




### Prepare the cleaned Zillow housing data to perform the statistical test.¶
#### File: zillow_df_regression.ipynb
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
1. We have cleaned the crime data set in the previous steps so we can start. We are ready to shape our data frame so that we can work with it and test our hypothesis.
     * 1.1. We get rid of the columns we wont need for running our analysis usisng the function `.drop(columns=[])`.
    * 1.2. We will merge our data frames using the comon index `Date` so we nedded to get the `year-month`from the `Incident Datetime`column. To performn that we implemented the following function:

      ```python
        def extract_year_month(date_str):
          return "-".join(date_str.split("-")[:2])
          incident_reports["Incident Year-Month"] = incident_reports["Incident Date"].apply(extract_year_month)
        ```
    * 1.3 We reduced the number of columns to those we will need: `Analysis Neighborhood`, `Incident Year-Month` and `Incident Category`.
2. To get our final data frame ready to merge with the Zillow data frame we grouped by `Analysis Neighborhood` and `Incident Year-Month`nd we used the function `.count()`, because we wanted to know how many times `neighborhood-year-month` was repeated throughout our data frame, so we would get the total number of crimes for each neighborhood in each time period.
    * 2.1. Reset the Index from the new crime data frema to merge with the Zillow data ser usisng `df_reset_idex()`.
    * 2.2. Renamed the columns to match with the Zillow data frame columns with the `.rename(columns={})`function.

      ```python
        grouped_index = grouped_index.rename(columns={"Analysis Neighborhood":"Neighborhood","Incident Year-Month":"Date","Incident Category":"Number of Crimes"})
      ````
    * 2.3. We merged the  Zillow data frame with the crime data frame with the function `pd.merge(crime_regression_rename,zillow_df,on=["Neighborhood","Date"])`.

3. Since 
      ```python
        crimes_price_population["Crime Rate"] =(crimes_price_population["Number of Crimes"]/crimes_price_population["Population"])*10000
      ```

4. We are ready to run our statistical analysis and calculate the *Pearson Correlation Coefficient*. To performn the analysis we would use the following functions:
    * 4.1 `groupby("Neighborhood")` because we want to know the correlation between crime and housing prices in each neighborhood, not in the whole city.
    * 4.2 `.corr()`to calculate the correlation coeffcient between the two variables. 
    * 4.3 `.iloc[]` to filter and `.reset_index()`to reset the index.


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

  





   

       
