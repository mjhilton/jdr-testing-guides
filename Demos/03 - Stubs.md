# Stubs - Controlling Dependencies
*App now contains two new logic files: HttpClient (Promise-based wrapper for HTTP requests) and SalaryService, which makes requests to a Salary service based on job name*

`git checkout master`

`git checkout Prac03-StartPoint -b Prac03`

## Testing the Salary Service
1. Have a look at the newly implemented code in `salary-service.js`
1. We also have `http-client.js`, a simple wrapper around lower-level HTTP libraries allowing us to use a promise syntax in pulling results from an API.
1. Check out the logic in Salary Service: looks like we need some tests
1. Add a new `salary-service-tests.js` file
1. Grab an instance of the service using `var salaryService = require('../src/salary-service')`
1. Create a test which calls the Salary API.
1. Let's add an initial assertion to the result of the salary and see how it compares to reality: try $30000 ;)
1. We can see from the console output that the HTTP request was made, and our test passes! Nice guess on the salary.
1. Let's debug through the Service logic to see what's going on under there.
1. Huh? The debugger won't break into the code...
1. When working with promises/testing async code, you need to pass the final promise back to mocha so it can let it complete
1. Make your test return the promise: `return salaryService.getSalary("Junior Developer").then...;`, now re-run it
    ```
    describe("Salary Service", function() {
        it("says Junior Devs get an ultra-low salary :'(", function() {
            return salaryService.getSalary("Junior Developer").then(result => {
                result.salary.should.equal(30000);
            });
        });
    });
    ```
1. Boom, we get the log message, and our debugging will also work now too.
1. Note that Mocha gives us a warning that this test takes too long to run - see the timing info in red after the test name
1. Also note: before, it looked like the test was passing, even though it wasn't. The assertion within the promise completion was never actually running, so it didn't get a chance to fail. Turns out our guess wasn't that good after all.

## Stubbing dependencies
1. We want to ensure our tests are fast and consistent/reliable: test our code, not external APIs
1. We need a way to control the return value of the call to the Salary API
1. We'll be using Sinon as our stubbing library: `npm install sinon --save-dev`
1. We need two things: a reference to Sinon, and a reference to the library we want to stub the results of. Go ahead and require both Sinon and the HttpClient at the top of the test file.
1. What we want to achieve: control the return result of a call to the API, so we can specify the returned salary value:
    ```javascript
    describe("Salary service without hitting the real API", function() {
        beforeEach(function() {
            sinon.stub(httpClient, "get");
        });

        afterEach(function() {
            httpClient.get.restore();
        });

        it('Should run really quickly!', function() {
            var expectedResult = {
                job: "Junior Developer",
                salary: 70000
            }
            httpClient.get.resolves(JSON.stringify(expectedResult));

            return salaryService.getSalary("Junior Developer").then(result => {
                result.salary.should.equal(35000);
            });
        });
    });
    ```
1. Set up Sinon to stub HttpClient's `get` method
1. In the test, set up an expected return value
1. Use the `resolves` method to set `get`'s promise resolution: `httpClient.get.resolves(value)`
1. Make your assertion, then call `httpClient.get.restore()` to un-stub the method
1. If you're writing multiple tests that stub the same methods, use `beforeEach` and `afterEach` to do the stubbing and restoring
1. You don't actually want to hit the API in unit tests, so let's remove the slow one and keep our test suite running nice and quickly.

`git merge Prac03-Midpoint`

1. Now use your magical stubbing powers to write some actual tests for the logic contained in the SalaryService: ensure that Junior and Grad salaries are half what's returned from the API, where others are the full value.
