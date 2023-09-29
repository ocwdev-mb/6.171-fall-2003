---
content_type: page
description: ''
draft: false
learning_resource_types:
- Assignments
ocw_type: CourseSection
parent_title: Assignments
parent_type: CourseSection
parent_uid: 7c3bc40c-1950-1cf6-ba32-c5d48982344b
title: Basic db-backed Web sites, AOLserver, Tcl, SQL
uid: 7f0f2453-a29c-f68c-2cf3-34d9648cc6f2
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---
File decompression software, such as [Winzip®](http://www.winzip.com/) or [StuffIt®](https://my.smithmicro.com/stuffit-expander-mac.html#), is required to open the .tar files in this section. Any number of software tools can be used to import the .csv files in this section.

## Reading for This Week

- [Philip and Alex's Guide to Web Publishing](http://philip.greenspun.com/panda/?). Chapters 1, 4, 5, 10 and 11.
- [SQL for Web Nerds](http://philip.greenspun.com/sql/). Chapters 1-9.
- [Tcl for Web Nerds](http://philip.greenspun.com/tcl/) and/or _Practical Programming in Tcl and Tk_ (Brent Welch 1997; Prentice-Hall), all the chapters up until the Tk stuff and/or the Tcl 8.2 main pages.
- Introduction to AOLserver: [Part 1](http://philip.greenspun.com/wtr/aolserver/introduction-1) and [Part 2](http://philip.greenspun.com/wtr/aolserver/introduction-2).
- Reference: [_Using the LCS Web/db Computing Facility_](http://philip.greenspun.com/teaching/manuals/usermanual/).

Helper and example files (if you're not doing this at MIT): 6916.ps1.tar ({{% resource_link d8238c85-463a-e0ce-4542-ed92c431f46f "TAR" %}}) (This .tar file includes 1 .txt, 2 .adp, 1 .csv, 10 .tcl files.)

## Objectives

We're trying to make sure that everyone knows:

- How to log into his or her development server
- Rudiments of Tcl
- How to run Tcl via tclsh
- about the ADP templating facility of AOLserver
- How to create, execute, test, and debug a .tcl page
- How to write a .tcl page that queries a foreign server
- Rudiments of SQL
- How to query Oracle from the shell using SQL\*Plus
- How to write a .tcl page that queries Oracle
- How to personalize Web services by issuing and reading cookies
- How to read and write data in XML
- How to use SQL\*Loader

This first problem set requires you to learn a lot of new software, so make sure you get started early: plan to spend at least two or three sessions on it. There is nothing difficult here, but we do want to lead you through the mechanics of using Tcl, SQL, and running the Web server.

[Getting Started with Tcl](#1)     
[Getting Started with SQL\*Plus](#2)     
[Working with AOLserver and Oracle](#3)     
[Personalizing Web Services with Cookies](#4)     
[Sharing Data with XML](#5)     
[The Wide World of Oracle](#6)     
[Information Architecture and User Interface](#7)

## {{< anchor "1" >}}{{< /anchor >}}Getting Started with Tcl

Start by reading _Using the LCS Web/db Computing Facility_ and following its instructions to log into your server machine.

### Exercise 1: Running Tcl from the Shell

Run Emacs. Type "m-x shell" to get a Unix® shell. Type "tclsh" to start the Tcl shell program. Define a _recursive_ Fibonacci procedure in Tcl. Execute and test.

Hint: If you're writing Tcl programs of more than two or three lines, you may find it convenient to type the code into a separate Emacs buffer (set to tcl mode) and cut and paste from there into the Tcl shell buffer.

Type info tclversion at the tclsh prompt to make sure that you're running Tcl 8.2, the same version that is compiled into AOLserver.

### Exercise 2: Running Tcl from an (almost) HTML Page

Look at two-plus-two.adp ([HTM]({{< baseurl >}}/pages/assignments/twoplustwo.htm)) source ([TXT](/courses/electrical-engineering-and-computer-science/6-171-software-engineering-for-web-applications-fall-2003/assignments/twoplustwoadp.txt)). This is an example of the ADP templating facility in AOLserver.

Augment the page so that (1) you add a $4000 South American Cichlid aquarium as an option, (2) you use a constructor procedure to build each aquarium element (instead of simply calling list), (3) you add an element to the aquarium for how many of each type of aquarium will be installed (4) you use procedures to extract type, cost and quantity from an aquarium element (instead of simply calling lindex), (5) you print out quantity-dependent subtotals and the grand total at the bottom.

### Exercise 3: Simple Tcl Pages

Using the Web browser running on your local machine, visit the URL [http://yourvirtualserver/psets/ps1/simple-tcl-page.tcl](http://yourvirtualserver/psets/ps1/simple-tcl-page.tcl). Using Emacs running on the server machine, examine the source code for this page in /web/yourvirtualserver/www/psets/ps1/simple-tcl-page.tcl. Also look at the source code for the target of the form in /web/yourvirtualserver/psets/ps1/simple-tcl-page-2.tcl. (If these files are missing, download them from 6916.ps1.tar ({{% resource_link d8238c85-463a-e0ce-4542-ed92c431f46f "TAR" %}}) (This .tar file includes 1 .txt, 2 .adp, 1 .csv, 10 .tcl files.) and put them in /web/yourvirtualserver/www). Notice how we use Tcl to read the form variables. Try out the form a couple of times, using your browser. Now debug the regular expression in simple-tcl-page-2.tcl so that it properly handles the names "Tammy Faye Baker" and "William H. Gates III".

Hint 1: it is easier if you don't try to do this in one regexp. Use if then elseif then elseif …

Hint 2: regexp has a side-effect. If you use a multi-clause if statement, make sure that you wrap your calls to regexp in braces so that they don't all get evaluated immediately.

### Exercise 4: Tcl Pages that Query Foreign Servers

Using the Web browser running on your local machine, visit the [Seeq](https://www.seeq.com/). Read the discussion of this program in [Chapter 10](http://philip.greenspun.com/panda/server-programming?) of Philip and Alex's Guide to Web Publishing. Drawing upon that program as a model, build a new web service that takes the ISBN of a book from a form and then uses ns\_httpget to query several online bookstores to find price and stock information and displays the results in an HTML table. Save your program in files called /web/yourvirtualserver/www/psets/ps1/books.tcl and books-2.tcl so people can access your service over the web.

We suggest querying wordsworth.com, barnesandnoble.com, and [www.1bookstreet.com](http://www.1bookstreet.com) (amazon.com tends to respond with a 302 redirect you if the client doesn't give them a session ID in the query). Your program should be robust to timeouts, errors at the foreign sites, and network problems. You can ensure this by wrapping a Tcl catch statement around your call to ns\_httpget. Test your program with the following ISBNs: 0385494238, 0062514792, 0140260404, 0679762906.

Extra credit: From which of the preceding books is the following quote taken?

"The obvious mathematical breakthrough would be development of an easy way to factor large prime numbers."

This would be a good time to take break.

## {{< anchor "2" >}}{{< /anchor >}}Getting Started with SQL\*Plus

Start up again with Emacs (you took a break, right?) and start a Tcl shell as before ("M-x shell" then "tclsh"). Type "M-x rename-buffer" to rename the shell to "tcl-shell". Type "M-x shell" to then get a new Unix® shell. Rename this buffer "sql-shell". In the SQL shell, type "sqlplus" to start SQL\*Plus, the Oracle shell client. It's convenient to work like this using two shells, one for Tcl and one for SQL.

### Exercise 5: Talking to Oracle from the Shell

Type the following at SQL\*Plus to a table for keeping track of the classes you're taking this semester:

```plaintext
create table my_courses ( course_number varchar(20));
```

Note that you have to end your SQL commands with a semicolon in SQL\*Plus. These are not part of the SQL language and you shouldn't use these when writing SQL in your Tcl progams for AOLserver.

Insert a few rows, e.g.,

```plaintext
insert into my_courses (course_number) values ('6.916');   
```

Commit your changes:

```plaintext
commit;   
```

See what you've got:

```plaintext
select * from my_courses;
```

One of the main benefits of using an RDBMS is persistence. Everything that you create stays around even after you log out. Normally, that's a good thing, but in this case you probably want to clean up after your experiment:

```plaintext
drop table my_courses;
```

Quit SQL\*Plus by typing "c-c c-d".

### Exercise 6: Tcl Pages that Talk to Oracle

Look at the file /web/yourvirtualserver/www/psets/ps1/quotations.tcl, which is the source code for a page that displays quotations that have been stored in the Oracle database. Visit this page with your Web browser and you should get an error. The reason for the error is that the program is calling a procedure that doesn't exist: ad\_header ("ArsDigita Header"). You can confirm this suspicion by using Emacs to read /home/nsadmin/log/yourvirtualserver-error.log, which is where AOLserver logs any notices or problems.

To get AOLserver to load procedure definitions at server startup, you have to put .tcl files in your server's private Tcl library: /web/yourvirtualserver/tcl/. Create a file called "ps1-defs.tcl" in this directory and define the following Tcl procedures:

ad\_header _page\_title_ -- returns HTML, HEAD, TITLE, and BODY tags, with argument enclosed within the TITLE tags

ad\_footer -- returns a string that will close the BODY and HTML tags

Reload the quotations.tcl page and you get … the same error! AOLserver doesn't know that you've added a file to the private library; this is only checked at server startup. Go to a Unix® shell and "restart-aolserver yourservername" (this is the big hammer; it kills your server's Unix® process so that Unix® will restart AOLserver automatically). If restart-aolserver does not come back with "Killing 10234" or some other process ID, you'll know that you did not succeed (perhaps you made a typo when specifying your server name).

Reload the quotations.tcl page and you get … a slightly different error! The program is trying to query a table that doesn't exist: quotations.

Go back to your sql shell and restart SQL\*Plus. Copy the table definition from the comments at the top of the file quotations.tcl and feed this definition to Oracle. Go back to your Web browser and reload the page that previously gave you an error. Things should now work, although the quotations table is empty.

Use the form on the web page to manually add the following quotation, under an appropriate category of your choice: "640K ought to be enough for anybody" (Bill Gates). Note that it would be funnier if our table had a column for recording the date of the quotation (1981) but we purposely kept our data model as simple as possible.

Return to SQL\*Plus and SELECT \* from the table to see that your quotation has been recorded. The horrible formatting is an artifact of your having declared the quote column to be 4000 characters long.

In SQL\*Plus, insert a quotation with some hand-coded SQL (if you are feeling lazy, you can cut and paste some SQL from the AOLserver error log; all SQL statements that AOLserver sends to Oracle are logged here). Now reload the quotations.tcl URL from your Web browser. If you don't see your new quote here, that's because you didn't type COMMIT; at SQL\*Plus. This is one of the big features of a relational database management system: simultaneously connected users are protected from seeing each other's unfinished transactions.

### Loading Tables from .csv Files

Now it is time to preload your quotations database with some interesting material. Load /web/yourvirtualserver/www/psets/ps1/quotations.csv into Emacs and look at the format of the file (this is a standard kind of output that you can get from any desktop spreadsheet program). Using SQL\*Loader (see Oraexercise 1 below), load these data into your quotations table.

## {{< anchor "3" >}}{{< /anchor >}}Working with AOLserver and Oracle

Let's look at how to access the database from Tcl programs. The basic idea is that AOLServer includes a data abstraction called a _set_, defined by the operations listed under the ns\_set API. A set is a collection of (key,value) pairs, which should be a familar idea from [6.001](/courses/6-001-structure-and-interpretation-of-computer-programs-spring-2005). Selecting from a table with ns\_db select returns an identifier for a set, whose keys are the names of the selected columns. Subsequent successive calls with ns\_db getrow will fill in the values in this set with the values from successive selected rows. For example, suppose you obtain a set identifier by selecting the following table with ns\_select:

{{< tableopen >}}{{< theadopen >}}{{< tropen >}}{{< thopen >}}
WRITERS
{{< thclose >}}{{< thopen >}}
BOOKS
{{< thclose >}}{{< trclose >}}{{< theadclose >}}{{< tbodyopen >}}{{< tropen >}}{{< tdopen >}}
Tolstoy
{{< tdclose >}}{{< tdopen >}}
_Anna Karenina_
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
Steinbeck
{{< tdclose >}}{{< tdopen >}}
_Grapes of Wrath_
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
Greenspun
{{< tdclose >}}{{< tdopen >}}
_Guide to Web Publishing_
{{< tdclose >}}{{< trclose >}}{{< tbodyclose >}}{{< tableclose >}}

Then, after the first call to ns\_db getrow the set will be

```plaintext
{{writer Tolstoy} {book "Anna Karenina"}}
```

After the second call to ns\_db getrow the set will will be

```plaintext
{{writer Steinbeck} {book "Grapes of Wrath"}}
```

and after the third call the set will be

```plaintext
{{writer Greenspun} {book "Guide to Web Publishing"}}
```

The programs in the files quotations.tcl and quotation-add.tcl illustrate these ideas. It will be well worth your while to study these programs until you understand how they work, because you'll be doing a lot of this kind of programming throughout the semester.

### Exercise 6a: Eliminating the Lock Table via a Sequence

Read about Oracle's sequence database object in [Data Modeling](http://philip.greenspun.com/sql/data-modeling). By creating a sequence, you should be able to edit quotation-add.tcl to

- eliminate the lock table
- eliminate the begin and end transaction (since you're no longer tying multiple SQL statements together)
- generate a key for the new quotation within the INSERT statement

### Exercise 7: Improving the User Interface for Data Entry

Go back to the main quotations.tcl page and modify it so that categories entry is done via a select box of existing categories (you will want to use the "SELECT DISTINCT" SQL command). For new categories, provide an alternative text entry box labeled "new category". Make sure to modify quotation-add.tcl so that it recognizes when a new category is being defined.

### Exercise 8: Searching

Add a small form at the top of quotations.tcl that takes a single query word from the user. Build a target for this form that returns all quotes containing the specified word. Your search should be case-insensitive and also look through the authors column. Hint: like '%foo%'.

## {{< anchor "4" >}}{{< /anchor >}}Personalizing Web Services with Cookies

We'd like you to build a system that implements per-user personalization of the quotation database. The overall goal should be

- A user can "kill" a quotation and have it never show up again either from the top-level page or the search page.
- Killing a quotation is persistent and survives the quitting and restarting of a browser.
- Quotations killed by one user have no effect on what is seen by other users.
- Users can erase their personalizations and see the complete quotation database again by clicking on an "erase my personalization" link on the main page. This link should appear only if the user has personalized the quotation database.

You can personalize Web services with the aid of magic cookies. A cookie issued by the server directs the browser to store data in browser's computer. To issue a cookie, the server includes a line like

```plaintext
Set-Cookie: cookie_name=value; path=/ ; expires=Fri, 01-Jan-2010 01:00:00 GMT
```

in the HTTP header sent to the browser. Here cookie\_name is the name for this cookie, and value is the associated value, which can contain any character or format except for semicolon, which terminates a cookie. The path specifies which URLs on the server the cookie applies to. Designating a path of slash (/) includes all URLs on the server.

After the browser has accepted a server's cookie, it will include the cookie name and value as part of its HTTP requests whenever it asks that server for an applicable URL. Your Tcl programs can read this information using the AOLServer API

```plaintext
[ns_set get [ns_conn headers] Cookie]
```

After the expiration date, the browser no longer sends the cookie information. The server can also issue cookies with no specified expiration date, in which case, the cookie is not persistent -- the browser uses it only for that one session.

You can see an example of how cookies are issued and read, by visiting the URL [http://yourvirtualserver/psets/ps1/set-cookies.tcl](http://yourvirtualserver/psets/ps1/set-cookies.tcl) and examining the Tcl for file and the associated URLs check-cookies.tcl and expire-cookies.tcl. Observe how expire-cookies gets rid of cookies by reissuing them with an expiration date that has already past.

Reference: The magic cookie spec is available from [Persistent Client State HTTP Cookies](http://curl.haxx.se/rfc/cookie_spec.html).

### Exercise 9

Implement the personalized quotation system described above.

Hint 1: it is possible to build this system using an ID cookie for the browser and keeping the set of killed quotations in Oracle. However, if you're not going to allow users to log in and claim their profile, there really isn't much point in keeping data on the server. In fact, by keeping killed quotation IDs in your users' browser cookies, you've achieved the holy grail of academic database management system researchers: a distributed database!

Hint 2: it isn't strictly copacetic with the cookie spec, but you can have a cookie value containing spaces. Tcl stores a list of integers internally as those numbers separated by spaces. So the easiest and simplest way to store the killed quotations is as a space-separated list.

Hint 3: don't filter the quotations in Tcl; it is generally a sign of incompetent programming when you query more data from Oracle than you're going to display to the end-user. SQL is a very powerful query language. You can use the NOT IN feature to exclude a list of quotations.

How about taking another break?

## {{< anchor "5" >}}{{< /anchor >}}Sharing Data with XML

As you learned above from querying bookstores, data on the Web have not traditionally been formatted for convenient use by computer programs. In theory, people who wish to exchange data over the Web can cooperate using XML, a 1997 standard from the Web Consortium. In practice, hardly anybody uses XML right now (1999). Fortunately for your sake in completing this problem set, you can cooperate with your fellow students: the overall goal is to make quotations in your database exportable in a structured format so that other students' applications can read them.

Here's what we need in order to cooperate:

- an agreed-upon URL at everyone's server where the quotations database may be obtained: "/quotations-xml.tcl"
- an agreed-upon format for the quotations.

We'll format the quotations using XML, which is simply a conventional notation for describing structured data. XML structures consist of data strings enclosed in HTML-like tags of the form \<foo> and \</foo>, describing what kind of thing the data is supposed to be.

Here's an informal example, showing the structure we'll use for our quotations:

```plaintext
<quotations>
<onequote>
<quotation_id>1</quotation_id>
<insertion_date>1999-02-04</insertion_date>
<author_name>Bill Gates</author_name>
<category>Computer Industry Punditry</category>
<quote>640K ought to be enough for anybody.</quote>
</onequote>
<onequote>
.. another row from the quotations table …
</onequote>
… some more rows 
</quotations>
```

Notice that there's a separate tag for each column in our SQL data model:

```plaintext
<quotation_id>
<insertion_date>
<author_name>
<category>
<quote>
```

There's also a "wrapper" tag that identifies each row as a \<onequote> structure, and an outer wrapper that identifies a sequence of \<onequote> stuctures as a \<quotations> document.

### Building a DTD

We can give a formal decription of our XML structure, rather than an informal example, by means of an XML Document Type Definition (DTD).

Our DTD will start with a definition of the quotations tag:

```plaintext
<!ELEMENT quotations (onequote)+>
```

This says that the quotations element must contain at least one occurrence of onequote but may contain more than one. Now we have to say what constitutes a legal onequote element:

```plaintext
<!ELEMENT onequote (quotation_id,insertion_date,author_name,category,quote)>
```

This says that the sub-elements, such as quotation\_id must each appear exactly once and in the specified order. Now we have to define an XML element that actually contains something other than other XML elements:

```plaintext
<!ELEMENT quotation_id (#PCDATA)>
```

This says that whatever falls between \<quotation\_id> and \</quotation\_id> is to be interpreted as raw characters rather than as containing further tags (PCDATA stands for "parsed character data").

Here's our complete DTD:

```plaintext
<!-- quotations.dtd --> <!ELEMENT quotations (onequote)+>
<!ELEMENT onequote (quotation_id,insertion_date,author_name,category,quote)>
<!ELEMENT quotation_id (#PCDATA)> <!ELEMENT insertion_date (#PCDATA)>
<!ELEMENT author_name (#PCDATA)>
<!ELEMENT category (#PCDATA)>
<!ELEMENT quote (#PCDATA)>
```

You will find this extremely useful… Hey, actually you won't find this DTD useful at all for completing this part of the problem set. The only reasons that DTDs are ever useful is for feeding to XML parsers because they can then automatically tokenize an XML document. For implementing your quotations-xml.tcl page, you will only need to look at informal example.

### Exercise 10: Generating XML

Create a Tcl program that queries the quotations table, produces an XML document in the preceding form, and returns it to the client with a MIME type of "application/xml". Place this in a file quotations-xml.tcl, so that other users can retrieve the data by visiting that agreed upon URL.

To get you started, we've provided /psets/ps1/example-xml.tcl. Requesting this URL with a Web browser should offer to let you to save the document to a local file, and you can then examine it with a text editor on your local machine. (This assumes that you haven't defined some special behavior for your browser for MIME type application/xml.) The differences between our example and your program is that you'll need to produce a document containing the entire table and you'll need to generate it on the fly.

### Exercise 11: Importing XML

Write a program to import a quotations database from another student's XML output page (if you have completed Exercise 9 and your peers have not, this might be a good time to exhort them to greater efforts). Your program must

- Grab /psets/ps1/quotations-xml.tcl from another student's database using ns\_httpget
- Parse the file into records and the records into fields
- If a quote from the foreign server has identical author and content as a quote in your own database, ignore it; otherwise, insert it into your database with a new quotation\_id (you don't want keys from the foreign server conflicting with what is already in your database)

Hints: You might want to set up a temporary table using create table quotations\_temp as select \* from quotations and then drop it after you're done debugging. You should use DoubleApos when presenting data to Oracle for comparisons.

Rather than having you link in a 100,000-line C program (or a 5,000-line Lisp program) that parses XML documents based on a DTD, we've gone for simplicity here by predefining for you a parser in Tcl that understands only this particular DTD for quotations. The procedure is parse\_all ([TXT](/courses/electrical-engineering-and-computer-science/6-171-software-engineering-for-web-applications-fall-2003/assignments/parseall.txt)) (you have to install this file in your server's private Tcl library, /web/yourvirtualserver/tcl/, for this function to be callable by .tcl and .adp pages) . The parse\_all proc takes an XML quotation structure as argument and returns a Tcl list, showing the parts and subparts of the structure. To see an example of the format, use your browser to visit the page [http://yourvirtualserver/psets/ps1/xml-parse-test.tcl](http://yourvirtualserver/psets/ps1/xml-parse-test.tcl).

Note: these exercises are designed to familiarize you with XML. In most cases, sophisticated XML processing should be done inside Oracle using Java® libraries.

### Exercise 12: Tracking a Book's Popularity

Neurotic authors will constantly check amazon.com to see where their book is ranked in terms of sales. That these figures are updated hourly only makes the habit more destructive. Write a program to track a very neurotic author's work (ISBN: 1558605347). You will need to

1. Define an Oracle table to hold ISBN, date-time, sales rank
2. Write a procedure that will grab the Amazon page, REGEXP out the sales rank, and stuff it into the Oracle table
3. Use the AOLserver API call ns\_schedule\_proc to schedule your procedure to run once every hour
4. Build a .tcl page to look at the popularity over time

One of the interesting things about Amazon is that they often lose control of their server farm and database (they write a lot of C code and one programmer's sloppiness can generate a catastrophic failure of the entire service). You might want to build your system so that you can record (a) times when amazon.com is unreachable, and (b) for which of those times the page served contains the string "Our store is closed temporarily for scheduled maintenance" (you'll sometimes get this during the middle of weekdays when they would definitely not have intentionally scheduled any maintenance).

### Exercise 13: Becoming a Chartoonist

Why print a table of a book's popularity when you can print a chart? You're going to learn about the wonders of single-pixel GIFs and WIDTH and HEIGHT tags now. Grab the software and put it in your server's private Tcl directory (/web/yourservername/tcl/). Read the docs and then write code to generate a pretty chart of the data from Exercise 12.

Note that you're dipping into the ArsDigita Community System toolkit here, the software with which you'll be occupied in Problem Set 2.

## {{< anchor "6" >}}{{< /anchor >}}The Wide World of Oracle

We're going to shift gears now into a portion of the problem set designed to teach you more about Oracle and SQL.

### Oraexercise 1: SQL\*Loader

- create a tab-separated file in Emacs containing five lines, each line to contain your favorite stock symbol, an integer number of shares owned, and a date acquired (in the form MM/DD/YYYY)
- create an Oracle table to hold these data: 

```plaintext
create table my_stocks ( symbol varchar(20) not null, n_shares integer not null, 
date_acquired date not null );
```

- use the sqlldr shell command to invoke SQL\*Loader to slurp up your tab-separated file into the my\_stocks table (see page 1183 of Oracle8: The Complete Reference and the official Oracle docs.)

### Oraexercise 2: Copying Data from One Table to Another

This exercise exists because we found that, when faced with the task of moving data from one table to another, programmers were dragging the data across SQL\*Net from Oracle into AOLserver, manipulating it in Tcl, then pushing it back into Oracle over SQL\*Net. This is not the way! SQL is a very powerful language and there is no need to bring in any other tools if what you want to do is move data around within Oracle.

- using only one SQL statement, create a table called stock\_prices with three columns: symbol, quote\_date, price. After this one statement, you should have created the table and filled it with one row per symbol in my\_stocks. The date and price columns should be filled with the current date and a nominal price. Hint: select symbol, sysdate as quote\_date, 31.415 as price from my\_stocks;.
- create a new table: 

```plaintext
create table newly_acquired_stocks ( symbol varchar(20) not null, n_shares integer not null, 
date_acquired date not null );
```

- using a single insert into .. select … statement (with a WHERE clause appropriate to your sample data), copy about half the rows from my\_stocks into newly\_acquired\_stocks

### Oraexercise 3: JOIN

With a single SQL statement JOINing my\_stocks and stock\_prices, produce a report showing symbol, number of shares, price per share, and current value.

### Oraexercise 4: OUTER JOIN

Insert a row into my\_stocks. Run your query from Oraexercise 3. Notice that your new stock does not appear in the report. This is because you've JOINed them with the constraint that the symbol appear in both tables.

Modify your statement to use an OUTER JOIN instead so that you'll get a complete report of all your stocks, but won't get price information if none is available.

### Oraexercise 5: PL/SQL

Inspired by Wall Street's methods for valuing Internet companies, we've developed our own valuation method for this problem set: a stock is valued at the sum of the ascii characters making up its symbol. (Note that students who've used lowercase letters to represent symbols will have higher-valued portfolios than those will all-uppercase symbols; "IBM" is worth only $216 whereas "ibm" is worth $312!)

- define a PL/SQL _function_ that takes a trading symbol as its argument and returns the stock value (hint: Oracle's built-in ASCII function will be helpful)
- with a single UPDATE statement, update stock\_prices to set each stock's value to whatever is returned by this PL/SQL procedure
- define a PL/SQL function that takes no arguments and returns the aggregate value of the portfolio (n\_shares \* price for each stock). You'll want to define your JOIN from Oraexercise 3 (above) as a cursor and then use the PL/SQL Cursor FOR LOOP facility. Hint: when you're all done, you can run this procedure from SQL\*Plus with select portfolio\_value() from dual;.

SQL\*Plus Tip: though it is not part of the SQL language, you will find it very useful to type "/" after your PL/SQL definitions if you're feeding them to Oracle via the SQL\*Plus application. Unless you write perfect code, you'll also want to know about the SQL\*Plus command "show errors". For exposure to the full range of this kind of obscurantism, see the _SQL\*Plus User's Guide and Reference_.

### Oraexercise 6: Buy More of the Winners

Rather than taking your profits on the winners, buy more of them!

- use SELECT AVG() to figure out the average price of your holdings
- Using a single INSERT with SELECT statement, double your holdings in all the stocks whose price is higher than average (with date\_acquired set to sysdate)

Rerun your query from Oraexercise 4. Note that in some cases you will have two rows for the same symbol. If what you're really interested in is your current position, you want a report with at most one row per symbol.

- use a SELECT … GROUP BY query from my\_stocks to produce a report of symbols and total shares held
- use a SELECT .. GROUP BY query JOINing with stock\_prices to produce a report of symbols and total value held per symbol
- use a SELECT .. GROUP BY .. HAVING query to produce a report of symbols, total shares held, and total value held per symbol _restricted to symbols in which you have at least two blocks of shares_ (i.e., the "winners")

### Oraexercise 7: Encapsulate your Queries with a View

Using the final query above, create a view called stocks\_i\_like that encapsulates the final query.

## {{< anchor "7" >}}{{< /anchor >}}Information Architecture and User Interface

You've got a database table filled with stock data. There are a couple of ways to provide a Web interface to these data. The loser Web developer presents a page with options for retrieving stock data:

- show recent acquisitions
- show best performers
- show highest value stocks
- show entire portfolio

The bottom line is that the user doesn't see anything on the first page. From an information presentation point of view, the first page is therefore a waste.

An alternative is to show the user a table of holdings right on the top-level page, with a sensible subset of the data by default, and provide controls to adjust what is included in the display. What kind of controls? You could have the ones above, plus whatever other views the publisher of the site and the users eventually decide are necessary. Suppose, though, that you can organize the controls along orthogonal dimensions. If you can do that, with just a handful of "dimensional sliders", the user will have many options.

For example, the ArsDigita ticket tracking system is used to store bug reports and feature requests. The following dimensions are employed

- involvement of connected user (values: mine/everyone's)
- ticket status (values: open/+deferred/+closed)
- ticket age (values: last day/last week/last month/all)

Note that even though these are modeled as continuous dimensions, the user is not presented with continuous sliders. The user picks one of several discrete points on each dimension. This interface is compatible with the Netscape 1.1 browser and provides the user with 24 choices in total. Yet instead of seeing 24 options in a big list, the user sees one line across the top of the browser, with nine buttons arranged logically into three dimensions.

If those 24 options aren't enough, the ticket tracking system lets the user re-sort the table by any of the columns by clicking on the column heading.

Try building the same sort of thing for your stock portfolio. You want a .tcl page that shows the contents of my\_stocks with stock\_prices. Provide controls across the top (hint: TABLE WIDTH=100% and TD ALIGN= RIGHT will be useful) for the following dimensions:

- recency of acquistions (within last week/last month/last year/all)
- value of holding (more than 10% of total portfolio/more than 2%/all)

Provide the ability for users to sort by any of the columns presented (hint: export\_ns\_set\_vars in $SERVER\_HOME/tcl/00-ad-utilities.tcl will be useful for this, notably because of the exclusion\_list argument), e.g., symbol, number of shares, price per share, value of holding.

You can build this from scratch or use the ArsDigita Community System toolkit API calls as a building block.

## Who Wrote This and When

This problem set was written by Philip Greenspun and Hal Abelson in January 1999. It is copyright 1999 by them but may be reused provided credit is given to the original authors.