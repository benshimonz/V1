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

There is no standard way to define property graph schemas. Implementations may vary in many aspects: the properties' data types (basic types, categorical, multivalued, composite, nested) and supported operators, the relationship types supported directionality (unidirectional, bidirectional, mixed), constraints (mandatory properties, relationships cardinality, etc.), entity type hierarchies, relationship type hierarchies, etc.

A **schema-based property graph** is a property graph that conforms to a given schema.

**Why do we need a schema?**

Schema-free property graphs do not define nor enforce entity-types nor relationships-types; each vertex and each edge may contain properties with any name and any value type. Without schema we can’t enforce integrity. Without such integrity we can't define formal building-blocks for representing patterns.

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

**A pattern language defines syntax and semantics for expressing patterns.**

A pattern can be viewed as a query that can be executed against a property graph. **An assignment** is a concrete multi-set of graph elements (entities and relationships) that conform to the pattern (note that an element may appear in an assignment more than once). The answer to a query can be defined either as (i) the set of all assignments, or (ii) the union of all assignments. (ii) is usually prefered since it can avoid exponential explosion in many queries.

Pattern languages differs in the following aspects:

* **Genericity** - generic (e.g. schema-driven) vs. domain-specific
* **Representation** - textual (structured / controlled natural) or visual (AKA graphical, diagrammatic)
* **Expressivity** - The richness of the patterns that can be expressed. The expressivity is always limited (in a Gödelian sense)
* **Simplicity** - How simple it is to construct new patterns and to understand existing patterns
* **Efficiency** - to parse patterns and to execute pattern queries

## The V1 Pattern Language

Many potential users won't use textual pattern (query) languages. The learning curve may be too sharp for someone with little prior experience in programming. Even users that do use textual query languages often spend a lot of time on the technicalities. 

Users want to be able to pose complex queries in a manner that is coherent with the way they think. They want to do it with minimal technical training and minimal effort. The ability to express patterns in a way that is aligned with their mental processes is crucial to the flow of their work, and to the value of the insights they can deduce.

Visual query languages are very attractive. They have the potential to be more 'user friendly' in the sense that patterns may be constructed and understand much more quickly, with much less mental effort. A long-standing challenge is to design a visual query language that is generic, simple, and has rich expressivity. This is what V1 aims to be. 

V1 is a rich simple generic visual pattern language for schema-based property graphs. It is named after the [primary visual cortex in our brain](https://en.wikipedia.org/wiki/Visual_cortex), which is also known as Visual area one (V1).

## V1 Basics

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB01.png)

Red, blue and yellow rectangles represent entities. **A yellow rectangle** represents a concrete entity: a specific person, a specific car, etc. The text inside a yellow rectangle denotes the entity type, and the value of the entity's leading properties (e.g. first name and last name of a person). **A blue rectangle** represents an entity of a given type. A blue 'Person' for example represents any person. **A red rectangle** represents an entity of any type (unless type constraints are defined). For every blue rectangle and for every red rectangle, the query processor will look for **assignments** of concrete entities that match the pattern.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB04.png)

Any pattern starts with a small black diamond, and is read from left to right. 

A pair of entities can be connected with:

* A horizontal **black arrow** - denoting a **directional relationship**,
* A horizontal **black line** - denoting either a **non-directional relationship** or a directional relationship where the direction doesn't matter, or
* A horizontal **red line** - denoting a **path** (explained later)

Each relationship has a label which denotes the relationship's type.

Patterns have a tree-like structure (as opposed to graph-like structure).

Here are some basic patterns:

_**Q1:** Any phone owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q001.png)

_**Q2:** Any phone that received at least one call from a phone owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q002.png)

_**Q184:** Any phone that made or received at least one call from a phone owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q184.png)

The call's direction does not matter. Therefore - a **non-directional relationship** is used in the pattern.

## Properties

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB02.png)

**A green rectangle** is connected to an entity or to a relationship, and represents an entity's / relationship's property. It contains:

* The property's name
* Optionally - constraint on the value of that property, expressed by an equation (e.g. 'age > 30')
* Optionally - A **property tag**, depicted by an index wrapped in **purple curly brackets**. 

A property tag serves as a placeholder for the property's value in a given assignment, and used for defining constraints on the value of other properties (e.g. age > {1}, where {1} is defined as the age property of another entity). More on property tags - later.

_**Q3:** Any person who owns a phone, and his first name is Lior **(v1)**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-1.png)

_**Q190:** Any person who owns a phone since 1/1/2011 or since a later date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q190.png)

## Quantifiers #1

Vertical quantifiers (or simply 'quantifiers') are used when several conditions need to be checked. Here is a simple example:

_**Q3:** Any person who owns a phone, and his first name is Lior **(v2)**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-2.png)

A quantifier has one connection on its left side, and two or more branches on its right side. We'll call the left side of the quantifier 'the left component', and anything that follows a branch, up to the end of the branch, 'a right component'.

**The first way to use quantifiers:** The left component ends with an entity (red / blue / yellow), and each right component starts with:

* A relationship (optionally preceded by an 'X', an '↛', an 'O' or an 'L'),
* A path (optionally preceded by an 'X', an '↛', an 'O' or an 'L'),
* A green rectangle (entity's property value constraint / tag), or
* A quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB03.png)

The most useful quantifiers are:

* **All** (denoted '&') - An assignment is valid only if it satisfies every branch
* **Some** (denoted '&#124;') - An assignment is valid only if it satisfies at least one branch
* **Not all** (denoted by an '&' with stroke) - An assignment is valid only if it doesn't satisfy at least one branch
* **None** (denoted '0') - An assignment is valid only if it satisfies no branch

("_only if_" denotes a necessary but not sufficient condition, since the pattern may contain other quantifiers / conditions)

Additional quantifiers:

* **_n_** - An assignment is valid only if it satisfies exactly _n_ branches. _n_ ∈ [1, _b_]
* **_> n_** - An assignment is valid only if it satisfies more than _n_ branches. _n_ ∈ [0, _b-1_]
* **_≥ n_** - An assignment is valid only if it satisfies _n_ or more branches. _n_ ∈ [1, _b_]
* **_< n_** - An assignment is valid only if it satisfies less than _n_ (but more than 0) branches. _n_ ∈ [2, _b_]
* **_≤ n_** - An assignment is valid only if it satisfies _n_ or less (but more than 0) branches. _n_ ∈ [1, _b_]
* **_n1..n2_** - An assignment is valid only if it satisfies _n1_ up to _n2_ branches. _n1_ ∈ [1, _b_], _n2_ ∈ [2, _b_], _n1_ < _n2_
* **_≠ n_** - An assignment is valid only if it satisfies any number (except 0, _n_) branches. _n_ ∈ [1, _b_]
* **_∉ n1..n2_** - An assignment is valid only if it satisfies (more than 0 but less than _n1_) or (more than _n2_) branches. _n1_ ∈ [2, _b-1_], _n2_ ∈ [3, _b_], _n1_ < _n2_

(_b_ denotes the number of branches)

Only satisfied branches are part of a query's answer.

Here is an example of **nested quantifiers**:

_**Q8:** Any person born prior to 1970 and died, or that his father born no later than 1/1/1950_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q008.png)

## Horizontal Quantifiers

Horizontal quantifiers are used with relationships / paths. On top of a horizontal quantifier is a relationship / path, and on its bottom - two or more branches.

Each branch starts with:

* A green rectangle (relationship only - relationship's property value constraints / tag),
* An aggregate condition / an aggregation tag, or
* An horizontal quantifier

We'll call the left side of the relationship 'the left component', and anything on the right of the relationship 'the right component'.

Horizontal quantifiers behave quite differently from vertical quantifiers:

* **All** (denoted '&') - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also (for each branch: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch)
* **Some** (denoted '&#124;') - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch)
* **Not all** (denoted by an '&' with stroke) - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also (for at least one branch: there is no assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch)
* **None** (denoted '0') - Each assignment is valid only if (for each branch: there is no assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch)

("_only if_" denotes a necessary but not sufficient condition, since the pattern may contain other quantifiers / conditions)

Additional horizontal quantifiers:

* **_n_** -  An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also (for exactly _n_ branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch). _n_ ∈ [1, _b_]
* **_> n_** - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also (for more than _n_ branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch). _n_ ∈ [0, _b-1_]
* **_≥ n_** - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also (for _n_ or more branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the the left component, the right component, and branch). _n_ ∈ [1, _b_]
* **_< n_** - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also (for less than _n_ (but more than 0) branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch). _n_ ∈ [2, _b_]
* **_≤ n_** - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also (for _n_ or less (but more than 0) branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch). _n_ ∈ [1, _b_]
* **_n1..n2_** - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also (for _n1_ up to _n2_ branches: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch). _n1_ ∈ [1, _b_], _n2_ ∈ [2, _b_], _n1_ < _n2_
* **_≠ n_** - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also (for any number of branches except 0, _n_: there is at least one assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch). _n_ ∈ [1, _b_]
* **_∉ n1..n2_** - An assignment is valid only if (it satisfies the left component, the right component, and at least one branch) and also ((for more than 0 but less than _n1_ branches) or (for more than _n2_ branches): there is at least one assignment with the same concrete elements in its left and right components - that satisfies the left component, the right component, and the branch). _n1_ ∈ [2, _b-1_], _n2_ ∈ [3, _b_], _n1_ < _n2_

Here are two examples:

_**Q187:** Any phone that was called from 052-333-4444: (at least one call after 1/1/2010) and (at least one call longer than 10 minutes) - same or different calls_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q187.png)

_**Q189:** Any phone that was called from 052-333-4444: (at least one call after 1/1/2010) or (at least one call longer than 10 minutes) **Alternative wording:** Any phone that was called from 052-333-4444: at least one call (after 1/1/2010 or longer than 10 minutes)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q189.png)

Green rectangles, aggregate conditions, and aggregation tags  below an horizontal quantifier can also be **chained**. When chained, each element serves as a filtering step. The branch's condition is met only if there is an assignment that passes all the filtering steps.

Here is an example:

_**Q188:** Any phone that was called from 052-333-4444: at least one call (after 1/1/2010 and longer than 10 minutes)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q188.png)

## Quantifiers #2

A second way to use quantifiers: The left component ends with a relationship, and each right component starts with either:

* An entity (red / blue / yellow)
* A quantifier

Here is an example:

_**Q11:** Any current employee of IBM that, since 2011 or later, knows someone that left Oracle or Microsoft on or after June 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q011.png)

_**Q10:** Any person whose first name is Lior, who owns some phone B which called a phone C that belongs to an offspring of James Smith and called a phone that belongs either to John Price or to George Davis. At least one call from B to C was longer than 100 seconds, and took place in or after 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q010.png)

## Entity Tags

In the top-left corner of any red, blue, or yellow rectangle - there a letter. This letter is called an **'entity tag'**.

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

_**Q24:** Any person who has (at least) two parents and owns a phone that was called from a phone that is not owned by either of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q024.png)

## No-existence and No-connection

Sometimes we are looking for things that are not in the graph (e.g. _any person whose first name is Lior, and doesn't own a red vehicle_). Such patterns are composed of:

* A left component that ends with an entity (_any person whose first name is Lior_) 
* A No-existence / a no-connection language element (_"doesn't"_) 
* A relationship / path (_own)
* A right component that starts with an entiy (_a red vehicle_)

An assignment is a multi-set of concrete elements only to the left component.

The example above covers two cases: (i) there may be no red vehicles at all, or (ii) there may be red vehicles, but none of them is owned by a person whose first name is Lior. 

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB05.png)

Usually it doesn't matter which is the case, since _any person whose name is Lior and doesn't own a red vehicle_ is a valid assignment to the pattern. This is where the **no-existence language element (depicted with a pink 'X' box)** can be used.

An assignment is valid only if:

* It satisfies the left component (_a person whose first name is Lior_) 
* There is no assignment that satisfies a query composed of the left component, the relationship, and the right component (There is no _person whose first name is Lior and own's a red vehicle)_

An 'X' may not be used directly before a relationship or a path with an aggregation.

In certain situations, however, we need an assignment to be valid only if:

* It satisfies the left component (_a person whose first name is Lior_) 
* There is an assignment that satisfies a query composed only of the right component (there is _a red vehicle_)
* There is no assignment that satisfies a query composed of the left component, the relationship, and the right component (There is no _person whose first name is Lior and own's a red vehicle)_

This is where the **no-connection language element (depicted with a pink '↛' box)** can be used. 

'↛''s are usually used directly before a relationship or a path with an aggregation.

Examples:

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

An 'X' / an '↛' may also appear before a relationship that directly follows a quantifier's branch:

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

_**Q25:** Any of my friend's friends that isn't my friend (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-2.png)

_**Q26:** Any book that is liked by people who like some book that I like, but is not liked by me (four versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-3.png)

## Quantifiers #3

A third way to use quantifiers is at the start of a pattern. On the quantifier's left side - the pattern's start, while each right component may start with either:

* An entity (red / blue / yellow)
* A quantifier

The '0' quantifier cannot start a pattern.

Here is a fourth way to represent Q26:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-4.png)

## E-combiner

An E-combiner combines two or more branches (not necessarily of the same quantifier). On its left side are entities and on its right side either:

* A relationship / a path (optionally preceded by an 'X', an '↛', an 'O' or an 'L')
* A green rectangle (entity's property value constraints / tag)
* A quantifier

The right side of an E-combiner is a direct continuation of each of the combined branches. An E-combiner is simply a syntactic sugar used to save duplication when several branches terminates identically.

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

_**Q170:** Any phones triplet (A, B, D) where A SMSed B, A SMSed some phone that SMSed B, B called D, and B called some phone that called D_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q170.png)

_**Q33:** Any phone A that called some phone B, called some phone that called B, and SMSed some phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q033.png)

_**Q34:** Any phone A that called some phone B, called some phone that called B, SMSed some phone D and SMSed some phoned that SMSed D (B and D may be the same phone or different phones)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q034.png)

## Latent and Optional Components

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB07.png)

Anything on the right of an **'L'** is **latent**: It won't be part of the answer, though it still must have an assignment.

- An 'L' may appear just before a relationship, a path, or a quantifier (excluding a quantifier at the start of the pattern)
- An 'L' may not appear right on an 'L', an 'X' or an '↛'
- An 'L' may not appear right of a '0' quantifier
- An 'L' may not appear right of a "relationships = 0", "paths = 0" or "→ = 0" aggregate condition 

Here are two examples:

_**Q142:** Any person who owns a red vehicle, and has a parent who owns a vehicle. The parent and his vehicle are not part of the answer_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q142.png)

_**Q143:** Any person who owns a red vehicle, and has a parent that doesn't own a vehicle. The parent is not part of the answer_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q143.png)

Anything on the right of an **'O'** is **optional**: if it has a valid assignment - it will be part of the answer. Otherwise - it won't.

- An 'O' may appear just before a relationship, a path, or a quantifier (excluding a quantifier at the start of the pattern)
- An 'O' may not appear right of an 'L', an 'X' or an '↛'
- An 'O' may not appear right of a '0' quantifier
- An 'O' may not appear right of a "relationships = 0", "paths = 0" or "→ = 0" aggregate condition 

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

_**Q39:** Anything that can own a phone, but doesn't_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q039.png)

_**Q40:** Any phone that has no owner_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q040.png)

_**Q41:** Anything that can be owned, but is not owned_

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

## Property Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB06.png)

todo

todo: property tag's scope

_**Q108:** Any person who has the same birth-date as Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q108.png)

_**Q109:** Any person who his parent owned a vehicle and a phone prior to his birth_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q109.png)

Note that if a branch contains only a green rectangle with no constraints (only with a property tag) - the branch is always satisfied.

_**Q110:** Any 3 bank accounts with a cyclic transfers of more than $10000 in chronological order, and their owners_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q110.png)

_**Q111:** Any person who doesn't know someone with a birth date similar to his_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q111.png)

_**Q112:** Any person who owned a vehicle and a phone in the same time frames_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q112.png)

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

## Aggregate Conditions and Aggregate Tags

todo: aggregate conditions

todo: aggregate tag, aggregate tag's scope


## Aggregate Conditions #1 (L1C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-L1C.png)

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

_**Q177:** Any phone that was called from at least 10 phones, and called each one of those (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q177-1.png)

First, any pair (A,B) that matches the pattern is found. Then, the aggregate condition is checked:

For any assignment of A:

* There are at least 10 assignments of B such that (B called A, and A called B)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q177-2.png)

For any assignment of A:

* There are at least 10 assignments of B such that (B called A, and A called B)
* There are at least 10 assignments of B such that (A called B, and B called A)

_**Q178:** Any phone A that was called from at least 10 phones and either (i) A called only one phone - which is not one of those (ii) A called at least 2 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q178.png)

First, any triplet (A,B,C) that matches the pattern is found. Then, the aggregate condition is checked:

For any assignment of A:

* There are at least 10 assignments of B such that (B called A, and A called a phone that is not B)

Hence, for any assignment of A:

* At least 10 phones called A and either (i) A called only one phone - which is not one of those (ii) A called at least 2 phones

_**Q85:** Any phone that called at least 10 phones, and was called from at least 10 phones (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q085-1.png)

This 2nd version is for illustrative purposes only...

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q085-2.png)

First, any triplet (A,B,C) that matches the pattern is found. Then, the aggregate conditions are checked:

For any assignment of A:

* There are at least 10 assignments of B such that (B called A, and A called a phone that is not B)
* There are at least 10 assignments of C such that (A called C, and A was called from a phone that is not C)

Hence, for any assignment of A:

* At least 10 phones called A and either (i) A called only one phone - which is not one of those (ii) A called at least 2 phones

and also

* A called at least 10 phones and either (i) only one phone called A - which is not one of those (ii) at least 2 phones called A

Hence:

* A called at least 10 phones, and at least 10 phones called A

_**Q101:** Any person who owns at least 10 red vehicles_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q101.png)

_**Q102:** Any phone that was called from at least 2 phones; each of these 2 phones was called from at least one phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q102.png)

_**Q166:** Any **phone** that more than 5 ABC employees own a phone that called **it**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q166.png)

_**Q113:** Any person who knows at least 5 people with a birth data similar to his_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q113.png)

_**Q114:** Any person who owns more than 5 vehicles with the same color_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q114.png)

_**Q115:** Any person who owns more than 5 vehicles since the same date_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q115.png)

_**Q125:** Any phone that the number of phones it called is greater than the number of phones that called it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q125.png)

_**Q126:** Any phone that the number of phones it called is greater than the number of phones it didn't called_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q126.png)

_**Q63:** Any ABC employee who more than 5 ABC employees don't know_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q063.png)

_**Q65:** Any person who doesn't own more than 2 (things that his spouse owns)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q065.png)

_**Q64:** Any **phone** that less than 5 (phones owned by ABC employees) didn't call **it**_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q064.png)

_**Q165:** Any **phone** that less than 5 (ABC employees own a phone that didn't call **it**)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q165.png)

_**Q66:** Any person from whom more than 5 people are not within graph distance ≤ 6_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q066.png)

_**Q151:** Any person who owns more than 10 vehicles, at least one is Chinese. Only the Chinese vehicles will be returned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q151.png)

_**Q152:** Any person who owns more than 10 vehicles. Only the Chinese vehicles will be returned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q152.png)

## Aggregate Conditions #2 (L2C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-L2C.png)

todo

_**Q71:** Any phone that made more than 10 calls (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q071.png)

_**Q72:** Any phone that was called exactly 10 times (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q072.png)

_**Q73:** Any phone that made no more than 10 calls (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q073.png)

_**Q74:** Any phone that the number of times it was called (cumulatively) is not 10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q074.png)

_**Q79:** Any person with more than 5 paths (cumulatively) with length ≤ 4 to other persons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q079.png)

_**Q83:** Any phone that didn't call (0 outgoing calls)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q083.png)

_**Q84:** Any phone with no paths with length ≤ 3 to other phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q084.png)

_**Q104:** Any person who owned red vehicles at least 10 times (same or different vehicles)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q104.png)

_**Q105:** Any phone A that was called 2 exactly times (cumulatively) from (phones that each was called from at least one phone)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q105.png)

_**Q127:** Any phone that made more calls to phones owned by IBM employees, than to phones owned by Oracle employees_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q127.png)

## Aggregate Conditions #3 (LA3C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LA3C.png)

todo

_**Q87:** Any phone that was called at least once, and the cumulative incoming call duration is smaller than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q087.png)

_**Q88:** Any phone pair (A,B) where A called B at least once, but the cumulative call duration is 0 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q088.png)

_**Q89:** Any phone that its outgoing calls have more than 3 different durations_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q089.png)

## Aggregate Conditions #4 (LA4C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LA4C.png)

todo

_**Q116:** Any person who owns vehicles with no more than 3 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q116.png)

_**Q117:** Any person whose owned vehicles have an average model year greater than 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q117.png)

_**Q134:** Any person that the number of distinct colors of all vehicles owned by people he knows - is not more than 3_
Any person whose know

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q134.png)

_**Q135:** Any person that the average model year of all vehicles owned by people he knows - is at least 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q135.png)

_**Q137:** Any phone A that called phones B that made calls. The cumulative duration of all calls that all these B phones made is greater than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q137.png)

_**Q139:** Any person who owns vehicles with the same number of colors as the number of colors of the vehicles owned by his parents cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q139.png)

_**Q167:** Any person who owns things of at least 3 types_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q167.png)

## Aggregate Conditions #5 (D2C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-D2C.png)

todo

_**Q75:** Any phones pair (A, B) where B called A between 8 and 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q075.png)

_**Q76:** Any phone that called 052-333-4444 between 8 and 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q076.png)

## Aggregate Conditions #6 (DA3C)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-DA3C.png)

todo

_**Q86:** Any phones pair (A, B) where the cumulative call duration from A to B is greater than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q086.png)

## Aggregate Conditions before a Quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-Before-Quantifier1.png)

todo

_**Q121:** Any phone that called or SMSed at least 10 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q121.png)

_**Q122:** Any phone that SMSed phone B, and SMSed a phone that SMSed B - for at least 10 different B's_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q122.png)

_**Q175:** Any phone that called at least once, and SMSed at least once. The number of phones it called/SMSed is at least 10 (if a phone was both called and SMSed - it would be counted twice)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q175.png)

_**Q176:** Any phone that either (i) called at least one phone and SMSed at least one phone it didn't call. The number of phones it called/SMSes is at least 10 (ii) called at least 10 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q176.png)

_**Q123:** Any phone that either (was called) or (made a call) - at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q123.png)

_**Q124:** Any phone that either (called a phone) or (SMSed a phone that SMSed a phone) - at least 10 times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q124.png)

(counting the number of relationships to each entity on the right of the quantifier)

_**Q173:** Any phone that SMSed at least 2 phones - at least 10 times cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q173.png)

(counting the number of *distinct* SMS relationships)

_**Q174:** Any phone that either (i) called at least one phone and SMSed at least one phone it didn't call (ii) called at least two phones. If (i): the number of SMSes and calls is at least 10. otherwise: The number of calls is at least 10._

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q174.png)

## Aggregate Conditions both before and after a Quantifier

todo

_**Q191:** Any phone that called X≥3 phones and SMSed Y≥3 phones. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q191.png)

_**Q192:** Any phone that made X≥3 calls and sent Y≥3 SMSes. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q192.png)

_**Q193:** Any phone that called X≥3 phones, SMSed Y phones, and sent Z≥3 SMSes. X+Y≥10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q193.png)

_**Q194:** Any phone that made X calls, sent Y≥3 SMSes, and called Z≥3 phones. X+Y≥10__

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q194.png)

## Min/Max Aggregations

todo

## Min/Max Aggregations #1 (LRM1)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LRM1.png)

_et_ is a entity tag of a blue/red entity defined right of (but not directly right of) the aggregator. 

For each assignment to the entity on its left (←), this aggregation will limit the number of assignments to the entity on its right (→) - to those (→) with the minimal / maximal number of assignments to _et_. 

- Suppose the pattern is "5 → with max D" but there are only 3 →'s with >0 assignments to D - Only these 3 will be included.
- Suppose the pattern is "5 → with max D" but there are 10 →'s with identical max number of assignments to D - all 10 will be included.

- LRM1 may appear below a relationship / path. The relationship / path may be wrapped by an '↛' or an 'L'
- LRM1 may not appear directly before a quantifier
- Except for '&' quantifer - LRM1 aggregator cannot start a quantifier's branch.

todo

_**Q196:** Any **phone** owned by Lior Kogan, and the 3 phones **it** called that called the largest number of phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q196.png)

_**Q197:** Any person and his 3 phones that the phones they called - called the largest number of distinct phones cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q197.png)

_**Q198:** Any person and his 3 phones that (for each of them: the 4 phones it called that called the largest number of phones) - called the largest number of distinct phones cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q198.png)

## Prefix Min/Max Aggregations #1 (PRM1)

_et_ is a entity tag of a blue/red entity defined right of (but not directly right of) the aggregator. 

- PRM1 cannot be used when the pattern starts with a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-PRM1.png)

_**Q67:** The 3 people with the maximal number of parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q067.png)

_**Q68:** The 2 phones that were called from the largest number of phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q068.png)

_**Q69:** The 2 things that own the largest number of things_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q069.png)

_**Q70:** The 5 people that the number of people within graph distance ≤ 4 from them - is the smallest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q070.png)

## Min/Max Aggregations #2 (LRM2)

- LRM2 may appear below a relationship / path. The relationship / path may be wrapped by an 'L'.
- LRM2 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LRM2.png)

todo

_**Q195:** Any phone owned by Lior Kogan, and the 3 phones it called the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q195.png)

## Prefix Min/Max Aggregations #2 (PRM2)

- PRM1 cannot be used when the pattern starts with a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-PRM2.png)

todo

_**Q171:** The 2 phones that were called the largest number of times_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q171.png)

_**Q172:** The 5 people with the smallest number of paths with length ≤ 4 to some person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q172.png)

## Min/Max Aggregations #3 (LRMA3)

- LRMA3 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LRMA3.png)

todo

_**Q182:** Any phone owned by Lior Kogan, and the 3 phones with the largest cumulative call duration from it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q182.png)

_**Q201:** For any phone that called at least 10 phones: the 3 phones with the largest cumulative call duration from it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q201.png)

## Min/Max Aggregations #4 (LRM4)

- LRM4 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LRM4.png)

todo

_**Q118:** Any person and his 3 eldest offsprings_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q118.png)

_**Q119:** Any person and his 3 youngest sons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q119.png)

## Prefix Min/Max Aggregations #4 (PRM4)

- PRM1 cannot be used when the pattern starts with a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-PRM4.png)

todo

_**Q130:** The 4 eldest people_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q130.png)

_**Q131:** The 4 eldest males_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q131.png)

_**Q91:** The 4 phones with the longest (shortest incoming call)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q091.png)

_**Q92:** The 4 phones with the longest (outgoing calls average duration)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q092.png)

_**Q132:** The 4 people who own vehicles with the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q132.png)

_**Q133:** The 4 people that the average model year of their vehicles is maximal_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q133.png)

_**Q138:** The 4 people that the people they know cumulatively own vehicles with the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q138.png)

_**183:** The 3 phones with the largest cumulative call duration from phones owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q183.png)

_**Q168:** The 3 people that the number of types of things they own is maximal_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q168.png)

## Min/Max Aggregations #5 (DM2)

- DM2 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-DM2.png)

todo

_**Q77:** The 5 phone pairs (A, B) with the largest number of calls from B to A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q077.png)

_**Q78:** The 4 phones that called the largest number of times to 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q078.png)

_**Q80:** The 3 person pairs with the largest number of paths with length ≤ 4 between them_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q080.png)

## Min/Max Aggregations #6 (DMA3)

- DMA3 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-DMA3.png)

todo

_**Q90:** The 4 phone pairs (A, B) with the maximal cumulative call duration from A to B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q090.png)

## Min/Max Aggregations #7 (LDM3)

- LDM3 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-LDM3.png)

todo

_**Q161:** For any phone: the 4 longest outgoing calls_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q161.png)

## Min/Max Aggregations #8 (DDM3)

- DDM3 may not appear directly before a quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Agg-DDM3.png)

todo

_**Q160:** For any phones pair (A, B): The 4 longest calls from A to B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q160.png)

## Aggregation Chains

todo

_**Q96:** Any phone that received more than 10 calls that are shorter than 10 minutes after 1/1/2010 from 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q096.png)

_**Q93:** Any phone that called (> 10 minutes calls) to at least 3 phones. The number of calls to each phone is at least 5_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q093.png)

Filtering stages:

- Pass only (> 10 minutes calls)
- Pass only phones A that called (> 10 minutes calls) to at least 3 phones
- Pass only phone pairs (A, B) where A made at least 5 (> 10 minutes calls) to B_

_**Q94:** Any phone that called at least 5 (> 10 minutes calls). The calls were made to at least 3 phones_

Filtering stages:

- Pass only (> 10 minutes calls)
- Pass phone pairs (A, B) where A made at least 5 (> 10 minutes calls) to B
- Pass only phones A that called (> 10 minutes calls) to at least 3 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q094.png)

_**Q162:** Any phones pair (A, B) where the second shortest call from A to B is longer than 60 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q162.png)

_**Q163:** Any phone that the average duration of its 10 shortest outgoing calls is greater than 60 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q163.png)

_**Q185:** Any phone that received calls from 052-333-4444: at least one call after 1/1/2010, at least one call shorter than 10 minutes, more than 10 calls (after 1/1/2010 or shorter than 10 minutes)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q185.png)

_**Q95:** Any phone that received more than 5 (> 10 minutes calls) with a total duration of > 100 minutes from 052-333-4444_ (3 versions)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q095-1.png)

The two 'per pair' conditions could be chained instead. The meaning would be similar:

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q095-2.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q095-3.png)

_**Q97:** Any phone that called more than 3 phones. For each callee, more than 10 calls, or calls with total duration of more than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q097.png)

_**Q98:** First, pass phones A that called more than 3 phones. Then, pass phone pairs (A,B) where A called B more than 10 calls, or calls with a total duration of > 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q098.png)

_**Q186:** Any phone to that 052-333-4444 called more than 10 calls of less than 10 minutes, and at least one call of 10 minutes or more_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q186.png)

_**Q99:** Any phone to that 052-333-4444 called more than 10 calls of less than 10 minutes, and no calls of 10 minutes or more_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q099.png)

_**Q100:** Any phone to which 052-333-444 called with more then 10 (< 10 minutes calls), more than 10 calls after 1/1/2010, more than 15 (< 10 minutes calls) after 1/1/2010, and more than 100 calls_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q100.png)

_**Q199:** Any phone that (called more than 10 times to each of more than 10 phones) and (called more than 20 times to less than 10 phones)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q199.png)

_**Q200:** Any phone that (called more than 10 times to each of more than 10 phones. For each of these 10 phones - the calls have exactly 2 distinct durations) and (called more than 20 times to less than 10 phones. The average call duration with all these phones - is greater than 3 minutes)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q200.png)

_**Q120:** Any person for whom the sum of the heights of his 3 eldest offsprings is less than his own height_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q120.png)

_**Q140:** Any person who his 3 eldest sons cumulatively own vehicles with 3 colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q140.png)

_**Q141:** Any person who his 3 eldest sons cumulatively own vehicles with the same number of colors as his 3 younger daughters cumulatively_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q141.png)

## Aggregation Sequences

todo

_**Q128:** Any person and his 3 offspring that own vehicles with the largest number of colors_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q128.png)

_**Q103:** Any phone A that called at least 3 phones that each of them was called from at least 4 phones other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q103.png)

_**Q106:** Any phone A that made at least 3 calls (cumulatively) to phones that each of them received at least 4 calls (cumulatively) from phones other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q106.png)

_**Q107:** Any phone that (the number of phones owned by 5 people each that called it) is 5, and that the number of calls received from those phones (cumulatively) is not 5_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q107.png)

_**Q169:** Any person that (each of his offsprings who owns at least 3 vehicles - owns vehicles with at least 3 colors)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q169.png)

_**Q129:** Any person that (each of his offsprings who owns at least one vehicle - owns a different number of vehicles)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q129.png)

_**Q181:** Any **phone** with no intersection between the groups of phones called by any two phones **it** called_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q181.png)

_**Q164:** Any phone that the cumulative number of outgoing calls of the phones it called - is equal to the cumulative number of outgoing SMSes of the phones it SMSed_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q164.png)

_**Q179:** Any phones pair (A, B) where A's cumulative call duration to B is greater than B's cumulative outgoing call duration (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q179-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q179-2.png)

_**Q180:** Any phones pair (A, B) where the cumulative call duration between A and B is greater than A's cumulative outgoing call duration, and greater than B's cumulative outgoing call duration_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q180.png)

## Splits

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

## Split before a Quantifier

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB10.png)

todo

_**Q158:** Any phone that in at least 10 days - the number of phones it called is greater than the number of phones that called it_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q158.png)

## Split Tag

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/BB11.png)

todo: split tag, split tag's scope

_**Q159:** Any **phone** for which there are more days where (the number of phones **it** called is greater than the number of phones that called **it**) than days where (the number of phones that called **it** is greater than the number of phones **it** called)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q159.png)

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

**R11:** Any _"... n → ..."_ aggregation (L1C, LRM1, PRM1, LRM2, PRM2, LRMA3, LRM4 and PRM4) is illegal when the entity on the right is concrete (yellow).

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Agg01.png)

## Other Rules

**R12:** Properties of concrete entities can't have conditions.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Illegal-Tag11.png)
