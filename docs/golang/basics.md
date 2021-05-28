# Basics

## Hello World
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello World!")
}
```

## Variables

```go
// variable declaration
var a int
var b bool

// variable assignment
a = 20
b = "string"

// variable declaration and assignment
x := 20
y := "string"

// multiple declaration
var (
    m int
    n bool
)
```

## Types

| Type   | Desc              |
| ------ | ----------------- |
| `bool` | _true_ or _false_ |
|`int` | Will be 32 bit on 32 bit machines and 64 on 64 bit machines|
|`uint` | Similar to int but with no bit for sign|
|`int8`  `uint8` `int16` `uint16` `int32` `uint32` `int64` `uint64` | other ints available|
|`byte` | an alias for `uint8`|
|`float32` `float64` | floating point but there is no other type called ~~`float`~~|
|`string` | UTF-8, enclosed in "", Immutable|
|`rune` | alias for int32, UTF-8 encoded code point|
|`complex64` `complex128` | 32 bit real and imaginary and 64 bit real and imaginary parts respectively. Written as **re+im*i*** where **re** is real part, **im** is imaginary part and ***i*** is literal|

> `byte` can only represent 8-bit ASCII so `rune`should be used in such cases where UTF-8 is needed

## Constants

Created at compile time and can only be `number`, `string` or `bool`

```go
const (
    a = iota
    b
)
```

The first use of iota will yield 0, so a is equal to 0. Whenever iota is used again on a new line its value is incremented with 1, so b has a value of 1. Or, as shown here, you can even let Go repeat the use of iota. You may also explicitly type a constant: const b string = "0". Now b is a string type constant.

## Operators

| Precedence | Operator(s)      |
| ---------- | ---------------- |
| Highest    | * / % << >> & &^ |
|            | \`+ -            |
|            | == != < <= > >=  |
|            | <-               |
|            | &&               |
| Lowest     | \|\|             |


## Control Structures

### if else

```go
if x > 0 {
    return y
} else {
    return x
}
```

`if` and `switch` accept an initialization statement and its common to see it used to setup a local variable

```go
if err := SomeFunction(); err == nil {
    // do something
} else {
    return err
}
```

### goto

```go
func myfunc() {
    i := 0
Here:
    fmt.Println(i)
    i++
    goto Here
}
```

### for

has 3 forms
* `for init; condition; post {}` - regular C style loop
* `for condition {}` - a while loop
* `for {}` - an endless loop

```go
sum := 0
for i := 0; i < 10; i++ {
    sum = sum + i
}
```

### break and continue

```go
J:  for j := 0; j < 5; j++ { 
        for i := 0; i < 10; i++ {
            if i > 5 {
                break J
            }
            fmt.Println(i)
        }
    }
```

here `J` is a label

### range

will create iterators over following types and is useful for `for` loops

| Type     | Returns        |
| -------- | -------------- |
| slices   | int, sliceType |
| arrays   | int, arrayType |
| strings  | int, rune      |
| maps     |                |
| channels |                |

```go
list := []string{"a", "b", "c", "d", "e", "f"}
for k, v := range list {
    // do something with k and v
}
```

### switch

Go's `switch` is similar to the one in `C` and `Java` with some key differences

```go
switch os := runtime.GOOS; os {
case "darwin":
    fmt.Println("OS X.")
case "linux":
    fmt.Println("Linux.")
default:
    // freebsd, openbsd,
    // plan9, windows...
    fmt.Printf("%s.\n", os)
}
```

There is no fallthrough mechanism here so there is no need for `break` statements. If we need to replicate the fallthrough we can use `fallthrough`

```go
switch i {
    case 0:  fallthrough
    case 1:
        f()
    default:
        g()
}
```

Multiple conditions can be defined under a case
```go
switch i {
    case 0, 1:
        f()
    default:
        g()
}
```

An `if-else-if-else` chain can be simplified using  `switch`.

```go
// Convert hexadecimal character to an int value
switch {
case '0' <= c && c <= '9':
    return c - '0' 3
case 'a' <= c && c <= 'f':
    return c - 'a' + 10
case 'A' <= c && c <= 'F':
    return c - 'A' + 10
}
return 0
```

## Built-in Functions

| function                  | usage                                                                                                                                       |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `close`                   | used in channel communication. It closes a channel                                                                                         |
| `delete`                  | used for deleting entries in maps                                                                                                          |
| `len`, `cap`              | used on a number of different types, len is used to return the lengths of strings, maps, slices, and arrays                                |
| `new`                     | used for allocating memory for user defined data types                                                                                     |
| `make`                    | used for allocating memory for built-in types (maps, slices, and channels)                                                                 |
| `copy`, `append`          | copy is for copying slices. And append is for concatenating slices                                                                         |
| `panic`, `recover`        | used for an exception mechanism                                                                                                            |
| `print`, `println`        | low level printing functions that can be used without reverting to the fmt package. These are mainly used for debugging. built-in,println) |
| `complex`, `real`, `imag` | all deal with complex numbers                                                                                                              |

## Arrays

`Arrays` are of pre-defined length with a particular type.

```go
var arr [10]int
arr[0] = 42
arr[1] = 13
fmt.Printf("The first element is %d\n", arr[0])
```

Whenever an array is assigned to another variable the whole array is copied. Similarly if you pass an array to a function it will receive a copy of the array, not a pointer to it.


creation and intialization can be done in singl e go using

```go
a := [3]int {1,2,3}

c := [...]int{2,3,4}
```

Multi-dimension arrays are also possible

```go
d := [2][2]int{ {1,2}, {3,4} }
```

We will almost never use them in Go, because there is something much more flexible: `slices`

## Slices

