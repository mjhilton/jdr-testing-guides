# What is a "unit"?
- It depends!
- Smallest portion of code that makes sense to test on its own
  - Always remember: tests are there to provide value!
- Depending on your design, it could be a function or a class
- Don't test private functions
  - These are internal details of your implementation. 
  - You'll end up with tests that are really fragile and break with every change
- Don't hit the database
- Don't hit external services (eg payment APIs, other apps' APIs)

# A basic unit test
- We've got some code that looks like this:
    ```
    function calculator() {
        var addNumbers = function(int numberOne, int numberTwo) {
            return numberOne + numberTwo;
        }

        return {
            add: addNumbers;
        }
    }
    ```

- The responsibilities of a unit test are:
  - Set up test data
  - Execute the code we're testing, and capture the result
  - Check the actual result against the expected result
- Our test might look like this:
    ```
    describe('Using the calculator to add 1 to 2', function() {
        var calculator = new calculator();
        var number1 = 1;
        var number2 = 2;
        var expectedResult = 3;

        var result = calculator.add(number1, number2);

        it('Should return 3' {
            assert.eq(expectedResult, result);
        });
    });
    ```
- When this test runs, the assertion library will verify that the actual result is the same as the expected result. If they match, it will do nothing and the test will complete without error. The test framework will report the test as passing or "green".
- If the actual doesn't match the expected, the assert call will throw an exception with details of what failed. The test framework will catch this exception and report the test as failing or "red", and provide details of the failure based on the exception thrown.

---
⌨ Practical - Write your first unit test
---

- You might write a second test for an edge case:
    ```
    describe('Using the calculator to add 0 to 0', function() {
        var calculator = new calculator();
        var number1 = 0;
        var number2 = 0;
        var expectedResult = 0;

        var result = calculator.add(number1, number2);

        it('Should return 0' {
            assert.eq(expectedResult, result);
        });
    });
    ```

# Arrange, Act, Assert
- Three very distinct sections that make your tests consistently laid out and easy to read
    - Arrange: set up any test data and expected results
    - Act: execute the unit you're testing, passing in the test data you set up in Arrange, and capturing the output/response
    - Assert: verify that the result matches your expectations
- Makes it much easier to identify repetative test code that could be refactored
- I usually keep them grouped by putting an empty line in between
- You can even add a one-line comment above each section to make it extremely clear
    ```
    describe('Using the calculator to add 0 to 0', function() {
        // Arrange
        var number1 = 0;
        var number2 = 0;
        var expectedResult = 0;
        var calculator = new calculator();

        // Act
        var result = calculator.add(number1, number2);

        // Assert
        it('Should return 0' {
            assert.eq(expectedResult, result);
        });
    });
    ```

# System-under-test/SUT
- Fancy/convention for naming the class or function you're testing
- Intended to encourage you to be thoughtful and clear about the one thing you're testing
- Some people use `sut` as their variable name for the thing they're testing:
    ```
    describe('Using the calculator to add 0 to 0', function() {
        // Arrange
        var number1 = 0;
        var number2 = 0;
        var expectedResult = 0;
        var sut = new calculator();

        // Act
        var result = sut.add(number1, number2);

        // Assert
        it('Should return 0' {
            assert.eq(expectedResult, result);
        });
    });
    ```
- I'm not a huge fan because I think it leads to less-readable tests. But if you see this pattern used, it's nothing fancy; just a naming convention

# Refactoring test code    
- A critical part of writing a maintainable test suite is valuing your test code as a first-class citizen, just like your production code. It's still something you need to maintain, so you should give it the same respect as you would any other code.
- See the duplicate code?
- You can refactor your test code too
    ```
    describe('Using the calculator to add 1 to 2', function() {
        // Arrange
        var number1 = 1;
        var number2 = 2;
        var expectedResult = 3;

        // Act + Assert
        assertCalculation(number1, number2, expectedResult);
    });

    describe('Using the calculator to add 0 to 0', function() {
        // Arrange
        var number1 = 0;
        var number2 = 0;
        var expectedResult = 0;

        // Act + Assert
        assertCalculation(number1, number2, expectedResult);
    });
    
    function assertCalculation(number1, number2, expectedResult) {
        var calculator = new calculator();
        var result = calculator.add(number1, number2);
        it('Should return ' + expectedResult, function() {
            assert.eq(expectedResult, result);
        });
    }
    ```
- Refactoring your test code can tend to break the nice clear arrange/act/assert convention, they end up split across functions
- Sometimes you might even take it as far as pulling all of the AAA steps into a test function, then your tests just become one-liners:
    ```
    describe('Using the calculator to add 1 to 2', function() {
        testAddition(1, 2, 3);
    });

    describe('Using the calculator to add 0 to 0', function() {
        testAddition(0, 0, 0);
    });
    
    function testAddition(number1, number2, expectedResult) {
        // Arrange
        var calculator = new calculator();

        // Act
        var result = calculator.add(number1, number2);

        // Assert
        it('Should return ' + expectedResult, function() {
            assert.eq(expectedResult, result);
        });
    }
    ```
- The main trade-off when refactoring test code is that you want to keep the tests explicit: it needs to be clear in the code what you're trying to test
  - Sometimes you might leave the test code a bit more verbose/duplicated, so that it's clearer what you're testing
  - Eg, in the last example it's no longer clear which are the test inputs and which is the expected result. Could easily be the test data for "3 minus 2 equals 1".