SQL syntax to calculate MoM Sales change
~~~ SQL
SELECT 
  sub.Month,
  sub.Sales,
  sub.MoM_Change,
  AVG(sub.MoM_Change) OVER () AS Avg_MoM_Change, -- Avg MoM Change
FROM
(/*Calculating Difference of Sales MoM*/
SELECT
  Month,
  Sales,
  COALESCE(Sales - LAG(Sales,1) OVER (ORDER BY Month),0) AS MoM_Change -- MoM Sales Change
FROM `single-being-353600.Projection_Activity.Faux_Sales_Data`
ORDER BY Month) AS sub
ORDER BY sub.Month;
~~~

SQL syntax to prepare dataset forecasting in python

~~~ SQL
/*Preparing for Forecasting in Python*/
SELECT 
  CAST(CONCAT('2023','-','0',sub.Month,'-','01') AS date) Date_, -- putting date in the ISO 8601
  sub.Sales,
  sub.MoM_Change,
  AVG(sub.MoM_Change) OVER () AS Avg_MoM_Change,
FROM
(/*Calculating Difference of Sales MoM*/
SELECT
  Month,
  Sales,
  COALESCE(Sales - LAG(Sales,1) OVER (ORDER BY Month),0) AS MoM_Change
FROM `single-being-353600.Projection_Activity.Faux_Sales_Data`
ORDER BY Month) AS sub
ORDER BY sub.Month
~~~
