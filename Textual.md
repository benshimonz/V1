## V1 pattern textual representation

## V1 Element types

|   | JSON type | V1 element type         | visual representation
|   | property  |                         |
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

* Eno -  Mandatory JSON property for each V1 element type - The element number. distinct integer value for each element
* Type - Mandatory JSON property for each V1 element type - String - the element type

There must be a single element with type 'Start'. Its Eno must equal to 1.

## Query start

* R - Mandatory JSON property - Eno of the element of the right
* A - Optional JSON property - optional Min/Max aggregate

## Yellow entity

* R - Optional JSON property - the Eno of the element on the right. 
 * Valid element types: Rel, EntProp, Quant, NoExis, NoConnect, EComb, Path, 
* UID - 
