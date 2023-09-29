---
content_type: page
description: ''
learning_resource_types:
- Assignments
ocw_type: CourseSection
parent_title: Assignments
parent_type: CourseSection
parent_uid: 7c3bc40c-1950-1cf6-ba32-c5d48982344b
title: Problem Set 2 (Extending the ArsDigita Community System, Web Sites to Support
  Collaboration)
uid: b52998d3-9b4c-627b-3277-2f73914ccc1d
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---

Reading for This Week
---------------------

*   [_Philip and Alex's Guide to Web Publishing_](http://photo.net/wtr/thebook/). Chapters 3, 13, 14 and 15.
*   [Using the ArsDigita Community System](http://photo.net/wtr/using-the-acs.html).

Objectives
----------

Students should learn:

*   The fundamentals of the ArsDigita Community System (the "ACS", a toolkit that we'll be living with for the rest of the semester),
*   How to build, install, and document a new module in the ACS,
*   Rudiments of ecommerce; how to bill someone's credit card.

In the first problem set, you learned how to use lots of new software tools. Mostly you needed to learn commands and procedure calls. You didn't care about the internals of Unix®, Oracle, AOLserver, or the Tcl interpreter. In this second problem set, you will learn how to extend a library of source code. Thus, you'll have to learn about the internal structure of the toolkit. If you thought looking at your own source code was unpleasant, here's your chance to look at 200,000 lines of someone else's :-)

[The Big Picture](#1)  
[The Big (Software) Picture](#2)  
[Making it Real](#3)  
[The Wide World of Oracle](#4)  
[The Narrow World of AOLserver](#5)

{{< anchor "1" >}}{{< /anchor >}}The Big Picture
------------------------------------------------

Most software development projects start with a dialog between the users and the application programmers (you guys). From this dialog, the application programmers form a big picture of what needs to be done.

Users: We're the world's leading experts on computers. We have a big building full of researchers, hundreds of advanced computer systems, and a handful of conference rooms. We'd like a Web-based system for scheduling conference rooms.

You: How are you doing it now?

Users: Secretaries, pencils, and paper appointment books.

You: Ah. So how do you envision the computer-based system working?

Users: Jane Nerd comes to the server and asks for a room from Time X to Time Y, capable of holding N people. The server offers Jane a choice of rooms, each one accompanied by a description of the AV facilities, and she can pick whichever she likes the best.

You: Anything else special?

Users: There is a set of Uber-Users. If Alice Uber can't find a room that she likes, we want her to be able to boot some grunts out of their reserved spot. Alice should not be able to preempt another Uber-User, though. The booted grunt should get an email alert from the server saying "Alice booted you; come back and pick another room."

You: So any user can reserve any room?

Users**:** No. Most of the conference rooms are reservable by anyone. However, some rooms require approval of reservations by a designated administrator (who can be any user in the community), unless the reservation was requested by an Uber-User. The designated administrator is notified by email any time that a reservation is requested for a particular room, and should be able to approve or deny the request by clicking on one of two encoded URLs in the email message.

{{< anchor "2" >}}{{< /anchor >}}The Big (Software) Picture
-----------------------------------------------------------

You're building a program that keeps track of users, groups of users, rooms, and room reservations. You could build this program from scratch, but we already have a documented debugged toolkit, the ArsDigita Community System, designed to support communities of users, subsets of which are collected into groups. You can probably get your program finished faster if you start with the toolkit, but more importantly you will achieve higher reliability (e.g., the user registration stuff has been debugged already) and higher maintainability (because the user and user groups portions of the software are documented in a book chapter residing at a permanent URL: [Chapter 3: Scalable Systems for Online Communities](http://photo.net/wtr/thebook/community.html)).

Some of what you'll be doing in this problem set is creating RDBMS tables from scratch and writing Tcl procedures to perform transactions on those tables, just as you did in problem set 1. Much of what you're doing here, though, is figuring out clean ways of using and extending the toolkit. Sometimes this is as simple as visiting the toolkit admin pages (/admin) of your server and using Web forms to add user groups.

### Exercise 0: Installing the Arsdigita Community System

The first thing you need to do is install the ACS. It is the base of code from which you will be building your service. Here are the steps for you to follow:

> *   Move your code from problem set 1 to your home directory.
>     
>     mkdir /home/stdXX/ps1  
>     cd /web/studentXX/  
>     mv \* /home/stdXX/ps1
>     
> *   Fetch the current ACS, and install it in your web directory. You might find the following commands helpful.
>     
>     cd /web/studentXX/  
>     wget http://software.arsdigita.com/dist/acs....tar.gz  
>     gunzip acs....tar.gz  
>     tar -xf acs....tar  
>     mv acs/\*.
>     

Read the installation instructions. (Which should also now be available in your /web/studentXX/www/doc/ directory.) Your AOLServers have already been configured so you can start at "Feeding Oracle the Data Model," and skip the "Configuring AOLServer" section. Remember to run the "load-geo-tables" script before loading the ACS data model.

### Exercise 0.5: Give Your Server a Personality

The "parameters" subdirectory contains server personality constants in a file called student\*\*.ini. This has been customized with your server's name (e.g., "yourservername" will be changed to "student51"). You will also find the original ad.ini in your parameters file. You can use this for reference but if you want to change the behavior of your server, you must edit server\*\*.ini.

One horrible bug with AOLserver (at least in 2.3.2): If there are Emacs backup files in the /parameters directory that start with #, you might find that your AOLserver is unable to start. If all seems hopeless, do a m-x Dired on the /parameters directory and look for backup files to delete.

You should consider at least setting the system name, the publisher name, the email address of the system owner to your actual personal email address. For example, suppose you set the first parameter (SystemName) to "Joan's Scholarly Center" and then restart your server (with restart-aolserver student\*\* from the shell). Subsequently, you'll see many pages of your site display "Joan's Scholarly Center", including the very top-level page.

### Exercise 0.7: Become Your Own Best Friend

You now have an installed ArsDigita Community System but there aren't any users. Visit the top-level page on your site (e.g., http://lcsweb51.lcs.mit.edu/) and note that you're asked to log in. Type your standard personal email address and register as a user.

### Exercise 0.8: Notice that Your Server is Self-documenting

Use a Web browser to connect to the /doc subdirectory of your server and notice that you have a complete set of ArsDigita Community System documentation. You might find it useful to read the installation instructions and you will certainly want to read the developer's guide (developers.html). The latter starts off with an admonition to use proc\_doc instead of proc when defining externally called procedures. Please follow this convention for the rest of the semester.

### Exercise 0.9: Create Some Users

As noted in the installation instructions, you'll want to add yourself as a user, log in as the "system" user, add yourself to the site administrators user group, and change the passwords of system and anonymous users.

Repeatedly visit the login pages to create a bunch of users for your community, i.e., pretend you are "billg@microsoft.com", "president@whitehouse.gov", "wile\_e\_coyote@iq207.supergenius.com" and fill out the registration form. Remember that these will be the folks who are reserving conference rooms. Make sure that at least a couple of them have real email addresses because you want to be sure that you can test your email alerts. If you don't have any cooperative friends, you might wish to make yourself a hotmail or yahoo email account. Sadly, you can't make several users with the same email address because we constrain the email column in the users table to be unique.

### Exercise 0.95: Create a User Group

Read the documentation for the permissions package at /doc/permissions.html. You're going to use this package to figure out if someone is an uber user or not.

Using the Web forms in /admin/ug/ create a new user group of type "Administration." Call it "Reservation System Uber Users." The module field should be "reserve" and leave the submodule field blank.

This is the only user group that you will need to complete the pset. Using the admin forms, place one or two users in this group. They don't need to have any special role within the group, simple membership is fine.

### Exercise 1: Build the Data Model

Using the file naming and placement conventions set forth create a SQL data model file to hold new table definitions for your room reservation system. Since part of the naming convention is that your data model file must be called "module-name.sql", you have to come up with a name for your module. In order to make life easier for us in looking over your shoulder, please refrain from being creative and call your module "reserve".

A good thing to do now is to step away from your computer with hardcopies of this problem set and [Data Modeling](http://photo.net/sql/data-modeling.html). Read the problem set cover-to-cover before writing anything down. Otherwise, halfway through the pset, you might find yourself having to back up, alter tables, rewrite .tcl scripts, and rewrite documentation.

Here are some guidelines for your data model. Please note that this is not intended to be a complete listing of all the necessary steps.

> *   Remember that users of your system are already represented in the ArsDigita Community System users table; an Uber User will be recognized by membership in the Uber Users user group that you created.
>     
> *   In order that your data model be comprehensible to other ArsDigita Community System programmers, try to follow variable and table-naming conventions. Start by skimming Chapter 3 of Philip and Alex's Guide to Web Publishing, looking only at create table statements. Then surf around inside your own Web server's /doc/sql/ directory and look at community-core.sql, news.sql, and other files. When you're done, you should have picked up items such as
>     *   boolean data are represented in columns ending in "\_p" and are char(1) with a check constraint limiting them to "f" and "t" (the "\_p" comes from "predicate" and is an ancient Lisp programmer's convention)
>     *   keys in the database are all-lowercase, no spaces, underscores for separation, e.g., "q\_and\_a" for a bboard presentation type.
>         
> *   Any time you are representing a person, you should do it by referencing the users table (defined in the /doc/sql/community-core.sql file). For example, here is how the bboard system records the identity of the person posting:
>     
>     create table bboard (  
>             msg\_id          char(6) primary key,  
>      ...  
>             user\_id         integer not null references users,  
>      ...
>     
> 
> > You will need to do something similar in the rooms table to record which user is the designated administrator.
> 
> *   You probably want to use generated keys for your table that holds rooms, rather than using the MIT classroom number or name. This will enable your system to handle gracefully situations in which a room is renamed because a rich person has donated money.
>     
> *   To represent room reservations, you can use start\_time and end\_time columns (both of type date) or with start\_time and duration columns (the latter would be of type number and would represent the fraction of a day during which the room was reserved).
>     
> *   Instead of simply recording that a reservation was made (or approved), record the times of the users' actions. Columns of type date may be used for this (and testing for NULL in the approval\_time can be used to find an unapproved reservation).

Now that you're written all of your create table statements and put them in a file named "/doc/sql/reserve.sql", feed the definitions to Oracle. If you don't want to cut and paste into SQL\*Plus, you can use Unix® shell I/O redirection:

> > sqlplus / \< reserve.sql

If you slept through 6.170 and didn't learn how to write bug-free code, you might find yourself running this command repeatedly. Although Oracle provides powerful transaction management, data definition language statements such as create table can not be rolled back. So if your file defines three tables and there is an error in the definition of the second table, you will usually end up succeeding in defining two tables with the preceding command. You can go into Emacs and edit reserve.sql and then rerun the shell command. Oracle will ignore the first and third table definitions since the tables already exist and define only your second table.

You can check your work by using the SQL\*Plus "describe" command, e.g., "describe users" or "describe rooms".

### Exercise 2: Set Down a List of Legal Transactions

Write a design spec for your system.

Include in this file a list of the legal transactions in your system. For each transaction, sketch the SQL and note any external implications, e.g., "administrator is sent email alert".

### Exercise 3a: Get the Basic Admin Pages Done

We're not going to worry about being able to edit room names or designated room administrators for the moment. Instead, let's build the simplest possible system that enables the site owner to create rooms:

> *   Build an index.tcl page under /admin/reserve/ that shows an HTML unordered list (UL) of the names of all the rooms, a blank line, and then an "add a room" link. This page should simply handle the situation in which there are no rooms registered.
>     
> *   Build /admin/reserve/room-add.tcl (or .adp) and /admin/reserve/room-add-2.tcl that will insert a room into the database.

Note: By placing these pages under the /admin directory, you've restricted them to use by the site owner. This is acceptable because we presume adding a room to be an infrequent activity. Therefore, it isn't worth building user submission pages for new rooms and admin approval pages for those submissions.

### Exercise 3b: Write the Basic User Pages

We're not going to worry about uber-users or room approval for the moment. Instead, let's build a basic system:

> *   Build /reserve/index.tcl, the gateway to the room reservation system. The script should start out by checking for a user\_id cookie (use ad\_verify\_and\_get\_user\_id, defined in /tcl/ad-security.tcl) and, if not present, redirecting users over to /register/index.tcl.
>     
>     For a logged-in user, the page starts out with an HTML form for requesting a new reservation. The form contains the following elements:
>     
>     *   start and end date-times (or start time and duration; the user interface does not have to be consistent with the data model). If you want to make life easier for yourself, it is perfectly acceptable to force the user to enter date/times in ANSI timestamp format: "YYYY-MM-DD HH24:MI:SS". Then you can drop this right into Oracle using a to\_date.
>     *   number of people who will be occupying the room
>     
>     Underneath this form, the page displays a personalized list of the rooms that the authenticated user has already reserved for the future, ordered by the start times. By showing this list automatically, you free yourself from having to write special pages to confirm reservations or check reservation history.
>     
> *   Build /reserve/new.tcl that will take input from the form in index.tcl and find available rooms. This page should refrain from showing rooms that are booked for any part of the time period requested or rooms that aren't large enough for the number of people specified. For your query, it is wise to remember that
>     
>     *   you can select from the rooms table with a WHERE clause that subqueries the room\_reservations table.
>     *   Although SQL contains a BETWEEN operator, e.g., t1.start\_time between t2.start\_time and t2.end\_time, it might not be the best way to catch all cases of overlapping rooms.
>     *   a professional SQL programmer would probably end up coding this with a where not exists (select 1 from ...)
>     *   don't worry about efficiency; it is okay to have a query that Oracle executes with an O\[N^2\] plan. It might be fast enough and, in any case, we're not paying you enough to build a scalable system.
>     
>     You'll have to pass the user's input from index.tcl through to new.tcl. You'll either be doing this with URL variables (if you've chosen an interface based on simple links) or HTML form hidden variables (if you've chosen a form interface with method=POST). In /home/nsadmin/modules/tcl/utilities.tcl we've defined export\_url\_vars and export\_form\_vars that facilitate this. If you have start\_time and end\_time defined as Tcl local variables, you can export them to the next page with new-2.tcl?\[export\_url\_vars start\_time end\_time\] or \[export\_form\_vars start\_time end\_time\] inside a form.
>     
> *   Build /reserve/new-2.tcl to take input from new.tcl and ns\_returnredirect back to index.tcl (where the user will see that the room has in fact been reserved). This page should make sure that there isn't a time conflict, then insert the reservation. If there is a time conflict, return an message to that effect and a link back to new.tcl with the user's original parameters in place (presumably this time new.tcl will not offer the already-booked room).

How could there be a time conflict given that the preceding page only showed free rooms? Here's the concurrency situation which we're worried about:

{{< tableopen >}}
{{< tropen >}}
{{< tdopen >}}
Olin Egghead says that he needs a room for 8 people at 3:00 pm tomorrow, for a 45-minute meeting.
{{< tdclose >}}
{{< tdopen >}}

{{< tdclose >}}

{{< trclose >}}
{{< tropen >}}
{{< tdopen >}}
Your module serves Olin the new.tcl page, showing that the Boesky Room is available.
{{< tdclose >}}
{{< tdopen >}}
Dorothy Alvelda says that she needs a room for 5 people tomorrow at 3:30 pm.
{{< tdclose >}}

{{< trclose >}}
{{< tropen >}}
{{< tdopen >}}


Olin is about to confirm the Boesky Room when the phone rings. A Wall Street firm is offering him $250,000 to become a C programmer in their commodities trading division. Olin immediately replies that he wouldn't consider abandoning his sacred academic principles for a minute.

An hour later, Olin is still on the phone.


{{< tdclose >}}
{{< tdopen >}}


Since Olin has not yet booked any rooms, new.tcl shows Dorothy that the Boesky Room is among the available rooms at 3:30 tomorrow.

Dorothy confirms the Boesky Room and new-2.tcl inserts a row into the database.


{{< tdclose >}}

{{< trclose >}}
{{< tropen >}}
{{< tdopen >}}
Olin hangs up and gets preoccupied with copying impressive-looking equations from the pizza delivery section of the Thessaloniki Yellow Pages into his latest research paper.
{{< tdclose >}}
{{< tdopen >}}

{{< tdclose >}}

{{< trclose >}}
{{< tropen >}}
{{< tdopen >}}
Olin remembers that he needs that room for tomorrow and unburies his browser window, but does not reload new.tcl (i.e., he is looking at the page generated a couple of hours ago). Oh yes, the Boesky Room will be fine. Olin clicks to reserve the Boesky Room at 3:00 pm tomorrow.
{{< tdclose >}}
{{< tdopen >}}

{{< tdclose >}}

{{< trclose >}}

{{< tableclose >}}

If you want to act like a professional Web programmer, you therefore have to run the same query in new-2.tcl that you ran in new.tcl to make sure that the room is in fact still free.

What do the users get from their professionally programmed systems? Double booked rooms. The average professional Web programmer probably won't handle the case where two users submit room requests at precisely the same second.

Hold yourself to a higher standard by making new-2.tcl lock the reservations table before querying to confirm room availability. Read the transactions chapter of [_SQL for Web Nerds_](http://photo.net/sql/) to find out how to lock tables.

{{< anchor "3" >}}{{< /anchor >}}Making it Real
-----------------------------------------------

We've got the rudiments. The site owner can define rooms. Users can book rooms. Reservations won't conflict. If you were a West Coast software company, you'd declare victory at this point. Issue some press releases, ship the product in a nice-looking shrink-wrapped box (as Release 3.2), and sell some shares to the public.

One of the ugly facets of offering a Web service, though, is that users will hammer you with email until you make the thing real.

### Exercise 4: Finish the Admin Pages

Here's a step-by-step plan:

> *   Link each room in the /admin/reserve/index.tcl listing to room-edit.tcl where the site owner can change a room's name or mark a room as requiring approval. Part of marking a room as requiring approval is using /user-search.tcl to let the site owner choose a room administrator from the users table.
>     
> *   Build a /admin/reserve/reports/ directory. The index.tcl page in this new directory should list the rooms and, for each room, show the total number of hours that the room has been reserved and the average number of people in a reservation for that room. This list should be sortable by name or number of hours booked. All the stats should be limited to reservations in the preceding 12 months (use the ADD\_MONTHS function in SQL).
>     
>     Hints: You can do this report with a 4-line SQL query. You don't need to do any date arithmetic, summing, rounding, or averaging in Tcl (Oracle SQL has functions for these things). Keys to solving the problem are reading up on the SQL GROUP BY clause. Also note that order by 3 will order by the 3rd item in the select list.
>     
>     If you want to really do something nice, make the report include rooms for which there haven't been any reservations (showing the total hours as 0; hint: use NVL). If you want to preserve the one-year limit, you'll end up with a SQL query of the form
>     
>     select ...  
>     from table1, (select ... from table2 where ...) as table2  
>     where table1.col1 = table2.col2(+)
>     
> *   Here you're querying from a real table and a view created on-the-fly from table2.
> *   Make the room names in the /admin/reserve/reports/index.tcl page hyperlink anchors. The targets for those anchors should be a details.tcl page where all the reservations are shown, most recent at the top.

### Exercise 5: Finish the User Pages

> *   Edit /reserve/new.tcl so that a member of the Uber User group sees both unreserved and reserved-but-bumpable (i.e., not reserved by another Uber User) rooms. A user who is not an Uber User should see a flag by rooms that require approval.
>     
> *   Edit /reserve/new-2.tcl to recognize the following cases:
>     
>     *   Uber User bumping regular user (delete original reservation; send email to regular user; place Uber User's reservation)
>         
>     *   Regular user requesting approval-required room (insert reservation, email administrator, serve special page to user saying "your request has been submitted to ... and we'll let you know if it is approved")
>         
>     
>     See the AOLserver Tcl Developer's Guide at [AOLserver](http://aolserver.com/) **for an explanation of the ns\_sendmail API call.**
>     
> *   We would like room administrators to be able to approve or deny room requests directly from their email client ("one-click approval"). So the email notice to a room administrator must contain a URL which, if visited, will approve the room request without the room administrator having to log in. An alternative URL in the same email message would allow the room admin to deny a reservation request. This mechanism should be reasonably secure, i.e., a user should not be able to approve his or her own request by doing some URL surgery.
>     
>     Hint: You can write a secret key into the reservations table at the time of the request and require that a one-click approval URL contain the secret key. Tcl doesn't contain a random number generator, though you can call randomRange, defined in our /home/nsadmin/modules/tcl/utilities.tcl file. Alternatively, you can simply use the integer returned by ns\_time.
>     

You now have a working system as envisioned by your clients. Congratulations.

### _Exercise 6: Making it Pay_

If you're not doing this on an ArsDigita-maintained machine, you'll probably find it more trouble than it is worth to install the CyberCash software on your machine (painful), install our CyberCash interface from AOLserver (annoying), and get a merchant credit card account with CyberCash capability (takes 6 weeks and costs around $1000). You can still do the exercise, though, by using Eve Andersson's CyberCash emulator.

Occasionally folks in Academia ask themselves "What is there that distinguishes us from McDonalds, Microsoft®, and Oracle Corporation, aside from a lower growth rate and smaller revenues?" If the answer turns out to be "not much" then why not apply commercial logic to the conference room scheduler?

Assume that the admin reports show that the Boesky Room is in heavy demand. Let's make people pay to use the room. We'll bill their credit card numbers in real-time. Best of all: no refunds (even if the fee-based room has a room administrator and that person doesn't approve the request).

We're not kidding. Your code will be putting real transactions into the real banking system using CyberCash. If you set a $1000 price for a room and use your credit card to test your software, you will find a $1000 charge on your next statement. Ergo, you might prefer to set prices more in the range of 50 cents.

Here's the plan:

> *   Add a column to your data model so that a room can have an associated charge (a number; name the column fee unless you are passionately devoted to something else). Assume that we're only using US dollars so you don't need to record units.
>     
>     Remember that one of the beauties of Oracle is that you can add columns to tables and legacy software will continue to work. Visit [the data modeling chapter of SQL for Web Nerds](http://photo.net/sql/data-modeling.html) if you need to be reminded of the syntax of alter table.
>     
> *   Alter the reservations table so that it has some of the ecommerce fields mentioned in [Chapter 14: ecommerce](http://photo.net/wtr/thebook/ecommerce.html), e.g., order\_state, name\_on\_card, billing\_zip\_code.
>     
> *   Augment /admin/reserve/room-edit.tcl so that the administrator can adjust the prices of various rooms and then add a small fee to one room (we recommend making this 0.25 or 0.50; remember that these are real credit cards and real money).
>     
> *   Augment /reserve/new-2.tcl so that it will refuse to record a reservation for a room that has a non-NULL or non-0 fee. You'll be sending fee-based reservations into a separate ordering pipeline (see below) and you don't want clever nerds doing URL surgery to be able to bypass your billing system.
>     
> *   Augment /reserve/new.tcl so that it mentions the fee for rooms that have one and links reservations for those over to http://yourservername/reserve/new-2-with-fee.tcl
>     
> *   Augment /reserve/index.tcl so that it can display the following:
>     *   future rooms reserved for which a fee has been paid
>     *   old fee-based reservations
>     *   failed attempts to reserve fee-based rooms
> *   Build /reserve/new-2-with-fee.tcl so that it generates a reservation\_id (for double-click protection) and presents a form asking for a credit card number, the name on the card, expiration date, and billing zip code. The form should target /reserve/new-2-with-fee-2.tcl
>     
> *   Build /reserve/new-2-with-fee-2.tcl to:
>     
>     1.  Open a transaction and take a lock on the reservations table.
>         
>     2.  Check to see if there is already a row in the reservations table with the same reservation\_id; if so, assume double click, abort the transaction (ns\_db dml $db "abort transaction", ns\_returnredirect to index.tcl page, and call return to terminate execution of the page.
>         
>     3.  Insert a row in the reservations table with order\_state of "confirmed". As explained in the "An Extra Layer of Transactions" section of [Chapter 14: ecommerce](http://photo.net/wtr/thebook/ecommerce.html), you want to make sure that you write something into your local database before going out to the credit card system and trying to bill someone's card.
>         
>     4.  Release the lock (end the transaction).
>         
>     5.  Bill credit card via CyberCash. Everyone in the class is using the same CyberCash account. CyberCash requires unique order IDs. So that you won't conflict with other students, you have to be absolutely sure that your order ID starts with "yourusername-" and then the reservation ID. You have two financial options. You can do an mauthonly with CyberCash, which will result in your credit limit being knocked down by the amount of the charge. No money will be withdrawn from your account. Or you can do the mauthonly and then a postauth to mark the transaction for settlement. In this case you'll find a charge on your next credit card bill from "ArsDigita, LLC" (we're using philg's personal merchant account; your money will be donated to charity or spent on dog treats).
>         
>     6.  Update the row to reflect "authorized" or "failed".

See [cybercash.com](http://www.cybercash.com/) if you really want to understand the CyberCash system. If you just want to learn enough to get by, look at the code in order-2.tcl (from the open-source [ArsDigita Shoppe package](http://www.eveandersson.com/arsdigita/free-tools/shoppe.html)). Interesting lines in order-2.tcl include the following:

> #the auth step  
> cc\_send\_to\_server\_21 "mauthonly" $args $cc\_output
> 
> \# the mark for settlement step; only do this if you  
> \# want the thrill of seeing charges on your bank statement  
> cc\_send\_to\_server\_21 "postauth" $args $cc\_output
> 
> *   Modify /new.tcl and /new-2.tcl to prevent booking of rooms where there is already a reservation in any order\_state except "failed".
> *   Build a financials directory /admin/reserve/money/ where you show lists of paid reservations and any problems, e.g., orders that were confirmed some time ago but didn't get pushed into "failed" or "authorized".

Congratulations. You're now an Official Web Entrepreneur (TM).

### Exercise 7 (Extra Credit): Find a Venture Capitalist

Take the code that you wrote for this problem set. Give it a name that includes the neologism "middleware". Find a venture capitalist to give you $6 million.

{{< anchor "4" >}}{{< /anchor >}}The Wide World of Oracle
---------------------------------------------------------

We're going to shift gears now into a portion of the problem set designed to teach you more about Oracle and SQL.

### Oraexercise 1: Concurrency and Isolation

Connect to Oracle in SQL\*Plus and type "describe my\_stocks" to make sure that your my\_stocks table is still defined from {{% resource_link 7f0f2453-a29c-f68c-2cf3-34d9648cc6f2 "problem set 1" %}}.

> *   start a second SQL\*Plus session, connected as the same user as your first
> *   type "delete from my\_stocks;" in Session 1
> *   type "select \* from my\_stocks;" in both sessions
> *   type "rollback;" in the Session 1

### Oraexercise 2: The Importance of Commitment

> *   go back into SQL\*Plus:
>     
>     SQL> create table foobar (  
>     the\_key integer primary key  
>     );  
>     SQL> insert into foobar (the\_key)  
>     values (1);
>     
>     1 row created.
>     
>     Leave this session open and do not type COMMIT
>     
> *   build an AOLserver .tcl page that gets a database handle and then executes the same insert into foobar (the\_key) values (1) statement
> *   go to a Web browser and request your AOLserver .tcl page
> *   watch the Web browser hang
> *   type "COMMIT;" into SQL\*Plus
> *   look at the AOLserver error log

Repeat the above sequence but use ROLLBACK at the end in SQL\*Plus instead of COMMIT.

{{< anchor "5" >}}{{< /anchor >}}The Narrow World of AOLserver
--------------------------------------------------------------

Imagine that you are a working Web developer and a client calls you to ask how much traffic the site you've built is getting. At the very least, you need the following skills:

> *   Find your AOLserver's access log and load it into an Emacs buffer (hint: it should be at /home/nsadmin/log/yourservername.log). Generally we configure our AOLservers to roll the log at midnight so you're only looking at requests since early this morning.
> *   Use "m-x count-lines-page" to figure out how many hits there have been so far today.
> *   Go to the end of the buffer ("m->"). Grab a page from your server with a Web browser. Type "m-x revert-buffer" and make sure that the new request is logged (this way you know you're looking at your live access log).

Then the client calls upset because a development site is available for public access. Read the AOLserver administrator's guide at www.aolserver.com and then restrict access to URLs beginning with /foobar. Actually make a directory "foobar" under your server root and place a file in it. See if you can grab the file from a browser. (Note that AOLserver 3.0 will have you editing the perms database files, and calling Tcl procs, while 2.3 has a fancy GUI interface for editing permissions while 3.0.)

Mostly relevant for AOLserver 2.3: suppose that the client wants access to server admin pages, e.g., so that they can create new users. You don't want to give the client access to the nsadmin password, which you might share with other programmers for use on many servers. Create a new user called "myclient" in the "system" group so that they can have the same privileges as nsadmin.

Who Wrote This and When
-----------------------

This problem set was written by Philip Greenspun and Hal Abelson in February 1999. It is copyright 1999 by them but may be reused provided credit is given to the original authors.