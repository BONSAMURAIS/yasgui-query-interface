# yasgui-query-interface

This is the [yasgui](https://triply.cc/docs/yasgui-api) query interface which is used for querying the BONSAI database.
The interface uses the yasgui interface, and is built using javascript.

## Installation

The yasgui requires no installation. The index.html file should just be placed in the `srv/import/vsp` folder.

## Inserting new queries

To insert new template queries, first create a new option for the `select` element with id `queryTemplate`.

An example could be:
```javascript
<option value="chinaActivitiesUsingSteel">Chinese Activities using Steel</option>
```

Now write the query in the function called `queryText`.
The function matches the value of the option to choose which query to display in the yasgui.

Extendign on the example, we have the query:
```javascript
  else if (queryType == "chinaActivitiesUsingSteel") {
    query = ""
    query = "PREFIX bont: <http://ontology.bonsai.uno/core#>\n" +
            "PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>\n" +
            "PREFIX om2: <http://www.ontology-of-units-of-measure.org/resource/om-2/>\n\n" +

            "SELECT ?activityType (xsd:string(sum(?value)) as ?value)\n"

    query += exiobase_graphs

    query += "WHERE\n" +
            "{\n" +
            "  ?flow a bont:Flow .\n" +
            "  ?flow bont:isInputOf ?act .\n" +
            "  ?act bont:hasLocation / rdfs:label \"CN\" .\n" +
            "  ?act bont:hasActivityType / rdfs:label ?activityType .\n" +
            "  ?flow bont:hasObjectType / rdfs:label \"Basic iron and steel and of ferro-alloys and first products thereof\" .\n" +
            "  ?flow om2:hasNumericalValue ?value .\n" +
            "  ?flow om2:hasUnit / rdfs:label ?unit .\n" +
            "}\n" +
            "GROUP BY ?activityType \n"
  }
```

The `query` variable is displayed in the yasgui query interface.
The two variables `exiobase_graphs` and `ystafdb_graphs` can be used to easily import all 
named graphs from the respective datasets. 

## Contributing
Please do not edit the scripts directly. All contributions should be via pull request. 

