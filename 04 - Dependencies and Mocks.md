# Mocks and Substitutes
- Not all of our code is a nice self-contained algorithm
- Frequently our code has dependencies: external things it relies on to get the job done
- For example, you've got a function that relies on a service which returns currency foreign-exchange values, to do a currency conversion:
    ```
    var OzForexCurrencyService = require('OzForex');

    function forexController() {
        var currencyService = new OzForexCurrencyService();
        
        function convertUSDtoAUD(usdAmount) {
            // Today's exchange rate is 0.76
            var exchangeRate = currencyService.getExchangeRate('USD', 'AUD');

            var audAmount = usdAmount * exchangeRate;
            
            return audAmount;
        };

        return {
            convertUSDtoAUD: convertUSDtoAUD
        };
    };
    ```
- Writing a test for this code:
    ```
    describe('Converting US$10 to AU$', function() {
        // Arrange
        var controller = new forexController();
        var usdAmount = 10;
        var expectedResult = 7.6;

        // Act
        var result = controller.ConvertUSDtoAUD(usdAmount);

        // Assert
        it('Should equal AU$7.60', function() {
            assert.eq(expectedResult, result);
        });
    });
    ```
- I've tested my code, it's green and passing. Awesome! 
  - Any problems?

# Isolating your dependencies
- Problem with the original code is that the forexController is responsible for creating an instance of the currency service that it relies on
- This means that we're at the mercy of the exchange rates. As soon as the rate changes from 0.76, our test will start breaking :(
- When testing, it's important to identify dependencies in your code: things where you don't control the result/outcome
- If you want reliable test, you need to control all the inputs so you can be sure you'll get the same output

## Stubs/Mocks (Javascript)


## Dependency injection (Compiled languages)
- Pattern designed to deal with this problem, and help you write more modular code
- Don't create things you depend on: let them be passed to you by whoever's consuming your code
- Example of forexController using Dependency Injection instead:
    ```
    function forexController(currencyService) {
        function convertUSDtoAUD(usdAmount) {
            // Today's exchange rate is 0.76
            var exchangeRate = currencyService.getExchangeRate('USD', 'AUD');

            var audAmount = usdAmount * exchangeRate;
            
            return audAmount;
        };

        return {
            convertUSDtoAUD: convertUSDtoAUD
        };
    };
    ```
- Our test can now provide a different implementation of the currencyService, as long as it has the expected functions on it:
    ```
    describe('Converting US$10 to AU$', function() {
        // Arrange
        var unchangingCurrencyService = {
            getExchangeRate: function(from, to) { 
                return 0.76; 
            }
        };

        var controller = new forexController(unchangingCurrencyService);
        var usdAmount = 10;
        var expectedResult = 7.6;

        // Act
        var result = controller.ConvertUSDtoAUD(usdAmount);

        // Assert
        it('Should equal AU$7.60', function() {
            assert.eq(expectedResult, result);
        });
    });
    ```