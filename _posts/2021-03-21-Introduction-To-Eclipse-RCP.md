---
title: Introduction to Eclipse RCP
author: Anouar Asmai
date: 2021-03-26 16:47 +0800
categories: [Blogging, Tutorial]
tags: [Eclipse_RCP]
---

![rcp]({{ "/assets/img/sample/rcp.png"}})

# Introduction

---

   **Eclipse RCP (Eclipse Rich Client Platform)** is an Eclipse based application, we could say that RCP provides us a generic Eclipse workbench or framework that developers can use to build and develop Desktop applications, it provides also a basic framework that can be extended with custom application functionality.
So, the Eclipse IDE is just a specific Eclipse application that provides developers to have an environment for their software development, many aspects and components of the Eclipse IDE can also be used in other applications such as, the workbench structure of the user interface, the extensible plug-in system, the help component.. An Eclipse application consists of at least one
software component called ***plug-in*** which is extensible to be used by other plug-ins.


> ***I hope this definition was clear, now we could take a look for why we should build an Eclipse RCP application.***

# The major Pros of Eclipse RCP:

* The platform is mainly designed for modularity and extensibility.
* Extensions based on the Eclipse RCP platform can work together without
without knowing each other. 
* Applications can be delivered in different configurations, as well as plug-ins that were initially developed separately can later be combined into one application.
* Using Eclipse RCP, we can mention the extensebility will allow us to create an open platforms and we can create our own feature-rich and professional application quickly under multiple platforms as well as RCP provides us a high quality of components that are able for reuse.
* Eclipse RCP provides base components for graphical applications that were common and have been proven in many use cases encountered by developers before.


> Now, let's have a quick look for the basic elements of the Eclipse user interface:

# Basic Elements of the Eclipse user interface:

![rcp]({{ "/assets/img/sample/eclipse.png"}})

- **Workbench Window**: The broad container for all windows.
- **View**: A visual container to display resources of a particular type. Typically, a view contains a data grid or tree structure. In the figure, the tasks view is an example of a view that is used within the Java perspective.
- **Short Cut Bar**: A set of icons that enables the user to quickly access different perspectives.
- **Menu Bar**: A set of content-sensitive actions that gives the user the ability to execute some predefined function.
- **Tool Bar**: A set of context-sensitive actions that enables the user to execute some predefined function. All the items found within the toolbar appear within the menu bar.
- **Editor**: Editors are the primary tool users employ to display and manipulate data. In the case of the Eclipse IDE, developers use an editor to edit Java source files.


# The difference between Eclipse Plug-in Development and RCP Development :


Traditional Eclipse plug-in development is focused on plugging into an IDE. Unlike RCP Development, You have then the opportunity to define more of the look and feel, the branding, and other fundamental elements of Eclipse which are not provided with Plug-in Development. Therefore, RCP development like expands the boundaries of your application and pushes you to think about frameworks of your own and how others will integrate into your product.


> **Note**: We can slice/dice the RCP itself or any other plug-in set to suit our needs.


# The components of the Eclipse RCP Architecture:

---

![rcp]({{ "/assets/img/sample/eclipseRCPfigure2.png"}})


- All of Eclipse's functionality is implemented through the use of plug-ins. A ***plug-in*** is the base building block that developers use to add new capabilities and functionality to the environment. This plug-in have to define a set of ***extensions***, with which you can add feature or extend from other existing extensions, as well as, each of these extensions is defined within a plug-in's manifest file. The ***Eclipse runtime*** is responsible for managing the lifecycle of a plug-in within a workbench. 



![rcp]({{ "/assets/img/sample/SWT&JFace.png"}})


- ***JFace*** : briefly is a UI toolkit with classes for handling many common UI programming tasks and provides helper classes for developing UI features. Moreover, JFace is designed to provide common application UI functionality on top of the SWT library.

- ***SWT*** : the Standard Widget Toolkit is the library used by Eclipse that  provides a platform-independent API that is tightly integrated with the operating system's native windowing environment. SWT's approach provides Java developers with a cross-platform API to implement
solutions that "feel" like native desktop applications.

- The differences between SWT and JFace are much cleaner. SWT does not depend on any JFace or platform code at all. Many of the SWT examples show how you can build a standalone application.


# Simple first steps to create an RCP Application:

---

- You have first to open your Eclipse IDE and select **Window > Open Perspective > Other** from the menu bar. Then select ***Plug-in Development***. 

> ***Plug-in Development*** : is what you need for developing Eclipse Plug-in projects. So you have to install PDE first for building an RCP development. If you need to install it, here is the link for you: https://www.eclipse.org/rap/downloads/, and choose **Download an Eclipse IDE package with RAP Tools included**.

- This figure shows, choose the name of the project and press next:

![rcp]({{ "/assets/img/sample/figure1.png"}})

- After, press next:

![rcp]({{ "/assets/img/sample/figure2.png"}})

- Now, select **Eclipse 4 RCP application** and click on **Finish**:

![rcp]({{ "/assets/img/sample/figure3.png"}})

- This ***Overview*** tab will be shown like this :

![rcp]({{ "/assets/img/sample/figure4.png"}})



- This Overview tab is a tool for editing the generated **plug-in manifest**. This one is responsible for defining the resources, dependencies, and extensions the
Eclipse runtime will manage. The **plug-in manifest** for any project is located within the
project's root directory and is called ***plugin.xml***.

![rcp]({{ "/assets/img/sample/figure5.png"}})


Indeed, the ***plugin.xml*** allows you to view the XML that each section of the editor
generates. 


- Now, we can mention some **Tags**, each one of them has its functionality, we will discuss about that on the last blog as well as many other things concerning the RCP Applications.




## Conclusion:

---

Eclipse RCP Development is an exciting paradigm intended to developers to have a single cross-platform environment to create highly-interactive business applications. But it's not that easy to learn how to build an RCP Application because there are several instructions to follow and much knowledge to learn about it. I didn't mention many things concerning the RCP Development but I has just clarified some basics for you if you are the first time using it, and I will talk about many other things on my next blogs on that concern (RCP).

I hope you enjoyed reading my simple and brief blog :)!!
 
