# ID.LOC.GOV API notes

[https://id.loc.gov/views/pages/swagger-api-docs/index.html](https://id.loc.gov/views/pages/swagger-api-docs/index.html)

## Suggest services 2.0

[https://id.loc.gov/views/pages/swagger-api-docs/#suggest-service-2.json](https://id.loc.gov/views/pages/swagger-api-docs/#suggest-service-2.json)

- does some relevant ranking if executed as "keyword" search
- searches:
  - authorized labels (case and diacritic insensitive)
  - variant labels (case and diacritic insensitive)
  - codes (case sensitive), e.g. language codes
  - tokens (case sensitive), i.e. record IDs
- is punctuation sensitive if there is punctuation in keyword search search terms; strip punctuation from them if you want punctuation insensitivity.
- **choice of `searchtype` is important!**
  - `leftanchored`
    - must match exactly from left to right. E.g. search term "Helmut Trotnow" will not return results; search term must be "Trotnow, Helmut" (punctuation optional). * and ? can be used as wildcards.
    - results are sorted alphabetically
    - results are a list of matched headings/codes/tokens and do not include any further information. Because both authorized and variant headings are searched, the results may contain headings appearing in the same record.

  - `keyword`
    - is a keyword search, so no requirement for search terms to match directionally.
    - results are sorted by relevance ranking
    - results are a list of objects containing further information from the matched record.

### Endpoint and syntax

**Endpoints**: https://id.loc.gov/{scheme}/suggest2?q=

{base}/{scheme}**/suggest2?**q={URL escaped keyword(s)}**&**{parameter}={parameter value}**&**{parameter}={parameter value} ...

```
https://id.loc.gov/authorities/names/suggest2?q=helmut%20trotnow&rdftype=PersonalName&searchtype=keyword&count=10&offset=0&mime=json&usage=true&rawlist=false
```

### Results formats

- `q` → the used search term(s)
- `count` → the number of matched headings (not necessarily the same as the number of matched records as multiple headings (authorised/variant) can be matched by the search)
- `pagesize` → the number of results per page (this can be adjusted as parameter (`count`) in the request URL to up to 250)
- `start` → the offset (if more than `pagesize` number of results and paging through them)
- `sortmethod` → the sort method applied to `hits`
- `searchtype` → the type of search carried out
- `rdftype` (optional) → if limited, the `rdftype` results are limited to
- `directory` → the scheme/collection queried, "all" if no scheme/collection is specified
- `hits` → a list of matched headings (`leftanchored`)/record objects (`keyword`)

#### JSON

##### `leftanchored`

```json
{
  "q": "trotnow, helmut",
  "count": 2,
  "pagesize": 10,
  "start": 0,
  "sortmethod": "alpha",
  "searchtype": "left-anchored",
  "rdftype": "PersonalName",
  "directory": "/authorities/names/",
  "hits": [
    "Trotnow, Helmut",
    "Trotnow, Helmut, 1946-"
  ]
}
```

##### `keyword`

```json
{
  "q": "trotnow, helmut*",
  "count": 1,
  "pagesize": 10,
  "start": 0,
  "sortmethod": "rank",
  "searchtype": "keyword",
  "rdftype": "PersonalName",
  "directory": "/authorities/names/",
  "hits": [
    {
      "suggestLabel": "Trotnow, Helmut, 1946-",
      "uri": "http://id.loc.gov/authorities/names/n80069484",
      "aLabel": "Trotnow, Helmut, 1946-",
      "vLabel": "",
      "sLabel": "Trotnow, Helmut",
      "code": "",
      "token": "n80069484",
      "rank": "8741",
      "contributions": 5,
      "subject-of": 5,
      "browseAgent": "TROTNOW  HELMUT  1946 ",
      "more": {
        "nonlatinLabels": [],
        "vernacularLabels": [],
        "rdftypes": [
          "PersonalName",
          "SimpleType",
          "Name",
          "Authority"
        ],
        "collections": [
          "http://id.loc.gov/authorities/names/collection_NamesAuthorizedHeadings",
          "http://id.loc.gov/authorities/names/collection_LCNAF"
        ],
        "genres": [],
        "variantLabels": [],
        "languages": [],
        "relateds": [],
        "occupations": [],
        "activityfields": [],
        "sources": [
          "found : His Karl Liebknecht, 1980: t.p. (Helmut Trotnow) jkt. (b. 1946; Raferent im Wissenschaftszentrum Bonn)"
        ],
        "locales": [],
        "birthplaces": [],
        "birthdates": [],
        "deathdates": [],
        "establishDates": [],
        "terminateDates": [],
        "lcclasss": [],
        "lcclasses": [],
        "hasRelatedAuthoritys": [],
        "broaders": [],
        "sees": [],
        "hasEarlierEstablishedForms": [],
        "hasLaterEstablishedForms": [],
        "useFors": [],
        "marcKeys": [
          "10010$aTrotnow, Helmut,$d1946-"
        ],
        "gacs": []
      }
    }
  ]
}
```

#### XML

##### `leftanchored`

```xml
<json type="object" xmlns="http://marklogic.com/xdmp/json/basic">
  <q type="string">trotnow, helmut</q>
  <count type="number">2</count>
  <pagesize type="number">10</pagesize>
  <start type="number">0</start>
  <sortmethod type="string">alpha</sortmethod>
  <searchtype type="string">left-anchored</searchtype>
  <rdftype type="string">PersonalName</rdftype>
  <directory type="string">/authorities/names/</directory>
  <hits type="array">
    <hit type="string" xmlns:jb="http://marklogic.com/xdmp/json/basic">Trotnow, Helmut</hit>
    <hit type="string" xmlns:jb="http://marklogic.com/xdmp/json/basic">Trotnow, Helmut, 1946-</hit>
  </hits>
</json>
```

##### `keyword`

````xml
<json type="object" xmlns="http://marklogic.com/xdmp/json/basic">
  <q type="string">trotnow, helmut*</q>
  <count type="number">1</count>
  <pagesize type="number">10</pagesize>
  <start type="number">0</start>
  <sortmethod type="string">rank</sortmethod>
  <searchtype type="string">keyword</searchtype>
  <rdftype type="string">PersonalName</rdftype>
  <directory type="string">/authorities/names/</directory>
  <hits type="array">
    <hit type="object">
      <suggestLabel type="string">Trotnow, Helmut, 1946-</suggestLabel>
      <uri type="string">http://id.loc.gov/authorities/names/n80069484</uri>
      <aLabel type="string">Trotnow, Helmut, 1946-</aLabel>
      <vLabel type="string"/>
      <sLabel type="string">Trotnow, Helmut</sLabel>
      <code type="string"/>
      <token type="string">n80069484</token>
      <rank type="string">8741</rank>
      <contributions type="number">5</contributions>
      <subject-of type="number">5</subject-of>
      <browseAgent type="string">TROTNOW  HELMUT  1946 </browseAgent>
      <more type="object">
        <nonlatinLabels type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <vernacularLabels type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <rdftypes type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic">
          <rdftype type="string">PersonalName</rdftype>
          <rdftype type="string">SimpleType</rdftype>
          <rdftype type="string">Name</rdftype>
          <rdftype type="string">Authority</rdftype>
        </rdftypes>
        <collections type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic">
          <collection type="string">http://id.loc.gov/authorities/names/collection_NamesAuthorizedHeadings</collection>
          <collection type="string">http://id.loc.gov/authorities/names/collection_LCNAF</collection>
        </collections>
        <genres type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <variantLabels type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <languages type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <relateds type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <occupations type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <activityfields type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <sources type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic">
          <source type="string">found : His Karl Liebknecht, 1980: t.p. (Helmut Trotnow) jkt. (b. 1946; Raferent im Wissenschaftszentrum Bonn)</source>
        </sources>
        <locales type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <birthplaces type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <birthdates type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <deathdates type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <establishDates type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <terminateDates type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <lcclasss type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <lcclasses type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <hasRelatedAuthoritys type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <broaders type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <sees type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <hasEarlierEstablishedForms type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <hasLaterEstablishedForms type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <useFors type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
        <marcKeys type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic">
          <marcKey type="string">10010$aTrotnow, Helmut,$d1946-</marcKey>
        </marcKeys>
        <gacs type="array" xmlns:jb="http://marklogic.com/xdmp/json/basic"/>
      </more>
    </hit>
  </hits>
</json>
````



### Usage in OpenRefine

The general pattern in OpenRefine Add columns by fetching URLs is:

``` 
'https://id.loc.gov/authorities/names/suggest2?q=' + escape({search term(s)}, 'url') + '&{parameter1}={parameter1 value}&{parameter2}={parameter2 value}'
```

Obviously, if you want to search subjects instead of names or any other MADS scheme/collection change the respective part(s) of the base URL (e.g. https://id.loc.gov/authorities/subjects to search LCSH).

#### Example

E.g. to carry out a **keyword** search (`searchtype=keyword`) for **Helmut Trotnow**, limiting results to records of **type** `PersonalName` (`rdftype=PersonalName`)[^1] and retrieve results in **JSON**:

  ``` 
'https://id.loc.gov/authorities/names/suggest2?q=' + escape('helmut trotnow', 'url') + '&searchtype=keyword&rdftype=PersonalName'
  ```

To retrieve results in XML add `mime=xml` as a parameter (the intention and design is for JSON responses, so the XML is a bit clunky):

  ```
'https://id.loc.gov/authorities/names/suggest2?q=' + escape('helmut trotnow', 'url') + '&searchtype=keyword&rdftype=PersonalName&mime=xml'
  ```



## Resource retrieval

[https://id.loc.gov/views/pages/swagger-api-docs/#download.json](https://id.loc.gov/views/pages/swagger-api-docs/#download.json)

### Endpoint(s) and syntax

**Endpoints**: 

- https://id.lov.gov/{scheme}/{id}.{serialisation}

  {base}**/{id}.rdf**

  ```
  https://id.loc.gov/authorities/subjects/sh85126736.rdf
  ```

  - rdf → returns MADS and SKOS RDF/XML
  - nt → returns MADS and SKOS n-Triples
  - json → returns JSON serialisation of MADS and SKOS RDF/XML
  - xml → returns HTML of graphic display site

- https://id.lov.gov/{scheme}/{id}.{format}.{serialisation}

  {base}**/{id}.skos.rdf**

  ```
  https://id.loc.gov/authorities/subjects/sh85126736.madsrdf.rdf
  ```

  madsrdf:

  - rdf → returns MADS RDF/XML
  - nt → returns MADS n-Triples
  - json → returns JSON serialisation of MADS RDF/XML
  - xml → returns MADS XML

  skos:

  - rdf → returns SKOS RDF/XML
  - nt → returns SKOS n-Triples
  - json → returns JSON serialisation of SKOS RDF/XML

  marcxml[^2]:

  - xml → returns MARCXML XML

### Results formats

#### Formats: MADS vs SKOS

| MADS                                                         | SKOS                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **M**etadata Authority **D**escription **S**chema            | **S**imple Knowledge **O**rganization System                 |
| **specialised**, **detailed** schema for library authority data | **lightweight**, **general-purpose** standard for taxonomies and thesauri |
| MADS documentation: https://www.loc.gov/standards/mads/<br>MADS/RDF: https://www.loc.gov/standards/mads/rdf/ | https://www.w3.org/2004/02/skos/                             |

→ MADS/RDF records carry more information than SKOS ones. 

### Usage in OpenRefine

The general pattern in OpenRefine Add columns by fetching URLs is:

``` 
'https://id.loc.gov/authorities/names/' + id + '.{serialisation}'
'https://id.loc.gov/authorities/names' + id + '.{format}.{serialisation}'
```

Obviously, if you want to search subjects instead of names or any other MADS scheme/collection change the respective part(s) of the base URL (e.g. https://id.loc.gov/authorities/subjects to search LCSH).

#### Example

E.g. to retrieve the person record for **Helmut Trotnow** in `rdf` format (this will include both MADS and SKOS).

```
'https://id.loc.gov/authorities/names/' + 'n80069484' + '.rdf'
```

To retrieve the same record in MADS xml:

```
'https://id.loc.gov/authorities/names/' + 'n80069484' + '.madsrdf.xml'
```

## General searching

https://id.loc.gov/views/pages/swagger-api-docs/#searching.json

- results are sorted by relevance
- wildcards ("*" and "?"), Booleans (AND, NOT NOR)  and "-" for negation can be used
- double quotes for phrases

- Constraints can be added regarding

  - which labels to search (`aLabel`, `vLabel`), 
  - `rdftype`[^1], 
  - scheme and/or collection membership (`memberOf`, `scheme`), 
  - specific control numbers (`token`) and codes (`code`), 

  - record creator (`contentSource`)and modifier (`contentSourceModified`), and

  - creation (`cdate`) and modified (`mdate`) dates.

### Endpoint and syntax

**Endpoint**: [https://id.loc.gov/search/](https://id.loc.gov/search/)?q={URL escaped search term(s)}

```
https://id.loc.gov/search/?q=Trotnow%20Helmut
```

To use any of the constraints they need to be appended using the syntax `&q={constraint}:{value}`. You'll likely want to URL escape the `{constraint}:{value}` part. E.g. to constrain the above search to NAF and `rdftype` to `PersonalName`:

`https://id.loc.gov/search/?q=Trotnow Helmut`**`&q=scheme:`**`http://id.loc.gov/authorities/names`**`&q=rdftype:`**`PersonalName`


Nicely escaped that makes:

```
https://id.loc.gov/search/?q=Trotnow%20Helmut&q=scheme%3Ahttp%3A%2F%2Fid.loc.gov%2Fauthorities%2Fnames&q=rdftype%3APersonalName
```

> [!IMPORTANT]
>
> The `scheme` URL must be `http` not `https`. Don't ask me why, no idea.

You don't need to have search terms in the general search, it can be all constraints. E.g. if you want to search only authorised labels:

`https://id.loc.gov/search/?`**`q=aLabel:`**`Trotnow Helmut`**`&q=scheme:`**`http://id.loc.gov/authorities/names`**`&q=rdftype:`**`PersonalName`

```
https://id.loc.gov/search/?q=aLabel%3ATrotnow%20Helmut&q=scheme%3Ahttp%3A%2F%2Fid.loc.gov%2Fauthorities%2Fnames&q=rdftype%3APersonalName
```

### Result formats

Not sure what to make of the result formats with this. It's an atom feed either in XML, JSON or JSONP. Suspect the useful one to be JSONP, but unsure how to use that. Alternatively, by providing no format or accept mime type, one get the HTML of the search, which does hold the search results in a table and contains paging links.

The format is determined either by passing a mime type in the header or appending `&format={format}` to the URL. Valid formats are:

| Format | Header accept mime type  | URL format value |
| ------ | ------------------------ | ---------------- |
| XML    | `application/atom+xml`   | `atom-xml`       |
| JSON   | `application/json`       | `atom-json`      |
| ?      | `application/javascript` | `atom-jsonp`     |
| HTML   | omitted                  | omitted          |

### Usage in OpenRefine

Modify the header to have the appropriate value in the "Accept" field to define the format of the response.

The general pattern in OpenRefine Add columns by fetching URLs is:

```
'https://id.loc.gov/search/?q=' + escape('{search terms(s)}', 'url') 
```

Or if you want to use constraints:

```
'https://id.loc.gov/search/?q=' + escape('{search terms(s)}', 'url') + '&q=' + escape('{constaint1}:{value1}', 'url') + '&q=' + escape('{constraint2}:{value2}', 'url')
```

> [!IMPORTANT]
>
> If you use the `scheme` constraint the `scheme` URL must be `http` not `https`. Don't ask me why, no idea.

Obviously, if you want to search subjects instead of names or any other MADS scheme/collection change the respective part(s) of the base URL (e.g. http://id.loc.gov/authorities/subjects to search LCSH).

#### Example

E.g. to search for **Helmut Trotnow** constraint to name authorities and `PersonalName`:

```
'https://id.loc.gov/search/?q=' + escape('aLabel:Trotnow Helmut', 'url') + '&q=' + escape('scheme:http://id.loc.gov/authorities/names', 'url') + '&q=' + escape('rdftype:PersonalName', 'url')
```

## Known-label retrieval

This is only useful if you know that the string you're searching with is either a valid authorised or variant label, and you want to lookup the respective URI.

**This search is sensitive to everything**. Case, spelling, punctuation; everything needs to match exactly. No use of wildcard, truncation etc.

Non-ASCII characters need to be URL escaped (e.g. "Zoë" becomes "Zo%C3%AB").

It returns a 404 if no exactly matching label is found. Else it's JSON encoded RDF. Bit irritating. 

> [!NOTE]
>
> The response header contains the URI as well, so that might be the better thing to use to get it; not sure one can do that in OpenRefine though.

### Endpoints and syntax

- https://id.loc.gov/authorities/label/{URL escaped search label}

  This searches all authority files. Not particularly recommended, as it'll return the first matching result only and that may be from the wrong authority file (e.g. LCSH vs. Children's Subjects). Better to use the endpoint for the respective file if known.

  ```
  https://id.loc.gov/authorities/label/France
  ```

- https://id.loc.gov/authorities/{dataset}/label/{URL escaped search label}

  ```
  https://id.loc.gov/authorities/names/Trotnow%2C+Helmut%2C+1946-
  ```

- https://id.loc.gov/vocabulary/{dataset}/label/{URL escaped search label}

### Result format

Returns the JSON serialisation of MADS and SKOS RDF/XML of the first matched record or a 404 error if no record is found.

### Usage in OpenRefine

The general pattern in OpenRefine Add columns by fetching URLs is:

``` 
'https://id.loc.gov/authorities/names/label/' + escape({search label}, 'url')
```

Obviously, if you want to search subjects instead of names or any other MADS scheme/collection change the respective part(s) of the base URL (e.g. https://id.loc.gov/authorities/subjects to search LCSH).

#### Example

E.g. to retrieve the person record for **Helmut Trotnow** :

```
'https://id.loc.gov/authorities/names/label/' + escape('Trotnow, Helmut, 1946-', 'url')
```

## Footnotes

[^1]: see [Appendix A](#appendix-a-madstype-subclasses) for other valid `rdftype` values
[^2]: [https://www.loc.gov/standards/marcxml](https://www.loc.gov/standards/marcxml)

## Appendices

### Appendix A: `MADSType` subclasses

```
MADSType
├── SimpleType
│   ├── GenreForm
│   ├── Geographic
│   │   ├── Area
│   │   ├── City
│   │   ├── CitySection
│   │   ├── Continent
│   │   ├── Country
│   │   ├── County
│   │   ├── ExtraterrestrialArea
│   │   ├── Island
│   │   ├── Province
│   │   ├── Region
│   │   ├── State
│   │   └── Territory
│   ├── Language
│   ├── Medium
│   ├── Name
│   │   ├── ConferenceName
│   │   ├── CorporateName
│   │   ├── FamilyName
│   │   └── PersonalName
│   ├── Occupation
│   ├── Temporal
│   ├── Title
│   └── Topic
├── ComplexType
    ├── ComplexSubject
    ├── HierarchicalGeographic
    └── NameTitle
```

