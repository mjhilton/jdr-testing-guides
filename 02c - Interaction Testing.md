# Interaction testing
- A style of unit test that, rather than checking the output of a function, tests the way pieces of code interact with each other
- Fragile
- Uses mocks/fakes/spies to record calls that happened, and later assert against the calls or values passed to them
- Example code that might be interaction tested:
    ```
    function signUpController() {
        var userService = new UserService();
        var emailSender = new EmailSender();

        function signUp(fullName, emailAddress) {
            var firstName = fullName.split(' ')[0];
            var lastName = fullName.split(' ')[1];
            
            var user = userService.createNewUser(emailAddress, firstName, lastName);

            var emailSubject = 'JuniorDev sign up successful!';
            var emailBody = 'Hello ' + firstName + ', welcome to the JuniorDev community!. Your user ID is ' + user.id + '.';
            emailSender.sendToUser(user, emailSubject, emailBody);
        };

        return {
            signUp: signUp
        };
    }
    ```
- Firstly, dependencies would need to be isolated out:
    ```
    function signUpController(userService, emailSender) {
        function signUp(fullName, emailAddress) {
            var firstName = fullName.split(' ')[0];
            var lastName = fullName.split(' ')[1];
            
            var user = userService.createNewUser(emailAddress, firstName, lastName);

            var emailSubject = 'JuniorDev sign up successful!';
            var emailBody = 'Hello ' + firstName + ', welcome to the JuniorDev community! Your user ID is ' + user.id + '.';
            emailSender.sendToUser(id, emailSubject, emailBody);
        };

        return {
            signUp: signUp
        };
    }
    ```
- Then you could write a test that either used hand-rolled spies, or spies from a mocking library, to record the values of calls:
    ```
    describe('Signing up a new user', function() {
        // Arrange
        // Create some fake versions of userService and emailSender that track calls/arguments
        var controller = new signUpController(fakeUserService, fakeEmailSender);
        
        // Act
        controller.signUp('Matt Hilton', 'matt@mjhilton.net');

        // Assert
        it('Should create a new user', function() {
            fakeUserService.assertWasCalled('createNewUser').withArgs('matt@mjhilton.net', 'Matt', 'Hilton');
        });

        it('Should send an email to the new user', function() {
            fakeEmailSender.assertWasCalled('sendToUser').withArgs(123, 'JuniorDev sign up successful!', 'Hello Matt, welcome to the JuniorDev community! Your user ID is 123.');
        });
    });
    ```
- These kinds of tests are really fragile
  - They're so closely tied to the implementation of the code that usually they don't provide much value
  - Testing lots of different things
  - Results from one call tested as inputs to another
- Avoid them wherever you can