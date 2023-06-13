# Calculation
Calculation is a programming language that will integrate into most of the existing programming language to calcutate value.
Calculation is a extremely reading friendly functional programming language. It will integrate into existing programming language using string. That means calculation can be written in configuation file completely same in different programming language.

## Example
Here is an example

```Calculation
Input foodFeeding: number, good: number, normal: number
Def bad : number = foodFeeding - good - normal
Def goodRate : number = good / foodFeeding * 100
Def normalRate : number = normal / foodFeeding * 100
Def badRate : number = bad / foodFeeding * 100
Def allQuantity : enum(Good|Bad) =
{
    Good <- goodRate > 60 or goodRate + normalRate > 70
    Bad <- otherwise
}
Def notBad : number = good + normal
Output allQuantity, notBad
```

Calculation will be written in string and can be used in most of the existing programming language like SQL.

## Context
Currently, I'm consider using Calculation in dictionary context.
Like

```C#
var calcDic = new Dictionary<string, object>();
calcDic["foodFeeding"] = 100;
calcDic["good"] = 70;
calcDic["normal"] = 10;
Calcucation.Calc(calcProgram, calcDic);
var quantity = calcDic["allQuantity"]; // "Good"
var notBad = calcDic["notBad"]; // 80
```
