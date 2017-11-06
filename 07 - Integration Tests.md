# Appropriate boundaries
- What even is an integration test?
- Boundaries between unit and integration tests often blurry
- Sometimes the right "unit" for a unit test isn't a pure function, but might be something that has multiple code components interacting/integrating
- I define integration tests as those that have a more realistic level of system integration than unit tests: invovles realistic systems that you'd mock/stub in a unit test
- Entry point is still code-level
- Might be calling a controller action directly, with a real database or persistence layer underneath

# Managing test data
- Can/should you hit the database in an integration test?
  - You can, and this is where we run into increased complexity.
- Remember that for tests to be valid and unfragile, we need to be able to control the inputs
- Potential for state conflicts across tests

## Managing state between tests
- Every test is responsible for setting up its own state
- Be careful with assumptions about state of the system: unless the test explicitly sets it up, that's a potential area for hidden/difficult to diagnose test failures
- Campsite rule: tests shouldn't leave the campsite messy. 
  - Each test is also responsible for cleaning up any data or state it creates.
- All of this state management makes integration tests:
  - More expensive to write: lots more to think about, more data to set up
  - More expensive to maintain: changes to state assumptions can invalidate tests, require them to be updated
  - More expensive to run: each test will run a lot slower due to all the calls involved to set up and tear down test data
  - More fragile: dependencies might have unexpected or transient failures

## Set-up & tear-down
- Most test frameworks have concepts of set-up and tear-down, or code that can run before every test and after every test
- In Javascript frameworks, that's usually via `beforeEach` and `afterEach` functions:
    ```
    describe('Editing an existing user', function() {
        var controller = new signUpController(fakeUserService, fakeEmailSender);
        var user;

        beforeEach(function() {
            user = controller.signUp('Matt Hilton', 'matt@mjhilton.net');
        });      
        
        describe('When updating a user's email address', function() {
            before(function() {
                controller.updateUserEmail(user.Id, 'newaddress@mjhilton.net');
            });
            
            it('Should send a confirmation email', function() {
                fakeEmailSender.assertWasCalled('sendToAddress').withArgs('matt@mjhilton.net', 'JuniorDev email changed!', 'Hello Matt, your email was updated to newaddress@mjhilton.net. If you didn't do this, please get in touch asap.');
            });

            it('Should update the email address on file', function() {
                var updatedUser = controller.getUser(user.id);
                assert.eq('newaddress@mjhilton.net', updatedUser.email);
            });
        });       

        afterEach(function() {
            controller.deleteUser(user);
        });
    });
    ```
- 

# How much to test in one go?