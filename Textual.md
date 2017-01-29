## V1 Patterns - Textual Representation

## V1 Element types

|   | JSON type property | V1 element type         | visual representation
|---|-----------|-------------------------|----------------------
|1  | Start     | query start             | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element01.png)
|2  | Yellow    | yellow entity           | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element02.png)
|3  | Blue      | blue entity             | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element03.png)
|4  | Red       | red entity              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element04.png)
|5  | AggEnt    | aggregate entity        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element05.png)
|6  | LogEnt    | logical entity          | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element06.png)
|7  | Rel       | relationship            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element07.png)
|8  | EntProp   | Entity's property       | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element08.png)
|9  | RelProp   | relationship's Property | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element09.png)
|10 | Quant1    | Quantifier 1            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element10.png)
|11 | Quant2    | Quantifier 2            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element11.png)
|12 | EComb     | E-Combiner              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element12.png)
|13 | RComb     | R-Combiner              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element13.png)
|14 | Path      | Path                    | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element14.png)
|15 | PathSeg   | Path segment            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element15.png)
|16 | HQuant    | Horizontal quantifier   | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element16.png)
|17 | HComb     | Horizontal combiner     | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element17.png)
|18 | SplitBy   | Split by                | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element18.png)
|19 | Splits    | Splits                  | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element19.png)
|31 | AggL1C    | L1C aggregation         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element31.png)
|32 | AggL2C    | L2C aggregation         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element32.png)
|33 | AggLA3C   | LA3C aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element33.png)
|34 | AggLA4C   | LA4C aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element34.png)
|35 | AggD2C    | D2C aggregation         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element35.png)
|36 | AggDA3C   | DA3C aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element36.png)
|37 | AggLRM1   | LRM1 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element37.png)
|38 | AggLRM2   | LRM2 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element38.png)
|39 | AggLRMA3  | LRMA3 aggregation       | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element39.png)
|40 | AggLRM4   | LRM4 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element40.png)
|41 | AggPRM1   | PRM1 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element41.png)
|42 | AggPRM2   | PRM2 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element42.png)
|43 | AggPRM4   | PRM4 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element43.png)
|44 | AggLDM3   | LDM3 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element44.png)
|45 | AggDDM3   | DDM3 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element45.png)
|46 | AggDMA3   | DMA3 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element46.png)
|47 | AggDM2    | DM2 aggregation         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element47.png)
|51 | AggLSM1   | LSM1 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element51.png)
|52 | AggLSM2   | LSM2 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element52.png)
|53 | AggLSMA3  | LSMA3 aggregation       | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element53.png)
|54 | AggLSM4   | LSM4 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element54.png)
|55 | AggPSM1   | PSM1 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element55.png)
|56 | AggPSM2   | PSM2 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element56.png)
|57 | AggPSM4   | PSM4 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element57.png)

## Wrappers for relationships / paths

**N** - no-existance  ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperX.png)
**X** - no-connection ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperN.png)
**L** - Latent        ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperL.png)
**O** - Optional      ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperO.png)

**TODO**: add entity tag inequalities

## JSON structure

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | Ont       | string | Ontology name
| +       | Name      | string | Query name
| +       | Elements  | [...]  | Elements composing the query

**Elements: for each query element:**

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | ENum      | int    | Element number. Distinct value for each element
| +       | Type      | string | JSON element type

## E1: Query Start (Type = 'Start')

There must be a single element with type 'Start'. Its ENum must equal to 1.

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | R         | int    | ENum of the element on the right. <br> Valid element types: Yellow, Together, Blue, LogEnt, Red, Quant1
|         | B         | int    | ENum of the element below. <br> Valid element types: AggPRM1, AggPRM2, AggPRM4

## E2: Yellow Entity (Type = 'Yellow')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | ETag      | string | Entity tag (e.g. "A")
| +       | EID       | int    | Technical ID of the entity
| +       | EType     | int    | Entity type (e.g. of 'Person') <br> According to the ontology
| +       | EName     | string | Display name of the entity (e.g. "Lior Kogan")
|         | R         | int    | ENum of the element on the right. <br> Valid element types: Rel, EntProp, Quant1, EComb, Path

## E3: Blue Entity (Type = 'Blue')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | ETag      | string | Entity tag (e.g. "A")
| +       | EType     | int    | Entity type (e.g. of 'Person') <br> According to the ontology
|         | R         | int    | ENum of the element on the right.  <br> Valid element types: Rel, EntProp, Quant1, EComb, Path

## E4: Red Entity (Type = 'Red')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | ETag      | string | Entity tag (e.g. "A")
|         | R         | int    | ENum of the element on the right. <br> Valid element types: Rel, EntProp, Quant1, EComb, Path
|         | VTypes    | [int]  | Valid entity types <br> According to the ontology <br> VTypes and NVTypes can't be both present
|         | NVTypes   | [int]  | Invalid entity types <br> According to the ontology <br> VTypes and NVTypes can't be both present

## E5: Aggregate Entity (Type = 'AggEnt')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | ETag      | string | Entity tag (e.g. "A")
| +       | FName     | string | file name, where defined
| +       | EName     | string | name - as defined in file

## E6: Logical Entity (Type = 'LogEnt')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | ETag      | string | Entity tag (e.g. "A")
| +       | FName     | string | file name, where defined
| +       | EName     | string | name - as defined in file

## E7: Relationship (Type = 'Rel')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | RType     | int    | Relationship type (e.g. of 'own') <br> According to the ontology
| +       | Dir       | char   | "-": non-directional, "R": Arrow pointing right, "L": Arrow pointing left
|         | Wrapper   | char   | "X": no-existance, "N": no-connection, "L": Latent, "O": Optional
|         | R         | int    | ENum of the element on the right. <br> Valid element types: Yellow, Together, Blue, LogEnt, Red, Quant2, RComb
|         | B         | int    | ENum of the element below. <br> Valid element types: <ul><li>RelProp</li> <li>HQuant</li> <li>AggL1C (valid wrappers: NLO)</li> <li>AggL2C (valid wrappers: LO)</li> <li>AggLA3C (valid wrappers: LO)</li> <li>AggLA4C (valid wrappers: NLO)</li> <li>AggD2C (valid wrappers: LO)</li> <li>AggDA3C(valid wrappers: LO)</li> <li>AggLRM1 (valid wrappers: NL)</li> <li>AggLRM2 (valid wrappers: L)</li> <li>AggLRMA3 (valid wrappers: L)</li> <li>AggLRM4 (valid wrappers: L)</li> <li>AggDM2 (valid wrappers: none)</li> <li>AggDMA3 (valid wrappers: none)</li> <li>AggLDM3 (valid wrappers: none)</li> <li>AggDDM3 (valid wrappers: none)</li></ul> 

## E8: Entity's Property (Type = 'EntProp') 

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | PType     | int    | Property type (e.g. of 'own') <br> According to the ontology
|         | PTag      | string | Property tag to assign (e.g. "1")
|         | Cond      | string | condition

## E9: Relationship's Property (Type = 'RelProp')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | PType     | int    | Property type (e.g. of 'own') <br> According to the ontology
|         | PTag      | string | Property tag to assign (e.g. "1")
|         | Cond      | string | condition
|         | B         | int    | ENum of the element below. <br> Valid element types: HQuant, HComb, AggL1C, AggL2C, AggLA3C, AggLA4C, AggD2C, AggDA3C, AggLRM1, AggLRM2, AggLRMA3, AggLRM4, AggDM2, AggDMA3, AggLDM3, AggDDM3

## E10: Quantifier 1 (Type = 'Quant1')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | QType     | string | "all", "notall", "none", "notnone", "eq", "gt", "ge", "lt", "le", "ne", "range", "notrange"
| +       | Branches  | int    | number of branches (>1)
| +       | R         | [int]  | ENum of elements on the right. <br> Valid element types: Rel, Path, EntProp, Quant1
|         | B         | int    | ENum of element below. Valid element types: AggL1C, AggL2C, AggLA4C (Aggregation is valid only if there is at least one entity right of the quantifier)

## E11: Quantifier 2 (Type = 'Quant2')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | QType     | string | "all", "notall", "none", "notnone", "eq", "gt", "ge", "lt", "le", "ne", "range", "notrange"
| +       | Branches  | int    | number of branches (>1)
| +       | R         | [int]  | ENum of elements on the right. <br> Valid element types: Yellow, Together, Blue, LogEnt, Red, Quant2

## E12: E-Combiner (Type = 'EComb')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | R         | int    | ENum of elements on the right. <br> Valid element types: Rel, EntProp, Quant1, EComb, Path

## E13: R-Combiner (Type = 'RComb')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | R         | int    | ENum of element on the right. <br> Valid element types: Yellow, Together, Blue, LogEnt, Red, RComb, Quant2

## E14: Path (Type = 'Path') 

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
|         | ETypes    | [ * ]      | Entity types <br> For each entry: <ul><li>Entity type - according to the ontology</li> <li> Optional [string] operator ("eq", "lt", or "le") and [int] value</li></ul>
|         | RTypes    | [ * ]      | Relationship types <br> For each entry: <ul><li>Relationship type - according to the ontology</li> <li>Optional [string] operator ("eq", "lt", or "le") and [int] value</li> <li>Optional [string] direction ("R" or "L")</li></ul>
|         | Length    | *          | Path length. Either <ul><li>[string] operator ("eq", "lt", "le") and [int] value</li> <li>[string] operator ('in') and [int],[int] values</li> <li>[string] operator ('shortest')</li></ul>
|         | Wrapper   | string     | "X": no-existance, "N": no-connection, "L": Latent, "O": Optional
|         | R         | int        | ENum of the element on the right. <br> Valid element types: Yellow, Together, Blue, LogEnt, Red, Quant1, RComb
|         | B         | int        | ENum of the element below. <br> Valid element types: <ul><li>RelProp</li> <li>HQuant</li> <li>AggL1C (valid wrappers: NLO)</li> <li>AggL2C (valid wrappers: LO)</li> <li>AggLA3C (valid wrappers: LO)</li> <li>AggLA4C (valid wrappers: NLO)</li> <li>AggD2C (valid wrappers: LO)</li> <li>AggDA3C(valid wrappers: LO)</li> <li>AggLRM1 (valid wrappers: NL)</li> <li>AggLRM2 (valid wrappers: L)</li> <li>AggLRMA3 (valid wrappers: L)</li> <li>AggLRM4 (valid wrappers: L)</li> <li>AggDM2 (valid wrappers: none)</li> <li>AggDMA3 (valid wrappers: none)</li> <li>AggLDM3 (valid wrappers: none)</li> <li>AggDDM3 (valid wrappers: none)</li></ul> 

## E15: Path Segment (Type = 'PathSeg')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E16: Horizontal Quantifier (Type = 'HQuant')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | QType     | string | "all", "notall", "none", "notnone", "eq", "gt", "ge", "lt", "le", "ne", "range", "notrange"
| +       | Branches  | int    | number of branches (>1)
| +       | B         | int    | ENum of element below. <br> Valid element types: RelProp, HQuant, AggL1C, AggL2C, AggLA3C, AggLA4C, AggD2C, AggDA3C, AggLRM1, AggLRM2, AggLRMA3, AggLRM4, AggDM2, AggDMA3, AggLDM3, AggDDM3


## E17: Horizontal Combiner (Type = 'HComb')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | B         | int    | ENum of elements below. <br> Valid element types: RelProp, HQuant, HComb, AggL1C, AggL2C, AggLA3C, AggLA4C, AggD2C, AggDA3C, AggLRM1, AggLRM2, AggLRMA3, AggLRM4, AggDM2, AggDMA3, AggLDM3, AggDDM3

## E18: Split by (Type = 'SplitBy')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | RelProp   | string | name of relationship's property to split by (e.g. "since")
|         | Tag       | string | pt/at/st to split by (e.g. "1")
|         | ETTag     | string | entity type tag to split by (e.g. "1")

Exactly one of the above must be presented

## E19: Splits (Type = 'Splits')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | Cond      | string | condition
|         | STag      | string | split tag to assign (e.g. "1")

At least one of the above must be presented

## E31: L1C aggregation (Type = 'AggL1C')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | ETag      | string | entity tag on the right of the aggregation. if not present: treat as '→'
|         | ATag      | string | attribute tag to assign (e.g. "1")
|         | Cond      | string | condition

## E32: L2C aggregation (Type = 'AggL2C')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | ATag      | string | attribute tag to assign (e.g. "1")
|         | Cond      | string | condition

## E33: LA3C aggregation (Type = 'AggLA3C')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | ATag      | string | attribute tag to assign (e.g. "1")
| +       | AggOp     | string | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | RelProp   | string | name of relationship's property to aggregate (e.g. "since")
|         | Cond      | string | condition

## E34: LA4C aggregation (Type = 'AggLA4C')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | ATag      | string | attribute tag to assign (e.g. "2")
| +       | AggOp     | string | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | Tag       | string | pt/at/st to aggregate (e.g. "1")
|         | Cond      | string | condition

## E35: D2C aggregation  (Type = 'AggD2C')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | ATag      | string | attribute tag to assign (e.g. "1")
|         | Cond      | string | condition

## E36: DA3C aggregation (Type = 'AggDA3C')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | ATag      | string | attribute tag to assign (e.g. "1")
| +       | AggOp     | string | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | RelProp   | string | name of relationship's property to aggregate (e.g. "since")
|         | Cond      | string | condition

## E37: LRM1 aggregation (Type = 'AggLRM1')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | ETag      | string | entity tag on the right of the aggregation
| +       | op        | string | "min" / "max"

## E38: LRM2 aggregation (Type = 'AggLRM2')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | op        | string | "min" / "max"

## E39: LRMA3 aggregatio (Type = 'AggLRMA3')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | op        | string | "min" / "max"
| +       | AggOp     | string | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | RelProp   | string | name of relationship's property to aggregate (e.g. "since")

## E40: LRM4 aggregation (Type = 'AggLRM4')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | op        | string | "min" / "max"
| +       | Tag       | string | pt/at/st to aggregate (e.g. "1")

## E41: PRM1 aggregation (Type = 'AggPRM1')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | ETag      | string | entity tag on the right of the aggregation
| +       | op        | string | "min" / "max"

## E42: PRM2 aggregation (Type = 'AggPRM2')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | op        | string | "min" / "max"

## E43: PRM4 aggregation (Type = 'AggPRM4')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | op        | string | "min" / "max"
| +       | Tag       | string | pt/at/st to aggregate (e.g. "1")

## E44: LDM3 aggregation (Type = 'AggLDM3')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | op        | string | "min" / "max"
| +       | RelProp   | string | name of relationship's property to aggregate (e.g. "since")

## E45: DDM3 aggregation (Type = 'AggDDM3')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | op        | string | "min" / "max"
| +       | RelProp   | string | name of relationship's property to aggregate (e.g. "since")

## E46: DMA3 aggregation (Type = 'AggDMA3')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | op        | string | "min" / "max"
| +       | AggOp     | string | aggregation operator ("min" / "max" / "sum" / "avg" / "distinct")
| +       | RelProp   | string | name of relationship's property to aggregate (e.g. "since")

## E47: DM2 aggregation  (Type = 'AggDM2')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | n         | int    | number of min/max entities
| +       | op        | string | "min" / "max"
