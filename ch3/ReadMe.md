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

**Overview:**
* When comparing tow floating-point numbers, check that their difference is within an acceptable range.
* When performing additions or subtractions, group operations with a similar order of *magnitude*
* To favour accuracy, if a sequence of operations requires addition, subtractioon, multiplication, or division, perform the *multiplicaiton and division operations first*

## *#20* Not understanding slice length and capcity

*common to mix slice lenght and capacity*

* slice is backed by an array. (data is stored contiguously in an array data structure)

```Go
s := make([]int, 3, 6)

// # 1 Three-length, six-capacity slice
```

*Note: Capacity argument is optional*
*Accessing an element outside of the lenght range is forbidden (even when there is still capacity in mem)*

*How can we use the rest of the array?* Use append!

```GO
s[0] = 0
s[1] = 1
s[2] = 0
s.append(s,2)

s = append(s, 3, 4, 5)
fmt.Println(s)
// [0,1,0,2,3,4,5]
```
Note: A slice grows by doubling its size until it contains *1,024 elements* after which its grows by 25%


**Slicing**
```GO
s1 := make([]int, 3, 6)
s2 := s1[1:3]
// #1 Three-length, six-capacity slice
// #2 Slicing from indices 1 to 3
```

## *#21* Inefficient slice initialization

*make(type, length, capacity?)* 

If you don't pass appropriate params can lead to widespread mistakes

```GO
func convert(foos, []Foo) []Bar {
    bars := make([]Bar,0)
    for _, foo := range foos {
        bars = append(bars, fooToBar(foo))
    }
    return bar


}

// #1 Creates the resulting slice
// #2 Converts a Foo into a Bar and adds it to the slice
```
*Recall: everytime a backing array is full => Go creates another backing array doubling its capacity, copying contents new array*

Note: If we allocate our slice capacity we don't need GO to do it for us

## *#22* Being confused about nil vs empty slices
* A slice is empty if its length is equal to **zero**
* A slice is nil if it **equals** *nil* (does not require any allocation)



## *#23* Not Properly checking if a slice is empty

*How do we check if a slice contains elements?*

Solution: Check the length of the slice 

## *#24* Not Making slice copies correctly
*copy* built-in function allows copying elements from a source slice into a destination slice

```GO
    src := []int{0,1,2}
    var ds []int
    copy(dst, src)
    fmt.Println(dst)
```
*Note: example printss []*

*Why? The dest slice must correspond to the minium of the source slices length and the destinations slice length*

**Version with complet copy**
```GO
    src := []int{0,1,2}
    dst := make([]int, len(src))
    copy(dst, src)
    fmt.Println(dst)
```
*note: the destination is the former arg where the source is the latter*

## *#25* Unexpected side effects using slice append

**Example:**
```GO
s1 := []int{1,2,3}
s2 := s1[1:2]
s3 := append(s2, 10)
```
*Results: s1 <- [1, 2, 10]*

Helpful when passing a sub slice do it with a full slice to try to minimize side effects

*s[:2:2]*


