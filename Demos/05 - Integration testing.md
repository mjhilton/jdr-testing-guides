## Testing a real running system
1. First thing we need to do is be able to require a running instance of the server. Add a `module.exports = app` at the end of `server.js`
1. Because these tests are more labour intensive, you really want to be able to run them separately. Create a new script in your package.json called `int-test`. We're going to set it up to run mocha but pointing to a different test directory:
    - Linux/OSX: `NODE_ENV=test mocha --timeout 10000 **/integration_test/*.js`
    - Windows: `SET NODE_ENV=test& mocha --timeout 10000 **/integration_test/*.js`
1. Firstly we set the NODE_ENV variable to "test" so our app can distinguish which database it needs to hit. Check the `./config/` directory to see the different config modes our app has.
1. Then we spin up mocha, give each test a bit more time to run because slow, and give a path-spec specific to integration tests
1. We can run these tests at the commandline with `npm run-script int-test` (Run-script because it's a non-standard script name)
1. You can also add a separate vscode config:
    ```
    {
        "type": "node",
        "request": "launch",
        "name": "Integration Tests",
        "program": "${workspaceFolder}/node_modules/mocha/bin/_mocha",
        "args": [
            "-u",
            "tdd",
            "--timeout",
            "999999",
            "--colors",
            "${workspaceFolder}/integration_test"
        ],
        "env": {
            "NODE_ENV": "test"
        },
        "internalConsoleOptions": "openOnSessionStart"
    }
    ```
1. Add a test file under your `integration_test` folder, and require an instance of your server. When you run the tests, you should see your server spin up and listen on a test port.

## Writing some integration tests
`git checkout master`

`git checkout Prac05-MidPoint -b Prac05`

1. Add references to `httpClient`, `dataService`, and add a `chai.should()`
1. Create a new test that requests the `/api/salaries/` API with a `Junior Developer` parameter
1. You should be able to write an assertion against the results
1. Someone added some functionality that logs all requests to the salaries endpoint, so let's add a second `it` block to test the number of items in the database
    - `db.getKeys()` gives you an array of keys, which we should be able to expect has only 1 entry after our test calls the API once
1. Works the first time but fails thereafter; we need to make sure to clean up after ourselves. Leave the system in the same state we found it.