# Virtual-Reality-

## Group 2: Mary-Elizabeth Vogel, Vidul Dasan, Ariel Lin, Mirian Ruanova, Ahmed Saleem, Blaze Hickey

## Introduction: 
Our group is testing the **hypothesis:** A high crime rate for violent crime in San Francisco neighborhoods negatively affects the price of home values for a given neighborhood.

### Data We Used: 
We needed housing and crime data, and preferred free online resources for this. We got our housing data from Zillows online [Housing Data portal](https://www.zillow.com/research/data/), specifically the **Zillow Home Value Index**, and got our crime data from the [San Francisco Open Data Online Portal](https://datasf.org/opendata/). We used [Police Department Incident Reports: 2018 to Present](https://data.sfgov.org/Public-Safety/Police-Department-Incident-Reports-2018-to-Present/wg3w-h783). 

## Cleaning The Data: 
### Housing Data: 
To get the housing data into a format we can use we had to isolate the geography to neighborhoods in San Francisco and making sure the timeframe starts on January 2018 and ends on December 2022. 

### Crime Data: 
To clean the crime data and get it into a format we can work with we first isolated which reports were filed by police, as the crime data included reports filed online by private citizens. We then categorized crime into violent, non-violent, and removal as not all police reports filed do not fall into criminal activity. 

### Preparing for statistical analysis: 
Once we had the data cleaned we had to join the tables to make sure we can run a meaningful statistical test. 

## Running our analysis: 
### Pearson Coefficient Correlation test: 


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
