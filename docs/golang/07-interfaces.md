# Interfaces


Go supports partial Duck Typing which means any type can satisfy an `interface` if it has the same methods 

```go
type I interface {
    Get() int
    Put(int)
}
```
Below type __S__ can be used as __I__ since it has the same methods

```go
type S struct { i int }
func (p *S) Get() int  { return p.i }
func (p *S) Put(v int) { p.i = v }
```