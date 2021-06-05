# Packages

* A `package` is a collection of functions and data
* `package` naming convention is to use all lowercase characters with single word names.
* only one `package` is allowed in a dir
* sub-packages inside a module has to imported as `import "module-name/sub-package"`
* once a package is imported it can be referenced in the program using `package.FunctionName`.
* We can provide an alternate name for a package using `import newname "packagename"`
* `package` within a another package will be refrenced using the package name directly. For example the `package` `compress/gzip` is imported as `import "compress/gzip"` and referenced in the program as `gzip.Read`
* Any declaration that starts with capital letter will be Public and others would be private to the package
* Capitals are not restricted to US-ASCII they extend to Latin, Greek, Cyrillic etc

```go
package even

func Even(i int) bool { // Public function
	return i%2 == 0
}

func odd(i int) bool { // Private function
	return i%2 == 1
}
```
## Useful packages

### fmt
* Includes formatted IO functions similar to C.
* Some verb sequences commonly used are
  * %v - the value in default format eg: `{Ganesh 31}`
  * %+v - adds field names eg: `{name:Ganesh age:31}`
  * %#v - Go's syntax representation of value eg: `main.test{name:"Ganesh", age:31}`
  * %T - Go's syntax representation of the type of the value eg: `main.test`

### io
* Provides basic interfaces for IO primitives in `os` package.

### bufio
* Provides buffered IO interfaces like `io.Reader` and `io.Writer`.

### sort
* Provides primitives for sorting arrays and user-defined collections

### strconv
* Provides utitlities for to and from string conversions

### os
* Provides platform-independent utilities for operating system functionalities.
### sync
* Provides basic synchronization primitives such as mutual exclusions
### flag
* Provides utilities for command-line flag parsing
### encoding/json
* Encoding and decoding of JSON
### html/template
* data driven html templates
### net/http
* http server utilities
### unsafe
* operations that step around type safety of Go
### reflect
* Run-time reflection
### os/exec
* Runs external commands