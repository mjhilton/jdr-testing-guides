*App now contains two new logic files: HttpClient (Promise-based wrapper for HTTP requests) and SalaryService, which makes requests to a Salary service based on job name*

`git reset --hard HEAD`
`git checkout Practical-03-StartPoint`

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
1. When working with promises, you need to pass the final promise back to mocha so it can let it complete
1. Make your test return the promise: `return salaryService.getSalary("Junior Developer").then...;`, now re-run it
1. Boom, we get the log message, and our debugging will also work now too.
1. Note that Mocha gives us a warning that this test takes too long to run - see the timing info in red after the test name

1. `npm install sinon --save-dev`