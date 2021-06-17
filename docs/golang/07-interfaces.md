# Interfaces


Go supports partial Duck Typing which means any type can satisfy an `interface` if it has the same methods 

```go
type I interface {
    Get() int
    Put(int)
}
```
Below type _S_ can be used as _I_ since it has the same methods

```go
type S struct { i int }
func (p *S) Get() int  { return p.i }
func (p *S) Put(v int) { p.i = v }
```

With this we can use any variable with type _S_ as _I_ like.
Also `Get()` and `Put()` functions here are called _methods_ since they have a _receiver_ type. _receiver_ types can never by native types, we have to create a custom type to be able to append methods to it.

```go
func f(i I) {
    fmt.Println(i.Get())
    i.Put(1)
}

s := S{10}
f(&s)
```

Note we are passing as pointer since the declaration of methods on _S_ is done so.

## Empty Interface

Since every type can satisfy an empty interface `interface{}` we can use this to create functions which can receive any type as input

```go
func g(s interface{}) int {
    return s.(I).Get()
}
```
Here we type cast variable _s_ to _I_ to be able to call `Get()` method

Eventhough this works if function _g_ is called with type that satisfies _I_, it will panic for other types since it won't be able to find `Get()` method. We can avoid this by validating the type before using it.

```go
func g(s interface{}) int {
    if t, ok := s.(I); ok {
        return t.Get()
    } else {
        return -1
    }
}
```

Other than this we can also get the type of a variable by using switch

```go
func f(p I) {
    switch p.(type) {       // Note: this works only in switch
        case *S:
        //  do-something
        case *R:
        //  do-something
        default:
        //  do-something
    }
}
```


## Additional Points
* One-method interfaces are generally named by the method name followe by _-er_ suffix. Ex: `Reader`, `Writer`, `Formatter`.
* A _receiver_ cannot receive an _interface_ type.