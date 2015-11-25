There are a number of configuration options you can set for the client, but most of them can be left at their default value.  To see a full list of options, look in the `/cfcouchbase/config/CouchbaseConfig.cfc` or the user friendly [API Docs](http://apidocs.ortussolutions.com/cfcouchbase/2.0.0).
Here are some of the most common setting you will need to use:

##Common Config Settings

| Setting | Type | Default | Description |
| -- | -- | -- | -- |
| servers           | any     | 127.0.0.1:8091 | The list of servers to connect to. Can be comma-delimited list or an array. If you have more than one server in your cluster, you only need to specify one, but adding more than one will help in the event a node is down when the client connect.  |
| bucketName        | string  | default        | The name of the bucket to connect to on the cluster. This is case-sensitive |
| password          | string  |                | The optional password of the bucket. |
| caseSensitiveKeys | boolean | false          | Keys in Couchbase are case sensitive by default, the SDK by default converts all keys to lowercase, setting this to *true* will prevent this |
| dataMarshaller    | any     |                | The data marshaller to use for serializations and deserializations, please put the class path or the instance of the marshaller to use. Remember that it must implement our interface: `cfcouchbase.data.IDataMarshaller` |
