# Why write tests?
- Give you confidence
- Ability to safely refactor code
- Assist you to design decoupled code
- Reduce development time

# Types of tests
- Three main types of test: unit, integration and automated UI

## Unit tests
- Test that small parts of your logic work as expected
- Test only one unit of your code at a time
- Tests algorithmic parts of your code

### Pros
- (Should be) Really fast to write
- (Should be) Really fast to execute. +100ms is bad.
- Validate algorithmic code without having to spin up the whole system to manually test
- Non-modular code is hard to test; unit testing encourages you to design small isolated components

### Cons
- (Can be) Very closely tied to implementation
- (Can be) Expensive to maintain
- Low confidence the system will actually work
- Nightmare to write good tests for non-modular code
- Possible to waste lots of time setting up test data and scenarios
- Mocking/stubbing out external dependencies is hard

## Integration tests
- Test multiple modules of your code to ensure they work together as expected
- Potentially test additional dependencies internal to the system such as database, message processors etc

### Pros
- (Should) Give you more confidence than unit tests, that the system will actually work as expected
- (Should be) More resilient to change, because you test and assert the system more like a black-box
- (Should be) More maintainable and valuable in the long run

### Cons
- (Can be) More fragile because of external dependencies
- Take longer to write than unit tests
- Much slower to run than unit tests
- Potentially need to manage test data

## Automated UI tests
- Also called "UI tests", "Selenium tests" or "Automation tests"
- Runs on a fully integrated system: all databases, external services etc
- Driven and asserted via the UI, just as if a real user was using the system

### Pros
- Very high confidence the system will actually work as intended & achieve business goals
- Force you to think from the business perspective and not miss things because bogged down in technical concerns
- Can highlight issues that other test types might not, such as concurrency problems, state problems etc.

### Cons
- Extremely slow to execute
- Take the longest to write
- (Can be) Most prone to error and transient failure if not well written
- (Can be) Significant investment to maintain

# Test-driven Development (TDD)
- A way of designing your code where you "drive" your design by writing tests first
- Intended to force you to write more decoupled, isolated code, because this is easier to write tests for
- Encourages you to think about edge cases earlier
- Encourages you to think about code contracts and boundaries earlier
- Ensures you actually write tests, because if you write the functionality first and then tests at the end, inevitable time pressures get in the way

## Red-green-refactor loop
1. (Red) Write a failing test
2. (Green) Write the simplest code possible to make the test, and all others in the suite, pass
3. (Refactor) Revisit your code and clean it up, while ensuring all tests still pass
4. Repeat with another test

## How to TDD
- Start with your business or functional requirements
- Add edge cases
- Continue until you've got 80% confidence the thing will work
- Spin up the system and test it as an actual user
- Ensure you've actually met the acceptance criteria!

### Considerations
- It's very very easy to get stuck at the technical level when working this way, and forget the business purpose
- You'll generate a shitload of tests
  - Some of these tests may only be valuable for the design stage
  - When you zoom out a bit, tests will likely be quite closely tied to implementation
- Always make sure you're stepping back every few hours and revisiting the business goals/acceptance criteria

----

----