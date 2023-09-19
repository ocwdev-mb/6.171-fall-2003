---
content_type: page
description: ''
learning_resource_types:
- Assignments
ocw_type: CourseSection
parent_title: Assignments
parent_type: CourseSection
parent_uid: 7c3bc40c-1950-1cf6-ba32-c5d48982344b
title: Problem Set 5 (Family Tree)
uid: 19578dc4-e855-76a6-7943-9056f9686b1d
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---

Reading for This Week
---------------------

*   Oracle Intermedia Text reference.
*   Oracle Tree Extensions sections in Oracle 8: The Complete Reference.
*   SQL for Web Nerds, [tree chapter](http://philip.greenspun.com/sql/trees.html?).

Objectives
----------

Teach students how to build a collaborative authoring environment for structured multi-media data. Introduce students to Oracle's full-text indexing system.

[The Huge Picture](#1)  
[The Big Picture](#2)  
[Privacy](#3)  
[Commentability](#4)  
[Assumptions](#5)  
[Assignment](#6)  
[Optional Assignment](#7)

{{< anchor "1" >}}{{< /anchor >}}The Huge Picture
-------------------------------------------------

Civilization and its discontents have fractured the extended family. You are building a Web service to bring everyone back together.

{{< anchor "2" >}}{{< /anchor >}}The Big Picture
------------------------------------------------

All of the members of your family should be able to come together to

*   Collaboratively maintain the family's geneaology
*   Associate photographs and documents with individuals or subtrees at different ages, e.g., "Joey Smith at Age 3" or "Bill and Margaret Smith and theirs kids in 1983" or "Alice Wong's English paper, Grade 10"
*   Plan reunions and smaller get-togethers

{{< anchor "3" >}}{{< /anchor >}}Privacy
----------------------------------------

Every item uploaded to the server should have three possible privacy settings:

*   Show to designated users (i.e., if it is something nasty about your Aunt Maude that your brother would appreciate, you want him to see it but not Aunt Maude)
*   Show to registered users (i.e., only people in the extended family)
*   Show to everyone

Why the last category? A family may wish to present a collaboratively developed face to the world.

{{< anchor "4" >}}{{< /anchor >}}Commentability
-----------------------------------------------

Every item on the server should serve as the focus for comments. If you use the ACS general comments facility, your server will automatically be able to accept photos and documents attached to comments.

Assumptions{{< anchor "5" >}}{{< /anchor >}}
--------------------------------------------

You can't assume that everyone in the family tree will be a user in the users table: some of them might be dead and/or not have email addresses.

You can assume that your server needs to serve only one family at a time. You don't have to persistently maintain a tree for more than one family.

For the purposes of the problem set, you can assume that your family is trustworthy. You can let any registered user edit anything.

{{< anchor "6" >}}{{< /anchor >}}Assignment
-------------------------------------------

### Exercise 1: Build the Data Model

Using the file naming and placement conventions, create a SQL data model file. Before you can do this, you have to come up with a name for your module. In order to make life easier for us in looking over your shoulder, please refrain from being creative and call your module "family".

Here are some guidelines for your data model:

*   create a table called "family\_relatives" to store the family members. Here is a skeleton:
    
    > create table family\_relatives (  
    > relative\_id integer primary key,  
    > -- optional pointer to users table  
    > user\_id  references users,  
    > spouse  references family\_relatives,  
    > mother  references family\_relatives,  
    > father  references family\_relatives,  
    > -- in case they don't know the exact birthdate  
    > birthyear integer,  
    > birthday date,  
    > -- sadly, not everyone is still with us  
    > deathyear integer,  
    > first\_names varchar(100) not null,  
    > last\_name varchar(100) not null,  
    > sex  char(1) check (sex in ('m','f')),  
    > -- note the use of multi-column check constraints  
    > check ( birthyear is not null or birthday is not null)  
    > );
    
*   create a table called "family\_photos" to store photographs and scanned documents, such as diplomas. We'll draw some inspiration from the /doc/sql/general-comments.sql file:
    
    > create table family\_photos (  
    > family\_photo\_id  integer primary key,  
    > photo   blob not null,  
    > -- file name including extension but not path  
    > client\_file\_name varchar(500),  
    > file\_type  varchar(100), -- this is a MIME type (e.g., image/jpeg)  
    > file\_extension  varchar(50),  -- e.g., "jpg"  
    > caption   varchar(4000),  
    > -- when was this photo taken  
    > item\_date  date,  
    > item\_year  integer,  
    > original\_width  integer,  
    > original\_height  integer,  
    > access\_control  varchar(20)  
    > check (access\_control in ('public', 'family', 'designated')),  
    > check (item\_date is not null or item\_year is not null)  
    > );
    

> > \-- a photo might contain more than one person so we need  
> > -- an extra table  
> > create table family\_photo\_relative\_map (  
> > relative\_id  references family\_relatives,  
> > family\_photo\_id  references family\_photos,  
> > primary key (relative\_id, family\_photo\_id)  
> > );
> 
> > create table family\_photo\_access\_control (  
> > family\_photo\_id  references family\_photos,  
> > user\_id   references users,  
> > -- note the order we spec the primary key  
> > -- (makes it fast to ask "who gets to see this photo")  
> > primary key (family\_photo\_id, user\_id)  
> > );

*   create a table called "family\_stories" to record stories about family members at different ages:
    
    > Date/time arithmetic
    > create table family\_stories (  
    > family\_story\_id  integer primary key,  
    > story   clob not null,  
    > item\_date  date,  
    > item\_year  integer,  
    > access\_control  varchar(20)  
    > check (access\_control in ('public', 'family', 'designated')),  
    > check (item\_date is not null or item\_year is not null)  
    > );
    

> > \-- a story might be about more than one person  
> > create table family\_story\_relative\_map (  
> > family\_story\_id  references family\_stories,  
> > relative\_id  references family\_relatives,  
> > primary key (relative\_id, family\_story\_id)  
> > );

### Exercise 2: The Family Tree Page

Build a set of Tcl scripts at /family that will allow users to

*   View the family tree
*   Add rows to the family\_relatives table
*   Rremove rows added by mistake

A good thing to remember is that the Macintosh user interface works well. Pick the object first and then the verb. So let the user add a parent, spouse, or child by first selecting an existing relative.

### Exercise 3: Uploading Photos

Extend the "/family/one-relative.tcl" page that you built in Exercise 2. so that users can attach photos to relatives. You'll need to use the ns\_ora blob\_dml\_file API call documented in http://arsdigita.com/free-tools/oracle-driver.html and also ns\_queryget from the AOLserver API.

You can find source code models for photo uploading in /comments/upload-attachment.tcl.

Once a photo is in the system and associated with one relative, provide an interface to add an association to one or more additional relatives.

Edit the family tree display page to show an extra little "p" when one or more photos including a relative are available. Once the user clicks on an individual, show the available photos of that person organized by the age the person would be in the photo (based on what your server knows about the date of the photo and the birth date of the relative).

For writing photos back to the Web, you'll need to return appropriate HTTP and MIME headers, then call ns\_ora write\_blob to pull the photo direct from Oracle and write it to the Web. Examples of ACS pages that do this include the following:

*   http://software.arsdigita.com/www/file-storage/download-file.tcl
*   http://software.arsdigita.com/www/shared/portrait-bits.tcl

### Exercise 4: Making Photos a Focus for Collaboration

Using the system described at http://photo.net/doc/general-comments.html, extend your photo display pages so that family members can comment on photos and even upload additional photos or documents as attachments.

### Exercise 5: Adding Stories

Repeat exercises 3 and 4 but for stories. You'll need to use the ns\_ora clob\_dml API call documented in [http://arsdigita.com/free-tools/oracle-driver.html](http://arsdigita.com/free-tools/oracle-driver.html)

### Exercise 6: Event Planning Data Model

Add an event planner, e.g., for reunions, to your system. You'll need to extend the data model to include at least two extra tables: family\_events (what is being planned) and family\_events\_registration (who is signed up). Each event should have a creator who will be the coordinator.

### Exercise 7: Event Planning Pages

Modify the /pvt/home.tcl page to show registered users upcoming events right at the top of the workspace. If they click on an event summary, take them to a page where they can sign up. Upon registering for the event, the creator of the event should be emailed.

Provide pages in a /family/events/ directory that will let people see who is signed up for a particular event, spam the attendees, etc. Link this in with general comments so that users can make public comments on an event.

### Exercise 8: Full-text Indexing

Once you've got a bunch of stories in the family\_stories, it will be nice to provide users with the ability to search through them.

Build a /family/search-stories.tcl page that lets users search through the CLOBs in family\_stories. Note that

1.  The SQL LIKE command doesn't work with CLOBs (thank you, Oracle).
2.  If you had many megabytes of text, it would be very slow to sequentially search all of it
3.  It would be nicer to users if querying for "dogs" matched documents containing "dog" (stemming)

All of these issues can be addressed to some extent by using the Oracle Intermedia text indexing system:

> create index family\_stories\_ctx on family\_stories (story) indextype is ctxsys.context;

For examples of how to query and present results, read http://photo.net/doc/site-wide-search.html and the Oracle Intermedia Text reference at http://philip.greenspun.com/sql/ref/intermediatext.

### Exercise 9: Access Control

Develop a test suite of URLs and users and make sure that the main /family area presents the right information to the public, to registered users (the family), and to designated users (ones on the access control list for particular items).

{{< anchor "7" >}}{{< /anchor >}}Optional Assignment
----------------------------------------------------

Read Sigmund Freud's Civilization and Its Discontents.

Who Wrote This and When
-----------------------

This problem set was written by Philip Greenspun in October 1999. It is copyright 1999 by him but may be reused provided credit is given to the original author.