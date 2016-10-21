---
layout: post
title: "System Design & News Feed System"
description: "notes for System Design"
date: 2016-10-21
tags: [systemDesign]
comments: true
share: false
---

#### Scenario
What are the key functions:  

1. User Engagement Numbers:   
      Daily Active Users (DAU)/Monthly Active Users (MAU)   
      eg. Twitter: MAU 320M, DAU ~150M+ 
2. Design Process  
   1. Enumerate   
   For a social network app, all possible functions needed are:    
      * Register / Login
      * User Profile Display / Edit 
      * Upload Image / Video
      * Search
      * Post / Share posts
      * Timeline / Newsfeed
      * Follow / Unfo a user
    2. Sort  
    Make a priority queue: 
       * Tweet a post (most important)
       * Timeline
       * Newsfeed
       * Fo/Unfo auser
       * Register/ Login
    3. Analysis & Predict   
       * Concurrent User    
            ```
            Concurrent User = DAU * Average Request Per User Per day / 86400 (seconds in a day) 
            ```
        * QPS(Queries Per Second)   
        Read QPS vs Write QPS   
        QPS vs Web Server/ Database:   
        
            * 1k QPS : one Web Server/ one SQL Database
            * 10k QPS : one NoSQL Database(Cassandra)
            * 1M QPS : one NoSQL Database(Memcached)
              
#### Service 
Break it into sub modules: Reply + Merge   

1. From a receptionist's view:     
      * User Service (Reiger/Login)
      * Tweet Service (Post + Newfeed + Timeline)
      * Media Service (Upload Img/Vid)
      * Friendship Service (Fo/Unfo)

#### Storage   
data IO 

1. Database types:         
      * SQL Database (Relation based)    
           * Used to store User table
           * MySQL
      * NoSQL Database (Not relation based)
           * Used to store Tweets and Social Graphs (Followers) 
           * MongoDB
      * File System
           * Used to store Media Files
           * S3
           
2. Design Process      
      **Select** Database types for Application/Service, then **Schema** Data Structure  
      For example, in the social media app case, the desired Database for each services are: 
      * User Service (Reiger/Login)
        * Select: SQL Database, MySQL
        * Schema:     
| Header1 | Header2 | Header3 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|----
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
                | User   Table     |            | 
                | ------------- |:-------------:| 
                | id      | integer | 
                | username     | varchar      | 
                | email | varchar      |   
                | password | varchar      | 
    * Tweet Service (Post + Newfeed + Timeline)
        * NoSQL Database, MongoDB 
        * Schema:     
                | Friendship Table     |            | 
                | ------------- |:-------------:| 
                | id      | integer | 
                | from_user_id     | Foreign Key      | 
                | to_user_id | Foreign Key     |   
     * Media Service (Upload Img/Vid)
       * File System, S3
       * Schema:     
                | Tweet Table     |            | 
                | ------------- |:-------------:|
                |  id      | integer | 
                | user_id     | Foreign Key      | 
                | content | text      |   
                | created_at | timestamp      | 
     * Friendship Service (Fo/Unfo)
        * SQL/NoSQL Database     

        In summary, a program can be viewed as algorithm + data structure, while a system can be viewed as service + data storage
3. Pull Model:
    * Algorithm   
        * Merge K Sorted Arrays  
    * Complexity  
        * News Feed => O(n)  **<- slow**
        * Post a tweet => 1 DB Write  
    * getNewsFeed(request)  
     
        ```python
         getNewsFeed(request)
         followings = DB.getFollowings(user=request.user)
         news_feed = empty
         for follow in followings:
            tweets = DB.getTweets(follow.to_user, 100)
            news_feed.merge(tweets)
         sort(news_feed)
         return news_feed
        ``` 
    * postTweet(request, tweet) 
     
        ```python
         DB.insertTweet(request.user, tweet)
         return success
        ``` 
4. Push Model:
    * Algorithm   
        * List  
    * Complexity  
        * News Feed => 1 DB Write 
        * Post a tweet => O(n)  **<- slow**  
    * getNewsFeed(request)  
     
        ```python
         getNewsFeed(request)  
         return DB.getNewsFeed(request.user)
        ``` 
    * postTweet(request, tweet_info) 
     
        ```python
         postTweet(request, tweet_info) 
         tweet = DB.insertTweet(request.user, tweet_info)
         AsyncService.fanoutTweet(request.user, tweet)
         return success
        ``` 
    * AsyncService::fanoutTweet(user, tweet) 
     
        ```python
         AsyncService::fanoutTweet(user, tweet) 
         followers = DB.getFollowers(user)
         for follower in followers:
             DB.insertNewsFeed(tweet, follower)
        ``` 
4. News Feed Table:
    * Algorithm   
        * List  
    * Complexity  
        * News Feed => 1 DB Write 
        * Post a tweet => O(n)  **<- slow**  
#### Service 
Break it into sub modules: Reply + Merge
1. From a receptionist's view:   
    * User Service (Reiger/Login)
    * Tweet Service (Post + Newfeed + Timeline)
    * Media Service (Upload Img/Vid)
    * Friendship Service (Fo/Unfo)
