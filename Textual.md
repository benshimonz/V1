## V1 pattern textual representation

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
|9  | NoExis    | no-existance            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element09.png)
|10 | NoConnect | no-connection           | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element10.png)
|11 | EComb     | E-Combiner              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element11.png)
|12 | RComb     | R-Combiner              | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element12.png)
|13 | Path      | Path                    | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element13.png)
|14 | PathSeg   | Path segment            | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element14.png)
|15 | HQuant    | Horizontal quantifier   | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element15.png)
|16 | HComb     | Horizontal combiner     | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element16.png)
|17 | Latent    | Latent                  | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element17.png)
|18 | Optional  | Optional                | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element18.png)
|19 | SplitBy   | Split by                | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element19.png)
|20 | Splits    | Splits                  | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element20.png)
|21 | Agg       | Aggregate               | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element21.png)

## JSON structure

For each V1 element:

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | Eno       | int    | element number. distinct value for each element
| +       | Type      | string | JSON element type

**TODO**: add entity tag inequalities

## E1: Query start (Type = 'Start')

There must be a single element with type 'Start'. Its Eno must equal to 1.

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | R         | int    | Eno of the element on the right. <br> Valid element types: Yellow, Blue, Red, Quant
|         | B         | int    | Eno of the element on the below. <br> Valid element types: Agg

## E2: Yellow entity (Type = 'Yellow')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | EID       | int    | Technical ID of the entity
| +       | EType     | string | Type of the entity (e.g. 'Person')
| +       | EName     | string | Display name of the entity (e.g. 'Lior Kogan')
| +       | Tag       | string | Entity tag (e.g. 'A')
|         | R         | int    | Eno of the element on the right. <br> Valid element types: Rel, EntProp, Quant, NoExis, NoConnect, EComb, Path

## E3: blue entity (Type = 'Blue')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | EType     | string | Type of the entity (e.g. 'Person')
| +       | Tag       | string | Entity tag (e.g. 'A')
|         | R         | int    | Eno of the element on the right.  <br> Valid element types: Rel, EntProp, Quant, NoExis, NoConnect, EComb, Path

## E4: red entity (Type = 'Red')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | Tag       | string | Entity tag (e.g. 'A')
|         | R         | int    | Eno of the element on the right. <br> Valid element types: Rel, EntProp, Quant, NoExis, NoConnect, EComb, Path
|         | VTypes    | string | Valid entity types TODO

## E5: relationship (Type = 'Rel')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |
|         | B         | int    | Eno of the element on the below. <br> Valid element types: RelProp, Agg

## E6: Entity's property (Type = 'EntProp') 

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E7: relationship's Property (Type = 'RelProp')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E8: Quantifier (Type = 'Quant')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E9: no-existance (Type = 'NoExis')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E10: no-connection (Type = 'NoConnect')

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

## E14: Path segment (Type = 'PathSeg')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E15: Horizontal quantifier (Type = 'HQuant')

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         |           |        |

## E16: Horizontal combiner (Type = 'HComb')

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

## E21: Aggregate (Type = 'Agg')
|Mandatory| Name      | Type   | Description

|---------|-----------|--------| ------
|         |           |        |
