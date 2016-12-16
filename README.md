<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-89133312-1', 'auto');
  ga('send', 'pageview');

</script>

Copyright (c) 2017 Lior Kogan.
This work is provided under the [CC BY-NC-SA 4.0 licence](https://creativecommons.org/licenses/by-nc-sa/4.0/). For other uses please contact the author.

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

* A set of entity types
  Entities with the same basic properties are grouped (typed) into an entity type (e.g. Person, Car, Product)
  For each entity type: 
  * A name 
  * A set of properties. For each property: name (key) and value type
* A set of relationship types. For each relationship type:
  * A name
  * A set of properties. For each property: name (key) and value type
  * A set of pairs of entity types for which the relationship type holds (e.g. Person owns Phone; Person owns Car)

There is no standard way to define property graph schemas. Implementations may vary in many aspects: the properties' data types (basic types, categorical, multivalued, composite, nested) and supported operators, the relationship types supported directionality (unidirectional, bidirectional, mixed), constraints (mandatory attributes, relationships cardinality, etc.), entity type hierarchies, relationship type hierarchies, and more.

A **schema-based property graph** is a property graph that conforms to a given schema.

**Why do we need a schema?**

Schema-free property graphs do not define nor enforce entity-types nor relationships-types; each vertex and each edge may contain attributes with any name and any value type. Without schema we can’t enforce integrity. Without integrity there are no formal building-blocks for representing patterns.

In order to ask and answer queries such as *“Any person that owns a red car”* we need first to:
* Define 'Person' and 'Car' entity types
* Ensure that each Person and Car entities are of the relevant entity types
* Define 'owns' relationship type
* Defines that the 'owns' relationship type holds between a Person and a Car
* Define a 'Color' property for the Car entity type
* Define that Color holds a string, or better, define a categorical data type

## Patterns and pattern languages

A Pattern defines a structure of a sub-graph in a schema-based property graph. Here is an example:

*“Any person that owns a blue car, his age is between 40 and 50, his cell-phone number ends with “156”, and has a brother that called 5 or more phones belonging to employees of company X in the last month"*

A pattern can be viewed as a query that can be executed against a graph database (similar to SELECT statement in SQL). The answer to such query is the [union of the] set of all the sub-graphs that conforms to the pattern's structure.

A Pattern language defines the syntax for expressing patterns.

Pattern languages differs in the following anpects:
* Genericity - generic (e.g. schema-driven) or domain-specific
* Representation - textual (structured / controlled Natural) or visual (AKA graphical, diagrammatic)
* Expressivity - The richness of the patterns that can be expressed, The expressivity is always limited (in a Gödelian sense)
* Simplicity -  both for constructing new patterns and for understanding existing patterns
* Efficiency - to parse and to find patterns

## The V1 pattern language

Many potential users won't use textual query languages. The learning curve may be too sharp for someone with no perior experience in programming. Even users that use textual query languages may spend a lot of time on the technicalities. 

Users want to be able to pose complex queries in a manner that is coherent with the way they think. They want to do it with minimal technical training and minimal effort. The ability to express patterns in a way that is aligned with their mental processes is crucial to the flow of their work, and to the value of the insights they can deduce.

Visual query languages are very attractive. They have the potential to be more 'user friendly', in the sense that patterns may be constructed and understand much more quickly, with much less mental effort. A long-standing challenge is to design a visual query language that is generic, simple, and has rich expressivity. This is what V1 aims to be. 

V1 is a rich simple generic visual pattern language for schema-based property graphs. It is named after the [primary visual cortex in our brain](https://en.wikipedia.org/wiki/Visual_cortex), which is also known as Visual area one (V1).




_**Q1:** Any phone that Lior Kogan owns_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q001.png)

_**Q2:** Any phone, that was called by a phone that Lior Kogan owns_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q002.png)

_**Q3:** Any person that owns a phone, and his first name is Lior (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-1.png)

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q003-2.png)

_**Q4:** Any person that a call was made to his phone from a phone owned by at least one of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q004.png)

_**Q5:** Any person that a call was made to his phone from a phone owned by two of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q005.png)

_**Q6:** Any person that calls were made to his phone from two phones – one owned by one of his parents, the other owned by his other parent (note that one or both phones may be owner by both his parents)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q006.png)

_**Q7:** Any person that either a call was made to his phone from a phone owned by two of his parents, or from two phones – one owned by one of his parents and the other owned by his other parent_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q007.png)

_**Q8:** Any person that was born before 1970 and died, or that his father was born before or at 1/1/1950_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q008.png)

_**Q9:** Any pair of phones A, B were A called B both in 1980 and in 1984_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q009.png)

_**Q10:** Any person that his first name is Lior, that owns some phone B which called a phone C that belongs to an offspring of James Smith and called a phone that belongs either to John Price or to George Davis. At least one call from B to C was longer than 100 seconds, and took place in or after 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q010.png)

_**Q11:** Any current employee of IBM that, since 2011 or later, knows someone that left Oracle or Microsoft on or after June 2010_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q011.png)

_**Q12:** Any person that doesn't own a vehicle_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q012.png)

_**Q13:** Any vehicle that is not owned by a person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q013.png)

_**Q14:** Get vehicle 34-234-99, if not owned by a person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q014.png)

_**Q15:** Get Lior Kogan, if he doesn't own a vehicle_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q015.png)

_**Q16:** Any person that doesn't own vehicle 34-234-99_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q016.png)

_**Q17:** Any vehicle that is not owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q017.png)

_**Q18:** Get Lior Kogan, if he doesn't own vehicle 34-234-99_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q018.png)

_**Q19:** Get Vehicle 34-234-99, if it is not owned by Lior Kogan_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q019.png)

_**Q20:** Any vehicle that is not owned by James Smith nor by John Price (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q020-2.png)

_**Q21:** Any vehicle that is not owned by both James Smith and John Price(two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q021-2.png)

_**Q22:** Any vehicle that is not owned by a person that owns a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q022.png)

_**Q23:** Any vehicle that is not owned by a person that doesn't own a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q023.png)

_**Q24:** Any person that has (at least) two parents and owns a phone that was called from a phone that is not owned by either of his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q024.png)

_**Q25:** Any of my friend's friends that isn't my friend(two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q025-2.png)

_**Q26:** Any book, liked by people that like some book that I like, that I don't like(four versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-2.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-3.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q026-4.png)

_**Q27:** Any person with a Chinese citizenship that owns a vehicle and a phone of the same origin; Any company registered in Japan that owns a vehicle and a phone of the same origin_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q027.png)

_**Q28:** Any Chinese citizen that owns a red vehicle; Any person that doesn't know someone who owns a red vehicle_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q028.png)

_**Q29:** Any phone that called or SMSed some phone (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q029-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q029-2.png)

_**Q30:** Any pair of phones A, B where A both called and SMSed B (two versions)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q030-1.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q030-2.png)

_**Q31:** Any pair of phones A, B where A called B, A SMSed B, B called A, and B SMSed A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q031.png)

_**Q32:** Any pair of phones A, B where A SMSed B, and A SMSed some phone that SMSed B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q032.png)

_**Q33:** Any phone A that called some phone B, called some phone that called B, and SMSed some phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q033.png)

_**Q34:** Any phone A that called some phone B, called some phone that called B, SMSed some phone D and SMSed some phoned that SMSed D (B and D may be the same phone or different phones)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q034.png)

_**Q35:** Any person that either (i) knows a Chinese citizen that owns a red vehicle (ii) know a person that doesn't know someone that owns a red vehicle (iii) know someone that doesn't own a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q035.png)

_**Q36:** Any person that owns something_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q036.png)

_**Q37:** Any person that owns a vehicle or a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q037.png)

_**Q38:** Any person that owns something which is not a car or a phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q038.png)

_**Q39:** Anything that can own something, but doesn't_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q039.png)

_**Q40:** Any phone that have no owner_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q040.png)

_**Q41:** Anything that can be owned, and is not owned_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q041.png)

_**Q42:** Anything that can own something, but owns nothing_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q042.png)

_**Q43:** Any phone that all of its owners (if any) are persons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q043.png)

_**Q44:** Any path with length ≤ 4 between these two phones, which is composed only of 'call' relationships_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q044.png)

_**Q45:** Any path with length ≤ 4 between these two phones, which is composed only of 'call' and 'SMS' relationships_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q045.png)

_**Q46:** Any path with length ≤ 4 between a phone owned by James Smith to a phone owned by John Price, which is composed of up to 2 'call' relationships, and only of 'phone' entities.

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q046.png)

_**Q47:** All shortest paths between these two phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q047.png)

_**Q48:** All shortest paths between these two phones, which are not composed of 'call' relationships and 'phone' entities_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q048.png)

_**Q49:** Any 3 phones with circular call pattern, and their owners_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q049.png)

_**Q50:** Any person that own (at least) two things of the same type_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q050.png)

_**Q51:** Any person that own (at least) two things of different types_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q051.png)

_**Q52:** Any person that own (at least) two things of different types, both are not cars_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q052.png)

_**Q53:** Any person within graph distance ≤ 4 from James Smith_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q053.png)

_**Q54:** Any person within graph distance ≤ 3 from James Smith, John Price and George Davis_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q054.png)

_**Q55:** Anything within graph distance ≤ 3 from James Smith, John Price and George Davis_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q055.png)

_**Q56, Q57, Q58:** limitations on the count and types of the sub-paths that a path may be composed of_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q056.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q057.png)
![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q058.png)

_**Q59:** Any person having more than 2 parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q059.png)

_**Q60:** Any phone that received calls from exactly 5 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q060.png)

_**Q61:** Anything that owns more than 2 things_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q061.png)

_**Q62:** Any person that is within graph distance ≤ 4 from more than 5 persons_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q062.png)

_**Q63:** Any person that more than 2 persons are not his parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q063.png)

_**Q64:** Any phone that number of phone that didn't call him is 5_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q064.png)

_**Q65:** Anything that that number of things it doesn't own is greater than 2_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q065.png)

_**Q66:** Any person that the number of persons that are not within graph distance ≤ 4 from him is 5_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q066.png)

_**Q67:** The 3 persons with the maximal number of parents_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q067.png)

_**Q68:** The 2 phones that were called from the largest number of phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q068.png)

_**Q69:** The 2 things that own the largest number of things_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q069.png)

_**Q70:** The 5 persons, that the number of persons within graph distance ≤ 4 from them is the smallest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q070.png)

_**Q71:** Any phone that made more that 10 calls (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q071.png)

_**Q72:** Any phone that received exactly 10 calls (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q072.png)

_**Q73:** Any phone that made no more than 10 calls (cumulatively)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q073.png)

_**Q74:** Any phone that the number of calls it received (cumulatively) is not 10_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q074.png)

_**Q75:** Any pair of phones A, B where A received between 8 to 10 calls from B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q075.png)

_**Q76:** Any phone that called between 8 to 10 times to 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q076.png)

_**Q77:** The 5 phone pairs A, B where the number of calls from B to A is the largest_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q077.png)

_**Q78:** The 4 phones that called the largest number of times to 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q078.png)

_**Q79:** Any person with more than 5 paths (cumulatively) with length ≤ 4 to some person_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q079.png)

_**Q80:** The 3 person pairs with the largest number of paths with length ≤ 4 between them_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q080.png)

_**Q81:** Any phone that didn't call (0 callees)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q081.png)

_**Q82:** Any phone that wasn't called (0 callers)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q082.png)

_**Q83:** Any phone that didn't call (0 outgoing calls)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q083.png)

_**Q84:** Any phone with no paths with length ≤ 3 to other phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q084.png)

_**Q85:** Any phone that called at least 10 phones, and received calls from at least 10 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q085.png)

_**Q86:** Any phones pair A, B where the call from A to B have a cumulative length of more than 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q086.png)

_**Q87:** Any phone that was called at least once, where the cumulative calls duration is less that 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q087.png)

_**Q88:** Any phone pair A,B where A called B at least once, but the cumulative calls duration is 0 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q088.png)

_**Q89:** Any phone that its outgoing calls have more than 3 different durations_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q089.png)

_**Q90:** The 4 phone pairs A, B with the maximal cumulative calls duration from A to B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q090.png)

_**Q91:** The 4 phones with the longest (shortest incoming call)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q091.png)

_**Q92:** The 4 phones with the longest (outgoing calls average duration)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q092.png)

_**Q93:** First pass only (> 10 minutes calls). Then pass only phones A that called (> 10 minutes calls) to at least 3 phones. For these, pass only phone pairs A, B where A made at least 5 (> 10 minutes calls) to B_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q093.png)

_**Q94:** First pass only (> 10 minutes calls). Then pass phone pairs A, B where A made at least 5 (> 10 minutes calls) to B. From these A's pass only those that called (> 10 minutes calls) to at least 3 phones_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q094.png)

_**Q95:** Any phone that received at least 5 (> 10 minutes calls) with a total duration of > 100 minutes from 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q095.png)

_**Q96:** Any phone that received at least 10 (< 10 minutes calls) after 1/1/2010 from 052-333-4444_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q096.png)

_**Q97:** Any phone that called at least 3 phones. For each callee, more than 10 calls, or calls with total duration of > 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q097.png)

_**Q98:** First pass phones A that called at least 3 phones. The pass phone pairs A,B where A called B more than 10 calls, or calls with a total duration of > 100 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q098.png)

_**Q99:** Any phone to which 052-333-4444 called more than 10 calls of < 10 minutes, and no calls of ≥ 10 minutes_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q099.png)

_**Q100:** Any phone to which 052-333-444 called with more then 10 (< 10 minutes calls), more than 10 calls after 1/1/2010, more than 15 (< 10 minutes calls) after 1/1/2010, and more than 100 calls_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q100.png)

_**Q101:** Any person than owns at least 10 red cars_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q101.png)

_**Q102:** Any phone that received calls from at least 2 phones that each of them received calls from at least one phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q102.png)

_**Q103:** Any phone A that called at least 3 phones that each of them was called from at least 4 phones other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q103.png)

_**Q104:** Any person that owned red cars at least 10 times (same of different cars)_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q104.png)

_**Q105:** Any phone that received 2 calls (cumulatively) from phones that each of them was called from at least one phone_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q105.png)

_**Q106:** Any phone A that made at least 3 calls (cumulatively) to phones that each of them received at least 4 calls (cumulatively) from phones other than A_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q106.png)

_**Q107:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q107.png)

_**Q108:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q108.png)

_**Q109:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q109.png)

_**Q110:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q110.png)

_**Q111:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q111.png)

_**Q112:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q112.png)

_**Q113:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q113.png)

_**Q114:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q114.png)

_**Q115:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q115.png)

_**Q116:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q116.png)

_**Q117:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q117.png)

_**Q118:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q118.png)

_**Q119:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q119.png)

_**Q120:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q120.png)

_**Q121:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q121.png)

_**Q122:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q122.png)

_**Q123:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q123.png)

_**Q124:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q124.png)

_**Q125:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q125.png)

_**Q126:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q126.png)

_**Q127:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q127.png)

_**Q128:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q128.png)

_**Q129:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q129.png)

_**Q130:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q130.png)

_**Q131:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q131.png)

_**Q132:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q132.png)

_**Q133:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q133.png)

_**Q134:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q134.png)

_**Q135:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q135.png)

_**Q136:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q136.png)

_**Q137:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q137.png)

_**Q138:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q138.png)

_**Q139:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q139.png)

_**Q140:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q140.png)

_**Q141:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q141.png)

_**Q142:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q142.png)

_**Q143:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q143.png)

_**Q144:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q144.png)

_**Q145:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q145.png)

_**Q146:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q146.png)

_**Q147:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q147.png)

_**Q148:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q148.png)

_**Q149:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q149.png)

_**Q150:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q150.png)

_**Q151:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q151.png)

_**Q152:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q152.png)

_**Q153:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q153.png)

_**Q154:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q154.png)

_**Q155:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q155.png)

_**Q156:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q156.png)

_**Q157:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q157.png)

_**Q158:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q158.png)

_**Q159:** Any_

![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Pictures/Q159.png)
