# Practical 3 - Debugging Test Code

## Time allocation
10 minutes

## Objectives
* Learn how to debug test code
* Set up VSCode to debug your code
* Debug a failing test and get it passing

### Extensions
None

## Set-up steps
Same pattern as previous pracs
1. Commit any outstanding changes on your Prac02 branch: `git commit -am 'Some message'`
1. Head back over to master: `git checkout master`
1. Check out the Start Point for Prac 3: `git checkout Prac03-StartPoint`
1. Create a branch to make your changes on without impacting the main repo: `git checkout -b Prac03`

---

## Practical Steps
1. Run the test suite
1. Add a new Launch Configuration to your project in VSCode, to launch Mocha with the debugger attached
1. Set a breakpoint in an existing failing test
1. Debug the test
1. Find the problems and fix them

### Solutions
<details>
<summary>
Here's a step-by-step walkthrough of the practical steps, for if you get stuck :)
</summary>
<p>

1. Run the test suite
    - `npm test`
    - Notice there are failing tests
1. Add a new Launch Configuration to your project in VSCode, to launch Mocha with the debugger attached
    - Debug Menu > Open Configurations
    - If it doesn't exist, this will create a `.vscode\launch.json` file, with a default "Launch" configuration. You might need to close a pop-up prompt to see this clearly. The default launch configuration doesn't do much right now, so delete it.
    - Press the "Add Configuration" button, bottom right
    - Select `Node.js: Mocha Tests`. Result should end up like this:
        ```javascript
        {
            "type": "node",
            "request": "launch",
            "name": "Mocha Tests",
            "program": "${workspaceFolder}/node_modules/mocha/bin/_mocha",
            "args": [
                "-u",
                "tdd",
                "--timeout",
                "999999",
                "--colors",
                "${workspaceFolder}/test"
            ],
            "internalConsoleOptions": "openOnSessionStart"
        }
        ```
1. Set a breakpoint in an existing failing test
    - Open `name-to-number-service-tests.js`
    - Set a breakpoint by clicking just to the left of the line number in the VSCode editor; a filled red circle appears
    - You can set the breakpoint either in the test code, or in the code being tested, depending on what you want to investigate
1. Debug the test
    - Open the debugging panel, (4th icon down in the left menu)
    - Ensure `Mocha Tests` is selected from the dropdown directly to the left of the green Play button
    - Press F5, or the green Play button, or Debug > Start Debugging
    - The tests will run and code execution will stop where you've set your breakpoint
    - Inspect variable values by hovering over the variable, or using the Variables panel in the left menu
    - Progress the code by pressing F10 to step to the next line, or F11 to step into a function
    - Press F5 to allow the test run to continue executing (it will stop on any subsequent breakpoints)
1. Find the problems and fix them
    - One failing test is failing due to a problem with the test code itself. The number of buckets passed to the service is `10` but should be `1`
    - One failing test is failing due to a problem with the prod code. The first condition asserting a max value for number of buckets should use a `<=`, not `<`
</p>
</details>