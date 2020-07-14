---
title: Rest API with Spring Boot , MVC , Data JPA
author: Anouar Asmai
date: 2020-07-14 01:33:00 +0800
categories: [Blogging, Demo]
tags: [Rest_API]
---

![REST]({{ "/assets/img/sample/rest.jpg"}})



# Introduction 

---

  Spring Boot provides a very good support to building RESTful Web Services for enterprise applications. This chapter will explain in detail about building RESTful web services using Spring Boot.So , let's start to learn how to build a RESTful API with Spring Boot,MVC & JPA.
  
  >Before start this blog , we'll understand what is REST ?
  
  
## What is REST ?

---

 **REST** (representational state transfer) is a style of software architecture defining a set of constraints to be used to create web services. REST architecture style web services, also called RESTful web services, establish interoperability between computers on the Internet. REST web services allow requesting systems to manipulate web resources via their text representations through a set of stateless, predefined uniform operations. Other types of web services such as SOAP web services expose their own sets of arbitrary operations.Now , it's time to understand what is a RESTful API .
 
 
## RESTful API :

---

**A RESTful API** is an application program interface (API) that uses HTTP requests to GET, PUT, POST and DELETE data.

An **API** for a website is code that allows two software programs to communicate with each other. The API spells out the proper way for a developer to write a program requesting services from an operating system or other application.

**A RESTful API** also referred to as a RESTful web service or REST API -- is based on representational state transfer (REST), an architectural style and approach to communications often used in web services development. 


>*Well , we can now begin our example for a Restful Api or Restful Web Services*


Our example is a simple API for managing a school,here is the UML diagram that I was working on :


![UML]({{ "/assets/img/sample/databaseerdiagram.png"}})

  
  Firstly, Spring Boot provides a very good support to building RESTful Web Services for enterprise applications. This chapter will explain in detail about building RESTful web services using Spring Boot.
  Spring Boot is an open source Java-based framework used to create a Micro Service. It is developed by Pivotal Team. It is easy to create a stand-alone and production ready spring applications using Spring Boot. Spring Boot contains a comprehensive infrastructure support for developing a micro service and enables you to develop enterprise-ready applications that you can **“just run”**.
  
  
  This is our **pom.xml** : we need these dependencies for building our REST API
  
 ```xml
  <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.3.0.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>API-demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>API-demo</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>

		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

>Note : You can go to Spring Initializr and create a project with below settings by adding your dependencies that you'll need like the following picture.



![init]({{ "/assets/img/sample/init.png"}})


Next Create the database name called user_database in MySQL database server and define connection properties in *School-API/src/main/resources/application.properties :*

```
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost/demoapi?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5InnoDBDialect
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
pring.jpa.show-sql = true
```

This properties files are used to keep ‘N’ number of properties in a single file to run the application in a different environment. In Spring Boot, properties are kept in the application.properties file under the classpath.

After that , we define our models (I just define three models because the others are the same)

```java
package com.example.API.demo.Models;

import com.example.API.demo.Models.*;
import lombok.Data;
import org.hibernate.annotations.LazyCollection;
import org.hibernate.annotations.LazyCollectionOption;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;
@Entity
@Data
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    private String first_name;
    private String last_name;

    @ManyToOne
    private Groups group;

    @LazyCollection(LazyCollectionOption.FALSE)
    @OneToMany(mappedBy = "student",cascade = CascadeType.ALL)
    List<Mark> marks = new ArrayList<>();

    public Student() {
    }

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getFirst_name() {
        return first_name;
    }

    public void setFirst_name(String first_name) {
        this.first_name = first_name;
    }

    public String getLast_name() {
        return last_name;
    }

    public void setLast_name(String last_name) {
        this.last_name = last_name;
    }

    public Groups getGroup() {
        return group;
    }

    public void setGroup(Groups group) {
        this.group = group;
    }

    public List<Mark> getMarks() {
        return marks;
    }

    public void setMarks(List<Mark> marks) {
        this.marks = marks;
    }
}
```

```java
package com.example.API.demo.Models;

import lombok.Data;
import org.hibernate.annotations.LazyCollection;
import org.hibernate.annotations.LazyCollectionOption;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Entity
@Data
public class Groups {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    private String name;

    @LazyCollection(LazyCollectionOption.FALSE)
    @OneToMany(mappedBy = "group",cascade = CascadeType.ALL)
    private List<Student> students = new ArrayList<>();

    @LazyCollection(LazyCollectionOption.FALSE)
    @OneToMany(mappedBy = "groupp",cascade = CascadeType.ALL)
    private List<SubjectTeacher> subjectTeachers = new ArrayList<>();

    public Groups() {
    }

    public long getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public List<Student> getStudents() {
        return students;
    }

    public void setStudents(List<Student> students) {
        this.students = students;
    }


}
```

```java

package com.example.API.demo.Models;


import com.example.API.demo.Models.Subject;
import lombok.Data;
import org.hibernate.annotations.CreationTimestamp;

import javax.persistence.*;
import java.util.Date;

@Entity
@Data
public class Mark {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    private Date date;
    private float mark;
    @ManyToOne
    private Subject subject;
    @ManyToOne
    private Student student;

    public Mark() {
    }

    public long getMarkId() {
        return id;
    }

    public void setMarkId(int markId) {
        this.id = markId;
    }

    public Date getDate() {
        return date;
    }

    public void setDate(Date date) {
        this.date = date;
    }

    public float getMark() {
        return mark;
    }

    public void setMark(float mark) {
        this.mark = mark;
    }

    public Subject getSubject() {
        return subject;
    }

    public void setSubject(Subject subject) {
        this.subject = subject;
    }

    public Student getStudent() {
        return student;
    }

    public void setStudent(Student student) {
        this.student = student;
    }
}
```

Next, it's time for our **RestControllers** that define the *GET,POST,DELETE,PATCH* HTTP requests : the **@RestController** annotation is used to define the RESTful web services. It serves JSON, XML and custom response:

```java
package com.example.API.demo.Controllers;

import com.example.API.demo.Models.Groups;
import com.example.API.demo.Models.Subject;
import com.example.API.demo.Repo.GroupRepo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
public class GroupsController {

    @Autowired
    GroupRepo groupRepo;

    @GetMapping(value = "/group")
    public ResponseEntity<List<Groups>> showAllGroups() {
        return ResponseEntity.ok(groupRepo.findAll());
    }

    @GetMapping(value = "/group/{id}")
    public Groups groupById(@PathVariable long id) {
        return groupRepo.findById(id).get();
    }

    @PostMapping(value = "/postgroup")
    public void addgroup(@RequestBody Groups groups){
        groups.getStudents().forEach(s -> s.setGroup(groups));
        groups.getSubjectTeachers().forEach(x -> x.setGroupp(groups));
        groupRepo.save(groups);}

    @DeleteMapping(value = "/group/{id}")
    public void deleteGroup(@PathVariable long id) { groupRepo.deleteById(id); }
}
```

```java
package com.example.API.demo.Controllers;
import com.example.API.demo.Models.Student;
import com.example.API.demo.Models.Subject;
import com.example.API.demo.Repo.StudentRepo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
public class StudentController {

    @Autowired
    StudentRepo studentRepo;

    @GetMapping(value = "/student")
    public ResponseEntity<List<Student>> showAllStudent() {
        return ResponseEntity.ok(studentRepo.findAll());
    }

    @GetMapping(value = "/student/{id}")
    public Student studentById(@PathVariable long id) {
        return studentRepo.findById(id).get();
    }

    @PostMapping(value = "/poststudent")
    public void addstudent(@RequestBody Student student){
        student.getMarks().forEach(m -> m.setStudent(student));
        studentRepo.save(student);}

    @DeleteMapping(value = "/student/{id}")
    public void deleteStudent(@PathVariable long id) { studentRepo.deleteById(id); }
}
```

```java
package com.example.API.demo.Controllers;


import com.example.API.demo.Models.Mark;
import com.example.API.demo.Models.Student;
import com.example.API.demo.Repo.MarkRepo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
public class MarkController {

    @Autowired
    MarkRepo markRepo;

    @GetMapping(value = "/mark")
    public ResponseEntity<List<Mark>> showAllMarks() {
        return ResponseEntity.ok(markRepo.findAll());
    }

    @GetMapping(value = "/mark/{id}")
    public Mark markById(@PathVariable long id) {
        return markRepo.findById(id).get();
    }

    @PostMapping(value = "/postmark")
    public void addmark(@RequestBody Mark mark){ markRepo.save(mark);}

    @DeleteMapping(value = "/mark/{id}")
    public void deleteMark(@PathVariable long id) { markRepo.deleteById(id); }
}
```

>***Note*** :We'll understand this part of controllers and models (Web MVC) in the next Spring Web MVC's blog 

Now, we must use the reposotries interfaces that are extending the *JpaRepositry* : ***JpaRepository*** is JPA specific extension of Repository. It contains the full API of CrudRepository and PagingAndSortingRepository. So it contains API for basic CRUD operations and also API for pagination and sorting.

```java
package com.example.API.demo.Repo;


import com.example.API.demo.Models.Groups;
import com.example.API.demo.Models.Teacher;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface GroupRepo extends JpaRepository<Groups, Long> {
}
```

```java
package com.example.API.demo.Repo;


import com.example.API.demo.Models.Student;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface StudentRepo extends JpaRepository<Student,Long> {
}
```

```java
package com.example.API.demo.Repo;


import com.example.API.demo.Models.Mark;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import org.springframework.web.bind.annotation.RestController;

@Repository
public interface MarkRepo extends JpaRepository<Mark,Long> {
}
```

>*Note* : The **@Reposotry** annotation is used to indicate that the class provides the mechanism for storage, retrieval, search, update and delete operation on objects.
  
Finally , we can run our Api application by running this class :

```java

package com.example.API.demo;

import lombok.Data;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.domain.EntityScan;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Repository;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

@SpringBootApplication
public class ApiDemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(ApiDemoApplication.class, args);
	}

}
```

We can run the application in your IDE and test the Requests from Postman or Web Navigator , you can also add a front side and consume data from this simple Api.

## Conclusion :

---

This is a basic example for learn how to build a Restful API by using Spring Boot , Data & Web MVC .REST Architecture still usefull and stronger in the software development despite the creation of another Architecture such as GraphQL .I hope that you enjoy this blog 👍👍

