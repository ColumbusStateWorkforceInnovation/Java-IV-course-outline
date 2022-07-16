# Java IV - Sylabus & Course Outline

## Week 1 - Introduction to Web APIs

* Reading:
    * _Spring Web Essentials_ through the end of chapter 4
* Demos:
    * Demo of the inspiration website -- https://degrees.cscc.edu/.
    * Demo of the final API and front end running.
* Lecture:
    * HTTP as the foundation of REST
        * Describe web services and how they work.
        * Be able to explain the Hypertext Transport Protocol (HTML) and associated standards.
        * Explain how Representational State Transfer (REST).application programming interfaces APIs power web applications
        * Show how the OpenAPI spec is used to describe APIs.
    * Creating our first API
        * Describe dependency management for web applications.
        * Create a new Spring application using https://start.spring.io/.
        * Examine the structure of our starter application.
        * Create a GitHub private repository for starter application.
    * Writing our first API method
        * Develop a unit test for posting a new blog article.
        * Implement a controller method to get our test passing.
* Activities and assignments:
    * __In class group activity:__ [Create a base application and commit it to GitHub](./lab-0/README.md).
    * __Knowledge checks:__ Quiz on HTTP, REST, and APIs.
    * __LAB 1:__ [Implementing our first API method](./lab-1/README.md) &mdash; Due at the start of week 2.


## Week 2 - Entity classes and the Database

* Reading:
    * _Spring Web Essentials_ through the end of chapter 7
* Lecture:
    * Adding an object model
        * Create a blog post domain class
        * Incorporate it into our API
    * Adding an in-memory database
        * Describe how in-memory databases work
        * Configure Spring to use one for development
        * Create an integration test for our POST method
        * Use a Spring Java Persistence Architecture (JPA) repository as backing store for our blog posts
    * Testing with mocks
        * Configure our new class for mocking
        * Use a mock bean for unit testing
        * Develop a complete unit test for POST
* Activities and assignments:
    * __LAB 2:__ [Mocks, Stubs, Entities, and Persistence](./lab-2/README.md) &mdash; Due at the start of week 3.

## Week 3 - CRUD, Profiles and Postgres

* Reading:
    * _Spring Web Essentials_ chapters 8 and 10 (skip 9)
* Lecture:
    * Build read, update and delete methods for our API.
    * Learn how Spring profiles are used to support different runtime configurations
    * Add configuration to connect to a Postgres database
    * Use Spring profiles to switch between the H3 in-memory database and the external Postgres database.
* Activities and assignments:
    * __LAB 3:__ [Read, Update, and Delete for our API](./lab-3/README.md) &mdash; Due at the start of week 5.


## Week 4 - Property Validation & JPA Entity Relationships

* Reading:
    * _Spring Web Essentials_ chapters 9 and 12 (skip 11).
* Lecture:
    * Use JPA relationships to associate entity classes.
* Activities and assignments:
    * __LAB 4:__ Property Validation & JPA Entity Relationships &mdash; Due at the start of week 6.


## Week 5 - Putting our Services to Work

* Reading:
    * _Spring Web Essentials_ chapter 11.

* Activities and assignments:
    * __LAB 5:__ Hook up the Vue app &mdash; Due at the start of week 7.

## Week 6 - Securing our API

* Reading:
    * _Spring Web Essentials_ chapter 13.

* Activities and assignments:
    * __LAB 6:__ Secure the API &mdash; Due at the start of week 8.

## Week 7 - Server Side Websites

Demo of Spring MVC with Tymeleaf

* Reading:
    * _Spring Web Essentials_ chapters 14 - 16.


## Week 8 - Course Wrap-Up and Supplemental Resources

