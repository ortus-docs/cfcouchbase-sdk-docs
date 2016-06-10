# Working with Futures

You have probably noticed that all the asyncronous operations in the SDK return a Java [OperationFuture](http://www.couchbase.com/autodocs/spymemcached-2.8.1/net/spy/memcached/internal/OperationFuture.html) object. This allows control of your application to return immediately to your code without waiting for the remote calls to complete. The future object gives you a window into whats going on and you can elect to monitor the progress on your terms-- deciding how long you're willing to wait-- or ignore it entirely in order to complete the request as quickly as possible.

The most common method is `get()`. Calling this will instruct your code to wait until th eoperation is complete before continuing. Calling `future.get()` essentially makes an ansynchronou call syncronous.

