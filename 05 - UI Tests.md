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


# Testing against a live system


# Test reliability

