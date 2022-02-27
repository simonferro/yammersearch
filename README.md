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
 
 
 As we can see, search events are not very prevalent compared to other events like view_inbox and send_message. Indeed, the total amount of search events is around 40000, while the total amount of signups is around 3600. We can actually expand on this in order to get a better grasp of the number of search events per user through the following query. In this query, we are also comparing the number of events to like_message, which is the second most popular event after home_page



