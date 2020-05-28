---
title: Spring AOP Introduction and terminologies of AOP
author: Anouar Asmai
date: 2020-05-28 01:33:00 +0800
categories: [Blogging, Demo]
tags: [Spring AOP]
---


![Aop View]({{ "/assets/img/sample/aop.png"}})



# Introduction 

---

   Spring Framework is developed on two core concepts – Dependency Injection and Aspect Oriented Programming ( Spring AOP). We'll discuss  the DI (Dependency Injection) in the near future .So, in this tutorial, we’ll introduce AOP (Aspect Oriented Programming) with Spring and try to understanding how we can start using this powerful tool in practical scenarios. So let's strated !!

# What means AOP (Aspect Oriented Object) ?


 *Aspect Oriented Object* is a programming paradigm that aims to increase modularity by allowing the separation of cross-cutting concerns.I will explain this separation on the next step. Briefly , it does so by adding additional behavior to existing code without modification of the code itself.
 
 
> This definition seems not to be easy to understand it from the first reading and it's very normal because I'll explain you every part of this concept and a little exemple . Let's go ahead !!

  
  
# Spring AOP Overview ?



  For more explanation , most of the enterprise applications have some common crosscutting concerns that are applicable to different types of Objects and modules. For being clear , this crosscutting concerns can be a methods that are repeated on your different objects . Some of the common crosscutting concerns are logging, transaction management, data validation, etc..
  
  
  We can achieve to separate these crosscutting concerns by adding a new class which has this concern in the form of method . But , there is some problems that we can facing using this solution including : too many relationships to the crosscuttting objects !!
  
  
  Although , Spring AOP takes out the direct dependency of crosscutting tasks from classes that we can’t achieve through normal object oriented programming model. For example, we can have a separate class for logging but again the functional classes will have to call these methods to achieve logging across the application.
  
  
  To be exact , AOP is about encapsulating system-wide concerns or cross-cutting concerns (a concern that is applicable throughout the system and it affects the entire system). logging, security, and data transfer are the concerns which are needed in almost every part of a system.
  
  
# What problems solve AOP for devs ?


 
  The most important functionality is AOP provides the pluggable way to dynamically add the additional concern before, after or around the actual logic.
  So , actually AOP comes to :
  
   - Help you against tightly coupled dependencies in the code by extracting and moving them to an aspect
   
   - Allow users to implement custom aspects
   
   - Help you avoid repeating code in multiple places..
   
   
---

 *Let's see the Aspect Oriented Programming's Core Concepts/Terminology* 

---



# Core AOP Concepts 


 
 Spring AOP consists of 7 core concepts which are depicted in the following diagram:
 

   ![Aop View]({{ "/assets/img/sample/concepts_aop.png"}})
 
 
  ***Aspect***:
  
   A modularization of a concern that cuts across multiple classes. Transaction management is a good example of a crosscutting concern in enterprise Java applications. In Spring AOP, aspects are implemented using regular classes (the schema-based approach) or regular classes annotated with the @Aspect annotation
  
  ***Join point***: 
  
   A point during the execution of a program, such as the execution of a method or the handling of an exception. In Spring AOP, a join point always represents a method execution.
  
  ***Pointcut***: 
  
   A pointcut expression identifies the join point through some sort of expression matching.
  
  ***Advice***: 
  
   This is the block of code that you execute at a join point that was selected by a pointcut. So It is your cross-cutting concern method that we’re applying to a joint point in our system.
  
  ***Target Object***:
  
   These are the objects on which advices are applied. In Spring AOP, a subclass is created at runtime where the target method is overridden and advices are included based on their configuration.
  
  ***AOP proxy***:
  
   Spring AOP implementation uses JDK dynamic proxy to create the Proxy classes with target classes and advice invocations, these are called AOP proxy classes. We can also use CGLIB proxy by adding it as the dependency in the Spring AOP project.
  
  ***Weaving***:
  
   Weaving is the process of linking an aspect with other application types or objects to create an advised object.
  
  
 ---
  
  
  
  
  
# Steps to create AOP Example
  
  
  *So , Having understood this, let’s move further and see what are the steps required to create an AOP*
  
 ---
  
  
   ![Desktop View]({{ "/assets/img/sample/steps.jpg"}})
   
   
   
# AOP Advice Types : 
   

  
  Based on the execution strategy of advice, they are of the following types.
  

  ***Before Advice:*** 
   
   - These advices runs before the execution of join point methods. We can use @Before annotation to mark an advice type as Before advice.
   
  ***After (finally) Advice:***
   
   - An advice that gets executed after the join point method finishes executing, whether normally or by throwing an exception. We can create after advice using @After annotation.
   
   ***Around Advice :*** 
   
   - Annotation specifies that the advice is executed both before and after the execution of the target method, and so on.
   
   
   
   
# Spring AOP :
  

  
  > Spring AOP is proxy-based. Spring uses either JDK proxies (default) or CGLIB proxies to create the proxy for a given target bean. At runtime, calls to the target object are intercepted by the proxy, and advices that apply to the target method are executed by the proxy.
In Spring AOP, a target object is a bean instance registered with the spring container.



# Spring AOP Example :

***

In this simple example demo, I’ve used Spring AOP to add Logging behavior to a my *ShapeService*’s methods.
Here is my *ShapeService ,Circle , Triangle* classes and note that the methods annoted by ***@Loggable*** are our pointcut expression in the future :


***Circle.java***

```java
@Component
public class Circle {

        private String name="John";
        @Loggable
        public String getName() {
            System.out.println("Inside getName of circle ");
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
}
```

***Triangle.java***


```java


@Component
public class Triangle {

    private String name="triangle";
    @Loggable
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}


```

***ShapeService.java***

```java
 
@Component
public class ShapeService { 
    
    @Autowired
    private Circle circle;
    @Autowired
    private Triangle triangle;

    public Circle getCircle() {
        System.out.println("Inside getCircle method");
        return circle;
    }

    public void setCircle(Circle circle) {
        this.circle = circle;
    }

    public Triangle getTriangle() {
        return triangle;
    }

    public void setTriangle(Triangle triangle) {
        this.triangle = triangle;
    }
}


```



Then , if you want to use AspectJ annotation-style for creating aspects, you need to enable support for using AspectJ annotation-style by annotating the config class with ***@EnableAspectJAutoProxy*** . The element also instructs the Spring AOP framework to automatically create AOP proxies for target objects.



***ConfigClass.java***

```java

@Configuration
@ComponentScan("Mypackage")
@EnableAspectJAutoProxy
public class ConfigClass {

}

```
 
 
Then adding our Aspect named *LoggingAspect* :


***LoggingAspect.java***

```java
@Aspect
@Component
public class LoggingAspect {
    @Before("@annotation(Loggable)")
    public void LoggingAdvice(){
        System.out.println("Running Advice ");
    }
}
```

So , the advice here is the *LoggingAdvice* method .



AspectJ’s ***@Before*** annotation specifies that the advice is executed before the invocation of the target method. It takes an argument for the pointcut expression that will identify the join points. Here the join points are any method that is annotated with @Loggable, the custom annotation I’ve defined:

```java

public @interface Loggable {
}

```


Finally , here is the main class where I have called the getName of the getCircle method for testing our simple Aspect :  



***MainApp.java***

```java

public class MainApp {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(ConfigClass.class);
        ServiceShape serviceShape = ctx.getBean("serviceShape",ServiceShape.class);
        serviceShape.getCircle().getName();

    }
}

```


I was working with confugration based Java Annotations configuration because it's more efficient and preformant than the XML configuration , if you're working with the config based XML-configuration , there is no problem to do this config .


So , the Output is something like this :

```console

Inside getCircle
Running Advice 
Inside getName of Circle 

```


# Conclusion

***

That’s all for Spring AOP Example Tutorial, and you have seen how  AOP encapsulates all of these cross-cutting concerns into something called an Aspect. I hope you learned the basics of AOP with Spring and can learn more from examples.

Here is the link of the Demo if you want to test it  :  [Demo AOP](https://github.com/Anouar1611/AopDemo.git)

Wish you that you enjoy this blog !!





<div id="fb-root"></div>
<script async defer crossorigin="anonymous" src="https://connect.facebook.net/fr_FR/sdk.js#xfbml=1&version=v7.0"></script>


<div class="fb-comments" data-href="https://anouar1611.github.io/posts/text-and-typography/" data-numposts="5" data-width=""></div>


<div class='jekyll-twitter-plugin' align="center">
    {% twitter https://twitter.com/AnouarAsmai maxwidth=500 limit=5 %}
</div>
