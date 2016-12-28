<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-89133312-1', 'auto');
  ga('send', 'pageview');

</script>

## V1 - A Visual Query Language for Schema-based Property Graphs

Copyright (c) 2017 Lior Kogan (koganlior1 [at] gmail [dot] com)

This work is licensed under the [CC BY-NC-SA 4.0 licence](https://creativecommons.org/licenses/by-nc-sa/4.0/). For other uses please contact the author.

**This is an early draft. It’s known to be incomplet and incorrekt.**

## The Property Graph Data Model

A [*graph*](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)) is an ordered pair G = (V, E) comprising a set V of vertices (nodes) together with a set E of edges (arcs), which are 2-element subsets of V.

A **property graph** (AKA attributed graph) is a graph where

- Graph vertices represent entities.
  An entity is an objects or ‘thing’ in our mini-world, with an independent existence and which is distinguishable from other objects (e.g. a person, a vehicle, a product)
- Graph edges represent relationships between pairs of entities (e.g. 'owns', 'friend of'). Usually all edges are directional.
- Each vertex has a set of descriptive features called properties (AKA attributes) (e.g. 'First Name', 'Last Name' for a person)
- Each edge has a set of properties as well
- Each property has a name (string), a value type (e.g. string / integer), and a value (e.g. "First_Name": string = "Lior")
- Usually each vertex has a type (e.g. Person, Phone, Vehicle), but in a schema-free graph - the type is just another property
- Edges have types as well (e.g. 'owns', 'call'). Again - without schema - the type is just another property

More about property graphs can be found [here](http://tinkerpop.apache.org/docs/current/reference/) and [here](https://neo4j.com/developer/graph-database/).

A **property graph's schema** is defined by

* A set of entity types.
  Entities with the same basic properties are grouped (typed) into an entity type (e.g. Person, Vehicle, Product).
  For each entity type: 
  * A name 
  * A set of properties. For each property: name (key) and value type
* A set of relationship types. For each relationship type:
  * A name
  * A set of properties. For each property: name (key) and value type
  * A set of pairs of entity types for which the relationship type holds (e.g. owns(Person,Phone); owns(Person,Vehicle) )

There is no standard way to define property graph schemas. Implementations may vary in many aspects: the properties' data types (basic types, categorical, multivalued, composite, nested) and supported operators, the relationship types supported directionality (unidirectional, bidirectional, mixed), constraints (mandatory attributes, relationships cardinality, etc.), entity type hierarchies, relationship type hierarchies, etc.

A **schema-based property graph** is a property graph that conforms to a given schema.

**Why do we need a schema?**

Schema-free property graphs do not define nor enforce entity-types nor relationships-types; each vertex and each edge may contain attributes with any name and any value type. Without schema we can’t enforce integrity. Without such integrity we can't define formal building-blocks for representing patterns.

In order to ask and answer queries such as *“Any person who owns a red vehicle”* we first need to:

* Define 'Person' and 'Vehicle' entity types
* Ensure that each Person and Vehicle entities are of the relevant entity types
* Define 'owns' relationship type
* Defines that the 'owns' relationship type holds between Person and Vehicle
* Define a 'Color' property for the Vehicle entity type
* Define that Color holds a string, or better, define a categorical data type

## Patterns and Pattern Languages

A pattern defines a structure of a sub-graph in a schema-based property graph. Here is an example:

*“Any person who owns a blue car, his age is between 40 and 50, his cell-phone number ends with “156”, and has a brother that called 5 or more phones belonging to employees of company X in the last month"*

A pattern can be viewed as a query that can be executed against a graph database (similar to SELECT statement in SQL). The answer to such query is the union of the elements (entities and relationships) of the set of all the sub-graphs that conforms to the pattern.

A pattern language defines a syntax for expressing patterns.

Pattern languages differs in the following aspects:

* Genericity - generic (e.g. schema-driven) vs. domain-specific
* Representation - textual (structured / controlled natural) or visual (AKA graphical, diagrammatic)
* Expressivity - The richness of the patterns that can be expressed, The expressivity is always limited (in a Gödelian sense)
* Simplicity - How simple it is to construct new patterns and to understand existing patterns
* Efficiency - to parse patterns and to execute pattern queries

## The V1 Pattern Language

Many potential users won't use textual query languages. The learning curve may be too sharp for someone with little prior experience in programming. Even users that use textual query languages may spend a lot of time on the technicalities. 

Users want to be able to pose complex queries in a manner that is coherent with the way they think. They want to do it with minimal technical training and minimal effort. The ability to express patterns in a way that is aligned with their mental processes is crucial to the flow of their work, and to the value of the insights they can deduce.

Visual query languages are very attractive. They have the potential to be more 'user friendly' in the sense that patterns may be constructed and understand much more quickly, with much less mental effort. A long-standing challenge is to design a visual query language that is generic, simple, and has rich expressivity. This is what V1 aims to be. 

V1 is a rich simple generic visual pattern language for schema-based property graphs. It is named after the [primary visual cortex in our brain](https://en.wikipedia.org/wiki/Visual_cortex), which is also known as Visual area one (V1).

## V1 Basics

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB01.png)

Red, blue and yellow rectangles represent entities. **A yellow rectangle** represents a concrete entity: a specific person, a specific car, etc. The text inside a yellow rectangle denotes the entity type, and the value of the entity's leading properties (e.g. first name and last name of a person). **A blue rectangle** represents an entity of a given type. A blue 'Person' for example represents any person. **A red rectangle** represents an entity of any type (unless type constraints are defined). For every blue rectangle and for every red rectangle, the query processor will look for **assignments** of concrete entities that match the pattern.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB02.png)

**A green rectangle** is connected to an entity or to a relationship, and represents an entity's / relationship's property. It contains the property's name, and may contain constraints on the value of that property, expressed by an equation (e.g. 'age > 30'). A green rectangle may also contain a **property tag**, depicted by a numeric index wrapped in **purple curly brackets**. A property tag serves as a placeholder for the property's value in a given assignment, and can be used to define constraints on the value of other properties (e.g. age > {1}, where {1} is defined as the age of another entity). More on this later.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB04.png)

Any pattern starts with a small diamond, and is read from left to right. A pair of entities can be connected with a horizontal **black arrow** - denoting a **directional relationship**, with a horizontal **black line** - denoting **non-directional relationship** (if directional relationships are supported), or with a horizontal **red line** - denoting a **path** (more on paths - later). Each relationship has a label which denotes the relationship's type.

Patterns have a tree-like structure (as opposed to graph-like structure).

Here are some basic patterns:

_**Q1:** Any phone owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q001.png)

_**Q2:** Any phone that received at least one call from a phone owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q002.png)

_**Q3:** Any person who owns a phone, and his first name is Lior **(v1)**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-1.png)


## Quantifiers

Quantifiers are used when several conditions need to be checked. Here is a simple example:

_**Q3:** Any person who owns a phone, and his first name is Lior **(v2)**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-2.png)

In its simplest usage, a quantifier is connected to an entity on its left side, and has two or more branches on its right side. We'll call the left side of the quantifier 'the left component', and anything that follows a branch, up to the end of the branch, 'a right component'.

Each branch may start with:

* A relationship / a path (optionally preceded by an 'X', crossed arrow, 'O' or 'L')
* A green rectangle (entity's property value constraints / tag)
* A quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB03.png)

The most useful quantifiers are:

* **All** (denoted '&') - For each assignment to the left component - there should be at least one assignment to any right component
* **At least one** (denoted '&#124;') - For each assignment to the left component - there should be at least one assignment to at least one right component
* **Not all** (denoted by a crossed '&') - For each assignment to the left component - there should be no assignment to at least one right component
* **None** (denoted '0') - For each assignment to the left component - there should be at no assignment to any right component

Additional quantifiers:

* _n_ - For each assignment to the left component - there should be at least one assignment to exactly _n_ right components
* _> n_ - For each assignment to the left component - there should be at least one assignment to more than _n_ right components
* _≥ n_ - For each assignment to the left component - there should be at least one assignment to _n_ or more right components
* _< n_ - For each assignment to the left component - there should be at least one assignment to less than _n_ right components
* _≤ n_ - For each assignment to the left component - there should be at least one assignment to _n_ or less right components
* _n1..n2_ - For each assignment to the left component - there should be at least one assignment to _n1_ up to _n2_ right components
* _≠ n_ - For each assignment to the left component - there should be at least one assignment to any number but _n_ right components
* _∉ n1..n2_ - For each assignment to the left component - there should be at least one assignment to less than _n1_ or to more than _n2_ right components

**Note:** If a branch contains only a property tag (without constraints) - it is evaluated as a valid assignment.

Here are some more examples:

_**Q8:** Any person born prior to 1970 and died, or that his father born no later than 1/1/1950_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q008.png)

_**Q11:** Any current employee of IBM that, since 2011 or later, knows someone that left Oracle or Microsoft on or after June 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q011.png)

Quantifiers can also be used for relationships. In such cases, each branch may start with:

* A green rectangle (relationship's property value constraints / tag)
* An aggregate condition / an aggregation tag
* A quantifier

_**Q10:** Any person whose first name is Lior, who owns some phone B which called a phone C that belongs to an offspring of James Smith and called a phone that belongs either to John Price or to George Davis. At least one call from B to C was longer than 100 seconds, and took place in or after 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q010.png)

## Entity Tag

There a letter in the top-left corner of every red, blue, or yellow rectangle. This letter is called an **'entity tag'**.

Entity tags serve two purposes. First, when a pattern is used as a query, entity tags should appear in the query's answer as well. Any concrete entity in the answer is tagged with the same tag as the query's entity it was assigned to. This helps the user understand why any given entity is part of the answer. Second, entity tags are used to express _identicality constraints_ and _nonidenticality constraints_.

**Identicality constraint** is used when two entities in the pattern must have identical assignment. Here is an example:

_**Q4:** Any person whose phone received a call from a phone owned by (at least one) of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q004.png)

Entity tag 'B' is used enforce identical assignment to two entities. The 'B' tags are green - this is a visual indication that these tags are used to enforce identicality.

Here is another example:

_**Q9:** Any phones pair (A, B) where A called B both in 1980 and in 1984_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q009.png)

**Nonidenticality Constraint** is used when two entities in the pattern must have nonidentical assignments. Here is an example:

_**Q5:** Any person whose phone received a call from a phone owned by two of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q005.png)

Without enforcing nonidenticality, the same parent can be assigned to both C and E. The 'C' and '≠C' tags are red - this is a visual indication that these tags are used to enforce nonidenticality.

Here are two more examples:

_**Q6:** Any person whose phone received calls from two phones – one owned by one of his parents, the other owned by another parent (note that none, one or both phones may be owner by both parents)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q006.png)

_**Q7:** Any person whose phone was either (i) called from a phone owned by two of his parents, or (ii) from two phones – one owned by one of his parents and the other owned by his other parent_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q007.png)

Entity tags are also used in Q24 and Q25 below.

## Component Nonexistance

Sometimes we are looking for things that are not in the graph (e.g. _any person whose first name is Lior, and doesn't own a red vehicle_). Such patterns are composed of a left component and a right component. The left component (_person whose first name is Lior_) should have assignments, while for any such assignment - the right component shouldn't have any assignment (_a relationship between an assignment of the left component to a red vehicle_). Needless to say, the answer to such queries contains only assignments to the left component.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB05.png)

In the example above, there are actually two possibilities: there may be no red vehicles at all, or there may be red vehicles, but none of them is owned by a person whose first name is Lior. Usually it doesn't matter which is the case since any person whose name is Lior is a valid assignment to the pattern. This is where a **pink 'X' box** can be used (see the examples below). However, in certain situations, we may want to get the valid assignments to left component only if a valid assignment to the right component exist, except to the direct relationship between such components. In such cases, a **pink crossed-arrow box** can be used (more on this later).

Here are some examples:

_**Q12:** Any person who doesn't own a vehicle_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q012.png)

_**Q13:** Any vehicle that is not owned by a person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q013.png)

_**Q14:** Get vehicle 34-234-99, if not owned by a person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q014.png)

_**Q15:** Get Lior Kogan, if he doesn't own a vehicle_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q015.png)

_**Q16:** Any person who doesn't own vehicle 34-234-99_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q016.png)

_**Q17:** Any vehicle that is not owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q017.png)

_**Q18:** Get Lior Kogan, if he doesn't own vehicle 34-234-99_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q018.png)

_**Q19:** Get Vehicle 34-234-99, if it is not owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q019.png)

_**Q20:** Any vehicle that is not owned by James Smith nor by John Price (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-1.png)

This pattern can also be represented using the '0' quantifier:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-2.png)

_**Q21:** Any vehicle that is not owned by both James Smith and John Price (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-1.png)

This pattern can also be represented using the 'not all' quantifier, but notice that there is a slight difference: in the version above if either James Smith or John Price owns the vehicle - the owner won't be a part of the answer, while in the version below - the owner will be a part of the answer.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-2.png)

_**Q22:** Any vehicle that is not owned by a person who owns a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q022.png)

Note that the left component is _'vehicle'_ while the right component is _'owned by a person who owns a phone'_. The right component is anything that follows the pink 'X' box - up to the end of the branch.

_**Q23:** Any vehicle that is not owned by a person who doesn't own a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q023.png)

_**Q24:** Any person who has (at least) two parents and owns a phone that was called from a phone that is not owned by either of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q024.png)

_**Q25:** Any of my friend's friends that isn't my friend (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-2.png)

_**Q26:** Any book that is liked by people who like some book that I like, but is not liked by me (four versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-3.png)

## Starting a Pattern with a Quantifier 

A pattern may start with a quantifier. Any branch of such quantifier must start with an entity (red / blue / yellow).

Here is a fourth way to represent Q26:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-4.png)

## E-combiner

An E-combiner combines two or more branches (not necessarily of the same quantifier). On its left side are entities and on its right side either:

* A relationship / a path (optionally preceded by an 'X', crossed arrow, 'O' or 'L')
* A green rectangle (entity's property value constraints / tag)
* A quantifier

The right side of an E-combiner is a direct continuation of each of the combined branches. An E-combiner is simply a syntactic sugar that can be used to save duplication when several branches terminates identically.

The relationship / property types on an E-combiner's right side must match all the entity types on its left side. 

Here are some examples:

_**Q27:** Any person with a Chinese citizenship who owns a vehicle and a phone of the same origin; Any company registered in Japan that owns a vehicle and a phone of the same origin_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q027.png)

_**Q28:** Any Chinese citizen who owns a red vehicle; Any person who doesn't know someone who owns a red vehicle_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q028.png)

_**Q35:** Any person who either (i) knows a Chinese citizen who owns a red vehicle (ii) know a person who doesn't know someone who owns a red vehicle (iii) know someone who doesn't own a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q035.png)

An entity tag can't be used both on the left side and on the right side of an E-combiner (to prevent expressing identicality / nonidenticality constraints that are valid only in some branches).

## R-Combiner

An R-combiner combines two or more branches of the same quantifier. On its left side are relationships, and on its right side - an entity.

The entity type on an R-combiner's right side must match all the relationship types on its left side.

Here are some examples:

_**Q29:** Any phone that called or SMSed some phone (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q029-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q029-2.png)

_**Q30:** Any phones pair (A, B) where A both called and SMSed B (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q030-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q030-2.png)

Note that the concrete entity on the right side of the R-combiner has the same assignment for all the branches.

_**Q31:** Any phones pair (A, B) where A called B, A SMSed B, B called A, and B SMSed A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q031.png)

_**Q32:** Any phones pair (A, B) where A SMSed B, and A SMSed some phone that SMSed B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q032.png)

_**Q33:** Any phone A that called some phone B, called some phone that called B, and SMSed some phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q033.png)

_**Q34:** Any phone A that called some phone B, called some phone that called B, SMSed some phone D and SMSed some phoned that SMSed D (B and D may be the same phone or different phones)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q034.png)

## Red Entities

Sometimes the same relationship type can hold between different pairs of entity types (e.g. owns(Person, Vehicle); owns(Company, Vehicle) ). We need red entities to express patterns such as "_any red car and its owners_", when the owner can be either a person or a company.
 
 In it simplest form, a red rectangle represents an entity of any type.

_**Q36:** Any person who owns something_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q036.png)

The entity type is implicitly constrained because of the relationship type. In Q36, the entity type is constrained to things that a person can own.

_**Q49:** Any 3 phones with a cyclic call pattern, and their owners_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q049.png)

Similarly, the entity type of all owners is implicitly constrained to things that can own a phone.

In addition to the implicit type constraints, explicit type constraints can be enforced by defining a set of allowed types or a set of disallowed types. Here are two examples:

_**Q37:** Any person who owns a vehicle or a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q037.png)

_**Q38:** Any person who owns something which is not a vehicle nor a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q038.png)

Even when a red entity is on either side of an 'X' - the entity's type is constrained by relationship types. Here are some examples:

_**Q39:** Anything that can own something, but doesn't_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q039.png)

_**Q40:** Any phone that has no owner_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q040.png)

_**Q41:** Anything that can be owned, and is not owned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q041.png)

_**Q42:** Anything that can own something, but owns nothing_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q042.png)

_**Q43:** Any phone that all of its owners (if any) are people_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q043.png)

## Paths

A path connects two entities - similar to a relationship. However, while a relationship is a direct connection between two entities, a path is an indirect connection : a path is a sequence of relationships and entities between the two entities.

Each path has a length. The length of the path is equal to the number of entities along the path. Relationships are actually paths with length 0. The number of relationships along a path is always larger by one than the number of entities along the path.

A path is depicted by a red line between two entities. Above the line there must be a limitation to the path length, which is  expressed using one of the following operators: _< n, ≤ n, in [n1..n2], in [n1, n2, ...]_, where all numbers a positive integers.

An assignment to a path consists of all the relationships and entities along the path.

Here are two examples:

_**Q53:** Any person within graph distance ≤ 4 from James Smith_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q053.png)

_**Q55:** Anything within graph distance ≤ 3 from James Smith, John Price and George Davis_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q055.png)

Optional constraints may be defined for the entities and relationships along the path.

Optional relationship constraints are listed in red curly brackets above the path's line. The brackets may list:

- Allowed relationship types - e.g. {SMS, call}
- Constraints on the number of relationships of each allowed type - e.g. {call < 2, SMS = 2}
- Constraints on the number of relationships of each allowed type in each direction - e.g. {→ call = 2, ← call = 1}
- Disallowed relationships types constraints - e.g. {call = 0}

Optional entity constraints are listed in red curly brackets below the path's line. The brackets may list:

- Allowed entity types - e.g. {phone}
- Constraint on the number of entities of each allowed type - e.g. {phone < 1}
- Disallowed entity types - e.g. {phone = 0}

Here are some examples:

_**Q44:** Any path with length ≤ 4 between these two phones, which is composed only of 'call' relationships_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q044.png)

_**Q45:** Any path with length ≤ 4 between these two phones, which is composed only of 'call' and 'SMS' relationships_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q045.png)

_**Q54:** Any person within graph distance ≤ 3 from James Smith, John Price and George Davis_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q054.png)

_**Q46:** Any path with length ≤ 4 between a phone owned by James Smith to a phone owned by John Price, which is composed of up to 2 'call' relationships, and only of 'phone' entities_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q046.png)

## Shortest Paths

Instead of specifying a constraint on the path length - paths can be limited to the shortest ones that subject to the entities/relationships constraints. If, for example, the shortest path length that subject to the constraints is 3 - all paths with length 3 are valid assignments.

Here are two examples:

_**Q47:** All shortest paths between these two phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q047.png)

_**Q48:** All shortest paths between these two phones, which are not composed of 'call' relationships nor 'phone' entities_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q048.png)

## Path Segments

An alternative to constraints on the entities and relationships along the path, are constraints on **path segment types**. A path segment type is a valid chain of yellow / blue / red entities and relationship types that starts and ends with an entity. There is an 'overlap' between successive segment types:

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

In this example, the path must contain James Smith. Any other path segments are allowed

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q058.png)

## Aggregate Condition #1

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG01.png)

todo

_**Q59:** Any person having more than 2 parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q059.png)

_**Q60:** Any phone that was called from exactly 5 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q060.png)

_**Q61:** Anything that owns more than 2 things_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q061.png)

_**Q62:** Any person who is within graph distance ≤ 4 from more than 5 people_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q062.png)

_**Q136:** Any phone A that called (phones that called phones B). The cumulative number of distinct Bs (per A) is greater than 100  (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q136-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q136-2.png)

_**Q81:** Any phone that didn't call (0 callees)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q081.png)

_**Q82:** Any phone that wasn't called (0 callers)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q082.png)

_**Q85:** Any phone that called at least 10 phones, and was called from at least 10 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q085.png)

_**Q101:** Any person who owns at least 10 red vehicles_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q101.png)

_**Q102:** Any phone that was called from at least 2 phones; each of these was called from at least one phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q102.png)

_**Q166:** Any **phone** that more than 5 ABC employees own a phone that called **it**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q166.png)

## Aggregate Condition #2

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG04.png)

todo

_**Q71:** Any phone that made more than 10 calls (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q071.png)

_**Q72:** Any phone that was called exactly 10 times (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q072.png)

_**Q73:** Any phone that made no more than 10 calls (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q073.png)

_**Q74:** Any phone that the number of times it was called (cumulatively) is not 10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q074.png)

_**Q75:** Any phones pair (A, B) where A was called between 8 to 10 times from B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q075.png)

_**Q76:** Any phone that called between 8 to 10 times to 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q076.png)

_**Q79:** Any person with more than 5 paths (cumulatively) with length ≤ 4 to some person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q079.png)

_**Q83:** Any phone that didn't call (0 outgoing calls)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q083.png)

_**Q84:** Any phone with no paths with length ≤ 3 to other phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q084.png)

_**Q104:** Any person who owned red vehicles at least 10 times (same or different vehicles)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q104.png)

_**Q105:** Any phone A that was called 2 times (cumulatively) from phones that each was called from at least one phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q105.png)

## Aggregate Condition #3

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG06.png)

todo

_**Q86:** Any phones pair (A, B) where the calls from A to B have a cumulative length of more than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q086.png)

_**Q87:** Any phone that was called at least once, where the cumulative calls duration is less than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q087.png)

_**Q88:** Any phone pair (A,B) where A called B at least once, but the cumulative calls duration is 0 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q088.png)

_**Q89:** Any phone that its outgoing calls have more than 3 different durations_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q089.png)

## Aggregate Condition #4

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG02.png)

todo

_**Q63:** Any ABC employee who more than 5 (ABC employees don't know)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q063.png)

_**Q65:** Any person who doesn't own more than 2 (things that his spouse owns)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q065.png)

_**Q64:** Any **phone** that less than 5 (phones owned by ABC employees) didn't call **it**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q064.png)

_**Q165:** Any **phone** that less than 5 (ABC employees own a phone that didn't call **it**)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q165.png)

_**Q66:** Any person from whom more than 5 people are not within graph distance ≤ 6_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q066.png)

## Min/Max Aggregation #1

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG03.png)

todo

_**Q67:** The 3 people with the maximal number of parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q067.png)

_**Q68:** The 2 phones that were called from the largest number of phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q068.png)

_**Q69:** The 2 things that own the largest number of things_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q069.png)

_**Q70:** The 5 people, that the number of people within graph distance ≤ 4 from them, is the smallest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q070.png)

## Min/Max Aggregation #2

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG05.png)

todo

_**Q77:** The 5 phone pairs (A, B) where the number of calls from B to A is the largest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q077.png)

_**Q78:** The 4 phones that called the largest number of times to 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q078.png)

_**Q80:** The 3 person pairs with the largest number of paths with length ≤ 4 between them_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q080.png)

## Min/Max Aggregation #3

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG07.png)

todo

_**Q160:** For any phones pair (A, B): The 4 longest calls from A to B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q160.png)

_**Q161:** For any phone: the 4 longest outgoing calls_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q161.png)

## Min/Max Aggregation #4

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG08.png)

todo

_**Q90:** The 4 phone pairs (A, B) with the maximal cumulative calls duration from A to B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q090.png)

_**Q91:** The 4 phones with the longest (shortest incoming call)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q091.png)

_**Q92:** The 4 phones with the longest (outgoing calls average duration)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q092.png)

## Aggregation Chain

todo

_**Q93:** First, pass only (> 10 minutes calls). Then, pass only phones A that called (> 10 minutes calls) to at least 3 phones. From these, pass only phone pairs (A, B) where A made at least 5 (> 10 minutes calls) to B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q093.png)

_**Q94:** First, pass only (> 10 minutes calls). Then, pass phone pairs (A, B) where A made at least 5 (> 10 minutes calls) to B. From these A's, pass only those that called (> 10 minutes calls) to at least 3 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q094.png)

_**Q162:** Any phones pair (A, B) where the second shortest call from A to B is longer than 60 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q162.png)

_**Q163:** Any phone that the average duration of its 10 shortest outgoing calls is greater than 60 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q163.png)

_**Q95:** Any phone that received at least 5 (> 10 minutes calls) with a total duration of > 100 minutes from 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q095.png)

_**Q96:** Any phone that received at least 10 (< 10 minutes calls) after 1/1/2010 from 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q096.png)

_**Q97:** Any phone that called at least 3 phones. For each callee, more than 10 calls, or calls with total duration of > 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q097.png)

_**Q98:** First, pass phones A that called at least 3 phones. Then, pass phone pairs (A,B) where A called B more than 10 calls, or calls with a total duration of > 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q098.png)

_**Q99:** Any phone to which 052-333-4444 called more than 10 calls of < 10 minutes, and no calls of ≥ 10 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q099.png)

_**Q100:** Any phone to which 052-333-444 called with more then 10 (< 10 minutes calls), more than 10 calls after 1/1/2010, more than 15 (< 10 minutes calls) after 1/1/2010, and more than 100 calls_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q100.png)

## Aggregation Sequence

todo

_**Q103:** Any phone A that called at least 3 phones that each of them was called from at least 4 phones other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q103.png)

_**Q106:** Any phone A that made at least 3 calls (cumulatively) to phones that each of them received at least 4 calls (cumulatively) from phones other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q106.png)

_**Q107:** Any phone that (the number of phones owned by 5 people each that called it) is 5, and that the number of calls received from those phones (cumulatively) is not 5_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q107.png)

## Property Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB06.png)

todo

_**Q108:** Any person who has the same birth-date as Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q108.png)

_**Q109:** Any person who his parent owned a vehicle and a phone prior to his birth_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q109.png)

_**Q110:** Any 3 bank accounts with a cyclic transfers of more than $10000 in chronological order, and their owners_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q110.png)

_**Q111:** Any person who doesn't know someone with a birth date similar to his_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q111.png)

_**Q112:** Any person who owned a vehicle and a phone in the same time frames_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q112.png)

## Aggregations and Property Tag

todo

_**Q113:** Any person who knows at least 5 people with a birth data similar to his_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q113.png)

_**Q114:** Any person who owns more than 5 vehicles with the same color_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q114.png)

_**Q115:** Any person who owns more than 5 vehicles since the same date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q115.png)

## Aggregate Condition on Property Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG09.png)

todo

_**Q116:** Any person who owns vehicles with no more than 3 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q116.png)

_**Q117:** Any person for whom the average model year of his owned vehicles is greater than 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q117.png)

_**Q134:** Any person for whom the number of distinct colors of all vehicles owned by people he knows - is not more than 3_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q134.png)

_**Q135:** Any person for whom the average model year of all vehicles owned by people he knows - is at least 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q135.png)

_**Q137:** Any phone A that called phones B that made calls. The cumulative duration of all the calls that all these B phones made is more than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q137.png)

## Entity Type Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB08.png)

A red rectangle may contain an **entity type tag**, depicted by a numeric index wrapped in **purple triangular brackets**. An entity type tag serves as a placeholder for the entity type in a given assignment, and can be used to define constraints on the type of other red entities.

Here are some examples:

_**Q50:** Any person who owns (at least) two things of the same type_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q050.png)

_**Q51:** Any person who owns (at least) two things of different types_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q051.png)

_**Q52:** Any person who owns (at least) two things of different types, both are not vehicles_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q052.png)

## Aggregate Condition on Entity Type Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG17.png)

todo

_**Q167:** Any person who owns things of at least 3 types_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q167.png)

## Aggregation Tag #1

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG11.png)

todo

_**Q125:** Any phone that the number of phones it called is greater than the number of phones that called it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q125.png)

_**Q126:** Any phone that the number of phones it called is greater than the number of phones it didn't called_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q126.png)

_**Q127:** Any phone that made more calls to phones owned by IBM employees, than to phones owned by Oracle employees_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q127.png)

## Aggregation Tag #2

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG16.png)

todo

_**Q139:** Any person who owns vehicles with the same number of colors as the number of colors of the vehicles owned by his parents cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q139.png)

## Aggregate Condition on Aggregation Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG18.png)

todo

_**Q169:** Any person who (each of his offsprings who owns at least 3 vehicles - owns vehicles with at least 3 colors)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q169.png)

_**Q129:** Any person who (each of his offsprings who owns at least one vehicle - owns a different number of vehicles)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q129.png)

_**Q164:** Any phone that the cumulative number of outgoing calls of the phones it called - is equal to the cumulative number of outgoing SMSes of the phones it SMSed_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q164.png)

## Latent and Optional Components

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB07.png)

Anything on the right of an **'L'** is **latent**: It won't be part of the answer, though it still must have an assignment.

- 'L' may appear just before a relationship, a path, or a quantifier (excluding a quantifier at the start of the pattern)
- 'X' may not appear directly after an 'L'
- After an 'X', after a '0' quantifier, or after an aggregate condition such as "relationships = 0" - 'L' is meaningless anyway

Here are two examples:

_**Q142:** Any person who owns a red vehicle, and has a parent who owns a vehicle. The parent and his vehicle are not part of the answer_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q142.png)

_**Q143:** Any person who owns a red vehicle, and has a parent that doesn't own a vehicle. The parent is not part of the answer_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q143.png)

Anything on the right of an **'O'** is **optional**: if it has a valid assignment - it will be part of the answer. Otherwise - it won't.

- 'O' may appear just before a relationship, a path, or a quantifier (excluding a quantifier at the start of the pattern)
- 'O' may not appear (directly or indirectly) after an 'L'
- 'X' may not appear directly after an 'O'
- After an 'X', after a '0' quantifier, or after an aggregate condition such as "relationships = 0" - 'O' is meaningless anyway

Here are some examples:

_**Q144:** Any person who owns a red vehicle. If he has a parent who owns a vehicle - the parent and his vehicle will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q144.png)

_**Q145:** Any person who owns a red vehicle. If he has a parent that doesn't own a vehicle - the parent will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q145.png)

_**Q146:** Any person who owns a red vehicle. If he has a parent - the parent will be part of the answer as well. If his parent owns a vehicle - this vehicle will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q146.png)

_**Q147:** Any person. If he owns both a vehicle and a phone - they will be a part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q147.png)

_**Q148:** Any person who owns both a vehicle and a phone. If he has a parent - the parent will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q148.png)

_**Q149:** Any person. If he owns a vehicle - it will be a part of the answer as well. If he owns a phone - it will be a part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q149.png)

_**Q150:** Any person who owns a vehicle or a phone. If he has a parent - the parent will be part of the answer as well_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q150.png)

The following examples combines 'L' and 'O' with aggregate conditions:

_**Q151:** Any person who owns more than 10 vehicles, at least one is Chinese. Only the Chinese vehicles will be returned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q151.png)

_**Q152:** Any person who owns more than 10 vehicles. Only the Chinese vehicles will be returned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q152.png)

## Split

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB09.png)

todo

_**Q153:** Any phone that in at least 10 days called no more than 5 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q153.png)

_**Q154:** Any phone that in at least 4 years: in at least 11 days called no more than 5 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q154.png)

_**Q155:** Any phone that in at least 11 days called more than 100 minutes cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q155.png)

_**Q156:** Any phone that called more than 1000 minutes cumulatively - in the days it called more than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q156.png)

_**Q157:** Any person who owns at most 3 vehicles with the same color - for more than 5 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q157.png)

_**Q158:** Any phone that in at least 10 days - the number of phones it called is greater than the number of phones that called it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q158.png)

## Split Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB11.png)

todo

_**Q159:** Any phones for which there are more days where (the number of phones it called is greater than the number of phones that called it) than days where (the number of phones that called it is greater than the number of phones it called)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q159.png)

## Min/Max Aggregation on Tags

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG10.png)

todo

_**Q118:** Any person and his 3 eldest offsprings_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q118.png)

_**Q119:** Any person and his 3 youngest sons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q119.png)

_**Q120:** Any person for whom the sum of the heights of his 3 eldest offsprings is less than his own height_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q120.png)

_**Q140:** Any person who his 3 eldest sons cumulatively own vehicles with 3 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q140.png)

_**Q141:** Any person who his 3 eldest sons cumulatively own vehicles with the same number of colors as his 3 younger daughters cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q141.png)

_**Q128:** Any person and his 3 offspring that own vehicles with the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q128.png)

# Prefix Min/Max Aggregation on Tags

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG12.png)

todo

_**Q130:** The 4 eldest people_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q130.png)

_**Q131:** The 4 eldest males_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q131.png)

_**Q132:** The 4 people who own vehicles with the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q132.png)

_**Q133:** The 4 people that the average model year of their vehicles is maximal_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q133.png)

_**Q138:** The 4 people that the people they know cumulatively own vehicles with the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q138.png)

_**Q168:** The 3 people that the number of types of things they own is maximal_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q168.png)

## Aggregate Condition before an 'at least one' Quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG15-1.png)

todo

_**Q124:** Any phone that either (called a phone) or (SMSed a phone that SMSed a phone) - at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q124.png)

_**Q123:** Any phone that either (was called) or (made a call) - at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q123.png)

## Aggregate Condition before Quantifier and R-Combiner

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/AG15-2.png)

todo

_**Q121:** Any phone that (was called from) or (made a call to) at least 10 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q121.png)

_**Q122:** Any phone that SMSed phone B, and SMSed a phone that SMSed B - for at least 10 different B's_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q122.png)
