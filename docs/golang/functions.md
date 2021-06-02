# Functions

## syntax

```go
type mytype int
func (p mytype) funcname(q int) (r,s int) {
    return 0,0
}
```

* `(p mytype)` is optional and is required when a function is defined as a method for a type
* the return values names are optional but acts as documentation
* functions can be declared in any order

Variables declared outside functions are global in Go, while those defined inside functions are local to those functions. If names overlap the local variable will override the global one

```go
var a int = 3

func main() {
    a := 5
    print(a) // prints 5
}
```

## functions as values

```go
func main() {
    a := func() {
        fmt.Println("Hello")
    }
    a()
}
```

functions can also be part of maps 

```go
var xs = map[int] func() int {
    1: func() int { return 10 },
    2: func() int { return 20 },
    3: func() int { return 30 },
}
```

## functions as callbacks

```go
func printit(x int) {
    fmt.Printf("%v\n", x)
}

func callback( y int, f func(int)) {
    f(y)
}
```

## deferred code

we can let a piece of code to be executed at the end of function with the use of `defer`

```go
func ReadWrite() bool {
    file.open("filename")
    defer file.close()
    // do somelogic
    return true
}
```

`defer` can be used multple times in a function and all the statements would be executed in reverse order

```go
for i:=0;i<5;i++ {
    defer fmt.Println(i) // will print 4,3,2,1,0
}
```

we can also use `defer` on a function 

```go
func f() int {
    defer func() {
        fmt.Println("End function")
    }() // () are required as we need it if we need call function with parameters

    return 0
}
```

`defer`red statements can also change the return value of a function. This is only possible when function has named return value

```go
func f() (r int) {
	defer func() {
		r = 2
	}()
	fmt.Println("Inside f")
	return 0
}
```

## variadic parameters

we can let functions take a variable no of parameters as variadic parameters. 

```go
func myfunc(arg ...int) {}
```

the variadic parameter would be a `slice` and can be used as the same.

> All the arguments should be of same type

## panic and recover

Go's error handling mechanism is different from other languages in which functions returns the error rather than throwing it. Hence `panic` and `recover` are last resorts and should be used sparingly.

__panic__ is a built-in function that stops the ordinary flow of control and begins panicking. When the function F calls panic, execution of F stops, any deferred functions in F are executed normally, and then F returns to its caller. To the caller, F then behaves like a call to panic. The process continues up the stack until all functions in the current goroutine have returned, at which point the program crashes. Panics can be initiated by invoking panic directly. They can also be caused by runtime errors, such as out-of-bounds array accesses.

__recover__ is a built-in function that regains control of a panicking goroutine. Recover is only useful inside deferred functions. During normal execution, a call to recover will return nil and have no other effect. If the current goroutine is panicking, a call to recover will capture the value given to panic and resume normal execution.

```go
func panicy() (r bool) {
	defer func() {
		if x := recover(); x != nil {
			fmt.Println(x)
			r = true
		}
	}()

	var a []int
	a[3] = 10
	return
}
```