# Testing

* Go has inbuilt testing support via the `testing` package and `go test` command
* Tests are named as `*_test.go`
* Each test function should have the same signature and shoult start with `Test` eg: `func TestXxx(t *testing.T)`.
* When writing a test we need to tell `go test` to fail explicitly. Anything which just returns will be considered as success.
* Some important functions from the testing package are
  * `func (t *T) Fail()` - Fails the test but continues test execution
  * `func (t *T) FailNow()` - Fails the test and stops the execution
  * `func (t *T) Log(args ...interface{})` - formats its args similar to `Print()` and records it in the error log.
  * `func (t *T) Fatal(args ...interface{})` - Similar to `Log()` followed by `FailNow()`

```go
package even

import "testing"

func TestEven(t *testing.T) {
	if !Even(2) {
		t.Log("2 should be even!")
		t.Fail()
	}
}
```

* Test file should be in same package. This allows us to test both Public and Private functions