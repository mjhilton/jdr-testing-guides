# Spies - Interaction Testing
`git checkout master`

`git checkout Prac06-StartPoint -b Prac06`

1. Spies let you record calls to a fake object/method, and perform assertions on the call: how many times, which arguments etc
1. Works very similarly to stubbing, but will allow the actual method to be called
1. Add a new describe block, with a test that spies on the httpClient get method instead of stubbing it out
1. Once it's spied on, you can access spy methods/properties like `called`, `calledOnce`, and `args`, and create assertions from them
1. In sinon, the `spy.args` object is an array of calls to the spy. Each call contains the arguments that were passed with the call. So you can access `spy[callNumber][argIndex]`
1. Add some assertions against the call to `httpClient.get` and the arguments passed through.
1. Surprise! Stubs in Sinon also provide a Spy interface, so you can both control the response of a method, and assert on the way it was called
1. **Warning**: Be very careful of this kind of testing: the more specific you are about *how* interactions happen, the more difficult it is to refactor your code. Always focus on the outcome or output, and test interactions with the least amount of specificity needed to be confident the system is working.