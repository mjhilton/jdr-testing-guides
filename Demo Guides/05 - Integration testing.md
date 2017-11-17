# Practical 5 - Integration testing

## Time allocation
30 minutes

## Objectives
* Ensure integration tests are kept separate from unit tests
* Spin up a live copy of the system from your test code
* Write a test which calls an API, and asserts on the API result (black-box)
* Write a test which calls an API, and asserts on database contents (white-box)
* Use tear-down hooks to clean up state left behind by the test

## Extensions
* Write integration tests for all APIs exposed by the system, stubbing things if necessary :)
* Write a bash script to execute unit tests first, then integration tests

## Set-up steps
Same pattern as previous pracs
1. Commit any outstanding changes on your Prac04 branch: `git commit -am 'Some message'`
1. Head back over to master: `git checkout master`
1. Check out the Start Point for Prac 5: `git checkout Prac05-StartPoint`
1. Create a branch to make your changes on without impacting the main repo: `git checkout -b Prac05`

---

## Practical Steps
1. Push existing unit tests down into a "unit" folder within the "test" folder
    - Create a `test/unit` folder
    - Move existing tests into the new folder
    - Update any `require` references to our app's code to ensure their relative paths are still correct
    - eg `require('../src/pet-service');` will need to become `require('../../src/pet-service');`, because the test file is now one level deeper in the directory structure
1. Create a new "integration" folder under the "test" folder
    - Create a `test/integration` folder
1. Update the existing `package.json` and `launch.json` config for the new location
    - In `package.json`, update the `test` script with a pathspec: `"test": "mocha test/unit/**/*.js"`
    - In `launch.json`:
        - Update the name of the existing `Mocha Tests` configuration to `Unit Tests`
        - Update the final `args` entry to point to the new unit test folder: `"${workspaceFolder}/test/unit"`
1. Add new `package.json` and `launch.json` entries to run integration tests separately to unit tests
    - Add a new `package.json` script called `int-test` which sets the NODE_ENV variable to test, and starts the integration tests
        - Linux/OSX: `NODE_ENV=test mocha --timeout 5000 test/integration/**/*.js`
        - Windows: `SET NODE_ENV=test& mocha --timeout 5000 test/integration/**/*.js`
    - Add a new Mocha configuration to `launch.json` which points to the `integration` folder:
    ```javascript
    {
        "type": "node",
        "request": "launch",
        "name": "Integration Tests",
        "program": "${workspaceFolder}/node_modules/mocha/bin/_mocha",
        "args": [
            "-u",
            "tdd",
            "--timeout",
            "5000",
            "--colors",
            "${workspaceFolder}/test/integration"
        ],
        "env": {
            "NODE_ENV": "test"
        },
        "internalConsoleOptions": "openOnSessionStart"
    }
    ```
1. Create an integration test file
    - Add a new file `test/integration/api-tests.js`
1. Make the `app.js` exportable and get a reference to it in
    - Open `app.js`
    - At the bottom of the file, add a line to export the `app` once it's spun up: `module.exports = app;`
1. Add placeholder `describe` and `it` blocks
1. Run the integration tests and ensure the app was spun up
1. Implement a test that calls the `/api/pets/generateMatch` API and asserts on the result
    ```javascript
    describe("When requesting a dog match for Alice", function() {
        it("returns a Beaglier", function() {
            return httpClient.get("http://localhost:4000/api/pets/generateMatch?ownerName=Alice&petType=Dog")
                .then(result => {
                    result.should.exist;
                    result.should.have.property('petName').that.equals("Beaglier");
                });
        });
    });
    ```
1. Implement a test that calls the `/api/pets/random` API, and asserts on the history content directly from the database
    ```javascript
    describe("When requesting a random pet match", function() {
        it("adds a single entry to the request history", function() {
            return httpClient.get("http://localhost:4000/api/pets/random")
                .then(function() {
                    var keys = db.getKeys();
                    keys.length.should.equal(1);
                });
        });
    });
    ```
1. Add a tear-down hook to reset the app's state after each test
    - Add an `afterEach()` function which calls `db.clear()`, like this:
    ```javascript
    afterEach(function() {
        db.clear();
    });
    ```