# Beyond Basics

## Pointers

* Go has pointers but has no pointer arithmetic which means `*p++` will be interpreted as `(*p)++`(increment value being pointed by p) not to access value at next address byte like in **C** 
* During a function call, Go uses _pass-by-value_ so to improve performance and also to allow the function to change the passed value we need to use pointers.
* pointer are declared just like any other type but with a __*__ prefixes the type
* To make a pointer point to somthing can be done usig __&__ operator
```go
	var p *int
	fmt.Println(p)      // prints <nil>
	// fmt.Println(*p)  // causes run-time error 

	var i int

	p = &i
	fmt.Println(p)      // prints address of i
	fmt.Println(*p)     // prints value of i
```

## Allocation

In Go allocation is done using 2 primitive functions    .

### new
* Similar to new in other languages
* `new(T)` Allocates a zeroed storage of a new item of type T and returns its address `*T`

### make
* `make(T, args)` intilaizes (not zero) value of type T and returns it (not pointer).
* Only useful for creating arrays, slices and channels.

The reason for two different functions is because the 3 types intialized by `make` uses data-structures under the hood and need to be intialized before being used.

Also `make([]int, 10, 100)` allocates an array of 100 ints and then creates a slice structure with length 10 and a capacity of 100 pointing at the first 10 elements of the array. In contrast, `new([]int)` returns a pointer to a newly allocated, zeroed slice structure, that is, a pointer to a nil slice value

## Constructors

For a type, instead of creating a new instance with `new` and then assigning values to each field we can simplify using _composite literal_

```go
func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    return &File{fd, name, nil, 0}
}
```

Infact all the arrays, slices we have created earlier uses the same technique

```go
ar := [...]string{Enone: "no error", Einval: "invalid argument"}
sl := []string{Enone: "no error", Einval: "invalid argument"}
ma := map[int]string {Enone: "no error", Einval: "invalid argument"}
```
## Defining own types

Go allows the creation of new types using `type` keyword. For ex `type foo int` will create a new type called _foo_ which acts like _int_. For more sophisticated types we can use `struct`.

```go
type Person struct {
    name string
    age int
}

func main() {
    a := new(Person)
    a.name = "Robert"
    a.age = 20
    fmt.Println("Person is %v", a)

    b := Person{"Rahim", 30} // simplified and idomatic
    fmt.Println("Another Person is %v", b)
}
```
* Each item in a structure is called a field.
* If no name is provided to a field then they are called anonymous field.

### Methods

We can create function that work on a custom types using 2 ways

1. Create a function that takes the type as an argument
```go
func doWork(p Person, arg1 int) { /* */}
```
2. Create a function that works on that type
```go
func (p *Person) doWork(arg1 int) { /* */}
```
The second one is a method which can be used directly on the type as shown below

```go
p := Person{"Robert", 23}
p.doSomething(20)
```

this way of method call works even if p is a pointer of type Person instead of type Person. The reason this works is Go will translate `p.doSomething(20)` to `(&p).doSomething(20)`.

There is a subtle but major difference between the following type declarations
```go
// A Mutex is a data type with two methods, Lock and Unlock.
type Mutex struct         { /* Mutex fields */ }
func (m *Mutex) Lock()    { /* Lock impl. */ }
func (m *Mutex) Unlock()  { /* Unlock impl. */ }
```

We now create two types in two different manners:

```go
type NewMutex Mutex.
type PrintableMutex struct{Mutex}.
```
Here NewMutex is equal to Mutex, but it does not have any of the methods of Mutex. In other words its method set is empty. But PrintableMutex has inherited the method set from Mutex. The Go term for this is embedding.

The method set of *PrintableMutex contains the methods Lock and Unlock bound to its anonymous field Mutex.

## Conversions

| From      | `b []byte`  | `i []int`   | `r []rune`  | `s string`  | `f float32` | `i int`      |
| --------- | ----------- | ----------- | ----------- | ----------- | ----------- | ------------ |
| To        |             |             |             |             |             |              |
| `[]byte`  | -           |             |             | `[]byte(s)` |             |              |
| `[]int`   |             | -           |             | `[]int(s)`  |             |              |
| `[]rune`  |             |             |             | `[]rune(s)` |             |              |
| `string`  | `string(b)` | `string(i)` | `string(r)` | -           |             |              |
| `float32` |             |             |             |             | -           | `float32(i)` |
| `int`     |             |             |             |             | `int(f)`    | -            |