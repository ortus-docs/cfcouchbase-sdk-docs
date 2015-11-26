#Binary

If you pass a CFC instance in that has no properties and no `$serialize()` method, CFCouchbase will use ColdFusion's `objectSave()` to turn the component into binary and it will be saved as a base64-encoded string.