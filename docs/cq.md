## Competency Questions

BiRO can be used for answering several questions related to bibliographic records and references.
In the following subsections, some of them are introduced together with their respective SPARQL queries. 

The prefixes that are used in all the SPARQL queries provided below are defined as follows:

    PREFIX biro: <http://purl.org/spar/biro/>
    PREFIX co: <http://purl.org/co/>
    PREFIX dcterms: <http://purl.org/dc/terms/>
    PREFIX frbr: <http://purl.org/vocab/frbr/core#>>
    PREFIX foaf: <http://xmlns.com/foaf/0.1/>
    PREFIX literal: <http://www.essepuntato.it/2010/06/literalreification/>

### CQ1

Which bibliographic references and citation contexts are associated with an in-text reference pointer?

    SELECT ?inTextPointer ?reference ?context ?contextContent
    WHERE {
        ?inTextPointer a c4o:InTextReferencePointer  ;
            c4o:denotes  ?reference ;
            c4o:hasContext  ?context .
        OPTIONAL { ?context c4o:hasContent ?contextContent . }
    }

### CQ2

Which textual passages in a cited document are relevant to a citing context?

    SELECT ?citingContext ?citingContent ?citedPassage ?citedContent
    WHERE {
        ?citedPassage c4o:isRelevantTo  ?citingContext .
        OPTIONAL { ?citingContext c4o:hasContent ?citingContent . }
        OPTIONAL { ?citedPassage c4o:hasContent ?citedContent . }
    }

### CQ3

What is the in-text citation frequency of a bibliographic reference within a citing document?

    SELECT ?reference ?frequency ?citedWork
    WHERE {
        ?reference a biro:BibliographicReference ;
            c4o:hasInTextCitationFrequency  ?frequency .
        OPTIONAL { ?reference biro:references ?citedWork . }
    }

### CQ4

What is the global citation count of a document, including the source and measurement date?

    SELECT ?document ?citationCount ?countValue ?source ?date
    WHERE {
        ?document c4o:hasGlobalCitationFrequency ?citationCount .
        ?citationCount a c4o:GlobalCitationCount ;
            c4o:hasGlobalCountValue  ?countValue ;
            c4o:hasGlobalCountSource ?source ;
            c4o:hasGlobalCountDate ?date .
    }

### CQ5

What information sources are used to track global citation counts, along with their homepages?

    SELECT ?source ?homepage
    WHERE {
        ?source a c4o:BibliographicInformationSource  .
        OPTIONAL { ?source foaf:homepage ?homepage . }
    }