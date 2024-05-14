# sales_data_analysis (W.I.P.)
Repository for the Sales Data project across the various tools used in the analysis. 

In this analysis, our goal is to explore what kind of factors influence the revenue generated on stores, like outdoor temperature, holidays, and the unemployment rate, fuel cost, and  for example. 

First We notice how many rows we have on the first page of our table: 

![First Page](https://github.com/xDavidHx/sales_data_analysis/blob/main/Walmart%20Sales%20Data%20Table%20Page%201%20.png) 

We have plenty of data to work with; Over 6400 entries!

Next I'd like to get more familiar with the data I'm working with. How many unique stores are included here? 

We can Run a SQL QUERY to find out the Information.

```
WITH
  store_duplicates AS (
    SELECT Store, COUNT(*) AS duplicate_store_count
    FROM dehproject24.Kaggle_Datasets.walmart_sales_data
    GROUP BY Store
  )

SELECT Store
FROM store_duplicates
WHERE duplicate_store_count > 1;

```



Here's the  result: 
![Distinct Stores](https://github.com/xDavidHx/sales_data_analysis/blob/main/Distinct%20Store%20Result.png)



We notice that each distinct store is listed, but the data type of the store number (Store) is in the Integer type. That means it counted each store number that's value was greater than 1. That worked, but its not exactly the degree of confidence we need. 
Lets Change that with our next SQL query by changing the Data Type to a STRING and see if the Count of Distinct Stores is the same. 

```
SELECT 
  CAST(Store AS STRING) AS store_string,
  COUNT(DISTINCT(Store)) AS distinct_stores

FROM dehproject24.Kaggle_Datasets.walmart_sales_data
GROUP BY Store
```

Here's our new Result: 
![CAST table column from INT to STRING](https://github.com/xDavidHx/sales_data_analysis/blob/main/Distinct%20Store%20Cast%20Value%20to%20String.png) 


It looks like we have no duplicate store numbers!

Next, lets get a general picutre of what amount of revenue our collection of stores has generated on a week-by-week basis from earliest to latest. 

The SQL Query to write that follows here:

```
SELECT
  SUM(Weekly_Sales) AS region_sales_per_week,
  FORMAT_DATE('%m/%d/%y', date) AS date_in_MMDDYY
 

FROM dehproject24.Kaggle_Datasets.walmart_sales_data

GROUP BY date
ORDER BY date
```
And the Result is: 

![Weekly Regional Sales](https://github.com/xDavidHx/sales_data_analysis/blob/main/weekly%20sales%20by%20date%20SQL%20results%20.png)

Looks Good! So now lets get our Revenue amounts into a 2 decimal place format to reflect currency:
```
SELECT
  ROUND(SUM(weekly_sales), 2) AS region_sales_per_week,
  FORMAT_DATE('%m/%d/%y', date) AS date_in_MMDDYY
 

FROM dehproject24.Kaggle_Datasets.walmart_sales_data

GROUP BY date
ORDER BY date
```
Here's the result: 
![Rounded Weekly Regional Sales](https://github.com/xDavidHx/sales_data_analysis/blob/main/Rounded%20Weekly%20sales%20RESULTS%20.png)

Much Better! 

Here's a glimpse of what we're looking at in terms of weekly sales for our region. I've included each year to make sure we have a picture of all of our data's story. 

![Regional Sales 2010-2012 in Tableau](https://github.com/xDavidHx/sales_data_analysis/blob/main/Screen%20Shot%202024-05-14%20at%202.21.30%20PM.png)

At first sight, we can see that there is a common pattern each year follows. Each year starts out lower than most of the same year, then ends on a very high note, only to drop trememdously a the start of the following year. 

There's also another thing I'm seeing. In the United States, Each spike in revenue tends to correspond to a national holiday.

Quick Side note: After Noticing that our data visualization had dates ending in October of 2012, I decided to make sure they did with a query here to see if our data does in fact end before Winter holiday 2012)

![Filter for all 2012 Dates](https://github.com/xDavidHx/sales_data_analysis/blob/main/Query%20for%20revised%202012%20dates%20.png)

Our Result was clear!

![All 2012 dates](https://github.com/xDavidHx/sales_data_analysis/blob/main/2012%20dates%20query%20results.png)

So it does in fact show our data ends before winter holiday 2012!

The next thing I'd love to know is how do holidays affect our revenue after noticing the large spikes during Typical holiday times in the United States. 




#This is a Work in progress! Next time we'll use this data in Tableau to get a full picture of what our regional sales are. 
#then we'll compare that to what the other columns data contains!




