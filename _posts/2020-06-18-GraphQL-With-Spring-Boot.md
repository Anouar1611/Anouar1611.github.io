---
title: GraphQL with Spring Boot
author: Anouar Asmai
date: 2020-06-18 16:33:00 +0800
categories: [Blogging, Demo]
tags: [GraphQL]
---

![Dep]({{ "/assets/img/sample/graphQL.png"}})

# Introduction 

---

  Before starting our blog , I should note that the REST web services are one of the most popular architectures to expose data from the backend.But , with a REST API, you would typically gather the data by accessing multiple endpoints .So , the question is, "Is GraphQL better than REST?" 😳 . 
  
***Let's see what the blog will show us !!***

  
# What Is GraphQL?
 
 ---
 
  GraphQL is a query language for APIs that allows clients to request limited data they need : the clients will get data in a limited number of requests .It gives control to the user to query what he needs .Therefore , it reducing the payload on the network .

  There are some termonologies of GraphQL to know :

   - ***Schema :*** The contract between the GraphQL client and the GraphQL server
   - ***Query :*** it's Similar to GET call in REST and used by the client to get the fields
   - ***Mutations:*** It's similar to a POST/PUT call in REST and is used by the client for any insert/update operation
   
   
  
 
# Why GraphQL ?
 
 ---
 
 If you do not know about GraphQL then you are in the right place.GraphQL was developed to cope with the need for more flexibility and efficiency! It solves many of the shortcomings and inefficiencies that developers experience when interacting with REST APIs.Also , thanks to the flexible nature of GraphQL, changes on the client-side can be made without any extra work on the server. Since clients can specify their exact data requirements, no backend engineer needs to make adjustments when the design and data needs on the frontend change.
 
> Let's see some differences between REST and GraphQL

 
# REST vs GraphQL
 
 ---
 
 - **Network Performance:** No need to create multiple API (Application Programming Interface) endpoints in an application unlike in REST where we expose multiple endpoints to retrieve data like this, GraphQL provides a mechanism to specify the fields of interest in response. This reduces network overload by fetching the smallest data possible.
 - **Endpoint:** In REST, each resource is identified by a URI. This makes the client obliged to know each endpoint. In GraphQL, all resources are identified by a single endpoint. There is no hassle of maintaining multiple URI’s.
 - **Data Fetching Strategy:** In GraphQL, we have only one endpoint. The client sends a single query with the required fields. This helps with improved network performance and avoids the over-fetching/under-fetching data problem.
 

***Let's take a little look for some disavantages of GraphQL .***


# GraphQL disadvantages :

---

I'll not explain each point in detail , but it's a little look of some cons of GraphQL :

 - Performance issues with complex queries
 
 - Web caching complexity
 
 - Takes a while to understand.
 
 - Retrieving objects recursively is not supported in GraphQL
 
 
# GraphQL Architecture

In general , this picture describe the  architecture of GraphQL :

![Dep]({{ "/assets/img/sample/GraphQL_Architecture-1024x461.png"}})
 
 Briefly ,in this section, we’ll walk you through 3 different kinds of architectures that include a GraphQL server:

   - GraphQL server ***with a connected database***
   
![gql]({{ "/assets/img/sample/graphql-architecture.png"}})

   - GraphQL server that ***integrates existing systems***
   
![gplq]({{ "/assets/img/sample/graphql-architecture2.png"}})

   - ***A hybrid approach of a connected database and third party or legacy systems*** that can all be accessed through the same GraphQL API
   
![gql1]({{ "/assets/img/sample/graphql-architecture3.png"}})
   
   
# Schema Definition Language (SDL)
  
  ---
  
  One very important property of GraphQL is that it is statically typed: the server knows exactly the shape of every object you can query and any client can actually “introspect” the server and ask for the so called “schema”. The schema describes what queries are possible and what fields you can get back. (Note: when we refer to schema here, we always refer to a “GraphQL Schema”, which is not related to other schemas like “JSON Schema” or “Database Schema”).It's a contract between client and server. In the schema, we define all the fields and functionalities exposed by APIs.
   
   Here is an example of how we can use the SDL to define a simple type called `User` :
   
 ```java
 
 type User {
          Id: ID!,
          name: String,
          age: Int!,
          mail: String!,
          apps: [App!]!
}

type App {
         name: String!,
         user: User!
}

type Query {
          users(name:String): [User!]!
          user(id :ID): User!
}

type Mutation {
  createUser(name: String!, age: Int!): User!
}

type Subscription {
  newUser: User!
}

```

 - Note that we just created a ***one-to-many-relationship*** between User and App since the apps field on User is actually an array of apps.The `!` following the type means that this field is required.


# Fetching Data with Queries :

  In this example , the GraphQL query contains the root field i.e. Users and query’s payload i.e. name and age.This query returns the user’s name and their age whose name matches to Anouar.
  
  
 ```java
query{
       users(name:”Anouar”)  {
                        name
                        age
       }
}
```
The output is something like this :

        {
          "users": [
              { 
                "name": "Anouar",
                "age": 22
              }
          ]
        }
        
# Mutations

---

  This is similar to a REST POST method. If we need an API to create/modify data, then we can go for mutations. Possible mutations are:

   - Creating new data
   - Modifying existing data
   
   
Mutation syntax starts with the keyword mutation. The below mutation creates a student, and the server returns the unique ID of the inserted record.

Example:

```java

mutation {
  createUser(name: "Jack", age: 36) {
      name
      age
  }
}
```


# Resolvers GraphQL :

---

  Resolvers are functions written in the GraphQL server corresponding to each field in GraphQL query/mutations. When a request comes to the server, it will invoke resolvers’ functions corresponding to fields mentioned in the query.
  
 
# GraphQL With Spring Boot :
 
 ---
 
 let’s go over the details on implementing GraphQL with Spring Boot.
 
 **POM.xml** :
 
 ```xml

<dependency>     
    <groupId>com.graphql-java-kickstart</groupId>
    <artifactId>graphql-spring-boot-starter</artifactId>
    <version>5.11.1</version>
</dependency> 
<dependency>     
   <groupId>com.graphql-java-kickstart</groupId>
    <artifactId>graphql-java-tools</artifactId>     
    <version>5.7.1</version> 
</dependency>

```

***Now , Spring Boot downloads the necessary GraphQL handlers to parse the schema, and GraphQL API’s are exposed at the endpoint /graphql.***

## Schema and Resolvers :

---

   Create a graphql folder under ***src/main/resources***, and create a ***userql.graphqls*** file under that folder. Copy the above contents and paste it in the ***userql.graphqls*** file. Note that the name of the file could be any name of your choice. Just make sure to keep the file extension as .graphqls. We can have as many schema files as we require. Each type defined in the schema must have field resolvers. **FieldResolverError results if the GraphQL server cannot find the resolver for any given field**.
   
   So , Spring bean must implement GraphQL **Query Resolver**.Query resolver for Users may look like:
   
 ```java
import com.coxautodev.graphql.tools.GraphQLQueryResolver;
import com.sample.graphql.modal.User;
import com.sample.graphql.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class UserQueryResolver implements GraphQLQueryResolver
{
    @Autowired
    private UserService userService;
    public User user (String name)
    {
        return userService.getUserDetailsByName(name);
    }
}
```

Then , mutation resolver to create User looks like :

```java
import com.coxautodev.graphql.tools.GraphQLMutationResolver;
import com.sample.graphql.modal.User;
import com.sample.graphql.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class UserMutationResolver implements GraphQLMutationResolver
{
    @Autowired
    private UserService userService;
    public User createUser (User user) {
        return userService.createUserRecord(user);
    }
}
```

### Root Query

---

  Query or Mutation objects are root GraphQL objects. They don’t have any associated data class. In such cases, the resolver classes would implement GraphQLQueryResolver or GraphQLMutationResolver. These resolvers will be searched for methods that map to fields in their respective root types.
  
**Now your GraphQL server is ready. You can test your API’s using Postman or the GraphiQL UI tool.**

> We looked at a basic GraphQL Java example with Spring Boot !!


# Conclusion :

---

  We cannot say that the GraphQL is always the best choice or the alternative of REST web services , both have advantages and disadvantages , you have to choose according to your need .
  I hope this blog may help you to more understanding or discovring the GraphQL query language , don't forget to share it . Enjoy ! 😃😃


