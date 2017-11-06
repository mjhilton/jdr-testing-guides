# Naming Tests
- Organising and naming your tests well is important, because it forms part of the test output
- In a large test suite, being able to quickly identify the problem is very important to how valuable your tests are
- In non-javascript languages, tests are arranged using classes and methods, which form part of the test output
    ```
    public class CalculatorTests
    {
        public void AddingOneToTwo_ReturnsThree()
        {
            // ...
        }
    }

    // vs

    public class Tests
    {
        public void Test1() 
        {
            // ...
        }
    }
    ```
  - Might result in a line of test output saying "CalculatorTests.AddingOneToTwo_ReturnsThree() failed: expected 3, but was 0"
  - vs "Tests.Test1 failed: expected 3 but was  0"
- In javascript world, your describe/it blocks define the test output. Each nested one is chained to create the output:
    ```
    describe('Calculator', function() {
        describe('adding 1 to 2', function() {
            it('returns 3', function() {
                // ...
            })
        });
    });
    ```
  - Might result in a line of output saying "Calculator Adding 1 to 2 Returns 3 failed: expected 3, but was 0"
- Be consistent with your naming/description formats
- Ensure your test outputs will make it very clear what the problem is without too much investigating

# Testing one thing
- Every test should test only one thing
- Multiple tests/assertions in one test make it hard to figure out what's actually broken
- Depending how you structure it, in a multi-assertion test, the first failure could cover up subsequent failures
- An example of what not to do:
    ```
    decribe('Calculator', function() {
        var calc = new Calculator();

        describe('addition', function() {
            it('works correctly', function() {
                assert.eq(0, calc.add(0, 0));
                assert.eq(1, calc.add(1, 0));
                assert.eq(1, calc.add(0, 1));
                assert.eq(2, calc.add(1, 1));
                assert.eq(3, calc.add(1, 2));
                assert.eq(6, calc.add(4, 2));
            });
        });
    });
    ```
- In that example, if there's some bug that causes the first assertion to fail, you've got no idea of the scope of the problem. All could be failing, or maybe just the ones where 0 is the first parameter.
- Rewrite this as one assertion per test:
    ```
    decribe('Calculator', function() {
        var calc = new Calculator();

        describe('addition', function() {
            it('adds 0 to 0 correctly', function() {
                assert.eq(0, calc.add(0, 0));
            });

            it('adds 1 to 0 correctly', function() {
                assert.eq(1, calc.add(1, 0));
            });

            it('adds 0 to 1 correctly', function() {
                assert.eq(1, calc.add(0, 1));
            });

            it('adds 1 to 1 correctly', function() {
                assert.eq(2, calc.add(1, 1));
            });

            it('adds 1 to 2 correctly', function() {
                assert.eq(3, calc.add(1, 2));
            });

            it('adds 4 to 2 correctly', function() {
                assert.eq(6, calc.add(4, 2));
            });
        });
    });
    ```
- Think about how many edge cases you really need, though. 
- Each test should provide value. How likely is it to uncover a real problem, vs just feeling good about writing 100 tests?
- If you write tests that never fail, they're just extra code to be maintained without return on investment.

# Cleaning up after yourself
- Another reason for one assertion per test: state should never be shared between tests!
- Shared state can cause bugs in 
- Every test should perform its own setup from scratch

# Other guidance
- Never put logic in tests: then you have potential for buggy tests
    ```
    decribe('Calculator', function() {
        var calc = new Calculator();

        describe('addition', function() {
            // Nope
            assert.eq(1 * 2, calc.add(1, 2));
        });
    });
    ```
- Never put loops or control flows in test either: same reason
    ```
    decribe('Calculator', function() {
        var calc = new Calculator();

        describe('addition', function() {
            it('should be correct', function() {
                // Not a fan of this, because it encourages logic in the test
                for (var i = 0; i < 10; i++) {
                    assert.eq(i + 1, calc.add(1, i));
                }
            });
        });
    });
    ```
- For non-Javascript languages: Use data-driven/test data fixtures tests to clean up repetative test code
    ```
    public class CalculatorTests
    {
        [TestData(0, 0, 0)]
        [TestData(1, 0, 1)]
        [TestData(1, 2, 3)]
        [TestData(4, 2, 6)]
        public void Addition(int num1, int num2, int expectedResult)
        {
            var calc = new Calculator();
            Assert.AreEqual(expectedResult, calc.Add(num1, num2));
        }
    }
    ```
- Probably only place I would allow loops in a test, but still heaps of potential for bugs in the test code. Not recommended.
    ```
    decribe('Calculator', function() {
        var calc = new Calculator();

        describe('addition', function() {
            var testCases = [
                { num1: 0, num2: 0, expected: 0 },
                { num1: 1, num2: 0, expected: 1 },
                { num1: 1, num2: 2, expected: 3 }
            ];

            for (var i = 0; i < testCases.length; i++) {
                var testCase = testCases[i];
                it('should add ' + testCase.num1 + ' to ' + testCase.num2 + ' correctly', function() {
                    var result = calc.add(testCase.num1, testCase.num2);
                    assert.eq(testCase.expected, result);
                });
            }
        });
    });
    ```
