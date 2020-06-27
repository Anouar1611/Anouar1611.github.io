---
title: Hibernate ?
author: Anouar Asmai
date: 2020-06-27 12:34:00 +0800
categories: [Blogging, Tutorial]
tags: [Hibernate]
---

# Introduction

---

--> **The issue :** When we work with an object-oriented system, there is a mismatch between the object model and the relational database. RDBMSs represent data in a tabular format whereas object-oriented languages, such as Java or C# represent it as an interconnected graph of objects.
--> **The solution :** The Object Relational Mapping 

> Now it's time for understanding the meaning of **ORM**

---


  **ORM** stands for Object-Relational Mapping (ORM) is a programming technique which automatically convert, on demand, the database into an object graph. The ORM relies for this on a configuration associating the classes of the functional model and the schema of the database. The ORM generates SQL queries which make it possible to materialize this graph or part of this graph as needed.Here is the following schema which describe this paragraph :
  
  ![Dep]({{ "/assets/img/sample/dep.jpeg"}})
  

## Some solutions of ORM :

 - It reduces the amount of code that needs to be written and allows consistency with the rest of the code for object oriented languages.

 - An API to perform basic CRUD operations on objects of persistent classes.
 
 - A configurable facility for specifying mapping metadata.

There are several persistent frameworks and ORM options in Java. So, a persistent framework is an ORM service that provides some operations on objects into a relational database such as :

 - TopLink
 
 - **Hibernate**
 
 - iBATIS
 
 - Grails..
 
 ***We'll just discuss about Hibernate 
 
 ## What is Hibernate ?
 
 ---

 Hibernate is an **O bject-R elational M apping (ORM)** solution for JAVA. It is a persistent open source framework created by Gavin King in 2001. It is a powerful and efficient object-relational request and persistence service for any Java application.Hibernate maps Java classes to database tables and Java data types to SQL data types and frees the developer from 95% of the programming tasks related to data persistence.It sits between traditional Java objects and database server to handle all the works in persisting those objects based on the appropriate O/R mechanisms and patterns.The follownig picture explain this :
 
 
> Note : Hibernate supports almost all the major RDBMS such as : MySQL , PostgreSQL , Oracle , Microsoft SQL Server Database..

---

## Hibernate Architecture :

---

Following is a detailed view of the Hibernate Application Architecture with its important core classes :



As you see , Hibernate framework uses many objects session factory, session, transaction etc. alongwith existing Java API such as JDBC (Java Database Connectivity), JTA (Java Transaction API) and JNDI (Java Naming Directory Interface).

### Elements of Hibernate Architecture :

---

For creating the first hibernate application, we must know the elements of Hibernate architecture. They are as follows:

 - **Configuration :**
 
     The Configuration object is the first Hibernate object you create in any Hibernate application and usually created only once during application initialization. It represents a configuration or properties file required by the Hibernate. The Configuration object provides two keys components:
     - ***Database Connection:*** This is handled through one or more configuration files supported by Hibernate. These files are hibernate.properties and hibernate.cfg.xml.
     - ***Class Mapping Setup:*** This component creates the connection between the Java classes and database tables..
     
  - **SessionFactory :**
  
      The SessionFactory is a factory of session and client of ConnectionProvider. It holds second level cache (optional) of data. The org.hibernate.SessionFactory interface provides factory method to get the object of Session.
      The SessionFactory is heavyweight object so usually it is created during application start up and kept for later use. You would need one SessionFactory object per database using a separate configuration file. So if you are using multiple databases then you would have to create multiple SessionFactory objects. 
      
   - **Session :**
   
      The session object provides an interface between the application and data stored in the database. It is a short-lived object and wraps the JDBC connection. It is factory of Transaction, Query and Criteria. It holds a first-level cache (mandatory) of data. The org.hibernate.Session interface provides methods to insert, update and delete the object. It also provides factory methods for Transaction, Query and Criteria.
      The session objects should not be kept open for a long time because they are not usually thread safe and they should be created and destroyed them as needed.
   - **Transaction :**
   
      The transaction object specifies the atomic unit of work. It is optional. The org.hibernate.Transaction interface provides methods for transaction management.
      This is an optional object and Hibernate applications may choose not to use this interface, instead managing transactions in their own application code.
        
   - **ConnectionProvider :**
   
  It is a factory of JDBC connections. It abstracts the application from DriverManager or DataSource.Hibernate Connection management service provide efficient management of the database connections. Database connection is the most expensive part of interacting with the database as it requires a lot of resources of open and close the database connection.
  
   - **TransactionFactory :**
   
      It is a factory of Transaction.
      
   - **Query :**
   
      Query objects use SQL or Hibernate Query Language (HQL) string to retrieve data from the database and create objects. A Query instance is used to bind query parameters, limit the number of results returned by the query, and finally to execute the query.
      
   - **Criteria :**
   
      Criteria object are used to create and execute object oriented criteria queries to retrieve objects.Hibernate provides a lot of flexibility in use. It is called "Lite" architecture when we only uses the object relational mapping component. While in "Full Cream" architecture all the three component Object Relational mapping, Connection Management and Transaction Management) are used.
