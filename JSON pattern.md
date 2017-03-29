## V1 Patterns - JSON-based Representation

## V1 Element types

| JSON type  | V1 element type         | visual representation
|------------|-------------------------|----------------------
| Start      | Query Start             | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element01.png)
| EConcrete  | Concrete Entity         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element02.png)
| ETyped     | Typed Entity            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element03.png)
| EUntyped   | Untyped Entity          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element04.png)
| EAgg       | Aggregate Entity        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element05.png)
| ELog       | Logical Entity          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element06.png)
| Rel        | Relationship            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element07.png)
| EProp      | Entity's Property       | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element08.png)
| RelProp    | Relationship's Property | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element09.png)
| Quant1     | Quantifier 1            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element10.png)
| Quant2     | Quantifier 2            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element11.png)
| Comb       | Combiner                | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element13.png)
| Path       | Path                    | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element14.png)
| PathSeg    | Path Segment            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element15.png)
| HQuant     | Horizontal Quantifier   | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element16.png)
| HComb      | Horizontal Combiner     | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element17.png)
| SplitBy    | Split By                | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element18.png)
| SplitsCon  | Splits Constraint       | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element19.png)
| AggL1      | L1 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element31.png)
| AggL2      | L2 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element32.png)
| AggL3      | L3 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element33.png)
| AggL4      | L4 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element34.png)
| AggM1      | M1 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element41.png)
| AggM2      | M2 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element42.png)
| AggM3      | M3 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element43.png)
| AggM4      | M4 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element44.png)
| AggR1      | R1 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element45.png)
| AggP1      | P1 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element51.png)
| AggP2      | P2 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element52.png)
| AggP3      | P3 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element53.png)
| AggP4      | P4 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element54.png)

## Wrappers for relationships / paths

**N** - No-existance  ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperX.png)
**X** - No-connection ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperN.png)
**L** - Latent        ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperL.png)
**O** - Optional      ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperO.png)

## JSON structure

|Mandatory| Name         | Type   | Description
|---------|--------------|--------| ------
| +       | ont          | string | Ontology name
| +       | name         | string | Query name
| +       | elements     | [...]  | Elements composing the query
|         | nonidentical | [...]  | nonidenticality constraints between entity tags

**Elements: for each query element:**

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eNum      | int    | Element number. Distinct value for each element
| +       | type      | string | JSON element type

**Additional JSON elements for each V1 element type:**

## Query Start (type = "Start")

There must be a single element with type "Start". Its ENum must equals to 0.

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | next      | int    | ENum of next element. <br> Valid element types: EConcrete, ETyped, EUntyped, EAgg, ELog, Quant2
|         | b         | int    | ENum of element below. <br> Valid element types: SplitBy

## Concrete Entity (type = "EConcrete")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
| +       | eID       | string | Technical ID of the entity
| +       | eType     | int    | Entity type (e.g. of 'Person') <br> According to the ontology
| +       | eName     | string | Display name of the entity (e.g. "Brandon Stark"). It is better to read the name from the database. This  was the display name when the pattern was stored
|         | next      | int    | ENum of next element. <br> Valid element types: Rel, EProp, Quant1, Path

## Typed Entity (type = "ETyped")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
| +       | eType     | int    | Entity type (e.g. of 'Person') <br> According to the ontology
|         | next      | int    | ENum of next element.  <br> Valid element types: Rel, EProp, Quant1, Path

## Untyped Entity (type = "EUntyped")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
|         | vTypes    | [int]  | Valid entity types <br> According to the ontology <br> VTypes and NVTypes can't be both present
|         | nVTypes   | [int]  | Invalid entity types <br> According to the ontology <br> VTypes and NVTypes can't be both present
|         | next      | int    | ENum of next element. <br> Valid element types: Rel, EProp, Quant1, Path

## Aggregate Entity (type = "EAgg")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
| +       | fName     | string | file name, where defined
| +       | eName     | string | name - as defined in file
|         | next      | int    | ENum of next element. <br> Valid element types: Rel, EProp, Quant1, Path

## Logical Entity (type = "ELog")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
| +       | fName     | string | file name, where defined
| +       | eName     | string | name - as defined in file
|         | next      | int    | ENum of next element.  <br> Valid element types: Rel, EProp, Quant1, Path

## Relationship (type = "Rel")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | rType     | int    | Relationship type (e.g. of 'own') <br> According to the ontology
| +       | dir       | string | "-": non-directional, "R": Arrow pointing right, "L": Arrow pointing left
|         | wrapper   | string | "X": no-existance, "N": no-connection, "L": Latent, "O": Optional
|         | next      | int    | ENum of next element. <br> Valid element types: EConcrete, ETyped, EUntyped, EAgg, ELog, Quant2, Comb
|         | b         | int    | ENum of element below. <br> Valid element types: <ul><li>RelProp</li> <li>HQuant</li> <li>AggL1 (valid wrappers: NL)</li> <li>AggL2 (valid wrappers: L)</li> <li>AggL3 (valid wrappers: L)</li> <li>AggL4 (valid wrappers: NL)</li> <li>AggM1 (valid wrappers: NL)</li> <li>AggM2(valid wrappers: L)</li> <li>AggM3 (valid wrappers: L)</li> <li>AggM4 (valid wrappers: L)</li> <li>AggR1 (no valid wrappers)</li> <li>SplitBy (valid wrappers: NL)</li></ul> 

## Entity's Property (type = "EProp") 

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | pType     | string | Property type (e.g. "2" for "last name" of "Person") <br> According to the ontology
| +       | pTag      | string | Property tag to assign to property / to property.f (e.g. "1")
|         | f         | string | Function to apply to property
|         | con       | {...}  | Constraint. see below

## Relationship's Property (type = "RelProp")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | pType     | string | Property type (e.g. "1.2" for "tf.till" of "member of") <br> According to the ontology
| +       | pTag      | string | Property tag to assign to property / to property.f (e.g. "1")
|         | f         | string | Function to apply to property
|         | con       | {...}  | Constraint. see below
|         | b         | int    | ENum of element below. <br> Valid element types: <ul><li>RelProp</li> <li>HQuant</li> <li>AggL1 (valid wrappers: NL)</li> <li>AggL2 (valid wrappers: L)</li> <li>AggL3 (valid wrappers: L)</li> <li>AggL4 (valid wrappers: NL)</li> <li>AggM1 (valid wrappers: NL)</li> <li>AggM2(valid wrappers: L)</li> <li>AggM3 (valid wrappers: L)</li> <li>AggM4 (valid wrappers: L)</li> <li>AggR1 (no valid wrappers)</li> <li>SplitBy (valid wrappers: NL)</li> <li>SplitsCon</li></ul> 

## Constraint ("con") for EProp/RelProp

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | con       | {...}  | Constraint <br> **Over any property type:** <br> _mandatory_ **op**: "empty"/"not empty" <br> <br> For any op except "empty"/"not empty": <br> _optional_ **evalEmptyAs**: true/false (default: false) - how to evaluate the condition when the property's value is empty <br> <br> **Over ordinal properties / function-range** (integer types, floating-point types, date, datetime): <br> _mandatory_ **op**: "eq"/"ne"/gt"/"ge"/"lt"/"le" <br> _mandatory_ **expr**: string <br> or <br> _mandatory_ **op**: "in set"/"not in set" <br> _mandatory_ **expr**: array [string] of size 2 or more<br> or <br> _mandatory_ **op**: "in range"/"not in range" <br> _mandatory_ **expr**: array [string] of size 2 <br> _mandatory_ **iType**: interval type: "()"/"(]"/"[)"/"[]"  <br> <br> **Over string properties / function-range:** <br> _mandatory_ **op**: "eq"/"ne"/"contains"/"not contains"/"starts with"/"not starts with"/"ends with"/"not ends with"/"match"/"not match"/"fuzzy eq"/"fuzzy ne" <br> _mandatory_ **expr**: string <br> or <br> _mandatory_ **op**: "in set" <br> _mandatory_ **expr**: array [string] or size 2 or more <br> <br> **Over timespan / datetimespan properties / function-range:** <br> _mandatory_ **op**: "eq"/"ne"/"contains"/"not contains"/"overlaps"/"not overlaps"/"shorter"/"not shorter"/"longer"/"not longer"/"same duration"/"not same duration" <br> **expr**: timespan / datetimespan <br> <br> **For multivalued property (either set or array), in addition:** _mandatory_ **valIdx**: value-index (see Q27, Q28) <br> or <br> A condition over the number of values that satisfies the value-constraint: <br> _mandatory_ **aop**: operator: "eq"/"ne"/gt"/"ge" <br> _mandatory_ **aexpr**: expression that evalues to integer <br> aops "lt" and "le" are not used. To avoid ambiguity - either _in range_ [0 .. aexpr] or _in range_ [1 .. aexpr] should be used. <br> or <br> _mandatory_ **aop**: operator: "in set"/"not in set" <br> _mandatory_ **aexpr**: array [string] of size 2 or more <br> or <br> _mandatory_ **aop**: "in range"/"not in range" <br> _mandatory_ **aexpr**: array [string] of size 2 <br> _mandatory_ **aiType**: interval type: "()"/"(]"/"[)"/"[]" <br> <br> Each element in **expr** may be a constant (e.g. "2"), a tag (e.g. "{2}") or a complex expression (e.g. "{2}+5"). Its type should match the type of property / property.f 

## Quantifier 1 (type = "Quant1")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | qType     | string | "all"/"some"/"gt"/"ge"/"notall"/"none"/"eq"/"ne"/"range". <br> "lt" and "le" are not used. To avoid ambiguity - either _range_ [0 .. value] or _range_ [1 .. value] should be used. "ne" is satisfied only if > 0
|         | qVal      | int    | mandatory if qType = "gt"/"ge"/"eq"/"ne"
|         | qVal      | [int]  | mandatory if qType = "range": array[int] of size 2
| +       | next      | [int]  | ENum of first element in each branch (>1 branches). <br> Valid element types: Rel, Path, EProp, Quant1
|         | b         | int    | ENum of element below. <br> Valid element types: <ul><li>HQuant </li> <li> AggL1 </li> <li> AggL2 </li> <li> AggL4 </li> <li> AggM1 </li> <li> AggM2 </li> <li> AggM4 </li> <li> SplitBy </li></ul> (Aggregation is valid only if there is at least one entity right of the quantifier)

## Quantifier 2 (type = "Quant2")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | qType     | string | "all"/"some"/"gt"/"ge"/"notall"/"none"/"eq"/"ne"/"range". <br> "lt" and "le" are not used. To avoid ambiguity - either _range_ [0 .. value] or _range_ [1 .. value] should be used. "ne" is satisfied only if > 0
|         | qVal      | int    | mandatory if qType = "gt"/"ge"/"eq"/"ne"
|         | qVal      | [int]  | mandatory if qType = "range": array[int] of size 2
| +       | next      | [int]  | ENum of first element in each branch (>1 branches). <br> Valid element types: EConcrete, ETyped, EUntyped, EAgg, ELog, Quant2

## Combiner (type = "Comb")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | next      | int    | ENum of next element. <br> Valid element types: EConcrete, ETyped, EUntyped, EAgg, ELog, Comb, Quant2

## Path (type = "Path") 

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
|         | eTypes    | [ * ]      | Entity types and constraints <br> For each: <br> **eType**: string - entity type - according to the ontology <br> _not mandatory_ **op**: string - operator ("eq"/"lt"/"le") and **val**: int - value
|         | rTypes    | [ * ]      | Relationship types and constraints <br> For each: <br> **relType**: string - Relationship type - according to the ontology <br> _not mandatory_ **op**: string - operator ("eq"/"lt"/"le") and **val**: int - value <br> _not mandatory_ **dir** - string - direction ("-"/"R"/"L")</li></ul>
|         | length    | *          | Path length. Either <ul><li>[string] operator ("eq"/"lt"/"le") and [int] value</li> <li>[string] operator ('in') and [int],[int] values</li> <li>[string] operator ('shortest')</li></ul>
|         | wrapper   | string     | "X": no-existance, "N": no-connection, "L": Latent, "O": Optional
|         | next      | int        | ENum of next element. <br> Valid element types: EConcrete, ETyped, EUntyped, EAgg, ELog, Quant1, Comb
|         | b         | int        | ENum of element below. <br> Valid element types: <ul><li>RelProp</li> <li>HQuant</li> <li>AggL1 (valid wrappers: NLO)</li> <li>AggL2 (valid wrappers: LO)</li> <li>AggL4 (valid wrappers: NLO)</li> <li>AggM1 (valid wrappers: NL)</li> <li>AggM2 (valid wrappers: L)</li> <li>AggM4(valid wrappers: L)</li></ul> 

## Path Segment (type = "PathSeg")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|todo     |           |        |

## Horizontal Quantifier (type = "HQuant")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | qType     | string | "all"/"some"/"gt"/"ge"/"notall"/"none"/"eq"/"ne"/"range". <br> "lt" and "le" are not used. To avoid ambiguity - either _range_ [0 .. value] or _range_ [1 .. value] should be used. "ne" is satisfied only if > 0
|         | qVal      | int    | mandatory if qType = "gt"/"ge"/"eq"/"ne"
|         | qVal      | [int]  | mandatory if qType = "range": array[int] of size 2
| +       | b         | [int]  | ENum of first element in each branch (>1 branches). <br> Valid element types: RelProp, HQuant, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1


## Horizontal Combiner (type = "HComb")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | b         | int    | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1

## Split by (type = "SplitBy")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | pType     | string | Relationship's property type (e.g. of "tf.since" of "member of") to split by
|         | tag       | string | pt/at/st to split by (e.g. "1")
|         | eTTag     | string | entity type tag to split by (e.g. "1")
| +       | b         | int    | ENum of element below. <br> Valid element types: RelProp, HQuant, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitsCon

Exactly one of the above must be presented

## Splits constraint (type = "SplitsCon")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | sTag      | string | split tag to assign (e.g. "1")
|         | con       | {...}  | Constraint <br> _mandatory_ **op**: "eq"/"ne"/gt"/"ge" <br> _mandatory_ **expr**: string <br> ops "lt" and "le" are not used. To avoid ambiguity - either _in range_ [0 .. expr] or _in range_ [1 .. expr] should be used. <br> or <br> _mandatory_ **op**: "in set" <br> _mandatory_ **expr**: array [string] of size 2 or more <br> or <br> _mandatory_ **op**: "in range" <br> _mandatory_ **expr**: array [string] of size 2 <br> _mandatory_ **iType**: interval type: "()"/"(]"/"[)"/"[]" <br> <br> Each element in **expr** may be a constant (e.g. "2"), a tag (e.g. "{2}") or a complex expression (e.g. "{2}+5"). It should be evaluated to a non-negative int.
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy, SplitsCon

## L1 Aggregation (type = "AggL1")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | eTag      | string   | entity tag on the '→/{et}' clause. '→' is denoted "->"
| +       | aTag      | string   | aggregation tag to assign (e.g. "1")
|         | con       | {...}    | Constraint. see below
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy, SplitsCon

## L2 Aggregation (type = "AggL2")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | aTag      | string   | aggregation tag to assign (e.g. "1")
|         | con       | {...}    | Constraint. see below
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy, SplitsCon

## L3 Aggregation (type = "AggL3")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | aTag      | string   | aggregation tag to assign (e.g. "1")
| +       | aggOp     | string   | aggregation operator: "min" / "max" / "sum" / "avg" / "distinct"
| +       | pType     | string   | Relationship's property type (e.g. of "tf.since" of "member of") to aggregate
|         | con       | {...}    | Constraint. see below
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy, SplitsCon

## L4 Aggregation (type = "AggL4")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | aTag      | string   | aggregation tag to assign (e.g. "2")
| +       | aggOp     | string   | aggregation operator: "min" / "max" / "sum" / "avg" / "distinct"
| +       | tag       | string   | pt/at/st to aggregate (e.g. "1")
|         | con       | {...}    | Constraint. see below
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy, SplitsCon

## Constraint ("con") for L1/L2/L3/L4

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | con       | {...}    | Constraint <br> _mandatory_ **op**: "eq"/"ne"/gt"/"ge" <br> _mandatory_ **expr**: string <br> or <br> _mandatory_ **op**: "lt"/"le" - L3/L4 only, only if aggOp is not "distinct" <br> _mandatory_ **expr**: string <br> ops "lt" and "le" are not used for L1/L2. To avoid ambiguity - either _in range_ [0 .. expr] or _in range_ [1 .. expr] should be used. <br> or <br> _mandatory_ **op**: "in set" <br> _mandatory_ **expr**: array [string] of size 2 or more <br> or <br> _mandatory_ **op**: "in range" <br> _mandatory_ **expr**: array [string] of size 2 <br> _mandatory_ **iType**: interval type: "()"/"(]"/"[)"/"[]"  <br> <br> Each element in **expr** may be a constant (e.g. "2"), a tag (e.g. "{2}") or a complex expression (e.g. "{2}+5"). It should be evaluated to a non-negative int.

## M1 Aggregation (type = "AggM1")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | n         | int      | number of min/max entities
| +       | eTag      | [string] | entity tags on the 'n {et,et,...} clause
| +       | op        | string   | "min" / "max"
| +       | eTag2     | [string] | entity tags on the 'with min/max {et,et,...}/→' clause. '→' is denoted "->"
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy

## M2 Aggregation (type = "AggM2")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | n         | int      | number of min/max entities
| +       | eTag      | [string] | entity tags on the 'n {et,et,...} clause
| +       | op        | string   | "min" / "max"
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy

## M3 Aggregation (type = "AggM3")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | n         | int      | number of min/max entities
| +       | eTag      | [string] | entity tags on the 'n {et,et,...} clause
| +       | op        | string   | "min" / "max"
| +       | aggOp     | string   | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | pType     | string   | Relationship's property type (e.g. of "tf.since" of "member of") to aggregate
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy

## M4 Aggregation (type = "AggM4")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | n         | int      | number of min/max entities
| +       | eTag      | [string] | entity tags on the 'n {et,et,...} clause
| +       | op        | string   | "min" / "max"
| +       | tag       | string   | pt/at/st to aggregate (e.g. "1")
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy

## R1 Aggregation (type = "AggR1")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | n         | int      | number of min/max entities
| +       | op        | string   | "min" / "max"
| +       | pType     | string   | Relationship's property type (e.g. of "tf.since" of "member of") to aggregate
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy

## P1 Aggregation (type = "AggP1")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | n         | int      | number of min/max splits
| +       | op        | string   | "min" / "max"
| +       | eTag2     | [string] | entity tags on the 'with min/max {et,et,...}/→' clause. '→' is denoted "->"
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy

## P2 Aggregation (type = "AggP2")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | n         | int      | number of min/max splits
| +       | op        | string   | "min" / "max"
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy

## P3 Aggregation (type = "AggP3")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | n         | int      | number of min/max splits
| +       | op        | string   | "min" / "max"
| +       | aggOp     | string   | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | pType     | string   | Relationship's property type (e.g. of "tf.since" of "member of") to aggregate
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy

## P4 Aggregation (type = "AggP4")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted "->"
| +       | n         | int      | number of min/max splits
| +       | op        | string   | "min" / "max"
| +       | tag       | string   | pt/at/st to aggregate (e.g. "1")
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggR1, SplitBy

## Nonidenticality constraints between entity tags

The following snippet demonstrates Nonidenticality constraints between tags "C" and "E", as well as between tags "C" and "F":

  "nonidentical": [ 
    ["C", "E"]
    ["C", "F"]
  ]
