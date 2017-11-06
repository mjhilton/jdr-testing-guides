# Spies - Interaction Testing

1. Spies let you record calls to a fake object/method, and perform assertions on the call: how many times, which arguments etc
1. Works very similarly to stubbing, but will allow the actual method to be called
1. Add a new describe block, with a test that spies on the httpClient get method instead of stubbing it out
1. Once it's spied on, you can access spy methods/properties like `called`, `calledOnce`, and `args`, and create assertions from them
1. In sinon, the `spy.args` object is an array of calls to the spy. Each call contains the arguments that were passed with the call. So you can access `spy[callNumber][argIndex]`
1. Add some assertions against the call to `httpClient.get` and the arguments passed through.
