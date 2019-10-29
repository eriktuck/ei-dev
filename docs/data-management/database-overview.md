Do you (or your client) have data stored across so many files you sometimes can't remember where they are? Are you scrambling to pull together information when requested, constantly months behind in reporting, or finding yourself dumping data from multiple sources into a single spreadsheet for analysis? If so, it might be time for a database.

This tutorial will 

1. [introduce you to databases](#introduction to databases),
2. [describe available database solutions](#database-options), and
3. [help you select the right database for your needs](#how-to-choose?). 

## Introduction to Databases

A database is simply an organized store for information. Data are stored in tables and relationships are defined between tables (hence the name, 'relational database'). For example, a list of employees and their position titles may be stored in one table while position titles and their respective salaries are stored in another. Each employee's position title can be used to lookup the correct salary. Storing this information in separate tables avoids unnecessary duplication, which reduces the possibility of error.

How the data are stored is often less important than how the data are accessed (which is why we are often hesitant to set up a database in the first place). When choosing and designing a database, you'll need to pay as much attention to how the user will interface with the database as the database solution itself.  

### Interacting with Databases

Most users will interact with a database through a navigation form. The navigation form surfaces the important functionality of the database, including entering data, querying data, and generating reports. 

Here's an example navigation form for a bakery's order database created in Microsoft Access. 

![navigation-form](assets/navigation-form.gif)



The navigation form is set up after the database is configured by the database administrator (DBA; this may be you, someone on the Metrics Service Line, or an outside contractor). The navigation for, as well as other forms, queries and reports that are made available to the user by the DBA, make the data in the database accessible to users while protecting the integrity of the database 

#### Forms

Data is entered into the database primarily through forms. Forms can be made available through the Navigation Form or through individual forms designed and made available by the DBA.

#### Queries

Queries allow you to search for and update records in the database. Want to know how many orders were placed yesterday? How many blueberry muffins you've sold this year? You'll need a query for that.

#### Reports

Reports are queries or collections of queries that can be batched together and run periodically. You might have a standard report that is produced each quarter or a ready-made report to provide real time information to your boss when requested. Reports must also be set up and made available by the DBA.

## Database Options

There are many choices when it comes to databases, each with its own advantages and disadvantages.

### Desktop Software

**Microsoft Access** and **Open Office Base** are great options. Open Office Base is free but has fewer features and less support than Microsoft Access. **Microsoft Excel** can even serve as a database, in a pinch.

### Open Source Database Software

**PostgreSQL**, **MySQL**, and **MariaDB** are examples of open source (free) database solutions. While these are robust, scalable database solutions used by many large corporations (e.g., Netflix), they are not user friendly out of the box. Often, these open source solutions are paired with a web-based interface. If you are working with a web developer and have the budget for them to design the forms, queries and reports you need you'll likely choose one of these open source options. 

> Project Spotlight: See the [Registry]( http://monarchhabitatexchange.org/projects ) for the Monarch Butterfly Habitat Exchange.

### Commercial Database Solutions

Microsoft, IBM, and Oracle all sell enterprise database solutions. It's very unlikely we'll require the features and support of one of these databases.

### Alternative Platforms

The need for data storage and analytics solutions has led to a boom in alternative solutions like **AirTable**, **Zoho**, **Notion**, **QuickBase** and others. These platforms are feature-rich and intended for non-technical audiences to make data analysis more accessible. **Salesforce** and other CRMs (Customer Relationship Management software) also combine data storage and analytics in a way that could serve as a solution for your needs. In addition, "low-code" application platforms like **Appian** and **Microsoft PowerApps** allow you to develop custom solutions without knowing how to code. 

> If you're simply looking for data analysis and visualization (think: Tableau), check out our *Introduction to Data Visualization*.

## How to Choose?

There are many options when it comes to choosing a database. This section will help you pick the right one. 

### Budget

Consider both the cost of the software itself and the cost of designing and maintaining the database.

### Scalability

Unless you're dealing with \*Big Data*, you won't need to worry about the database growing too large to fit on a single server. The only likely exception would be if attempting to store geospatial data within a relational database outside of ArcGIS.

### Hosting & Integration

If the database will be hosted and managed by the client or their website manager, your options will likely be limited to those that integrate with their existing systems. Work with them when selecting a database and during database design.

In most other cases, the database will live on the client's local drive or server (unless you picked a cloud-based database alternative). Most database solutions can be run locally or on a remote server.

### Maintenance & Support

Who will maintain the database over time? If it's the client, make sure they are comfortable with the solution. Who will provide support if something crashes or the client needs a new type of report developed? If it's you, make sure *you* are comfortable with the solution. Keep in mind that paying a contractor to set up a cool web-based database will likely require paying a contractor to maintain that database periodically and as needed. 

## Final Thoughts

Setting up a database is an important step in the maturation of any data-driven program. The right database solution will keep your data safe and secure while also providing access to the data for analysis and reporting. When choosing a database, the best approach is to keep it simple. 

While you may not be responsible for choosing or building the database, you should be involved in the design of the database. Continue to the next section, *Database Design*, for more.