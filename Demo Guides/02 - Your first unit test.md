> *StartPoint note*: Remove error handling from prod code, remove tests

> *EndPoint note*: ?Have refactored, nicer logic in prod code?

# Practical 2 - Your first unit test

## Time allocation
30 minutes

## Objectives
* Get access to your production code & test libraries within your test code
* Use `describe` and `it` blocks to create a test fixture
* Write a basic test which enforces a happy-case scenario
* Run the test suite with a green result

### Extensions
* Write tests for other happy-case scenarios, and refactor the test code if necessary
* Write edge-cases for unexpected input
* Write a test and then production code to handle invalid inputs
* Refactor the production code using tests as a safety-net

## Set-up Steps
1. Commit any outstanding changes on your Prac01 branch: `git commit -am 'Some message'`
1. Head back over to master: `git checkout master`
1. Check out the Start Point for Prac 2: `git checkout Prac02-StartPoint`
    - This references a defined tag in the repository, you must use exactly this name
1. Create a branch to make your changes on without impacting the main repo: `git checkout -b Prac02`
    - This branch is your own working space, you can call it anything you want

---

## Practical Steps
1. Use node's `require` to get access to the NameToNumberService for your test code
1. Also get acces to Chai to perform assertions
1. Add a top-level `describe` block to describe the thing we're testing
1. Add a second-level `describe` block to describe the scenario we're testing
1. Add an `it` block to create your first test case
1. Inside the `it` block, write code to set up test data and call the service
1. Add an assertion to actually verify the result
1. Run the test and see a green result

### Solutions
<details>
<summary>
Here's a step-by-step walkthrough of the practical steps, for if you get stuck :)
</summary>
<p>

1. Use node's `require` to get access to the NameToNumberService for your test code
    - `var nameToNumberService = require('../src/name-to-number-service');`
1. Also get acces to Chai to perform assertions
    - `var chai = require('chai');`
1. Add a top-level `describe` block to describe the thing we're testing
    - `describe('NameToNumber Service', function() { ... });`
1. Add a second-level `describe` block to describe the scenario we're testing
    - `describe('with 4 buckets', function() { ... });`
1. Add an `it` block to create your first test case
    - `it('should put Alice in the first', function() { ... });`
1. Inside the `it` block, write code to set up test data and call the service
    ```javascript
    var inputName = "Alice";
    var buckets = 4;
    var expectedBucket = 1;

    var actualBucket = nameToNumberService.generateNumber(inputName, buckets);
    ```
1. Add an assertion to actually verify the result
    - `actualBucket.should.equal(expectedBucket);`
1. Run the test and see a green result
    - `npm test`
</p>
</details>