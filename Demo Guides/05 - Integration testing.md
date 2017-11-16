# Practical 5 - Integration testing

## Time allocation
30 minutes

## Objectives
* Spin up a live copy of the system from your test code
* Write a test which calls an API, and asserts on the API result (black-box)
* Write a test which calls an API, and asserts on database contents (white-box)
* Use tear-down hooks to clean up state left behind by the test

## Extensions
* Use set-up hooks to set up expected test data for use in a test
* Write tests for all APIs exposed by the system

## Set-up steps
Same pattern as previous pracs
1. Commit any outstanding changes on your Prac04 branch: `git commit -am 'Some message'`
1. Head back over to master: `git checkout master`
1. Check out the Start Point for Prac 5: `git checkout Prac05-StartPoint`
1. Create a branch to make your changes on without impacting the main repo: `git checkout -b Prac05`

---

## Practical Steps