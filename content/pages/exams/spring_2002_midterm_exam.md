---
content_type: page
description: ''
learning_resource_types:
- Exams
ocw_type: CourseSection
parent_title: Exams
parent_type: CourseSection
parent_uid: 3cf8bdea-1d49-826e-cbfd-ddc07baf1318
title: Spring 2002 Midterm Exam
uid: 43bdb31e-9f67-cbc9-7901-392e71126e86
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---

Rules
-----

This is a take-home exam. You can take it where you want to and use any resources you like, except that you must do the exam by yourself. If you are relying on a section of a book or Web page, cite the source.

When you are through with the exam, send your work by email (in plain text, please) to instructor. At the beginning of the message, please include a statement affirming that you worked on the exam completely by yourself.

You have one week to complete the exam.

Resources
---------

Here are some resources that will help you answer the questions below.

*   [The course textbook](http://philip.greenspun.com/seia/)
*   [Philip and Alex's Guide to Web Publishing](http://philip.greenspun.com/panda/?)
*   [SQL for Web Nerds](http://philip.greenspun.com/sql/)

Problems or questions? Email instructor.

Question 1: Developing a Data Model
-----------------------------------

Visit [netflix.com](http://www.netflix.com). Sign up for the free 10-day trial membership and take the following steps:

1.  Look at a page describing the movie "Strictly Ballroom". Look carefully at the information presented on this movie, including user reviews and the categorization of user reviews.
2.  Add a few movies to your rental queue.
3.  Reorder the queue so that the last movie you chose will be shipped next.
4.  Delete some movies from the queue.
5.  Look into the options for reporting a shipping problem but don't actually report a movie lost or delayed.
6.  Rate a few movies.
7.  Ask for recommended movies based on ratings that you previously entered.

Now develop a data model for the netflix.com site. Your data model should be complete enough to handle all of the preceding actions and operate the core netflix.com service. You may assume the existence of a users table, keyed with user\_id. Your data model should include appropriate integrity constraints and indices to facilitate the queries that you believe are necessary for the pages that you just experienced. Your data model should contain appropriate in-line comments.

Test your data model by feeding it to an RDBMS and making sure that the CREATE TABLE and index definition statements are accepted without errors. You will probably want to create a new database user so that your table definitions don't conflict with any tables that you might have defined for your team project.

Expected length of answer: if your data model contains more than 15 or 20 tables, you're probably handling too many special cases.

Question 2: Fancy SQL Queries
-----------------------------

Write a computer program that generates the page "here are the movies that we recommend based on your ratings of movies compared to other users' ratings of movies in the database" by querying the data model that you defined in Question 1. Ideally your program will be a single SQL query that returns rows in descending order of likeliness that the user will like a movie. It is acceptable to use procedural language crutches, e.g., PL/SQL functions in Oracle.

Test your query by feeding your data model from Question 1 into the RDBMS that you've been using this semester. Fill the tables with at least the following test data (you may add more if you like):

*   Schlomo Lifschitz loved "Terminator 2", liked "Terminator" and "The Replacement Killers", was neutral on "Crouching Tiger Hidden Dragon", and hated "Gone With the Wind."
*   Schlomo's wife Rachael hated T2, disliked Terminator, was neutral on "Crouching Tiger", and loved "GWTW."
*   Their divorce attorney Biff Hutchinson hated "GWTW" and liked "Gaslight."
*   The judge handling the case, Ronald Heidegger, loved "GWTW" and liked "Emma."

Print out a transcript of a database session in which you use your query to generate movie recommendations to Biff and Ronald. Note that your query should never recommend a movie that the person has already seen.

Question 3: Usability Analysis
------------------------------

Visit [photo.net](http://www.photo.net/)  with the goal of uploading three photos and sharing them with six friends (by emailing them a URL).

Take the following steps:

1.  Find the photo sharing service on photo.net.
2.  Upload your three photos (if you used my photos, note that they were taken with a Canon EOS-5 camera, 70-200/2.8L lens, and Fuji Astia slide film).
3.  Try to find a facility within the photo sharing service for preparing an exhibit in which those three photos can appear.
4.  Prepare an exhibit (photos plus some explanatory text).

Critique the user experience. Your critique should be clear concerning what is wrong with the current system. Your critique should be explicit about what to change, such that a junior programmer could implement your improvements without depending on his or her own taste and judgement.

Expected length of answer: 1-2 pages.

Question 4: Thinking About the Overall Application
--------------------------------------------------

Suppose that someone notices the inconsistencies and prevailing lameness of the average MIT campus organization's Web site (see [Activities and Clubs at MIT](http://www.mit.edu/activities/) for a list of offenders). Here we are at the school that invented the Web (with a bit of help from the Swiss physicists and NCSA) and this is the best we can do?

Imagine that you've been asked by the Associate Dean for Internet Self-Esteem to develop a central server that will provide outsourced IT to MIT student organizations. Each organization will get its own corner of this server rather than having to build a standalone Web site. What services should this central facility provide to organizations? To a club leader? To a club member? To a new student trying to figure out which clubs to join? To an MIT Dean trying to figure out which clubs to fund?

Expected length of answer: 2-3 pages.

Question 5: Justifying an Engineering Decision
----------------------------------------------

Pick some hardware and software tools to support the outsource-IT-for-MIT-clubs system that you designed for Question 4. Explain these decisions in terms of required user capacity, any special circumstances attributable to the service being run at MIT, and with an eye toward long-term maintainability. Try to anticipate and answer questions that someone coming fresh to the problem might have, such as the following:

*   Why are you using \[your chosen RDBMS\]? Wouldn't it be cheaper and faster to use MySQL?
*   Which Web server program is the best?
*   Would it be a lot easier and cheaper to do all of this with Microsoft Sharepoint or some other prepackaged software?
*   Does it make more sense to have a 2-tier system with SQL queries embedded in page scripts or something like Java® 2 Enterprise Edition where Enterprise Java® Beans are made persistent via machine-generated SQL transactions?
*   What training will be required before a programmer can safely take over maintenance on this system? How will we know if a new programmer is adequately prepared to start modifying the system?

Expected length of answer: 2 pages total.

Turning it in
-------------

Please email your answers to the questions to instructor.

At the beginning of the message, please include a statement affirming that you worked on the exam completely by yourself.