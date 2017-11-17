# Practical 6 - UI Testing

## Objectives
* Install WebDriver and ChromeDriver, and get access to them in test code
* Spin up a web browser pointing to the system
* Use locators to assert on expected page text
* Use locators to navigate to a new page

## Extensions
* Use locators to fill form values
* Use locators to submit a form
* Write test cases for happy-path functionality

## Set-up Steps
Same pattern as previous pracs
1. Commit any outstanding changes on your Prac02 branch: `git commit -am 'Some message'`
1. Head back over to master: `git checkout master`
1. Check out the Start Point for Prac 3: `git checkout Prac06-StartPoint`
1. Create a branch to make your changes on without impacting the main repo: `git checkout -b Prac06`

---

## Practical Steps
1. Add `selenium-webdriver` and `chromedriver` as dev dependencies
1. Create a new `automated-ui` folder
1. Create new `package.json` and `launch.json` entry points for our UI tests
1. Create a new test file for our UI tests
1. Add a reference to our app to ensure it gets spun up
1. Get access to `chromedriver` and `selenium-webdriver` in your test file
1. Get a reference to webdriver's `By` and `until` utilities
1. Ensure your tests start a Chrome instance and navigate to the app homepage before they run
1. Add a new test that asserts the homepage has today's date on it
1. Add another test that navigates to the Random page and requests a random pet match, and asserts that the correct text is displayed

### Solutions
<details>
<summary>
Here's a step-by-step walkthrough of the practical steps, for if you get stuck :)
</summary>
<p>

1. Add `selenium-webdriver` and `chromedriver` as dev dependencies
    - `npm install selenium-webdriver --save-dev`
    - `npm install chromedriver --save-dev`
1. Create a new `automated-ui` folder
    - New folder `test/automated-ui`
1. Create new `package.json` and `launch.json` entry points for our UI tests
    - Follow equivalent steps from the Integration Testing prac for details
1. Create a new test file for our UI tests
    - New file `test/automated-ui/browser-tests.js`
1. Add a reference to our app to ensure it gets spun up
    - `var app = require('../../app');`
1. Get access to `chromedriver` and `selenium-webdriver` in your test file
    - `var chromedriver = require('chromedriver');`
    - `var webdriver = require('selenium-webdriver');`
1. Get a reference to webdriver's `By` and `until` utilities
    - `var by = webdriver.By;`
    - `var until = webdriver.until;`
1. Ensure your tests start a Chrome instance and navigate to the app homepage before they run
    ```javascript
    var driver;
    before("Start Chrome and navigate to homepage", function() {
        driver = new webdriver.Builder()
            .forBrowser('chrome')
            .build();

        return driver.get("http://localhost:4000/");
    });
    ```
1. Add a new test that asserts the homepage has today's date on it
    ```javascript
    describe("Spinning up our app", function() {
        it("should be greeted with today's date", function() {
            return driver.findElement(by.id("todaysDate"))
                .then((element) => element.getText())
                .then((dateText) => dateText.should.equal(new Date().toDateString()));
        });
    });
    ```
1. Add another test that navigates to the Random page and requests a random pet match
    ```javascript
    describe("Randomiser", function() {
        describe("when randomly matching a pet", function() {
            beforeEach(function() {
                return driver.findElement(by.name("random"))
                    .then(m => m.click())
                    .then(() => driver.findElement(by.tagName("button")))
                    .then(button => button.click())
                    .then(() => driver.findElement(by.name("petInfo")))
                    .then(petInfo => driver.wait(until.elementIsVisible(petInfo)))
            });

            it("should find a match", function() {
                return driver.findElement(by.name("matchResult"))
                    .then(mr => driver.wait(until.elementIsVisible(mr)))
                    .then(mr => mr.getText())
                    .then(mrText => mrText.should.contain("Your perfect pet is a"));
            });
        });
    });
    ```
</p>

## Extenion Steps
1. Write tests for the MatchMaker functionality
1. Write tests for the History functionality