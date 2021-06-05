# Documentation

* Each package can have documentation written as comment before the `package` statement.
* If a package has multiple files then any one of the file can have the comments.
* In bigger packages it is convention to have a separate file name `doc.go` which just hold the package statement with necessary comments.
* We can use `go doc packagename` for the package documentation along with the list of Public functions and types.
* Similar to packages every function can also have documentation as a comment before the declaration. We can look at this doc using `go doc FunctionName`.

```go
// The even package implements a fast function for detecting if an integer
// is even or not.
package even
```

## Examples inside Documentation

* We can incorporate example functions inside documentation which can act as documents and as tests. Those example functions need to start with `Example`

```go
func ExampleEven() {
    if Even(2) {
        fmt.Printf("Is even\n")
    }
    // Output:
    // Is even
}
```
`Output:` will be used by tests to do comparision