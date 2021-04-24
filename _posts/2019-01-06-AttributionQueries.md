---
title: "Attribution Queries [SQL]"
date: 2019-01-06
tags: [Data Science]
header:
    image: "/images/projects.jpg"
excerpt: "Using touch attribution, map CoolTShirts’ customer journey - from the initial visit to purchase - and use that information to optimize their marketing campaigns."
mathjax: "true"
---

## Capstone Work from Codecademy
<img src="{{ site.url }}{{ site.baseurl }}/images/sqlcert.JPG" alt="sql certificate">

### Goal
Using touch attribution, map CoolTShirts’ customer journey - from the initial visit to purchase - and use that information to optimize their marketing campaigns.


**How many campaigns and sources does CoolTShirts use? Which source is used for each campaign?**

Use three queries:

one for the number of distinct campaigns,<br>
one for the number of distinct sources,<br>
one to find how they are related.


    SELECT COUNT(DISTINCT utm_source), COUNT(DISTINCT utm_campaign)
    FROM page_visits;

    SELECT DISTINCT utm_campaign, utm_source
    FROM page_visits;


**What pages are on the CoolTShirts website?**

Find the distinct values of the page_name column.


    SELECT DISTINCT page_name
    FROM page_visits;


**How many first touches is each campaign responsible for?**


    WITH first_touch AS (
        SELECT user_id,
            MIN(timestamp) as first_touch_at
     FROM page_visits
     GROUP BY user_id),

    ft_attr AS (SELECT ft.user_id,
     ft.first_touch_at,
      pv.utm_source,
         pv.utm_campaign
    FROM first_touch ft
    JOIN page_visits pv
        ON ft.user_id = pv.user_id
        AND ft.first_touch_at = pv.timestamp)
   
    SELECT ft_attr.utm_source,
	    		ft_attr.utm_campaign,
        COUNT(*)
    FROM ft_attr
    GROUP BY 1, 2
    ORDER BY 3 DESC;


**How many last touches is each campaign responsible for?**

Starting with the last-touch query from the lesson, group by campaign and count the number of last touches for each.


    WITH last_touch AS (
        SELECT user_id,
            MAX(timestamp) as last_touch_at
        FROM page_visits
        GROUP BY user_id),
    lt_attr AS (SELECT lt.user_id,
        lt.last_touch_at,
        pv.utm_source,
            pv.utm_campaign
    FROM last_touch lt
    JOIN page_visits pv
        ON lt.user_id = pv.user_id
        AND lt.last_touch_at = pv.timestamp)
    
    SELECT lt_attr.utm_source,
           lt_attr.utm_campaign,
        COUNT(*)
    FROM lt_attr
    GROUP BY 1, 2
    ORDER BY 3 DESC;


**How many visitors make a purchase?**

Count the distinct users who visited the page named 4 - purchase.


    SELECT page_name, COUNT(*)
    FROM page_visits
    WHERE page_name = '4 - purchase';


**How many last touches on the purchase page is each campaign responsible for?**


    WITH last_touch AS (SELECT user_id, MAX(timestamp) AS last_touch_at
    FROM page_visits
    WHERE page_name ='4 - purchase'
    GROUP BY user_id),

    ltcl AS (SELECT lt.user_id, pv.utm_campaign, lt.last_touch_at
    FROM last_touch lt
    JOIN page_visits pv
    	ON lt.last_touch_at = pv.timestamp)
  
    SELECT ltcl.utm_campaign, COUNT(*)
    FROM ltcl
    GROUP BY ltcl.utm_campaign;

Additionally, this is the **result report** [link](https://github.com/limdata/sql-scratch-capstone-turn-in/blob/master/codecademy/capstone%20project.pdf).


