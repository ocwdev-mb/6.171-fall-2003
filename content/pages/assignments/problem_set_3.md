---
content_type: page
description: ''
learning_resource_types:
- Assignments
ocw_type: CourseSection
parent_title: Assignments
parent_type: CourseSection
parent_uid: 7c3bc40c-1950-1cf6-ba32-c5d48982344b
title: Content Management
uid: c08018d4-d6e8-73a9-2d1d-4b60adebf465
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---

Reading for This Week
---------------------

*   _[Philip and Alex's Guide to Web Publishing](http://philip.greenspun.com/panda/)_. Chapter 4.
*   AOLserver Tcl Developer's Guide. Chapter 2.

Objectives
----------

Teach people to understand the dimensions of the site design and content management problems. They first need to understand what challenges a site publisher faces then build software to support the publisher, programmers, and authors.

[The Huge Picture](#1)  
[The Big Picture](#2)  
[The Picture](#3)  
[Functional Spec](#4)  
[Assignment Details](#5)

{{< anchor "1" >}}{{< /anchor >}}The Huge Picture
-------------------------------------------------

Building a Web site is trivial. Maintaining a Web site is hard.

{{< anchor "2" >}}{{< /anchor >}}The Big Picture
------------------------------------------------

It is pretty easy to build and maintain a Web site if:

*   One person is publisher, author, and programmer
*   The site comprises only a few pages
*   Nobody cares whether these few pages are formatted consistently
*   Nobody cares about retrieving old versions or figuring out how a version got to be the way that it is

Sadly for you, the Web service developer, the preceding conditions seldom obtain. What is more typical are the following conditions:

*   Labor is divided among publishers, information designers, graphic designers, authors, and programmers
*   The site contains thousands of pages
*   Pages must be consistent within sections and sections must have a unifying theme
*   Version control is critical

In fact, the project that you'll be working on later this semester will probably require a content management system of some sort.

{{< anchor "3" >}}{{< /anchor >}}The Picture
--------------------------------------------

We need a system that:

*   Records who is contributing to a site and in what role
*   Records publishing and design decisions
*   Collects programs and content

{{< anchor "4" >}}{{< /anchor >}}Functional Spec
------------------------------------------------

The publisher decides (1) what major content sections are available, (2) when a content section goes live, (3) relative prominence to be assigned the content sections.

The information designer decides (1) what navigational links are available from every document on the page, (2) how to present the available content sections, (3) what graphic design elements are required.

The graphic designer contributes drawings, logos, and other artwork in service of the information designer's objectives. The graphic designer also produces mock-up templates (static HTML files) in which these artwork elements are used. You won't have to demonstrate this role in your prototype, but make sure that your data model supports this.

The programmer builds production templates (HTML with embedded Tcl and SQL) that reflect the instructions of publisher, information designer, and graphic designer.

In keeping with their relative financial compensation, we consider the needs and contributions of authors second to last. Authors stuff fragments of HTML, plain text, photographs, music, sound, into the database. These authored entities will be viewed by users only through the templates developed by the programmers.

In keeping with their relative popularity among authors and publishers, we place the perennially kicked-around editor last. Editors approve content and decide when specific pages go live. Editors assign relative prominence among pages within sections.

{{< anchor "5" >}}{{< /anchor >}}Assignment Details
---------------------------------------------------

Your "practice project" will be a content management system to support a guide to Boston, along the lines of Boston. You will need to produce a design document and a prototype implementation. The prototype implementation should be able to support the following scenario:

1.  Log in as publisher and visit /admin/content-sections/
2.  Build a section called "movies" at /movies
3.  Build a section called "dining" at /dining
4.  Build a section called "news" at /news that simply uses the existing ACS news system
5.  Log out
6.  Log in as information designer and visit /cm and specify navigation. From anywhere in dining, readers should be able to get to movies. From movies, readers should be able to dining or news.
7.  Log out
8.  Log in as programmer and visit /cm
9.  Make two templates for the movie section, one called movie\_review and one called actor\_profile; make one template for the dining section called restaurant\_review
10.  Log out
11.  Log in as author and visit /cm
12.  Add two movie reviews and two actor profiles to the movies section and a review of your favorite restaurant to the dining section
13.  Log out
14.  Log in as editor and visit /cm
15.  Approve two of the movie reviews, one of the actor profiles, and the restaurant review
16.  Log out
17.  Without logging in (i.e., you're just a regular public Web surfer now), visit the /movies section and, ideally, you should see that the approved content has gone live
18.  Follow a hyperlink from a movie review to the dining section and note that you can find your restaurant review
19.  Log in as author and visit /cm
20.  Eedit the restaurant review to reflect a new and exciting dessert
21.  Log out
22.  Visit the /dining section and note that the old (approved) version of the restaurant review is still live
23.  Log in as editor and visit /cm and approve the edited restaurant review
24.  Log out
25.  Visit the /dining section and check that the new (with dessert) version of the restaurant review is being served

### Exercise 1: Build the Data Model

Using the file naming and placement conventions set forth, create a SQL data model file. Before you can do this, you have to come up with a name for your module. In order to make life easier for us in looking over your shoulder, please refrain from being creative and call your module "cm".

Here are some guidelines for your data model:

*   Any time you are representing a person, you should do it by referencing the users table.
*   Any time you have to lump users together because of a special property (e.g., "authority to make publishing decisions"), make sure that you do it with the user-groups facility of the ArsDigita Community System (goes by the short name of "ug").
*   For the actual content of the site (the authored articles), make sure that you have an audit trail. There are two classical ways to do this. The first is to set up separate audit tables, one for each production table. Every time an update is made to a production table, the old row is written out to an audit table, with a time stamp. This can be accomplished transparently via RDBMS triggers (see [Triggers](http://philip.greenspun.com/sql/triggers)). We aren't going to do things this way! Instead, you should adopt the second classical approach: keep current and archived information in the same table. This is more expensive in terms of computing resources required because the information that you want for the live site is interspersed with seldom-retrieved archived information. But it is easier if you want to program in the capability to show the site as it was on a particular day. Your templates won't have to query a different table, they will merely need a different WHERE clause.  
      
    Note: this is the kind of design trade-off that you have to make every day as a Web service developer. In general, if something greatly simplifies programming at the cost of additional computing resources, you should adopt the simper approach. Computers are cheap and getting cheaper. Programming is hard and expensive. This isn't an argument for profligacy. In this case, we're assured by the fact that the tables we're archiving contain information typed in by users and that versioning only happens when users take time-consuming actions such as filling out forms and hitting "Submit". It is very difficult to fill up a modern disk drive with information typed by even a large collection of human beings.
*   For modeling the major areas of the Web service, use the  content\_sections table in the community-core.sql file. You'll have to augment it at the very least with a templated\_p column to indicate that this content section is generated by the cm system.
*   For capturing the navigation decisions of the information designer, call your table cm\_navigation (one row for each link from one section to another)
*   For templates, rely on AOLserver's ADP facility (see Example 6 in Chapter 10 of [Philip and Alex's Guide to Web Publishing](http://philip.greenspun.com/panda/?) and AOLserver Tcl Developer's Guide, Chapter 2)
*   Keep everything in the relational database, including ADP scripts; synchronizing data in a file system with tables in a relational database adds a tremendous amount of complexity
*   Generally you'll want to associate one individual with a content element as the owner (from the users table) and then allow modifications by a group of users (from the user\_groups table).

### Deliverable 1: The Design Document

When doing real Web projects, you have to coordinate multiple contributors. The best way to do this is with a design document. The document should include:

1.  A data model (/doc/sql/cm.sql)
2.  What sections of the ACS will need to be modified and how
3.  What new directories you intend to create
4.  Functional specs for the Tcl scripts you expect to create
5.  Work plan: who is going to do what and in what order

This problem set itself specifies a work plan to some extent and you should read the entire problem set carefully, but you shouldn't take our plan as gospel.

Before working further on this problem set, discuss and refine your design document with a TA.

Now that you've gotten agreement that the design document is reasonable, it is time to build the prototype. We suggest a work plan below, but if you've gotten approval to proceed in some other order, that's fine.

### Exercise 2: Create User Groups

Using the Web forms in /admin/ug/ create a new user group type: "cm". Create six groups of this type: publisher, information designer, graphic designer, programmer, author and editor. Put a couple of users in each group.

Make sure that you're using appropriate Tcl API calls to check user group membership before allowing users to take particular actions.

### Exercise 3: Extend /admin/content-sections/

Warm yourself up by visiting the /admin/content-sections/ directory and augmenting the admin pages for the content\_sections table, to which your data model has presumably added at least one column.

Define brand-new movies and dining content sections; also define a non-templated news content section that points to the existing /news facility.

### Exercise 4: Navigation

Create the /cm directory and a subdirectory for information designers (/cm/id/). The index.tcl page in the /cm directory should check a user's role assignments (memberships in the content management user groups). If a user is in the information designer group, a link to the /cm/id/ subdirectory should be presented. The scripts in the /cm/id/ directory should let the information designer specify which content sections are to have links to other content sections.

Define a procedure called cm\_navbar that a programmer can insert into a template. This procedure takes no arguments and returns the HTML fragment for a section-appropriate navigation bar. The procedure should:

*   Use ns\_conn url to figure out from where it is being called
*   Grab a database handle from the subquery pool (ns\_db gethandle subquery)
*   Look at the content\_sections table to see which content section the current URL is part of
*   Look at the cm\_navigation table to see what links should be offered from the current content section
*   Release the database handle (you must do this before the return statement)
*   Return the HTML string

Define a link from dining to movies. Define links from movies to dining and news.

### Exercise 5: Templates

You have to define forms to let programmers create templates. Each template is an ADP script that pulls information for a page from a single database table.

Create a table to hold all the templates (cm\_templates is a reasonable name). Make sure that this table can hold at least the following information, which will be supplied by the programmers when they define new templates:

*   A human-readable but Oracle-friendly key. For example, a template to display movie reviews would be known as a "movie\_review" rather than "3". (Your program should trap the error where a user tries to create a new template with the same key as an old one and return a page explaining the problem with a link to \\"edit old template\\")
*   The ADP template to execute (in a CLOB column)
*   The name of the table that will hold information for individual pages using this template; by convention this should probably be ${key}\_info, e.g., "move\_review\_info". Each row of this table will hold enough information to fill the template (i.e., each row of this table will correspond to a distinct viewable page on the Web).
*   The Oracle CREATE TABLE statement for the template's \_info table

Programmers who write templates and create these \_info table definitions will be required to remember that each \_info table must contain a fixed set of cm system columns for page\_key, version, modification\_date, approved\_p, approved\_by, and author\_id.

Note that it would probably be better to have a separate table with one row for each column of a template and then generate the CREATE TABLE for the \_info automatically, more or less like user\_group\_type\_fields. We aren't doing that in this problem set because we want you to be able to get through it quickly.

Put your scripts for adding and editing templates in a directory called /cm/templates/. Users in the programmer group who visit /cm/index.tcl should be offered the option of visiting /cm/templates/. If a user who isn't in the programmer group happens to visit /cm/templates/, they shouldn't be able to add or edit templates.

Define templates for movie reviews, actor profiles, and restaurant reviews.

At this point, you're probably screaming with rage at having to edit source code using Web forms. Unfortunately, right now this is pretty much the only way that you have to get information into and out of Oracle. Powerful tools like Emacs aren't set up to talk directly to relational databases, which is why people still use the Unix® file system despite its myriad shortcomings. Relief is in sight towards the middle of 2000 with a patch to the 8.1.6 release of Oracle. This version of the database can pretend to be a Windows®-protocol file server. So you could run Emacs on a PC and have it write files to and from the "O: drive". You and Emacs would think that these were plain ordinary files, but the information would actually be stored in a CLOB column of a database table. Oracle calls this "iFS".

### Exercise 6: Template-Section Mapping

Return to the /admin/content-sections/ directory and add some Tcl scripts to allow the publisher to decide which templates are to be included in which content sections. Presumably you'll need a cm\_template\_section\_map table to reflect these choices.

Associate the movies section with movie\_review and actor\_profile templates. Associate the dining section with the restaurant\_review template.

### Exercise 7: Supporting Authors

When a user in the authors group requests the /cm/ page, he or she should be invited to contribute an article for one of the templates. The templates should be organized by content section for presentation.

Build forms that let an author add or edit an article. Your Tcl scripts will have to automatically generate forms by

1.  Looking in the cm\_templates table to find the \_info table name associated with a template
2.  Using the ns\_column API call to find out what fields are required for a particular template (ignoring the key, version, modification\_date, approved\_p, and author\_id system columns)

After an author adds or edits a rows in an \_info table, your scripts must generate keys and set system columns appropriately.

Add two movie reviews, two actor profiles, and one restaurant review.

### Exercise 8: Supporting Editors

When a user in the editors group requests the /cm/ page, he or she should see a list of all the currently contributed-but-unapproved content. For your prototype, you don't have to worry about letting the editors actually edit; it is enough that they can toggle the approved\_p column and that your software records the user\_id of the editor who approved the item.

Approve everything except one of the actor profiles.

### Exercise 9: Build the Public Pages

Now we have enough information in the database that we can consider serving pages to the public. We could "compile" a Web site from the information in the database. This would entail looping through the content\_sections and cm\_templates tables and building Unix file system directories filled with .adp files. The drawback to this approach is that there is a risk of the database and the file system becoming out of sync. Furthermore, we'd like ultimately to version and approve templates much as we version and approve content. That argues for a completely virtual approach to serving public pages. The fundamental AOLserver facility that supports this is ns\_register\_proc, which tells the server to run a Tcl script whenever it sees a matching URL pattern.

Put a file of Tcl scripts in a file called "cm.tcl" in your server's private Tcl directory (/web/yourservername/tcl/). These scripts will be evaluated by your AOLserver when it starts up and when you request "re-initialize private Tcl" from the /NS/Admin pages.

Your file must:

1.  Get a database connection
2.  Query the content\_sections table for those sections that require virtual directories (i.e., dining and movies but not news)
3.  Use ns\_register\_proc to tell AOLserver to send any request starting with "/dining" or "/movies" to a Tcl procedure (call it cm\_serve\_section)
4.  Release the database connection
5.  Define the cm\_serve\_section Tcl procedure to actually deliver a content section

Remember that the same cm\_serve\_section procedure is called for every content section. So it must use ns\_conn url to figure out which content section has been requested. Also, the same procedure is called for every subpage within a section. So cm\_serve\_section has to be smart enough to either display a list of articles (e.g., if the user requests /movies) or display one article by evaluating a template. For example, if the user requests /movies/movie\_review/39, cm\_serve\_section will have to look up page\_key 39 in movie\_review\_info, set local Tcl variables to the values from the database, pull the movie\_review ADP template from cm\_templates, then evaluate the ADP template in the context of these local variables with ns\_adp\_parse.

Remember that cm\_serve\_section should only offer approved articles, only offer one version of each article, and that the version displayed should be the most recent approved version. Implement Hint: you can query from a table, ordered by modification\_date desc, get the first row, then use ns\_db flush $db to throw away the rest of the query results.

Visit the /dining/ and /movies/ URLs on your site and verify that appropriate articles are being presented to the public. If it doesn't work perfectly the first time, edit the code in your cm.tcl file and restart your AOLserver.

### Exercise 10: What's for Dessert?

Now we just have make sure that all the steps in the "more concretely" section work properly. Demonstrate the final steps of editing and approving a change to the restaurant review.

Who Wrote This and When
-----------------------

This problem set was written by Philip Greenspun in February 1999. It is copyright 1999 by him but may be reused provided credit is given to the original author.