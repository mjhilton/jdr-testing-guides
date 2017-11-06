## Getting Selenium up and running:
1. We're going to need some new packages to get this thing running
1. Add `webdriverjs` and `chromedriver` as dev dependencies
1. Create a new test file where we'll get a browser up and running, controlled by our test code
1. Chromedriver is the bridge between Selenium and Chrome. Webdriver is the bridge between your test and Selenium:
    `test.js <---Webdriver---> Selenium <---Chromedriver---> Chrome`
1. You need to require `chromedriver` (but don't need a reference to it), and you'll need to require `selenium-webdriver` and grab a reference
1. Create a basic test structure with a describe block, beforeEach/afterEach, and an it block
1. You'll need a variable to store the driver reference within the describe block, called `driver`
1. In your beforeEach, you want to start up Webdriver:
    ```
    driver = new webdriver.Builder()
        .forBrowser("chrome")
        .build();
    ```
1. In your afterEach, make sure we shut down old browser windows:
    ```
    driver.quit();
    ```
1. The webdriver api is all async, everything you do returns a promise, so we'll need to take that into consideration when designing our test
1. Create a basic test that navigates to Google and asserts the title is as expected:
    ```
    return driver.get("https://www.google.com.au")
                 .then(() => { return driver.getTitle(); }
                 .then(title => title.should.equal("Google"); );
    ```
1. Running this test, you should see Chrome pop up, navigate to Google, and close down. Hooray! First automated UI test: written :)

## Interacting with our application
1. We now need to use the same techniques from the interaction tests to ensure we have a server up and running that we can point Selenium at.
1. Its useful to split your unit tests out from your slower running tests. You might want 3 different run tasks, one for each, or just two: fast and slow