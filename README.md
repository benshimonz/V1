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
  An entity is an objects or ‘thing’ in our mini-world, with an independent existence and which is distinguishable from other objects (e.g. a person, a horse, a dragon)
- Graph edges represent relationships between pairs of entities (e.g. 'owns', 'friend of'). Usually all edges are directional.
- Each vertex has a set of descriptive features called properties (AKA attributes) (e.g. 'First Name', 'Last Name' for a person)
- Each edge has a set of properties as well
- Each property has a name (string), a value type (e.g. string / integer), and a value (e.g. "First_Name": string = "Brandon")
- Usually each vertex has a type (e.g. Person, Horse, Dragon), but in a schema-free graph - the type is just another property
- Edges have types as well (e.g. 'owns', 'member of'). Again - without schema - the type is just another property

More about property graphs can be found [here](http://tinkerpop.apache.org/docs/current/reference/) and [here](https://neo4j.com/developer/graph-database/).

A **property graph's schema** is defined by

* A set of entity types.
  Entities with the same basic properties are grouped (typed) into an entity type (e.g. Person, Horse, Dragon).
  For each entity type: 
  * A name 
  * A set of properties. For each property: name (key) and value type
* A set of relationship types. For each relationship type:
  * A name
  * A set of properties. For each property: name (key) and value type
  * A set of pairs of entity types for which the relationship type holds (e.g. owns(Person,Horse); owns(Person,Dragon) )

There is no standard way to define property graph schemas. Implementations may vary in many aspects: the properties' data types (basic types, categorical, multivalued, composite, nested) and supported operators, the relationship types supported directionality (unidirectional, bidirectional, mixed), constraints (mandatory properties, relationships cardinality, etc.), entity type hierarchies, relationship type hierarchies, etc.

A **schema-based property graph** is a property graph that conforms to a given schema.

**Why do we need a schema?**

Schema-free property graphs neither define nor enforce entity-types and relationships-types; each vertex and each edge may contain properties with any name and any value type. Without schema we can’t enforce integrity. Without such integrity we can't define formal building-blocks for representing patterns.

In order to ask and answer queries such as *“Any person who owns a white horse”* we first need to:

* Define 'Person' and 'Horse' entity types
* Ensure that each Person and Horse entities are of the correct entity types
* Define 'owns' relationship type
* Defines that the 'owns' relationship type holds between Person entity type and Horse entity type
* Define a 'Color' property for the Horse entity type
* Define that Color holds a string, or better, define a categorical data type

## Patterns and Pattern Languages

**A pattern** defines a set of **acceptable** connected property graphs (entities and relationships), defined over a given property graph's schema.

Here are two examples:

* *Any person who owns at least 5 white horses*

* *Any person whose birth date is between 1/1/970 and 1/1/980, owns a white Horse, owns a dragon that his name starts with 'M', and his  dragon froze at least 3 dragons belonging to members of the Masons guild in the last month*

**Pattern matching** is the process of deciding whether a given (sub)graph is acceptable to a given pattern. 

**Pattern finding** is the process of finding minimal subgraphs of a given property graph, which match a given pattern. Any minimal subgraph that matches the pattern is called **an assignment**. *Minimal* means that if a single entity or relationship is removed - it won't match the pattern anymore.

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

The subjects of Sarnor, Omber and all the other kingdoms of the four continents: Essos, Westeros, Sothoryos and Ulthos like their horses. There is one thing they love even more - that is their dragons. They own dragons of fire and ice. Like all well-behaved dargons, their dragons love to play. Dragons always play in couples. When playing, dragons often get furious, fire at each other (fire breath) and freeze each other (cold breath). Dragons usually freeze each other for periods of several minutes, but on occasion, if they are really raging, they can freeze each other for periods of to 2 hours or even more. The subjects of the 4 continents enjoy watching their dragons play. Fascinated by these magnificant creatures, they wrote thousands of books, which document any play, any fire breathing, and any ice breathing in the last 900 years (that's not so much in dragon-years). The kings of Sarnor and Omber regularly pose queries about their history. Quite often, it takes the royal historians and royal analysts several days to come up with answers, during which the kings are quite nervous. Lately, the king of Sarnor posed a very complex query, and after waiting for results for more than two moons, he ordered the chief analyst to be executed. He then summoned his chief mechanics, and ordered them to develop an apparatus which they can use to pose queries about their history and quickly get results. 

The engineers started their work by 

* Collecting all queries that their master posed in the last few years
* Constructing a schema (entity types, relationship types - and their properties) using which these queries can be expressed

The schema is composed of the following entity types:

* Person : first name, last name, gender, birth date, death date, height
* Dragon : name
* Horse  : name, color, weight
* Guild  : name
* Kingdom: name

and of the following relationship types:

* owns         (Person , Horse  ): since, till
* owns         (Person , Dragon ): since, till
* fires at     (Dragon , Dragon ): time
* freezes      (Dragon , Dragon ): time, duration
* offspring    (Person , Person )
* member of    (Person , Guild  ): since, till
* subject of   (Person , Kingdom)
* registered in(Guild  , Kingdom)
* originated in(Horse  , Kingdom)
* originated in(Dragon , Kingdom)

This schema and the queries will serve us to to demonstrate the power of the V1 language.

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

Any pattern is written from left to right. It starts with (a small black diamond), continues with a (yello, blue or red rectangle), and continues with zero or more ((black arrow, black line, or red line) followed by a (yellow, blue, or red rectangle)).

**Semantics**

Yellow, blue and red rectangles represent entities. **A yellow rectangle** represents a concrete entity: a specific person, a specific horse, etc. The text inside a yellow rectangle denotes the entity type, and the value of the entity's leading properties (e.g. first name and last name of a person). **A blue rectangle** represents an entity of a given type. A blue 'Person' for example represents any person. **A red rectangle** represents an entity of any type (unless type constraints are defined - see later).

A pair of entities can be connected with:

* A horizontal **black arrow** - representing a **directional relationship**,
* A horizontal **black line** - representing either a **non-directional relationship** or a directional relationship where the direction doesn't matter, or
* A horizontal **red line** - representing a **path** (explained later)

Each relationship has a label which denotes the relationship's type.

Here are some basic patterns:

_**Q1:** Any dragon owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q001.png)

_**Q2:** Any dragon that was frozen at least one time by a dragon owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q002.png)

_**Q184:** Any dragon that froze or was frozen at least one time by a dragon owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q184.png)

The freeze's direction does not matter. Therefore - a **non-directional relationship** is used in the pattern.

**Syntax**

The relationship type between two entities must be valid according to the schema. As said before - for each relationship type, the schema defines a set of pairs of entity types for which the relationship type holds (e.g. owns(Person,Horse); owns(Person,Dragon) ).

**Semantics**

For every blue rectangle, red rectangle, black arrow, and black line - the query processor will look in the property graph for assignments - sets of concrete entities and relationships that match the given pattern. A answer to a V1 query is the union of all the assignments.

## Properties

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB02.png)

**A green rectangle** is connected to an entity (red, blue, or yellow) or to a relationship, and represents an entity's / relationship's property. It contains:

* The property's name
* Optionally - a constraint on the value of that property, expressed by an equation (e.g. 'age > 30')
* Optionally - a **property tag**, depicted by an index wrapped in **purple curly brackets** (explained later).

A constraint, a property tag, or both - must be presented.

Constraints cannot be defined for yellow entities. 

Constraints can be defined for red entities only if entity type constraints (explained later) are defined, and all the allowed types have a property with the same name and the same type.

_**Q3:** Any person who owns a dragon, and his first name is Brandon **(v1)**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-1.png)

_**Q190:** Any person who owns a dragon since 1/1/1011 or since a later date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q190.png)

## Quantifiers #1

Vertical quantifiers (or simply 'quantifiers') are used when several conditions need to be checked. Here is a simple example:

_**Q3:** Any person who owns a dragon, and his first name is Brandon **(v2)**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-2.png)

A quantifier has one connection on its left side, and two or more branches on its right side. We'll call the left side of the quantifier 'the left component', and anything that follows a branch, up to the end of the branch, 'a right component'.

**The first way to use quantifiers:** The left component ends with an entity (yellow/aggregated/blue/logical/red), and each right component starts with:

* A relationship (optionally preceded by an 'X', an '↛', an 'O' or an 'L'),
* A path (optionally preceded by an 'X', an '↛', an 'O' or an 'L'),
* A green rectangle (entity's property value constraint / tag), or
* A quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB03.png)

4 quantifiers are defined here, and another 8 quantifiers are defined later on - after No-existence is explained.

We'll define _b_ sub-patterns, _P1..Pb_ (where _b_ denotes the number of branches), each composed of the left component, and one of the right components.

Given patterns _P1..Pb_, the quantifiers are defined as follows:

* **_All_** (denoted '&') - An assignment matches the pattern only if it matches _P1..Pb_ (though not necessarily minimal for each separately)
* **_Some_** (denoted '&#124;') - An assignment matches the pattern only if it matches at least one of _P1..Pb_ (though not necessarily minimal for each separately)
* **_> n_** - An assignment matches the pattern only if it matches more than _n_ of _P1..Pb_ (though not necessarily minimal for each separately). _n_ ∈ [0, _b-1_]
* **_≥ n_** - An assignment matches the pattern only if it matches _n_ or more of _P1..Pb_ (though not necessarily minimal for each separately). _n_ ∈ [1, _b_]

As said - an assignment is a _minimal_ subgraph: if a single entity or relationship is removed - it won't match the pattern anymore.

"_Only if_" denotes a necessary but not sufficient condition, since assignments must satisfy other conditions expressed by the pattern.

All (and only) matched sub-patterns are included in a query's answer.

Here is an example of **nested quantifiers**:

_**Q8:** Any person born prior to 970 and died, or that his father born no later than 1/1/950_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q008.png)

## Quantifiers #2

**A second way to use quantifiers:** The left component ends with a relationship, and each right component starts with either:

* An entity (yellow/aggregated/blue/logical/red)
* A quantifier

Here is an example:

_**Q11:** Any current member of the Masons guild, who since 1011 or later knows someone who left the Saddlers guild or the Blacksmiths guild - on June 1010 or later_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q011.png)

_**Q10:** Any person whose first name is Brandon, who owns some dragon B which froze a dragon C that (i) belongs to an offspring of Rogar Bolton and (ii) froze a dragon that belongs either to Robin Arryn or to Arrec Durrandon. B froze C at least once in or after 1010 for longer than 100 seconds_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q010.png)

When a relationship need to satisfy several conditions - green rectangles can be chained. Chaining is not valid for entities. Chaining is explained below (see _Horizontal Quantifiers_)

## Entity Tags

In the top-left corner of any red, blue, or yellow rectangle - there a letter. This letter is called an **'entity tag'**.

Entity tags serve two purposes. First, when a pattern is used as a query, entity tags should appear in the query's answer as well. Any concrete entity in the answer is tagged with the same tag as the query's entity it was assigned to. This helps the user understand why any given entity is part of the answer. Second, entity tags are used to express _identicality constraints_ and _nonidenticality constraints_.

**Identicality constraint** is used when two entities in the pattern must have identical assignment. Here is an example:

_**Q4:** Any person whose dragon was frozen by a dragon owned by (at least one) of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q004.png)

Entity tag 'B' is used enforce identical assignment to two entities. The 'B' tags are green - this is a visual indication that these tags are used to enforce identicality.

Here is another example:

_**Q9:** Any dragon pair (A, B) where A froze B both in 980 and in 984_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q009.png)

**Nonidenticality Constraint** is used when two entities in the pattern must have nonidentical assignments. Here is an example:

_**Q5:** Any person whose dragon was frozen by a dragon owned by two of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q005.png)

Without enforcing nonidenticality, the same parent can be assigned to both C and E. The 'C' and '≠C' tags are red - this is a visual indication that these tags are used to enforce nonidenticality.

Here are two more examples:

_**Q6:** Any person whose dragon was frozen by two dragons – one owned by one of his parents, the other owned by another parent (note that none, one or both dragons may be owner by both parents)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q006.png)

_**Q7:** Any person whose dragon was either (i) frozen by a dragon owned by two of his parents, or (ii) from two dragons – one owned by one of his parents and the other owned by his other parent_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q007.png)

_**Q24:** Any person who has (at least) two parents and owns a dragon that was frozen by a dragon that is not owned by either of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q024.png)

## No-existence and No-connection

Sometimes we are looking for things that are not in the graph (e.g. _any person whose first name is Brandon, and doesn't own a white horse_). Such patterns are composed of:

* A left component that ends with an entity (_any person whose first name is Brandon_) 
* A No-existence / a no-connection language element (_"doesn't"_) 
* A relationship / path (_own)
* A right component that starts with an entiy (_a white horse_)

An assignment is a multi-set of concrete elements only to the left component.

The example above covers two cases: (i) there may be no white horses at all, or (ii) there may be white horses, but none of them is owned by a person whose first name is Brandon. 

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB05.png)

Usually it doesn't matter which is the case, since _any person whose name is Brandon and doesn't own a white horse_ is a valid assignment to the pattern. This is where the **no-existence language element (depicted with a pink 'X' box)** can be used.

An assignment matches the pattern only if:

* It matches a pattern composed only of the left component (there is _a person whose first name is Brandon_) 
* It has no superset that matches a pattern composed of the left component, the relationship, and the right component (There is no assignment for _a person whose first name is Brandon and own's a white horse)_

An 'X' may not be used directly before a relationship or a path with an aggregation.

In certain situations, however, we need an assignment to match the pattern only if:

* It matches a pattern composed only of the left component (there is _a person whose first name is Brandon_) 
* It has no superset that matches a pattern composed of the left component, the relationship, and the right component (There is no assignment for _a person whose first name is Brandon and own's a white horse)_
* There is an assignment that matches a pattern composed only of the right component (there is _a whote horse_)

This is where the **no-connection language element (depicted with a pink '↛' box)** can be used. 

'↛''s are usually used directly before a relationship or a path with an aggregation.

Examples:

_**Q12:** Any person who doesn't own a horse_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q012.png)

_**Q13:** Any horse that is not owned by a person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q013.png)

_**Q14:** Get Sweetfoot, if not owned by a person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q014.png)

_**Q15:** Get Brandon Stark, if he doesn't own a horse_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q015.png)

_**Q16:** Any person who doesn't own Sweetfoot_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q016.png)

_**Q17:** Any horse that is not owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q017.png)

_**Q18:** Get Brandon Stark, if he doesn't own Sweetfoot_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q018.png)

_**Q19:** Get Sweetfoot, if it is not owned by Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q019.png)

An 'X' / an '↛' may also appear before a relationship that directly follows a quantifier's branch:

_**Q20:** Any horse that is neither owned by Rogar Bolton nor by Robin Arryn (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-1.png)

This pattern can also be represented using the '0' quantifier:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-2.png)

_**Q21:** Any horse that is not owned by both Rogar Bolton and Robin Arryn (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-1.png)

This pattern can also be represented using the 'not all' quantifier, but notice that there is a slight difference: in the version above if either Rogar Bolton or Robin Arryn owns the horse - the owner won't be a part of the answer, while in the version below - the owner will be a part of the answer.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-2.png)

_**Q22:** Any horse not owned by a person who owns a dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q022.png)

Note that the left component is _'horse'_ while the right component is _'owned by a person who owns a dragon'_. The right component is anything that follows the 'X' - up to the end of the branch.

That includes:

- Any horse that is not owned
- Any horse that non of his owners is a person (e.g. a horse owned by a guild)
- Any horse that each person who owns it - doesn't own a dragon

_**Q23:** Any horse not owned by a person who doesn't own a dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q023.png)

That includes:

- Any horse that is not owned
- Any horse that non of his owners is a person (e.g. a horse owned by a guild)
- Any horse that each person who owns it - also owns a dragon

_**Q25:** Any dragon, that a dragon that Balerion fired at - fired at, but Balerion didn't fire at (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-2.png)

_**Q26:** Any guild that people who are members of the same guild as Brandon Stark are member of, but Brandom is not a member of these guild (four versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-3.png)

## Quantifiers #3

**A third way to use quantifiers:** A quantifier may start of a pattern. On the quantifier's left side - the pattern's start, while each right component may start with either:

* An entity (yellow/aggregated/blue/logical/red)
* A quantifier

The _None_ quantifier cannot start a pattern.

Here is a fourth way to represent Q26:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-4.png)

## Quantifiers #4

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB14.png)

In addition to the 4 quantifiers described above (_All_, _Some_, _> n_, _≥ n_), V1 supports these 8 quantifiers:

* **Not all** (denoted by an '&' with stroke) - An assignment matches the pattern only if it matches a similar pattern, where the quantifer is replaced with _All_, and an 'X' is added to (or removed from) the start of one or more branches.
* **None** (denoted '0') - An assignment matches the pattern only if it matches a similar pattern, where the quantifer is replaced with _All_, and an 'X' is added to (or removed from)  the start of all branches.
* **_n_** - An assignment matches the pattern only if it matches a similar pattern, where the quantifer is replaced with _All_, and an 'X' is added to (or removed from) the start of _b-n_ branches. _n_ ∈ [1, _b_]
* **_< n_** - An assignment matches the pattern only if it matches a similar pattern, where the quantifer is replaced with _All_, and an 'X' is added to (or removed from) the start of more than _b-n_ branches. _n_ ∈ [2, _b_]
* **_≤ n_** - An assignment matches the pattern only if it matches a similar pattern, where the quantifer is replaced with _All_, and an 'X' is added to (or removed from) the start of _b-n_ or more branches. _n_ ∈ [1, _b_]
* **_≠ n_** - An assignment matches the pattern only if it matches a similar pattern, where the quantifer is replaced with _All_, and an 'X' is added to (or removed from) the start of any number but _b-n_ branches. _n_ ∈ [1, _b_]
* **_n1..n2_** - An assignment matches the pattern only if it matches a similar pattern, where the quantifer is replaced with _All_, and an 'X' is added to (or removed from) the start of less than _b-n2_ or more than _b-n1_ branches. _n1_ ∈ [1, _b_], _n2_ ∈ [2, _b_], _n1_ < _n2_
* **_∉ n1..n2_** - An assignment matches the pattern only if it matches a similar pattern, where the quantifer is replaced with _All_, and an 'X' is added to (or removed from) the beginning more than _b-n2_ but less than _b-n1_. _n1_ ∈ [2, _b-1_], _n2_ ∈ [3, _b_], _n1_ < _n2_

(_b_ denotes the number of branches)

## Horizontal Quantifiers

Horizontal quantifiers are used with relationships / paths. On top of a horizontal quantifier is a relationship / path, and on its bottom - two or more branches.

Each branch starts with:

* A green rectangle (relationship only - relationship's property value constraints / tag),
* An aggregate condition / an aggregation tag, or
* An horizontal quantifier

We'll call the left side of the relationship 'the left component', and anything on the right of the relationship 'the right component'.

Horizontal quantifiers behave quite differently from vertical quantifiers:

* **All** (denoted '&') - An assignment matches the pattern only if (it satisfies at least one branch) and also (for each branch: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the branch)
* **Some** (denoted '&#124;') - An assignment matches the pattern only if (it satisfies at least one branch)
* **Not all** (denoted by an '&' with stroke) - An assignment matches the pattern only if (it satisfies at least one branch) and also (for at least one branch: there is no assignment with the same concrete elements in its left and right components - that satisfies the branch)
* **None** (denoted '0') - Each assignment matches the pattern only if (for each branch: there is no assignment with the same concrete elements in its left and right components - that satisfies the branch)

"_Only if_" denotes a necessary but not sufficient condition, since assignments must satisfy other conditions expressed by the pattern.

Additional horizontal quantifiers:

* **_n_** -  An assignment matches the pattern only if (it satisfies at least one branch) and also (for exactly _n_ branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the branch). _n_ ∈ [1, _b_]
* **_< n_** - An assignment matches the pattern only if (it satisfies at least one branch) and also (for less than _n_ (but more than 0) branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the branch). _n_ ∈ [2, _b_]
* **_≤ n_** - An assignment matches the pattern only if (it satisfies at least one branch) and also (for _n_ or less (but more than 0) branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the branch). _n_ ∈ [1, _b_]
* **_n1..n2_** - An assignment matches the pattern only if (it satisfies at least one branch) and also (for _n1_ up to _n2_ branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the branch). _n1_ ∈ [1, _b_], _n2_ ∈ [2, _b_], _n1_ < _n2_
* **_≠ n_** - An assignment matches the pattern only if (it satisfies at least one branch) and also (for any number of branches except 0, _n_: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the branch). _n_ ∈ [1, _b_]
* **_∉ n1..n2_** - An assignment matches the pattern only if (it satisfies at least one branch) and also ((for more than 0 but less than _n1_ branches) or (for more than _n2_ branches): there is at least one assignment with the same concrete elements in its left and right components - that satisfies the branch). _n1_ ∈ [2, _b-1_], _n2_ ∈ [3, _b_], _n1_ < _n2_

Here are two examples:

_**Q187:** Any dragon that was frozen by Balerion: (at least once in 1/1/1010 or later) and (at least once - for longer than 10 minutes) - same or different freezes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q187.png)

_**Q189:** Any dragon that was frozen by Balerion: (at least once in 1/1/1010 or later) or (at least once for longer than 10 minutes) **Alternative wording:** Any dragon that was frozen by Balerion: at least once (in 1/1/1010 or later, or for longer than 10 minutes)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q189.png)

Green rectangles, aggregate conditions, and aggregation tags  below an horizontal quantifier can also be **chained**. When chained, each element serves as a filtering step. The branch's condition is met only if there is an assignment that passes all the filtering steps.

Here is an example:

_**Q188:** Any dragon that was frozen by Balerion at least once - in 1/1/1010 or later - for longer than 10 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q188.png)

## E-combiner

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB15.png)

An E-combiner combines two or more branches (not necessarily of the same quantifier). On its left side are entities and on its right side either:

* A relationship / a path (optionally preceded by an 'X', an '↛', an 'O' or an 'L')
* A green rectangle (entity's property value constraints / tag)
* A quantifier

The right side of an E-combiner is a direct continuation of each of the combined branches. An E-combiner is simply a syntactic sugar used to save duplication when several branches terminates identically.

The usage of an entity tag both before and after an E-combiner (to express identicality / nonidenticality constraints) subjects to tag rules (again - anything after the E-comniner is duplicated to each branch).

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

## Red Entities

Sometimes the same relationship type can hold between different pairs of entity types (e.g. owns(Person, Dragon); owns(Guild, Dragon) ). We need red entities to express patterns such as "_any dragon and its owners_", when the owner can be either a person or a guild.
 
 In it simplest form, a red rectangle represents an entity of any type.

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

Even when a red entity is on either side of an 'X' - the entity's type is constrained by relationship types. Here are some examples:

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

Each path has a length. The length of the path is equal to the number of entities along the path. Relationships are actually paths with length 0. The number of relationships along a path is always larger by one than the number of entities along the path.

A path is depicted by a red line between two entities. Above the line there must be a limitation to the path length, which is  expressed using one of the following operators: _< n, ≤ n, in [n1..n2], in [n1, n2, ...]_, where all numbers a positive integers.

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

An alternative to constraints on the entities and relationships along the path, are constraints on **path segment types**. A path segment type is a valid chain of entities (yellow/aggregated/blue/logical/red) and relationship types that starts and ends with an entity. There is an 'overlap' between successive segment types:

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

## Property Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB06.png)

A property tag is depicted by an index wrapped in **purple curly brackets**

A property tag serves as a placeholder for the property's value in a given assignment, and used for defining constraints on the value of other properties (e.g. _birth date_ > {1}, where {1} is defined as the _birth date_ property of another entity)

Here are some examples:

_**Q108:** Any person who has the same birth-date as Brandon Stark_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q108.png)

_**Q109:** Any person who his parent owned a horse and a dragon prior to his birth_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q109.png)

Note that if a branch contains only a green rectangle with no constraints (only with a property tag) - the branch is always satisfied.

_**Q110:** Any 3 dragons with cyclic freezes of more than 100 minutes in chronological order, and their owners_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q110.png)

_**Q111:** Any person who doesn't know someone with a birth date similar to his_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q111.png)

_**Q112:** Any person who owned a horse and a dragon in the same time frames_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q112.png)

## Entity Type Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB08.png)

A red rectangle may contain an **entity type tag**, depicted by a numeric index wrapped in **purple triangular brackets**. An entity type tag serves as a placeholder for the entity type in a given assignment, and can be used to define constraints on the type of other red entities.

Here are some examples:

_**Q50:** Any person who owns (at least) two things of the same type_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q050.png)

_**Q51:** Any person who owns (at least) two things of different types_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q051.png)

_**Q52:** Any person who owns (at least) two things of different types, both are not horses_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q052.png)

## Aggregate Conditions and Aggregate Tags

todo: aggregate conditions

todo: aggregate tag, aggregate tag's scope


## Aggregate Conditions #1 (L1C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-L1C.png)

- _et_ is an entity tag of a blue/logical/red entity defined right of (but not directly right of) the aggregator
- If →: directly right of the aggregator: a blue/logical/red entity (neither yellow/aggregated entity, nor quantifier)
- If et: directly right of the aggregator: an entity (yellow/aggregated/blue/logical/red) or a quantifier. If quantifier - et must be defined right of an R-combiner

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

First, any pair (A,B) that matches the pattern is found. Then, the aggregate condition is checked:

For any concrete A:

* There are at least 10 concrete B's such that (B froze A, and A froze B)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q177-2.png)

For any concrete A:

* There are at least 10 concrete B's such that (B froze A, and A froze B)
* There are at least 10 concrete B's such that (A froze B, and B froze A)

_**Q178:** Any dragon A that was frozen by at least 10 dragons and either (i) A froze only one dragon - which is not one of those (ii) A froze at least 2 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q178.png)

First, any triplet (A,B,C) that matches the pattern is found. Then, the aggregate condition is checked:

For each concrete A:

* There are at least 10 concrete B's such that (B froze A, and A froze a dragon that is not B)

Hence, for each concrete A:

* At least 10 dragons froze A and either (i) A froze only one dragon - which is not one of those (ii) A froze at least 2 dragons

_**Q85:** Any dragon that froze at least 10 dragons, and was frozen by at least 10 dragons (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q085-1.png)

This 2nd version is for illustrative purposes only...

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q085-2.png)

First, any triplet (A,B,C) that matches the pattern is found. Then, the aggregate conditions are checked:

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

_**Q166:** Any **dragon** that more than 5 Sarnorian subjects own a dragon that froze **it**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q166.png)

_**Q113:** Any person who knows at least 5 people with a birth data similar to his_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q113.png)

_**Q114:** Any person who owns more than 5 horses of the same color_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q114.png)

_**Q115:** Any person who owns more than 5 horses since the same date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q115.png)

_**Q125:** Any dragon that the number of dragons it froze is greater than the number of dragons that froze it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q125.png)

_**Q126:** Any dragon that the number of dragons it froze is greater than the number of dragons it didn't freeze_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q126.png)

_**Q63:** Any Masons guild member who more than 5 Masons guild members don't know_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q063.png)

_**Q65:** Any person who doesn't own more than 2 (things that his spouse owns)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q065.png)

_**Q64:** Any **dragon** that less than 5 (dragons owned by Sarnorian subjects) didn't freeze **it**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q064.png)

_**Q165:** Any **dragon** that less than 5 (Sarnorian subjects own a dragon that didn't freeze **it**)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q165.png)

_**Q66:** Any person from whom more than 5 people are not within graph distance ≤ 6_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q066.png)

_**Q151:** Any person who owns more than 10 horses, at least one is Sarnorian. Only the Sarnorian horses will be returned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q151.png)

_**Q152:** Any person who owns more than 10 horses. Only the Sarnorian horses will be returned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q152.png)

## Aggregate Conditions #2 (L2C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-L2C.png)

- Directly right of the aggregator: an entity (yellow/aggregated/blue/logical/red) or a quantifier

_**Q71:** Any dragon that froze dragons more than 10 times (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q071.png)

_**Q72:** Any dragon that was frozen exactly 10 times (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q072.png)

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

_**Q104:** Any person who owned white horses at least 10 times (same or different horsess)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q104.png)

_**Q105:** Any dragon A that was frozen exactly 2 times (cumulatively) by (dragons that each was frozen by at least one dragon)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q105.png)

_**Q127:** Any dragon that frozed more times dragons owned by Sarnorian subjects than dragons owned by Omberian subjects_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q127.png)

## Aggregate Conditions #3 (LA3C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LA3C.png)

- Directly right of the aggregator: an entity (yellow/aggregated/blue/logical/red)

_**Q87:** Any dragon that was frozen at least once, and the cumulative duration he was frozen is smaller than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q087.png)

_**Q88:** Any dragon pair (A,B) where A froze B at least once, but the cumulative freezing duration is 0 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q088.png)

_**Q89:** Any dragon that freezes dragons for more than 3 different durations_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q089.png)

## Aggregate Conditions #4 (LA4C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LA4C.png)

- Directly right of the aggregator: a yellow/aggregated/blue/logical/red entity, or a quantifier. If quantifier - pt/at/st/ett must be defined right of an R-combiner

_**Q116:** Any person who owns horses of no more than 3 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q116.png)

_**Q117:** Any person whose owned horses have an average weight greater than 450 Kg_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q117.png)

_**Q134:** Any person that the number of distinct colors of all horses owned by people he knows - is not more than 3_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q134.png)

_**Q135:** Any person that the average weight of all horses owned by people he knows - is greater than 450 Kg_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q135.png)

_**Q137:** Any dragon A that froze dragons B - each froze at least one dragon which is not A. All these B's together froze dragons for more than 100 minutes cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q137.png)

_**Q139:** Any person who owns horses of the same number of colors as the number of colors of the horses owned by his parents cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q139.png)

_**Q167:** Any person who owns things of at least 3 types_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q167.png)

## Aggregate Conditions #5 (D2C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-D2C.png)

- Directly right of the aggregator: an entity (yellow/aggregated/blue/logical/red)

_**Q75:** Any dragon pair (A, B) where B froze A between 8 and 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q075.png)

_**Q76:** Any dragon that froze Balerion between 8 and 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q076.png)

## Aggregate Conditions #6 (DA3C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-DA3C.png)

- Directly right of the aggregator: an entity (yellow/aggregated/blue/logical/red)

_**Q86:** Any dragon pair (A, B) where A froze B for a cumulative duration greater than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q086.png)

## Aggregate Conditions before a Quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-Before-Quantifier1.png)

- Valid only for L1C followed by an R-Combiner, L2C, and LA4C followed by an R-Combiner

_**Q121:** Any dragon that froze or fired at at least 10 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q121.png)

_**Q122:** Any dragon that fired at dragon B, and fired at a dragon that fired at B - for at least 10 different B's_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q122.png)

_**Q175:** Any dragon that froze at least once, and fired at least once. The number of dragons it froze/fired at - is at least 10 (if a dragon was both froze and fired at - it would be counted twice)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q175.png)

_**Q176:** Any dragon that either (i) froze at least one dragon and fired at at least one dragon it didn't froze. The number of dragons it frozed/fired at is at least 10 (ii) froze at least 10 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q176.png)

_**Q123:** Any dragon that either froze a dragon or was frozen by a dragon - at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q123.png)

_**Q124:** Any dragon that either (froze a dragon) or (fired a dragon that fired a dragon) - at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q124.png)

(counting the number of relationships to each entity on the right of the quantifier)

_**Q173:** Any dragon that fired at at least 2 dragons, and fired at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q173.png)

(counting the number of *distinct* 'fired at' relationships)

_**Q174:** Any dragon that either (i) froze at least one dragon and fired at at least one dragon it didn't froze (ii) froze at least two dragons. If (i): the number times it froze / fired at dragons is at least 10. otherwise: The number of times it froze dragons is at least 10._

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q174.png)

## Aggregate Conditions both before and after a Quantifier

todo

_**Q191:** Any dragon that froze X≥3 dragons and fired at Y≥3 dragons. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q191.png)

_**Q192:** Any dragon that froze dragon X≥3 times, and fired at dragons Y≥3 times. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q192.png)

_**Q193:** Any dragon that froze X≥3 dragons, fired at Y dragons, and fired Z≥3 times. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q193.png)

_**Q194:** Any dragon that froze dragons X times, fired at dragons Y≥3 times, and froze Z≥3 dragons. X+Y≥10__

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q194.png)

## Min/Max Aggregations

todo

## Min/Max Aggregations #1 (LRM1)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LRM1.png)

- _et_ is an entity tag of a blue/logical/red entity defined right of (but not directly right of) the aggregator
- Directly right of the aggregator: a blue/logical/red entity (neither yellow/aggregated entity, nor quantifier)
- LRM1 may appear below a relationship / path. The relationship / path may be wrapped by an '↛' or an 'L'
- Except for '&' quantifer - LRM1 aggregator cannot start a quantifier's branch

For each assignment to the entity on its left (←), this aggregation will limit the number of assignments to the entity on its right (→) - to those (→) with the smallest / largest number of assignments to _et_. 

- Suppose the pattern is "5 → with max D" but there are only 3 →'s with >0 assignments to D - Only these 3 will be included.
- Suppose the pattern is "5 → with max D" but there are 10 →'s with identical max number of assignments to D - all 10 will be included.

_**Q196:** Any **dragon** owned by Brandon Stark, and the 3 dragons **it** froze that froze the largest number of dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q196.png)

_**Q197:** Any person and his 3 dragons that the dragons they froze - froze the largest number of distinct dragons cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q197.png)

_**Q198:** Any person and his 3 dragons that (for each of them: the 4 dragons it froze that froze the largest number of dragons) - froze the largest number of distinct dragons cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q198.png)

## Prefix Min/Max Aggregations #1 (PRM1)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-PRM1.png)

- _et_ is an entity tag of a blue/logical/red entity defined right of (but not directly right of) the aggregator
- Directly right of the aggregator: a blue/logical/red entity (neither yellow/aggregated entity, nor quantifier)

_**Q67:** The 3 people with the largest number of parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q067.png)

_**Q68:** The 2 dragons that were frozen by the largest number of dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q068.png)

_**Q69:** The 2 things that own the largest number of things_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q069.png)

_**Q70:** The 5 people who the number of people within graph distance ≤ 4 from them - is the smallest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q070.png)

## Min/Max Aggregations #2 (LRM2)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LRM2.png)

- LRM2 may appear below a relationship / path. The relationship / path may be wrapped by an 'L'.
- Directly right of the aggregator: a blue/logical/red entity (neither yellow/aggregated entity, nor quantifier)

_**Q195:** Any dragon owned by Brandon Stark, and the 3 dragons it froze the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q195.png)

## Prefix Min/Max Aggregations #2 (PRM2)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-PRM2.png)

- Directly right of the aggregator: a blue/logical/red entity (neither yellow/aggregated entity, nor quantifier)

_**Q171:** The 2 dragons that were frozen the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q171.png)

_**Q172:** The 5 people with the smallest number of paths with length ≤ 4 to some person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q172.png)

## Min/Max Aggregations #3 (LRMA3)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LRMA3.png)

- Directly right of the aggregator: a blue/logical/red entity (neither yellow/aggregated entity, nor quantifier)

_**Q182:** Any dragon owned by Brandon Stark, and the 3 dragons it froze for the longest cumulative period_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q182.png)

_**Q201:** For any dragon that froze at least 10 dragons: the 3 dragons it froze for the longest cumulative period_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q201.png)

## Min/Max Aggregations #4 (LRM4)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LRM4.png)

- {pt} is an ordinal property of the entity directly right of the aggregator
- {at}/{st} is a calculated property of the entity directly right of the aggregator
- LRM4 may not appear directly before a quantifier

_**Q118:** Any person and his 3 eldest offsprings_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q118.png)

_**Q119:** Any person and his 3 youngest sons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q119.png)

## Prefix Min/Max Aggregations #4 (PRM4)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-PRM4.png)

- {pt} is an ordinal property of the entity directly right of the aggregator
- {at}/{st} is a calculated property of the entity directly right of the aggregator
- PRM4 may not be used when the pattern starts with a quantifier

_**Q130:** The 4 eldest people_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q130.png)

_**Q131:** The 4 eldest males_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q131.png)

_**Q91:** The 4 dragons with the maximal (shortest period they were frozen for)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q091.png)

_**Q92:** The 4 dragons with the maximal (average duration they froze dragons for)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q092.png)

_**Q132:** The 4 people who own horses of the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q132.png)

_**Q133:** The 4 people who the average weight of their horses is maximal_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q133.png)

_**Q138:** The 4 people who the people each of them knows - cumulatively own horses of the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q138.png)

_**Q183:** The 3 dragons that dragons owned by Brandon Stark froze for the longest cumulative period_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q183.png)

_**Q168:** The 3 people who the number of types of things each of them owns - is the largest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q168.png)

## Min/Max Aggregations #5 (DM2)

- DM2 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-DM2.png)

todo

_**Q77:** The 5 dragon pairs (A, B) with the largest number of times B froze A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q077.png)

_**Q78:** The 4 dragons that froze Balerion the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q078.png)

_**Q80:** The 3 person pairs with the largest number of paths with length ≤ 4 between them_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q080.png)

## Min/Max Aggregations #6 (DMA3)

- DMA3 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-DMA3.png)

todo

_**Q90:** The 4 dragon pairs (A, B) where A froze B for the longest cumulative period_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q090.png)

## Min/Max Aggregations #7 (LDM3)

- LDM3 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LDM3.png)

todo

_**Q161:** For any dragon: the 4 longest times it froze some dragon_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q161.png)

## Min/Max Aggregations #8 (DDM3)

- DDM3 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-DDM3.png)

todo

_**Q160:** For any dragon pair (A, B): The 4 longest times A froze B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q160.png)

## Aggregation Chains

todo

_**Q96:** Any dragon that was frozen by Balerion, in  1/1/1010 or later, more than 10 times, for periods shorter than 10 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q096.png)

_**Q93:** Any dragon that froze at least 3 dragons for more than 10 minutes. The number of times it froze each of these dragons is at least 5_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q093.png)

Filtering stages:

- Pass only (freezes that are longer than 10 minutes)
- Pass only dragons A that froze at least 3 dragons for more than 10 minutes
- Pass only dragon pairs (A, B) where A froze B for more than 10 minutes - at least 5 times

_**Q94:** Any dragon that froze at least 3 dragons - each at least 5 times for more than 10 minutes_

Filtering stages:

- Pass only (freezes that are longer than 10 minutes)
- Pass dragon pairs (A, B) where A freezd B at least 5 time for more than 10 minutes
- Pass only dragons A that froze for more than 10 minutes - at least 3 dragons

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q094.png)

_**Q162:** Any dragon pair (A, B) where the second shortest period A froze B is longer than 60 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q162.png)

_**Q163:** Any dragon that the average duration of the 10 shortest times it freezes is greater than 60 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q163.png)

_**Q185:** Any dragon that was frozen by Balerion: at least one time in 1/1/1010 or later, at least one time for less than 10 minutes, more than 10 times (in 1/1/1010 or later, or for less than 10 minutes)_

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

_**Q120:** Any person whose 3 eldest offsprings cumulative height is smaller than his own height_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q120.png)

_**Q202:** Any person whose 3 eldest offsprings average height is smaller than the average height of all his offsprings_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q202.png)

_**Q140:** Any person who his 3 eldest sons cumulatively own horses of 3 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q140.png)

_**Q141:** Any person who his 3 eldest sons cumulatively own horses of the same number of colors as those owned cumulatively by his 3 younger daughters_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q141.png)

## Aggregation Sequences

todo

_**Q128:** Any person and his 3 offspring that own horses of the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q128.png)

_**Q103:** Any dragon A that froze at least 3 dragons - each was frozen by at least 4 dragons other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q103.png)

_**Q106:** Any dragon A that froze dragons at least 3 times - each was frozen at least 4 times by dragons other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q106.png)

_**Q107:** Any **dragon** that (the number of dragons owned by 5 people each, that froze **it**) is 5, and that the number of times **it** was frozen by those dragons (cumulatively) is not 5_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q107.png)

_**Q169:** Any person that (each of his offsprings who owns at least 3 horses - owns horses of at least 3 colors)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q169.png)

_**Q129:** Any person that (each of his offsprings who owns at least one horse - owns a different number of horses)_

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

Note that the order of the filtering stages along the left aggregation chain can be switched. The semantics would remain the same.

_**Q180:** Any dragon pair (A, B) where the cumulative duration A froze B or vice versa - is greater the cumulative duration A froze dragons, and greater than the cumulative duration B froze dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q180.png)

## Splits

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB09.png)

todo

_**Q153:** Any dragon that in at least 10 days froze no more than 5 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q153.png)

_**Q214:** Any person who owns entities of at least 4 types. For each type - at least 5 entities_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q214.png)

_**Q154:** Any dragon that in at least 4 years - in at least 11 days - froze no more than 5 dragons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q154.png)

_**Q155:** Any dragon that in at least 11 days - froze more than 100 minutes cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q155.png)

_**Q156:** Any dragon that froze dragon for more than 1000 minutes cumulatively - in the days it froze dragons for more than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q156.png)

_**Q157:** Any person who owns at most 3 horses of the same color - for more than 5 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q157.png)

## Split before a Quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB10.png)

todo

_**Q158:** Any dragon that in at least 10 days - the number of dragons it froze is greater than the number of dragons that froze it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q158.png)

## Split Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB11.png)

todo: split tag, split tag's scope

_**Q159:** Any **dragon** for which there are more days where (the number of dragons **it** froze is greater than the number of dragons that froze **it**) than days where (the number of dragons that froze **it** is greater than the number of dragons **it** froze)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q159.png)

## Aggregate Entities

An aggregate entity is a virtual concrete entity, that encapsulates several entities. It is defined by:

* The entities it encapsulates: A set of
  * Concrete (yellow) entities
  * Entity types (blue) with optional constraints
  * Entities (red) with constraints
* A entity type name assigned to the aggregation

Aggregate entities can be defined, and then used in queries.

Here are some definition examples:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB13.png)

* _'The Beatles'_ is defined as an encapsulation of 4 concrete entities
* _'Old People'_ is defined as an encapsulation of all people born before 920
* _'Old males'_ is defined as an encapsulation of all males born before 920
* _'Red things'_ is defined as an encapsulation of all things which have a property 'color' with value 'red'
* _'Sarnorian'_ is defined as an encapsulation of all Sarnorian subjects and all guilds registered in Sarnor

An encapsulated entity in a query will show as an aggregate entity in the query's result. It won't 'disassemble' into the entities it encapsulates. 

Aggregate entities have the following auto-generated aggregate properties:

* _'count'_ - the number of encapsulated entities
* _'e.count'_ - the number of encapsulated entities of type _e_ (valid for any encapsulated entity type)
* _'e.p.distinct'_ - the number of distinct values of property _p_ of encapsulated entity type _e_ (valid for any property type of any encapsulated entity type)
* _'e.p.min', 'e.p.max', 'e.p.avg', 'e.p.sum'_ - the min, max, sum, and average of property _p_ of entity type _e_ (valid for any numeric property type of any encapsulated entity type)

Using aggregate entities in queries:

* Aggregate entities are used in a similar manner to **yellow** entities
* Adjacent relationship types should support at least one encapsulated entity type
* Constraints cannot be defined for aggregate entities
* Aggregate entities can't be counted. (e.g. the entity on the right of an “… n → …“ aggregations (L1C, LRM1, PRM1, LRM2, PRM2, LRMA3, LRM4 and PRM4) can't be an aggregate entity)

Here are some examples:

_**Q208:** Any dragon owned by an entity encapsulated within 'The Beatles' - since 1/1/1011 or since a later date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q208.png)

The aggregate entity 'The Beatles' will be part of the query result. It won't be disassembled into its four members.

_**Q209:** Any dragon than froze at least 3 dragons owned by entities encapsulated within 'Sarnorian'_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q209.png)

If, for example, some dragon froze 2 dragons owned by McCartney, and 1 dragon owned by Starr - it would be part of the answer. Again, 'The Beatles' will be aprt of the query result.

_**Q210:** Any person who has at least 3 'owns' relationships with entities encapsulated within 'Red Things'_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q210.png)

_**Q211:** Any path with length ≤ 4 between an entity encapsulated within 'Sarnorian' and an entity encapsulated within 'The Beatles'_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q211.png)

_**Q212:** Are there more than 10 days in which at least 10 ownership relationships started between entities encapsulated within 'Old People' and entities encapsulated within 'Red Things'?_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q212.png)

## Logical Entity Types

A logical entity type can be assigned to specific entities. It is defined by:

* The entities it assigns a new type name to:
  * Concrete (yellow) entities
  * Aggregate entities
  * Entity types (blue) with optional constraints
  * Entities (red) with constraints
* A entity type name assigned to each such entity

Logical entities types can be defined, and then used in queries.

Here are some definition examples:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB12.png)

* _'Beatle'_ is defined as one of 4 concrete entities
* _'Old Person'_ is defined as a person born before 920
* _'Old male'_ is defined as a male born before 920
* _'Red thing'_ is defined as a an entity that has a property 'color' with value 'red'
* _'Sarnorian'_ is defined as a a Sarnorian subject, or a Sarnorian registered guild
* _'Rock Band'_ is defined as one of 4 aggregate entities

In a query's result - entities of logical type are resolved to concrete entities (similar to blue and red entities).

Logical entity types have the following properties:

* _'et.p'_ - where _et_ is an entity type used in the definition, and _p_ is a name of a property of that entity type

Using logical entity types in queries:

* Logical entity types are used in a similar manner to **blue** entities
* Adjacent relationship types should support at least one entity type used in the definition

Here are examples of patterns that incorporate the logical entity types defined above:

_**Q203:** Any dragon owned by a Beatle since 1/1/1011 or since a later date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q203.png)

_**Q204:** Any dragon than froze at least 3 dragon owned by a 'Sarnorian' (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q204.png)

_**Q205:** Any person who has at least 3 'owns' relationships with 'Red Things'_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q205.png)

_**Q206:** Any 'Sarnorian' and 'Beatle' pair with graph distance ≤ 4_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q206.png)

_**Q207:** Are there more than 10 days in which at least 10 ownership relationships started between a certain 'Old Person' and a certain 'Red Thing'?_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q207.png)

## Tag Rules

**R1:** Only one property tag can be assigned to any property.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag01.png)

**R2:** For any quantifier except '&' - a tag defined in a branch cannot be referenced in other branches, and cannot be referenced left of the quantifier.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag03-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag03-2.png)

**R3:** A tag defined right of an 'X' - cannot be referenced left of its definition. Additionally - A tag defined right of an 'X' on a quantifier's branch - cannont be referenced in other branches.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag05-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag05-2.png)

**R4:** A tag defined right of an 'O' - cannot be referenced left of its definition. Additionally - A tag defined right of an 'O' on a quantifier's branch - cannont be referenced in other branches.

**R5:** A tag defined right of an '↛' - cannot be referenced left of its definition. Additionally - A tag defined right of an '↛' on a quantifier's branch - cannont be referenced in other branches.

**R6:** Circular conditions are invalid.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag02.png)

**R7:** Several branches of an '&' quantifier may not reference tags circularly (e.g. branch 1 reference a tag defined in branch 2 and vice versa). There must must a valid order to evaluate branches.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag04.png)

**R8:** An aggregation can only reference tags defined on its right.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag08.png)

**R9:** A tag defined right of an aggregator - cannot be referenced in other branches.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag09.png)

**R10:** A relationship's property tag defined as part of an aggregation chain - cannot be used in other branches.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag10.png)

## Aggregation Rules

**R11:** Concrete (yellow) entities can't be counted. (e.g. the entity on the right of an “… n → …“ aggregations (L1C, LRM1, PRM1, LRM2, PRM2, LRMA3, LRM4 and PRM4) can't be an aggregate entity).

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Agg01.png)

**R12:** Aggregations cannot be used when there is no entity on the right.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Agg02.png)

**R13:** A tag defined right of an LA3C aggregator - cannot be referenced in the aggregate condition.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Agg03.png)

**R14:** A tag defined in an LA3C aggregator - cannot be referenced right of its definition.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Agg04.png)

## Missing Values

todo
