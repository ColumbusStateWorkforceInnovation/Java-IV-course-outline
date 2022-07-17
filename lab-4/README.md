# Lab 4 - Property Validation & JPA Entity Relationships

This lab checks your knowledge of the content in chapters 9 and 12 of the *Spring Web Essentials* book.

## Before you begin

- [ ] This lab builds on the [previous one](../lab-3/README.md), so __do not start this lab__ until you have an approved pull request for Lab 1.
- [ ] Merge the changes in your Lab 3 pull request to your main (or master branch) if you have not already done so.
- [ ] Create a new branch from your main (or master) branch and switch to it before committing any changes for Lab 4.

## The Open API spec to implement

[Click here](https://app.swaggerhub.com/apis/DataDaddy/wiit-7340_degrees_at_cscc_api/0.3) to view the spec on SwaggerHub.

## Object model of the final product

The diagram below shows the object model for the lab solution.

![Object model for completed Lab 3 assignment](./images/ObjectModel.png)

## Tasks

### Update the project dependencies

- [ ] Add the following dependency to your `pom.xml` file:
  ```
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation</artifactId>
		</dependency>
  ```
- [ ] Right-click on your `pom.xml` file and choose "Maven > Reload Project" to refresh the project dependencies.

### Part 1 - Property validation

Add unit tests for each scenario listed below using the techniques shown in chapter nine of the *Spring Web Essentials* book. After adding a test, add just enough code to get the test to pass.

#### Scenario: POST with null required properties fails

* __When__ the client performs a POST request to `/api/menu/categories`,
* __and__ the request `contentType` is set to `application/json`,
* __and__ the request body contains a representation of a `MenuCategory` that has a `null` `categoryTitle`,
* __then__ the client receives a 400 bad request status,
* __and__ the controller never calls the repository `save()` method,
* __and__ the response body contains a `fieldErrors` JSON object with a `categoryTitle` property and a value of "must not be null".


#### Scenario: POST with invalid required properties fails

* __When__ the client performs a POST request to `/api/menu/categories`,
* __and__ the request `contentType` is set to `application/json`,
* __and__ the request body contains a representation of a `MenuCategory` that has a value shorter than 1 character or longer than 80 characters,
* __then__ the client receives a 400 bad request status,
* __and__ the controller never calls the repository `save()` method,
* __and__ the response body contains a `fieldErrors` JSON object with a `categoryTitle` property and a value of "Please enter a category title up to 80 characters in length".


#### Scenario: PUT with conflicting IDs fails

* __when__ the client performs a PUT request to `/api/menu/categories/%d` where `%d` is any positive non-zero value,
* __and__ the request `contentType` is set to `application/json`,
* __and__ the request body contains a valid representation of a `MenuCategory`,
* __and__ the `id` in the request body differs from the one specified in the URL,
* __then__ the client receives a 409 CONFLICT status,
* __and__ the controller never calls the repository `save()` method.


#### Scenario: PUT with null required properties fails

* __when__ the client performs a PUT request to `/api/menu/categories/%d` where `%d` is any positive non-zero value,
* __and__ the request `contentType` is set to `application/json`,
* __and__ the request body contains a representation of a `MenuCategory` that has a `null` `categoryTitle`,
* __and__ the `id` in the request body matches the one specified in the URL,
* __then__ the client receives a 400 bad request status,
* __and__ the controller never calls the repository `save()` method,
* __and__ the response body contains a `fieldErrors` JSON object with a `categoryTitle` property and a value of "must not be null".


#### Scenario: PUT with invalid required properties fails

* __when__ the client performs a PUT request to `/api/menu/categories/%d` where `%d` is any positive non-zero value,
* __and__ the request `contentType` is set to `application/json`,
* __and__ the request body contains a representation of a `MenuCategory` that has a value shorter than 1 character or longer than 80 characters,
* __and__ the `id` in the request body matches the one specified in the URL,
* __then__ the client receives a 400 bad request status,
* __and__ the controller never calls the repository `save()` method,
* __and__ the response body contains a `fieldErrors` JSON object with a `categoryTitle` property and a value of "Please enter a category title up to 80 characters in length".

### Part 2 - JPA Relationships

![MenuItem class diagram](./images/MenuItem.png)
Getting started:
- [ ] Add a new `MenuItem` domain class with the properties and a `@ManyToOne` JPA entity relationship as shown above.
- [ ] Add a new interface called `edu.cscc.degrees.data.MenuItemRepository` that extends `org.springframework.data.repository.CrudRepository`.
- [ ] Add a new unit test class called `edu.cscc.degrees.api.MenuItemControllerTests`.

Add unit tests for each scenario listed below using the techniques shown in chapters eight and twelve of the *Spring Web Essentials* book. After adding a test, add just enough code to get the test to pass.

#### Scenario: Clients can create new menu items.

* __When__ the client performs a POST request to `/api/menu/items`,
* __and__ the `@id` of menu item in the request body is 0,
* __and__ other menu item properties are populated,
* __then__ the controller calls the save() method __one time__ on the menu item repository,
* __and__ the menu item repository returns an updated instance with a non-zero `@id` value,
* __and__ the menu item controller returns a representation of the newly saved instance in the response body,
* __and__ the `id` property of the returned instance matches the `id` property returned by the repository `save()` method,
* __and__ the `menuCategory.id` property of the returned instance matches the `menuCategory.id` property returned by the repository `save()` method,
* __and__ the `name` property of the returned instance matches the `name` property returned by the repository `save()` method,
* __and__ the `description` property of the returned instance matches the `description` property returned by the repository `save()` method,
* __and__ the `sortOrder` property of the returned instance matches the `sortOrder` property returned by the repository `save()` method,
* __and__ the menu item controller returns a status of 201 created,
* __and__ the response contains a `Location` header with the value `http://localhost/api/menu/items/%d` where `%d` matches the `id` property returned by the repository `save()` method.

#### Scenario: get all returns an empty list when none exist

* __Given__ no menu items exist in the database,
* __When__ the client performs a GET request to `/api/menu/items`,
* __then__ the controller calls the repository [`findAll()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findAll--) method one time,
* __and__ the repository `findAll()` method returns an empty list,
* __and__ the controller sets the response body to the empty list returned,
* __and__ the controller sets the `contentType` of the response to `application/json`
* __and__ the menu item controller returns a status of 200 OK,
* __and__ the length of the list in the response body is 0.

#### Scenario: get all returns a non-empty list when one or more records exist

* __Given__ one or more menu items exist in the database,
* __When__ the client performs a GET request to `/api/menu/items`,
* __then__ the controller calls the repository [`findAll()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findAll--) method one time,
* __and__ the repository `findAll()` method returns an [`Iterable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html) with one or more items,
* __and__ the controller sets the response body to the list returned,
* __and__ the controller sets the `contentType` of the response to `application/json`
* __and__ the menu item controller returns a status of 200 OK,
* __and__ the length of the list in the response body equals the length of the list returned by the `findAll()` method,
* __and__ for each item in the response body list:
  * __the__ the `id` property of the returned instance matches the `id` property returned by the repository `save()` method,
  * __and__ the `menuCategory.id` property of the returned instance matches the `menuCategory.id` property returned by the repository `save()` method,
  * __and__ the `name` property of the returned instance matches the `name` property returned by the repository `save()` method,
  * __and__ the `description` property of the returned instance matches the `description` property returned by the repository `save()` method,
  * __and__ the `sortOrder` property of the returned instance matches the `sortOrder` property returned by the repository `save()` method,

#### Scenario: get by ID returns a list with one item when it exists in the database

* __Given__ the client is attempting to retrieve a record that exists in the database,
* __when__ the client performs a GET request to `/api/menu/items/%d` where `%d` is any positive non-zero value,
* __and__ the controller calls the repository [`findById()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findById-ID-) method one time,
* __and__ the repository `findById()` method returns a `MenuItem` with then given ID,
* __and__ the controller sets the response body to a singleton list with the item returned,
* __and__ the controller sets the `contentType` of the response to `application/json`
* __then__ the menu item controller returns a status of 200 OK,
* __and__ the length of the list in the response body equals 1,
* __and__ for the single item in the response body list:
  * __the__ the `id` property of the returned instance matches the `id` property returned by the repository `save()` method,
  * __and__ the `menuCategory.id` property of the returned instance matches the `menuCategory.id` property returned by the repository `save()` method,
  * __and__ the `name` property of the returned instance matches the `name` property returned by the repository `save()` method,
  * __and__ the `description` property of the returned instance matches the `description` property returned by the repository `save()` method,
  * __and__ the `sortOrder` property of the returned instance matches the `sortOrder` property returned by the repository `save()` method,


#### Scenario: get by ID returns a 404 not found when the requested item does not exist in the database

* __Given__ the client is attempting to retrieve a record that does not exist in the database,
* __when__ the client performs a GET request to `/api/menu/items/%d` where `%d` is any positive non-zero value,
* __and__ the controller calls the repository [`findById()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findById-ID-) method one time,
* __and__ the `findById()` method returns an [`Optional.empty()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Optional.html#empty()),
* __then__ the menu item controller returns a status of 404 not found.

#### Scenario: saving an updated record succeeds when the record exists in the database

* __Given__ the client is attempting to update a record that exists in the database,
* __when__ the client performs a PUT request to `/api/menu/items/%d` where `%d` is any positive non-zero value,
* __and__ the request body contains a representation of a `MenuItem`,
* __and__ the controller calls the repository [`existsById()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#existsById-ID-) method one time passing in the ID included in the URL,
* __and__ the `existsById()` method returns `true`,
* __then__ the controller calls the `save()` method on the repository one time passing a menu item instance with all properties equal to those provided in the request body omitting "menuCategory" from the comparison,
* __and__ the `save()` method returns an updated version of the saved record,
* __and__ the menu item controller returns a status of 204 no content.

#### Scenario: saving an updated record fails when the record does not exist in the database

* __Given__ the client is attempting to update a record that does not exists in the database,
* __when__ the client performs a PUT request to `/api/menu/items/%d` where `%d` is any positive non-zero value,
* __and__ the request body contains a representation of a `MenuItem`,
* __and__ the controller calls the repository [`existsById()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#existsById-ID-) method one time passing in the ID included in the URL,
* __and__ the `existsById()` method returns `false`,
* __then__ the controller does **not** call the `save()` method on the repository,
* __and__ the menu item controller returns a status of 404 not found.

#### Scenario: delete succeeds when the record exists in the database

* __Given__ the client is attempting to delete a record that exists in the database,
* __when__ the client performs a DELETE request to `/api/menu/items/%d` where `%d` is any positive non-zero value,
* __and__ the controller calls the repository [`findById()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findById-ID-) method one time passing in the ID included in the URL,
* __and__ the repository `findById()` method returns a `MenuItem` with then given ID,
* __then__ the controller calls the `delete()` method on the repository one time passing the instance returned by `findById()`,
* __and__ the menu item controller returns a status of 204 no content.

#### Scenario: attempt to delete nonexistent record fails with a 404

* __Given__ the client is attempting to delete a record that exists in the database,
* __when__ the client performs a DELETE request to `/api/menu/items/%d` where `%d` is any positive non-zero value,
* __and__ the controller calls the repository [`findById()`](https://docs.spring.io/spring-data/commons/docs/current/api/org/springframework/data/repository/CrudRepository.html#findById-ID-) method one time passing in the ID included in the URL,
* __and__ the `findById()` method returns an [`Optional.empty()`](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Optional.html#empty()),
* __then__ the menu item controller returns a status of 404 not found.
* __and__ the controller does **not** call the `delete()` method on the repository.

## OPTIONAL Stretch Goal Assignment

- [ ] Add a `@NotNull` validation constraint on `MenuItem.name`
- [ ] Write a unit test for POST and PUT that, when null, verifies the response body contains a `fieldErrors` JSON object with a `name` property and a value of "must not be null".
- [ ] Add a `@Size` validation constraint on `MenuItem.name` with a min size of 1 and a max of 80.
- [ ] Write a unit test for POST and PUT that, when the length is outside these bounds, the response body contains a `fieldErrors` JSON object with a `name` property and a value of "Please enter a name up to 80 characters in length".
- [ ] Add a `@NotNull` validation constraint on `MenuItem.price`
- [ ] Write a unit test for POST and PUT that, when null, verifies the response body contains a `fieldErrors` JSON object with a `price` property and a value of "must not be null".
- [ ] Add a `@Size` validation constraint on `MenuItem.price` with a min size of 1 and a max of 20.
- [ ] Write a unit test for POST and PUT that, when the length is outside these bounds, the response body contains a `fieldErrors` JSON object with a `price` property and a value of "Please enter a price up to 20 characters in length".
- [ ] Add a unit test for PUT that verifies the `id` in the URL matches the `MenuItem.id` in the request body and, if not, returns a 409 CONFLICT status,


## Notes
* Check the values of all properties returned by the controller match those returned by the stubbed repository methods. 
* Verify all interactions with the repository methods.

## Submitting your work

- [ ] Verify your tests pass.
- [ ] Commit your changes to the lab 4 branch you created.
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
