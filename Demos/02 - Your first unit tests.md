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

## Edge case
1. 