<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-89133312-1', 'auto');
  ga('send', 'pageview');

</script>

## V1 - A Visual Query Language for Schema-based Property Graphs

Copyright (c) 2017 [Lior Kogan](https://www.linkedin.com/in/liorkogan) (koganlior1 [at] gmail [dot] com)

The content of this site is licensed under the [CC BY-NC-SA 4.0 licence](https://creativecommons.org/licenses/by-nc-sa/4.0/). For other uses please contact the author.

**This is an early draft. It’s known to be incomplet and incorrekt.**

## The Property Graph Data Model

A [*graph*](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)) is an ordered pair _G = (V, E)_ comprising a set _V_ of vertices (nodes) together with a set _E_ of edges (arcs), which are 2-element subsets of _V_.

A **property graph** (AKA attributed graph) is a graph where

- Graph vertices represent entities.
  An entity is an objects or ‘thing’ in our mini-world, with an independent existence and which is distinguishable from other objects (e.g. a person, a horse, a dragon)
- Graph edges represent relationships between pairs of entities - directional (e.g. 'owns', 'offspring of') or bidirectional (e.g. 'sibling of')
- Each vertex has a set of descriptive features called properties (AKA attributes) (e.g. 'First Name', 'Last Name' for a person)
- Each edge has a set of properties as well
- Each property has a name (string), a data type (e.g. string / integer), and a value (e.g. "weight": int = 450). Composite properties are composed of a set of sub-properties - each has a name, a data type and a value (e.g. Name = {"First": string = "Brandon", "Last": string = "Stark"}
- Usually each vertex has a type (e.g. Person, Horse, Dragon), but in a schema-free graph - the type is just another property
- Edges have types as well (e.g. 'owns', 'member of'). Again - without schema - the type is just another property

More about property graphs can be found [here](http://tinkerpop.apache.org/docs/current/reference/) and [here](https://neo4j.com/developer/graph-database/).

A **property graph's schema** is defined by

* A set of entity types.
  Entities with the same basic properties are grouped (typed) into an entity type (e.g. Person, Horse, Dragon).
  For each entity type: 
  * A name 
  * A set of properties. For each property: name (key) and data type
* A set of relationship types. For each relationship type:
  * A name
  * A set of properties. For each property: name (key) and data type
  * A set of pairs of entity types for which the relationship type holds (e.g. owns(Person, Horse); owns(Person, Dragon) )

A **schema-based property graph** is a property graph which conforms to a given schema.

There is no standard way to define property graph schemas. Implementations may vary in many aspects: the properties' data types (basic types, categorical, multivalued, composite, nested) and supported operators, the relationship types supported directionality (unidirectional, bidirectional, mixed), constraints (mandatory properties, relationships cardinality, etc.), support of empty (missing) values, entity type hierarchies, relationship type hierarchies, etc.

**Why do we need a schema?**

Schema-free property graphs neither define nor enforce entity-types or relationships-types; each vertex and each edge may contain properties with any name and any data type. Without schema we can’t enforce integrity. Without such integrity we can't define formal building-blocks for representing patterns.

In order to ask and answer queries such as *“Any person who owns a white horse”* we first need to:

* Define 'Person' and 'Horse' entity types
* Ensure that each Person and Horse entities are of the correct entity types
* Define 'owns' relationship type
* Defines that the 'owns' relationship type holds between Person entity type and Horse entity type
* Define a 'Color' property for the Horse entity type
* Define that Color holds a string, or better, define a categorical data type

## Patterns and Pattern Languages

**A pattern** defines **acceptable** connected property graphs (entities and relationships), defined over a given property graph's schema.

Here are two examples:

* *Any person who owns at least 5 white horses*

* *Any person whose birth date is between 1/1/970 and 1/1/980, owns a white Horse, owns a dragon that his name starts with 'M', and his  dragon froze at least 3 dragons belonging to members of the Masons guild in the last month*

**Pattern matching** is the process of deciding whether a given (sub)graph is acceptable to a given pattern. 

**Pattern finding** is the process of finding minimal subgraphs of a given property graph, which match a given pattern. Any minimal subgraph that matches the pattern is called **an assignment**. _Minimal_ means that if any entity or relationship is removed - it won't match the pattern anymore.

A pattern can be viewed as a query that can be executed against a property graph. Similarly, pattern finding can be viewed as query answering.

**A pattern language** defines:

* Syntax and semantics for expressing patterns (queries)
* Syntax and semantics for expressing query results (answers)

The language semantics defines which elements (entities and relationships) are part of an assignment. 

In the 1st example above (which is expressed in English, not in a pattern language) the set of assignments (expressed in English as well) would be:

* For each person than owns at least 5 white horses: an assignment is composed of:
  * The 'Person' entity 
  * The 'Horse' entities - of 5 of his horses with property "color" that has the value "white"
  * The 'own' relationships between the Person entity the those Horse entities

The language semantics also define if an answer to a query is either (i) the set of all assignments, or (ii) the union of all assignments. (ii) is often prefered since it avoids exponential explosion in many queries.

Pattern languages differs in the following aspects:

* **Genericity** - generic (e.g. schema-driven) vs. domain-specific
* **Representation** - textual (structured / controlled natural) or visual (AKA graphical, diagrammatic)
* **Expressivity** - The richness of the patterns that can be expressed. The expressivity is always limited (in a Gödelian sense)
* **Simplicity** - How simple it is to construct new patterns and to understand existing patterns
* **Efficiency** - to parse patterns and to execute pattern queries

## Dragons of Ice and Fire

The subjects of Sarnor, Omber and all the other kingdoms of the four continents: Essos, Westeros, Sothoryos and Ulthos like their horses. There is one thing they love even more - that is their dragons. They own dragons of fire and ice. Like all well-behaved dragons, their dragons love to play. Dragons always play in couples. When playing, dragons often get furious, fire at each other (fire breath) and freeze each other (cold breath). Dragons usually freeze each other for periods of several minutes, but on occasion, if they are really raging, they can freeze each other for periods of to 2 hours or even more. The subjects of the 4 continents enjoy watching their dragons play. Fascinated by these magnificent creatures, they wrote thousands of books, which document any play, any fire breathing, and any ice breathing in the last 900 years (that's not so much in dragon-years). The kings of Sarnor and Omber regularly pose queries about their history. Quite often, it takes the royal historians and royal analysts several days to come up with answers, during which the kings are quite nervous. Lately, the king of Sarnor posed a very complex query, and after waiting for results for more than two moons, he ordered the chief analyst to be executed. He then summoned his chief mechanics, and ordered them to develop an apparatus which he would use to pose queries and quickly get results. 

The engineers started their work by:

* Collecting all queries that their master posed in the last few years
* Constructing a schema (entity types, relationship types - and their properties) using which these queries can be expressed

The schema is composed of the following entity types (and their properties):

* **Person** - name {first : string, last : string}, gender : enum, birth date : date, death date : date, height : int [cm]
* **Dragon** - name : string
* **Horse** - name : string, color : enum, weight : int [Kg]
* **Guild** - name : string
* **Kingdom** - name : string

and of the following relationship types (and their properties):

* **owns**(Person, Horse/Dragon) - tf {since : date, till : date} ("tf" stands for timeframe)
* **fires at**(Dragon, Dragon) - time : datetime
* **freezes**(Dragon, Dragon) - time : datetime, duration : int [min]
* **offspring**(Person, Person)
* **knows**(Person, Person) -: since : date
* **member of**(Person, Guild) - tf {since : date, till : date} ("tf" stands for timeframe)
* **subject of**(Person, Kingdom)
* **registered in**(Guild, Kingdom)
* **originated in**(Horse/Dragon, Kingdom)

Note that "Person"'s name, "owns"'s _tf_, as well as "member of"'s _tf_ - are composite properties.

This schema and the queries will serve us to demonstrate the power of the V1 language.

## The V1 Pattern Language

Like the king of Sarnor, many potential users won't use textual pattern (query) languages. The learning curve may be too sharp for someone with little prior experience in programming. Even users that do use textual query languages often spend a lot of time on the technicalities. 

Kings, as well as other query-posers would like to pose complex queries in a manner that is coherent with the way they think. They want to do it with minimal technical training and minimal effort. The ability to express patterns in a way that is aligned with their mental processes is crucial to the flow of their work, and to the value of the insights they can deduce.

Visual query languages are very attractive. They have the potential to be more 'user friendly' in the sense that patterns may be constructed and understand much more quickly, with much less mental effort. A long-standing challenge is to design a visual query language that is generic, simple, and has rich expressivity. This is what V1 aims to be. 

V1 is a rich simple generic visual pattern language for schema-based property graphs. It is named after the [primary visual cortex in our brain](https://en.wikipedia.org/wiki/Visual_cortex), which is also known as Visual area one (V1).

## V1 Basics

The following sections define the syntax and semantics of the V1 language. We start with the basics, adding more language elements as we go along.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB01.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB04.png)

**Syntax**

Patterns are written from left to right. Each pattern starts with (a small black diamond), continues with a (yellow, blue or red rectangle), and continues with zero or more ( (black arrow, black line, or red line) followed by a (yellow, blue, or red rectangle) ).

**Semantics**

Yellow, blue and red rectangles represent entities. **A yellow rectangle** represents a **concrete entity**: a specific person, a specific horse, etc. The text inside a yellow rectangle denotes the entity type, and the value of the entity's leading properties (e.g. first name and last name of a person). **A blue rectangle** represents a **typed entity**: an entity of a given type. A blue 'Person' for example means that in any assignment - only concrete 'Person' entities can match the pattern. **A red rectangle** represents an **untyped entity**: an entity of any type (unless type constraints are defined - see later).

A pair of entities can be connected with:

* A horizontal **black arrow** - representing a **directional relationship**, or
* A horizontal **black line** - representing either a **bidirectional relationship** or a directional relationship where the direction doesn't matter, or
* A horizontal **red line** - representing a **path** (explained later)

Each relationship has a label above the arrow/line that denotes the relationship's type.

The relationship type between two entities must be valid according to the schema. As said before - for each relationship type, the schema defines a set of pairs of entity types for which the relationship type holds (e.g. owns(Person, Horse); owns(Person, Dragon) ).

For every blue rectangle, red rectangle, black arrow, and black line - the query processor will look in the property graph for assignments. Concrete entities are assigned to blue and red rectangles. Concrete relationships are assigned to black arrows and lines. An assignment to the pattern is a set of concrete entities and relationships that match the whole pattern. A answer to a V1 query is the union of all assignments.

Here are some basic patterns:

_**Q1:** Any dragon owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q001.png)

_**Q2:** Any dragon that was frozen at least once by a dragon owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q002.png)

_**Q184:** Any dragon that froze or was frozen at least once by a dragon owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q184.png)

The freeze direction does not matter. Therefore - a line (instead of an arrow) is used in the pattern.

## Properties

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB02.png)

**A green rectangle** is connected to an entity (concrete/aggregate/typed/logical/untyped) - on its right, or to a relationship - on its bottom, and represents an entity's / relationship's property. It contains:

- The property's name (_'prop'_)
- An optional function to apply to the property's value (_'f'_)
- An optional **constraint** on the property's value / on _f_(property's value)

  **a constraint filters assignments to the entity/relationship - to only those assignments for which the entity/relationship that satisfy the pattern - satisfy the constraint.**
  
- A **property tag**, ('_{pt}_') - explained later

Constraints cannot be defined for concrete entities. 

For untyped entities, green rectangles can represent only properties that are common to all valid entity types. Valid entity types for an untyped entity can be defined explicitly (using entity type constraints - see later) and implicitly (according to the relationship types that are connected to the untyped entity).

_**Q3:** Any person who owns a dragon, and his first name is Brandon **(first version)**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-1.png)

_**Q190:** Any person who owns a dragon since 1/1/1011 or since a later date (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q190-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q190-2.png)

## Properties - Functions and Constraints

Constraints over ordinal properties / function-range (integer types, floating-point types, _date_, _datetime_):

* _= expr / ≠ expr / > expr / ≥ expr_ / _< expr_ / _≤ expr_ 
* _[not] in (expr .. expr) / in (expr .. expr] / in [expr .. expr) / in [expr .. expr]_
* _[not] in {expr, expr, ...}_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB09-01.png)

Functions over _date_ properties:

* _Year(date), Month(date), Day(date)_ → int
* _DayOfWeek(date), DayOfYear(date), WeekOfYear(date)_ → int

Functions over _time_ properties (00:00:00 - 23:59:59):

* _Hour(time), Min(time), Sec(time)_ → int

Functions over _datetime_ properties:

* _Date(datetime)_ → date
* _Time(datetime)_ → time
* _Year(datetime), Month(datetime), Day(datetime)_ → int
* _Hour(datetime), Min(datetime), Sec(datetime)_ → int
* _DayOfWeek(datetime), DayOfYear(datetime), WeekOfYear(datetime)_ → int

Constraints over _string_ properties / function-range:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB09-02.png)

Functions over string properties:

* _Length(string)_ → int

Implementations may support additional data types, functions and comarison operators.

## Empty (Missing) Values

The data model may support empty (missing) values for one or more properties. empty values may mean different things: the value of the property may be unknown, the property may have no value (e.g. a person with no middle name), a date that has not yet arrived (e.g. empty death date), etc. Regardless of the data semantics, the V1 language has several constructs that are useful in many cases:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB10.png)

* A condition with a blue comparison operator: If the value is missing - the condition is evaluated as false; otherwise - it is evaluated according to the value and the expression
* A condition with a red comparison operator: If the value is missing - the condition is evaluated as true; otherwise - it is evaluated according to the value and the expression
* An 'empty' condition - if the value is missing - the condition is evaluated as false; otherwise - it is evaluated as true
* A 'not empty' condition - if the value is missing - the condition is evaluated as true; otherwise - it is evaluated as false

## Quantifiers #1

Vertical quantifiers (or simply 'quantifiers') are used when more than one condition needs to be satisfied. Here is a simple example:

_**Q3:** Any person who owns a dragon, and his first name is Brandon **(second version)**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-2.png)

A quantifier has one connection on its left side, and two or more branches on its right side. We'll name the left side of the quantifier 'the left component', and anything that follows a branch, up to the end of the branch, 'a right component'.

**The first way to use quantifiers:** The left component ends with an entity (concrete/aggregate/typed/logical/untyped), and each right component starts with:

* A relationship (optionally preceded by an 'X', an '↛', an 'O' or an 'L'),
* A path (optionally preceded by an 'X', an '↛', an 'O' or an 'L'),
* A green rectangle (entity's property value constraint / tag), or
* A quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB03.png)

4 quantifiers are defined now, and 8 other quantifiers are defined later on - after No-existence is explained.

* _**All**_ (denoted '&') - An assignment matches the pattern _only if_ it matches the left component and all the right components. 
* _**Some**_ (denoted '&#124;') - An assignment matches the pattern _only if_ it matches the left component and at least one right component.
* _**> n**_ - An assignment matches the pattern _only if_ it matches the left component and more than _n_ right components. _n_ ∈ [0, _b_-1]
* _**≥ n**_ - An assignment matches the pattern _only if_ it matches the left component and _n_ or more right components. _n_ ∈ [1, _b_]

As said - an assignment must be a _minimal_ subgraph: if any entity or relationship is removed - it won't match the pattern anymore.

"_Only if_" denotes a necessary but not sufficient condition, since assignments must satisfy other requirements expressed by the pattern.

Quantifiers can be nested. Here is an example:

_**Q8:** Any person born prior to 970 and is still alive, or that his father born no later than 1/1/950_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q008.png)

## Quantifiers #2

**A second way to use quantifiers:** The left component ends with a relationship or with a path, and each right component starts with either:

* An entity (concrete/aggregate/typed/logical/untyped)
* A quantifier

The following examples demonstrate both ways to use quantifiers:

_**Q11:** Any current member of the Masons guild, who since 1011 or later knows someone who left the Saddlers guild or the Blacksmiths guild - on June 1010 or later_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q011.png)

Sub-properties of a composite property are denoted as "property name"."sub-property name" (e.g. "tf.till").

Note that the condition 'member of.till _empty_' is based on the assumption that an empty value means that the person is still a member. Alternatively, it could have meant that the _till_ date is unknown. This depends on the semantics of the _till_ property in the given schema. 

_**Q10:** Any person whose first name is Brandon, who owns some dragon B which froze a dragon C that (i) belongs to an offspring of Rogar Bolton, and (ii) froze a dragon that belongs either to Robin Arryn or to Arrec Durrandon. B froze C at least once in 1010 or after - for longer than 100 seconds_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q010.png)

When a relationship need to satisfy several conditions (as in Q10: the freeze time is in 1010 or after _and_ the freeze duration is longer than 100 seconds) - green rectangles can be chained. Chaining is not valid for entities. Chaining is explained below (see _Horizontal Quantifiers_)

## Entity Tags

There is a letter in the top-left corner of any entity rectangle (concrete/aggregate/typed/logical/untyped). This letter is called an **'entity tag'**.

Entity tags serve two purposes. First, when a pattern is used as a query, entity tags appear in the query's answer as well: any concrete entity in the answer is tagged with the same tag as the query's entity it was assigned to. This helps the user understand why any given entity is part of the answer. Second, entity tags are used to express _identicality constraints_ and _nonidenticality constraints_.

**Identicality constraint** is used when the same (identical) concrete entity must be assigned to several typed entities of the same type / several untyped entities. Here is an example:

_**Q4:** Any person whose dragon was frozen by a dragon owned by (at least) one of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q004.png)

Entity tag 'B' is used to enforce identical assignment to two typed entities. The 'B' tags are bold and green - this is a visual indication that these tags are used to enforce identicality.

Here is another example:

_**Q9:** Any dragon pair (A, B) where A froze B both in 980 and in 984_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q009.png)

**Nonidenticality Constraint** is used when different (nonidentical) concrete entities must be assigned to several typed entities of the same type / several untyped entities. Here is an example:

_**Q5:** Any person whose dragon was frozen by a dragon owned by two of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q005.png)

Without enforcing nonidenticality, the same parent can be assigned to both C and E. The 'C' and '≠C' tags are bold and red - this is a visual indication that these tags are used to enforce nonidenticality.

Here are two more examples:

_**Q6:** Any person whose dragon was frozen by two dragons – one owned by one of his parents, the other owned by another parent (none, one, or both dragons may be owned by both parents)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q006.png)

_**Q7:** Any person whose dragon was either (i) frozen by a dragon owned by two of his parents, or (ii) frozen by two dragons – one owned by one of his parents and the other owned by his other parent_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q007.png)

_**Q24:** Any person who has (at least) two parents and owns a dragon that was frozen by a dragon that is not owned by either of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q024.png)

Several nonidenticality constraints may be defined for the same rectangle (see Q170 for example)

For any quantifier except ‘&’ - identicality/nonidenticality constraints defined between rectangles in different branches, or between rectangles in a branch and rectangles left of the quantifier - may be meaningless.

## No-existence and No-connection

Sometimes we need to express a pattern using negative terms. For example: _any person whose first name is Brandon, and **doesn't** own a white horse_. Such patterns are composed of:

* The left 'positive' component - ends with an entity (_any person whose first name is Brandon_) 
* A no-existence / a no-connection language element (_"doesn't"_) 
* The right 'negative' component - starts with a relationship/path (_own a white horse_)

The example above covers two cases: (i) there may be no white horses at all, or (ii) there may be white horses, but none of them is owned by a person whose first name is Brandon. 

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB05.png)

When it doesn't matter which is the case - _any person whose name is Brandon and doesn't own a white horse_ is a valid assignment to the pattern - the **no-existence language element (depicted with a pink 'X' box)** can be used.

An assignment matches the pattern _only if_:

* It matches a pattern composed only of the left component (there is an assignment to _a person whose first name is Brandon_) 
* It has no superset that matches a pattern composed of the left component chained to the right component (There is no assignment to _a person whose first name is Brandon and own's a white horse)_

An 'X' may not be used directly before a relationship or a path with an aggregation (aggregations are explained later on)

In certain cases, we need an assignment to match the pattern _only if_:

* It matches a pattern composed only of the left component (there is an assignment to _a person whose first name is Brandon_) 
* It has no superset that matches a pattern composed of the left component chained to the right component (There is no assignment to _a person whose first name is Brandon and own's a white horse)_
* There is an assignment that matches a pattern composed only of the right component (there is _a white horse_)

In such cases, the **no-connection language element (depicted with a pink '↛' box)** can be used. 

'↛''s are usually used directly before a relationship or a path with an aggregation.

Examples:

_**Q12:** Any person who doesn't own a horse_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q012.png)

_**Q13:** Any horse that is not owned by a person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q013.png)

_**Q14:** Sweetfoot - if it is not owned by a person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q014.png)

_**Q15:** Brandon Stark - if he doesn't own a horse_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q015.png)

_**Q16:** Any person who doesn't own Sweetfoot_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q016.png)

_**Q17:** Any horse that is not owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q017.png)

_**Q18:** Brandon Stark - if he doesn't own Sweetfoot_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q018.png)

_**Q19:** Sweetfoot - if it is not owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q019.png)

_**Q22:** Any horse not owned by a person who owns a dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q022.png)

Note that the left component is _'horse'_ while the right component is _'owned by a person who owns a dragon'_. The right component is anything that follows the 'X' - up to the end of the branch.

That includes:

- Any horse that is not owned
- Any horse that none of its owners is a person (e.g. a horse owned by a guild)
- Any horse that each person who owns it - doesn't own a dragon

_**Q23:** Any horse not owned by a person who doesn't own a dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q023.png)

That includes:

- Any horse that is not owned
- Any horse that none of its owners is a person (e.g. a horse owned by a guild)
- Any horse that each person who owns it - also owns a dragon

_**Q256:** Any person who doesn't own a white horse_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q256.png)

_**Q257:** Any person who didn't become a horse owner on or after 1011_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q257.png)

_**Q258:** Any person who didn't become a white horse owner on or after 1011_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q258.png)

An 'X' / an '↛' may also appear before a relationship that directly follows a quantifier's branch:

_**Q20:** Any horse that is neither owned by Rogar Bolton nor by Robin Arryn (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-1.png)

This pattern can also be represented using the '0' quantifier (explained below)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-2.png)

_**Q21:** Any horse that is not owned by both Rogar Bolton and Robin Arryn (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-1.png)

This pattern can also be represented using the 'not all' quantifier (explained below), but there is a slight difference: in the version above if either Rogar Bolton or Robin Arryn owns the horse - the owner won't be a part of the answer, while in the version below - the owner will be a part of the answer.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-2.png)

_**Q25:** Any dragon (C) that wasn't fired at by Balerion, but was fired at by a dragon that Balerion fired at (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-2.png)

_**Q26:** Any guild that people who are members of the same guild as Brandon Stark are member of, but Brandom is not a member of that guild (four versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-3.png)

## Quantifiers #3

**A third way to use quantifiers:** A quantifier may start a pattern. On the quantifier's left side - the pattern's start, while each right component may start with either:

* An entity (concrete/aggregate/typed/logical/untyped)
* A quantifier

The _None_ quantifier cannot start a pattern.

Here is a fourth way to represent Q26:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-4.png)

## Quantifiers #4

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB14.png)

In addition to the 4 quantifiers described above (_All_, _Some_, _> n_, _≥ n_), V1 supports these 8 quantifiers:

* **Not all** (denoted by an '&' with stroke) - An assignment matches the pattern _only if_ it matches a similar pattern, where the quantifier is replaced with _All_, and an 'X' is added to (or removed from) the start of one or more branches.
* **None** (denoted '0') - An assignment matches the pattern _only if_ it matches a similar pattern, where the quantifier is replaced with _All_, and an 'X' is added to (or removed from)  the start of all branches.
* **_n_** - An assignment matches the pattern _only if_ it matches a similar pattern, where the quantifier is replaced with _All_, and an 'X' is added to (or removed from) the start of _b-n_ branches. _n_ ∈ [1, _b_]
* **_< n_** - An assignment matches the pattern _only if_ it matches a similar pattern, where the quantifier is replaced with _All_, and an 'X' is added to (or removed from) the start of more than _b-n_ branches. _n_ ∈ [2, _b_]
* **_≤ n_** - An assignment matches the pattern _only if_ it matches a similar pattern, where the quantifier is replaced with _All_, and an 'X' is added to (or removed from) the start of _b-n_ or more branches. _n_ ∈ [1, _b_]
* **_≠ n_** - An assignment matches the pattern _only if_ it matches a similar pattern, where the quantifier is replaced with _All_, and an 'X' is added to (or removed from) the start of any number except _b-n or _n_ branches. _n_ ∈ [1, _b_]
* **_n1..n2_** - An assignment matches the pattern _only if_ it matches a similar pattern, where the quantifier is replaced with _All_, and an 'X' is added to (or removed from) the start of less than _b-n2_ or more than _b-n1_ branches. _n1_ ∈ [1, _b_], _n2_ ∈ [2, _b_], _n1_ < _n2_
* **_∉ n1..n2_** - An assignment matches the pattern _only if_ it matches a similar pattern, where the quantifier is replaced with _All_, and an 'X' is added to (or removed from) the beginning more than _b-n2_ but less than _b-n1_ branches. _n1_ ∈ [2, _b-1_], _n2_ ∈ [3, _b_], _n1_ < _n2_

(_b_ denotes the number of branches)

## Horizontal Quantifiers

Horizontal quantifiers are used with relationships / paths. On top of a horizontal quantifier is a relationship / path, and on its bottom - two or more branches.

Each branch starts with:

* A green rectangle (relationship only - relationship's property value constraints / tag), or
* An orange rectangle (aggregate condition / aggregation tag - explained later), or
* A horizontal quantifier

The semantics of horizontal quantifiers is quite different from the semantics of vertical quantifiers:

We'll name the relationship / path to which the horizontal quantifier is attached _r_; We'll name the left side of _r_ 'the left component', and anything on the right of _r_ 'the right component'.

We'll define pattern _P0_ which is identical to the original pattern, but without the horizontal quantifier and anything that is below it. We'll also define _b_ sub-patterns, _P1..Pb_ (where _b_ denotes the number of branches), each composed of the left component, _r_, the right component, and one of the branches.

Given assignment _A_ we'll define _S(A)_ as a set of sub-assignments of A - all with the same concrete elements in its left component and its right components - but with different concrete elements in _r_.

The 12 quantifiers are defined as follows:

An assignment _A_ matches the pattern _only if_:

* **All** (denoted '&') - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and each of _P1..Pb_ is matched - each by at least one element of _S(A)_
* **Some** (denoted '&#124;') - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and at least one of _P1..Pb_ is matched - by at least one element of _S(A)_
* _**> n**_ - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and more than _n_ of _P1..Pb_ are matched - each by at least one element of _S(A)_
* _**≥ n**_ - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and _n_ or more of _P1..Pb_ are matched - each by at least one element of _S(A)_

* **Not all** (denoted by an '&' with stroke) - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and, any number but b of _P1..Pb_ are matched - each by at least one element of _S(A)_
* **None** (denoted '0') - _A_ matches _P0_  and there is no _S(A)_ such that at least one of _P1..Pb_ is matched - by at least one element of _S(A)_
* **_n_** - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and exactly _n_ of _P1..Pb are matched - each by at least one element of _S(A)_
* **_< n_** - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and less than _n_ of _P1..Pb_ are matched - each by at least one element of _S(A)_. _n_ ∈ [2, _b_]
* **_≤ n_** - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and _n_ or less of _P1..Pb_ are matched - each by at least one element of _S(A)_. _n_ ∈ [1, _b_]
* **_≠ n_** - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and any number except _b-n or _n_ of _P1..Pb_ are matched - each by at least one element of _S(A)_. _n_ ∈ [1, _b_]
* **_n1..n2_** - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and _n1_ or more, but not more than _n2_ of _P1..Pb_ are matched - each by at least one element of _S(A)_. _n1_ ∈ [1, _b_], _n2_ ∈ [2, _b_], _n1_ < _n2_
* **_∉ n1..n2_** - There is _S(A)_ such that each element of _S(A) matches at least one of _P1..Pb_, and Less than _n1_ or more than _n2_ of _P1..Pb_ are matched - each by at least one element of _S(A)_. _n1_ ∈ [2, _b-1_], _n2_ ∈ [3, _b_], _n1_ < _n2_

"_Only if_" denotes a necessary but not sufficient condition, since assignments must satisfy other conditions expressed by the pattern.

Here are two examples:

_**Q187:** Any dragon that was frozen by Balerion: (at least once in 1/1/1010 or later) and (at least once - for longer than 10 minutes) - same or different freezes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q187.png)

_**Q189:** Any dragon that was frozen by Balerion: (at least once in 1/1/1010 or later) or (at least once for longer than 10 minutes) **Alternative wording:** Any dragon that was frozen by Balerion at least once: (in 1/1/1010 or later, or for longer than 10 minutes)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q189.png)

Green rectangles and orange rectangles below a horizontal quantifier can also be **chained**. When chained, each element serves as a filtering step. The branch's condition is met only if there is an assignment that passes all the filtering steps.

Here is an example:

_**Q188:** Any dragon that was frozen by Balerion at least once - in 1/1/1010 or later - for longer than 10 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q188.png)

## E-combiner

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB15.png)

An E-combiner combines two or more branches (not necessarily of the same quantifier). On its left side are entities and on its right side either:

* A relationship (optionally preceded by an 'X', an '↛', an 'O' or an 'L')
* A path (optionally preceded by an 'X', an '↛', an 'O' or an 'L')
* A green rectangle (entity's property value constraints / tag)
* A quantifier

The right side of an E-combiner is a direct continuation of each of the combined branches. An E-combiner is simply a syntactic sugar used to save duplication when several branches terminates identically.

The usage of an entity tag both before and after an E-combiner (to express identicality / nonidenticality constraints) subjects to tag rules (again - anything after the E-combiner is duplicated to each branch).

The relationship / property types on an E-combiner's right side must match all the entity types on its left side. 

Here are some examples:

_**Q27:** Any Sarnorian subject who owns a horse and a dragon of the same origin; Any Obmerian guild that owns a horse and a dragon of the same origin_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q027.png)

_**Q28:** Any Sarnorian subject who owns a white horse; Any person who doesn't know someone who owns a white horse_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q028.png)

_**Q35:** Any person who (i) knows a Sarnorian subject who knows a person who owns a white horse, or (ii) know a person who doesn't know a person who owns a white horse, or (iii) know someone who doesn't own a dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q035.png)

## R-Combiner

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB15.png)

An R-combiner combines two or more branches of the same quantifier. On its left side are relationships, and on its right side - an entity.

The entity type on an R-combiner's right side must match all the relationship types on its left side.

Here are some examples:

_**Q29:** Any dragon that froze or fired at some dragon (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q029-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q029-2.png)

_**Q30:** Any dragon pair (A, B) where A both froze B and fired at B (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q030-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q030-2.png)

Note that the concrete entity on the right side of the R-combiner has the same assignment for all the branches.

_**Q31:** Any dragon pair (A, B) where A froze B, A fired at B, B froze A, and B fired at A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q031.png)

_**Q32:** Any dragon pair (A, B) where A fired at B, and A fired at some dragon that fired at B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q032.png)

_**Q170:** Any dragons triplet (A, B, D) where A fired at B, A fired at some dragon that fired at B, B froze D, and B froze some dragon that froze D_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q170.png)

_**Q33:** Any dragon A that froze some dragon B, froze some dragon that froze B, and fired at some dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q033.png)

_**Q34:** Any dragon A that froze some dragon B, froze some dragon that froze B, fired at some dragon D and fired at some dragon that fired at D (B and D may be the same dragon or different dragons)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q034.png)

## Latent and Optional Components

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB07.png)

Anything on the right of an **'L'** is **latent**: It won't be part of the answer, though it still must have an assignment.

- An 'L' may appear just before a relationship, a path, or a quantifier (excluding a quantifier at the start of the pattern)
- An 'L' may not appear right on an 'L', an 'X' or an '↛'
- An 'L' may not appear right of a '0' quantifier
- An 'L' may not appear right of a "relationships = 0", "paths = 0" or "→ = 0" aggregate condition 

Here are two examples:

_**Q142:** Any person who owns a white horse, and has a parent who owns a horse. The parent and his horse are not part of the answer_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q142.png)

_**Q143:** Any person who owns a white horse, and has a parent that doesn't own a horse. The parent is not part of the answer_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q143.png)

Anything on the right of an **'O'** is **optional**: if it has a valid assignment - it will be part of the answer. Otherwise - it won't.

- An 'O' may appear just before a relationship, a path, or a quantifier (excluding a quantifier at the start of the pattern)
- An 'O' may not appear right of an 'L', an 'X' or an '↛'
- An 'O' may not appear right of a '0' quantifier
- An 'O' may not appear right of a "relationships = 0", "paths = 0" or "→ = 0" aggregate condition 

Here are some examples:

_**Q144:** Any person who owns a white horse. If he has a parent who owns a horse - the parent and his horse will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q144.png)

_**Q145:** Any person who owns a white horse. If he has a parent that doesn't own a horse - the parent will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q145.png)

_**Q146:** Any person who owns a white horse. If he has a parent - the parent will be part of the answer as well. If his parent owns a horse - this horse will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q146.png)

_**Q147:** Any person. If he owns both a horse and a dragon - they will be a part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q147.png)

_**Q148:** Any person who owns both a horse and a dragon. If he has a parent - the parent will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q148.png)

_**Q149:** Any person. If he owns a horse - it will be a part of the answer as well. If he owns a dragon - it will be a part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q149.png)

_**Q150:** Any person who owns a horse or a dragon. If he has a parent - the parent will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q150.png)

## Untyped Entities

Sometimes the same relationship type can hold between different pairs of entity types (e.g. owns(Person, Dragon); owns(Guild, Dragon) ). We need untyped entities to express patterns such as "_any dragon and its owners_", when the owner can be either a person or a guild.
 
 In its simplest form, a empty red rectangle represents an entity of any type.

_**Q36:** Any person who owns something_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q036.png)

The entity type is implicitly constrained because of the relationship type. In Q36, the entity type is constrained to things that a person can own.

_**Q49:** Any 3 dragons with a cyclic freeze pattern, and their owners_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q049.png)

Similarly, the entity type of all owners is implicitly constrained to things that can own a dragon.

In addition to the implicit type constraints - explicit type constraints can be enforced by defining a set of allowed types or a set of disallowed types. Here are two examples:

_**Q37:** Any person who owns a horse or a dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q037.png)

_**Q38:** Any person who owns something which is neither a horse nor a dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q038.png)

Even when an untyped entity is on either side of an 'X' - the entity's type is constrained by relationship types. Here are some examples:

_**Q39:** Anything that can own a dragon, but doesn't_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q039.png)

_**Q40:** Any dragon that has no owner_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q040.png)

_**Q41:** Anything that can be owned, but is not owned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q041.png)

_**Q42:** Anything that can own something, but owns nothing_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q042.png)

_**Q43:** Any dragon that all of its owners (if any) are people_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q043.png)

## Paths

A path connects two entities - similar to a relationship. However, while a relationship is a direct connection between two entities, a path is an indirect connection : a path is a sequence of relationships and entities between the two entities.

Each path has a length. The length of the path equals to the number of entities along the path. Relationships are actually paths with length 0. The number of relationships along a path is always larger by one than the number of entities along the path.

A path is depicted by a red line between two entities. Above the line there must be a limitation to the path length, which is  expressed using one of the following operators: _< n, ≤ n, in [n1..n2], in {n1, n2, ...}_, where all numbers a positive integers.

An assignment to a path consists of all the relationships and entities along the path.

Here are two examples:

_**Q53:** Any person within graph distance ≤ 4 from Rogar Bolton_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q053.png)

_**Q55:** Anything within graph distance ≤ 3 from Rogar Bolton, Robin Arryn, and Arrec Durrandon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q055.png)

Optional constraints may be defined for the entities and relationships along the path.

Optional relationship constraints are listed in red curly brackets above the path's line. The brackets may list:

- Allowed relationship types - e.g. {'fires at', 'freezes'}
- Constraints on the number of relationships of each allowed type - e.g. {freezes < 2, fires at = 2}
- Constraints on the number of relationships of each allowed type in each direction - e.g. {→ freezes = 2, ← freezes = 1}
- Disallowed relationships types constraints - e.g. {freezes = 0}

Optional entity constraints are listed in red curly brackets below the path's line. The brackets may list:

- Allowed entity types - e.g. {Dragon}
- Constraint on the number of entities of each allowed type - e.g. {Dragon < 1}
- Disallowed entity types - e.g. {Dragon = 0}

Here are some examples:

_**Q44:** Any path with length ≤ 4 between these two dragons, which is composed only of 'freezes' relationships_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q044.png)

_**Q45:** Any path with length ≤ 4 between these two dragons, which is composed only of 'freezes' and 'fired at' relationships_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q045.png)

_**Q54:** Any person within graph distance ≤ 3 from Rogar Bolton, Robin Arryn, and Arrec Durrandon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q054.png)

_**Q46:** Any path with length ≤ 4 between a dragon owned by Rogar Bolton to a dragon owned by Robin Arryn, which is composed of up to 2 'freezes' relationships, and only of 'Dragon' entities_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q046.png)

## Shortest Paths

Instead of specifying a constraint on the path length - paths can be limited to the shortest ones that subject to the entities/relationships constraints. If, for example, the shortest path length that subject to the constraints is 3 - all paths with length 3 are valid assignments.

Here are two examples:

_**Q47:** All shortest paths between these two dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q047.png)

_**Q48:** All shortest paths between these two dragons, which are neither composed of 'freezes' relationships nor 'Dragon' entities_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q048.png)

## Path Segments

An alternative to constraints on the entities and relationships along the path, are constraints on **path segment types**. A path segment type is a valid chain of entities (concrete/aggregate/typed/logical/untyped) and relationship types that starts and ends with an entity. There is an 'overlap' between successive segment types:

- The type of the rightmost entity type of a segment must match the type of the leftmost entity in its successor
- The type of the leftmost entity type in the first segment of a path must match the entity type preceding the path
- The type of the rightmost entity type in the last segment of a path must match the entity type following the path

Each path is composed of consecutive path segments. 

A red table below the path defines constraints on the path's segments types. The table has three columns:

- A constraint on the number of allowed path segments of this type along the path
- The path segment type
- An indication whether a 'mirror image' of this path segment type is allowed as well

Here are some examples:

_**Q56:** constraints on path segment types_

In this example, 4 path segment types are allowed

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q056.png)

_**Q57:** constraints on path segment types_

In this example, one path segment type is disallowed, there must be between 2 and 3 path segments of one of two types, and any other path segments are allowed

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q057.png)

_**Q58:** constraints on path segment types_

In this example, the path must contain Rogar Bolton. Any other path segments are allowed

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q058.png)

## Property Tags

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB02.png)

**Property tag**, ('_{pt}_') - depicted by an index wrapped in curly brackets. Each green rectangle (entity's property / relationship property) has a property tag on its top-left corner. The numbering for property tags, aggregation tags and split tags is joint, and each tag is unique.

A property tag serves as a placeholder for the property's value in a given assignment. Property tags can be used:

* as part of another property tag's constraint (see Q108, Q109)
* as part of an aggregation tag's definition (see Q116, Q117)
* as part of an aggregation tag's constraint (see Q120)
* as part of a min/max aggregation (see Q131,Q132)
* as part of a split definition (see Q226,Q227)
* as part of a split constraint (see Q255)

Here are some examples:

_**Q108:** Any person who has the same birth-date as Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q108.png)

_**Q109:** Any person who his parent owned a horse or a dragon prior to his birth_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q109.png)

Note that if a branch contains only a green rectangle with no constraints (only with a property tag) - the branch is always satisfied.

_**Q110:** Any 3 dragons with cyclic freezes of more than 100 minutes in chronological order, and their owners_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q110.png)

_**Q111:** Any person who doesn't know someone with a birth date similar to his_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q111.png)

_**Q112:** Any person who owned a horse and a dragon in the same time frames_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q112.png)

Composite properties, as well as sub-properties, are tagged, and can be referenced similar to ordinary properties. Here are some examples:

_**Q266:** Any person who has the same name (first and last) as his parent (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q266-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q266-2.png)

_**Q267:** Any person who was a member of two guilds at intersecting timeframes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q267.png)

(Assuming that at least one of the _tf.till_ values is not empty. See also note under Q11). Note the red comparison operator.



## Entity Type Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB08.png)

A red rectangle (denoting an untyped entity) may contain an **entity type tag**, depicted by a numeric index wrapped in **purple triangular brackets**. An entity type tag serves as a placeholder for the entity type in a given assignment, and can be used to define constraints on the type of other untyped entities.

Here are some examples:

_**Q50:** Any person who owns (at least) two things of the same type_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q050.png)

_**Q51:** Any person who owns (at least) two things of different types_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q051.png)

_**Q52:** Any person who owns (at least) two things of different types, both are not horses_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q052.png)

## Multivalued Properties

Properties can contain a set of values of the same type. For example, a dragon may have several nicknames where each nickname is a string. The property type is 'array of strings' and is denoted as [string]. In general, the type [_t_] denotes an array of values - each of type _t_.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB09-03.png)

An array-constraint is expressed over the number of values (integer) for which a given value-constraint is satisfied: First, (_cmp_op expr1_) is evaluated over each value in the array. Then, the array-constraint is evaluated over the number of values that satisfies the value-constraint.

Here is an example:

_**Q252:** Any dragon that has at least 2 nicknames that contains 's'_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q252.png)

* In order to check if ANY nickname contains 's' - the condition would be _(contains 's') > 0_.
* In order to check if ALL nicknames contain 's' - the condition would be _(not contains 's') = 0_.

Functions over multivalued properties:

* _count([t])_ → int
* _distinct([t])_ → int (defined if the multivalued property supports duplicate values)

Functions over multivalued ordinal properties:

* _min([t])_ → t
* _min([t])_ → t
* _avg([t])_ → t
* _sum([t])_ → t (defined over _int_ and _double_, but not over _date_, _time_ nor _datetime_)

## Aggregations and Aggregation Tags

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB06.png)

todo: explain aggregation, aggregation constraint

**Aggregation tag**, ('_{at}_') - depicted by an index wrapped in curly brackets. Each orange rectangle (aggregation) has an aggregation tag on the top-left corner of its lower pert. The numbering for property tags, aggregation tags and split tags is joint, and each tag is unique.

An aggregation tag serves as a placeholder for the result of an aggregation. Aggregation tags can be used:

* as part of another aggregation tag's definition (see Q129,Q181)
* as part of another aggregation tag's constraint (see Q125,Q127)
* as part of a min/max aggregation (see Q91,Q132)
* as part of a split definition (see Q253)
* as part of a split constraint (see Q254)

## L1 Aggregation

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-L1.png)

- _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
- _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- → denotes the entity directly right of the aggregation (which must be typed/logical/untyped). When the aggregation is followed by a quantifier - → denotes all typed/logical/untyped entities directly right of the quantifier (see Q175, Q176)
- _et_ is an entity tag of a typed/logical/untyped entity

- _S1_ and _→/et_ may not intersect

- L1 appears below a relationship / path / quantifier-input. The relationship / path / quantifier may be wrapped by an '↛', an 'L' or an 'O'
- L1 appear directly right of the leftmost member of _S1 ∪ →/et_
- all _S1 ∪ →/et_ entities should be within scope at the relationship (see _scope_ later on)

- For each assignment combination to _S1_ entities: _{at}_ - **aggregation tag** - equals to the number of concrete '→' / _et_ entities that satisfy the pattern.
- An optional **constraint** on the number of '→' / _et_ entities that satisfy the pattern - for each assignment combination to _S1_ entities. The constraint is in one of these forms:
  - _= expr / ≠ expr / > expr / ≥ expr_
  - _in (expr .. expr) / in (expr .. expr] / in [expr .. expr) / in [expr .. expr]_
  - _in {expr, expr, ... expr}_
  
  _< expr_ and _≤ expr_ are not used. To avoid ambiguity - either _in [0 .. expr]_ or _in [1 .. expr]_ should be used. ≠ expr is satisfied only if _> 0_.

  **L1 filters assignment combinations to _S1_ entities - to only those combinations for which the number of '→' / _et_ entities that satisfy the pattern - satisfy the constraint.**

- The visual notation for the entity directly left of the aggregation is '←' instead of '_et_'. Similarly, the visual notation for the entities directly right of the aggregation is '→'. When there is a single entity directly left of the aggregation and a single entity directly right of the aggregation - the visual notation for both is 'pair' instead of '{_et_, _et_}'

_**Q59:** Any person having more than 2 parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q059.png)

_**Q60:** Any dragon that was frozen by exactly 5 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q060.png)

_**Q61:** Anything that owns more than 2 things_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q061.png)

_**Q62:** Any person who is within graph distance ≤ 4 from more than 5 people_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q062.png)

_**Q136:** Any dragon A that froze (dragons that froze dragons B). The cumulative number of distinct Bs (per A) is greater than 100  (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q136-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q136-2.png)

_**Q81:** Any dragon that didn't freeze any dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q081.png)

_**Q82:** Any dragon that wasn't frozen by any dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q082.png)

_**Q177:** Any dragon pair (A,B) were A was frozen by at least 10 B's, and froze each one of those (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q177-1.png)

First, any pair (A,B) that matches the pattern is found. Then, the aggregation constraint is checked:

For any concrete A:

* There are at least 10 concrete B's such that (B froze A, and A froze B)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q177-2.png)

For any concrete A:

* There are at least 10 concrete B's such that (B froze A, and A froze B)
* There are at least 10 concrete B's such that (A froze B, and B froze A)

_**Q178:** Any dragon A that was frozen by at least 10 dragons and either (i) A froze only one dragon - which is not one of those (ii) A froze at least 2 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q178.png)

First, any triplet (A,B,C) that matches the pattern is found. Then, the aggregation constraint is checked:

For each concrete A:

* There are at least 10 concrete B's such that (B froze A, and A froze a dragon that is not B)

Hence, for each concrete A:

* At least 10 dragons froze A and either (i) A froze only one dragon - which is not one of those (ii) A froze at least 2 dragons

_**Q85:** Any dragon that froze at least 10 dragons, and was frozen by at least 10 dragons (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q085-1.png)

This 2nd version is for illustrative purposes only...

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q085-2.png)

First, any triplet (A,B,C) that matches the pattern is found. Then, the aggregation constraints are checked:

For each concrete A:

* There are at least 10 concrete B's such that (B froze A, and A froze a dragon that is not B)
* There are at least 10 concrete C's such that (A froze C, and A was frozen by a dragon that is not C)

Hence, for each concrete A:

* At least 10 dragons froze A and either (i) A froze only one dragon - which is not one of those (ii) A froze at least 2 dragons

and also

* A froze at least 10 dragons and either (i) only one dragon froze A - which is not one of those (ii) at least 2 dragons froze A

Hence:

* A froze at least 10 dragons, and at least 10 dragons froze A

_**Q101:** Any person who owns at least 10 white horses_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q101.png)

_**Q102:** Any dragon that was frozen by at least 2 dragons; each of these 2 dragons was frozen by at least one dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q102.png)

_**Q246:** Any dragon B that froze dragons and that was frozen by more than 10 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q246.png)

_**Q247:** Any dragon C that more than 10 dragons (cumulatively) froze dragons that froze it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q247.png)

_**Q248:** Any dragon pair (A, C) where A froze more than 10 dragons that froze C_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q248.png)

_**Q166:** Any **dragon** that more than 5 Sarnorian subjects own a dragon that froze **it**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q166.png)

_**Q113:** Any person who knows at least 5 people with a birth data similar to his_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q113.png)

_**Q114:** Any person who owns more than 5 horses of the same color_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q114.png)

_**Q125:** Any dragon that the number of dragons it froze is greater than the number of dragons that froze it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q125.png)

_**Q126:** Any dragon that the number of dragons it froze is greater than the number of dragons it didn't freeze_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q126.png)

_**Q63:** Any Masons guild member who more than 5 Masons guild members don't know_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q063.png)

_**Q65:** Any person who doesn't own more than 2 (things that his spouse owns)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q065.png)

_**Q64:** Any **dragon** that between 0 and 4 (dragons owned by Sarnorian subjects) didn't freeze **it**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q064.png)

_**Q165:** Any **dragon** that between 0 and 4 (Sarnorian subjects own a dragon that didn't freeze **it**)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q165.png)

_**Q66:** Any person from whom more than 5 people are not within graph distance ≤ 6_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q066.png)

_**Q151:** Any person who owns more than 10 horses, at least one is Sarnorian. Only the Sarnorian horses will be returned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q151.png)

_**Q152:** Any person who owns more than 10 horses. Only the Sarnorian horses will be returned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q152.png)

_**Q121:** Any dragon that froze or fired at at least 10 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q121.png)

('→' is the entity directly right of the R-combiner (in this example - B))

_**Q122:** Any dragon that fired at dragon B, and fired at a dragon that fired at B - for at least 10 different B's_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q122.png)

_**Q175:** Any dragon that froze at least once, and fired at least once. The number of dragons it froze/fired at - is at least 10 (if a dragon was both froze and fired at - it would be counted twice)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q175.png)

_**Q249:**  Any dragon that froze at least one dragon, **and** fired at at least one dragon. Any dragon frozen - was frozen by at least 10 dragons. Any dragon fired at - was fired at by at least 10 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q249.png)

_**Q250:**  Any dragon that froze at least one dragon, **or** fired at at least one dragon. Any dragon frozen - was frozen by at least 10 dragons. Any dragon fired at - was fired at by at least 10 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q250.png)

_**Q176:** Any dragon that either (i) froze at least one dragon and fired at at least one dragon it didn't froze. The number of dragons it frozed/fired at is at least 10 (ii) froze at least 10 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q176.png)

_**Q243:** Any pair of people (A, D) where at least 5 of A's dragons froze one or more D's dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q243.png)

_**Q244:** Any pair of people (A, D) where at least 5 of A's dragons froze D's dragons, and at least 5 D's dragons were frozen by one or more A's dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q244.png)

## L2 Aggregation

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-L2.png)

- _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
- _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q251) typed/logical/untyped entities directly right of the aggregation

- L2 appears below a relationship / path / quantifier-input. The relationship / path / quantifier may be wrapped by an 'L' or an 'O'
- all _S1_ entities should be within scope at the relationship (see _scope_ later on)

- For each assignment combination to _S1_ entities: _{at}_ - **aggregation tag** - equals to the number of relationships / paths that satisfy the pattern.
- An optional **constraint** on the number of relationships / paths that satisfy the pattern - for each assignment combination to _S1_ entities. The constraint is in one of these forms:
  - _= expr / ≠ expr / > expr / ≥ expr_
  - _in (expr .. expr) / in (expr .. expr] / in [expr .. expr) / in [expr .. expr]_
  - _in {expr, expr, ... expr}_

  _< expr_ and _≤ expr_ are not used. To avoid ambiguity - either _in [0 .. expr]_ or _in [1 .. expr]_ should be used. ≠ expr is satisfied only if _> 0_.

  **L2 filters assignment combinations to _S1_ entities - to only those combinations for which number of relationships / paths that satisfy the pattern - satisfy the constraint.**

- The visual notation for the entity directly left of the aggregation is '←' instead of '_et_'. Similarly, the visual notation for the entities directly right of the aggregation is '→'. When there is a single entity directly left of the aggregation and a single entity directly right of the aggregation - the visual notation for both is 'pair' instead of '{_et_, _et_}'

_**Q71:** Any dragon that froze dragons more than 10 times (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q071.png)

_**Q72:** Any dragon A that was frozen exactly 10 times (cumulatively) (2 versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q072-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q072-2.png)

_**Q73:** Any dragon that froze dragons no more than 10 times (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q073.png)

_**Q74:** Any dragon that the number of times it was frozen (cumulatively) is not 10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q074.png)

_**Q79:** Any person with more than 5 paths (cumulatively) with length ≤ 4 to other people_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q079.png)

_**Q83:** Any dragon that didn't freeze any dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q083.png)

_**Q84:** Any dragon with no paths with length ≤ 3 to other dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q084.png)

_**Q104:** Any person who owned white horses at least 10 times (same or different horses)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q104.png)

_**Q105:** Any dragon A that was frozen exactly 2 times (cumulatively) by (dragons that each was frozen by at least one dragon)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q105.png)

_**Q245:** Any dragon B that was frozen at least once, and froze dragons exactly twice (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q245.png)

_**Q127:** Any dragon that froze more times dragons owned by Sarnorian subjects than dragons owned by Omberian subjects_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q127.png)

_**Q123:** Any dragon that either froze a dragon or was frozen by a dragon - at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q123.png)

(counting the number of concrete relationships directly right of the quantifier)

_**Q124:** Any dragon that either (froze a dragon) or (fired a dragon that fired a dragon) - at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q124.png)

_**Q173:** Any dragon that fired at at least 2 dragons, and fired at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q173.png)

(counting the number of *distinct* 'fired at' relationships)

_**Q174:** Any dragon that either (i) froze at least one dragon and fired at at least one dragon it didn't froze (ii) froze at least two dragons. If (i): the number times it froze / fired at dragons is at least 10; otherwise: the number of times it froze dragons is at least 10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q174.png)

_**Q251:** Any dragon that either (i) froze at least one dragon and fired at at least one dragon it didn't froze (ii) froze at least two dragons. Any dragon that was frozen - was frozen at least 10 times; any dragon that was fired at - was fired at at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q251.png)

_**Q75:** Any dragon pair (A, B) where B froze A between 8 and 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q075.png)

_**Q76:** Any dragon that froze Balerion between 8 and 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q076.png)

_**Q242:** Any pair of people (A, D) where at least 5 times any of A's dragons froze any of D's dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q242.png)

## L3 Aggregation

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-L3.png)

- _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
- _per →_ - _S1_ is the set of one or more (when following a quantifier) typed/logical/untyped entities directly right of the aggregation

- L3 appears below a relationship. The relationship may be wrapped by an 'L' or an 'O'
- all _S1_ entities should be within scope at the relationship (see _scope_ later on)

- _aggop_ is a _min/max/avg/sum_ aggregation of an ordinal property, or a _distinct_ aggregation of any property
- _relprop_ is a property of the relationship

- For each assignment combination to _S1_ entities: _{at}_ - **aggregation tag** - equals to the value of _aggop(relprop)_ of the relationships that satisfy the pattern.
- An optional **constraint** on the value of _aggop(relprop)_ of the relationships that satisfy the pattern - for each assignment combination to _S1_ entities. The constraint is in one of these forms:
  - _= expr / ≠ expr / > expr / ≥ expr_
  - _in (expr .. expr) / in (expr .. expr] / in [expr .. expr) / in [expr .. expr]_
  - _in {expr, expr, ... expr}_
  - _< expr_ / _≤ expr_ can be used only of _aggop_ is not _distinct_

  **L3 filters assignment combinations to _S1_ entities - to only those combinations for which the value of _aggop(relprop)_ of the relationships that satisfy the pattern - satisfy the constraint.**

- The visual notation for the entity directly left of the aggregation is '←' instead of '_et_'. Similarly, the visual notation for the entities directly right of the aggregation is '→'. When there is a single entity directly left of the aggregation and a single entity directly right of the aggregation - the visual notation for both is 'pair' instead of '{_et_, _et_}'

_**Q87:** Any dragon that was frozen at least once, and the cumulative duration he was frozen is smaller than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q087.png)

_**Q88:** Any dragon pair (A,B) where A froze B at least once, but the cumulative freezing duration is 0 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q088.png)

_**Q89:** Any dragon that freezes dragons for more than 3 different durations_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q089.png)

_**Q86:** Any dragon pair (A, B) where A froze B for a cumulative duration greater than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q086.png)

## L4 Aggregation

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-L4.png)

- _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
- _per →_ - _S1_ is the set of one or more (when following a quantifier) typed/logical/untyped entities directly right of the aggregation

- L4 appears below a relationship / path / quantifier-input. The relationship / path / quantifier may be wrapped by an '↛', an 'L' or an 'O'
- L1 appear directly right of the leftmost member of _S1 ∪ →/et_
- all _S1 ∪ →/et_ entities should be within scope at the relationship (see _scope_ later on)

- _aggop_ is a _min/max/avg/sum_ aggregation of an ordinal tag, or a _distinct_ aggregation of any tag
- {pt}/{at}/{st}/< ett > is a property tag / aggregation tag / split tag / entity type tag - defined on top of the L4 (in a previous filtering step) or right of the L4

- For each assignment to the _S1_ entities: _{at}_ - **aggregation tag** - equals to the value of _aggop(pt/at/st)_ of the subgraphs that satisfy the pattern.
- An optional **constraint** on the value of _aggop(pt/at/st)_ of the subgraphs that satisfy the pattern - for each assignment combination to the _S1_ entities. The constraint is in one of these forms:
  - _= expr / ≠ expr / > expr / ≥ expr_
  - _in (expr .. expr) / in (expr .. expr] / in [expr .. expr) / in [expr .. expr]_
  - _in {expr, expr, ... expr}_
  - _< expr_ / _≤ expr_ can be used only of _aggop_ is not _distinct_

  **L4 filters assignment combinations to _S1_ entities - to only those combinations for which the value of _aggop(pt/at/st)_ of the subgraphs that satisfy the pattern - satisfy the constraint.**

- The visual notation for the entity directly left of the aggregation is '←' instead of '_et_'. Similarly, the visual notation for the entities directly right of the aggregation is '→'. When there is a single entity directly left of the aggregation and a single entity directly right of the aggregation - the visual notation for both is 'pair' instead of '{_et_, _et_}'

_**Q116:** Any person who owns horses of no more than 3 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q116.png)

_**Q117:** Any person whose owned horses have an average weight greater than 450 Kg_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q117.png)

If a horse was owned twice - it would be counted only once.

_**Q134:** Any person that the number of distinct colors of all horses owned by people he knows - is not more than 3_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q134.png)

_**Q229:** Any person that the number of distinct colors of all horses owned by people he knows or he is offspring of - is not more than 3_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q229.png)

_**Q135:** Any person that the average weight of all horses owned by people he knows - is greater than 450 Kg_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q135.png)

_**Q137:** Any dragon A that froze dragons B - each froze at least one dragon which is not A. All these B's together froze dragons for more than 100 minutes cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q137.png)

_**Q139:** Any person who owns horses of the same number of colors as the number of colors of the horses owned by his parents cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q139.png)

_**Q167:** Any person who owns things of at least 3 types_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q167.png)

## Aggregate Conditions both before and after a Quantifier

todo

_**Q191:** Any dragon that froze X≥3 dragons and fired at Y≥3 dragons. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q191.png)

_**Q192:** Any dragon that froze dragon X≥3 times, and fired at dragons Y≥3 times. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q192.png)

_**Q193:** Any dragon that froze X≥3 dragons, fired at Y dragons, and fired Z≥3 times. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q193.png)

_**Q194:** Any dragon that froze dragons X times, fired at dragons Y≥3 times, and froze Z≥3 dragons. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q194.png)

_**Q158:** Any dragon that in any day of at least 10 days - the number of dragons it froze is greater than the number of dragons that froze it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q158.png)

## Min/Max Aggregations

todo

## M1 Min/Max Aggregation

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-M1.png)

- Optional:
  - _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
  - _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- _n_ is a positive integer
- _n {et, et, ...}_ - a set, _S2_, of entity tags of typed/logical/untyped entities

- _with min/max {et, et, ...}_ - a set, _S3_, of entity tags of typed/logical/untyped entities

- _S1_, _S2_ and _S3_ may not intersect

- M1 appears below a relationship / path / quantifier-input. The relationship / path / quantifier may be wrapped by an '↛' or an 'L'
- M1 appear directly right of the leftmost member of _S1_, _S2_ and _S3_
- Except for '&' quantifier - M1 aggregation cannot start a quantifier's branch
- The visual notation for the entity directly left of the aggregation is '←' instead of '_et_'. Similarly, the visual notation for the entities directly right of the aggregation is '→'. When there is a single entity directly left of the aggregation and a single entity directly right of the aggregation - the visual notation for both is 'pair' (as in 'per pair') or 'pairs' (as in '5 pairs with...') instead of '{_et_, _et_}'

If _S1_ is not given:

**M1 limits the assignment combinations to entities _S2_ - to the _n_ assignment combinations with the minimial / maximial positive number of assignment combinations of entities _S3_.**

If _S1_ is given:

**For each assignment combination to entities _S1_ - M1 limits the assignment combinations to entities _S2_ - to the _n_ assignment combinations with the minimal / maximal positive number of assignment combinations of entities _S3_.**

- Suppose the pattern is "5 → with max D" but there are only 3 →'s with >0 assignments to D - Only these 3 will be included.
- Suppose the pattern is "5 → with max D" but there are 10 →'s with identical max number of assignments to D - all 10 will be included.

_**Q67:** The 3 people with the largest number of parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q067.png)

_**Q68:** The 2 dragons that were frozen by the largest number of dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q068.png)

_**Q69:** The 2 things that own the largest number of things_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q069.png)

_**Q70:** The 5 people who the number of people within graph distance ≤ 4 from them - is the smallest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q070.png)

_**Q196:** Any **dragon** owned by Brandon Stark, and the 3 dragons **it** froze that froze the largest number of dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q196.png)

_**Q197:** Any person and his 3 dragons that the dragons they froze - froze the largest number of distinct dragons cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q197.png)

_**Q234:** Any person and the 3 dragons whose dragons froze - that froze the largest number of dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q234.png)

_**Q236:** Any person and the 3 dragons whose dragons froze - that were frozen by the largest number of his dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q236.png)

_**Q227:** Any **dragon** owned by Brandon Stark, and the 3 dragons **it** froze or fired at - that froze the largest number of dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q227.png)

_**Q238:** For any pair of people (A,D) where A's dragons froze D's dragons - A's 3 dragons that froze the largest number of D's dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q238.png)

## M2 Min/Max Aggregation

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-M2.png)

- Optional:
  - _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
  - _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- _n_ is a positive integer
- _n {et, et, ...}_ - a set, _S2_, of entity tags of typed/logical/untyped entities

- _S1_ and _S2_ may not intersect

- M2 appears below a relationship / path / quantifier-input. The relationship / path / quantifier may be wrapped by an 'L'
- The visual notation for the entity directly left of the aggregation is '←' instead of '_et_'. Similarly, the visual notation for the entities directly right of the aggregation is '→'. When there is a single entity directly left of the aggregation and a single entity directly right of the aggregation - the visual notation for both is 'pair' (as in 'per pair') or 'pairs' (as in '5 pairs with...') instead of '{_et_, _et_}'

If _S1_ is not given:

**M2 limits the assignment combinations to entities _S2_ - to the _n_ assignment combinations with the minimal / maximal positive number of relationships / paths on its top.**

If _S1_ is given:

**For each assignment combination to entities _S1_ - M2 limits the assignment combinations to entities _S2_ - to the _n_ assignment combinations with the minimal / maximal positive number of relationships / paths on its top.**

_**Q78:** The 4 dragons that froze Balerion the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q078.png)

_**Q171:** The 2 dragons that were frozen the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q171.png)

_**Q172:** The 5 people with the smallest number of paths with length ≤ 4 to some person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q172.png)

_**Q77:** The 5 dragon pairs (A, B) with the largest number of times B froze A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q077.png)

_**Q80:** The 3 person pairs with the largest number of paths with length ≤ 4 between them_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q080.png)

_**Q195:** Any dragon owned by Brandon Stark, and the 3 dragons it froze the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q195.png)

_**Q231:** Any person and the 3 dragons (D) that were frozen by dragons that were frozen the largest number of times by his dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q231.png)

_**Q228:** Any dragon owned by Brandon Stark that fired at at least 2 dragons, and the 3 dragons it fired the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q228.png)

(counting the number of concrete relationships directly right of the quantifier)

_**Q237:** For any pair of (A - a dragons owner, and C - a dragon that was frozen by A's dragons) - the 3 dragons owned by A that froze C the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q237.png)

_**Q239:** For any pair of people (A,D) where A's dragons froze D's dragons - the 3 pairs of (A's dragon B, D's dragon C) where B froze C the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q239.png)

## M3 Min/Max Aggregation

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-M3.png)

- Optional:
  - _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
  - _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- _n_ is a positive integer
- _n {et, et, ...}_ - a set, _S2_, of entity tags of typed/logical/untyped entities

- _S1_ and _S2_ may not intersect

- M3 appears below a relationship. The relationship may be wrapped by an 'L'.
- _aggop_ is a _min/max/avg/sum_ aggregation of an ordinal property, or a _distinct_ aggregation of any property
- _relprop_ is a property of the relationship
- Except for '&' quantifier - M3 aggregation cannot start a quantifier's branch
- The visual notation for the entity directly left of the aggregation is '←' instead of '_et_'. Similarly, the visual notation for the entities directly right of the aggregation is '→'. When there is a single entity directly left of the aggregation and a single entity directly right of the aggregation - the visual notation for both is 'pair' (as in 'per pair') or 'pairs' (as in '5 pairs with...') instead of '{_et_, _et_}'

If _S1_ is not given:

**M3 limits the assignment combinations to entities _S2_ - to the _n_ assignment combinations with the minimal / maximal value of _aggop(relprop)_ of the assignments to the relationship on its top.**

If _S1_ is given:

**For each assignment combination to entities _S1_ - M3 limits the assignment combinations to entities _S2_ - to the _n_ assignment combinations with the minimal / maximal value of _aggop(relprop)_ of the assignments to the relationship on its top.**

_**Q90:** The 4 dragon pairs (A, B) where A froze B for the longest cumulative period_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q090.png)

_**Q182:** Any dragon owned by Brandon Stark, and the 3 dragons it froze for the longest cumulative period_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q182.png)

_**Q233:** Any dragon A than froze dragons Bs that froze dragons Cs, and the 3 Cs for which A froze Bs for the longest cumulative period_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q233.png)

_**Q201:** For any dragon that froze at least 10 dragons: the 3 dragons it froze for the longest cumulative period_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q201.png)

## M4 Min/Max Aggregation

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-M4.png)

- Optional:
  - _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
  - _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- _n_ is a positive integer
- _n {et, et, ...}_ - a set _S2_ of typed/logical/untyped entities

- _S1_ and _S2_ may not intersect

- M4 appears below a relationship / path / quantifier-input. The relationship / path / quantifier may be wrapped by an 'L'
- M4 appear directly right of the leftmost member of _S1_ and _S2_
- {pt} is a property tag of an ordinal property - defined on top of the aggregation (in a previous filtering step) or right of the aggregation
- {at}/{st} is an aggregation tag / split tag - defined on top of the aggregation (in a previous filtering step) or right of the aggregation
- Except for '&' quantifier - M4 aggregation cannot start a quantifier's branch
- The visual notation for the entity directly left of the aggregation is '←' instead of '_et_'. Similarly, the visual notation for the entities directly right of the aggregation is '→'. When there is a single entity directly left of the aggregation and a single entity directly right of the aggregation - the visual notation for both is 'pair' (as in 'per pair') or 'pairs' (as in '5 pairs with...') instead of '{_et_, _et_}'

If _S1_ is not given:

**M4 limits the assignment combinations to entities _S2_ - to the _n_ assignment combinations with the minimal / maximal value of {pt}/{at}/{st}.**

If _S1_ is given:

**For each assignment combination to entities _S1_ - M4 limits the assignment combinations to entities _S2_ - to the _n_ assignment combinations with the minimal / maximal value of {pt}/{at}/{st}.**

_**Q130:** The 4 oldest people_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q130.png)

_**Q131:** The 4 oldest males_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q131.png)

_**Q91:** The 4 dragons with the maximal (shortest period they were frozen for)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q091.png)

_**Q92:** The 4 dragons with the maximal (average duration they froze dragons for)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q092.png)

_**Q133:** The 4 people who the average weight of their horses is maximal_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q133.png)

_**Q132:** The 4 people who own horses of the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q132.png)

_**Q138:** The 4 people who the people each of them knows - cumulatively own horses of the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q138.png)

_**Q183:** The 3 dragons that dragons owned by Brandon Stark froze for the longest cumulative period_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q183.png)

_**Q168:** The 3 people who the number of types of things each of them owns - is the largest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q168.png)

_**Q118:** Any person and his 3 oldest offsprings_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q118.png)

_**Q119:** Any person and his 3 youngest sons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q119.png)

_**Q230:** Any person and the 3 people he (knows or knows an ofspring of) - that owns the heaviest horses_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q230.png)

_**Q232:** Any person and the 3 heaviest horse owned by people he (knows or knows an ofspring of)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q232.png)

## M5 Min/Max Aggregation

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-M5.png)

- Optional:
  - _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
  - _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- _n_ is a positive integer
- _relprop_ is an ordinal property of the relationship

- M5 appears below a relationship. The relationship may not be wrapped
- The visual notation for the entity directly left of the aggregation is '←' instead of '_et_'. Similarly, the visual notation for the entities directly right of the aggregation is '→'. When there is a single entity directly left of the aggregation and a single entity directly right of the aggregation - the visual notation for both is 'pair' instead of '{_et_, _et_}'

If _S1_ is not given:

**M5 limits the assignments to the relationship - to the _n_ assignments with the smallest / largest value of _relprop_.**

If _S1_ is given:

**For each assignment combination to entities _S1_ - M5 limits the assignments to the relationship - to the _n_ assignments with the smallest / largest value of _relprop_.**

_**Q241:** The 4 longest freezes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q241.png)

_**Q161:** For any dragon that froze at least one dragon at least once: the 4 longest times it froze some dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q161.png)

_**Q160:** For any dragon pair (A, B) where A froze B at least once: The 4 longest times A froze B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q160.png)

_**Q240:** For any pair of people (A, D): The 3 longest freezes where any of A's dragons froze any of D's dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q240.png)

## Aggregation Chains

todo

_**Q96:** Any dragon that was frozen by Balerion, in  1/1/1010 or later, more than 10 times, for periods shorter than 10 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q096.png)

_**Q259:** Any person who since 1011 become owner of 0 to 4 horses_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q259.png)

_**Q115:** Any person who at a certain date became an owner of more than 5 horses (version 1)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q115-1.png)

_**Q93:** Any dragon that froze at least 3 dragons for more than 10 minutes. The number of times it froze each of these dragons is at least 5_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q093.png)

Filtering stages:

- Pass only (freezes that are longer than 10 minutes)
- Pass only dragons A that froze at least 3 dragons for more than 10 minutes
- Pass only dragon pairs (A, B) where A froze B for more than 10 minutes - at least 5 times

_**Q94:** Any dragon that froze at least 3 dragons - each at least 5 times for more than 10 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q094.png)

Filtering stages:

- Pass only (freezes that are longer than 10 minutes)
- Pass dragon pairs (A, B) where A froze B at least 5 time for more than 10 minutes
- Pass only dragons A that froze for more than 10 minutes - at least 3 dragons

_**Q163:** Any dragon that the average duration of the 10 shortest times it freezes is greater than 60 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q163.png)

_**Q162:** Any dragon pair (A, B) where the second shortest period A froze B is longer than 60 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q162.png)

_**Q268:** Any dragon pair (A, B) where the average duration of the 11th-20th longest freezes A forze B is longer than 60 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q268.png)

_**Q272:** Any dragon pair (A, B) where the longest freeze duration is at least 10 times longer than the shortest freeze duration_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q272.png)

_**Q185:** Any dragon that was frozen by Balerion: at least once in 1/1/1010 or later, at least once for less than 10 minutes, more than 10 times (in 1/1/1010 or later, or for less than 10 minutes)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q185.png)

_**Q95:** Any dragon that was frozen by Balerion: there were more than 5 freezes for more than 10 minutes, and their total duration was longer than 100 minutes_ (3 versions)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q095-1.png)

The two 'per pair' conditions could be chained instead. The meaning would be similar:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q095-2.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q095-3.png)

_**Q97:** Any dragon that froze more than 3 dragons - each more than 10 times, or for a cumulative duration of more than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q097.png)

_**Q98:** First, pass dragons A that froze more than 3 dragons. Then, pass dragon pairs (A,B) where A froze B more than 10 times, or froze B for a cumulative duration of more than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q098.png)

_**Q186:** Any dragon to that Balerion froze more than 10 times for less than 10 minutes, and at least once for 10 minutes or more_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q186.png)

_**Q99:** Any dragon to that Balerion froze more than 10 times for less than 10 minutes, and not once for 10 minutes or more_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q099.png)

_**Q100:** Any dragon to which Balerion froze more than 10 times for less than 10 minutes, more than 10 times in 1/1/1010 or later, more than 15 times for less than 10 minutes in 1/1/1010 or later, and more than 100 times altogether_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q100.png)

_**Q199:** Any dragon that (froze more than 10 times each of more than 10 dragons) and (froze more than 20 times each of less than 10 dragons)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q199.png)

_**Q200:** Any dragon that (froze more than 10 times each of more than 10 dragons. For each of these 10 dragons - the freezes had exactly 2 distinct durations) and (froze more than 20 times each of less than 10 dragons. The average freeze duration of all these dragons - is greater than 3 minutes)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q200.png)

_**Q120:** Any person whose 3 oldest offspring cumulative height is smaller than his own height_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q120.png)

_**Q202:** Any person whose 3 oldest offspring average height is smaller than the average height of all his offsprings_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q202.png)

_**Q140:** Any person who his 3 oldest sons cumulatively own horses of 3 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q140.png)

_**Q141:** Any person who his 3 oldest sons cumulatively own horses of the same number of colors as those owned cumulatively by his 3 youngest daughters_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q141.png)

## Aggregation Sequences

todo

_**Q128:** Any person and his 3 offspring that own horses of the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q128.png)

_**Q198:** Any person and his 3 dragons that (for each of them: the 4 dragons it froze that froze the largest number of dragons) - froze the largest number of distinct dragons cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q198.png)

_**Q103:** Any dragon A that froze at least 3 dragons - each was frozen by at least 4 dragons other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q103.png)

_**Q106:** Any dragon A that froze dragons at least 3 times - each was frozen at least 4 times by dragons other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q106.png)

_**Q107:** Any **dragon** that (the number of dragons owned by 5 people each, that froze **it**) is 5, and that the number of times **it** was frozen by those dragons (cumulatively) is not 5_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q107.png)

_**Q169:** Any person that (each of his offspring who owns at least 3 horses - owns horses of at least 3 colors)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q169.png)

_**Q129:** Any person that (each of his offspring who owns at least one horse - owns a different number of horses)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q129.png)

_**Q181:** Any **dragon** with no intersection between the groups of dragons froze by any two dragons **it** froze_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q181.png)

_**Q164:** Any dragon that the number of times dragons he froze have frozen dragons (cumulatively) - is equal to the number of times dragons he fired at have fired at dragons (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q164.png)

_**Q179:** Any dragon pair (A, B) where A froze B for a cumulative period longer than the cumulative period that B froze dragons (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q179-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q179-2.png)

_**Q213:** Out of the dragon pairs (A, B) where A froze B for a cumulative period longer than the cumulative period that B froze dragons - the 5 pairs with the largest number of times A froze B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q213.png)

Note that the order of the filtering stages along the left chain can be switched. The semantics would remain the same.

_**Q180:** Any dragon pair (A, B) where the cumulative duration A froze B or vice versa - is greater the cumulative duration A froze dragons, and greater than the cumulative duration B froze dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q180.png)

## L1/L2/L3/L4 Aggregation below a Split

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Split-L1.png)

todo

_**Q115:** Any person who at a certain date became an owner of more than 5 horses (version 2)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q115-2.png)

_**Q217:** Any Dragon and the dragon it froze - on days it froze not more than 5 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q217.png)

_**Q218:** Any Person and the entities he owns - of types he owns at least 5 entities_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q218.png)

_**Q219:** Any Person and all his horses - of colors he owns at least 3 horses_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q219.png)

## L1/L2/L3/L4 Aggregation below a Split, and Constraint on the Number of Splits

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Split-L2.png)

- A **constraint** on the number of splits that satisfy the pattern may appear below a split. The constraint is in one of these forms:
  - _= expr / ≠ expr / > expr / ≥ expr_
  - _in (expr .. expr) / in (expr .. expr] / in [expr .. expr) / in [expr .. expr]_
  - _in {expr, expr, ... expr}_
  
  _< expr_ and _≤ expr_ are not used. To avoid ambiguity - either _in [0 .. expr]_ or _in [1 .. expr]_ should be used. ≠ expr is satisfied only if _> 0_.

_**Q153:** Any dragon that in each day of at least 11 days - froze no more than 5 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q153.png)

_**Q214:** Any person who owns entities of at least 4 types. For each type - at least 5 entities_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q214.png)

_**Q154:** Any dragon that in each of at least 4 years - in each of at least 11 days - froze no more than 5 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q154.png)

_**Q155:** Any dragon that in each of at least 11 days - froze more than 100 minutes cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q155.png)

_**Q157:** Any person who owns at most 3 horses of the same color - for more than 5 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q157.png)

_**Q156:** Any dragon that froze dragon for more than 1000 minutes cumulatively - in the days it froze dragons for more than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q156.png)

_**Q235:** Any dragon that the number of dragons it froze on average - on months it froze dragons in - is at least 10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q235.png)

_**Q254:** Any dragon that in each year it froze dragons - it froze more than 3 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q254.png)

_**Q255:** Any dragon that the number of days in which it forze more than 10 dragons - equals to the length of his name_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q255.png)

## Global L1/L2/L3/L4 Aggregation below a Split

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Split-L3.png)

_**Q261:** Any horse of any color of which there are at least 10 horses_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q261.png)

_**Q270:** Any person and his horses - of the horse colors for which there are more than 5 horse ownerships by persons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q270.png)

_**Q271:** Any person and his horses - of the horse colors for which the average ownership start date is at least 1/1/1010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q271.png)

_**Q263:** Any horse of each color of which the average horses' weight is greater than 450 Kg_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q263.png)

_**Q265:** Any horse color of which the average horses' owners' height is at least 180 cm_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q265.png)

## M1/M2/M3/M4/M5 Aggregation below a Split

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Split-M1.png)

todo

_**Q215:** For any color of dragons that Balerion froze - the 3 dragons it froze the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q215.png)

_**Q253:** For each number of dragon's owners - the 3 dragons Balerion froze the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q253.png)

_**Q273:** For each horse color - the 3 horse ownership with the latest ownership start date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q273.png)

## Split before a Quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Split-Before-Quantifier1.png)

_**Q260:** Any dragon that in any day of at least 10 days: (i) the number of dragons it froze is greater than the number of dragons that froze it, and (ii) it froze / was frozen at least 5 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q260.png)

(See Q158 for comparison)

## Min/Max Aggregations on Splits

todo

## P1 Min/Max Aggregation on Splits

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-P1.png)

- P1 may appear below a 'split by _relprop_' below a relationship. The relationship may be wrapped by an 'L'
- _relprop_ is a property of the relationship

- P1 may appear below a 'split by _{pt/at/st}/< ett >_' below a query-start / relationship / path /  quantifier-input. The relationship / path / quantifier may be wrapped by an '↛' or an 'L'
- {pt}/{at}/{st}/< ett > is a property tag / aggregation tag / split tag / entity type tag - defined on top of the P1 (in a previous filtering step) or right of the '→' entity

- Optional:
  - _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
  - _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- _n_ is a positive integer

- _with min/max {et, et, ...}_ - a set, _S2_, of entity tags of typed/logical/untyped entities

- _S1_ and _S2_ may not intersect

_**Q262:** Any horse of the 3 colors of which the number of horses is maximal_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q262.png)

_**Q224:** Any person and his horses - of the 3 horse colors with the smallest positive number of horse owners (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q224-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q224-2.png)

_**Q220:** Any person and his horses - of the 3 colors he owns the largest number of horses_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q220.png)

## P2 Min/Max Aggregation on Splits

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-P2.png)

- P2 may appear below a 'split by _relprop_' below a relationship. The relationship may be wrapped by an 'L'
- _relprop_ is a property of the relationship

- P2 may appear below a 'split by _{pt/at/st}/< ett >_' below a relationship / path / quantifier-input. The relationship / path / quantifier may be wrapped by an 'L'
- {pt}/{at}/{st}/< ett > is a property tag / aggregation tag / split tag / entity type tag - defined on top of the P1 (in a previous filtering step) or right of the '→' entity

- Optional:
  - _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
  - _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- _n_ is a positive integer

_**Q225:** Any person and his horses - of the 3 horse colors with the smallest positive number of horse ownerships by persons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q225.png)

_**Q221:** Balerion and the dragons it froze - of the 3 colors he froze dragons the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q221.png)

## P3 Min/Max Aggregation on Splits

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-P3.png)

- P3 may appear below a 'split by _relprop_' below a relationship. The relationship may be wrapped by an 'L'
- _relprop1_ is a property of the relationship

- P3 may appear below a 'split by _{pt/at/st}/< ett >_' below a relationship. The relationship may be wrapped by an 'L'
- {pt}/{at}/{st}/< ett > is a property tag / aggregation tag / split tag / entity type tag - defined on top of the P1 (in a previous filtering step) or right of the '→' entity

- Optional:
  - _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
  - _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- _n_ is a positive integer

- _aggop_ is a _min/max/avg/sum_ aggregation of an ordinal property, or a _distinct_ aggregation of any property
- _relprop2_ is a property of the relationship (different from _relprop1_)

_**Q269:** Any person and his horses - of the 3 horse colors of which the average ownership start date is the latest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q269.png)

_**Q222:** Any Person, and his horses - of the 3 colors for which his average ownership start date is the latest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q222.png)

## P4 Min/Max Aggregation on Splits

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-P4.png)

- P4 may appear below a 'split by _relprop_' below a relationship. The relationship may be wrapped by an 'L'
- _relprop_ is a property of the relationship

- P4 may appear below a 'split by _{pt/at/st}/< ett >_' below a relationship / path /  quantifier-input. The relationship / path / quantifier may be wrapped by an '↛' or an 'L'
- {pt}/{at}/{st}/< ett > is a property tag / aggregation tag / split tag / entity type tag - defined on top of the P1 (in a previous filtering step) or right of the '→' entity

- Optional:
  - _per {et, et, ...}_ - a set, _S1_, of entity tags of typed/logical/untyped entities
  - _per →_ - _S1_ is the set of one or more (when following a quantifier - see Q249, Q250) typed/logical/untyped entities directly right of the aggregation

- _n_ is a positive integer

- {pt} is a property tag of an ordinal property - defined on top of the aggregation (in a previous filtering step) or right of the aggregation
- {at}/{st} is an aggregation tag / split tag - defined on top of the aggregation (in a previous filtering step) or right of the aggregation

_**Q264:** Any horse of the 3 colors of which the average horses' weight is maximal_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q264.png)

_**Q226:** Any person and his horses - of the 3 horse colors of which the average height of the horse owners is minimal_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q226.png)

_**Q223:** Any Person, and his horses - of the 3 colors for which the cumulative weight if his horses in the largest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q223.png)

## Min/Max Filter on Splits before a Quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Split-Before-Quantifier2.png)

todo

_**Q216:** Any dragon that Balerion froze in the three 30-day timeframes in which it froze dragons the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q216.png)

## Split Tags

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB11.png)

**Split tag**, ('_{st}_') - depicted by an index wrapped in curly brackets. Each 'splits' purple rectangle has a property tag on its top-left corner. The numbering for property tags, aggregation tags and split tags is joint, and each tag is unique.

A split tag serves as a placeholder for the number of splits that satisfy the condition. Split tags can be used:

* as part of a another split definition
* as part of a another split constraint (see Q159)
* as part of a min/max aggregation

_**Q159:** Any **dragon** for which there are more days where (he froze/was frozen at least 5 times, and the number of dragons **it** froze is greater than the number of dragons that froze **it**) than days where (he froze/was frozen at least 5 times, and the number of dragons that froze **it** is greater than the number of dragons **it** froze)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q159.png)

## Aggregate Entities

An aggregate entity is a virtual concrete entity, that encapsulates several entities. It is defined by:

* The entities it encapsulates: A set of
  * Concrete entities
  * Typed entities with optional constraints
  * Untyped entities with constraints
* A entity type name assigned to the aggregation

Aggregate entities can be defined, and then used in queries.

Here are some definition examples:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB13.png)

* _'Kings'_ is defined as an encapsulation of 4 concrete entities
* _'Old People'_ is defined as an encapsulation of all people born before 920
* _'Old males'_ is defined as an encapsulation of all males born before 920
* _'Black things'_ is defined as an encapsulation of all things which have a property 'color' with value 'black'
* _'Sarnorian'_ is defined as an encapsulation of all Sarnorian subjects and all guilds registered in Sarnor

An encapsulated entity in a query will show as an aggregate entity in the query's result. It won't 'disassemble' into the entities it encapsulates. 

Aggregate entities have the following auto-generated aggregate properties:

* _'count'_ - the number of encapsulated entities
* _'e.count'_ - the number of encapsulated entities of type _e_ (valid for any encapsulated entity type)
* _'e.p.distinct'_ - the number of distinct values of property _p_ of encapsulated entity type _e_ (valid for any property type of any encapsulated entity type)
* _'e.p.min', 'e.p.max', 'e.p.avg', 'e.p.sum'_ - the min, max, sum, and average of property _p_ of entity type _e_ (valid for any numeric property type of any encapsulated entity type)

Using aggregate entities in queries:

* Aggregate entities are used in a similar manner to concrete entities
* Adjacent relationship types should support at least one encapsulated entity type
* Constraints cannot be defined for aggregate entities
* Aggregate entities cannot be counted (L1, M1, M2, M3, M4 aggregations)

Here are some examples:

_**Q208:** Any dragon owned by an entity encapsulated within 'Kings' - since 1/1/1011 or since a later date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q208.png)

The aggregate entity 'Kings' will be part of the query result. It won't be disassembled into its four members.

_**Q209:** Any dragon than froze at least 3 dragons owned by entities encapsulated within 'Sarnorian'_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q209.png)

If, for example, some dragon froze 2 dragons owned by Stark, and 1 dragon owned by Bolton - it would be part of the answer. Again, 'Kings' will be a part of the query result.

_**Q210:** Any person who has at least 3 'owns' relationships with entities encapsulated within 'Black Things'_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q210.png)

_**Q211:** Any path with length ≤ 4 between an entity encapsulated within 'Sarnorian' and an entity encapsulated within 'Kings'_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q211.png)

_**Q212:** Are there more than 10 days in which at least 10 ownership relationships started between entities encapsulated within 'Old People' and entities encapsulated within 'Black Things'?_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q212.png)

## Logical Entity Types

A logical entity type can be assigned to specific entities. It is defined by:

* The entities it assigns a new type name to:
  * Concrete entities
  * Aggregate entities
  * Typed entities with optional constraints
  * Untyped entities with constraints
* A entity type name assigned to each such entity

Logical entities types can be defined, and then used in queries.

Here are some definition examples:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB12.png)

* _'King'_ is defined as one of 4 concrete entities
* _'Old Person'_ is defined as a person born before 920
* _'Old male'_ is defined as a male born before 920
* _'Black thing'_ is defined as a an entity that has a property 'color' with value 'black'
* _'Sarnorian'_ is defined as a a Sarnorian subject, or a Sarnorian registered guild
* _'Royalty'_ is defined as one of 4 aggregate entities

In a query's result - entities of logical type are resolved to concrete entities (similar to typed and untyped entities).

Logical entity types have the following properties:

* _'et.p'_ - where _et_ is an entity type used in the definition, and _p_ is a name of a property of that entity type

Using logical entity types in queries:

* Logical entity types are used in a similar manner to typed entities
* Adjacent relationship types should support at least one entity type used in the definition

Here are examples of patterns that incorporate the logical entity types defined above:

_**Q203:** Any dragon owned by a King since 1/1/1011 or since a later date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q203.png)

_**Q204:** Any dragon than froze at least 3 dragons owned by Sarnorians_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q204.png)

_**Q205:** Any person who has at least 3 'owns' relationships with 'Black Things'_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q205.png)

_**Q206:** Any 'Sarnorian' and 'King' pair with graph distance ≤ 4_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q206.png)

_**Q207:** Are there more than 10 days in which at least 10 ownership relationships started between a certain 'Old Person' and a certain 'Black Thing'?_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q207.png)

## Logical Relationship Types

A logical relationship type is defined by:

* A pattern
* Two typed/untyped entities in the pattern
* A relationship type name assigned to each such relationship

A logical relationship type can be either directional or bidirectional.

Logical relationship types can be defined, and then used in queries.

Logical relationships in a query - appear as logical relationships in the query's result as well.

Here are some definition examples:

**LR1:** _sibling_ - a bidirectional relationship. Two people are siblings if they share a parent

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/LR01.png)

**LR2:** _cousin_ - a bidirectional relationship. Two people are cousins if their parents are siblings

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/LR02.png)

**LR3:** _prison-mate_ - a bidirectional relationship. Two people are prison-mates if they were imprisoned in the same prison at intersecting timeframes

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/LR03.png)

**LR4:** _1st degree_ - a bidirectional relationship. Two people have a 1st degree relationship if one is an offspring of the other, if they are siblings, or if they are spouses

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/LR04.png)

**LR5:** _2nd degree_ - a bidirectional relationship. Two people have a 2nd degree relationship if there is a person who have 1st degree relationship with both of them

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/LR05.png)

**LR6:** _family friend_ - a directional relationship. A person is family friend of person A if (s)he is A's friend, if (s)he is a friend of A's 1st degree, or if (s)he is a friend of A's 2nd degree

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/LR06.png)

**LR7:** _tmo_ - a directional relationship. Person A is a tradition maintainer offspring of person C if A is an offspring of C, and they are both members of the same guild.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/LR07.png)

## Tag Rules

**R1:** Only one property tag can be assigned to any property.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag01.png)

**R2 - Tag scope:** For any quantifier except '&' - a tag defined in a branch cannot be referenced in other branches, and cannot be referenced left of the quantifier.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag03-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag03-2.png)

**R3 - Tag scope:** A tag defined right of an 'X' - cannot be referenced left of its definition. Additionally - A tag defined right of an 'X' on a quantifier's branch - cannot be referenced in other branches.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag05-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag05-2.png)

**R4 - Tag scope:** A tag defined right of an 'O' - cannot be referenced left of its definition. Additionally - A tag defined right of an 'O' on a quantifier's branch - cannot be referenced in other branches.

**R5 - Tag scope:** A tag defined right of an '↛' - cannot be referenced left of its definition. Additionally - A tag defined right of an '↛' on a quantifier's branch - cannot be referenced in other branches.

**R6:** Circular conditions are invalid.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag02.png)

**R7:** Several branches of an '&' quantifier may not reference tags circularly (e.g. branch 1 reference a tag defined in branch 2 and vice versa). There must be a valid order to evaluate branches.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag04.png)

**R8 todo: delete:** An aggregation can only reference tags defined on its right.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag08.png)

**R9 todo: delete:** A tag defined right of an aggregator - cannot be referenced in other branches.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag09.png)

**R10 - Tag scope:** A relationship's property tag defined as part of an aggregation chain - cannot be used in other branches.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag10.png)

## Aggregation Rules

**R11:** Concrete/aggregate entities cannot be counted (L1, M1, M2, M3, M4 aggregation).

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Agg01.png)

**R12:** Aggregations cannot be used when there is no entity on the right.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Agg02.png)

**R13:** A tag defined right of an L3 aggregator - cannot be referenced in the aggregate condition.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Agg03.png)

**R14:** A tag defined in an L3 aggregator - cannot be referenced right of its definition.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Agg04.png)
