# Lab 3 - Read, Update, and Delete for our API

This lab checks your knowledge of the content in chapter eight of the *Spring Web Essentials* book.

## Before you begin

- [ ] This lab builds on the [previous one](../lab-2/README.md), so __do not start this lab__ until you have an approved pull request for Lab 1.
- [ ] Merge the changes in your Lab 2 pull request to your main (or master branch) if you have not already done so.
- [ ] Create a new branch from your main (or master) branch and switch to it before committing any changes for Lab 3.

## The Open API spec to implement

[Click here](https://app.swaggerhub.com/apis/DataDaddy/wiit-7340_degrees_at_cscc_api/0.3) to view the spec on SwaggerHub.

## Object model of the final product

The diagram below shows the object model for the lab solution.

![Object model for completed Lab 2 assignment](./images/ObjectModel.png)

## Tasks

Using the techniques shown in chapter eight of the *Spring Web Essentials* book, add unit tests for each of the scenarios listed below. After adding a test, add just enough code to get the test to pass.

### Get all menu categories

#### If none exist, return an empty list

* __Scenario:__ get all returns an empty list.
* __Given__ no menu categories exist in the database,
* __When__ the client performs a GET request to `/api/menu/categories`,
* __then__ the controller calls the repository [`findAll()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findAll--) method one time,
* __and__ the repository `findAll()` method returns an empty list,
* __and__ the controller sets the response body to the empty list returned,
* __and__ the controller sets the `contentType` of the response to `application/json`
* __and__ the menu category controller returns a status of 200 OK,
* __and__ the length of the list in the response body is 0,


#### One or more categories exist

* __Scenario:__ get all returns a non-empty list.
* __Given__ one or more menu categories exist in the database,
* __When__ the client performs a GET request to `/api/menu/categories`,
* __then__ the controller calls the repository [`findAll()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findAll--) method one time,
* __and__ the repository `findAll()` method returns an [`Iterable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html) with one or more items,
* __and__ the controller sets the response body to the list returned,
* __and__ the controller sets the `contentType` of the response to `application/json`
* __and__ the menu category controller returns a status of 200 OK,
* __and__ the length of the list in the response body equals the length of the list returned by the `findAll()` method,
* __and__ for each item in the response body list:
  * __the__ `id` property of the returned instance matches the `id` property of the corresponding item returned by the repository `findAll()` method,
  * __and__ the `categoryTitle` property of the returned instance matches the `categoryTitle` property of the corresponding item returned by the repository `findAll()` method,
  * __and__ the `categoryNotes` property of the returned instance matches the `categoryNotes` property of the corresponding item returned by the repository `findAll()` method,
  * __and__ the `sortOrder` property of the returned instance matches the `sortOrder` property of the corresponding item returned by the repository `findAll()` method,

### Get menu category by ID

#### Valid id provided

* __Scenario:__ get menu category by ID returns a non-empty list.
* __When__ the client performs a GET request to `/api/menu/categories/%d` where `%d` is any valid ID value,
* __then__ the controller calls the repository [`findById()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findById-ID-) method one time,
* __and__ the repository `findById()` method returns a `MenuCategory` with then given ID,
* __and__ the controller sets the response body to a singleton list with the item returned,
* __and__ the controller sets the `contentType` of the response to `application/json`
* __and__ the menu category controller returns a status of 200 OK,
* __and__ the length of the list in the response body equals 1,
* __and__ for the single item in the response body list:
  * __the__ `id` property of the returned instance matches the `id` property of the corresponding item returned by the repository `findById()` method,
  * __and__ the `categoryTitle` property of the returned instance matches the `categoryTitle` property of the corresponding item returned by the repository `findById()` method,
  * __and__ the `categoryNotes` property of the returned instance matches the `categoryNotes` property of the corresponding item returned by the repository `findById()` method,
  * __and__ the `sortOrder` property of the returned instance matches the `sortOrder` property of the corresponding item returned by the repository `findById()` method,


#### No category exists with that ID

* __Scenario:__ get menu category by ID returns a 404 not found.
* __When__ the client performs a GET request to `/api/menu/categories/%d` where `%d` is any valid ID value,
* __then__ the controller calls the repository [`findById()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findById-ID-) method one time,
* __and__ the `findById()` method returns an [`Optional.empty()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Optional.html#empty()),
* __then__ the menu category controller returns a status of 404 not found.



## Notes
* Check the values of all properties returned by the controller match those returned by the stubbed repository methods. 
* Verify all interactions with the repository methods.

## Submitting your work

- [ ] Verify your tests pass.
- [ ] Commit your changes to the lab 2 branch you created.
- [ ] Push your new branch to GitHub.
- [ ] Create a pull request on the new branch.
- [ ] Add your instructor as a reviewer.
- [ ] __Wait__ for your instructor to approve your pull request before submitting your work in Blackboard.
- [ ] If changes are requested, make them and push them as extra commits on the branch you created at the start of the lab. Do *not* create a new branch or second pull request.
- [ ] __Once your instructor has approved your pull request:__
  - [ ] [Create a Git Bundle](https://git-scm.com/docs/git-bundle) of your work.
  - [ ] Submit your work in Blackboard, including both:
    1. The Git bundle you created, and 
    2. A link to the pull request based on the work.

## Before moving on to the next lab

- [ ] Merge your pull request to your main (or master) branch, so it has all the changes you made in this lab.
