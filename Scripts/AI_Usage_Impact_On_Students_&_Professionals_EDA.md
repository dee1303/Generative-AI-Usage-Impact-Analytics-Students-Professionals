\#Question 1: Do students and professionals differs efficiency gains from AI?  
SELECT  
    User\_Type,  
    COUNT(User\_ID) AS Total\_Users,  
    ROUND(AVG(AI\_Usage\_Hours\_Per\_Day), 2) AS Avg\_Daily\_Usage\_Hours,  
    ROUND(AVG(Time\_Saved\_Hours\_Per\_Week), 2) AS Avg\_Weekly\_Hours\_Saved,  
    ROUND(AVG(Productivity\_Score), 2) AS Avg\_Productivity\_Score,  
     ROUND(  
        AVG(  
            CASE  
                WHEN AI\_Usage\_Hours\_Per\_Day \= 0 THEN NULL  
                ELSE Time\_Saved\_Hours\_Per\_Week / AI\_Usage\_Hours\_Per\_Day  
            END  
        ), 2  
    ) AS Time\_Saved\_Per\_Usage\_Hour  
FROM  
\`ai-impact-usage.ai\_project\_db.cleaned\_ai\_usage\`

GROUP BY User\_Type;

\#Question 2:  higher daily AI usage really associated with more weekly time saved?  
WITH Usage\_Tiers AS (

    SELECT

        User\_ID,User\_Type,    
        AI\_Usage\_Hours\_Per\_Day,  
        Time\_Saved\_Hours\_Per\_Week,  
    CASE

        WHEN AI\_Usage\_Hours\_Per\_Day \< 2 THEN '1\. Low Usage (\<2 hrs)'  
        WHEN AI\_Usage\_Hours\_Per\_Day BETWEEN 2 AND 5 THEN '2\. Medium Usage (2-5 hrs)'  
        ELSE '3\. High Usage (\>5 hrs)'

    END AS Usage\_Category

    FROM  \`ai-impact-usage.ai\_project\_db.cleaned\_ai\_usage\`  
)

SELECT

Usage\_Category,  
COUNT(User\_ID) AS User\_Count,User\_Type,  
ROUND(AVG(AI\_Usage\_Hours\_Per\_Day), 2) AS Avg\_AI\_Usage\_Hours,  
ROUND(AVG(Time\_Saved\_Hours\_Per\_Week), 2) AS Avg\_Time\_Saved

FROM

    Usage\_Tiers

GROUP BY

    Usage\_Category,User\_Type

ORDER BY

    Usage\_Category ASC;

\# Question 3: Which AI tools purpose specific use cases, and which deliver the highest accuracy?  
WITH Filtered\_Tools AS (

    SELECT

        User\_ID,  
        AI\_Tool,  
        AI\_Purpose,  
        Accuracy\_Rating,  
        Satisfaction\_Score  
    FROM  
        \`ai-impact-usage.ai\_project\_db.cleaned\_ai\_usage\`  
    WHERE  
    AI\_Tool \!= 'Unknown')

SELECT  
    AI\_Tool,  
    AI\_Purpose,  
    COUNT(User\_ID) AS Total\_Adoption\_Count,  
    ROUND(AVG(Accuracy\_Rating), 2) AS Avg\_Accuracy\_Rating,  
    ROUND(AVG(Satisfaction\_Score), 2) AS Avg\_Satisfaction\_Score

FROM Filtered\_Tools

GROUP BY AI\_Tool, AI\_Purpose  
ORDER BY Total\_Adoption\_Count DESC;

\#Question 4: Does paid investment in AI tools lead to higher productivity/statisfication and recommendation rates?

SELECT  
\--  DISTINCT AI\_Tool AS Recommended\_AI\_Tool,  
CASE  
    WHEN Monthly\_AI\_Cost \= 0 THEN '1\. Free (\$0)'  
    WHEN Monthly\_AI\_Cost BETWEEN 1 AND 25 THEN '2\. Pro ($1-$25\)'  
    ELSE '3\. Enterprise / Power (\>\$25)'  
END AS Paid\_Plan,  
COUNT(User\_ID) AS User\_Count,  
ROUND(AVG(Satisfaction\_Score), 2) AS Avg\_Satisfaction,  
ROUND(AVG(Productivity\_Score), 2) AS Avg\_Productivity,  
ROUND(SUM(CASE WHEN Would\_Recommend \= 'Yes' THEN 1 ELSE 0 END) \*100  / COUNT(User\_ID),1) AS Recommendation\_Rate\_Pct  
FROM  
 \`ai-impact-usage.ai\_project\_db.cleaned\_ai\_usage\`  
WHERE  
    AI\_Tool \!= 'Unknown'

GROUP BY  
    \-- Recommended\_AI\_Tool,  
    Paid\_Plan  
ORDER BY  
    Paid\_Plan ASC,  
    Avg\_Satisfaction DESC,  
    Avg\_Productivity DESC;

\# Question 5: How does AI adoption and daily dependency vary across age  and satisfaction?  
SELECT  
    CASE  
        WHEN Age \<= 21 THEN 'Gen Z / Teens (\<=21)'  
        WHEN Age BETWEEN 22 AND 30 THEN 'Young Adults (22-30)'  
        WHEN Age BETWEEN 31 AND 45 THEN 'Adults (31-45)'  
        ELSE 'Senior (46+)/ Established'  
    END AS Age\_Cohort,  
    COUNT(User\_ID) AS Demographic\_Count,  
    ROUND(AVG(AI\_Usage\_Hours\_Per\_Day), 2) AS Avg\_Daily\_Usage\_Hours,  
    ROUND(AVG(Satisfaction\_Score), 2) AS Avg\_Satisfaction,  
    ROUND(AVG(Work\_or\_Study\_Hours\_Per\_Day), 2) AS Avg\_Total\_Work\_Hours,  
    ROUND(AVG(      
        CASE      
            WHEN Work\_or\_Study\_Hours\_Per\_Day \= 0 THEN NULL    
            ELSE (AI\_Usage\_Hours\_Per\_Day / Work\_or\_Study\_Hours\_Per\_Day) \* 100  
        END

        ), 1

    ) AS AI\_Share\_Of\_Workday\_Pct  
FROM  
    \`ai-impact-usage.ai\_project\_db.cleaned\_ai\_usage\`  
GROUP BY  
    Age\_Cohort  
ORDER BY  
    Age\_Cohort ASC;

