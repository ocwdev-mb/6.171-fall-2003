---
content_type: page
description: ''
learning_resource_types:
- Exams
ocw_type: CourseSection
parent_title: Exams
parent_type: CourseSection
parent_uid: 3cf8bdea-1d49-826e-cbfd-ddc07baf1318
title: Spring 2000 Midterm Exam
uid: d78d4564-ed8e-a513-d6d2-c9e9a71ff5fa
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---

Rules
-----

This exam is open-book and take-home. Do not discuss your answers or the problems with other students, however. If you are relying on a section of a book or Web page, cite the source.

When you are through with the exam, send your work by email (in plain text, please) to Instructor.

You have one week to complete the exam.

Resources
---------

Here are some resources that will help you answer the questions below.

*   [Philip and Alex's Guide to Web Publishing](http://philip.greenspun.com/panda/)
*   [SQL for Web Nerds](http://philip.greenspun.com/sql/)
*   "[Introduction to AOLserver](http://philip.greenspun.com/wtr/)", parts 1 and 2 (see Server Tools section)    

Question 1: Thinking About the Overall Application
--------------------------------------------------

Suppose that you are part of committee designing a Web service for intramural sports at MIT. Visit the  [MIT IM sports](https://intramurals.mit.edu/) site to get an idea of what an attempt to build a site like this looks like but do not be constrained in your thinking by what you see there.

What are the user classes for an ideal site like this? What should each class of user be able to do? What are the core benefits of doing this as a Web application instead of adding a section to the Tech?

Question 2: Building the Application
------------------------------------

How would you apply the latest version of the ACS to the problem of constructing the ideal IM sports site?

How would you configure existing modules? Which ones would you have to extend via writing new code? What new modules would you have to write?

Question 3: Mollifying the Client
---------------------------------

Suppose that the publisher of the IM sports site hires a Chief Technology Officer. This will be a person who cannot program. In order to bolster his or her ego, you'll get questioned:

*   Why are you using Oracle? Wouldn't it be cheaper and faster to use MySQL?
*   Why are you using Oracle? Wouldn't it be cheaper to use Microsoft SQL Server?
*   Why are you using AOLserver? Wouldn't Apache be better?
*   Why are you using this ACS toolkit?
*   Why are you using Solaris instead of Linux?
*   Why do you want to host this at AboveNet instead of in your dorm room? Or underneath my desk in the Athetics Department?

Question 4: User Activity Analysis
----------------------------------

Once you've launched your new IM Sports site, the publisher will want to know how users are interacting with the service.

Discuss the relative merits of the following approaches to user activity analysis:

*   standard server log analysis
*   recording predefined important events, such as "user clicked through to a foreign site" into the RDBMS
*   a dimensional data warehouse of clickstream data

Question 5: Usability Analysis
------------------------------

Every successful Web project has at least one participant who is passionate about the end-user experience. It is important to become skilled at working through a usage scenario and noting specifically what should be changed on each page. We haven't built our ideal IM sports site so we'll shift gears to a real running system:  [http://photo.net/](http://photo.net/) (the image sharing service portion of the site was built as a 6.170 project in Spring 1999).

Work through the following scenarios:

*   Take the role of a novice photographer who has just taken a single picture a digital camera and wishes to use the system to share that photo with a relative. Upload one photo into the system.
*   Take the role of the relative and come to the site to find the picture.
*   Take the role of a serious photographer. Upload a number of photos, a few captions for photos that one does not have scanned, and create a presentation.

Write up a usability analysis stating, for each page, what you'd change to make the experience better. Note that your analysis may suggest changing the flow, adding new pages, eliminating pages, or adding configuration switches for users.

Question 6: Workflow
--------------------

A really effective use for a Web-based system is to support cooperative work, i.e., a **purposeful community**. When people seek an information system to support their work together, it is usually because the workflow is at least a little bit complex, e.g., someone in the support group must write an answer to a customer inquiry, Amanda or Barney must approve it but if they disapprove the answer, the inquiry gets kicked back into the poor of questions to be answered.

A good way to support workflow is with a finite-state machine modelled in the database. See the Oracle Workflow product description in the Oracle documentation for some ideas about how engineers have approached this problem.

Build the SQL data model for a finite-state machine-based workflow system. It should be powerful enough to handle the approval process for purchasing in a company, with states such as "employee\_wants\_thing", "supervisor\_has\_approved\_thing", "thing\_is\_on\_order", "thing\_was\_received", "thing\_was\_paid\_for". Have a journal where data accumulated during state transitions, e.g., how much was paid for the thing, may be logged.

Who Wrote This and When
-----------------------

This problem set was written by Philip Greenspun in April 2000. It is copyright 2000 by him but may be reused provided credit is given to the original author.