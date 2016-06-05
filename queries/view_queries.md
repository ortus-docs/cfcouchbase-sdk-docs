#View Queries

The minimum information you need to execute a view query is the name of the **design document** and the **view** you wish you use.

```coldfusion
results = client.query( designDocumentName='beer', viewName='brewery_beers' );

for( var result in results ) {
	writeOutput( result.document.name );
	writeOutput( '<br>' );
}
```

