# Overview 

The CFCouchbase SDK is a CFML library for interacting with [Couchbase NoSQL Server](http://www.couchbase.com). It can be used by any CFML application or CFML framework to provide them with NoSQL, distributed caching, dynamic queries and many more capabilities. The CFCouchbase SDK wraps the Couchbase Java SDK and enhances it to provide with a nice dynamic language syntax and usability.

###Requirements

- Couchbase Server 1.8+
    - For N1QL queries Couchbase Server 4.0+ 
- Adobe ColdFusion 9.01+
- Railo 3.1+
- Lucee 4.0+

##### Couchbase SDK Version

The Couchbase SDK Version used is 2.2.6

## Capabilities 

- Lightweight, standalone library can be used with any CFML application or CFML framework
- High performance
- Asynchronous calls 
- Auto-sharding of documents evenly across cluster
- 24/7 uptime via on-the-fly node removal and rebalance operations   
- Easily configurable
- Fully-featured API includes view management and execution
- Built on the official Java SDK, but customized to take advantage of CFML
- Optimistic concurrency control (Documents are not locked by default for maximum throughput)
- Conflict management via Compare And Swap (CAS) mechanism
- Full cluster and document stats available
- Provides direct access to underlying Java SDK for advanced usage 
- Ability to create as many Couchbase clients as necessary