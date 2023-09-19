---
content_type: page
description: ''
draft: false
learning_resource_types:
- Exams
ocw_type: CourseSection
parent_title: Exams
parent_type: CourseSection
parent_uid: 3cf8bdea-1d49-826e-cbfd-ddc07baf1318
title: Fall 2000 Final Exam
uid: 5f0a9fe8-5569-ee06-9be1-d79825b30d27
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---
## Rules

This is an MIT "ex-camera" exam, which means that you can take it where you want to and use any resources you like, except that you must do the exam by yourself. In order to do the exam, you will need access to a Web browser. We suggest that you find a place to take the exam where you have quiet, private use of a web browser, a text editor, and email.

When you are through with the exam, send your work by email (in plain text, please) to Instructor. At the beginning of the message, please include a statement affirming that you worked on the exam completely by yourself.

Even though we have given you six hours to work on this exam, you should be able to complete it very quickly if you've you've been coming to lecture and doing the readings. Note in particular that we've specified an expected length of answer for each question: don't feel that you have to write reams of stuff just because you have the time -- a clear, succinct answer is better than an extensive ramble.

## Resources

Here are some resources that will help you answer the questions below.

- [Philip and Alex's Guide to Web Publishing](http://philip.greenspun.com/panda/)
- [SQL for Web Nerds](http://philip.greenspun.com/sql/)
- "[Introduction to AOLserver](http://philip.greenspun.com/wtr/)", parts 1 and 2 (see Server Tools section)

## Question 1: Normalizing a Data Model

Here is a data model for storing a collaboratively collected set of MP3s (digitized music clips). These users are presumed to be registered in a single ArsDigita Community System. List two reasons why this table is not even in First Normal Form. Then rewrite this data model to get it into Third Normal Form. Hint: your normalized data model will contain more than one SQL table.

```plaintext
create table mp3s (
 mp3_id   integer primary key,
 the_music  blob,
 submitting_user_id references users,
 playing_minutes  number,
 musicians  varchar(4000),
 composers  varchar(4000),
 date_recorded  date,
 – user IDs separated by spaces
 – (these are users who clicked “add to my personal list”
 personal_list_ids clob,
 – user IDs separated by spaces
 – (these are all the users who’ve played a song
 downloading_user_ids clob,
 – ratings, separated by spaces
 – each rating is a user ID,integer (between 1 and 10)
 ratings   clob,
 original_lyrics  clob,
 – what language are the lyrics in (use codes)
 original_lyric_lang char(2),
 – let users upload translations into other languages
 lyric_translation1 clob,
 lyric_translation1_lang clob,
 lyric_translation2 clob,
 lyric_translation2_lang clob,
 lyric_translation3 clob,
 lyric_translation3_lang clob,
 – if this came from a Compact Disc, store disc info
 – is this disc on file at cddb.com?  If so, record key.
 cddb_key  varchar(200),
 barcode_on_disc  varchar(100),
 record_label  varchar(200),
 catalog_number  varchar(200),
 total_playing_minutes number

);
```

Expected length of answer: 1 page.

## Question 2: Usability

You're sick of MIT. You're sick of the sub-freezing temperatures (and it isn't even officially winter yet!). You're sick of being so thin and want to look more like your instructors. Where can you find warm temperatures and the unlimited meals that you'll need to put some flesh on your bones? Clearly a Caribbean cruise is in order.

Here are your constraints:

- you've got to spend Christmas with your parents or they'll refuse to pay tuition for next term; i.e., you can't leave until December 26
- you've got to be back at MIT on January 8 for the start of IAP; more than 200 people have signed up for the "Joy of PL/SQL" seminar that you're teaching
- being a student in the coolest course at MIT has been great for your love life; you've got a new romantic partner and you'll need two spots on the cruise

Start by visiting the [Carnival](http://www.carnival.com) site and spend no more than 3 minutes attempting to answer the question "Do they have two free spots on any cruise departing on or after December 26 and returning before January 8?"

### Question 2a

Suggest five things that the designers of this site could have done differently to improve your experience. (Expected length of answer: 1-3 sentences per suggestion.)

Now we'll add a twist: you're gay. You don't want to end up on a Patrick Buchanan-themed cruise.

Spend another three minutes surfing around the [Carnival](http://www.carnival.com) site to get a feel for life on board one of the ships (maybe start by picking the ship Paradise). See if you can find any information about whether Carnival offers special gay-themed cruises or gathering places for gays.

### Question 2b

What could Carnival have done to give you a better answer to your question? A better feel for life on board their ships? Could collaborative authorship play a role? Sketch how this could be done. (Expected length of answer: 1 or 2 paragraphs.)

Question 2c

Critique the user interface of gay.com's Q&A forum software. If you wish, you may do this in the form of a pluses and minuses comparison to the standard ACS forum software (see the [photo.net forums](http://photo.net/community/) for a richly populated archive). Expected length of answer: 2 paragraphs.

Visit the PlanetOut site (CEO is MIT Course 2 graduate Megan Smith) and search for "Carnival cruise". Look at enough results to figure out whether or not Carnival is gay-friendly. Google for the search string "Carnival cruise gay". Again, look at enough results to figure out whether or not Carnival is gay-friendly.

### Question 2d

Compare the user interfaces of the preceding search engines for usability. (Expected length of answer: 2 paragraphs.)

## Question 3: Oracle

### Question 3a: Fun with Oracle

You've been handed a running Web service that relies on an Oracle database. The original developers weren't strong on documentation and they've not left you any .sql files with CREATE TABLE statements. Using only SQL queries (i.e., not any SQL\*Plus commands such as "DESCRIBE"), how can you find out (1) what tables have been defined, (2) what columns are in those tables, (3) which of those columns are nullable (i.e., can contain the value "NULL"), and (4) which of those table columns are constrained. Your SQL queries need not be exactly right but you should have found the tables and columns that contain the information you need.

Expected length of answer: four queries, each no more than a line or two.

### Question 3b: More Fun with Oracle

You've written an application program that:

1. queries a row of information from Oracle (using "SELECT \* FROM FOOBAR")
2. performs some computation on the resulting data
3. based on the computation, updates the information that was queried in Step 1 (using UPDATE)

Explain the concurrency problem that arises when two copies of this program are running simultaneously (expected length of answer: 1 paragraph). Give two modifications to the program that would eliminate the concurrency problem and explain the advantages of each approach. Your modifications should include explicit SQL statements. (Expected length of answer: 1 paragraph).

## Parting Words

> Percentage of MBA students who say they would hire a competitor's employee to steal trade secrets: 73%
> 
> Percentage of convicted criminals who say they would do this: 60%
> 
> \-- _Wall Street Journal_, May 3rd 1999

It takes more than engineering to make a successful Web service. Pick your partners carefully.

At the beginning of the message, please include a statement affirming that you worked on the exam completely by yourself.

## Who Wrote This and When

This exam was written by Hal Abelson and Philip Greenspun in December 2000. It is copyright 2000 by them but may be reused provided credit is given to the original authors.