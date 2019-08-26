# query-testing-ontology

## What and why
This repository contains a query-testing ontology which enables you to write test-manifests which can be used to test query-engines such as the comunica-query engines (based on the [comunica](https://github.com/comunica/comunica) query engine platform) over `TPF-, SPARQL-, File-, HDT, RDFJS`-linked data sources. 

## Ontology
The ontology can be found [here](https://github.com/ManuDeBuck/query-testing-ontology/) and what follows is a short summary of the ontology.

#### LdfQueryEvaluationTest (Class)
A class representing a test of a test-manifest.

#### LinkedDataFragment (Class)
A linked data resource that the query-engine should use to query the data.

The ontology contains the following `LinkedDataFragments`:
* `TPF`
* `File`
* `SPARQL`
* `HDT`
* `RDFJS`

These `LinkedDataFragments` can be used to declare the `sourceType` of a specific `source`.

#### dataSources (Property)
The `dataSources` property can be used for a `LdfQueryEvaluationTest` to define it's datasources. This can be done as a list of blank nodes with the `source`- and `sourceType`- properties.

#### sourceType (Property)
The `sourceType` property can be used to give information about a `source`'s sort of `LinkedDataFragment` (a list of `LinkedDataFragment`'s is listed above).

#### mockFolder (Property)
The `mockFolder` property is (only) necessary when including tests concerning `TPF`- and/ or `SPARQL`- sources. The `mockFolder` property describes a literal which represents the folder where mocking-files for `TPF`- and/ or `SPARQL`- endpoints can be found. More information about these mocking-files can be found in the [tpf-recorder](https://github.com/ManuDeBuck/tpf-recorder) repository which creates mock-files that can be used in the [rdf-test-suite-ldf.js](https://github.com/ManuDeBuck/tpf-recorder) testing-suite.

#### Extra's

This ontology builds further upon the [Manifest vocabulary for test cases](http://www.w3.org/2001/sw/DataAccess/tests/test-manifest#) and the [Vocabulary for query test cases](http://www.w3.org/2001/sw/DataAccess/tests/test-query#) both written by Andy Seaborne . Tests further consist of: `mf:name, mf:action, qt:query, mf:result`. With `mf` and `qt` representing prefixes for ontologies named above.

## Examples

An example manifest can be found here:
```bash
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:	  <http://www.w3.org/2000/01/rdf-schema#> .
@prefix mf:     <http://www.w3.org/2001/sw/DataAccess/tests/test-manifest#> .
@prefix qt:     <http://www.w3.org/2001/sw/DataAccess/tests/test-query#> .
@prefix et:     <https://manudebuck.github.io/query-testing-ontology/query-testing-ontology.ttl#> .
@prefix :       <https://manudebuck.github.io/query-testing-ontology/examples/ldf-manifest.ttl#> .

<>  rdf:type mf:Manifest ;
  rdfs:label "SELECT" ;
  mf:entries
  ( 
  :selectwhere01
  ) .

:selectwhere01 rdf:type et:LdfQueryEvaluationTest ;
  mf:name    "selectwhere01 - SELECT WHERE" ;
  rdfs:comment "SELECT * WHERE { ?s ?p <http://dbpedia.org/resource/Belgium>. ?s ?p ?o } LIMIT 5" ;
  mf:action
        [ qt:query  <selectwhere01.rq> ;
          et:mockFolder <selectwhere01> ] ;
  et:dataSources (
    [ et:source <http://fragments.dbpedia.org/2015/en> ;
      et:sourceType et:TPF ]
    [ et:source <http://dbpedia.org/sparql> ;
      et:sourceType et:SPARQL ] ) ;
  mf:result  <selectwhere01/result.srj> .
```
### Explanation

```bash
<>  rdf:type mf:Manifest ;
  rdfs:label "SELECT" ;
  mf:entries
  ( 
  :selectwhere01
  ) .
```
Every manifest is given a label and has entries. These entries are the tests (e.g.`LdfQueryEvaluationTest`). When adding new tests to the manifest, an entry should be added to `mf:entries` too.

```bash
:selectwhere01 rdf:type et:LdfQueryEvaluationTest ;
  mf:name    "selectwhere01 - SELECT WHERE" ;
  rdfs:comment "SELECT * WHERE { ?s ?p <http://dbpedia.org/resource/Belgium>. ?s ?p ?o } LIMIT 5" ;
  mf:action
        [ qt:query  <selectwhere01.rq> ;
          et:mockFolder <selectwhere01> ] ;
  et:dataSources (
    [ et:source <http://fragments.dbpedia.org/2015/en> ;
      et:sourceType et:TPF ]
    [ et:source <http://dbpedia.org/sparql> ;
      et:sourceType et:SPARQL ] ) ;
  mf:result  <selectwhere01/result.srj> .
```

Every test is given a unique name (here: `selectwhere01`) and has the `LdfQueryEvaluationTest`-type. 
A `mf:name` and `rdfs:comment` property should give some extra explanation about the test.

The `mf:action` contains the `qt:query` a `.rq`-file containing the *SPARQL*-query the test sould execute. When testing `TPF`- and/ or `SPARQL`- endpoints, a `mockFolder` should be given (cfr. explanation above and the [recorder](https://github.com/ManuDeBuck/tpf-recorder)).

The `et:dataSources` gives a list of dataSources that supplying the query-engine with data. This list contains blank nodes containing `et:source`- and `et:sourceType`-properties describing the source.

The `mf:result` property defines the file where the expected result of the test can be found. This file will be used to compare with the received result to check wether or not the query-engine did it's job correctly.

## More information

More information about usage of this ontology and the creation of `mockFolder` folders can be found here: [https://github.com/ManuDeBuck/rdf-test-suite-ldf.js](https://github.com/ManuDeBuck/rdf-test-suite-ldf.js) and here: [https://github.com/ManuDeBuck/tpf-recorder](https://github.com/ManuDeBuck/tpf-recorder)
