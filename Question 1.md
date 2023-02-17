# Question 1

The Marketing team at MJFF wants to evaluate if their email audience is more likely to respond to CTAs
that use “emotional” language over those that use “impact-oriented” language (the default that has
always been sent) in their emails. The marketing and communications departments crafted two versions
of their Monthly Appeal series of emails – slated to be sent out the next month. The recipient list was
split equally and at random, and each list received one of the email versions.
Based on the email engagement results, would you recommend that the marketing switch to using more
emotional language in their emails, or should the team continue using impact-oriented language? What
additional information/experiment design changes would you require to make a clear recommendation?

**Use tabs titled – Email V1 Impact Language & Email V2 Emotional Language**

Access datasets [HERE](https://docs.google.com/spreadsheets/d/1dGFzsvhVzTSr971M5tdpPUhRXZGFhPOLe59FRomsr3w/edit?usp=sharing)

### Analysis


![Analytics Assessment](https://user-images.githubusercontent.com/112409778/219525577-e18b4653-5add-47c6-94d4-d8013d31d5e6.png)

**Impact Language Analysis** 

~~~ SQL
/*Impact Language Metrics*/
SELECT 
  CONCAT(ROUND(AVG(CASE 
  WHEN Opened IS false THEN 0
  ELSE 1 END) * 100.00),'%')  AS Opened_Percentage_Impact, -- Percentage of emails opened impact language
  
  CONCAT(ROUND(AVG(CASE 
  WHEN Clicked IS false THEN 0
  ELSE 1 END) * 100.00),'%')  AS Clicked_Percentage_Impact--Percentage of email clicks impact language
 FROM `single-being-353600.Analytics_Assessment_Data.Email_Impact_Langauge` ;
~~~
**Result:**

|Opened_Percentage_Impact|Clicked_Percentage_Impact|
|---|---|
|18%|1%|

**Emotional Language Analysis**

~~~ SQL
/*Emotional Language Metrics*/
 SELECT 
  
  CONCAT(ROUND(AVG(CASE --Using CASE statement to calculate average 
  WHEN Opened IS false THEN 0
  ELSE 1 END) * 100.00),'%')  AS Opened_Percentage_Emotional, -- Percentage of emails opened emotional language
  
  CONCAT(ROUND(AVG(CASE --Using CASE statement to calculate average 
  WHEN Clicked IS false THEN 0
  ELSE 1 END) * 100.00),'%')  AS Clicked_Percentage_Emotional--Percentage of email clicks emotional language
FROM `single-being-353600.Analytics_Assessment_Data.Email_Emotional_Language` ;
~~~
**Result:**
|Opened_Percentage_Emotional|Clicked_Percentage_Emotional|
|---|---|
|23%|4%|

### Conclusion
Based on engagement results I’d recommend that marketing switch to using more emotional language in their emails. Monthly appeal emails that featured emotional language had higher engagements than monthly appeal emails that featured impact language. Monthly appeals emails that featured emotional language resulted in a 127% increase in opened emails and a 400% increase in clicks.

### Recommendation
To make the outcome more conclusive, both versions of the monthly appeal emails should be emailed to the same recipients albeit in different months, one month use emotional language and another using impact language, collecting data, and analyzing engagement data metrics.
