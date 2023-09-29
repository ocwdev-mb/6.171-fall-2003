---
content_type: page
description: Take home exam for fall 2003 midterm.
draft: false
learning_resource_types:
- Exams
ocw_type: CourseSection
parent_title: Exams
parent_type: CourseSection
parent_uid: 3cf8bdea-1d49-826e-cbfd-ddc07baf1318
title: Fall 2003 Midterm Exam
uid: 3a9cdb09-de71-3e54-75d4-8fbdc8b018b1
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---
## Rules

This is a take-home exam. You can take it where you want to and use any resources you like, except that you must do the exam by yourself (no collaboration, not even with your team members). If you are relying on a section of a book or Web page, cite the source. An 80/100 will be a great score on this exam so if there is one part that is taking forever and doesn't interest you, write up your thoughts and move on.

When you are through with the exam, send your work by email (in plain text, please) to Instructor. At the beginning of the message, please include a statement affirming that you worked on the exam completely by yourself.

You have one week to complete the exam.

## Resources

Here are some resources that will help you answer the questions below.

- [The course textbook](http://philip.greenspun.com/seia/?) 
- [Philip and Alex's Guide to Web Publishing](http://philip.greenspun.com/panda/)
- [SQL for Web Nerds](http://philip.greenspun.com/sql/)

Problems or questions? Email Instructor.

## Question 1: Developing a Data Model

Visit [friendster.com](http://www.friendster.com). If you're not already a member, sign up for an account.

1. Find Ben, using a "user search," and add him as a friend (Ben said that he went into Computer Science because he wanted to meet a lot of cool people). You should do this earlier rather than later so that you have time to complete the exercises.
2. Look at a page describing a user. Look carefully at the information presented on this profile, including friends' comments.
3. Add a few other friends (people in the class are a good idea).
4. Change your favorite movies or TV shows to try to find people who have the same tastes. Observe how the site presents this information.
5. Add some testimonials on your friends.
6. Search for people in your network who live in specific cities other than Boston.

Now develop a data model for the friendster.com site. Your data model should be complete enough to handle all of the preceding actions and operate the core friendster.com service. (Ignore the "Shopping" and "Events" sections). Assume the existence of a users table, keyed with user\_id. The data model should include appropriate integrity constraints and indices to facilitate the queries that you believe are necessary for the pages that you just experienced. The data model should contain appropriate in-line comments.

Test your data model by feeding it to an RDBMS and making sure that the CREATE TABLE and index definition statements are accepted without errors. You will probably want to create a new database user so that your table definitions don't conflict with any tables that you might have defined for your team project.

Expected length of answer: if your data model contains more than 15 or 20 tables, you're probably handling too many special cases. The important thing is to get the structure of the data model right and the relations. Don't spend a lot of time making sure that you have every possible column in every table. For example it would be okay to say "-- … address columns …" to imply that there would be a way to store a member's street address, city, state, etc.

## Question 2: Fancy SQL Queries

Using the data model you defined in Question 1 write two computer programs:

Part a: a computer program that generates the page "find people in my network of friends that live within 50 miles of me and who say that they are interested in meeting people for Dating someone of my sex". The program should list those people ordered by ascending geographically distance.

Part b: a computer program that generates the page "how am I related to this person?" and displays all the possible paths between two people.

Ideally each program will be a single SQL query. It is acceptable to use procedural language crutches, e.g., stored procedures. Hint: you can postulate the existence of a table or function that will give you the latitude/longitude of the centroid of a U.S. zip code.

Test your queries by feeding your data model from Question 1 into the RDBMS that you've been using this semester (though you can use a separate tablespace/database/namespace). Fill the tables with at least the following test data (you may add more if you like):

- George is 57, likes pretzels, Segway Human Transporters, Texas, and SUVs. George lives in the U.S. at zip code 20500.
- Dick is 60, likes bacon double cheese burgers, SUVs, and energy policy. Dick won't tell us where he lives.
- Ken is 55, likes energy policy, Texas, and body building. Ken lives at the United States District Court in Houston 77002.
- Arnold is 55, likes California, body building, and SUVs. Arnold is interested in Dating Women. (Your data model need not include "groping" as a possible object of meeting people.) Arnold lives in zip code 90210.
- Britney is 22, likesSemiconductor Physics and is interested in dating both men and women (as long as they are rich and famous and/or MIT Course 6 grads). Britney put down 90210 as her home zip code because she's spending a lot of time out there working on a movie.
- George is friends with Dick.
- Dick is friends with Ken.
- Ken is friends with Arnold.
- George is friends with Britney.

Print out a transcript of a database session in which you use your queries (for (b) you could query to find the friend path between George and Arnold).

Note that it is possible that you'll start doing these queries and discover that you want to make changes to your data model. That's a perfectly natural way to refine a system. See if a data model supports some queries then go back and modify it until it does.

Don't spend more than a few hours on this question. You can get partial credit if you show that you're thinking along the right lines, e.g., by coming up with some smaller queries that give you reports that could be useful in solving the larger problem.

## Question 3: Usability Analysis

Based on your experience in Question 1 identify three usability problems with Friendster and write instructions to their programmers on how to fix them. Extra credit: email these ideas to the Friendster folks and attach their response.

Expected length of answer: 1 page.

## Question 4: Extending the Application with Distributed Computing

Jose Frio Cuarenta, senior MIT administrator, has decided that friendship networks are the next hot thing and wants to bring the magic of Friendster to MIT by building *MITster*. Being the total [Organization Man](http://www.english.upenn.edu/~afilreis/50s/whyte-main.html), he conceives this as a standalone application in which every MIT-affiliated person registers and comes to visit the site. Philip '82 and Gerry '68 can discover that they both live in the same city, share some interests, and are connected through Hal 'XX \[we could tell you but we'd have to kill you\].

Jose brings you into the project to lay out the system requirements and your first question is Why can't they just use Friendster? Jose responds that he isn't satisfied with Friendster's community features. A group of friends should be able to start an ad-hoc group with a private discussion forum, collaborative workspace, event planner, etc.

You point out that standalone servers were great in the 1990s but this century is all about distributed computing. Maybe the goal should be to augment rather than supplant Friendster. An MIT affiliate could log into a single server and get all of the services of Friendster plus additional services for groups of MIT friends.

Clearly you're going to have to set up some sort of server to provide the additional community support features. What kind of SOAP interface would you want from Friendster? Write a 1-2-page English description of the interface that you want and one WSDL file for a portion of the interface.

Here again we're looking for a reasonable structure not a complete solution with every possible field defined. It is okay to have some parts elided and commented on.

## Question 5: Fun with Data Warehousing

Using the Levi Strauss data model defined in the "Real World Example" section of [Data Warehousing](http://philip.greenspun.com/sql/data-warehousing), write SQL queries that answer the following questions:

- For each region in the U.S., how many pairs of pants were sold to repeat versus first-time customers?
- Starting from the inception of this business, what are the aggregate sales for each month, ordered from least recent to most recent months?

Suppose now that you are asked to prepare a report of average waist size of a pair of shipped pants, broken down by region of the country. To which table would you add a waist\_size column and why?

## Turning it in

Please email your answers to the questions to Instructor.

At the beginning of the message, please include a statement affirming that you worked on the exam completely by yourself.