# ColdBox Module (Recommended)
The CFCouchbase SDK is already a ColdBox Module. So if you are building a ColdBox MVC application you can install the SDK with CommandBox via: `box install cfcouchbase`, it will be installed in your `modules` directory, create a mapping called `cfcouchbase` for you and create all the WireBox binders for you.  If you are not using ColdBox, then you will have to use the longer installation methods below.

## WireBox Mappings
The following objects are already mapped in the module edition:

* `CouchbaseConfig` maps to `cfcouchbase.config.CouchbaseConfig`
* `CouchbaseClient@cfcouchbase` maps to `cfcouchbase.CouchbaseClient`

## ColdBox Settings
You can configure the SDK by creating a `couchbase` structure in your `ColdBox.cfc configuration file.  This structure can contain any setting from the `CouchbaseConfig` object and ColdBox will configure the `CouchbaseClient` for you.

```
couchbase = {
    servers 	= "http://127.0.0.1:8091",
    bucketname 	= "default",
    viewTimeout	= "1000"
};
```