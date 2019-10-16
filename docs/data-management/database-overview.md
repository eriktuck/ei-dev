Learning Objectives

* Be able to recommend a database technology for the SETT
* Understand the functionality of the selected database technology

Goal: Demystify the database and its uses to encourage faster adoption of database tech in our projects

Objectives

* Illustrate what a database looks like from the user perspective
* Clarify the process and level of effort for developing a database
* Link to more advanced topics for technical staff

Audience: All EI staff



Content

* Intro
  * When to use a database
* Database functionality
  * Navigation View
  * Data Entry
  * Tables
  * Queries (search/replace)
  * Reports
  * Forms
* Considerations when selecting a database
  * Free vs Paid (e.g. Salesforce)
  * CAP theorem
  * SQL vs NoSQL
  * Keep it simple
  * Updates/Releases
  * Support
  * Lifetime
* Types of databases
* How to create a database
  * Schema design
    * ACID
  * Setting up the database
  * Populating the database
    * Importing/Exporting Data
  * Distributing the database
  * Managing the database
    * DBA vs User
  * Transitions
    * Porting to the web
* Advanced Topics
  * SQL language basics
    * Joins (inner, outer)
  * Advanced Interface Design (esp. for Postgres, MySQL)
* Cloud Analytics Platforms
  * [Zoho, Domo]( https://www.techradar.com/news/best-cloud-analytics )
* Non-traditional options
  * Airtable
* Terminology



Resources

* [Intro to Access Databases (YouTube: Design & Deploy)]( https://www.youtube.com/playlist?list=PL4UezTfGBADBmCOYtQ8QohflQNY1y3oE7 )
* [Udemy Excerpt on Database Selection (YouTube: Frank Kane)]( https://www.youtube.com/watch?v=v5e_PasMdXc)
* [How to Choose Relational Databases (YouTube: Socratica)](https://www.youtube.com/watch?v=WzfDLqt-WIg)
* [Postgres Installation & Overview (YouTube: Socratica)](https://www.youtube.com/watch?v=fZQI7nBu32M)
* Monarch - Database Options Memo (in Downloads)

Assets

* [xcld bobby tables cartoon](https://www.google.com/url?sa=i&source=images&cd=&ved=2ahUKEwi219Kwgp_lAhU4ITQIHctfBSMQjRx6BAgBEAQ&url=https%3A%2F%2Fxkcd.com%2F327%2F&psig=AOvVaw08GxKbuhIlpoZqDFcB99A4&ust=1571254745719498)
* [xlcd data pipeline]( https://xkcd.com/2054/ )
* [xlcd hard problems with algorithms]( http://twcmattress.com/2017/05/xkcd-com-here-to-help/ )
* [xKcd curve fitting]( https://xkcd.com/2048/ )
*  

To Do

- [ ] Better example for database need in Introduction to Databases
- [ ] Asset: linked tables example

Decision Tree Concept

1. Where will the database live?
   1. Website hosted by the client (2)
   2. On the client's local server
   
2. Are we talking about a Squarespace they update occasionally or a managed website?
   1. Full stack web developers in the mix (3)
   2. Actually, its a Wordpress site...
   
3. Work with the client's web provider to design the schema, they'll do the rest

4. What kind of budget are we talking?
   1. Just inked a deal with the Bill & Melinda Gates Foundation (5)
   2. eh (6)
   
5. Consider hiring a provider to set up the database and offer ongoing support

6. You probably don't have budget for a publicly available database. Consider a local option instead (7)

7. How much data are we talking?

   1. Seems like a lot but its manageable (8)
   2. We make Facebook look like punks (11)

8. Can we create the data analyses?

   1. Yes (9)
   2. No (10)

9. MS Access or OO Base

10. Salesforce, AirTable, Zoho, Domo, Notion

11. You'll want professional help.

    

---

Do you (or your client) have data stored across so many files you sometimes can't remember where it is? Are you scrambling to pull together information when requested, constantly months behind in reporting, or finding yourself dumping data from multiple sources into a single spreadsheet for analysis? If so, it might be time for a database.

This tutorial will 

1. [introduce you to databases](#introduction to databases),
2. help you select the right database for your needs, and 
3. describe the process for developing and managing a database. 

## Introduction to Databases

A database is simply an organized store for information. Data are stored in tables and relationships are defined between tables (hence the name, 'relational database'). 

Imagine you have a list of employees, their position titles, and their salaries (which are based on position). You can create one table that includes the employee information and their position titles, and another table that stores the salary associated with each position. You can see the salary for each employee by 'joining' the tables based on the position title. Now imagine you want to give everyone a cost-of-living adjustment. Simply update the salaries in the salary table and the change will automatically be carried over to each employee based on their position title. In this way, you don't need to edit salary information that is repeated, for example with multiple employees of the same position. 

A trivial example for sure, but imagine instead you have hundreds or thousands of employees. You certainly wouldn't want to manually change the salary information for every employee!

[ASSET: ILLUSTRATE TABLE LINKAGES] 

### Interacting with Databases

So what's a database even look like? Most users will interact with a database through a navigation form. The navigation form surfaces the important functionality of the database, including entering data, querying data, and generating reports. 

Here's an example navigation form for a bakery's order database created in Microsoft Access. 

![navigation-form](assets/navigation-form.gif)


The navigation form is set up after the database is configured by the database administrator (DBA; this may be you, someone on the Metrics Service Line, or an outside contractor). The navigation form (as well as other forms, queries and reports that are made available to the user by the DBA) make the data in the database accessible to users while protecting the integrity of the database 

#### Forms

Data is entered into the database primarily through forms. Forms can be made available through the Navigation Form or through individual forms designed and made available by the DBA.

#### Queries

Queries allow you to search for and update records in the database. Want to know how many orders were placed yesterday? How many blueberry muffins you've sold this year? You'll need a query for that.

#### Reports

Reports are queries or collections of queries that can be batched together and run periodically. You might have a standard report that is produced each quarter or a ready-made report to provide real time information to your boss when requested. Reports must also be set up and made available by the DBA.

---

> ### More than Just a Database

> Before we go any further...

---

## How to Choose a Database 

There are many options when it comes to choosing a database. This section will help you pick the right one. Here are a few considerations.

#### Budget

Consider both the cost of the software and the cost of designing and maintaining the database. 

Many database options are available at no cost as open source software (e.g., MySQL, PostgreSQL, MariaDB). While these are robust, scalable database solutions used by many international corporations, they are not user friendly out of the box. Often, these open source solutions are paired with a web-based interface. If you are working with a web developer and have the budget for them to design the forms, queries and reports you need--you'll likely choose one of these open source options. 

If not, you may need a different solution, like Microsoft Access or even Salesforce. These may require a license or subscription but will allow you to query data and create reports without hiring specialized expertise.

#### Scalability

For most of our applications, you won't need to worry about the database growing too large to fit on a single server. The only likely exception would be if attempting to store geospatial data within a relational database outside of ArcGIS. Try not to do that. If you are worried about scalability and think you need a cloud-based solution, consult an expert.

#### Hosting & Integration

If the database will be hosted and managed by the client or their website manager, your options will likely be limited to those that integrate with their existing systems. Work with them when selecting a database and during database design.

In most other cases, the database will live on the client's local drive or server. Most database solutions can be run locally or on a remote server.

#### Support

What would you do if your database crashes at 2am? Run into the office and call technical support? Or keep snoozing? Enterprise solutions provided by Oracle and IBM are available if the thought of a database crash keeps you up at night. For the rest of us, 24-hour on-call support just isn't necessary. Some open source options have companies behind them that sell support if you need it. Some are only supported by their user community. 

Also consider that 