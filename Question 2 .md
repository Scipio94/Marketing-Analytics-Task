# Question 2

Audience segments inform key strategic initiatives for several teams across MJFF. Identifying relevant segments helps the communications team to craft targeted messaging, the email team to set up journeys, and the marketing team to predict conversions. The marketing team wants to understand if they can segment their existing database into meaningful and distinct categories, as well as understand their behavior, to inform specific targeting tactics.

This dataset (sample data) looks at around 50K contacts in the database along with information collected over time. What segments would you recommend based on the data available?

Use tab titled – Audience Data

Access dataset [HERE](https://docs.google.com/spreadsheets/d/1dGFzsvhVzTSr971M5tdpPUhRXZGFhPOLe59FRomsr3w/edit?usp=sharing)

## Analysis

![Audience Data](https://user-images.githubusercontent.com/112409778/219527466-0914a832-77c1-4b52-b0e5-5ec40781e0f5.png)

**Analysis of PD Connection Variable**
~~~ SQL
/*PD Connection Calculations*/
SELECT 
  DISTINCT PD_Connection, COUNT(*) AS Count, 
  CONCAT(ROUND(COUNT(*)/ SUM(CAST(COUNT(*) AS numeric)) OVER() * 100.00),'%')AS Pctge,-- percentage PD_Connection 
  SUM(CAST(COUNT(*) AS numeric)) OVER() AS Total --Calculating Total records
FROM `single-being-353600.Analytics_Assessment_Data.Audience_Data` 
GROUP BY PD_Connection
ORDER BY Count DESC;
~~~

**Result:**
Use PD Connection Tab - Access results [HERE](https://docs.google.com/spreadsheets/d/1GeEYAo0l2VeqsY5q61uCPChsCm2qV40xmnArLA2CDvI/edit?usp=sharing)

The PD Connection variable informs the communication team the source of their connections in the database which has a correlation to donations.

**Analysis of the correlation between acquisition channel,Total clicks,and donations**

~~~ SQL
/*Acquisition Channel Donation Calculations*/
SELECT 
   DISTINCT Acquisition_channel, COUNT(*) OVER (PARTITION BY Acquisition_channel) AS Count,--Count per Channel,
   SUM(CAST(Emails_Clicked_in_the_last_three_months AS numeric)) OVER (PARTITION BY Acquisition_channel) AS Total_click_last_3_months,
   CONCAT('$',ROUND(AVG(CAST(Donation_Amount AS float64)) OVER (PARTITION BY Acquisition_channel ),2)) AS Avg_Donation,--Avg donation per chnl
   CONCAT('$',ROUND(SUM(CAST(Donation_Amount AS float64)) OVER (PARTITION BY Acquisition_channel),2)) AS Total_Donation_Channel , --total donations per 
   CONCAT('$',ROUND(SUM(CAST(Donation_Amount AS float64)) OVER (),2)) AS Total_Donation_Overall -- Total donations 
FROM `single-being-353600.Analytics_Assessment_Data.Audience_Data` 
ORDER BY Count DESC;
~~~
**Result:**

Use Acquisition Channel Donations Tab - Access results [HERE](https://docs.google.com/spreadsheets/d/1GeEYAo0l2VeqsY5q61uCPChsCm2qV40xmnArLA2CDvI/edit?usp=sharing)

The Manual import segement of the acquisition channel variable received the most donations in total and had the second total highest total clicks in the last three months. 

**Analysis of correlation between acquisition channel,Total clicks,first date of donations ,and donations**

~~~ SQL
/*Acquisition Channel Donation Calculations Filtered by Contacts with contact created date and first donation date*/
SELECT 
   DISTINCT Acquisition_channel, COUNT(*) OVER (PARTITION BY Acquisition_channel) AS Count,--Count per Channel,
   SUM(CAST(Emails_Clicked_in_the_last_three_months AS numeric)) OVER (PARTITION BY Acquisition_channel) AS Total_click_last_3_months,
   CONCAT('$',ROUND(AVG(CAST(Donation_Amount AS float64)) OVER (PARTITION BY Acquisition_channel ),2)) AS Avg_Donation, --Average Donation per channel
   CONCAT('$',ROUND(SUM(CAST(Donation_Amount AS float64)) OVER (PARTITION BY Acquisition_channel),2)) AS Total_Donation_Channel , --Sum of donations oer channel 
   CONCAT('$',ROUND(SUM(CAST(Donation_Amount AS float64)) OVER (),2)) AS Total_Donation_Overall, -- Total donations in the dataset
   EXTRACT(DAY FROM AVG(CAST(First_donation_date AS Date) - CAST(Contact_Created_Date AS date)) OVER (PARTITION BY Acquisition_channel)) AS Average_Days_Unitl_Donation
    --Calculating Average days until first donation, casting date, subtracting to create interval, and extracting average day(s) from the timestamp
FROM `single-being-353600.Analytics_Assessment_Data.Audience_Data` 
WHERE First_donation_date IS NOT NULL
ORDER BY Count DESC;
~~~

**Result:**

Use Acquisiton Channel Donations Avg Days Tab - Access results [HERE](https://docs.google.com/spreadsheets/d/1GeEYAo0l2VeqsY5q61uCPChsCm2qV40xmnArLA2CDvI/edit?usp=sharing)

There is a direct correlation between donation amount number of days that elapses between first contact and first donation made. For example, the manual import segment in the acquisition channel column has the lowest average days until first donation and the largest total donation value as well as the second most total clicks in the last three months

### Conclusion

I’d suggest that the communications team crafts targeting messaging surrounding the following segments:

- PD Connection
- Acquisition Channel
- Contact Created Date 
- First Donation Date 
- Donation Amount
- Emails Clicked in the last three months

All of the variables listed above have direct correlation to donation volume, amount, and donor source of contact.


