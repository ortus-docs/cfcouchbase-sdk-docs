# Custom Component Serialization

If you really want to get extra funky and control how your components are serialized, you can fall back on our conventions.  If the CFC has a public method called `$serialize()`, it will be called and its output (must be a *string*) will be saved in Couchbase.  The CFC can also have a public method called `$deserialize( id, data )`, that if declared will be called and given the data so it can populate itself.

####CustomUser.cfc

```coldfusion
// CustomUser is an object that implements its own serialization scheme
// using pipe-delimited lists to store the data instead of JSON.  It has both
// a $serialize() and $deserialize() method to facilitate that.
component accessors="true"{

	property name="firstName";
	property name="lastName";
	property name="age";

	function $serialize(){
		// Serialize as pipe-delimited list
		return '#getFirstName()#|#getLastName()#|#getAge()#';
	}
	
	function $deserialize( ID, data ){
		// Deserialize the pipe-delimited list
		setFirstName( listGetAt( data, 1, '|' ) );
		setLastName( listGetAt( data, 2, '|' ) );
		setAge( listGetAt( data, 3, '|' ) );		
	}
}
```

####Usage

```coldfusion
user = new CustomUser();
user.setFirstName( "Brad" );
user.setLastName( "Wood" );
user.setAge( 45 );

couchbase.upsert( 'half-pipe', user );

reinflatedUser = couchbase.get( id="half-pipe", inflateTo='CustomUser' );

writeOutput( reinflatedUser.getFirstName() );
writeOutput( reinflatedUser.getLastName() );
writeOutput( reinflatedUser.getAge() );
```