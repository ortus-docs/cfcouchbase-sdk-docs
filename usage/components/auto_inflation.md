# Auto Inflation

You can annotate a CFC with `autoInflate` and CFCouchbase will be smart enough to detect the CFC's path and defined properties so it can store them in Couchbase.  When storing the component, the data will be stored as a JSON struct along with some metadata about the component.  When retrieving that document from Couchbase, a **new** component will be created with the inflated data.  This is the easiest approach as it is completely seamless and pretty fast I might say!

####Song.cfc

```coldfusion
component accessors="true" autoInflate="true"{
	property name="title"  default="";
	property name="artist" default="";
}
```

####Usage

```coldfusion
funkyChicken = new path.to.song();
funkyChicken.setTitle( "Chicken Dance" );
funkyChicken.setArtist( "Werner Thomas" );

// Pass the CFC in
couchbase.upsert( 'funkyChicken', funkyChicken );

// And get a CFC out!
birdieSong = couchbase.get( 'funkyChicken' );

writeOutput( birdieSong.gettitle() );
writeOutput( birdieSong.getArtist() );
```

####CFC data in couchbase

```javascript
{
  "data": {
    "title": "Chicken Dance",
    "artist": "Majano",
  },
  "classpath": "model.funky.Song",
  "type": "cfcouchbase-cfcdata"
}
```

As you can see, this approach will allow you to still create views about your data in Couchbase if necessary and also provide a nice way to do CFC to Couchbase to CFC translations.