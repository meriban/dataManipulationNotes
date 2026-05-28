# MDG Library Carpentry Workshop, Birmingham, Aston University 5th and 12th June 2026, Day 1: Data formats

To work with files in the formats discussed, work with a text editor that can do syntax highlighting and show invisible characters such as e.g. [Notepad++](https://notepad-plus-plus.org/)[^c], [Visual Studio Code](https://code.visualstudio.com/), [Sublime Text](https://www.sublimetext.com/) etc. They have a bit of a learning curve, but you'll save yourself a lot of headache!

**Notepad++** is a nice and easy one to get started with. The interface looks kind of familiar to office programmes. Plugins are available to make working with e.g. JSON and XML files easier.

**Visual Studio Code** (VS Code) is a lot more than just a text editor, you can use this as a development environment and run code from it. It has a terminal and debugging functionality. The learning curve is a bit steeper, the interface is less familiar if you come from the usual office apps and more is done via keyboard shortcuts or commands. There are lots of extensions and plugins, you can connect it to AI etc. You get the idea: it can do a lot and thus is more complex to learn.

Visual Studio Code also has a browser version if you cannot or don't want to install anything on your machine: https://vscode.dev/. 

## Tabular data

Tabular data formats, like CSV and TSV, are **plain text** data formats that store **tables**. The table **rows** are separated by a **newline** and the **columns** are separated by a **delimiter**. The delimiter varies between formats. Using custom delimiters is also possible, though you might want to save the file as simple text file (.txt) in this case to avoid confusion.

**CSV** stands for **C**omma-**S**eparated **V**alues, i.e. the used column delimiter is comma `,`. 

**TSV** for **T**ab-**S**eparated **V**alues, i.e. the used column delimiter is tab ⇥.

> [!NOTE]
>
> **Newline character(s)**
>
> The character(s) used for **newline** depend(s) on your operating system. **Windows** uses **CRLF** (carriage return and line feed), while Unix and Unix-like systems (e.g. **Linux** or **macOS**) use **LF** (line feed) only. You may run into problems if the newline character(s) in the file(s) your working with do not conform to what your operating system is expecting.
>
> Text editors like Notepad++ and Visual Studio Code show you which newline character(s) are used in the file you have open and also let you change it. Look at the bottom right:
>
> ![image-20260517125303293](assets/notepad_newline.png)
>
> ![image-20260517125522465](assets/vscode_newline.png)
>
> Click or right-click on the respective area let's you change the newline character(s). 

### CSV

> [!TIP]
>
> VS Code has an extension called "**Rainbow CSV**" that gives the values in each column a different colour and can also make the header line sticky. Very useful! It doesn't just work for CSV but also other delimited formats and you can even set it to use a custom delimiter.
>
> VS Code may as you if you want to install this the first time you open a CSV file in it. Just click the "Install" button.
>
> ![image-20260517130700950](assets/vscode_rainbowcsv.png)
>
> To install it otherwise click on the extensions button in the left side menu: ![image-20260517130332989](assets/vs_code_extensions.png)
>
> Type "rainbow csv" into the search bar and click the "Install" button.
>
> ![image-20260517131010671](assets/vscode_rainbowcsv_install.png)

#### CSV basics

The first line in the CSV file we're creating in this tutorial will be **header line** containing the column names. Header lines are not mandatory, it's fine to have the data only.

```text
ID,name,age,breed,colour
```

Note that there are **no spaces between** the **delimiter** (`,`) and the **column names**.

Filling in the rest of our demo data we have:

```
Id,name,age,breed,colour
1,Duncan,10,moggy,orange
2,Izzy,5,moggy,black
3,Trixie,8,moggy,black
4,Sushi,15,Korat,blue
5,Pippin,2,Munchkin,white
```

![image-20260524162534435](assets/vscode_cats1.png)

**Each line**, or record, should **have the same number of column values (fields).**

#### Quote and escape characters

What if one of our **values contains a comma** however? Let's say Duncan is both orange and white.

```
Id,name,age,breed,colour
1,Duncan,10,moggy,orange, white
```

![image-20260517134404772](assets/vscode_cats2.png)

We now have a mismatch between columns specified in the header and columns given for the first record. The table looks something like that now:

| ID   | name   | age  | breed | colour |       |
| ---- | ------ | ---- | ----- | ------ | ----- |
| 1    | Duncan | 10   | moggy | orange | white |

To avoid this we use a so called **quote character** to enclose the value. The default quote characters is single quotes (`''`). 

> [!CAUTION]
>
> A text editor shouldn't mangle those, but be careful if you for some reason work in something like Word or Google Doc. They have a habit of automatically turning `"Duncan"` into `“Duncan”`. Though they look very similar `"` and `“` or `”` are not the same character!

So let's enclose those colours:

```
Id,name,age,breed,colour
1,Duncan,10,moggy,"orange, white"
```

![image-20260517134457085](assets/vscode_cats3.png)

| ID   | name   | age  | breed | colour        |
| ---- | ------ | ---- | ----- | ------------- |
| 1    | Duncan | 10   | moggy | orange, white |

What if `"` itself is in a value that needs to be closed? 

In this case the `"` in the value needs to be **escaped** (i.e. marked) as not to be interpreted as a quote character. The default CSV dialect uses another `"` as the **escape character**, so unescaped `"` becomes `""` when escaped.

```
Id,name,age,breed,colour
1,"Isambard ""Izzy"" Sooty",5,moggy,black
```

> [!TIP]
>
> The quote and escape characters vary between different CSV dialects (yes, really). `"` Is the default defined in the standard CSV definition [RFC 4180](https://www.rfc-editor.org/rfc/rfc4180). 
>
> Because different delimiters, line terminators, quote and escape characters as well as changes in other behaviours can make sense a specification on how to express the specific dialect exists: [CSV Dialect](https://specs.frictionlessdata.io/csv-dialect/) and e.g. the Python standard CSV parser lets you set delimiter, line terminators etc. as parameters when reading in a CSV file.

**It's fine to quote all values, irrespective of them containing the delimiter.** I like this for consistency and readability.

Applying this to our demo data and adding some more fur colours in, we get:

```
"id","name","age","breed","colours"
"1","Duncan","10","moggy","orange, white"
"2","Izzy","5","moggy","black, white"
"3","Trixie","8","moggy","black, orange"
"4","Sushi","15","Korat","blue"
"5","Pippin","2","Munchkin","white, blue"
```

![image-20260524162731190](assets/vscode_cats4.png)

### Advantages and disadvantages of tabular data

CSV and other tabular data formats are used a lot because they

- can be created, read and edited in **any text editor** as well as common **spreadsheet applications**,
- are **human-readable**,
- use a very **simple schema**, and
- are **fast** to read and write to memory as **parsing** them **is easy**.

There are some downsides as well though and depending on the structure of your data, a tabular format might be very poor choice:

- a tabular data file can hold only **one table per file**. There are no "sheets" as in spreadsheet applications.
- you **cannot put formulas** into them. They are just data and can't hold data transformation logic.
- **nested and complex data doesn't** really **work** with them. If you can't easily put your data into a table, tabular data formats are not the weapon of choice.

## XML

XML stands for e**X**tensible **M**arkup **L**anguage. So, first what is a markup language?

### Markup languages

Markup languages allow one to **mark parts of data as a specific thing**. They have their roots in typesetting and text presentation: printers or typographers would *mark up* which typeface, style or text size to use for each part of a text. These instructions would then be followed by the typesetter or typesetting machine to set the text for printing.

There are markup languages around today (such as e.g. HTML, LaTeX, Markdown) that do pretty much exactly that: mark something as for example a heading, or to be emphasised. That markup is somewhat disconnected from the presentation though: something is marked as a heading, but it doesn't say anything about how that heading should look. The look is defined by so-called stylesheets. Just swap the stylesheet out and the content stays the same but may look completely different. The **data is separated from the presentation**. 

Abstracting this a bit away from marking things for presentation and towards **labelling**, **categorising** and **structuring** information into elements and adding in some **rules** about which elements that can be, where they can occur etc. so the structure can be validated: we get XML. 

### XML history and use

XML as a standard has been around for nearly 30 years. Work on it started in 1996 and it was first published in 1998. It is maintained by the [World Wide Web Consortium](https://www.w3.org/) (W3C). 

There are two versions: **[XML 1.0](https://www.w3.org/TR/xml/)**, which is in its 5th edition published in 2008 and [XML 1.1](https://www.w3.org/TR/xml11/) which is in its 2nd edition published in 2006. XML 1.0 is the default and what the W3C recommends to use. XML 1.1 has some additional, advanced features and should only be used if these are required[^77].

The main **purpose** of XML is **serialisation**, i.e. the 

- **storage**,
- **transmission**, and
- **reconstruction** of data.

XML allows this to be done in  a **software- and hardware independent** way.

It is designed to be both **machine- and human-readable**.

XML is so fundamental that a lot of data formats you are working with every day are built on it. For example Word and Excel documents (.docx, .xlsx), vector graphics (.svg), HTML, RSS, ...

### XML syntax

Documents conforming to the XML syntax are **well formed.**

#### Tags and elements

XML wraps data in **tags**: an **opening** and a **closing** **tag**. The names of the tags are not predefined, but it's up to **the author** of the XML document to **define tags** and overall **document structure**. Day to day one usually works with XML documents that follow specific definitions already developed. Coming up with one's own is less common in regular metadata work.

Opening **tags** names are enclosed in `<>` and closing tag names in `</>`. Every tag that's opened must also be closed again.

```xml
<person>
	<name>Fran</name>
</person>
```

Some tags exist but have no content, they are called **empty tags** and can be opened and closed in one go:

```xml
<emptyTag/>
```

**Tag names** cannot being with `-`, `.` or a number and they cannot contain any of the following characters ``!"#$%&'()*+,/;<=>?@[\]^`{|}~`` or a space. Otherwise one can choose whatever one wants.

Let's try this out with our cat data:

```xml
<id>1</id>
<name>Duncan</name>
<age>10</age>
<breed>moggy</breed>
<colour>orange</colour>
<colour>white</colour>
<id>2</id>
<name>Izzy</name>
<age>5</age>
<breed>moggy</breed>
<colour>black</colour>
<id>3</id>
<name>Trixie</name>
<age>8</age>
<breed>moggy</breed>
<colour>black</colour>
<colour>orange</colour>
<id>4</id>
<name>Sushi</name>
<age>15</age>
<breed>Korat</breed>
<colour>blue</colour>
<id>5</id>
<name>Pippin</name>
<age>2</age>
<breed>Munchkin</breed>
<colour>white</colour>
<colour>blue</colour>
```

Mh, how do we know where the description for one cat stop and the next begins though? This needs a bit more structure!

#### Nesting

The **content** of a tag **can also be other tags**, not just text or numbers. This is called **nesting**.

So let's wrap each of our cat description in the tag `<cat>` and add some indents so the nesting is visually clear as well.

```xml
<cat>
    <id>1</id>
    <name>Duncan</name>
    <age>10</age>
    <breed>moggy</breed>
    <colour>orange</colour>
    <colour>white</colour>
</cat>
<cat>
    <id>2</id>
    <name>Izzy</name>
    <age>5</age>
    <breed>moggy</breed>
    <colour>black</colour>
</cat>
<cat>
    <id>3</id>
    <name>Trixie</name>
    <age>8</age>
    <breed>moggy</breed>
    <colour>black</colour>
    <colour>orange</colour>
</cat>
<cat>
    <id>4</id>
    <name>Sushi</name>
    <age>15</age>
    <breed>Korat</breed>
    <colour>blue</colour>
</cat>
<cat>
    <id>5</id>
    <name>Pippin</name>
    <age>2</age>
    <breed>Munchkin</breed>
    <colour>white</colour>
    <colour>blue</colour>
</cat>
```

#### The root element and the XML tree

An XML document must have a **root element** that all other elements are nested/wrapped in. This is currently lacking from our example. The root element can be called anything, it's not necessary to be called "root". What's important is that on the top level there can be only one element.

So, let's wrap all out cats into a `<cats>` element.

```xml
<cats>
    <cat>
        <id>1</id>
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
    </cat>
    <cat>
        <id>2</id>
        <name>Izzy</name>
        <age>5</age>
        <breed>moggy</breed>
        <colour>black</colour>
    </cat>
    <cat>
        <id>3</id>
        <name>Trixie</name>
        <age>8</age>
        <breed>moggy</breed>
        <colour>black</colour>
        <colour>orange</colour>
    </cat>
    <cat>
        <id>4</id>
        <name>Sushi</name>
        <age>15</age>
        <breed>Korat</breed>
        <colour>blue</colour>
    </cat>
    <cat>
        <id>5</id>
        <name>Pippin</name>
        <age>2</age>
        <breed>Munchkin</breed>
        <colour>white</colour>
        <colour>blue</colour>
    </cat>
</cats>
```



What we have now is a **tree structure**. The various elements in it have **child** elements, **parent** elements or **sibling** elements.

![](assets/cat_tree.png)

#### XML prolog

Just one more thing is missing before we have a well formed XML document: the XML prolog that states the XML version used and the encoding.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<cats>
    <cat>
        <id>1</id>
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
    </cat>
    <cat>
        <id>2</id>
        <name>Izzy</name>
        ...
</cats>
```

You might have noticed that the prolog tag isn't closed. That's ok only for the prolog as it's not really part of the XML document. 

#### Value characters

Values can use nearly the entire Unicode spectrum, except most control characters (only Horizontal Tab, Line Feed and Carriage Return are allowed), surrogates and U+FFFE and U+FFFF. XML 1.1 allows everything except the `Null` control character, though some characters must be expressed in as their Unicode code point.

There are also some special characters that must be escaped in values:

| character | escaped      |
| --------- | ------------ |
| **<**     | **`&lt;`**   |
| >         | `&gt`        |
| &         | `&amp;`      |
| **'**     | **`&apos;`** |
| **"**     | **`&quot;`** |

#### Attributes

Attributes contain **data related to a specific element**. It's not immediately obvious how they differ from another nested element and they could be expressed as such too. 

Attributes are **key value pairs** added directly after the tag name of the **opening tag**. The value must be enclosed in `""` and the key and value separated by `=`.

For example, we could add a cat's weight as an attribute of the `<cat>` element:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<cats>
    <cat weigth="4kg">
        <id>1</id>
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
    </cat>
    ...
</cats>
```

The same could be expressed by making the weight another element nested in `<cat>`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<cats>
    <cat>
        <id>1</id>
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
        <weight>4kg</weight>
    </cat>
    ...
</cats>
```

So what's the difference?

There are n**o clear or fixed rules** when to use elements and when to use attributes, but **attributes** have a couple of **disadvantages**:

- they **cannot contain multiple values**,
- they **cannot contain other elements** or **attributes**, and
- thus they are **not easily expandable**.

A good rule of thumb is to

- use an **attribute** when the data is **metadata about the element** itself, and to
- use an **element** when the data is just that, **data**. 

So, in the example above, weight is definitely better placed as an element than as an attribute. However, the `<id>` element could easily become an attribute as all it does is number the respective cat.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<cats>
    <cat id="1">
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
    </cat>
    <cat id="2">
        <name>Izzy</name>
        <age>5</age>
        <breed>moggy</breed>
        <colour>black</colour>
    </cat>
    <cat id="3">
        <name>Trixie</name>
        <age>8</age>
        <breed>moggy</breed>
        <colour>black</colour>
        <colour>orange</colour>
    </cat>
    <cat id="4">
        <name>Sushi</name>
        <age>15</age>
        <breed>Korat</breed>
        <colour>blue</colour>
    </cat>
    <cat id="5">
        <name>Pippin</name>
        <age>2</age>
        <breed>Munchkin</breed>
        <colour>white</colour>
        <colour>blue</colour>
    </cat>
</cats>
```

#### Comments in XML documents

You can add comments to XML documents by enclosing them in `<!--` `-->`.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<cats>
    <cat>
        <id>1</id>  <!-- Consider making this an attribute -->
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
        <weight>4kg</weight>
        <!-- Not sure if we need the weight -->
    </cat>
    ...
</cats>
```

### XML Schema and DTD: the rule sets

We now have a well formed XML document, but it has not been **validated** against a schema yet.

A schema defines the tag names that can be used, in which order elements are used, how often they can occur, which kind of value they contain and vice versa for attributes. It defines the structure of an XML document.

There are several ways of making schemas for use with XML documents. The most common ones are **DTD** and **XML Schema Definition**s. 

DTD is the older one with less features, but it is still sometimes encountered. We're not going to run through this in detail, I'll just show an example for our cat data and talk you through it. XML Schema is a lot more common and we'll spend more time on this. 

#### DTD

DTD stands for **D**ocument **T**ype **D**efinition. Unlike XML Schema it can live inside the XML document itself rather than be a separate file (though that is also possible); XML Schemas are always separate from the XML documents they define.

Let's say the following is the DTD file `cats.dtd`:

```dtd
<!DOCTYPE cats
[
<!ELEMENT cat (name, age, colour+)>
<!ATTLIST cat id ID #REQUIRED>
<!ATTLIST cat colour (black|white|orange|blue) "black">
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
<!ELEMENT breed (#PCDATA)>
]
>
```

To reference it and validate our XML document with it we put the following into the XML:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE cats SYSTEM "cats.dtd">
<cats>
    <cat id="1">
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
    </cat>
    ...
</cats>
```

In this case the XML document and the `cats.dtd` file live in the same place, so it's ok to give just the name. If the DTD file is elsewhere you'll need to give the path or web address it lives at.

##### Doctype declaration

`!DOCTYPE elementName` declare the root element name followed by a list `[ ]` of its elements and attributes. It is opened by `<` and closed by `>`.

##### Element declarations

`!ELEMENT elementName elementCategory` or `!ELEMENT elementName (elementContent)` declares an element. 

`<!ELEMENT cat (name, age, colour+)>` declares the element `cat` which has the child elements `name`, `age` and `colour`. The `+` after `colour` is a quantifiers that means that each `cat` must have at least one `colour` child element. As there is no symbol behind `name` and `age` this means they must occur exactly once in each `cat` element. The child elements must also occur within the `cat` element in the order given. There are other quantifiers apart from `+`:

| Quantifier | Meaning                  |
| ---------- | ------------------------ |
| +          | at least one occurrence  |
| *          | zero or more occurrences |
| ?          | zero or one occurrence   |

> [!TIP]
>
> We'll meet these quantifiers again when talking about regular expressions. They are pretty universal and worth remembering.

One can also declare that one or more of a selection of child elements must occur. `<!ELEMENT note (from, to (message|greeting))>` means that the element `note` must have exactly one child element `from`, exactly one child element `to` and either a child element `message` or `greeting`, but not both. 

> [!TIP]
>
> `|` (pipe) meaning "or" is also a good one to remember and one we'll meet again in regular expressions.

`<!ELEMENT break EMPTY>` declares that the element `break` is an empty element that cannot have a value or child elements.

`<!ELEMENT name (#PCDATA)>` declares that the element `name` has content, but no child elements. `PCDATA` stands for Parsed Character Data. 

> [!TIP]
>
> You might see constructs like `<[!CDATA[some text here]]>` in XML files. `CDATA` is the opposite of `PCDATA`: it is data that could be interpreted as markup, but should not be. The parser doesn't check if there are any characters in there that would signal the start of a new element etc., it just takes the data as literal.

`<!ELEMENT cat ANY>` declared that the element `cat` can have a value, or child elements or both; basically anything that is parseable is possible.

`<!ELEMENT name (#PCDATA|firstname|lastname|title)*>` declared that the element `name` must have at least one of the following: a value (`PCDATA`), a child element `firstname`, a child element `lastname` or a child element `title`.

##### Attribute declarations

The pattern for attribute declarations is `<!ATTLIST elementName attributeName attributeType attributeValue>`

`<!ATTLIST cat id ID #REQUIRED>` declares that the element `cat` has the attribute `id` which is of data type `ID` and must be given.

`<!ATTLIST cat breed (moggy|Korat|Munchkin) "moggy">` declares that the element `cat` has the attribute `breed` which must have one of the values `moggy`, `Korat` or `Munchkin` and the default value is `moggy`.

`<!ATTLIST cat breed CDATA #IMPLIED>` declares that the element `cat` has the attribute `breed`, which is optional (``#IMPLIED``). 

`<!ATTLIST cat hungry CDATA "yes">` declares that the element `cat` has the attribute `hungry` with the default value "yes". 

`<!ATTLIST cat hungry CDATA #FIXED "yes">` declares that the element `cat` has the attribute `hungry` which must always be (is fixed) "yes". 

##### Embedding the DTD in the XML document

The example above is for a DTD in a separate file. DTDs can be embedded into the XML file directly like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE cats [
<!ELEMENT cat (name, age, colour+)>
<!ATTLIST cat id ID #REQUIRED>
<!ATTLIST cat colour (black|white|orange|blue) "black">
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
<!ELEMENT breed (#PCDATA)>
]>
<cats>
    <cat id="1">
        <name>Duncan</name>
        <age>10</age>
        <breed>moggy</breed>
        <colour>orange</colour>
        <colour>white</colour>
    </cat>
    ...
</cats>
```

##### DTD disadvantages and further reading

DTD is an old way of doing things that has some distinct disadvantages over XML Schema:

- it does not support namespaces (we'll get to those in a minute),
- is a bit of a security risk (DOS attacks via exponentially expanding nested entities or providing the parser with an external resource that doesn't return),
- it knows only one data type: string,
- it lack expressiveness and readability,
- it uses regular expression syntax (remember `+`, `*`, `?` and `|`?), which makes it hard to parse.

There is more to it then what's covered above, if you want to know more, W3Schools has a pretty accessible tutorial: [https://www.w3schools.com/xml/xml_dtd_intro.asp](https://www.w3schools.com/xml/xml_dtd_intro.asp) and the Wikipedia entry is pretty comprehensive too: [https://en.wikipedia.org/wiki/Document_type_definition](https://en.wikipedia.org/wiki/Document_type_definition). The actual specification is part of the XML 1.0 specification [https://www.w3.org/TR/xml/](https://www.w3.org/TR/xml/). Be warned though! It's not easy to tease out and barely comprehensible, stick with the tutorials, seriously!

### XML Schema Definition (XSD)

XSD stands for **X**ML **S**chema **D**efinition and is the current standard to define the structure of an XML document. It defines:

- the **allowed** **elements** and attributes,
- the **order** and **number** of child elements,
- the **data types** of elements and attributes, and
- **default** and **fixed values** of elements and attributes.

**XSDs are themselves XML documents.** The schema defining their structure is the XML Schema schema. So what rules is this based on? Well, itself. Yes, that's somewhat confusing: a definition uses itself as the rules. But really it's no different that defining e.g. the English grammar rules using the English language which uses the English grammar rules, i.e. the very thing it is defining.

As mentioned before, XML Schema has **advantages** over its predecessor DTD:

- no need to learn another syntax, XML Schemas are XML documents,
- it supports data types,
- it supports namespaces (nearly there),
- it is extensible as it is XML based,
- it allows for more fine-grained definitions of elements and attributes.

### Namespaces

#### What is a namespace?

Namespaces are **prefixes** that keep **identifiers** from different schemas **apart** and thus **prevent name conflicts** and **ensure identifiers are unique**. 

These **prefixes are URIs** (**U**niform **R**esource **I**dentifier) and commonly of the URL (**U**niform **R**esource **L**ocator) subset, i.e. a **web address**. Web addresses are unique: every URL points to a single location, it cannot point to more than one location. Thus using them as a prefix means the identifiers (tags in this case) they are prefixing become unique. 

The **URI** used for this **does not need to be real**, its sole purpose is to give the namespace a unique name. It can be entirely fictional. However, for many schemas the URI is used and functional and contains information about the schema.

If you're using an XML Schema someone else has put together, the URI to use will be stated in the XML Schema Definition file in its root tag `<xs:schema>`'s `targetNamespace` attribute. 

For example, if one wanted to use elements defined in [MARCXML](https://www.loc.gov/standards/marcxml/) to record the favourite book of each cat, the namespace to use is [http://www.loc.gov/MARC21/slim](http://www.loc.gov/MARC21/slim). It's pretty cumbersome to prepend this every time and there is still no pointer to the schema that defines MARCXML and would allow validation:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<cats>
    <cat>
        ...
        <favouriteBook>
            <http://www.loc.gov/MARC21/slim/record>
    			<http://www.loc.gov/MARC21/slim/datafield ind1="1" ind2="4" tag="245">
        			<http://www.loc.gov/MARC21/slim/subfield code="a">The hobbit, or, There and back again /</http://www.loc.gov/MARC21/slim/subfield>
        			<http://www.loc.gov/MARC21/slim/subfield code="c">by J.R.R. Tolkien ; illustrations by the author.</http://www.loc.gov/MARC21/slim/subfield>
    			</http://www.loc.gov/MARC21/slim/datafield>
			</http://www.loc.gov/MARC21/slim/record>
        </favouriteBook>
    </cat>
</cats>

```

#### Prefixed namespaces, qualified names

Instead of writing it out every time, one defines a **shorthand for the namespace** that is used instead of the URI itself. When declaring an element this shorthand and `:` are prepended to the element name. E.g. if we define that the shorthand for `http://www.loc.gov/MARC21/slim` is `marc` we can now declare a record element as `<marc:record></marc:record>`.

**Both** the **shorthand** and the **actual namespace**, i.e. the URI, are **referred to as "namespace"**. 

> [!IMPORTANT]
>
> An element that uses a so **prefixed** tag name is a **qualified element**. And the prefixed **tag name** is referred to as a **qualified name** or **QName**. 

#### The default namespace, unqualified names

Besides these **prefixed namespaces** there can also be a **default namespace**. The default namespace can be bound to a URI, but the tag names in it have **no shorthand prefix**.[^i]

> [!IMPORTANT]
>
> They are thus referred to as **unqualified names**. 

#### Defining namespaces and linking XSD files for validation

The namespace and shorthand definition is usually done in the root element or the highest parent element whose children need to use the respective namespace. 

To **define the default namespace** add `xmlns="URI"` as an attribute to the root element or respective parent. E.g. to declare that the default namespace is `http://www.example.com` you would write `xmlns="http://www.example.com"`.

To **define a prefixed namespace** add `xmlns:prefix="URI"` to the root element or respective parent. E.g. to declare the namespace `http://www.loc.gov/MARC21/slim` with the prefix `marc` you would write `xmlns:marc="http://www.loc.gov/MARC21/slim"`.

At the same time one also sets the **pointer to the XML Schema file** defining the respective element(s) in the namespace. To do this we need an attribute that is defined in the XML Schema Instance schema, namely `schemaLocation`. Thus we need to define a prefixed namespace for it before we can use it: `xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`.[^2] 

> [!NOTE]
>
> `xsi` is the prefix conventionally used to reference the XML Schema Instance. Technically one can use whatever prefix one wants, but over time conventions have been established for often used schemas/vocabularies.

Now we can point to both the XSD file for our default namespace and the `marc` namespace using the `xsi:schemaLocation` attribute. Each "pointer" has two components:

- the respective namespace **URI**, and
- the **location** of the XSD file. This can be a URL as well, or a path to the respective file.

They are **separated** by a **space** from **each other** as well as the **next "pointer"** (if more than one XSD needs to be referenced).

`xsi:schemaLocation="https://www.example.com cats.xsd http://www.loc.gov/MARC21/slim https://www.loc.gov/standards/marcxml/schema/MARC21slim.xsd"`

Putting this all together for our example we end up with:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<cats xmlns="http://www.example.com" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:marc="http://www.loc.gov/MARC21/slim" xsi:schemaLocation="https://www.example.com cats.xsd http://www.loc.gov/MARC21/slim https://www.loc.gov/standards/marcxml/schema/MARC21slim.xsd">
<cat>
        ...
        <favouriteBook>
            <marc:record>
                <marc:datafield ind1="1" ind2="4" tag="245">
                    <marc:subfield code="a">The hobbit, or, There and back again /</marc:subfield>
                    <marc:subfield code="c">by J.R.R. Tolkien ; illustrations by the author.</marc:subfield>
                </marc:datafield>
            </marc:record>
        </favouriteBook>
    </cat>
</cats>
```

You might be wondering why we pointed to XSD files for the default namespace and `marc` but not `xsi`. The reason is that the pointer to the XSD for `xsi` is built-in. As long as you use `xsi` as the prefix, there is no need to point to the schema file for it.

### Creating an XML Schema Definition

#### `xs:schema`

Every XML Schema Definition's **root element is `<xs:schema>`**, with attributes defining 

- the `xs` namespace (so we can use the elements and attributes defined there) `xmlns:xs="http://www.w3.org/2001/XMLSchema"`, 
- the schema's default namespace `xmlns="https://www.example.com"`, 
- the namespace the elements, attributes etc. in the schema belong to `targetNamespace="https://www.example.com"` (this is the namespace that should be used to reference this schema in XML documents using it),
- some default values that guide how to XSD validation behaves `elementFormDefault="qualified"` and `attributeFormDefault="unqualified"`.[^ii]

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
</xs:schema>
```

#### Defining elements `xs:element`

Elements are defines using the `xs:element` element. 

To define our first element `cats` we add an `xs:element` element with the `name` attribute containing the name of the defined element:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
    </xs:element>
</xs:schema>
```

There are two types of elements in XML Schema:

- **simple elements**,
  - **can only contain a value**. They cannot contain other elements or have attributes.
  - the value can be defined as being of a **specific data type**. XML Schema has built-in data types than can be used or one can define one's own data type.
  - the value can be **restricted** to match a pattern. One can specify that the value must e.g. be an integer between 0 and 10, or a string exactly 5 characters long.
- **complex elements**
  - **contain** other **elements** and/or **have attributes**.
  - can be:
    - **empty elements**
    - elements only containing other elements
    - elements only containing a value
    - elements containing both other elements and value(s)

As our root element `cats` contains other elements (`cat`) it must be a complex element.

```xml  
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Now we need to indicate in which fashion the child elements can appear:

- `<xs:all>` means that the children can appear in any order, but each only once. That's no good for our example as we have several cats, so `<cats>` must be repeatable.
- `<xs:choice>` means that either one child or another can occur. Again not good for our example.
- `<xs:sequence>` means that the children must occur in the order given, but there is no limitation on occurrences. That'll work for our example, so let's go with this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

To define the `cat` element we use the same tags and attribute at for `cats`, just swap the attribute value to 'cat'. `cat` also needs to be a complex type as it contains other elements (`name`, `age`, `breed`, and `colour`) and the `<xs:sequence>` indicator fits best again too.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat">
                    <xs:complexType>
                        <xs:sequence>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

The child elements of `cat` only contain values, so they can be simple elements. Unless there need to be restrictions defined for a simple element the entire element definition can be expressed in one line giving the name of the element and its data type:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer"/>
                            <xs:element name="breed" type="xs:string"/>
                            <xs:element name="colour" type="xs:string"/>
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

#### XML Schema data types

As mentioned before XML Schema has built-in data types. Common ones are:

- `xs:string`
- `xs:normalizedString` (remove line feeds, carriage returns, and tab characters)
- `xs:integer`
- `xs:boolean`
- `xs:decimal`
- `xs:date` (2026-05-17, 2026-05-17Z -> UTC, 2026-05-07+06:00 -> UTC + 6hrs)
- `xs:time` (10:30:00, 10:30:00Z, 10:30:00-05:00 -> UTC - 5hrs)
- `xs:dateTime` (2026-05-17T10:30:00)

#### Defining attributes `xs:attribute`

Attribute definitions follow the element definitions (they are also not part of the `xs:sequence`, `xs:all` or `xs:choice` block) and use the `xs:attribute` element for their definition. Attribute name and data type use the same syntax as elements.

Our example has the cat IDs as an attribute, so let's add that:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer"/>
                            <xs:element name="breed" type="xs:string"/>
                            <xs:element name="colour" type="xs:string"/>
                        </xs:sequence>
                        	<xs:attribute name="id", type="xs:integer"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

#### Occurence

So far so good, but if we look back at the meaning of `xs:sequence` it means that `cat` can only occur once. That's no good as we have several cats. To fix this we can use the attributes `minOccurs` and `maxOccurs` with element definitions. 

By **default** the value of  `minOccurs`  and `maxOccurs` is `1`. So if neither are defined the element must occur **exactly once**.

If you set `minOccurs` to `0`, the element becomes **optional**, as it's now ok for it to not be there at all. 

For our example, we can't tell how many `cat` elements might be added, so we can use `unbounded` as the value for `maxOccurs`, which means there is **no upper limit** to how often the element can occur. We leave `minOccurs` at `1` by not mentioning it at all. 

For `name` exactly one occurrence seems fine too. Every cat should have a name, but we may not know their age or breed. So let's make those two optional by setting `minOccur`s to `0`.

`colour` should also always be present, but it can also have multiple occurrences. `unbounded` seems a bit over the top here though, there are limited fur colour categories. Let's limit this to 4. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer" minOccurs="0"/>
                            <xs:element name="breed" type="xs:string" minOccurs="0"/>
                            <xs:element name="colour" type="xs:string" maxOccurs="4"/>
                        </xs:sequence>
                        	<xs:attribute name="id", type="xs:integer"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

A similar construct exists for **attributes**, though here the choice is only between **optional** and **required** and the attribute is `use`. The default, i.e. if `use` is not specified, is optional. In our example it would be sensible to require an id, so let's implement that:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer" minOccurs="0"/>
                            <xs:element name="breed" type="xs:string" minOccurs="0"/>
                            <xs:element name="colour" type="xs:string" maxOccurs="4"/>
                        </xs:sequence>
                        	<xs:attribute name="id", type="xs:integer" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

#### Default and fixed values

Both **elements** and **attributes** can be assigned **default values** and **fixed values**. Default values are automatically assigned unless another value is specified. Fixed values mean the respective element or attribute value must be this. 

Let's say most cats we encounter and add to our example are moggies. It would make sense to set the default value of `breed` to `moggy` so we don't have to enter it all the time. The syntax is the same for elements and attributes.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" type="xs:integer" minOccurs="0"/>
                            <xs:element name="breed" type="xs:string" minOccurs="0" default="moggy"/>
                            <xs:element name="colour" type="xs:string" maxOccurs="4"/>
                        </xs:sequence>
                        	<xs:attribute name="id", type="xs:integer" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

The syntax to declare a fixed value is `fixed="value"` for both elements and attributes.

#### Restrictions

There is a range of restrictions that can be applied to **simple elements**. Which are possible and make sense depends on the data type of the element. The two we'll cover here are **numerical** restrictions and **enumerations**.

We already set the value of `age` to be in integer, but it would also make sense to restrict the range this can be as well. According to Wikipedia the oldest cat ever got to 38 years and 3 days[^4]. Giving a bit of leeway, let's set an upper limit of 40 and lower limit of 0 for the `age` element.  To do this we need to expand the simple element declaration into several lines:

- the element name and occurrence stays with the `<xs:element>` element, but it now needs a closing tag  (`</xs:element>`) as we'll add child elements to it containing the restriction,
- add a child element to it with the tag `xs:simpleType`,
- add a child to `xs:simpleType` with the tag `xs:restriction`. This is where the data type will live now, but the attribute is now called `base`. The data type is the basis of the restriction. 
- add a child with the tag `xs:minInclusive` and another with the tag `xs:maxInclusive` to the `<xs:restriction>` tag and set the `value` attribute for `xs:minInclusive` to `0` and for `xs:maxInclusive` to `40`. (Yes, `minExclusive` and `maxExclusive` are also options.)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" minOccurs="0">
                                <xs:simpleType>
                                    <xs:restriction base="xs:integer">
                                        <xs:minInclusive value="0"/>
                                        <xs:maxInclusive value="40"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>
                            <xs:element name="breed" type="xs:string" minOccurs="0" default="moggy"/>
                            <xs:element name="colour" type="xs:string" maxOccurs="4"/>
                        </xs:sequence>
                        	<xs:attribute name="id", type="xs:integer" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

For the **enumeration** example we'll restrict the `colour` element to only accept a list of values. Values outside this list will fail validation.

As before we need to expand the simple element into several lines. The `base` attribute value this time is `xs:string` and the child elements of `<xs:restriction>` are all `<xs:enumeration>` elements:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="https://www.example.com" targetNamespace="https://www.example.com" elementFormDefault="qualified" attributeFormDefault="unqualified">
    <xs:element name="cats">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="cat" maxOccur="unbounded">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="name" type="xs:string"/>
                            <xs:element name="age" minOccurs="0">
                                <xs:simpleType>
                                    <xs:restriction base="xs:integer">
                                        <xs:minInclusive value="0"/>
                                        <xs:maxInclusive value="40"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>
                            <xs:element name="breed" type="xs:string" minOccurs="0" default="moggy"/>
                            <xs:element name="colour" maxOccurs="4">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:enumeration value="black"/>
                                        <xs:enumeration value="white"/>
                                        <xs:enumeration value="orange"/> <!-- Technically this is called red, but I like orange better :P -->
                                        <xs:enumeration value="blue"/>
                                        <xs:enumeration value="cream"/>
                                        <xs:enumeration value="lilac"/>
                                        <xs:enumeration value="chocolate"/>
                                        <xs:enumeration value="cinnamon"/>
                                        <xs:enumeration value="fawn"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>
                        </xs:sequence>
                        	<xs:attribute name="id", type="xs:integer" use="required"/>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

### Related specifications

- **XSLT** (E**x**tensible **S**tylesheet **L**anguage **T**ransformations) -> transform from one XML into another
- **XPath** (XML Path Language) -> address/select elements with an XML document

## JSON

JSON stands for **J**ava**S**cript **O**bject **N**otation (it was extended from JavaScript). It was developed 2000/2001 and first standardised in 2013 as ECMA-404 and then again in 2017 as RFC 8259 and ISO-IEC 21778:2017. 

Just like XML its **purpose** is **data serialisation** (i.e. data storage, transmission and reconstruction). 

But, it's a lot easier! No namespaces for starters😋

### JSON Syntax

The basic premise is **key-value pairs**. You've seen this in action as attributes in XML: the attribute name is the key and the value, well, the value. Key and value are separated by `:`. 

**Keys must be strings** in JSON, but **values** can have a number of different **data types**:

- **string** values are enclosed in `""` (`"` occurring in a string value must be escaped as `\"`),
- **number** (there is no distinction between integer and float),
- **Boolean** (`true` or `false`),
- **array**s are enclosed in `[ ]` and can consist of values of all data types in this list, 
- **object**s are enclosed in `{ }` and consist of key-value pairs whose keys should be unique within the object, and
- `null`, which means no value (slightly different from the key not existing at all)

The values in arrays and objects are separated by `,`.

The **root** element must either be an **object** or an **array**.

There is **no provision for comments** in JSON.

That's it. That's all the rules.[^iii] 

### JSON example

Using our cats example, let's start by describing an individual cat. We'll need keys for id, name, age, breed and colour; they are already strings, so let's just use them. And we need to wrap them in an object (`{ }`) as we need a root object or array.

```json
{
    "id": 1,
    "name": "Duncan",
    "age": 10,
    "breed": "moggy",
    "colour": ["orange", "white"]
}
```

That's it. Cat described. 

To add another cat, let's wrap our existing cat object in an array and add a second object describing a cat.

```json
[
    {
        "id": 1,
        "name": "Duncan",
        "age": 10,
        "breed": "moggy",
        "colour": ["orange", "white"]
    },
    {
        "id": 2,
        "name": "Izzy",
        "age": 5,
        "breed": "moggy",
        "colour": ["black"]
    }
]
```

The only two data types that out example doesn't cover yet are Boolean and `null`. 

To demonstrate Boolean, let's add the key "hungry" to each cat. Duncan is hungry and Izzy is not:

```json
[
    {
        "id": 1,
        "name": "Duncan",
        "age": 10,
        "breed": "moggy",
        "colour": ["orange", "white"],
        "hungry": true
    },
    {
        "id": 2,
        "name": "Izzy",
        "age": 5,
        "breed": "moggy",
        "colour": ["black"],
        "hungry": false
    }
]
```

Finally, `null`: Say we don't know the age of our cats. There are two options, we simply omit the key or we set its value to `null`.

```json
[
    {
        "id": 1,
        "name": "Duncan",
        "breed": "moggy",
        "colour": ["orange", "white"],
        "hungry": true
    },
    {
        "id": 2,
        "name": "Izzy",
        "age": null,
        "breed": "moggy",
        "colour": ["black"],
        "hungry": false
    }
]
```

What's the difference? You'll mostly notice it when working with JSON data and extracting values from it. If a key doesn't exist you might get an error, where as if it does and the value is `null`, you get the `null` value. 

### Validating JSON

The validation vocabulary for JSON is called **JSON Schema**. JSON Schema documents are JSON documents. I have yet to encounter a situation where I had to work with this beyond experimentation, hence we'll not dwell on it. I'll just show you what it looks like and talk you through. 

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$id": "https://www.example.com/cats.json",
    "title": "Cats",
    "description": "Describe some cats",
    "type": "array",
    "properties": {
        "id": {
            "description": "The cat's ID",
            "type": "integer",
            "minimum": 1
        },
        "name": {
            "description": "The cat's name",
            "type": "string"
        },
        "age": {
            "description": "The cat's age",
            "type": "integer",
            "minimum": 0
        },
        "breed": {
            "description": "The cat's breed",
            "type": ["string", "null"]
        },
        "colour": {
            "description": "The cat's colour(s)",
            "type": "array",
            "items": {
                "enum":[
                    "black",
                    "white",
                    "orange",
                    "blue",
                    "cream",
                    "lilac",
                    "chocolate",
                    "cinnamon",
                    "fawn"
                ]
            }
        }
    },
    "required": [
        "id",
        "name"
    ],
    "additionalProperties": false
}
```

- `"$schema": "https://json-schema.org/draft/2020-12/schema"` defines the version of JSON Schema this document uses.
- `"$id": "https://www.example.com"` sets a unique identifier for the schema
- `"title"` and `"description"` is metadata about the schema, i.e. giving it a title and saying something about its purpose etc.
- `"type": "array"` means it expects an array as the root element
- `"properties": {...}` describes the keys and which data format their values are and any other restrictions placed on them. The keys described are:
  - `"id"`, whose value should be an integer with a minimum value of 1,
  - `"name"`, whose value should be a string,
  - `"age"`, whose value should be an integer with the minimum allowed value being 0,
  - `"breed"`, whose value should be a string or `null`,
  - `"colour"`, whose value should be an array whose value(s) should be `black`, `white`, `orange`, `blue`, `cream`, `lilac`, `chocolate`, `cinnamon` or `fawn`.
- `"required": ["id", "name" ]` means that the keys `id` and `name` must be present, for each cat, all others are optional.
- `additionalProperties": false"` means no keys other than the ones given are allowed.

## XML vs JSON

If your use case doesn't dictate which format to use, how do you decide? There are some significant differences between XML and JSON that usually make JSON the better and easier to work with choice, especially if you also work with the data programmatically.

|                        | XML                                                       | JSON                                                         |
| ---------------------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| Syntax and readability | Verbose and can be hard to read due to tag structure      | Compact and easier to read due to key-value pair syntax      |
| File size              | Larger, due to verbosity                                  | Smaller                                                      |
| Parsing                | Needs a complex parser to read and write programmatically | Is simpler and quicker to parse programmatically and maps easily into languages such as JavaScript and Python |
| Data type support      | More flexible and supports complex data types             | Limited range of data types                                  |
| Ease of use            | More complex and less flexible                            | Simple and flexible                                          |
| Validation             | Can be validated using XML Schema                         | Can be validated using JSON Schema                           |
| Security               | Has some security concerns re DTDs, don't use them.       | Is safer than XML                                            |

This might make it sound like JSON is always the better choice, but that's not true. If you need namespaces, data types JSON cannot support or very complex data structures, XML does the better job. 

## Footnotes

[^i]: The default namespace only works for elements. Namespaces for attributes work slightly differently. If you use an attribute that belong as per the schema to the respective element there is no need to prefix it, it is in this element's context. If you want to use an attribute from a different namespace though (as we e.g. do with `xsi:schemaLocation`) you need to prefix it. [https://stackoverflow.com/questions/3312390/xml-default-namespaces-for-unqualified-attribute-names](https://stackoverflow.com/questions/3312390/xml-default-namespaces-for-unqualified-attribute-names)[^82] has a good explanation of this.
[^ii]: What these two do goes much deeper than the scope of this lesson. If you want to know more see e.g. https://stackoverflow.com/questions/1463138/what-does-elementformdefault-do-in-xsd[^74].
[^iii]: There are some intricacies of which characters can be used for numbers and encoding bits for strings. None of which are terribly relevant for bibliographic use cases. See []. 

## Annotated bibliography and further reading

### Data types

**BBC Bitesize** has some very gentle introductions to data types and structures that cover strings, integers, floats, doubles, Booleans and arrays:

[^1]: 'Implementation: Data types and structures' (no date) *BBC Bitesize*. Available at: [https://www.bbc.co.uk/bitesize/guides/zghbgk7](https://www.bbc.co.uk/bitesize/guides/zghbgk7) [Accessed: 24 May 2026]
[^2]: 'Data types, structures and operators' (no date) *BBC Bitesize*. Available at: [https://www.bbc.co.uk/bitesize/guides/z788jty](https://www.bbc.co.uk/bitesize/guides/z788jty) [Accessed: 24 May 2026] -- The "Data types", "Arrays" and "Using operators" chapters are of interest for the topic. The other chapters are specifically geared towards VBA (Visual Basic) programming in Excel.

Discussions of data types and structures are usually **specific to a programming language or application**. 

**Wikipedia** articles on the various data types and structures are quite good at conveying both the basics and going into some detail for specific languages. Some of the detail is very deep though. The articles concerning the data types we discussed are:

[^3]: ‘Array (data structure)’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Array_(data_structure)](https://en.wikipedia.org/wiki/Array_(data_structure)) [Accessed: 24 May 2026]
[^4]: 'Array (data type)' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Array_(data_type)](https://en.wikipedia.org/wiki/Array_(data_type)) [Accessed: 24 May 2026]
[^5]: 'Boolean data type' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Boolean_data_type](https://en.wikipedia.org/wiki/Boolean_data_type) [Accessed: 24 May 2026]
[^6]: ‘Data structure’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Data_structure](https://en.wikipedia.org/wiki/Data_structure) [Accessed: 24 May 2026] -- This article has links to articles to other data structure mentioned but not discussed.
[^7]: ‘Data type’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Data_type](https://en.wikipedia.org/wiki/Data_type) [Accessed: 24 May 2026]
[^8]: ‘Hash table’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Hash_table](https://en.wikipedia.org/wiki/Hash_table) [Accessed: 24 May 2026]
[^9]:'Integer' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Integer](https://en.wikipedia.org/wiki/Integer) [Accessed: 24 May 2026]
[^10]: ‘Integer (computer science)’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Integer_(computer_science)](https://en.wikipedia.org/wiki/Integer_(computer_science)) [Accessed: 24 May 2026]
[^11]: 'Linked list' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Linked_list](https://en.wikipedia.org/wiki/Linked_list) [Accessed: 24 May 2026]
[^12]: 'List (abstract data type)' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/List_(abstract_data_type)](https://en.wikipedia.org/wiki/List_(abstract_data_type)) [Accessed: 24 May 2026]
[^13]: 'Primitive data type' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Primitive_data_type](https://en.wikipedia.org/wiki/Primitive_data_type) [Accessed: 24 May 2026]
[^14]:'String (computer science)' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/String_(computer_science)](https://en.wikipedia.org/wiki/String_(computer_science)) [Accessed: 24 May 2026]

Finally, I found this article by Andrii Chornyi really useful to understand the difference between float and double:

[^15]: Chornyi, Andrii (2024) ‘Float vs Double: a comprehensive guide’, *Codefinity blog*, August. Available at: https://codefinity.com/blog/Float-vs-Double [Accessed: 24 May 2026]

### Data encoding

BBC Bitesize again has two gentle introductions to **how data is stored by computers**. They introduce **bits** and **bytes**, **binary** and **hexadecimal** number systems as well as ASCII and the basics of image and sound storage. W3School's "Introduction to Programming" also has a very accessible intro to data storage (and it might lead you to reading more about programming concepts).

[^16]: ‘Binary and data representation’ (no date) *BBC Bitesize*. Available at: [https://www.bbc.co.uk/bitesize/guides/z6qqmsg](https://www.bbc.co.uk/bitesize/guides/z6qqmsg) [Accessed: 24 May 2026]
[^17]: ‘Fundamentals of data representation’ (no date) *BBC Bitesize*. Available at: [https://www.bbc.co.uk/bitesize/guides/zd88jty](https://www.bbc.co.uk/bitesize/guides/zd88jty) [Accessed: 24 May 2026]
[^18]: ‘What are Bits and Bytes?’ (no date) *W3Schools*. Available at: [https://www.w3schools.com/programming/prog_bits_and_bytes.php](https://www.w3schools.com/programming/prog_bits_and_bytes.php) [Accessed: 24 May 2026]

Moving on to actual **encodings** and code points, I found the following useful. They vary a bit in how deep they go and how techy they get, but all are on the accessible side:

[^19]: Ishida, Richard (2018) *Character encodings: Essential concepts*. Available at: [https://www.w3.org/International/articles/definitions-characters/](https://www.w3.org/International/articles/definitions-characters/) [Accessed: 24 May 2026] -- This explains Unicode and how UTF-8, UTF-16 and UTF-32 work.
[^20]: Ishida, Richard (2020) *Character encodings for beginners*. Available at: https://www.w3.org/International/questions/qa-what-is-encoding [Accessed: 24 May 2026] -- This one is more generally about character encoding and also explores the relationship between encoding and fonts.
[^21]: Ishida, Richard (2024) *Character Sets and Encodings*. Available at: [https://www.w3.org/International/getting-started/characters](https://www.w3.org/International/getting-started/characters) [Accessed: 24 May 2026] -- This is more of a landing page with links to more in-depth information. As this is a W3C site there is a lot of emphasis on web development and how to deal with character encoding in HTML.
[^22]: Krukowski, Ilya (2025) ‘Character encoding: Types, UTF-8, Unicode, and more explained’, *Lokalise blog*, 7 April. Available at: [https://lokalise.com/blog/what-is-character-encoding-exploring-unicode-utf-8-ascii-and-more](https://lokalise.com/blog/what-is-character-encoding-exploring-unicode-utf-8-ascii-and-more) [Accessed: 24 May 2026]
[^23]: Spolsky, Joel (2003) ‘The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)’, Joel on software, 8 October. Available at: [https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/) [Accessed: 24 May 2026] -- Quite old, but still very much true and often referenced.
[^24]: Zentgraf, David C. (2015) *What every programmer absolutely, positively needs to know about encodings and character sets to work with text*. Available at: [https://kunststube.net/encoding/](https://kunststube.net/encoding/) [Accessed: 24 May 2026]

The **Wikipedia** articles on the subject all take you a lot deeper into the topic and may be more suitable once you got the basics and want to know more.

[^25]: ‘ASCII’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/ASCII](https://en.wikipedia.org/wiki/ASCII) [Accessed: 24 May 2026]
[^26]: ‘Bit’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Bit](https://en.wikipedia.org/wiki/Bit) [Accessed: 24 May 2026]
[^27]: ‘Byte’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Byte](https://en.wikipedia.org/wiki/Byte) [Accessed: 24 May 2026]
[^28]: ‘Character encoding’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Character_encoding](https://en.wikipedia.org/wiki/Character_encoding) [Accessed: 24 May 2026]
[^29]: ‘Comparison of Unicode encodings’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Comparison_of_Unicode_encodings](https://en.wikipedia.org/wiki/Comparison_of_Unicode_encodings) [Accessed: 24 May 2026]
[^30]: ‘Mojibake’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Mojibake](https://en.wikipedia.org/wiki/Mojibake) [Accessed: 24 May 2026]
[^31]: ‘Unicode’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Unicode](https://en.wikipedia.org/wiki/Unicode) [Accessed: 24 May 2026]
[^32]: ‘UTF-8’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/UTF-8](https://en.wikipedia.org/wiki/UTF-8) [Accessed: 24 May 2026]
[^33]: ‘Windows-1252’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Windows-1252](https://en.wikipedia.org/wiki/Windows-1252) [Accessed: 24 May 2026]

Finally, some further sources I used to put the workshop together.

[^34]: ‘code, n., sense II.4.b’ (2026) in *Oxford English Dictionary*. Available at: [https://doi.org/10.1093/OED/1750064856](https://doi.org/10.1093/OED/1750064856) [Accessed: 24 May 2026]
[^35]: ‘code, n., sense II.7’ (2026) in *Oxford English Dictionary*. Available at: [https://doi.org/10.1093/OED/8027718018](https://doi.org/10.1093/OED/8027718018) [Accessed: 24 May 2026]
[^36]: ‘decode, v.’ (2025) in *Oxford English Dictionary*. Available at: [https://doi.org/10.1093/OED/3318848028](https://doi.org/10.1093/OED/3318848028) [Accessed: 24 May 2026]
[^37]: ‘encode, v.’ (2025) in *Oxford English Dictionary*. Available at: [https://doi.org/10.1093/OED/7215232060](https://doi.org/10.1093/OED/7215232060) [Accessed: 24 May 2026]
[^38]: *FAQ* (no date) Available at: [https://home.unicode.org/basic-info/faq/](https://home.unicode.org/basic-info/faq/) [Accessed: 24 May 2026]
[^39]: Library of Congress Network Development and MARC Standards Office (2000) *MARC 21 Specification for Record Structure, Character Sets, and Exchange Media*. Available at: [https://www.loc.gov/marc/specifications/spechome.html](https://www.loc.gov/marc/specifications/spechome.html) [Accessed: 24 May 2026]
[^40]: MrAureliusR (2025) *ASCII-Table-wide.svg*. Available at: [https://commons.wikimedia.org/wiki/File:ASCII-Table-wide.svg](https://commons.wikimedia.org/wiki/File:ASCII-Table-wide.svg) [Accessed: 24 May 2026]
[^41]: *Supported Scripts* (no date) Available at: [https://www.unicode.org/standard/supported.html](https://www.unicode.org/standard/supported.html) [Accessed: 24 May 2026]
[^42]: *Unicode 17.0 Character Code Charts* (no date) Available at: [https://www.unicode.org/charts/](https://www.unicode.org/charts/) [Accessed: 24 May 2026]
[^43]: *Unicode 17.0 Versioned Charts Index* (no date) Available at: [https://www.unicode.org/charts/PDF/Unicode-17.0/](https://www.unicode.org/charts/PDF/Unicode-17.0/) [Accessed: 24 May 2026]
[^44]: *Unicode Code Charts Help and Links* (no date) Available at: [https://unicode.org/charts/About.html](https://unicode.org/charts/About.html) [Accessed: 24 May 2026]
[^45]: *Unicode Planes* (no date) Available at: [https://codepoints.net/planes](https://codepoints.net/planes) [Accessed: 24 May 2026]
[^46]: *Unicode Statistics* (2025) Available at: [https://www.unicode.org/versions/stats/](https://www.unicode.org/versions/stats/) [Accessed: 24 May 2026]

### Data formats in general

Some articles explaining the end of line (EOL) character problem of **line feed** (LF) and **carriage return** (CR):

[^47]: *Carriage Returns: A Comprehensive Guide to Carriage Returns, Line Breaks and CRLF* (2025) Available at: [https://www.joshuahumphrey.co.uk/carriage-returns/](https://www.joshuahumphrey.co.uk/carriage-returns/) [Accessed: 24 May 2026]
[^48]: Meszaros, Bence (2021) 'Understanding digital line breaks: Carriage return, line feed, newline, \<br>, hard and soft breaks and all the line break mumbo-jumbo you can think of', *Medium*, 14 June. Available at: [https://decketts.medium.com/understanding-digital-line-breaks-5ddfaa66677b](https://decketts.medium.com/understanding-digital-line-breaks-5ddfaa66677b) [Accessed: 24 May 2026] -- This bring the HTML line break \<br> into the mix as well.
[^49]: ‘Newline’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Newline](https://en.wikipedia.org/wiki/Newline) [Accessed: 24 May 2026]

And some randomness I looked at and found useful while putting the workshop together:

[^50]: ‘Comparison of data-serialization formats’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Comparison_of_data-serialization_formats](https://en.wikipedia.org/wiki/Comparison_of_data-serialization_formats) [Accessed: 24 May 2026]

### Tabular data

The Wikipedia articles here to a very good job at explaining how CSV and TSV, and indeed any delimiter-separated format, work(s):

[^51]: ‘Comma-separated values’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Comma-separated_values](https://en.wikipedia.org/wiki/Comma-separated_values) [Accessed: 24 May 2026] 
[^52]:‘Delimiter’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Delimiter](https://en.wikipedia.org/wiki/Delimiter) [Accessed: 24 May 2026]
[^53]:‘Delimiter-separated values’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Delimiter-separated_values](https://en.wikipedia.org/wiki/Delimiter-separated_values) [Accessed: 24 May 2026]
[^54]:‘Tab-separated values’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Tab-separated_values](https://en.wikipedia.org/wiki/Tab-separated_values) [Accessed: 24 May 2026]
[^55]:‘Table (information)’ (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Table_(information)](https://en.wikipedia.org/wiki/Table_(information)) [Accessed: 24 May 2026]

Both CSV and TSV have no proper **standard definition**. The closest each has is still extremely readable and understandable as far as standards go. These are not scary:

[^56]: Shafranovich, Yakov (2005) *RFC 4180: Common Format and MIME Type for Comma-Separated Values (CSV) Files*. Available at: [https://doi.org/10.17487/RFC4180](https://doi.org/10.17487/RFC4180) [Accessed: 24 May 2026] -- The closest CSV has to a standard definition.
[^57]:University of Minnesota Internet Gopher Team (no date) *Definition of tab-separated-values (tsv)*. Available at: [https://www.iana.org/assignments/media-types/text/tab-separated-values](https://www.iana.org/assignments/media-types/text/tab-separated-values) [Accessed: 24 May 2026] -- The closest TSV has to a standard definition.

Further sources I used to compile the workshop and best practice documents for CSV:

[^58]:*CSV Format: History, Advantages and Why It Is Still Popular* (no date) Available at: [https://bytescout.com/blog/csv-format-history-advantages.html](https://bytescout.com/blog/csv-format-history-advantages.html) [Accessed: 24 May 2026]
[^59]:Pollock, Rufus (2021) *CSV Dialect*. Available at: [https://specs.frictionlessdata.io/csv-dialect/](https://specs.frictionlessdata.io/csv-dialect/) [Accessed: 24 May 2026] -- Defines a standard JSON-based format to specify CSV dialects.
[^60]:Tennison, Jeni (2016) *CSV on the Web: A Primer*. Available at: [https://www.w3.org/TR/2016/NOTE-tabular-data-primer-20160225/](https://www.w3.org/TR/2016/NOTE-tabular-data-primer-20160225/) [Accessed: 24 May 2026] -- This is pretty techy and mostly concerned with dealing with CSVs on the web.
[^61]:Tennison, Jeni, Kellogg, Greg and Herman, Ivan (2015) ‘Best Practice CSV’ in J. Tennison, G. Kellogg and I. Herman (eds.), *Model for Tabular Data and Metadata on the Web*. Available at: [https://www.w3.org/TR/2015/REC-tabular-data-model-20151217/#syntax](https://www.w3.org/TR/2015/REC-tabular-data-model-20151217/#syntax) [Accessed: 24 May 2026] -- This chapter is short, techy recap of CSV, the rest is way too techy for the scope of this webinar.
[^62]:Walsh, Paul, Pollock, Rufus and Keegan, Martin (2017) *Tabular Data Package*. Available at: [https://specs.frictionlessdata.io/tabular-data-package/](https://specs.frictionlessdata.io/tabular-data-package/) [Accessed: 24 May 2026] -- Similar to "CSV Dialect", but this time concerned with describing the data itself.

### XML

My default XML, DTD and XML Schema references are the **W3Schools' XML tutorials**:

[^63]: 'XML Tutorial' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/default.asp](https://www.w3schools.com/xml/default.asp) [Accessed: 24 May 2026]
[^64]: 'DTD Tutorial' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/xml_dtd_intro.asp](https://www.w3schools.com/xml/xml_dtd_intro.asp) [Accessed: 24 May 2026]
[^65]: 'XML Schema Tutorial' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/schema_intro.asp](https://www.w3schools.com/xml/schema_intro.asp) [Accessed: 24 May 2026]

There are a lot of other tutorials out there though. Ones I found useful before are the TutorialsPoint ones:

[^66]: 'XML Tutorial' (no date) *TutorialsPoint*. Available at: [https://www.tutorialspoint.com/xml/index.htm](https://www.tutorialspoint.com/xml/index.htm) [Accessed: 24 May 202
[^67]:'DTD Tutorial' (no date) *TutorialsPoint*. Available at: [https://www.tutorialspoint.com/dtd/index.htm](https://www.tutorialspoint.com/dtd/index.htm) [Accessed: 24 May 2026]
[^68]:'XSD Tutorial' (no date) *TutorialsPoint*. Available at: [https://www.tutorialspoint.com/xsd/index.htm](https://www.tutorialspoint.com/xsd/index.htm) [Accessed: 24 May 2026]

[GeeksforGeeks](https://www.geeksforgeeks.org/) is sometimes useful as well and, of course the awesomeness that is [StackOverflow](https://stackoverflow.com/) (though the usual way of ending up there is googling something). 

Specifically for **DTD**, the Wikipedia page is basically a tutorial too:

[^69]: 'Document type definition' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Document_type_definition](https://en.wikipedia.org/wiki/Document_type_definition) [Accessed: 24 May 2026]

There are loads of resources on YouTube as well, but I don't learn well that way so I don't have any recommendations. Sorry!

The **technical XML and XSD specifications**. These are on the barely readable end unless you are very techy (and in this case: what are you doing here?)

**XML (and DTD):**

[^70]:*Extensible Markup Language (XML) 1.0* (2008) 5th ed. Available at: [https://www.w3.org/TR/2008/REC-xml-20081126/](https://www.w3.org/TR/2008/REC-xml-20081126/) [Accessed: 24 May 2026]
[^71]:*Extensible Markup Language (XML) 1.1* (2006) 2nd ed. Available at: [https://www.w3.org/TR/2006/REC-xml11-20060816](https://www.w3.org/TR/2006/REC-xml11-20060816) [Accessed: 24 May 2026]

**XML Schema:**

[^72]:*W3C XML Schema Definition Language (XSD) 1.1 Part 1: Structures* (2012) Available at: [https://www.w3.org/TR/2012/REC-xmlschema11-1-20120405/](https://www.w3.org/TR/2012/REC-xmlschema11-1-20120405/) [Accessed: 24 May 2026]
[^73]: *W3C XML Schema Definition Language (XSD) 1.1 Part 2: Datatypes* (2012) Available at: [https://www.w3.org/TR/2012/REC-xmlschema11-2-20120405/](https://www.w3.org/TR/2012/REC-xmlschema11-2-20120405/) [Accessed: 24 May 2026]

Other sources related to XML, DTDs and XML Schema I used for putting the workshop together:

[^74]: kjhughes (2020) *Response to: What does elementFormDefault do in XSD?* Available at: [https://stackoverflow.com/a/46758277](https://stackoverflow.com/a/46758277) [Accessed: 24 May 2026]
[^75]: 'Markup language' (2026) *Wikipedia*. Available at: https://en.wikipedia.org/wiki/Markup_language [Accessed: 24 May 2026]

[^76]: 'XML' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XML](https://en.wikipedia.org/wiki/XML) [Accessed: 24 May 2026]
[^77]: XML Core Working Group Public Page (2017) Available at: [https://www.w3.org/XML/Core](https://www.w3.org/XML/Core) [Accessed: 24 May 2026]
[^78]: 'XML DTD' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/xml_dtd.asp](https://www.w3schools.com/xml/xml_dtd.asp) [Accessed: 24 May 2026]
[^79]: *XML in 10 points* (2014) Available at [https://www.w3.org/XML/1999/XML-in-10-points.html](https://www.w3.org/XML/1999/XML-in-10-points.html) [Accessed: 24 May 2026]
[^80]: 'XML Schema' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/xml_schema.asp](https://www.w3schools.com/xml/xml_schema.asp) [Accessed: 24 May 2026]
[^81]: 'XML Schema (W3C)' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XML_Schema_(W3C)](https://en.wikipedia.org/wiki/XML_Schema_(W3C)) [Accessed: 24 May 2026]

**Namespaces:**

[^82]: mckamey (2019) *XML Default namespaces for unqualified attribute names?* Available at: [https://stackoverflow.com/q/3312390](https://stackoverflow.com/q/3312390) [Accessed: 24 May 2026] -- see [^i]. 
[^83]: 'Namespace' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/Namespace](https://en.wikipedia.org/wiki/Namespace) [Accessed: 24 May 2026] -- This is reasonable accessible, but not specific to XML.
[^84]: *Namespaces in XML 1.0* (2009) 3rd ed. Available at: https://www.w3.org/TR/2009/REC-xml-names-20091208/[](https://www.w3.org/TR/2009/REC-xml-names-20091208/) [Accessed: 24 May 2026] -- This is the W3C technical specification, so pretty hard reading.
[^85]: *Namespaces in XML 1.1* (2006) 2nd ed. Available at: [https://www.w3.org/TR/2006/REC-xml-names11-20060816](https://www.w3.org/TR/2006/REC-xml-names11-20060816) [Accessed: 24 May 2026] -- This is the W3C technical specification for namespaces in XML 1.1, so pretty hard reading.
[^86]: 'XML Namespace' (2025) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XML_namespace](https://en.wikipedia.org/wiki/XML_namespace) [Accessed: 24 May 2026] -- This is reasonably accessible.
[^87]: 'XML Namespaces' (no date) *W3Schools*. Available at: [https://www.w3schools.com/xml/xml_namespaces.asp](https://www.w3schools.com/xml/xml_namespaces.asp) [Accessed: 24 May 2026] -- This is reasonably accessible.

**XML related standards:**

[^88]: 'XPath' (2025) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XPath](https://en.wikipedia.org/wiki/XPath) [Accessed: 24 May 2026]
[^89]: 'XPath 2.0' (2025) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XPath_2.0](https://en.wikipedia.org/wiki/XPath_2.0) [Accessed: 24 May 2026]
[^90]: 'XPath 3' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XPath_3](https://en.wikipedia.org/wiki/XPath_3) [Accessed: 24 May 2026]
[^91]: *XML Path Language (XPath) Version 1.0* (1999) Available at: [https://www.w3.org/TR/1999/REC-xpath-19991116](https://www.w3.org/TR/1999/REC-xpath-19991116) [Accessed: 24 May 2026]
[^92]: *XML Path Language (XPath) 2.0* (2011) 2nd ed. Available at: [https://www.w3.org/TR/2010/REC-xpath20-20101214/](https://www.w3.org/TR/2010/REC-xpath20-20101214/) [Accessed: 24 May 2026]
[^93]: *XML Path Language (XPath) 3.0* (2014) Available at: [https://www.w3.org/TR/2014/REC-xpath-30-20140408/](https://www.w3.org/TR/2014/REC-xpath-30-20140408/) [Accessed: 24 May 2026]
[^94]: *XML Path Language (XPath) 3.1* (2017) Available at: [https://www.w3.org/TR/2017/REC-xpath-31-20170321/](https://www.w3.org/TR/2017/REC-xpath-31-20170321/) [Accessed: 24 May 2026]
[^95]: *XSL Transformations (XSLT) Version 3.0* (2017) Available at: https://www.w3.org/TR/2017/REC-xslt-30-20170608/[](https://www.w3.org/TR/2017/REC-xslt-30-20170608/) [Accessed: 24 May 2026]
[^96]: 'XSLT' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/XSLT](https://en.wikipedia.org/wiki/XSLT) [Accessed: 24 May 2026]

### JSON

[^97]: 'JavaScript JSON' (no date) *W3Schools*. Available at: [https://www.w3schools.com/js/js_json.asp](https://www.w3schools.com/js/js_json.asp) [Accessed: 24 May 2026] -- The W3Schools JSON tutorial is part of the JavaScript one and thus maybe not the best unless you are also interested in JavaScript.
[^98]: 'JSON' (2026) *Wikipedia*. Available at: [https://en.wikipedia.org/wiki/JSON](https://en.wikipedia.org/wiki/JSON) [Accessed: 24 May 2026]
[^99]: 'JSON Cheatsheet' (no date) in *JSON Tutorial*. Available at: [https://www.tutorialspoint.com/json/index.htm](https://www.tutorialspoint.com/json/index.htm) [Accessed: 24 May 2026] -- The syntax summary is useful and has plenty of examples.

**The technical standards:**

[^100]: Bray, Tim (2013) *The JavaScript Object Notation (JSON) Data Interchange Format*. RFC 7158. Available at: [https://www.rfc-editor.org/info/rfc7158/](https://www.rfc-editor.org/info/rfc7158/) [Accessed: 24 May 2026] -- Intermediate proposed standard RFC, now obsolete, current standard is [].
[^101]: Bray, Tim (2014) *The JavaScript Object Notation (JSON) Data Interchange Format*. RFC 7159. Available at: [https://www.rfc-editor.org/info/rfc7159/](https://www.rfc-editor.org/info/rfc7159/) [Accessed: 24 May 2026] -- Intermediate proposed standard RFC, now obsolete, current standard is [].
[^102]: Bray, Tim (2017) *The JavaScript Object Notation (JSON) Data Interchange Format*. RFC 8259, STD 90. Available at: [https://www.rfc-editor.org/info/rfc8259/](https://www.rfc-editor.org/info/rfc8259/)[Accessed: 24 May 2026] -- The current JSON standard.
[^103]: Crockford, Douglas (2006) *The application/json Media Type for JavaScript Object Notation (JSON)*. RFC 4627. Available at: [https://www.rfc-editor.org/info/rfc4627/](https://www.rfc-editor.org/info/rfc4627/) [Accessed: 24 May 2026] -- The first definition, an informational RFC, now obsolete, current standard is [].
[^104]: ECMA International (2013) *ECMA-404: The JSON data interchange syntax*. 1st ed. Available at: [https://ecma-international.org/wp-content/uploads/ECMA-404_1st_edition_october_2013.pdf](https://ecma-international.org/wp-content/uploads/ECMA-404_1st_edition_october_2013.pdf) [Accessed: 24 May 2026] -- Now obsolete, the current standard is []. 
[^105]: ECMA International (2017) *ECMA-404: The JSON data interchange syntax*. 2nd ed. Available at: [https://ecma-international.org/wp-content/uploads/ECMA-404_2nd_edition_december_2017.pdf](https://ecma-international.org/wp-content/uploads/ECMA-404_2nd_edition_december_2017.pdf) [Accessed: 24 May 2026] -- Describes only the allowed syntax, the more complete current standard is [].
[^106]: International Organization for Standardization (2017) *ISO/IEC 21778:2017Information technology: the JSON data interchange syntax*. Available at: [https://www.iso.org/standard/71616.html](https://www.iso.org/standard/71616.html) [Accessed: 24 May 2026] -- Describes only the allowed syntax, the more complete current standard is [].

**JSON Schema:**

[^107]: *JSON Schema* (no date) Available at: [https://json-schema.org/](https://json-schema.org/) [Accessed: 24 May 2026]

The most recent version of JSON Schema is 2020-12. JSON Schema is not a recognised standard (yet?), hence the version thing.

[^108]: *Draft 2020-12* (no date) Available at: [https://json-schema.org/draft/2020-12](https://json-schema.org/draft/2020-12) [Accessed: 24 May 2026] -- The landing page of the version on the JSON Schema website
[^109]: Wright, Austin, Andrews, Henry and Hutton, Ben (2022): *JSON Schema Validation: A Vocabulary for Structural Validation of JSON*. Available at: [https://json-schema.org/draft/2020-12/draft-bhutton-json-schema-validation-01](https://json-schema.org/draft/2020-12/draft-bhutton-json-schema-validation-01) [Accessed: 24 May 2026] -- The actual specification document

To get started with JSON Schema I found these pages useful:

[^110]: Creating your first schema (no date) Available at: [https://json-schema.org/learn/getting-started-step-by-step](https://json-schema.org/learn/getting-started-step-by-step) [Accessed: 24 May 2026]
[^111]: *JSON Schema reference* (no date) Available at: [https://json-schema.org/understanding-json-schema/reference](https://json-schema.org/understanding-json-schema/reference) [Accessed: 24 May 2026]
[^112]: 'JSON schemas and settings' (2026) in *Editing JSON with Visual Studio Code*. Available at: [https://code.visualstudio.com/docs/languages/json#_json-schemas-and-settings](https://code.visualstudio.com/docs/languages/json#_json-schemas-and-settings) [Accessed: 24 May 2026] -- Has information in how to configure VS Code to validate a JSON file against a specific JSON Schema file.

Other sources I used when putting the workshop together:

[^113]: Amazon Web Services (no date) *What’s the Difference Between JSON and XML?* Available at: [https://aws.amazon.com/compare/the-difference-between-json-xml/](https://aws.amazon.com/compare/the-difference-between-json-xml/) [Accessed: 24 May 2026]
[^114]: Liquid Technologies (2025) *Understanding JSON: A Beginner’s Guide – Part 1 of 4*. Available at: [https://blog.liquid-technologies.com/understanding-json-a-beginners-guide-part-1-of-4](https://blog.liquid-technologies.com/understanding-json-a-beginners-guide-part-1-of-4) [Accessed: 24 May 2026]

### Other sources

[^115]: https://en.wikipedia.org/wiki/List_of_longest-living_cats

## Tools and software

Looking up **code points** for characters and how to represent them in various languages:

[^a]: *Codepoints* (no date) Available at: [https://codepoints.net/](https://codepoints.net/) [Accessed: 24 May 2026]

**Converting** between all sorts of number formats:

[^b]: *Number Converter* (no date) Available at: [https://www.rapidtables.com/convert/number/index.html](https://www.rapidtables.com/convert/number/index.html) [Accessed: 24 May 2026]

**Text editors** and extensions:

[^c]: Ho, Don (2026) *Notepad++* [Computer programme]. Available at: [https://notepad-plus-plus.org/](https://notepad-plus-plus.org/) [Accessed: 24 May 2026] -- Easy entry-level text editor that is useful for working with data files.
[^d]: Microsoft (2026) *Visual Studio Code* [Computer programme]. Available at: [https://code.visualstudio.com/](https://code.visualstudio.com/) [Accessed: 24 May 2026] -- Much more than a text editor, more of a development environment. Steeper learning curve than Notepad++.
[^e]: Microsoft (2026) *Visual Studio Code* (browser version) [Computer programme]. Available at: [https://vscode.dev/](https://vscode.dev/) [Accessed: 24 May 2026] -- The in-browser version of Visual Studio Code.
[^f]: Sublime HQ (2026) *Sublime Text* [Computer programme]. Available at: [https://www.sublimetext.com/](https://www.sublimetext.com/) [Accessed: 24 May 2026] -- Powerful text editor. The learning curve might be a bit steeper than Notepad++.

**Notepad++ extensions:**

[^g]: BdR76 (2025) *CSVLint* [Computer programme]. Available at: [https://github.com/BdR76/CSVLint/](https://github.com/BdR76/CSVLint/) [Accessed: 24 May 2026] -- Makes working with CVS, other delimited files in Notepad++ easier. Recommend installing this via Notepad++ rather than downloading from GitHub, it's easier: Plugins → Plugins Admin → Search for "CSVLint" → Tick the check box next to the name → Click "Install" (top right).
[^h]: molsonkiko (2026) *JsonToolsNppPlugin* [Computer programme]. Available at: [https://github.com/molsonkiko/JsonToolsNppPlugin](https://github.com/molsonkiko/JsonToolsNppPlugin) [Accessed: 24 May 2026] -- An extension/plugin to make working with JSON in Notepad++ easier. Recommend installing this via Notepad++ rather than downloading from GitHub, it's easier: Plugins → Plugins Admin → Search for "JSON Tools" → Tick the check box next to the name → Click "Install" (top right).
[^i]: morbac (2022) xmltools [Computer programme]. Available at: [https://github.com/morbac/xmltools](https://github.com/morbac/xmltools) [Accessed: 24 May 2026] -- An extension/plugin to make working with XML in Notepad++ easier. Recommend installing this via Notepad++ rather than downloading from GitHub, it's easier: Plugins → Plugins Admin → Search for "XML Tools" → Tick the check box next to the name → Click "Install" (top right).

**VS Code extensions:**

[^j]: mechatroner (2026) Rainbow CSV [Computer programme]. Available at: [https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv](https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv) [Accessed: 24 May 2026] -- A Visual Studio Code extension that makes working with delimited files a lot easier.
