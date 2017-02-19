## V1 Patterns - JSON-based Representation

## V1 Element types

|   | JSON type property | V1 element type         | visual representation
|---|------------|-------------------------|----------------------
|1  | Start      | Query Start             | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element01.png)
|2  | EConcrete  | Concrete Entity         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element02.png)
|3  | ETyped     | Typed Entity            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element03.png)
|4  | EUntyped   | Untyped Entity          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element04.png)
|5  | EAgg       | Aggregate Entity        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element05.png)
|6  | ELog       | Logical Entity          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element06.png)
|7  | Rel        | Relationship            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element07.png)
|8  | EProp      | Entity's Property       | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element08.png)
|9  | RelProp    | Relationship's Property | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element09.png)
|10 | Quant1     | Quantifier 1            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element10.png)
|11 | Quant2     | Quantifier 2            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element11.png)
|12 | EComb      | E-Combiner              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element12.png)
|13 | RComb      | R-Combiner              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element13.png)
|14 | Path       | Path                    | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element14.png)
|15 | PathSeg    | Path Segment            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element15.png)
|16 | HQuant     | Horizontal Quantifier   | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element16.png)
|17 | HComb      | Horizontal Combiner     | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element17.png)
|18 | SplitBy    | Split By                | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element18.png)
|19 | SplitsCond | Splits Condition        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element19.png)
|31 | AggL1      | L1 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element31.png)
|32 | AggL2      | L2 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element32.png)
|33 | AggL3      | L3 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element33.png)
|34 | AggL4      | L4 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element34.png)
|41 | AggM1      | M1 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element41.png)
|42 | AggM2      | M2 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element42.png)
|43 | AggM3      | M3 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element43.png)
|44 | AggM4      | M4 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element44.png)
|45 | AggM5      | M5 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element45.png)
|51 | AggP1      | P1 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element51.png)
|52 | AggP2      | P2 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element52.png)
|53 | AggP3      | P3 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element53.png)
|54 | AggP4      | P4 Aggregation          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element54.png)

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

## E1: Query Start (Type = 'Start')

There must be a single element with type 'Start'. Its ENum must equals to 0.

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | next      | int    | ENum of next element. <br> Valid element types: SplitBy

## E2: Concrete Entity (Type = 'EConcrete')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
| +       | eID       | string | Technical ID of the entity
| +       | eType     | int    | Entity type (e.g. of 'Person') <br> According to the ontology
| +       | eName     | string | Display name of the entity (e.g. "Lior Kogan"). It is better to read the name from the database. This  was the name when the pattern was stored
|         | next      | int    | ENum of next element. <br> Valid element types: Rel, EProp, Quant1, EComb, Path

## E3: Typed Entity (Type = 'ETyped')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
| +       | eType     | int    | Entity type (e.g. of 'Person') <br> According to the ontology
|         | next      | int    | ENum of next element.  <br> Valid element types: Rel, EProp, Quant1, EComb, Path
|         | b         | int    | ENum of element below. <br> Valid element types: AggP1, AggP4

## E4: Untyped Entity (Type = 'EUntyped')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
|         | vTypes    | [int]  | Valid entity types <br> According to the ontology <br> VTypes and NVTypes can't be both present
|         | nVTypes   | [int]  | Invalid entity types <br> According to the ontology <br> VTypes and NVTypes can't be both present
|         | next      | int    | ENum of next element. <br> Valid element types: Rel, EProp, Quant1, EComb, Path

## E5: Aggregate Entity (Type = 'EAgg')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
| +       | fName     | string | file name, where defined
| +       | eName     | string | name - as defined in file
|         | next      | int    | ENum of next element. <br> Valid element types: Rel, EProp, Quant1, EComb, Path

## E6: Logical Entity (Type = 'ELog')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | Entity tag (e.g. "A")
| +       | fName     | string | file name, where defined
| +       | eName     | string | name - as defined in file
|         | next      | int    | ENum of next element.  <br> Valid element types: Rel, EProp, Quant1, EComb, Path

## E7: Relationship (Type = 'Rel')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | rType     | int    | Relationship type (e.g. of 'own') <br> According to the ontology
| +       | dir       | char   | "-": non-directional, "R": Arrow pointing right, "L": Arrow pointing left
|         | wrapper   | char   | "X": no-existance, "N": no-connection, "L": Latent, "O": Optional
|         | next      | int    | ENum of next element. <br> Valid element types: EConcrete, ETyped, EUntyped, EAgg, ELog, Quant2, RComb
|         | b         | int    | ENum of element below. <br> Valid element types: <ul><li>RelProp</li> <li>HQuant</li> <li>AggL1 (valid wrappers: NL)</li> <li>AggL2 (valid wrappers: L)</li> <li>AggL3 (valid wrappers: L)</li> <li>AggL4 (valid wrappers: NL)</li> <li>AggM1 (valid wrappers: NL)</li> <li>AggM2(valid wrappers: L)</li> <li>AggM3 (valid wrappers: L)</li> <li>AggM4 (valid wrappers: L)</li> <li>AggM5 (no valid wrappers)</li> <li>SplitBy (valid wrappers: NL)</li></ul> 

## E8: Entity's Property (Type = 'EProp') 

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | pType     | int    | Property type (e.g. of 'own') <br> According to the ontology
|         | f         | string | function to apply to property
|         | pTag      | string | Property tag to assign to property / to f(property) (e.g. "1")
|         | cond      | {...}  | Condition <br> _mandatory_ **op**: "eq"/"ne"/gt"/"ge"/"lt"/"le"/"in set"/"not in set"/"in range"/"not in range"/"empty"/"not empty" <br> if _op_ is "eq"/"ne"/gt"/"ge"/"lt"/"le": _mandatory_ **value**: right side of the condition. same type as pType / f(pType) domain <br> if _op_ is "in set"/"not in set"/"in range"/"not in range": _mandatory_ **value**: right side of the condition. array [same type as pType / f(pType)]

At least one of {pTag, cond} must be present

## E9: Relationship's Property (Type = 'RelProp')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | pType     | int    | Property type (e.g. of 'own') <br> According to the ontology
|         | f         | string | function to apply to property
|         | pTag      | string | Property tag to assign to property / to f(property) (e.g. "1")
|         | cond      | {...}  | Condition <br> _mandatory_ **op**: "eq"/"ne"/gt"/"ge"/"lt"/"le"/"in set"/"not in set"/"in range"/"not in range"/"empty"/"not empty" <br> if _op_ is "eq"/"ne"/gt"/"ge"/"lt"/"le": _mandatory_ **value**: right side of the condition. same type as pType / f(pType) <br> if _op_ is "in set"/"not in set"/"in range"/"not in range": _mandatory_ **value**: right side of the condition. array [same type as pType / f(pType)]
|         | b         | int    | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5, SplitBy

At least one of {pTag, cond} must be present

## E10: Quantifier 1 (Type = 'Quant1')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | qType     | string | "all"/"some"/"gt"/"ge"/"notall"/"none"/"eq"/"lt"/"le"/"ne"/"range"/"notrange"
| +       | next      | [int]  | ENum of first element in each branch (>1 branches). <br> Valid element types: Rel, Path, EProp, Quant1
|         | b         | int    | ENum of element below. <br> Valid element types: <ul><li>HQuant </li> <li> AggL1 </li> <li> AggL2 </li> <li> AggL4 </li> <li> AggM1 </li> <li> AggM2 </li> <li> AggM4 </li> <li> SplitBy </li></ul> (Aggregation is valid only if there is at least one entity right of the quantifier)

## E11: Quantifier 2 (Type = 'Quant2')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | qType     | string | "all"/"some"/"gt"/"ge"/"notall"/"none"/"eq"/"lt"/"le"/"ne"/"range"/"notrange"
| +       | next      | [int]  | ENum of first element in each branch (>1 branches). <br> Valid element types: EConcrete, ETyped, EUntyped, EAgg, ELog, Quant2

## E12: E-Combiner (Type = 'EComb')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | next      | int    | ENum of next elements. <br> Valid element types: Rel, EProp, Quant1, EComb, Path

## E13: R-Combiner (Type = 'RComb')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | next      | int    | ENum of next element. <br> Valid element types: EConcrete, ETyped, EUntyped, EAgg, ELog, RComb, Quant2

## E14: Path (Type = 'Path') 

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
|         | eTypes    | [ * ]      | Entity types and constraints <br> For each: <br> **eType**: string - entity type - according to the ontology <br> _not mandatory_ **op**: string - operator ("eq"/"lt"/"le") and **val**: int - value
|         | rTypes    | [ * ]      | Relationship types and constraints <br> For each: <br> **relType**: string - Relationship type - according to the ontology <br> _not mandatory_ **op**: string - operator ("eq"/"lt"/"le") and **val**: int - value <br> _not mandatory_ **dir** - string - direction ("-"/"R"/"L")</li></ul>
|         | length    | *          | Path length. Either <ul><li>[string] operator ("eq"/"lt"/"le") and [int] value</li> <li>[string] operator ('in') and [int],[int] values</li> <li>[string] operator ('shortest')</li></ul>
|         | wrapper   | string     | "X": no-existance, "N": no-connection, "L": Latent, "O": Optional
|         | next      | int        | ENum of next element. <br> Valid element types: EConcrete, ETyped, EUntyped, EAgg, ELog, Quant1, RComb
|         | b         | int        | ENum of element below. <br> Valid element types: <ul><li>RelProp</li> <li>HQuant</li> <li>AggL1 (valid wrappers: NLO)</li> <li>AggL2 (valid wrappers: LO)</li> <li>AggL4 (valid wrappers: NLO)</li> <li>AggM1 (valid wrappers: NL)</li> <li>AggM2 (valid wrappers: L)</li> <li>AggM4(valid wrappers: L)</li></ul> 

## E15: Path Segment (Type = 'PathSeg')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|todo     |           |        |

## E16: Horizontal Quantifier (Type = 'HQuant')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | qType     | string | "all"/"some"/"gt"/"ge"/"notall"/"none"/"eq"/"lt"/"le"/"ne"/"range"/"notrange"
| +       | b         | [int]  | ENum of first element in each branch (>1 branches). <br> Valid element types: RelProp, HQuant, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5


## E17: Horizontal Combiner (Type = 'HComb')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | b         | int    | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

## E18: Split by (Type = 'SplitBy')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | pType     | int    | Relationship's property type (e.g. of 'since') to split by
|         | tag       | string | pt/at/st to split by (e.g. "1")
|         | eTTag     | string | entity type tag to split by (e.g. "1")

Exactly one of the above must be presented

## E19: Splits condition (Type = 'SplitsCond')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | cond      | string | condition
|         | sTag      | string | split tag to assign (e.g. "1")

At least one of the above must be presented

## E31: L1 Aggregation (Type = 'AggL1')

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted '->'
| +       | eTag      | string   | entity tag on the '→/{et}' clause. '→' is denoted '->'
|         | aTag      | string   | attribute tag to assign (e.g. "1")
|         | cond      | {...}    | Condition <br> _mandatory_ **op**: "eq"/"ne"/gt"/"ge"/"lt"/"le"/"in set"/"not in set"/"in range"/"not in range" <br> _mandatory_ **value**: right side of the condition. For "eq"/"ne"/gt"/"ge"/"lt"/"le": non-negative int. For "in set"/"not in set"/"in range"/"not in range" - array [non-negative int].
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

At least one of {aTag, cond} must be present

## E32: L2 Aggregation (Type = 'AggL2')

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted '->'
|         | aTag      | string   | attribute tag to assign (e.g. "1")
|         | cond      | {...}    | Condition <br> _mandatory_ **op**: "eq"/"ne"/gt"/"ge"/"lt"/"le"/"in set"/"not in set"/"in range"/"not in range" <br> _mandatory_ **value**: right side of the condition. For "eq"/"ne"/gt"/"ge"/"lt"/"le": non-negative int. For "in set"/"not in set"/"in range"/"not in range" - array [non-negative int].
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

At least one of {aTag, cond} must be present

## E33: L3 Aggregation (Type = 'AggL3')

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted '->'
|         | aTag      | string   | attribute tag to assign (e.g. "1")
| +       | aggOp     | string   | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | pType     | int      | Relationship's property type (e.g. of 'since') to aggregate
|         | cond      | {...}    | Condition <br> _mandatory_ **op**: "eq"/"ne"/gt"/"ge"/"lt"/"le"/"in set"/"not in set"/"in range"/"not in range" <br> _mandatory_ **value**: right side of the condition. For "eq"/"ne"/gt"/"ge"/"lt"/"le": same type as pType. For "in set"/"not in set"/"in range"/"not in range" - array [same type as pType].
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

At least one of {aTag, cond} must be present

## E34: L4 Aggregation (Type = 'AggL4')

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted '->'
|         | aTag      | string   | attribute tag to assign (e.g. "2")
| +       | aggOp     | string   | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | tag       | string   | pt/at/st to aggregate (e.g. "1")
|         | cond      | {...}    | Condition <br> _mandatory_ **op**: "eq"/"ne"/gt"/"ge"/"lt"/"le"/"in set"/"not in set"/"in range"/"not in range" <br> _mandatory_ **value**: right side of the condition. For "eq"/"ne"/gt"/"ge"/"lt"/"le": same type as property with _pt_ / non-negative int. For "in set"/"not in set"/"in range"/"not in range" - array [same type as property with _pt_ / non-negative int].
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

At least one of {aTag, cond} must be present

## E41: M1 Aggregation (Type = 'AggM1')

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted '->'
| +       | n         | int      | number of min/max entities
| +       | eTag      | [string] | entity tags on the 'n {et,et,...} clause
| +       | op        | string   | "min" / "max"
| +       | eTag2     | [string] | entity tags on the 'with min/max {et,et,...} clause
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

## E42: M2 Aggregation (Type = 'AggM2')

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted '->'
| +       | n         | int      | number of min/max entities
| +       | eTag      | [string] | entity tags on the 'n {et,et,...} clause
| +       | op        | string   | "min" / "max"
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

## E43: M3 Aggregation (Type = 'AggM3')

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted '->'
| +       | n         | int      | number of min/max entities
| +       | eTag      | [string] | entity tags on the 'n {et,et,...} clause
| +       | op        | string   | "min" / "max"
| +       | aggOp     | string   | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | pType     | int      | Relationship's property type (e.g. of 'since') to aggregate
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

## E44: M4 Aggregation (Type = 'AggM4')

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted '->'
| +       | n         | int      | number of min/max entities
| +       | eTag      | [string] | entity tags on the 'n {et,et,...} clause
| +       | op        | string   | "min" / "max"
| +       | tag       | string   | pt/at/st to aggregate (e.g. "1")
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

## E45: M5 Aggregation (Type = 'AggM5')

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|         | per       | [string] | entity tags on the 'per {et,et,...}/→' clause. '→' is denoted '->'
| +       | n         | int      | number of min/max entities
| +       | op        | string   | "min" / "max"
| +       | pType     | int      | Relationship's property type (e.g. of 'since') to aggregate
|         | b         | int      | ENum of element below. <br> Valid element types: RelProp, HQuant, HComb, AggL1, AggL2, AggL3, AggL4, AggM1, AggM2, AggM3, AggM4, AggM5

## Nonidenticality constraints between entity tags

The following snippet demonstrates Nonidenticality constraints between tags "C" and "E", as well as between tags "C" and "F":

  "nonidentical": [ 
    ["C", "E"]
    ["C", "F"]
  ]
