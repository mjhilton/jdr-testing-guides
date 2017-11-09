# Practical 1 - Set up a test framework

## Time allocation
20 minutes

## Objectives
* Create a test project/framework for an existing app
* Install a test runner
* Set the test runner to run when `npm test` is called
* Install an assertion library
* Run the "test suite" with a green result

## Extensions
None

## Set-up steps
1. Pre-req: Visual Studio Code is installed
1. Pre-req: Git is installed
1. Clone the Demo repository: 
    - `git clone https://github.com/mjhilton/testing-workshop-demos`
1. Check out the Start Point for Prac 1: 
    - `git checkout Prac01-StartPoint`
    - This references a defined tag in the repository, you must use exactly this name
1. Create a branch to make your changes on without impacting the main repo: 
    - `git checkout -b Prac01`
    - This branch is your own working space, you can call it anything you want

---

## Practical steps
1. Install Mocha and add it as a dev dependency for our project:
    - `npm install --save-dev Mocha`
1. Install Chai and add it as a dev dependency for our project:
    - `npm install --save-dev chai`
1. Update your `package.json` so that mocha is executed when `npm test` is called
    - Open `package.json`
    - Find the `scripts` array
    - Ensure it contains a test script, which executes mocha, like this: `"test": "mocha"`
1. Create an empty test file under the `test` folder
    - Call it `caclculator-tests.js`
1. Run `npm test` and ensure the result is green/passing