# Blinkit-Sales-Analysis

**1. Calculate the average sales, minimum sales, and maximum sales for each item type.**
``` sql
select `Item Type`, min(Sales), max(sales), avg(sales) from blinkitt
group by `Item Type`;
```
