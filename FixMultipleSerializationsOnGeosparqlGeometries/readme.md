# Fix Multiple Serializations on Geosparql Geometries (FiMuSGG)
FiMuSGG is a simple library or executable to fix multiple serializations on geosparql geometries.

### Compile
##### Pre-requisites
* Java 1.7 or later. See: http://www.oracle.com/technetwork/es/java/javase/downloads/index.html
* Maven 3.3.3 or later. See: https://maven.apache.org/download.cgi

Go to to project folder and execute the next cmd or bash command:
```sh
 mvn clean install
```
When finished, Maven will be generated a 'target' folder.
### Use as library
When build finished include in your project the next jar:
> target/FixMultipleSerializationsOnGeosparqlGeometries-VERSION.jar

You need a external libraries:
* Jena-ARQ
* Jena-Core 

You can found it here: https://jena.apache.org/

Add the next import in your own class:
> import es.upm.oeg.FixedMultipleSerializationsOnGeosparqlGeometries.FixMultipleSerialization;

Example code (for use the library):
> List<String> wktGeometries = new ArrayList<String>();
>
> String toTest = "POLYGON((2 3),(4 5),(6 7))";
>
> wktGeometries.add(toTest);
>
> toTest = "POLYGON((2 1,5 6))";
>
> wktGeometries.add(toTest);
>
> toTest = "GeometryCollection(Polygon((2 4,   3 4)),Polygon((9 10,7 8)))";
>
> wktGeometries.add(toTest);
>
> String fixed = FixMultipleSerialization.getSerializationOfWKTGeometries(wktGeometries);
>
> System.out.println(fixed);

The input param of function is a List<String> with all serialization (wkt) of a single geometry.

In this example the printed value is: 
> MultiPolygon(((2 3),(4 5),(6 7)),((2 1,5 6)),((2 4,3 4)),((9 10,7 8)))

### Use as executable
Go to target folder and execute (VERSION and FILE.TTL need to be replaced):
```sh
java -Xmx2048m -Xms1024m -jar FixMultipleSerializationsOnGeosparqlGeometries-VERSION-jar-with-dependencies.jar FILE.TTL [-simpleGeo | -multipleGeo]
```
If you does not put second parameter default is simpleGeo.    
simpleGeo parameter will do the union of all WKT serializations in the original geometry and remove original WKT serializations.  
multipleGeo parameter will create 1 geometry for each WKT serialization. New geometry URI is created with "originalUri_NUMBER" URI(NUMBER is a sequential integer start by 0). All original properties (of initial geometry) is added to the new geometry except rdf:type and geosparql:asWKT. Old "?resource geosparql:hasGeometry \<oldGeometry\>" and "\<oldgeometry\> ?p ?o" will be removed.  
When the app finished, a FILE_fixed.ttl will be generated (in the same folder of input file).


## Version
0.0.1
