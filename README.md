# NoName Game Performance Analysis

[NNGame.pdf](https://github.com/redpanda-fi/rfm_sql/files/14511397/NNGame.pdf)

The goal of this project is to demonstrate proficiency in creating clear and concise reports in PowerBI using DAX and focusing on key metrics including conversion rate, customer retention (via cohort analysis), and Average Revenue Per Paying User (ARPPU). 

## Description of Dataset

The dataset is a sample data from a hypothetical free-to-play mobile game. It contains three tables: account, account_date_session and iap_purchase. account contains user profiles, iap_purchase contains in-app purchases by the users, and account_date_session contains the number of sessions for the users for the days they have been active. All the tables contain data for the year of 2016.

## Report Measures

To provide a comprehensive overview of NoName Game's performance, we've selected the following key metrics:

* Daily Active Users (DAU) and its trend over time
* Revenue and its trend over time
* New User Registrations and their trend over time
* Conversion rate
* Average Revenue Per Paying User (ARPPU)
* Customer Retention Rate analyzed by cohort

We pay special attention to DAU and revenue, broken down by different regions. This breakdown will help us understand which areas are driving the most activity and revenue. 

In addition, we delve into details about what players are buying and how much they're spending on different in-game products to understand their preferences as well as the potential of these products.

### Some DAX-Measures used in the report

```DAX
DAU = 
CALCULATE(
    DISTINCTCOUNT(account_date_session[account_id]),
    USERELATIONSHIP(account_date_session[DateAsInteger],DateTable[DateAsInteger])
)

Customer Retention =
VAR CurrentMonthAfter = SELECTEDVALUE(Month_After[Value])
VAR CurrentFirstPurchaseMonth = SELECTEDVALUE(iap_purchase[Cohort])
RETURN
    CALCULATE(
        DISTINCTCOUNT(iap_purchase[account_id]),
        FILTER(
            iap_purchase,
            EOMONTH(iap_purchase[created_time],0) = EOMONTH(CurrentFirstPurchaseMonth, CurrentMonthAfter)
        )
    )/
    DISTINCTCOUNT(iap_purchase[account_id])
```

## Some Insights

### DAU & Revenue Geographic Mix

While only 11% of daily active users are from US&Canada region, a third of the game's revenue (35%) comes from this region. Remarkably, there is a wide disparity in revenue per user, ranging from $18.62 in Asia-Pacific to $54.11 in the US&Canada. Is this contrast viewed as a problem or an opportunity? Investigating why this happens could give us insight into how people from different places engage with the game.

### IAP Conversion Rate

The conversion rate for all regions combined is 1.37%, which is a bit higher than the average conversion rate for in-app purchases in mobile games, typically between 2% and 3%. However, when we break down the conversion rate by region, we see that it varies significantly from region to region. This insight helps to identify regions where it would be beneficial to focus additional efforts on increasing interest in purchasing in-app products.

## Conclusion

Overall, the report makes it clear that the NoName Game has a lot of room for improvement. Understanding how player behavior evolves can help improve the game and how it’s marketed.
