# Java IV - Syllabus & Course Outline

## Week 1 - Introduction to Web APIs

* Reading:
    * _Spring Web Essentials_ through the end of chapter 4.
* Demos:
    * Demo of the inspiration website -- https://degrees.cscc.edu/.
    * Demo of the final API and front end running.
* Lecture:
    * HTTP as the foundation of REST:
        * Describe web services and how they work.
        * Be able to explain the Hypertext Transport Protocol (HTML) and associated standards.
        * Explain how Representational State Transfer (REST) application programming interfaces APIs power web applications
        * Use the OpenAPI spec to describe APIs.
    * Creating our first API:
        * Describe dependency management for web applications.
        * Create a new Spring application using https://start.spring.io/.
        * Examine the structure of our starter application.
        * Create a GitHub private repository for the starter application.
    * Writing our first API method:
        * Develop a unit test for posting a new blog article.
        * Implement a controller method to get our test passing.
    * Test our application using Postman.
* Activities and assignments:
    * __In class group activity:__ [Create a base application and commit it to GitHub](./lab-0/README.md).
    * __Knowledge checks:__ Quiz on HTTP, REST, and APIs.
    * __LAB 1:__ [Implementing our first API method](./lab-1/README.md) &mdash; Due at the start of week 2.


## Week 2 - Entity classes and the Database

* Reading:
    * _Spring Web Essentials_ through the end of chapter 7.
* Lecture:
    * Demo: [HTML and web servers](https://gist.github.com/jeff-anderson-cscc/b2657d9e8bc14508bbe8a3f809f6b3dc)
    * Review of Chapter 3 from *Spring Web Essentials*.
    * How [Java Annotations](https://docs.oracle.com/javase/tutorial/java/annotations/) are used as a [decorator design pattern](https://learning.oreilly.com/library/view/head-first-design/0596007124/ch03.html) by Spring Boot to configure applications.
    * Adding an object model:
        * Create a blog post domain class.
        * Incorporate it into our API.
    * Adding an in-memory database:
        * Describe how in-memory databases work.
        * Configure Spring to use one for development.
        * Create an integration test for our POST method.
        * Use a Spring Java Persistence Architecture (JPA) repository as a backing store for our blog posts.
    * Test our controller with mocks:
        * Configure our new class for mocking.
        * Use a mock bean for unit testing.
        * Develop a complete unit test for POST.
    * Test our application using Postman.
* Activities and assignments:
    * __LAB 2:__ [Mocks, Stubs, Entities, and Persistence](./lab-2/README.md) &mdash; Due at the start of week 3.

## Week 3 - CRUD, Profiles, and Postgres

* Reading:
    * _Spring Web Essentials_ chapters 8 and 10 (skip 9).
* Lecture:
    * Build read, update and delete methods for our API.
    * Use Docker to run Postgres.
    * Use Spring profiles to support different runtime configurations.
    * Add a profile to use our Postgres database.
    * Use profiles to switch between the H3 in-memory and external Postgres databases.
    * Testing our application using Postman.
* Activities and assignments:
    * __LAB 3:__ [Read, Update, and Delete for our API](./lab-3/README.md) &mdash; Due at the start of week 4.

## Week 4 - Property Validation & JPA Entity Relationships

* Reading:
    * _Spring Web Essentials_ chapters 9 and 12 (skip 11).
* Lecture:
    * Spring JPA
        * Learn how to associate entity classes using Java Persistence Architecture (JPA) relationships.
        * Creating one-to-one, one-to-many, and many-to-one relationships.
        * Unit testing entities with JPA relationships.
    * Property Validation
        * Add the dependencies to our `pom.xml` file.
        * Learn how bean validation constraints work.
        * Apply validation constraints to our entity classes.
        * Create tests for invalid or missing properties.
* Activities and assignments:
    * __LAB 4:__ [Property Validation & JPA Entity Relationships](./lab-4/README.md) &mdash; Due at the start of week 6.

## Week 5 - Putting our Services to Work

* Reading:
    * _Spring Web Essentials_ chapter 11.
* Lecture:
    * Learn how property expressions make creating Spring Data Repository queries easy.
    * Develop new controller methods to provide supplemental data.
    * Download and run the front-end application.
    * Add Cross-Origin Resource Sharing (CORS) support in our API.
    * Test our application using Postman.
* Activities and assignments:
    * __LAB 5:__ [Connecting the Front End Application](./lab-5/README.md) &mdash; Due at the start of week 7.

## Week 6 - Securing our API

* Reading:
    * _Spring Web Essentials_ chapter 13.
* Lecture:
    * Write unit tests that verify unauthenticated requests are prohibited.
    * Add dependencies to our POM file for [Spring Security](https://spring.io/projects/spring-security), Spring security testing, and [Okta Spring Boot Starter](https://github.com/okta/okta-spring-boot).
    * Configure our application to use Okta OAuth2.
    * Test our changes with Postman.
    * Update our existing unit tests to authenticate with [JSON Web Tokens (JWT)](https://tools.ietf.org/html/rfc7523).
    * Test the secure version of our web application against the secure Spring application.
    * Test our application using Postman.
* Activities and assignments:
    * __LAB 6:__ [Securing our Application](./lab-6/README.md) &mdash; Due at the start of week 8.

## Week 7 - Server Side Websites

* Reading:
    * _Spring Web Essentials_ chapters 14 - 16.
* Lecture:
    * Explain the different view technologies.
    * Create unit tests for old-school MVC controllers.
    * Learn how to create views using Thymeleaf:
        * Store, access, and display model attributes.
        * Display lists and use conditional logic.
        * Develop and use a custom data formatter.
* Activities and assignments:
    * No new assignments.

## Week 8 - Course Wrap-Up and Supplemental Resources

* Reading:
    * No new reading.
* Lecture:
    * Java IV Course Recap.
* Activities and assignments:
    * No new assignments.
