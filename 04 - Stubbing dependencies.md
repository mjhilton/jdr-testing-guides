# Practical 4 - Stubbing Dependencies

## Time allocation
25 minutes

## Objectives
* Identify dependencies to be controlled when testing production code
* Install a stub library & get access to it in your test code
* Learn how to stub a method on a dependency and control its return value
* Write a passing test using a stubbed dependency

## Extensions
* Refactor repetatives tasks out into test set-up (`beforeEach`) and tear-down (`afterEach`) hooks
* Read up on how Angular handles dependency management and how you might control dependencies in this environment. This is much more similar to how static languages such as C# and Java manage dependencies.

## Set-up steps
Same pattern as previous pracs
1. Commit any outstanding changes on your Prac03 branch: `git commit -am 'Some message'`
1. Head back over to master: `git checkout master`
1. Check out the Start Point for Prac 4: `git checkout Prac04-StartPoint`
1. Create a branch to make your changes on without impacting the main repo: `git checkout -b Prac04`

---

## Practical Steps
1. Install Sinon and add it as a dev dependency for our project
1. We need to write some tests for `pet-service.js`. Identify the dependency to be stubbed.
1. Create a new test file for `pet-service.js`
1. Get a reference to the code we're testing
1. Get references to Sinon and HttpClient
1. Add a describe block for PetService
1. Nest a describe block for getting pet information
1. Add an it block for testing the transformation of the JSON result
1. Find out what the JSON result from the external API is going to look like
1. Create a mock return result for test purposes
1. Instruct sinon to stub the `get` method on `httpClient`, and resolve its promise with our fake result
1. Call the service and assert on the result
1. Instruct sinon to release the stub and restore the original functionality
1. Run the test and see a fast, green result
1. Ensure that the actual `httpClient.get` method isn't being called

### Solutions
<details>
<summary>
Here's a step-by-step walkthrough of the practical steps, for if you get stuck :)
</summary>
<p>

1. Install Sinon and add it as a dev dependency for our project
    - `npm install sinon --save-dev`
1. We need to write some tests for `pet-service.js`. Identify the dependency to be stubbed.
    - At the top of `pet-service.js`, it requires `http-client.js`
    - `pet-service` uses `http-client` makes a call off to an external API
    - To test that quickly and reliably we need to stub the `http-client`
1. Create a new test file for `pet-service.js`
    - Create a new file `test\pet-service-tests.js`
1. Get a reference to the code we're testing
    - `var petService = require('../src/pet-service');`
1. Get references to Sinon and HttpClient
    - `var sinon = require('sinon');`
    - `var httpClient = require('../src/http-client');`
1. Add a describe block for PetService
    - `describe('PetService', function() { ... });`
1. Nest a describe block for getting pet information
    - `describe('when retrieving pet details', function() { ... });`
1. Add an it block for testing the transformation of the JSON result
    - `it('returns a nicely consumable JSON result', function() { ... });`
1. Find out what the JSON result from the external API is going to look like
    - Get `https://juniordev-refactor.azurewebsites.net/api/Pets?petName=Beaglier`
    - Inspect the structure of the JSON result
1. Create a mock return result for test purposes
    ```javascript
    var externalApiJsonResult = {
        "AnimalVariety": "Dog",
        "AnimalInformation": "Beagliers are beautiful",
        "ReferenceSource": "https://en.wikipedia.org/wiki/Beaglier",
        "Base64EncodedVisualDepiction": "base64dog"
    };
    ```
1. Instruct sinon to stub the `get` method on `httpClient`, and resolve its promise with our fake result
    - `sinon.stub(httpClient, 'get');`
    - `httpClient.get.resolves(externalApiJsonResult);`
1. Call the service and assert on the result
    ```javascript
    return petService.getPetDetails("Beaglier")
        .then(result => {
            result.should.have.property("info").which.equals("Beagliers are beautiful");
            result.should.have.property("source").which.equals("https://en.wikipedia.org/wiki/Beaglier");
            result.should.have.property("image").which.equals("base64dog");
        });
    ```
1. Instruct sinon to release the stub and restore the original functionality
    - `httpClient.get.restore();`
1. Run the test and see a fast, green result
1. Ensure that the actual `httpClient.get` method isn't being called
    - You could set a breakpoint in the actual `get` method and debug, or add a `console.log` entry to the actual `get` method to ensure nothing is logged in the console during the test run
</p>
</details>