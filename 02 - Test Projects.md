# Components of a test project
*Flesh out items from here into terminology*
## For all tests
- Test Runner
  - Discovers tests in your tests project
  - Runs your tests
  - Collects and reports test results
  - Gives you "hooks" for test setup, tear-down, actions on pass/failure
- Assertion Library
  - Provides an API for checking an expected result agaist the actual
  - All basically do the same thing
  - Main difference is in syntax eg `Assert.areEqual(expected, actual)` vs `actual.shouldBe.sameAs(expected)`
  - Important to choose one that gives you an error message which allows you to track down the source of the problem very quicky
    - "Expected did not match actual"
    - "String named email with value 'wrongemail@mjhilton.net' differed to expected 'matt@mjhilton.net'"
- Mocking/stubbing Library
  - Used when we need to provide fake implementations of services like databases, external APIs etc
  - Used to isolate our tests from any dependencies or changes
  - Provides objects that act like the real thing without having to code up full fake implementations
  - Usually let you record calls made to their methods, to assert upon if interaction testing
  - Usually let you set up responses to queries, provide return objects for method calls, to provide data to your tests
  - A mock or stub are really the same thing
    - Some argue that a stub object will provide default return values where a mock requires return values to be specified
    - Whatever. They're basically the same, no need for distinction
*Remove this bit*
- Spy Library
  - Javascript concept very similar to a mock or a stub. A "spy" is a fake object that records calls to methods and lets you assert on them
  - Really a subset of mocks/stubs
- Test Data Library
  - Some libraries help generate semi-sensible data for your tests
  - Helps you avoid effort of generating test data, but you lose control
  - Probably not recommended for UI tests, because you want data as realistic as possible

## For UI tests
- Driver
  - Library that gives you an API to interact with a browser
  - Simulates clicks and inspects element values to get as close to a user interaction as possible
  - Realistically, Selenium is the main Driver out there
*Kill off the DSL talk*
- DSL
  - Domain-specific language
  - A library that you code on top of the Driver, which gives you a much more natural way to interact with your app, using concepts from the app itself rather than browser elements

# Test frameworks
- Many test frameworks will provide many of these components in the one

| Framework | Test Runner | Assertion Library | Mock/Stub/Spy Library | Test Data Library | Acceptance Testing | Driver | DSL |
|-----------|-------------|-------------------|-----------------------|-------------------|--------------------|--------|-----|
| Mocha     | ✅         | ✖                 | ✖                     | ✖                | ✖                  | ✖     | ✖  |
| Karma     | ✅         | ✖                 | ✖                     | ✖                | ✖                  | 🔳    | ✖  |
| Jest      | ✅         | ✅                | ✅                    | ✖                | ✅                 | ✖     | ✖  |
| Jasmine   | ✅         | ✅                | ✅                    | ✖                | ✅                 | ✖     | ✖  |
| Cucumber-js | ✅       | ✖                 | ✖                     | ✖                | ✖                  | ✖     | ✅ |
| Chai      | ✖          | ✅                | ✖                     | ✖                | ✖                  | ✖     | ✖  |
| Unexpected | ✖         | ✅                | ✖                     | ✖                | ✖                  | ✖     | ✖  |
| Sinon     | ✖          | ✅                | ✅                    | ✖                | ✖                  | ✖     | ✖  |
| testdouble | ✖         | ✅                | ✅                    | ✖                | ✖                  | ✖     | ✖  |
| enzyme    | ✖          | ✖                 | ✅                    | ✖                | ✖                  | 🔳    | 🔳 |
| webdriver-js | ✖       | ✖                 | ✖                     | ✖                | ✖                  | ✅    | ✖  |
| PhantomJS | ✖          | ✖                 | ✖                     | ✖                | ✖                  | ✅    | ✖  |
| Nightwatch.js | ✅     | ✅                | ✖                     | ✖                | ✖                  | ✅    | ✖  |
| Protractor | ✅        | ✅                | ✖                     | ✖                | ✖                  | ✅    | ✖  |
| faker.js | ✖           | ✖                 | ✖                     | ✅               | ✖                  | ✖     | ✖  |
| Chance   | ✖           | ✖                 | ✖                     | ✅               | ✖                  | ✖     | ✖  |

- Mocha is a Javascript test runner with lots of extension points
- Karma is a Javascript test runner that executes Javascript tests using a real DOM hosted in a browser, but you can't drive the browser like Selenium can
- Jest is all things to all people
- So is Jasmine but it's 5 minutes older so whyyyy would you use it?
- Cucumber is a test-runner and a DSL-mapper, for mapping plain English to Javascript functions by convention
- Chai is an assertion library with a range of syntaxes, you pick the one you like
- Unexpected is an assertion library focused on providing really nice error messages
- Sinon is a spy/stub/mock library with a number of other utilities including an assertion library, and a time-control library for testing time-dependent functions
- TestDouble is a mocking library, with ideas quite similar to compiled languages' mocking libraries - doesn't distinguish bewteen stubs/mocks/spies. Contains an assertion capability to verify calls happened as expected.
- Enzyme is a React-focused HTML renderer for testing your React components. It provides an assertion syntax specific to testing React rendering.
- Webdriver-js is a javascript binding to Selenium WebDriver, which gives you APIs to control and read fully-running websites in a variety of browsers
- PhantomJS is a "headless" browser that provides much faster browser-driving than Selenium, but not 100% true-world
- Nightwatch.js is combined test runner, and more modern Selenium binding based on the W3C WebDriver API
- Protractor is an Angular-optimised wrapper around PhantomJS, which is also a test runner and provides and assertions
- faker.js generates realistic-looking but random data based on categories like firstname, lastname, address, email etc
- Chance generates type-based random data, and isn't intended to be real-world realistic

# Creating a test project
- Will vary depending on which framework you use
- In compiled languages, you'd usually create a separate project which specifically contains tests only, and references the "production" projects
- In Javascript land, you usually intermingle them but keep tests separated using a `/tests/` folder
- For this workshop, we're going to use a combination of libraries. Realistically, most projects will use a number of different ones rather than something like Jest which tries to do everything.
  - Test Runner: Mocha
  - Assertion Library: Chai
  - Mock/Spy Library: Sinon
  - Driver: webdriver-js

---
⌨ Practical - Set up a test framework in an existing app
---


