*App same as 01b, contains test libraries and an empty `calculator-tests.js` file*

`git reset --hard HEAD`
`git checkout Practical-02-StartPoint`

## First unit test
1. Write a first test:
    ```
    var assert = require('assert');
    var calculator = require('../src/calculator');

    describe('Using the calculator to add 1 to 2', function() {
        it('Should return 3' {
            assert.equal(3, calculator.add(1, 2));
        });
    });
    ```
1. Run your test project and see what the result is!

## Nicer assertions with Chai
1. That was using the built-in assert library from NodeJS, it wasn't using Chai at all
1. Chai gives us 3 different styles of assertion syntax: `should`, `expect`, and `assert`
    - http://chaijs.com/#content
    - Try tags `Practical-02-ChaiAssert`, `Practical-02-ChaiExpect`, `Practical-02-ChaiShould`

## Edge cases
1. How about an edge case: adding 0 to 0?
1. Write another test for if a non-number is passed in. What happens? What would you expect to happen?
1. Go ahead and add a basic test for multiply, divide and subtract