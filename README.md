# Calculation
Calculation is a programming language that will integrate into most of the existing programming language to calcutate value.
Calculation is a extremely reading friendly functional programming language. It will integrate into existing programming language using string. That means calculation can be written in configuation file completely same in different programming language.

## Example
Here is an example

```Calculation
Input foodFeeding: number, good: number, normal: number
Def bad: number = foodFeeding - good - normal
Def goodRate: number = good / foodFeeding * 100
Def normalRate: number = normal / foodFeeding * 100
Def badRate: number = bad / foodFeeding * 100
Def allQuantity: enum(Good|Bad) =
{
    Good <- goodRate > 60 or goodRate + normalRate > 70
    Bad <- otherwise
}
Def notBad: number = good + normal
Output allQuantity, notBad
```

Calculation will be written in string and can be used in most of the existing programming language like SQL.

## Context
Currently, I'm consider using Calculation in dictionary context.<br>
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

# Reference
## Type
There are only four types in Calculation: number, enum, function and nil.
Calculation regards number as one type, no difference between float and integer.
Enum is a set of multiple values. e.g. (Good|Bad), (Start|Stop|Running), [1..10], [a..z], [A..Z]
Nil is a value that not been set. It can be appended to another constant but cannot join calculation. In Calculation number divide zero will be nil.
Function is a basic type that has input, output and process.
```Calculation
Input nil
Def func_sample: function = 
{
    Input x: number, y:number
    Def answer: number = (x+y)^2
    Output answer
}
Def run_func_value: number = func_sample(1,2)
Output run_func_value
```

## Constant
In Calculation, constant name can be started as number. We only regard numbers and dot only as number and numbers and symbols only as Calc expression.
Otherwise it's constant name.

By the way, there is no variable in Calculation.

### Lifecycle
Unlike most of the functional language, Calculation has lifecycle.
If an constant leave the lifecycle and been removed, it could be defined again outside lifecycle.
```Calculation
Input nil
Def func_sample: function = 
{
    Input x: number, y: number
    Def answer = (x+y)^2
    Output answer // lifecycle remove
}
Def func_sample_2: function =
{
    Input a: number, b: func(x: number,y: number->ans: number)
    Def answer: number = b(a, 2)
    Output answer // lifecycle remove
}
Def answer: number = function_sample_2(3, func_sample) //outside lifecycle define again
Output answer
```

## Notice
1. Input could be nil, but output could not be nil.
2. No for and foreach, if you want to write code like for and foreach, using enum.
3. In a normal functional programming language, func(x,y) an be called as func(1) to another function, but in Calculation, it is impossible.
4. But you can call func(x,y) in func(x,y,z), that means: in Calculation, if you want to call a function using any number parameter, input and output number should be matched and all parameters should be number.
5. Function cannot been used as parameter in the top level of Input in the code.

## Function as parameter (Determine, probably change to no function use as parameter)
Function as parameter is the same as function but has no content written.
e.g.
```Calculation
Input nil
Def func_sample: function = 
{
    Input x: number, y: number
    Def answer = (x+y)^2
    Output answer
}
Def func_sample_2: function =
{
    Input a: number, b: func(x: number,y: number->ans: number)
    Def answer: number = b(a, 2)
    Output answer
}
Def run_func_value: number = function_sample_2(3, func_sample)
Output a
```

## Enum
### Definiton
In Calculation, enum can be used in Input, Output, and Def.
e.g.
```Calculation
Input Value[1..4]: number // Which means Value1, Value2, Value3, Value4
Def Value_Output[1..4]: number = Value[1..4] // Which means Value_Output1 = Value1, Value_Output2 = Value2 ...
Output Value_Output[1..4]
```

You can use multiple enum in a single Variable.
```Calculation
Input Value[a..b][1..2] : enum(ON|ON[a..b]|OFF|OFF[a..b]) // which means Valuea1, Valuea2, Valueb1, Valueb2 can be set as ON, ONa, ONb, OFF, OFFa and OFFb
...
```

Enum return can be set like
```Calculation
...
Def x: number = 3
Def ans: allQuantity[1..4] =
{
    allQuantity[1..2] <- x == [0..1] // allQuantity1 <- x == 0 and allQuantity2 <- x == 1
    allQuantity3 <- x == 2 // allQuantity3 <- x == 2
    allQuantity4 <- otherwise
}
Output ans // allQuantity4
```

### Write Code like for
e.g.
```Calculation
Input nil
Def i[0..9]: number = [0..9]
Def ans = i[0..9](+) // i0 + i1 + i2 + i3 + i4 + i5 + i6 + i7 + i8 + i9
Output ans // 45
```

### Union Function
Union Function is a special function that should using x inputs and x-1 output(s).
```Calculation
Def add: function
{
    Input x: number, y: number
    Def ans: number = x + y
    Output ans
}
Def union_ans: number = [0..9](add)
Output union_ans // 45
```

### Number Enum
If your enum content are all numbers, this is regarded as number enum.
```Calculation
Def number_enum : enum(1|3|5|6|8)
Def number_enum_2 : [1..9]
```

Number enum can join calculation in expressions.
```Calculation
Def calc[1..5] = number_enum * 2
```

if you use number enum is on the left side of "=" to define type, that means, if calc value does not exist in enum type, it will return nil.
```Calculation
Def calc[1..5] : [1..9] = number_enum * 2 // calc3, calc4, calc5 are nil
```
