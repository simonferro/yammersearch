# Yammersearch

SQL Project about Yammer Search

Project information: https://mode.com/sql-tutorial/understanding-search-functionality/

Yammer is a fictional company, whose product team is considering working on the search functionality for their next sprint. They are interested in knowing whether they should tackle search or not and, if so, what can be improved.

There are two tables relevant for the analysis, which can be seen on the link above. The table "users" includes information about the user account, while the table "events" includes raw event data from events such as search, but also from events like login and signup.

# *Objective* 

Understand whether the product team should be working in the search functionality and, if so, recommending in which way the should be improving the functionality.

# Data

The data can be found in the following link

 https://mode.com/sql-tutorial/understanding-search-functionality/
 
 
 # Solution 
 
 As mentioned above, the objective of this project is to support the product team in defining whether the search functionality is an important functionality to work on and, if so, where the focus for improvement should be in the team.
 
 To begin with, we can grasp an understanding of the data that both tables have, such that we can understand the important variables that we can segment and analyze by. The first table (users) contains the following information: user_id, created_at, state, activated_at, company_id, language. Of these, user_id will be a field just used to join so we can analyze any possible metrics by the creation date of the user, whether the user´s activation state is active or pending and at which time this happened, their company and their language.
 
 On the other hand, for the events table we have the fields user_id, occurred_at, event_type, event_name, location and device. Again, the field user_id will be used just for joining to another table, however we can analyze data by the time the event occured, the type of event and its name, and the location and device of the event.
 
 First off, we can start getting a grasp of the importance of search as an event by analyzing the percentage of all events that are related to search. This can be done with the following query:
 
 SELECT
  event_name,
  COUNT(occurred_at) AS num_occurences
FROM
  tutorial.yammer_events
GROUP BY
  event_name
ORDER BY 
  num_occurences DESC
LIMIT
  100
 

 As we can see, search events are not very prevalent compared to other events like view_inbox and send_message. Indeed, the total amount of search events is around 40000, while the total amount of signups is around 3600. We can actually expand on this in order to get a better grasp of the number of search events per user through the following query. In this query, we are also comparing the number of events to like_message, which is the second most popular event after home_page:
 
 WITH events_per_user AS (
SELECT 
  e.user_id,
  CASE WHEN 
    event_name LIKE '%search%' THEN 'search_related_event'
  ELSE 
    event_name
  END AS event_name, 
  count(*) as count_event
FROM 
  tutorial.yammer_events AS e
LEFT JOIN
  tutorial.yammer_users AS u ON u.user_id=e.user_id
WHERE 
  event_name LIKE '%search%' OR event_name = 'like_message'
  GROUP BY event_name,e.user_id
  )
  SELECT
    event_name,
    percentile_cont (0.2) WITHIN GROUP
		(ORDER BY COUNT_EVENT ASC) as percentile_20,
		percentile_cont (0.4) WITHIN GROUP
		(ORDER BY COUNT_EVENT ASC) as percentile_40,
  percentile_cont (0.5) WITHIN GROUP
		(ORDER BY COUNT_EVENT ASC) as percentile_50,
		percentile_cont (0.6) WITHIN GROUP
		(ORDER BY COUNT_EVENT ASC) as percentile_60,
		percentile_cont (0.8) WITHIN GROUP
		(ORDER BY COUNT_EVENT ASC) as percentile_80
	FROM events_per_user
	GROUP BY event_name
 
 We see that the number of search events is significantly lower than for the like_message event , and that the distribution is right tailed in the sense that the percentile 40 of users only executes 1 search event. The median of search events is 2 events per user, which is not very much.
 
 We can now take a look at the distribution of the search events themselves in order to understand which search events are most common. Ideally what we would see is that all search events are search autocomplete, meaning that the search engine found what the user was looking for before they typed. We can use the following query:
 
 SELECT
  event_name,
  100*COUNT(occurred_at)/SUM(COUNT(occurred_at)) OVER() AS perc_occurences
FROM
  tutorial.yammer_events
WHERE
  event_name LIKE '%search_%'
GROUP BY 
  event_name
ORDER BY
  perc_occurences DESC
LIMIT
  100
  
  Not only do we see that this assumption of all searches being autocomplete is incorrect, but we also see that the search click result is not ordered, meaning that users click more on the result 2 than on the result 1 when they run a search, which means that the way the results are ordered in the  search engine could be improved
  
  We can now look at how many search runs actually get clicked. ( sum of search click results over search run)
 
 



