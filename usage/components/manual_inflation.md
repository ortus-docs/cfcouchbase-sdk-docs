# Manual Inflation

If you pass a CFC with properties but it does not have the `autoInflate` attribute like above, the data will still be stored as a JSON struct, but without the extra metadata about the CFC.  When retrieving this document, you will just get a struct back by default.  You can build a CFC yourself, or specify an **inflateTo** argument to instruct CFCouchbase on how to reinflate your data back to an object. You can pass in a component path (or component instance) and the SDK will instantiate that component and call its property setters to repopulate it with the data from Couchbase.

```js
// path
person = client.get(
	id = "funkyChicken",
	inflateTo = "path.to.song"
);

// instance
person = client.get(
	id = "funkyChicken",
	inflateTo = new path.to.Song()
);
```

If you need even more control such as performing dependency injection, passing constructor arguments to the CFC, or dynamically choosing the component to create, you can supply a closure that acts as a provider and produces an empty object ready to be populated.

```js
person = client.get(
	id = "funkyChicken",
	inflateTo = function( document ){
		// Let WireBox create and autowire an instance for us
		return wirebox.getInstance( 'path.to.song' );
	}
);
```

If you are getting multiple documents back from Couchbase, your **inflateTo** closure will be called once per document.

> **Note** You can even use `inflate` to when retrieving result sets, or querying Couchbase views and you'll get an array of populated CFCs!