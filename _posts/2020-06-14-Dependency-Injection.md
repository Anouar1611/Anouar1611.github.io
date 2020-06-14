---
title: What is Dependency Injection DI ?
author: Anouar Asmai
date: 2020-06-14 12:34:00 +0800
categories: [Blogging, Tutorial]
tags: [DI]
toc: false
---

![Dep]({{ "/assets/img/sample/dep.png"}})

---

# Introduction

   *What is DI ?* : 
   
   Dependency Injection (DI) is a design pattern used to implement IoC. It allows the creation of dependent objects outside of a class and provides those objects to a class through different ways. Using DI, we move the creation and binding of the dependent objects outside of the class that depends on them.
 
 ***
 
> It's not a problem if you don't understand the definition completely , let's understand this concept more !!

***

# Overview :

  DI can be implemented in any programming language. The general concept behind dependency injection is called Inversion of Control.
A Java class has a dependency on another class, if it uses an instance of this class. We call this a class dependency. For example, a class which accesses a logger service has a dependency on this service class.
Ideally Java classes should be as independent as possible from other Java classes. This increases the possibility of reusing these classes and to be able to test them independently from other classes.
If the Java class creates an instance of another class via the `new` operator, it cannot be used (and tested) independently from this class and this is called a hard dependency.
 
 
***
 
So , The Dependency Injection pattern involves 3 types of classes :

 -***Client Class***: The client class (dependent class) is a class which depends on the service class

 -***Service Class***: The service class (dependency) is a class that provides service to the client class.

 -***Injector Class***: The injector class injects the service class object into the client class.

***

The following figure illustrates the relationship between these classes:

![DI]({{ "/assets/img/sample/DI.png"}})

As you can see, the injector class creates an object of the service class, and injects that object to a client object. In this way, the DI pattern separates the responsibility of creating an object of the service class out of the client class. Inversion of Control can be achieved through various mechanisms such as: Strategy design pattern, Service Locator pattern, Factory pattern, and Dependency Injection (DI).

***


# What Is Inversion of Control?

 Inversion of Control is a principle in software engineering by which the control of objects or portions of a program is transferred to a container or framework. It's most often used in the context of object-oriented programming.Inversion of Control can be achieved through various mechanisms such as: Strategy design pattern, Service Locator pattern, Factory pattern, and Dependency Injection (DI).
 
 ***
 
 # Simple Example of DI :
 
 Here's how you would create an object dependency in traditional programming :
 
 ```java
 
   import java.util.logging.Logger;

   public class MyClass {

      private Logger logger;

      public MyClass() {
          logger = new Logger;
          logger.info("This is a log message.")
      }
    }
  ```
 
So , in this example , we need to instanciate the logger object within the MyClass constructor class.
But, by using DI, we can rewrite the example without using the `new` operator into MyClass constructor :


```java

  import java.util.logging.Logger;

  public class MyClass {

      private Logger logger;

      public MyClass(Logger logger) {
          this.logger = logger;
          logger.info("This is a log message.")
      }
  }
```


 
> Let's see now what is Coupling in Java and what is the difference between Tightly Coupled Objects and Loosely Coupled Objects


 Coupling in Java plays an important role when you work with Java Classes and Objects. It basically refers to the extent of knowledge one class knows about the other class.
 
 
 
# Types of Coupling

Coupling in Java is divided into two types, namely:

    - Tight coupling
    - Loose coupling
    
*Let’s understand each one of them.*


 -***Tight coupling :*** means classes and objects are dependent on one another.
 
 -***Loose coupling :*** means reducing the dependencies of a class that uses the different classes directly

For example :

```java

  public class Car {//Tightly Coupled
  
     private String wheel;
  }
  public class Car {//Loosely Coupled
  
     private Wheel[] wheel;   
  }
  
```


 So, the `first` Car class and Wheel class are an example for a ***tightly coupled objects*** , it means that if you want to buy a Car which you couldn’t change the wheels because when the car was created , the wheel was created because it's a part of the car , then if you wanted to change the wheel , you must buy a new car , which not flexible and it took more effort. In the other side , the `second` Car class and Wheel class are ***loosely coupled object*** , it means the objects and its dependencies are loosely coupled because you can change one of them without changing the other , which means the loose coupling means that the objects are independent . With this type of coupling we can achieve our **Dependency Injection** .
 
 
  > ***As a conclusion,*** tightly coupled code maintenance takes time and huge efforts which a loosely coupled code don't.
  
  
  ***
  
  
  
  # Advantages of Dependency Injection :
  
  
   - DI allows a client the flexibility of being configurable. Only client's behavior is fixed.
   
   - DI decreases coupling between a class and its dependency.
   
   - Reduced module complexity
   
   - Testing can be performed using mock objects.
   
   - Increased module reusability.
   
  ***
   
   # Types of Dependency Injection :
   

   As you have seen above, the injector class injects the service (dependency) to the client (dependent). The injector class injects dependencies broadly in three ways: through a constructor, through a property, or through a method.

   - ***Constructor Injection :*** In the constructor injection, the injector supplies the service (dependency) through the client class constructor.

   - ***Property Injection :*** In the property injection (aka the Setter Injection), the injector supplies the dependency through a public property of the client class.

   - ***Method Injection :*** In this type of injection, the client class implements an interface which declares the method(s) to supply the dependency and the injector uses this interface to supply the dependency to the client class.
   
***

 # Conclusion :
 
  `Dependency injection` can be useful when working with large applications because it relieves various code modules from the task of instantiating references to resources and allows dependencies , wa can also notice that DI is a powerful concept to create loosely couled objects , and Your code will be clean and more readable.
  
  I hope this blog was more usefull and you have more information after reading it !! Don't forget to share it , Enjoy 👍😃😃



