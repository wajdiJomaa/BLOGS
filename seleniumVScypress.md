# Selenium vs Cypress

## Why compare selenium and cypress?

Cypress and Selenium are test automation tools used for functional testing of web applications by automating browser actions. Selenium has been a widely-used tool for years, whereas Cypress is a recently introduced tool in the test community.

## Architecture differences

Most testing tools (like Selenium) operate by running outside of the browser and executing remote commands across the network. Cypress is the exact opposite. Cypress is executed in the same run loop as your application.

Cypress also operates at the network layer by reading and altering web traffic on the fly. This enables Cypress to not only modify everything coming in and out of the browser, but also to change code that may interfere with its ability to automate the browser.    

Because Cypress operates within your application, that means it has native access to every single object. Whether it is the `window`, the `document`, a `DOM element`, your `application instance`, a `function`, a `timer`, a `service worker`, or anything else - you have access to it in your Cypress tests. There is no object serialization, there is no over-the-wire protocol - you have access to everything. Your test code can access all the same objects that your application code can.

__*** from cypress docs ***__


## Browser support

Selenium supports bigger number of web browsers

Selenium: Chrome, IE, Safari, Edge, Firefox, Opera

Cypress: Chrome, Edge, Firefox, Electron

## Programming languages

cypress is only available for javascript, so you have to learn at least the basics of js if you
want to use cypress, some basic knowledge of jquery is also helpful.

selenium supports all popular languages java, python, ruby, php...


## Setup complexity

The setup of cypress is very simple no dependencies or additional downloads required, but for
selenium we have to set up the environment and download browser-specific drivers

## Documentation 

both selenium and cypress have a nice documentation

selenium: https://www.selenium.dev/documentation/webdriver/

cypress:  https://docs.cypress.io/


## projet structure

When you execute the command `npx cypress open` cypress will create a folder called cypress
in your project and cypress.config.js file. the cypress folder has 4 subfolders:

cypress/
    - integration: the folder where you have to put the tests
    - fixtures: this folder holds the data for your tests
    - plugins: add plugins
    - support folder: write reusable methods

cypress.config.js: add customize configurations

while for selenium you will start without any folder you have to create them by your own

## Synchronus vs Asynchronus

Selenium is synchronus: that's mean that the program will run code in sequence,
by nature cypress is asynchronous, but it have an engine to execute steps in sequence, cypress will
queue all commands and then run them in order for example if we have the following code

```python 
    cy.get("  ")    
    console.log("hi")
```
The log will be printed before the the cypress command because the cy command is queued and will run after
the console.log

## Tests related to Api

let's have the following scenario: we have an api that returns a list of books each time,
we want to test the case when the api returns only one book, but the most of times the 
api will return > 1, in selenium testing this is impossible but in cypress we can mock the 
response from the api server and return one book only.


## The cypress test runner

Using cypress we can run commands using the command line, but what's really important about cypress
is the test runner, the test runner provides snapshots and screenshots before and after each cypress
command is executed, with great error logs, this is really helpful for debugging or understanding the
behavior of the test.

## Waits

In cypress you don't have to add explicit and implicit waits because cypress waits automatically for commands


## Cucumber integration 

Both cypress and selenium have support for cucumber for BDD testing.

## Testing frameworks

Selenium with java could be integrated with Junit or TestNg to add assertions and different test
annotations, cypress is used with mocha for testing and use also chai assertions

## key advantages of selenium and cypress: 

Selenium:
    
1. Provides QAs the flexibility to select the programming language of their choice like Java, Ruby, Python, etc.

2. Compatible with modern browsers like Safari, Chrome, Firefox, etc.

3. have a large community

Cypress:

1. Cypress framework captures snapshots at the time of test execution. This allows QAs or developers to hover over a specific command in the Command Log to see exactly what happened at that particular step.

2. One doesn’t need to add explicit or implicit wait commands in test scripts, unlike Selenium. Cypress waits automatically for commands and assertions.
    
3. Developers or QAs can use Spies, Stubs, and Clocks to verify and control the behavior of server responses, functions, or timers.

4. has the ability to auto-retry failed tests for a given number of time
    

## limitations of cypress and selenium

Cypress:

1. it is not allowed to navigate to multi-domains in the same test (redirects allowed)

2. It doesn’t provide support for multi-tabs or windows

3. Cypress only supports JavaScript for creating test cases

4. Cypress doesn’t provide support for browsers like Safari and IE at the moment

5. Limited support for iFrames (for newer versions of cypress they have added iframe support)


Selenium:

1. Handling page load or element load is difficult

2. Creating test cases is time-consuming

3. Difficult to set up test environment as compared to Cypress

4. cannot modify the DOM while in cypress it is possible to change attributes of elements

5. cannot access the network layer of the browser, in Cypress we can mock requests and responses

6. no built-in structure like the cypress one which is very helpful


## Conclusion

Both Cypress and Selenium are very powerful tools for ui automation testing, almost every test case
could be implemented in both frameworks, but it's possible that the implementation of some cases is possible in only one of these two. depending on what you have use selenium and cypress.


|   Cypress     |       Selenium     |    
|:-------------:|:------------------:|  
| login without interacting with UI by sending post request then storing session id in the browser storage | login requires interacting with the UI|
| only css selectors | requires using the xpath to locate some elements
| clicking hidden elements is a magical feature                                                                         | clicking hidden elements is not possible which make the automation of some tests almost impossible 
| asynchronous, sometimes makes using loops, if.. statements a little bit more complicated                              | runs synchronously                                                                                 
| recording videos is a built-in feature                                                                                | not a built-in feature                                                                              
| the test runner makes debugging much easier,  and helps to check the integrity of the test (shows all the test steps) | debugging much harder


## Date: 30/9/2022