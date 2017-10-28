*App same as 01b, contains test libraries and an empty `calculator-tests.js` file*

## First unit test
1. Write a first test:
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
1. Run your test project and see what the result is!

## Edge cases
1. How about an edge case: adding 0 to 0?
1. Write another test for if a non-number is passed in. What happens? What would you expect to happen?

## Arrange act assert
1. Make sure your tests are grouped into AAA syntax
1. Add AAA comment headings if you want to be more explicit about it

## Refactoring
1. Refactor your tests to pull out common setup and assertion logic