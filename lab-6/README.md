# Lab 6 - Securing our Application

In this lab, we will:

1. Write unit tests that verify unauthenticated requests are prohibited.
1. Add dependencies to our POM file for [Spring Security](https://spring.io/projects/spring-security), Spring security testing, and [Okta Spring Boot Starter](https://github.com/okta/okta-spring-boot).
1. Configure our application to use Okta OAuth2.
1. Test our changes with Postman.
1. Update our existing unit tests to authenticate with [JSON Web Tokens (JWT)](https://tools.ietf.org/html/rfc7523).
1. Test the secure version of our web application against the secure Spring application. 

## Before you begin

- [ ] This lab builds on the [previous one](../lab-5/README.md), so __do not start this lab__ until you have an approved pull request for Lab 5.
- [ ] Merge the changes in your Lab 5 pull request to your main (or master branch) if you have not already done so.
- [ ] Create a new branch from your main (or master) branch and switch to it before committing any changes for Lab 6.

## Tasks

First, we will add ten tests to verify our controller methods are correctly secured. __At this time, add the tests and make sure they fail as indicated below.__ Do not attempt to fix any of them yet. 

The functional requirements are as follows:

__Scenario:__ Unauthenticated PUT, POST, and DELETE requests are forbidden \
__given__ the client request does not include a valid, unexpired JSON Web Token (JWT) \
__when__ the client performs an otherwise valid PUT, POST, or DELETE request, \
__then__ the controller returns a status code of FORBIDDEN, \
__and__ no changes are saved.

### Changes to `MenuCategoryControllerTests`

- [ ] __POSTING a valid menu category without a JWT token is forbidden:__
 
    __When__:
    * The request body contains the JSON representation of a valid test menu category
    * AND the content type is MediaType.APPLICATION_JSON
    * AND POST to `/api/menu/categories` is invoked
    
    __THEN:__
    * The status is forbidden
    * AND the `save()` method of the mock repository is never called
    
    __Verify__ this initially does not pass with either:
    * A null pointer exception if you don't stub the repository save call 
    * OR: expected 403, actual 201 if you do
    
- [ ] __PUTting a valid menu category without a JWT token is forbidden:__
 
    __When__:
    * The mock repository existsById() returns true
    * AND the request body contains the JSON representation of a valid test menu category
    * AND the content type is MediaType.APPLICATION_JSON
    * AND the ID of the menu category in the request body is 100
    * AND PUT to `/api/menu/categories/100` is invoked
    
    __THEN:__
    * the status is forbidden
    * AND the `save()` method of the mock repository is never called
    
    __Verify__ this initially does not pass with: expected 403, actual 204
    
- [ ] __Calling DELETE with a numeric ID but without a JWT token is forbidden:__
 
    __When__:
    * The mock repository findById(1L)) returns the optional of a valid menu category
    * AND DELETE to `/api/menu/categories/1` is invoked
    
    __THEN:__
    * The status is forbidden
    * AND the `delete()` method of the mock repository is never called
    
    __Verify__ this initially does not pass with: expected 403, actual 204

### Changes to `MenuItemControllerTests`

- [ ] __POSTING a valid menu item without a JWT token is forbidden:__
 
    __When__:
    * The request body contains the JSON representation of a valid test menu item
    * AND the content type is MediaType.APPLICATION_JSON
    * AND POST to `/api/menu/items` is invoked
    
    __THEN:__
    * The status is forbidden
    * AND the `save()` method of the mock repository is never called
    
    __Verify__ this initially does not pass with either:
    * A null pointer exception if you don't stub the repository save call 
    * OR: expected 403, actual 201 if you do
    
- [ ] __PUTting a valid menu item without a JWT token is forbidden:__
 
    __When__:
    * The mock repository existsById() returns true
    * AND the request body contains the JSON representation of a valid test menu item
    * AND the content type is MediaType.APPLICATION_JSON
    * AND the ID of the menu item in the request body is 100
    * AND PUT to `/api/menu/items/100` is invoked
    
    __THEN:__
    * the status is forbidden
    * AND the `save()` method of the mock repository is never called
    
    __Verify__ this initially does not pass with: expected 403, actual 204
    
- [ ] __Calling DELETE with a numeric ID but without a JWT token is forbidden:__
 
    __When__:
    * The mock repository findById(1L)) returns the optional of a valid menu item
    * AND DELETE to `/api/menu/items/1` is invoked
    
    __THEN:__
    * The status is forbidden
    * AND the `delete()` method of the mock repository is never called
    
    __Verify__ this initially does not pass with: expected 403, actual 204


### Add dependencies to our POM file

Next, add our three new dependencies to our Maven POM file. If you are asked to import the changes, choose to do so. 

```.xml
<dependency>
    <groupId>com.okta.spring</groupId>
    <artifactId>okta-spring-boot-starter</artifactId>
    <version>2.1.5</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <scope>test</scope>
</dependency>
```

### Configure our application to use Okta OAuth2

#### Add OktaOAuth2WebSecurityConfigurerAdapter

- [ ] Using the code from __Create our HTTP security configuration class__ in *Spring Web Essentials*, Chapter 12, Securing our API, as a reference, add an Okta OAuth 2.0 web security configurer adapter to the `edu.cscc.degrees` package.

#### Configure our Spring app to validate client tokens

**For help in this section, see "Configure our Spring app to validate client tokens" in Chapter 12 of *Spring Web Essentials*.**

#### Create the app in Okta

- [ ] Log in to Okta using the developer account link provided in your welcome email.
- [ ] From the hamburger menu, choose __Applications > Applications__
- [ ] Click "Create App Integration"
- [ ] Choose "OIDC - OpenID Connect"
- [ ] In the application type section that opens below, choose "Single-Page Application"
- [ ] Change the name from "My SPA" to `Degrees at CSCC Website`.
- [ ] In the "Grant type allowed" section, check both "Authorization Code" and "Implicit."
- [ ] Scroll to the bottom, and, under "Assignments", choose "Allow everyone in your organization to access"
- [ ] Click Save.


#### Add the authorization server properties

- [ ] From the hamburger menu, choose __Security > API__. Note the following from the "Authorization Servers" tab:

![Okta-application-properties](https://user-images.githubusercontent.com/46822968/131421051-cc41b2de-2992-49ec-81cd-f64a9f1c2491.png)

- [ ] In IntelliJ, open `application.properties` under __src > main > resources__:

* add `okta.oauth2.issuer=` + the __Issuer URI__
* add `okta.oauth2.audience=` + the __Audience__

Your property file should look similar to what you see below:
```
server.port=3000
okta.oauth2.issuer=https://dev-XXXXXX.okta.com/oauth2/default
okta.oauth2.audience=api://default
```

## Test our changes with Postman

### Testing unauthenticated access

- [ ] With the Postgres database Docker container running, start or restart your Spring application

#### GET localhost:3000/public/api/menus

- [ ] Open Postman and either create a new GET request to `localhost:3000/public/api/menus` or reuse the one you added in Lab 5. It should succeed with a status 200 and a response body with the list of menu categories and items.

![postman-get-printable-menu](https://user-images.githubusercontent.com/46822968/79277993-68690d80-7e9a-11ea-803d-de725503cac7.png)

#### GET localhost:3000/api/menu/categories
- [ ] create and send a POST request to `localhost:3000/api/menu/categories`.

![unauth-get-menu-categories](https://user-images.githubusercontent.com/46822968/131423904-a41adb12-29dc-46b8-af11-e6f10135a5fa.png)

### Testing authenticated access

#### Obtain an OAuth 2.0 authorization token

- [ ] Click on the __Authorization__ tab in the Postman request.
- [ ] For __TYPE__, choose "OAuth 2.0".
- [ ] Expand "Configure New Token."
- [ ] Using the table and graphic below, as your guides, fill out the fields in the get new access token dialog box:

| Field | Value |
| ------|--------| 
| Token Name | Choose a name that will identify this token under __Available Tokens__ |
| Grant Type | Choose ``Implicit `` |
| Callback URL | From the __Sign-in redirect URIs__ in the __LOGIN__ section under the application __General__ tab in your Okta application. For example: ``http://localhost:8080/implicit/callback`` |
| Auth URL | __Issuer URI__ from __Authorization__ tab under __Server Security > API__  + /v1/authorize?nonce= + *some random text* For example: ``https://dev-123456.okta.com/oauth2/default/v1/authorize?nonce=12345`` |
| Client ID | __Client ID__ under your application name in the Applications dashboard |
| Scope | ``openid profile email`` |
| State | *enter some random text* | 
| Client Authentication | Choose ``Send as Basic Auth header`` |

![Filling out the Get New Access Token dialog box](https://user-images.githubusercontent.com/46822968/131421570-13e2b2c4-2266-4409-a2b1-b775210f2462.png)


- [ ] Click "Get New Access Token".
- [ ] Login using your Okta user name and password:

![login-window](https://user-images.githubusercontent.com/46822968/79280497-7d37a800-7e7e-11ea-8758-c98a48c2ce5e.png)

- [ ] Assuming your credentials are valid, the access token will appear in the "Manage Access Tokens" dialog box:

![manage-tokens](https://user-images.githubusercontent.com/46822968/79280654-673adf00-7ea0-11ea-9c08-dc95be96246d.png)

#### Send the authenticated request

- [ ] Click "Use Token".
- [ ] Click "Send" and verify you get a 200 status and the menu category list:

![auth-get-menu-categories](https://user-images.githubusercontent.com/46822968/131424263-80d33ad5-6689-4123-b280-3eb46cb25f22.png)

### Update our existing unit tests to authenticate with JWT

Back in section 1 above, you added new tests to verify unauthenticated requests fail without credentials.  These tests should now pass.

- [ ] Run your unit tests verifying the tests you added in section 1 now *pass* and tests you added in previous labs all *fail*.
- [ ] Update *only the tests from previous* labs (not the ones you added earlier in this lab) as follows:
    * Append `.with(jwt())` immediately after the HTTP method and URI such that   `mockMvc.perform(___(___))` becomes `mockMvc.perform(___(___).with(jwt()))`. For example:
    
![test-changes](https://user-images.githubusercontent.com/46822968/79285175-4ed0c180-7eac-11ea-870a-f48a42973a70.png)
    
- [ ] After updating *only the tests from previous* labs (not the ones you added earlier in this lab) as shown above, verify all tests now pass.

 
### Test the secure version of our web application against the secure Spring application

- [ ] Make sure your Spring app is running with the postgres profile active.
- [ ] Navigate to the directory where you stored your copy of the [Degrees Vue web application](https://github.com/ColumbusStateWorkforceInnovation/wiit-7340-degrees-vue-3-app).
- [ ] Run `git fetch` to sync your local copy with GitHub.
- [ ] Run `git checkout with-okta-security` verifying it says:
    ```
    Switched to branch 'with-okta-security'
    Your branch is up to date with 'origin/with-okta-security'.
    ```
- [ ] Using an text editor, open __src > shared > config.js__.
- [ ] Replace "enter-value-here" with values from your Okta developer console:
    ```
    // The Issuer URI from your Okta API Authorization Servers:
    export const OAUTH_ISSUER = 'enter-value-here';
    
    // From your Okta applications list, or on the "General" tab of a specific application:
    export const CLIENT_ID = 'enter-value-here';
    ```
- [ ] Save the changes you made in config.js.
- [ ] Log out of Okta if you are still logged in.
- [ ] Run `yarn install` to install new dependencies.
- [ ] Run `yarn serve` to start the application.
- [ ] Open your browser and visit [http://localhost:8080/](http://localhost:8080/)

__NOTE:__ If the page is blank, the changes you made in `config.js` are either not saved or are incorrect and must be fixed before continuing. 

It should look identical to the previous version with a new "Login" link and without the "Menu Categories" and "Menu Items" links:
 
![degrees-not-logged-in](https://user-images.githubusercontent.com/46822968/79381440-fe9b4300-7f2f-11ea-85ed-5036985ade73.png)

- [ ] Click on __Menu__ and verify the menu displays correctly while being not logged in.
- [ ] Click __Login__:

If you did not logout of Okta as directed above, the page will redisplay with the logged in version. Otherwise, you will be prompted to login:

![degrees-login](https://user-images.githubusercontent.com/46822968/79381477-0ce95f00-7f30-11ea-958e-30ca9397f975.png)

Once you are logged in, you will see the original navigation options plus "Logout":

![degrees-logged-in](https://user-images.githubusercontent.com/46822968/79381590-330eff00-7f30-11ea-8e0a-bb883e3e4641.png)
 
- [ ] Verify you can create, update, and delete menu items and categories while logged in.
- [ ] Click logout and verify the application redisplays the logged out navigation.

## Submitting your work

- [ ] Verify your tests pass.
- [ ] Commit your changes to the lab 5 branch you created.
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

**Once your pull request has been approved by your instructor *and* you have submitted this lab in Blackboard using the procedure above:**

- [ ] Merge your pull request to your main (or master) branch, so it has all the changes you made in this lab.
