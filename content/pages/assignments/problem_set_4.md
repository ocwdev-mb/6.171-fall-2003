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
title: Metadata, Knowledge Management
uid: d86b9c5c-e13d-887d-9446-c7ba7d8fa527
video_files:
  video_thumbnail_file: null
video_metadata:
  youtube_id: null
---
## Reading for This Week

- Transactions chapter from Oracle Concepts document.

## Objectives

Teach students the virtues of metadata. More specifically, they learn how to formally represent the requirements of a Web service and then build a computer program to generate the computer programs that implement that service.

[The Huge Picture](#1)     
[The Big Picture](#2)     
[The Medium-Sized Picture](#3)     
[A Data Model to Represent your Data Model](#4)     
[Optional Assignment](#5)

## {{< anchor "1" >}}{{< /anchor >}}The Huge Picture

Organizations have complex requirements for their information systems. They also have insane schedules and demand that you build their system within weeks. Finally, they are fickle and have no compunction about changing the requirements mid-stream.

## {{< anchor "2" >}}{{< /anchor >}}The Big Picture

Corporations all have knowledge management systems even though generally they may not have any knowledge. Universities claim to have knowledge and yet none have knowledge management systems. You are going to build a knowledge management system for your university's computer science department. In order to ensure that you can get a job after this course is over, you must refer to your final product as "a KM system".

Another issue is a perennial side-show in the world-wide computer programming circus: the spectacle of nerds arguing over programming tools. The data model can't represent the information that the users need, the application doesn't do what what the users need it to do, and instead of writing code, the "engineers" are arguing about Java® versus Lisp versus Perl versus Tcl. If you want to know why computer programmers get paid less than medical doctors, consider the situation of two trauma surgeons arriving at an accident scene. The patient is bleeding profusely. If surgeons were like programmers, they'd leave the patient to bleed out in order to have a really satisfying argument over the merits of two different kinds of tourniquet.

If you're programming one Web page at a time, you can switch to the Language du Jour in search of higher productivity. But you won't achieve significant gains unless you switch from writing code for one page. You need to think about ways to write down a formal description of the application and user experience, then let the computer generate the application automatically.

## {{< anchor "3" >}}{{< /anchor >}}The Medium-Sized Picture

Knowledge is text, authored by a user of the community. The user may attach a document, photograph, or spreadsheet to this text. Other users can comment on the knowledge, submitting text and optional attachments of their own. What distinguishes a knowledge management system from the standard /bboard module of the ACS? The following:

- Links among knowledge objects
- Facilities for browsing and searching that are more powerful than in the standard /bboard system, where browse-by-category is the only option beyond full-text search
- Tracking and rewarding of contributions and reuse

## {{< anchor "4" >}}{{< /anchor >}}A Data Model to Represent your Data Model

Business people like to talk about "objects" rather than "rows in tables". This doesn't mean that the MBA curriculum includes inheritance and method combination in the Common Lisp Object System. What seems to have happened is that

1. Xerox PARC and MIT developed object-oriented programming systems in the 1970s, including Smalltalk and the Lisp Machine
2. People hailed these systems as rather advanced
3. The business community finally picked up the object-oriented buzz during the 1990s

When a business person talks about an "object", what he or she generally means is "a row in a relational database table".

In order to make your system comprehensible to the CIO/CTO types who will be adopting it, we'll use their vocabulary. A table row is an "object" and each column is an "element" of that object.

We need a way to represent the kinds of objects that our system will represent first. Let's assume that we'll have at least the following object types:

- Person
- Publication
- Data structure
- System
- Algorithm
- Problem
- Language

To say that "John McCarthy developed the Lisp programming language", the user would create two objects: one of type language and one of type person. Why not link to the users table instead? John McCarthy might not be a registered user of the system. Some of the people you'll be referencing, e.g., John Von Neumann, are dead. Characteristics of the Lisp language would be stored as elements of the language object.

For an example of what a completed system of this nature might look like to the casual reader, visit Paul Black's [_Dictionary of Algorithms and Data Structures_](http://www.nist.gov/dads/).

For each object type we'll be creating an Oracle table. For each Oracle table we create, we store one row in the metadata table:

```plaintext
create table km_metadata_object_types (
        table_name              varchar(21) primary key,
        pretty_name             varchar(100) not null,
        pretty_plural             varchar(100)
);
```

We need to store information about what elements will be kept for each type of object. Note that some elements are common across object types:

- Name (a short description of the thing)
- Overview (a longer description)
- Who created this object
- When they created it
- Who modified it last
- When they modified it
- Who has the right to modify it
- Who has the right to view it
- Does it need approval?
- Has it been approved?
- If so, by whom and when?
- Iif so, under what section?

You won't be writing code to implement fancy permissioning so not all of this information will be useful in this problem set. However, it is good to have it in your data model even if you aren't going to build .tcl pages to update or query it. Notice that you may need to define extra tables to support some of this many-to-one information, e.g., the access control list for an object.

For elements that are unique to an object type (i.e., elements other than the standard ones listed above), we need to insert one row in a metadata table per element:

```plaintext
create table km_metadata_elements (
        metadata_id             integer primary key,
        table_name              not null references km_metadata_object_types,
        column_name             varchar(30) not null,
        pretty_name             varchar(100) not null,
        abstract_data_type      varchar(30) not null,  – ie. “text” or “shorttext” “boolean” “user”
 – this one is not null except when abstract_data_type is “user”
        oracle_data_type        varchar(30),   – “varchar(4000)”
        – e.g., “not null” or “check foobar in (‘christof’, ‘patrick’)”
        extra_sql               varchar(4000),
        – values are ‘text’, ‘textarea’, ‘select’, ‘radio’,
 – ‘selectmultiple’, ‘checkbox’, ‘checkboxmultiple’, ‘selectsql’
        presentation_type       varchar(100) not null,
        – e.g., for textarea, this would be “rows=6 cols=60”, for select, Tcl list,
        – for selectsql, an SQL query that returns N district values
        – for email addresses mailto:
        presentation_options    varchar(4000),
        – pretty_name is going to be the short prompt,
 – e.g., for an update page, but we also need something
 – longer if we have to walk the user through a long form
        entry_explanation       varchar(4000),
 – if they click for yet more help
        help_text               varchar(4000),
        – note that this does NOT translate into a “not null” constraint in Oracle
        – if we did this, it would prevent users from creating rows incrementaly
        mandatory_p             char(1) check (mandatory_p in (’t',‘f’)),
        – ordering in Oracle table creation, 0 would be on top, 1 underneath, etc.
        sort_key                integer,
        – ordering within a form, lower number = higher on page
        form_sort_key           integer,
        – if there are N forms, starting with 0, to define this object,
 – on which does this go?  (relevant for very complex objects where
 – you need more than one page to submit)
        form_number             integer,
        – for full text index
        include_in_ctx_index_p  char(1) check (include_in_ctx_index_p in (’t',‘f’)),
        – add forms should be prefilled with the default value
        default_value           varchar(200),
 check ((abstract_data_type not in (‘user’) and oracle_data_type is not null)
                or
              (abstract_data_type in (‘user’))),
        unique(table_name,column_name)
);
```

Notice that the km\_metadata\_elements table contains specifications for 1) generating the CREATE TABLE sql commands that you'll need to build the database data structures for storing knowledge and 2) building user interfaces to manipulate data in those tables.

If you have trouble feeding the above table definition to SQL\*Plus running under an Emacs shell, cut and paste it into a file called "foo.sql". Then run

```plaintext
 sqlplus student23/thepassword < foo.sql
```

from the shell.

### Exercise 1: Use Prototype Builder to Construct Admin Pages for your Metadata

Create a directory /admin/km/ under your Web server page root. Go to a Unix® shell and type

```plaintext
> > cd /web/yourservername/www/admin/
chmod a+w km
```

So that the /admin/km/ directory will be writable by the Web server. Now use the prototype builder (available on your own server at /admin/prototype/) to generate admin pages for the     
km\_metadata\_elements and km\_metadata\_object\_typestables.

### Exercise 2: Fill your Metadata Tables with Info

Fill your meta data tables with some info. Add one entry to the objects table for each of the object types listed above. For each object type, fill in some elements. Remember that each object will have the default fields specified above so you don't need things like name or overview. Here are some examples:

For the person type

date\_of\_birth, title

For the language type

syntax\_example, garbage\_collection\_p (whether the language has automatic storage allocation like Lisp or memory leaks like C), strongly\_typed\_p, lexical\_scoping\_p, date\_first\_used

For the publication type

this is for storing references to books and journal articles so you want all the fields that you'd expect to see when referencing something, include also an abstract field

For the data structure type

complexity\_for\_insertion, complexity\_for\_retrieval (varchars containing "O(1)", "O(N)", etc.)

For the system type

examples of systems are "Multics", "Xerox Alto", "TCP/IP", "MIT Lisp Machine", "Apple Macintosh", "World Wide Web". These are usually developed over a period of several years by an organization. date\_of\_conception, date\_of\_birth, organization\_name (use links to objects of type person in order to represent the prime developers of the system)

For the problem type

this would be for storing something like "traveling salesman" or "dining philosophers". Binary search". You need elements for pseudo\_code and high\_level\_explanation. In general, objects of type problem will be linked to objects of type algorithm (which algorithm solves the problem), publication (papers that set forth the problem), and person (people who were involved in stating or solving the problem)

For the algorithm type

this would be for storing something like "Quicksort" or "Binary search". You need elements for pseudo\_code and high\_level\_explanation. In general, objects of type algorithm will be linked to objects of type problem (what need the algorithm addresses), publication (papers that describe the algorithm or implementations of it) person (people who were involved in developing the algorithm)

### Exercise 3: Write a Program to Generate DDL Statements

Write a script called /admin/km/generate-ddl.tcl that will generate CREATE TABLE statements from the meta data tables. It can simply output this to the Web browser with a MIME type of text/plain (then you can save it to your local file system as km-generated.sql) or write it back into the Unix® file system as /admin/km/km-generated.sql (in which case you'll have to be sure that the /admin/km/ directory is writable by the Web server user, which is generally "nsadmin").

Each object table should have an object\_id column. Use a single Oracle sequence to generate keys for this column.

In addition to the metadata-driven object table definitions we'll define a generalized mapping table to support links between knowledge objects:

```plaintext
create table km_object_object_map (
 table_name_a  not null references km_metadata_object_types,
 – Assume all object type tables have integer primary keys.
        – For a generalized mapping table we can’t put an integrity
        – constraint on this field.
        table_id_a  integer not null,
        table_name_b  not null references km_metadata_object_types,
        table_id_b  integer not null,
        – User-entered reason for relating two objects, e.g.
        – to distinguish between John McCarthy the developer of
 – Lisp and Gerry Sussman and Guy Steele, who added lexical scoping
        – in the Scheme dialect
 map_comment  varchar(4000),
 creation_user  not null references users,
 creation_date  date default sysdate not null,
        primary key (table_name_a, table_id_a, table_name_b, table_id_b)
);
```

Notice that this table allows the users to map an object to any other object in the system, regardless of type.

For simplicity, assume that associations are bidirectional. If a user associates the Huffman encoding algorithm (used in virtually every compression scheme, including JPEG) with the person David A. Huffman (MIT Course VI grad student and then faculty member), we should also interpret that to mean that the person David A. Huffman is associated with the algorithm for Huffman encoding. This is why the columns in km\_object\_object\_map have names like "table\_name\_a" instead of "from\_table".

The primary key constraint above has the side effect of creating an index that makes it fast to ask the question "is object A related to object B?". For efficiency in querying "is object B related to object A?", create a concatenated index on the columns in the other order. (The trees chapter of [_SQL for Web Nerds_](http://philip.greenspun.com/sql/trees), gives some examples of concatenated indices. Also read the composite indices section of the Oracle Tuning manual. See also the Oracle SQL Reference section.)

### Exercise 4: Write a Program to Generate a "Drop All Tables" Script

Write a script called /admin/km/generate-drop-tables.tcl that will generate DROP TABLE statements from the meta data tables. You probably won't get your data model right the first time so you might as well be ready to clear out Oracle and start over.

### Exercise 5: Build the Knowledge Capture Pages

Create a directory /km under the Web server page root. The index.tcl page should display an unordered list of object types and, next to each type, options to "browse" or "create". You don't have any information in the database, so you should build /km/object-create.tcl first. This page will query the metadata tables to build a data entry form to create a single object of a particular type. Build /km/object-create-2.tcl to process the results of this form. You may find util\_prepare\_update from /tcl/00-ad-utilities.tcl useful in building object-create-2.tcl.

When object-create-2.tcl is done inserting the row into the database, it should ns\_returnredirect to object-display.tcl. This page should have small hyperlinks to edit single fields at a time (all linking to object-edit-element.tcl with different arguments). This page should show all the currently linked objects and have an "add link" hyperlink to object-add-link.tcl.

The page returned by object-add-link.tcl will look virtually identical to /km/index.tcl and will in fact link to the same URL: object-browse-one-type.tcl. When called with only table\_name, this page will display a table of object names with dimensional controls at the top. The dimensions should be "mine|everyone's" and "creation date". The user ought to be able to click on a table header and sort by that column.

When called with extra arguments, object-browse-one-type.tcl will pass those arguments through to object-view-one.tcl and, if the user clicks a confirmation button, will eventually result in object-add-link-2.tcl being invoked. The extra arguments should be table\_name\_a and table\_id\_a, which will ultimately end up in the corresponding columns of a row in km\_object\_object\_map.

### Exercise 6: Gather Statistics

You want to know when people are looking at and reusing knowledge. Create a table to hold object views:

```plaintext
-- we will be updating the reuse_p column of views so it 
-- will be easier to have a primary key 
create sequence km_object_view_id;
create table km_object_views (
 object_view_id integer primary key,
 – which user
 user_id  not null references users,
 – two columns to specify which object
 object_id integer not null,
 table_name varchar(21) not null,
 view_date date not null,
 reuse_p  char(1) default ‘f’ check(reuse_p in (’t',‘f’))  
);
```

Modify object-view-one.tcl so that you explicitly close the TCP connection to the user (using ns\_conn close). This will stop the Netscape icon to stop spinning but the AOLserver thread will remain alive so that you can log.

After the ns\_conn close, insert a row into the km\_object\_views table iff there isn't already a log row for this user/object pair within 24 hours. You could do this with a

1. Open a transaction
2. Lock the table
3. Count the number of matching rows within the last 24 hours
4. Compare the result to 0 and insert if necessary
5. Close the transaction

However, you can also do this with a single ns\_db dml statement. Here's an example of an INSERT statement that only has an effect if there isn't already a row in the table.

```plaintext
insert into msg_id_generator (last_msg_id)
select ('000000') from dual
where 0 = (select count(last_msg_id) from msg_id_generator);
```

Apply this idea to the problem of thread-safe logging if and only if there isn't an identical row logged within the last 24 hours.

[Date/Time Arithmetic](http://philip.greenspun.com/sql/dates)

### Exercise 7: Gather More Statistics

Modify object-view-one.tcl to add a "I reused this knowledge" button. This should link to object-mark-reused.tcl, a page that updates the reuse\_p flag of the most recent relevant row in km\_object\_views. The page should raise an error if it can't find a row to update.

### Exercise 8: Explain the Concurrency Problem in Exercise 7

Explain the concurrency problem in Exercise 7 and talk about ways to address it.

### Exercise 9: Do a Little Performance Tuning

Create an Oracle index on km\_object\_views that will make the code in exercises 6 and 7 go fast.

### Exercise 10: Display Statistics

Build /admin/km/statistics.tcl to show, by day, the number of objects viewed and reused. This report should be broken down by object type and all the statistics should be links to "drill-down" pages where the underlying data are exposed, e.g., which actual users viewed or reused knowledge and when.

### Exercise 11: Build a Site-wide Index

Build an index of the content in the KM system objects.

Create a file /admin/km/generate-sws-triggers.tcl to read the meta data tables and

1. Generate database triggers that will automatically put indexable object content into the site\_wide\_index table
2. Insert one row into sws\_table\_to\_section\_map for every object type defined in km\_metadata\_object\_types

Add some new objects and verify that they are being automatically copied by the triggers into the index table.

### Exercise 12: Query Pages for the Site-wide Index

Create a page /km/search.tcl that displays a form letting the user search for a phrase either through all objects or only in one type of object. For /km/search-2.tcl you'll either want to use Intermedia or the DBMS\_LOB functions and the pseudo\_contains source code.

## {{< anchor "5" >}}{{< /anchor >}}Optional Assignment

Show your system to a business school professor.

## Who Wrote This and When

This problem set was written by Philip Greenspun in October 1999. It is copyright 1999 by him but may be reused provided credit is given to the original author.