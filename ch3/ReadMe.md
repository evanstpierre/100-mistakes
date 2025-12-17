# Chapter 3 - DataTypes

**Overview:**
* Common mistakes related to basic types
* Fundmanetal concepts for slices and maps to prevent possible bugs, leaks, or innaccuracies
* Comparing Values

## *#17* Creating confusion with octal literals
```Go
sum := 100 + 010
fmt.Println(sum)
```

**Questions: What is the expected value of the code**
- expected: 110
- actual: 108

*An integer literal staring with 0 is considers an octal integer (base 8)*

*010 (base 8) is 8 in base (10)*

**When are octal integers useful?**
- some funcs require numbers in base 8

Integer Literal Prefixes:
* Binary - *0b* or *0B* 
* Hex - *ox* or *oX*
* Imaginary Use an i suffix (*3i*)


## *#18* Neglecting integer overflows

**Concepts:**
* go provides 10 integer types
* int/ uint - 32bits or 64bit (depending on the system)

integer overflow or underflow is silent => does not lead to application panic

**When should we be concerned about overflow**
- 

**Prediction an overflow during addition**

```GO
func AddInt(a,b int) int {
    if a > math.MaxInt -b {
        panic("int overflow")
    }
    return a + b

}
#1 checks if an integer overflow will occur
```

**Detecting an integer overflow durring multiplication**

```GO
func MultiplyINt(a,b int) int {
    if a == 0 || b == 0 {
        return 0
    }
    result := a * b
    if a == 1 || b == 1 {
        return result
    }

    if a == math.MinInt || b == math.MinInt {
        panic("INteger Overflow")

    }
    if result/b != a {
        panic("integer overflow")
    }
}
// # 1 If one the operands is equal to 0, directly returns 0
// # 2 If 
// # 3 Checks if operand is euqal or to Math.MinInt
```

Go provides a package to deal with large number $math/big$

## *#19* Not understanding floating points

*Note: Two floating point types in GO float32 and float64*

*Note: floating pointing is approx of real arithmetic*

**Example:**
```GO
var n float32 = 1.001
fmt. Println(n * n)

```
*Finite number of bits in floats but an infinite number of real numbers*

*Ordering matters in flaoting point precision*

*want: arithmetic with similar magnitude*



