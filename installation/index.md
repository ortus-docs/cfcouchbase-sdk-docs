# Installation

CFCouchbase ships with the following contents:

- `/cfcouchbase` - This is the actual SDK code
- `/apidocs` - The API docs that show you the FULL list of SDK methods and their arguments (even ones not covered here). There is also an [Online Version] as well.
- `/samples` - A collection of sample apps that use the "beer-sample" bucket.  
    - `/beer-brewery-manager` - Showcases view creation, and basic CRUD operations utilizing native CF data structures
    - `/beer-brewery-manager-oo` - Same as above but with OO design patterns.  Showcases Couchbase CFC inflation
    - `/beer-brewery-manager-mvc` - Same as above but as an MVC application. Requires a /coldbox mapping on your server.
    - `/coldbox-sample` - A sample ColdBox application that shows loading CFCouchbase via a module. Requires a /coldbox mapping. 

> **Hint** The API Docs also have descriptions and code samples for every method. They are a must read as well!   


## System Requirements
- Lucee 4.5+
- ColdFusion 10+

## CommandBox Installation

The easiest way to get started with CFCouchbase is to use [CommandBox CLI](https://www.ortussolutions.com/products/commandbox) to install in any directory:

```bash
box install cfcouchbase	
```

This approach can be used for both ColdBox and non-ColdBox applications.

