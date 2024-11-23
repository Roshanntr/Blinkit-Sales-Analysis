# Blinkit-Sales-Analysis

**1. Calculate the average sales, minimum sales, and maximum sales for each item type.**
``` sql
select `Item Type`, min(Sales), max(sales), avg(sales) from blinkitt
group by `Item Type`;
```
**2.Find the outlet type with the highest average rating.**
``` sql
select `Outlet Type`,avg(Rating) as Average_Rating from blinkitt
group by `Outlet Type`
order by Average_Rating desc
limit 1;
```
**3. Find the year with the highest total sales across all outlets.**
``` sql
select  `Outlet Establishment Year`, sum(Sales) as Total_Sales
from blinkitt
group by `Outlet Establishment Year`
order by Total_Sales desc
limit 1;
```
**4. Calculate the total sales, average weight, and number of items for each outlet type.**
``` sql
select `Outlet Type`, sum(sales), avg(`Item Weight`), count(`Item Identifier`)
from blinkitt
group by `Outlet Type`;
```
**5. Analyze trends in sales and ratings for outlets over the years.**
``` sql
select `Outlet Establishment Year`,avg(sales), avg(Rating)
from blinkitt
group by `Outlet Establishment Year`
order by `Outlet Establishment Year` ;
```
**6. Get the most frequently sold item type in each outlet.**
``` sql
select `Outlet Identifier`, `Item Type`, COUNT(*) AS Count
from blinkitt
group by  `Outlet Identifier`, `Item Type`
order by `Outlet Identifier`, Count DESC;
```
**7. Rank outlets based on their total sales.**
``` sql
select `Outlet Identifier`, sum(sales),
rank() over( order by sum(sales) desc) as Sales_Rank
from blinkitt
group by `Outlet Identifier`;
```
**8. Identify outlets with total sales above the average total sales of all outlets.**
``` sql
select `Outlet Identifier`, sum(Sales) AS Total_Sales
from blinkitt
group by `Outlet Identifier`
having sum(Sales) > (select avg(sum(Sales)) 
                     from blinkitt 
                     group by `Outlet Identifier`);
```
**9. Identify items where sales are higher than the average sales of their item type.**
``` sql
SELECT `Item Identifier`, `Item Type`, Sales
FROM blinkitt
WHERE Sales > (SELECT AVG(Sales) 
               FROM blinkitt AS sub 
               WHERE sub.`Item Type` = blinkitt.`Item Type`);
```               
**10. Find the top 3 items with the highest sales in each outlet.**
``` sql
WITH RankedData AS (
    SELECT `Outlet Identifier`, `Item Identifier`, Sales,
           RANK() OVER (PARTITION BY `Outlet Identifier` ORDER BY Sales DESC) AS Ranks
    FROM blinkitt
)
SELECT `Outlet Identifier`, `Item Identifier`, Sales, Ranks
FROM RankedData
WHERE Ranks <= 3;
```
