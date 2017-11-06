# UI Tests
- Run against a fully integrated system and drive the UI just like a user would
- Scenario set up (arrange) is done via the UI
- Executing actions (act) is done via the UI
- Checking the results (assert) is  done via the UI
- Incredibly slow to run vs unit or integration tests
- When done poorly, can be extremely brittle and unrealiable

# Things you need to be able to think about
- How do you ensure that different tests don't conflict with each other?
- How to do get the most confidence possible to justify the cost of UI tests?
- What assumed data do you need to set up before you can start?
  - User accounts
  - Existing data (eg bookings, orders, invoices - whatever your app is about)
- How do you make this manageable across your UI test suite?

# Driving the UI
- Selenium WebDriver is the main framework used to run UI tests
  - Lets you choose which browser you want to run your tests in (Chrome, Firefox, IE, PhantomJS)
  - Can talk to it from practically any language official "bindings" (Javascript, Ruby, Python, C#, Java) plus many unofficial ones
  - Use a code API to interact with elements on the page
  - There's also Selenium Studio, which is a record-and-play-back app for automating manual regression tests. Doesn't really integrate into build processes well.
- Though it's widely used, Selenium is also buggy and unreliable
- Many libraries have been built to "wrap" Selenium and make it easier to interact with
  - Protractor is one you might know: it wraps Selenium to drive the browser, and gives you an AngularJS-friendly syntax to work with
- What does a UI test look like? A very basic one might be something like this:
  ```
  // Get a reference to Selenium Builder and Accessor via WebDriverJS bindings
  var Builder = require('selenium-webdriver').Builder;
  var By = require('selenium-webdriver').By;
  var until = require('selenium-webdriver').until;

  // Builder is our way of spinning up a Selenium server and browser:
  var driver = new Builder()
    .forBrowser('firefox')
    .build();

  // The driver is what we interact with to command and query the browser:
  driver.get('https://www.google.com.au/');

  // We use the By class to generate locators for specific elements:
  var googleQueryBox = driver.findElement(By.name('q'));
  var buttons = driver.findElements(By.type('button'));

  // You can also interact with elements:
  googleQueryBox.sendKeys('JuniorDev Refactor');
  buttons[0].click();
  driver.wait(until.titleIs('JuniorDev Refactor - Google Search'), 1000);

  // Then clean up afterwards
  driver.quit();
  ```   

# Testing against a live system
- You usually want a fully-fledged environment to run your UI tests against, because any environment differences could hide bugs that won't show up until you're in production
- Some companies will even run their tests *in* production. Requires:
  - Team maturity
  - Risk acceptance
  - High level of engineering around tenancy, data segregation etc
- Are you going to test against an environment that people will use too? How do you avoid data conflicts? 

# Test reliability
- Everything in testing is about controlling inputs, performing actions and asserting in outputs
- For your tests to be reliable, they need to be written in a way that things aren't changing underneath them
