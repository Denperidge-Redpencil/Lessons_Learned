# Learning.md: Notes from the SPARQL youtube series by Wouter Beek

Notes I made while watching [Wouter Beek's series on SparQL](https://www.youtube.com/watch?v=nbUYrs_wWto&list=PLaa8QYrMzXNnzY-4YVM5507iZuESWVcnU&index=1). It should be noted 

## Terms
Syntax:
- `?variable`
- `<absolute-uri>`
- `end of a tripple pattern.`
- `repeat the subject;`
- `repeat the subject and predicate,`

?s ?p ?o --> subject/predicate/object

---

| Notation     | Example                                             |
| ------------ | --------------------------------------------------- |
| Absolute IRI | `<http://www.w3.org/1999/02/22-rdf-syntax-ns#type>` |
| Relative IRI | `<type>` (requires base)                            |
| Prefixed IRI | `rdf:type` (requires prefix)                        |
| Type IRI     | `a` (only in predicate position)                    |

(Table imported from [this video](https://youtu.be/QPDRPmZ9f0w?t=644))

---

| Symbol | Notation type |
| ------ | ------------- |
| `.`    | Simple triple |
| `;`    | Predicate list|
| `,`    | Object list   |

(Table imported from [this video](https://youtu.be/Xi0rE1xczb8?t=225))

---

| Example | Datatype IRI |
| ------- | ------------ |
| false   | xsd:boolean  |
| 11      | xsd:integer  |
| 1.1     | xsd:decimal  |
| 1.1e0   | xsd:double   |
| "abc"   | xsd:string   |

`prefix xsd: <http://www.w3.org/2001/XMLSchema#>`

(Table imported from [here](https://youtu.be/M9MxKjz0KE0?t=118))

---


- ~~Okay so one thing you have to understand is that you can't look at sparql like most code where it goes line-by-line. The entire thing gets parsed at once~~
- Don't interpet it as an SQL database. Just because a Record has a property called Track, doesn't mean you can query for Tracks. 
- Also it's case sensitive! mo:Record & mo:track work. mo:record & mo:Track breaks. *(Usually, a capital means it's a type, and a non-capital means it's a predicate)*
- There are somewhat-standardised prefixes. See the [Linked Open Vocabularies vocabs page](https://lov.linkeddata.es/dataset/lov/vocabs) (for some/all of them?)

## Structure
Note: you can test out the triplydb/pokemon examples [here](https://triplydb.com/academy/-/queries/group-pattern-5/1)

Okay, the value you provide and where you put ?variables changes things
```sql
WHERE {
  ?thing rdf:type ?type .
}
```
Queries everything with rdf:type for example

### Basic spo select
```sql
# Prologue

SELECT ?s ?p ?o {  # Projection: columns
    ?s ?p ?o. # Pattern: cells/content
}
limit x  # Modifier: affects rows
```

### Filter select on object value
- ?p can be replaced with a predicate uri: ?p --> <https://triplydb.com/academy/pokemon/vocab/colour>
- ?o can be replaced with a specific object: ?0 --> "yellow"
```sql
select ?pokemon {
    ?pokemon <https://triplydb.com/academy/pokemon/vocab/colour> "yellow"
}
```


### Bind (assigning variables)
```sql
select ?pokemon ?color ? greeting {
    ?pokemon <https://triplydb.com/academy/pokemon/vocab/colour> ?color.
    bind("Hi!" as ?greeting)
}
limit 10
```

### Literal string interpretation
```sql
select ?location {
    # "literal"^^<how-to-interpet>
    bind("Point(4.8649 52.33287)"^^<http://www.opengis.net/ont/geosparql#wktLiteral> as ?location)
}
```

### Bind calculation
```
select ?pokemon ?weightKilograms ?weightPounds {
    ?pokemon <https://triplidb.com/academy/pokemon/vocab/weight> ?weightKilograms.
    bind(?weightKilograms * "2.20462"^^<http://www.w3.org/2001/XMLSchema#float> as ?weightPounds)
}
```

### Query against multiple of the same, only returning when there are multiple but allowing switching around/duplicate
```sql
select ?color ?cry ?name ?type1 ?type2 {
  ?pokemon <https://triplydb.com/academy/pokemon/vocab/colour> ?color.
  ?pokemon <https://triplydb.com/academy/pokemon/vocab/cry> ?cry.
  ?pokemon <http://www.w3.org/2000/01/rdf-schema#label> ?name.
  ?pokemon <https://triplydb.com/academy/pokemon/vocab/type> ?type1.
  ?pokemon <https://triplydb.com/academy/pokemon/vocab/type> ?type2.
}
```

### Get an object from an object of an object
```sql
select ?color ?cry ?name ?type1Name ?type2Name {
  ?pokemon <https://triplydb.com/academy/pokemon/vocab/colour> ?color.
  ?pokemon <https://triplydb.com/academy/pokemon/vocab/cry> ?cry.
  ?pokemon <http://www.w3.org/2000/01/rdf-schema#label> ?name.
  ?pokemon <https://triplydb.com/academy/pokemon/vocab/type> ?type1.
  ?pokemon <https://triplydb.com/academy/pokemon/vocab/type> ?type2.
  ?type1 <http://www.w3.org/2000/01/rdf-schema#label> ?type1Name.
  ?type2 <http://www.w3.org/2000/01/rdf-schema#label> ?type2Name.   
}
```

### Abbrevations

#### Use prefixes to shorten the previous query
Most prefixes end with / or #
```sql
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix vocab: <https://triplydb.com/academy/pokemon/vocab/>
select ?color ?cry ?name ?type1Name ?type2Name {
  ?pokemon vocab:colour ?color.
  ?pokemon vocab:cry ?cry.
  ?pokemon rdfs:label ?name.
  ?pokemon vocab:type ?type1.
  ?pokemon vocab:type ?type2.
  ?type1 rdfs:label ?type1Name.
  ?type2 rdfs:label ?type2Name.   
}
```

#### Use base IRI
```sql
base <https://triplydb.com/academy/pokemon/vocab/>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select ?color ?cry ?name ?type1Name ?type2Name {
  ?pokemon <colour> ?color.
  ?pokemon <cry> ?cry.
  ?pokemon rdfs:label ?name.
  ?pokemon <type> ?type1.
  ?pokemon <type> ?type2.
  ?type1 rdfs:label ?type1Name.
  ?type2 rdfs:label ?type2Name.   
}
```

#### Empty alias
```sql
base <https://triplydb.com/academy/pokemon/vocab/>
prefix : <http://www.w3.org/2000/01/rdf-schema#>
select ?color ?cry ?name ?type1Name ?type2Name {
  ?pokemon <colour> ?color.
  ?pokemon <cry> ?cry.
  ?pokemon :label ?name.
  ?pokemon <type> ?type1.
  ?pokemon <type> ?type2.
  ?type1 :label ?type1Name.
  ?type2 :label ?type2Name.   
}
```

#### Special IRI: rdf type
`<http://www.3.org/1999/02/22-rdf-syntax-ns#type>` -> a
(This can only be used in the predicate position, not object or subject)
```sql
base <https://triplydb.com/academy/pokemon/vocab/>
prefix : <http://www.w3.org/2000/01/rdf-schema#>
select ?color ?cry ?name ?type1Name ?type2Name {
  ?pokemon a ?class.
  ?pokemon <colour> ?color.
  ?pokemon <cry> ?cry.
  ?pokemon :label ?name.
  ?pokemon <type> ?type1.
  ?pokemon <type> ?type2.
  ?type1 :label ?type1Name.
  ?type2 :label ?type2Name.   
}
```

#### In literals
```sql
prefix geo: <http://www.opengis.net/ont/geosparql#>
select ?location {
    bind("Point(4.8649 52.33287)"^^geo:wktLiteral as ?location)
}
```

#### End-of-lines
```
base <https://triplydb.com/academy/pokemon/vocab/>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select ?color ?cry ?name ?type1Name ?type2Name {
  ?pokemon a ?class;
    <colour> ?color;
    <cry> ?cry;
    rdfs:label ?name;
    <type> ?type1, # You could also type <type> ?type1, ?type2 on one line
    ?type2.

  ?type1 rdfs:label ?type1Name, ?type2Name.   
}
```

#### Literal abbrevations (see table above)
```sql
prefix vocab: <https://triplydb.com/academy/pokemon/vocab/>
prefix xsd: <http://www.w3.org/2001/XMLSchema#>
select ?pokemon {
    # ?pokemon vocab:weight "80"^^xsd:integer.
    ?pokemon vocab:weight 80.
}
```

### Bind + functions + html
```sql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
prefix vocab: <https://triplydb.com/academy/pokemon/vocab/>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select ?color ?name ?text ?widget {
  ?pokemon
    vocab:colour ?color;
    vocab:cry ?cry;
    rdfs:label ?name;
    foaf:depiction ?image.
  bind(concat("Hi!", " ", ?name) as ?text)

  # bind(concat("202", "0")^^xsd:gYear as ?widget)  # --> Error
  # bind(strdt(concat("202", "0"), xsd:gYear) as ?widget)  # --> No error

  bind(strdt(concat("<h2 style='color:", ?color, "'>", ?name, "</h2><img src='", str(?image), "'><audio controls><source src='", str(?cry), "'></audio>"), rdf:HTML) as ?widget)
}
limit 10
```

### Discover all available predicates
```sql
prefix mo: <http://purl.org/ontology/mo/>
SELECT DISTINCT ?p {
  ?record a mo:Record; ?p ?o.
}
```
(Thanks Felix!)

Also check out [Sparklis](http://www.irisa.fr/LIS/ferre/sparklis/osparklis.html?title=Core%20English%20DBpedia&endpoint=http%3A//servolis.irisa.fr/dbpedia/sparql) & [its examples](http://www.irisa.fr/LIS/ferre/sparklis/examples.html)! (Thanks Claire!)

Thanks to [Wouter Beeks video series](https://www.youtube.com/watch?v=-Z2y6LeNacw&list=PLaa8QYrMzXNnzY-4YVM5507iZuESWVcnU) as well as my colleagues for not banning me from the rocket chat for my questions


---

Here's some other queries I made that don't fit in the above because they work but I barely understand them myself but they might contain wisdom somewhere

```sql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix mo: <http://purl.org/ontology/mo/>
prefix dc: <http://purl.org/dc/elements/1.1/>


SELECT ?title (SUM(?duration)/1000/60 AS ?total_duration_in_min) {
  ?record a mo:Record;
    dc:title ?title;
    mo:track ?track.
  ?track mo:duration ?duration.
}
GROUP BY ?title
HAVING((SUM(?duration)/(1000.0*60.0)) < 60)
ORDER BY DESC(?total_duration_in_min)
LIMIT 10
```
(Thanks Claire, Riad and Niels for dealing with my spam about this)
