# V1 - A visual query language for schema-based property graphs

## The property graph data model

A [*graph*](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)) is an ordered pair G = (V, E) comprising a set V of vertices (nodes) together with a set E of edges (arcs), which are 2-element subsets of V.

A **property graph** (AKA attributed graph) is a graph where

- Graph vertices represent entities
  An entity is an objects or ‘thing’ in our mini-world, with an independent existence and which is distinguishable from other objects (e.g. a person, a car, a product)
- Graph edges represent relationships between pairs of entities (e.g. 'owns', 'friend of'). Usually all edges are directional.
- Each vertex has a set of descriptive features, called properties (attributes) (e.g. 'First Name', 'Last Name' for a person)
- Each edge has a set of properties as well
- Each property has a name (string), a value type (e.g. string  / integer), and a value (e.g. "First_Name": string = "Lior")
- Usually each vertex has a type (e.g. Person, Phone, Vehicle), but in a schema-free graph - the type is just another property
- Edges have types as well (e.g. 'owns', 'call'). Again - without schema - the type is just another property

More about property graphs can be found [here](http://tinkerpop.apache.org/docs/current/reference/) and [here](https://neo4j.com/developer/graph-database/).

A **property graph's schema** is defined by

- A set of entity types
  Entities with the same basic properties are grouped (typed) into an entity type (e.g. Person, Car, Product)
  For each entity type: 
  - A name 
  - A set of properties. For each property: name (key) and value type
- A set of relationship types. For each relationship type:
  - A name
  - A set of properties. For each property: name (key) and value type
  - A set of pairs of entity types for which the relationship type holds (e.g. Person owns Phone; Person owns Car)

There is no standard way to define property graph schemas. Implementations may vary in many aspects: the properties' data types (basic types, categorical, multivalued, composite, nested) and supported operators, the relationship types supported directionality (unidirectional, bidirectional, mixed), constraints (mandatory attributes, relationships cardinality, etc.), entity type hierarchies, relationship type hierarchies, and more.

A **schema-based property graph** is a property graph that conforms to a given schema.

**Why do we need a schema?**

Schema-free property graphs do not define nor enforce entity-types nor relationships-types; each vertex and each edge may contain attributes with any name and any value type. Without schema we can’t enforce integrity. Without integrity there are no formal building-blocks for representing patterns.

In order to ask and answer queries such as *“Any person that owns a red car”* we need first to:
- Define 'Person' and 'Car' entity types
- Ensure that each Person and Car entities are of the relevant entity types
- Define 'owns' relationship type
- Defines that the 'owns' relationship type holds between a Person and a Car
- Define a 'Color' property for the Car entity type
- Define that Color holds a string, or better, define a categorical data type

## Patterns and pattern languages

A Pattern defines a structure of a sub-graph in a schema-based property graph. Here is an example:

*“Any person that owns a blue car, his age is between 40 and 50, his cell-phone number ends with “156”, and he has a brother that called 5 or more phones belonging to employees of company X in the last month"*

A pattern can be viewed as a query that can be executed against a graph database (similar to SELECT statement in SQL). The answer to such query is the [union of the] set of all the sub-graphs that conforms to the pattern's structure.

A Pattern language defines the syntax for expressing patterns.

Pattern languages differs in the following anpects:
- Genericity - generic (e.g. schema-driven) or domain-specific
- Representation - textual (structured / controlled Natural) or visual (AKA graphical, diagrammatic)
- Expressivity - The richness of the patterns that can be expressed, The expressivity is always limited (in a Gödelian sense)
- Simplicity -  both for constructing new patterns and for understanding existing patterns
- Efficiency - to parse and to find patterns

## The V1 Pattern language

Many potential users won't use textual query languages. The learning curve may be too sharp for someone with no perior experience in programming. Even users that use textual query languages may spend a lot of time on the technicalities. 

Users want to be able to pose complex queries in a manner that is coherent with the way they think. They want to do it with minimal technical training and minimal effort. The ability to express patterns in a way that is aligned with their mental processes is crucial to the flow of the work, and to the value of the insights that can be deduced.

Visual query languages are very attractive. They have the potential to be more 'user friendly', in the sense that patterns may be constructed and understand much more quickly, with much less mental effort. A long-standing challenge is to design a visual query language that is generic, simple, and has rich expressivity. This is what V1 aims to be. 

V1 is a rich simple generic visual pattern language for schema-based property graphs. It is named after the [primary visual cortex in our brain](https://en.wikipedia.org/wiki/Visual_cortex), which is also known as Visual area one (V1).





Q1: Any phone that Lior Kogan owns

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q001.png)

Q2: Any phone, that was called by a phone that Lior Kogan owns

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q002.png)

Q3: Any person that owns a phone, and his first name is Lior (two versions)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-2.png)

Q4: Any person that call was made to his phone from a phone owned by at least one of his parents

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q004.png)

Q5: Any person that call was made to his phone from a phone owned by two of his parents

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q005.png)

Q6: Any person that calls were made to his phone from two phones – one owned by one of his parents, the other owned by his other parent (note that one or both phones may be owner by both his parents)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q006.png)

Q7: Any person that either a call was made to his phone from a phone owned by two of his parents, or from two phones – one owned by one of his parents and the other owned by his other parent

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q007.png)

Q8: Any person that was born before 1970 and died, or that his father was born before or at 1/1/1950

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q008.png)

Q9: Any pair of phones A, B were A called B both in 1980 and in 1984

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q009.png)

Q10: Any person that his first name is Lior, that owns some phone B which called a phone C that belongs to an offspring of James Smith and called a phone that belongs either to John Price or to George Davis. At least one call from B to C was longer than 100 seconds, and took place in or after 2010

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q010.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q011.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q012.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q013.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q014.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q015.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q016.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q017.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q018.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q019.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q022.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q023.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q024.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-3.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-4.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q027.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q028.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q029-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q029-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q030-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q030-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q031.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q032.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q033.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q034.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q035.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q036.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q037.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q038.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q039.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q040.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q041.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q042.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q043.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q044.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q045.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q046.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q047.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q048.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q049.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q050.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q051.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q052.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q053.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q054.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q055.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q056.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q057.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q058.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q059.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q060.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q061.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q062.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q063.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q064.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q065.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q066.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q067.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q068.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q069.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q070.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q071.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q072.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q073.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q074.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q075.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q076.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q077.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q078.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q079.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q080.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q081.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q082.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q083.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q084.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q085.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q086.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q087.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q088.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q089.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q090.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q091.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q092.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q093.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q094.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q095.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q096.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q097.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q098.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q099.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q100.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q101.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q102.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q103.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q104.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q105.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q106.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q107.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q108.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q109.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q110.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q111.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q112.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q113.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q114.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q115.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q116.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q117.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q118.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q119.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q120.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q121.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q122.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q123.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q124.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q125.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q126.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q127.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q128.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q129.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q130.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q131.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q132.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q133.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q134.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q135.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q136.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q137.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q138.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q139.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q140.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q141.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q142.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q143.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q144.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q145.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q146.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q147.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q148.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q149.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q150.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q151.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q152.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q153.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q154.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q155.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q156.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q157.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q158.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q159.png)
