## V1 Patterns - Textual Representation

## V1 Element types

|   | JSON type property | V1 element type         | visual representation
|---|-----------|-------------------------|----------------------
|1  | Start     | query start             | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element01.png)
|2  | Yellow    | yellow entity           | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element02.png)
|3  | Blue      | blue entity             | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element03.png)
|4  | Red       | red entity              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element04.png)
|5  | Rel       | relationship            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element05.png)
|6  | EntProp   | Entity's property       | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element06.png)
|7  | RelProp   | relationship's Property | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element07.png)
|8  | Quant     | Quantifier              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element08.png)
|9  | EComb     | E-Combiner              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element09.png)
|10 | RComb     | R-Combiner              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element10.png)
|11 | Path      | Path                    | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element11.png)
|12 | PathSeg   | Path segment            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element12.png)
|13 | HQuant    | Horizontal quantifier   | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element13.png)
|14 | HComb     | Horizontal combiner     | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element14.png)
|15 | SplitBy   | Split by                | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element15.png)
|16 | Splits    | Splits                  | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element16.png)
|31 | AggL1C    | L1C aggregation         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element31.png)
|32 | AggL2C    | L2C aggregation         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element32.png)
|33 | AggLA3C   | LA3C aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element33.png)
|34 | AggLA4C   | LA4C aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element34.png)
|35 | AggD2C    | D2C aggregation         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element35.png)
|36 | AggDA3C   | DA3C aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element36.png)
|37 | AggLRM1   | LRM1 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element37.png)
|38 | AggLRM2   | LRM2 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element38.png)
|39 | AggLRMA3  | LRMA3 aggregatio        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element39.png)
|40 | AggLRM4   | LRM4 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element40.png)
|41 | AggPRM1   | PRM1 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element41.png)
|42 | AggPRM2   | PRM2 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element42.png)
|43 | AggPRM4   | PRM4 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element43.png)
|44 | AggLDM3   | LDM3 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element44.png)
|45 | AggDDM3   | DDM3 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element45.png)
|46 | AggDMA3   | DMA3 aggregation        | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element46.png)
|47 | AggDM2    | DM2 aggregation         | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element47.png)

## Wrappers for relationships / paths

no-existance  ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperX.png)
no-connection ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperN.png)
Latent        ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperL.png)
Optional      ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperO.png)

## JSON structure

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | Ont       | string | Ontology name

**For each V1 element:**

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | Eno       | int    | Element number. Distinct value for each element
| +       | Type      | string | JSON element type

**TODO**: add entity tag inequalities

## E1: Query Start (Type = 'Start')

There must be a single element with type 'Start'. Its Eno must equal to 1.

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | R         | int    | Eno of the element on the right. <br> Valid element types: Yellow, Blue, Red, Quant
|         | B         | int    | Eno of the element on the below. <br> Valid element types: AggPRM1, AggPRM2, AggPRM4

## E2: Yellow Entity (Type = 'Yellow')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | EID       | int    | Technical ID of the entity
| +       | EType     | int    | Entity type (e.g. of 'Person') <br> According to the ontology
| +       | EName     | string | Display name of the entity (e.g. 'Lior Kogan')
| +       | Tag       | string | Entity tag (e.g. 'A')
|         | R         | int    | Eno of the element on the right. <br> Valid element types: Rel, EntProp, Quant, EComb, Path

## E3: Blue Entity (Type = 'Blue')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | EType     | int    | Entity type (e.g. of 'Person') <br> According to the ontology
| +       | Tag       | string | Entity tag (e.g. 'A')
|         | R         | int    | Eno of the element on the right.  <br> Valid element types: Rel, EntProp, Quant, EComb, Path

## E4: Red Entity (Type = 'Red')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | Tag       | string | Entity tag (e.g. 'A')
|         | R         | int    | Eno of the element on the right. <br> Valid element types: Rel, EntProp, Quant, EComb, Path
|         | VTypes    | [int]  | Valid entity types <br> According to the ontology <br> VTypes and NVTypes can't be both present
|         | NVTypes   | [int]  | Invalid entity types <br> According to the ontology <br> VTypes and NVTypes can't be both present

## E5: Relationship (Type = 'Rel')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | RType     | int    | Relationship type (e.g. of 'own') <br> According to the ontology
| +       | Dir       | char   | '-': non-directional, 'R': Arrow pointing right, 'L': Arrow pointing left
|         | Wrapper   | char   | 'X': no-existance, 'N': no-connection, 'L': Latent, 'O': Optional
|         | R         | int    | Eno of the element on the right. <br> Valid element types: Yellow, Blue, Red, Quant, RComb
|         | B         | int    | Eno of the element on the below. <br> Valid element types: RelProp, HQuant, AggL1C, AggL2C, AggLA3C, AggLA4C, AggD2C, AggDA3C, AggLRM1, AggLRM2, AggLRMA3, AggLRM4, AggDM2, AggDMA3, AggLDM3, AggDDM3

## E6: Entity's Property (Type = 'EntProp') 

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | PType     | int    | Property type (e.g. of 'own') <br> According to the ontology
|         | Tag       | string | Property tag (e.g. '1')
|         | Cond      | string | condition

## E7: relationship's Property (Type = 'RelProp')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | PType     | int    | Property type (e.g. of 'own') <br> According to the ontology
|         | Tag       | string | Property tag (e.g. '1')
|         | Cond      | string | condition
|         | B         | int    | Eno of the element on the below. <br> Valid element types: HQuant, HComb, AggL1C, AggL2C, AggLA3C, AggLA4C, AggD2C, AggDA3C, AggLRM1, AggLRM2, AggLRMA3, AggLRM4, AggDM2, AggDMA3, AggLDM3, AggDDM3

## E8: Quantifier (Type = 'Quant')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E9: No-existance (Type = 'NoExist')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E10: No-connection (Type = 'NoConnect')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E11: E-Combiner (Type = 'EComb')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E12: R-Combiner (Type = 'RComb')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E13: Path (Type = 'Path') 

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E14: Path Segment (Type = 'PathSeg')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E15: Horizontal Quantifier (Type = 'HQuant')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E16: Horizontal Combiner (Type = 'HComb')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E17: Latent (Type = 'Latent')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E18: Optional (Type = 'Optional')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E19: Split by (Type = 'SplitBy')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E20: Splits (Type = 'Splits')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E31: L1C aggregation  (Type = 'AggL1C')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E32: L2C aggregation  (Type = 'AggL2C')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E33: LA3C aggregation (Type = 'AggLA3C')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E34: LA4C aggregation (Type = 'AggLA4C')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E35: D2C aggregation  (Type = 'AggD2C')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E36: DA3C aggregation (Type = 'AggDA3C')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E37: LRM1 aggregation (Type = 'AggLRM1')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E38: LRM2 aggregation (Type = 'AggLRM2')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E39: LRMA3 aggregatio (Type = 'AggLRMA')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E40: LRM4 aggregation (Type = 'AggLRM4')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E41: PRM1 aggregation (Type = 'AggPRM1')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E42: PRM2 aggregation (Type = 'AggPRM2')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E43: PRM4 aggregation (Type = 'AggPRM4')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E44: LDM3 aggregation (Type = 'AggLDM3')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E45: DDM3 aggregation (Type = 'AggDDM3')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E46: DMA3 aggregation (Type = 'AggDMA3')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |

## E47: DM2 aggregation  (Type = 'AggDM2')

|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |
