# Lab 1 - Implementing our first API method

This lab checks your knowledge of the content in Chapter Four of the *Spring Web Essentials* book.

## Before you begin

- [ ] This lab builds on the [previous one](../lab-0/README.md), so __do not start this lab__ until you have completed that activity.
- [ ] Create a new branch from your main (or master) branch and switch to it before committing any changes.

## The Open API spec to implement

```
openapi: 3.0.0
info:
  version: '0.1'
  title: wiit-7340-degrees-at-cscc-api
  description: Specification for the Degrees restaurant at Columbus State menu
paths:
  /api/menu/categories:
    post:
      summary: Create a new menu category
      responses:
        '201':
          description:
            The server created the menu category and the new instance is available at the specified location.
          headers:
            Location:
              description:
                The location of the newly created category
              schema:
                type: string
          content: {}
```

[Click here](https://app.swaggerhub.com/apis/DataDaddy/wiit-7340_degrees_at_cscc_api/0.1) to view the spec on SwaggerHub.

## Tasks

Using the techniques shown in Chapter 4 of the *Spring Web Essentials* book, create a unit test and controller method for the following scenario:

* __Scenario:__ Clients can create new menu categories.
* __When__ the client performs a POST request to `/api/menu/categories`,
* __then__ the controller returns a status of 201 created,
* __and__ the response contains a `Location` header with the value `http://localhost/api/menu/categories/1`.

## Notes

* For now, just hard-code the location in the test and controller method. We will make it dynamic in the next lab.
* You can use two test methods as shown in the book or consolidate it into one test. Either approach is acceptable, provided you test for both the return status code and the presence of the location header.

## Submitting your work

- [ ] Verify your tests pass.
- [ ] Commit your changes to the lab 1 branch you created.
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

- [ ] Merge your pull request to your main (or master) branch so it has all the changes you made in this lab.
