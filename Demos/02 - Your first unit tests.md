*App same as 01b, contains test libraries and an empty `calculator-tests.js` file*

`git checkout master`

`git checkout Prac02-StartPoint -b Prac02`

## First unit test
1. Write a first test:
    ```javascript
    var assert = require('assert');
    var calculator = require('../src/calculator');
    var chai = require('chai');
    chai.should();

    describe('Using the calculator to add 1 to 2', function() {
        it('Should return 3' {
            var expectedResult = 3;
            var actualResult = calculator.add(1,2)
            
            actualResult.should.equal(expectedResult);
        });
    });
    ```
1. Run your test project and see what the result is!

## Nicer assertions with Chai
1. That was using the built-in assert library from NodeJS, it wasn't using Chai at all
1. Chai gives us 3 different styles of assertion syntax: `should`, `expect`, and `assert`
    - http://chaijs.com/#content
    - Try tags `Practical-02-ChaiAssert`, `Practical-02-ChaiExpect`, `Practical-02-ChaiShould`

## Debugging our tests
1. VSCode supports debugging of all code including Javascript unit tests, but you need to tell it how to launch
1. We won't go into deep details of how this works, but VSCode will help you out with a lot of it. `Debug -> Add Configurations` will create `.vscode\launch.json` if it doesn't exist, and will prompt you to add tasks it knows about. There's one called `Node.js: Mocha Tests`. Use that.
1. Now when you press F5, the tests will launch and your Debug Console will show the outputs. You can also set breakpoints and you'll be able to debug into both the test and production code.

## Edge cases
1. How about an edge case: adding 0 to 0?
1. Write another test for if a non-number is passed in. What happens? What would you expect to happen?
1. Go ahead and add a basic test for multiply, divide and subtract

