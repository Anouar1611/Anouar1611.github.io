---
title: Hibernate ?
author: Anouar Asmai
date: 2020-06-27 12:34:00 +0800
categories: [Blogging, Tutorial]
tags: [Hibernate]
---

# Introduction

---

 - **The issue :** When we work with an object-oriented system, there is a mismatch between the object model and the relational database. RDBMSs represent data in a tabular format whereas object-oriented languages, such as Java or C# represent it as an interconnected graph of objects.
 - **The solution :** The Object Relational Mapping 

> Now it's time for understanding the meaning of **ORM**

---


  **ORM** stands for Object-Relational Mapping (ORM) is a programming technique which automatically convert, on demand, the database into an object graph. The ORM relies for this on a configuration associating the classes of the functional model and the schema of the database. The ORM generates SQL queries which make it possible to materialize this graph or part of this graph as needed.Here is the following schema which describe this paragraph :
  
  ![orm]({{ "/assets/img/sample/mapping.png"}})
  

## Some solutions of ORM :

 - It reduces the amount of code that needs to be written and allows consistency with the rest of the code for object oriented languages.

 - An API to perform basic CRUD operations on objects of persistent classes.
 
 - A configurable facility for specifying mapping metadata.

There are several persistent frameworks and ORM options in Java. So, a persistent framework is an ORM service that provides some operations on objects into a relational database such as :

 - TopLink
 
 - **Hibernate**
 
 - iBATIS
 
 - Grails..
 
 ***We'll just discuss about Hibernate ***
 
 ---
 
 ## What is Hibernate ?
 
 ---

 Hibernate is an **O bject-R elational M apping (ORM)** solution for JAVA. It is a persistent open source framework created by Gavin King in 2001. It is a powerful and efficient object-relational request and persistence service for any Java application.Hibernate maps Java classes to database tables and Java data types to SQL data types and frees the developer from 95% of the programming tasks related to data persistence.It sits between traditional Java objects and database server to handle all the works in persisting those objects based on the appropriate O/R mechanisms and patterns.The follownig picture explain this :
 
  ![map]({{ "/assets/img/sample/orm-mapping.PNG"}})
 
 
> Note : Hibernate supports almost all the major RDBMS such as : MySQL , PostgreSQL , Oracle , Microsoft SQL Server Database..

---

## Hibernate Architecture :

---

Following is a detailed view of the Hibernate Application Architecture with its important core classes :

  ![archi]({{ "/assets/img/sample/Hibernate_Architecture.png"}})


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
      
## Hibernate configuration :

---

Hibernate requires to know in advance — where to find the mapping information that defines how your Java classes relate to the database tables. Hibernate also requires a set of configuration settings related to database and other related parameters. All such information is usually supplied as a standard Java properties file called **hibernate.properties**, or as an XML file named **hibernate.cfg.xml**.Let's talk about some of these properties :

 - **hibernate.dialect**

     This property makes Hibernate generate the appropriate SQL for the chosen database.There are a lot various important databases dialect property type such as : DB2 , MySQL , HSQLDB...


 - **hibernate.connection.driver_class**

      The JDBC driver class.
	
 - **hibernate.connection.url**

      The JDBC URL to the database instance.

 - **hibernate.connection.username**

      The database username.

 - **hibernate.connection.password**

      The database password.
      
      
 ## Hibernate - Sessions
 
 ---
 
  A Session is used to get a physical connection with a database. The Session object is lightweight and designed to be instantiated each time an interaction is needed with the database. Persistent objects are saved and retrieved through a Session object.

  The session objects should not be kept open for a long time because they are not usually thread safe and they should be created and destroyed them as needed. The main function of the Session is to offer, create, read, and delete operations for instances of mapped entity classes.

  Instances may exist in one of the following three states at a given point in time :

   - ***Transient :*** A new instance of a persistent class, which is not associated with a Session and has no representation in the database and no identifier value is considered transient by Hibernate.

   - ***Persistent :*** You can make a transient instance persistent by associating it with a Session. A persistent instance has a representation in the database, an identifier value and is associated with a Session.

   - ***Detached :*** Once we close the Hibernate Session, the persistent instance will become a detached instance.
   

## Simple Demo :

---

I will show a simple example with Spring and some operations for more understand Hibernate.

So , our dependencies are in the pom.xml :

**pom.xml**:

```xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>hilbernatedemo</artifactId>
    <version>1.0-SNAPSHOT</version>



    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <source>13</source> <!-- 1.8,1.9,1.10,11,12,13 -->
                    <target>13</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.annotation/javax.annotation-api -->
        <dependency>
            <groupId>javax.annotation</groupId>
            <artifactId>javax.annotation-api</artifactId>
            <version>1.3.2</version>
        </dependency>
       

        <!-- https://mvnrepository.com/artifact/org.springframework.data/spring-data-jpa -->
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-jpa</artifactId>
            <version>2.2.6.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.5</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.6.4</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.36</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-core -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>5.4.14.Final</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-tx -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.2.6.RELEASE</version>
        </dependency>

    </dependencies>
</project>
```

Then , here is the class of Hibernate's configuration :

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.orm.hibernate5.HibernateTransactionManager;
import org.springframework.orm.hibernate5.LocalSessionFactoryBean;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;
import java.util.Properties;

@Configuration
@EnableTransactionManagement
@ComponentScan("Models")
@ComponentScan("Dao")
public class HibernateConfig {

    @Bean
    public LocalSessionFactoryBean sessionFactory() {
        LocalSessionFactoryBean sessionFactory = new LocalSessionFactoryBean();
        sessionFactory.setDataSource(mysqlDataSource());
        sessionFactory.setPackagesToScan(new String[]{"Models"});
        sessionFactory.setHibernateProperties(hibernateProperties());

        return sessionFactory;
    }

    @Bean
    public DataSource mysqlDataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost/hibernateDB");
        dataSource.setUsername("root");


        return dataSource;
    }

    @Bean
    public PlatformTransactionManager hibernateTransactionManager() {
        HibernateTransactionManager transactionManager
                = new HibernateTransactionManager();
        transactionManager.setSessionFactory(sessionFactory().getObject());
        return transactionManager;
    }

    private final Properties hibernateProperties() {
        Properties hibernateProperties = new Properties();
        hibernateProperties.setProperty(
                "hibernate.hbm2ddl.auto", "create-drop");
        hibernateProperties.setProperty(
                "hibernate.dialect", "org.hibernate.dialect.MySQL5InnoDBDialect");

        return hibernateProperties;
    }
}
```
> Note : This class is the same of **hibernate.cfg.xml** file 

After the configuration of Hibernate , it's the turn of our models :

```java

package Models;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Component
@Entity
public class App {
    @Id@GeneratedValue(strategy = GenerationType.AUTO)
    private int AppID;
    private String AppName;
    @ManyToOne
    @Autowired
    private User user;


    public User getUser() {
        return user;
    }

    public void setUser(User user) {
        this.user = user;
    }

    public int getAppID() {
        return AppID;
    }

    public App() {
    }

    public void setAppID(int appID) {
        AppID = appID;
    }

    public String getAppName() {
        return AppName;
    }

    public void setAppName(String appName) {
        AppName = appName;
    }
}
```

```java

package Models;

import org.springframework.stereotype.Component;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Component
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private int ID;
    private String Name;

    @OneToMany
    List<App> app = new ArrayList<App>();

    public User(){

    }

    public User(String name, List<App> app) {
        Name = name;
        this.app = app;
    }

    public int getID() {
        return ID;
    }

    public void setID(int ID) {
        this.ID = ID;
    }

    public String getName() {
        return Name;
    }

    public void setName(String name) {
        Name = name;
    }

    public List<App> getApp() {
        return app;
    }

    public void setApp(List<App> app) {
        this.app = app;
    }
}
```

Here , we have a OneToMany relation between the two models 
Next step is , some CRUD opertaions that Hibernate provides :

```java
package Dao;

import Models.App;
import Models.User;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import java.util.Collection;

@Repository
public class DaoImpl {
    @Autowired
    private SessionFactory sessionFactory;
    public void saveUser(User user, String name){
        user.setName(name);
        Session session = sessionFactory.openSession();
        session.beginTransaction();
        session.save(user);
        session.getTransaction().commit();
    }

    public void saveUserListApp(User user,App app1, App app2, String nameUser,String nameApp1,String nameApp2){
        app1.setAppName(nameApp1);
        app2.setAppName(nameApp2);
        user.getApp().add(app1);
        user.getApp().add(app2);
        user.setName(nameUser);
        Session session = sessionFactory.openSession();
        session.beginTransaction();
        session.save(user);
        session.save(app1);
        session.save(app2);
        session.getTransaction().commit();
    }

    public void saveUserWithCascade(User user1,App app1, App app2, String nameUser1,String nameApp1,String nameApp2){
        user1.getApp().add(app1);
        user1.getApp().add(app2);
        app1.setAppName(nameApp1);
        app2.setAppName(nameApp2);
        user1.setName(nameUser1);
        Session session = sessionFactory.openSession();
        session.beginTransaction();
        session.persist(user1);
        session.getTransaction().commit();

    }

    public User getUserById(int Id){
        Session session = sessionFactory.openSession();
        session.beginTransaction();
        User user = session.get(User.class,Id);
        session.getTransaction().commit();
        session.close();
        return user;
    }

    public void deleteUserById(User user,int Id){
        Session session = sessionFactory.openSession();
        session.beginTransaction();
        user = session.get(User.class,Id);
        session.delete(user);
        session.getTransaction().commit();
        session.close();
        System.out.println("Succesfully deleted");
    }


}
```

The @Repositry means that told the container this component is the responsible of some operations , we'll discuss about the annotaions in java
in the near futur

Finally , our main for testing :

```java

  
import Dao.DaoImpl;
import Models.App;
import Models.User;
import org.hibernate.SessionFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;


public class MainApp {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(HibernateConfig.class);
        SessionFactory sessionFactory =  ctx.getBean("sessionFactory",SessionFactory.class);
        User user = ctx.getBean("user",User.class);
        DaoImpl daoImpl = ctx.getBean("daoImpl",DaoImpl.class);
        App app = ctx.getBean("app",App.class);
        App app1 = new App();
        daoImpl.saveUser(user1,"Simo");
        app1.setUser(user);
        daoImpl.saveUserListApp(user,app,app1,"Anouar","Instagram","Telegram");

    }
}
```

## Conclusion :

---

This is a quick guide and blog for Hibernate , I can't notice all the termonologies of this framework because it's too much but it helps for more understanding this famous ORM framework , Enjoy 👍👍
