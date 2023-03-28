# Virtual-Reality-

1. We prepared the house value date to merge with the crime data. For that, we need to turn the columns into rows since each month from January 2018 until December 2022 is a column. We are interested in changing the shape of our house value DataFrame and turn it from wide to long.
    * For turn the DataFrame from wide into long we used the function *pd.melt()*
       ```python
        df_melted = pd.melt(incident_report, id_vars=['Neighborhood'], value_vars=['2018-01', '2018-02', '2018-03', '2018-04', '2018-05',".."], var_name='Date', value_name='Price')
        ```
    * In order to get the the medin house value per period of time (year-month) and neighborhood. We grouped by "Neighborhood" and "Dat" and we sum it since there is one unique house value for each neighborhood given a periord of time. 
      ```python
        gruopbymelted = df_melted.groupby(["Neighborhood","Date"]).sum()
        ```
    * 

       
