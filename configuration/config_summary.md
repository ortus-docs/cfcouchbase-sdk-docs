# Summary

The default configuration for CFCouchbase is located in  `/cfcouchbase/config/CouchbaseConfig.cfc`.  You can create the `CouchbaseClient` with no configuration and it will connect to the **default** bucket on [127.0.0.1:8091](http://127.0.0.1:8091).  However, there are more ways to configure the Couchbase client via the `config` argument of the constructor so it can connect to your server and options of choice:

1. Pass a config structure
2. Pass a config CFC

**Note:** The config CFC is the most contained and portable way to store your configuration.




