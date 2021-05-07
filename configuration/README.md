# Configuration

The most portable method for configuring the client is to use a CFC to place your config settings in much like our other libraries such as [WireBox](https://wirebox.ortusbooks.com) and [CacheBox](https://cachebox.ortusbooks.com) allow. To do this simply create a plain CFC with a public method called `configure()`. Inside of that method, put your config settings into the variables scope of the component. The `configure()` method does not need to return any value. It will be automatically invoked by the SDK prior to the config settings being extracted from the CFC.

```javascript
component {

    function configure() {
        servers = ['http://cache1:8091','http://cache2:8091'];
        bucketName = 'myBucket';
        password = 'myPass';
    }

}
```

To use the config CFC, simply pass it into the `CouchbaseClient's` constructor.

```javascript
// You can pass an instance
couchbase = new cfcouchbase.CouchbaseClient( new path.to.config() );

// You can also pass a path to the CFC
couchbase = new cfcouchbase.CouchbaseClient( 'path.to.config' );
```

