# Go Cheat Sheet

![Go](https://img.shields.io/badge/go-%2300ADD8.svg?style=for-the-badge&logo=go&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-blue.svg?style=for-the-badge)
![Version](https://img.shields.io/badge/version-1.23-00ADD8.svg?style=for-the-badge)

A comprehensive reference guide for Go programming language essentials.

## Table of Contents

- [Basic Syntax](#basic-syntax)
- [Data Types](#data-types)
- [Control Flow](#control-flow)
- [Functions](#functions)
- [Pointers](#pointers)
- [Structs](#structs)
- [Interfaces](#interfaces)
- [Error Handling](#error-handling)
- [Goroutines & Channels](#goroutines--channels)
- [Packages](#packages)
- [Testing](#testing)
- [Useful Commands](#useful-commands)

---

## Basic Syntax

### Hello World

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

### Variables

```go
// Declaration with type
var name string = "Alice"
var age int = 25

// Type inference
var city = "NYC"

// Short declaration (inside functions only)
count := 42
isActive := true

// Multiple declarations
var x, y int = 1, 2
a, b := "hello", 3.14

// Constants
const PI = 3.14159
const (
    StatusOK = 200
    StatusNotFound = 404
)

// Zero values
var i int        // 0
var f float64    // 0.0
var b bool       // false
var s string     // ""
```

---

## Data Types

### Basic Types

```go
bool                    // true, false
string                  // "hello"
int, int8, int16, int32, int64
uint, uint8, uint16, uint32, uint64
byte                    // alias for uint8
rune                    // alias for int32 (Unicode code point)
float32, float64
complex64, complex128
```

### Type Conversion

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

// String conversion
s := string(65)  // "A"
i := int(rune('A'))  // 65
```

### Arrays

```go
// Arrays (fixed size)
var arr [5]int
arr = [5]int{1, 2, 3, 4, 5}
arr := [...]int{1, 2, 3}  // size inferred

// Access elements
first := arr[0]
arr[1] = 10

// Array length
len(arr)  // 5
```

### Slices

```go
// Slices (dynamic size)
slice := []int{1, 2, 3}
slice = append(slice, 4, 5)
slice = make([]int, 5)      // length 5, capacity 5
slice = make([]int, 3, 10)  // length 3, capacity 10

// Slicing
sub := slice[1:3]   // elements 1 and 2
sub = slice[:3]     // first 3 elements
sub = slice[2:]     // from index 2 to end
sub = slice[:]      // copy of entire slice

// Length and capacity
len(slice)  // number of elements
cap(slice)  // capacity

// Copy slice
dst := make([]int, len(slice))
copy(dst, slice)
```

### Maps

```go
// Declaration
ages := make(map[string]int)
ages["Alice"] = 25
ages["Bob"] = 30

// Literal initialization
scores := map[string]int{
    "Alice": 90,
    "Bob":   85,
}

// Check if key exists
value, exists := ages["Alice"]
if exists {
    fmt.Println(value)
}

// Delete key
delete(ages, "Alice")

// Iterate
for key, value := range ages {
    fmt.Println(key, value)
}
```

### Strings

```go
// String basics
s := "hello"
s = `multi
line
string`

// String operations
len(s)              // length
s[0]                // first byte
s[1:4]              // substring

// Common string functions
import "strings"

strings.Contains("hello", "ll")     // true
strings.Split("a,b,c", ",")         // ["a", "b", "c"]
strings.Join([]string{"a", "b"}, ",")  // "a,b"
strings.ToUpper("hello")            // "HELLO"
strings.ToLower("HELLO")            // "hello"
strings.TrimSpace("  hello  ")      // "hello"
strings.HasPrefix("hello", "he")    // true
strings.HasSuffix("hello", "lo")    // true
strings.Replace("hello", "l", "L", -1)  // "heLLo"
```

---

## Control Flow

### If-Else

```go
if x > 10 {
    fmt.Println("Greater")
} else if x > 5 {
    fmt.Println("Medium")
} else {
    fmt.Println("Small")
}

// If with initialization
if err := doSomething(); err != nil {
    fmt.Println("Error:", err)
}

// Short variable declaration in if
if v := getValue(); v < 10 {
    fmt.Println(v)
}
```

### Switch

```go
// Basic switch
switch day {
case "Monday":
    fmt.Println("Start of week")
case "Friday":
    fmt.Println("Almost weekend")
default:
    fmt.Println("Regular day")
}

// Multiple values
switch day {
case "Saturday", "Sunday":
    fmt.Println("Weekend")
default:
    fmt.Println("Weekday")
}

// Switch without expression
switch {
case age < 18:
    fmt.Println("Minor")
case age < 65:
    fmt.Println("Adult")
default:
    fmt.Println("Senior")
}

// Type switch
switch v := i.(type) {
case int:
    fmt.Printf("Integer: %d\n", v)
case string:
    fmt.Printf("String: %s\n", v)
default:
    fmt.Printf("Unknown type\n")
}
```

### Loops

```go
// For loop (only loop keyword in Go)
for i := 0; i < 5; i++ {
    fmt.Println(i)
}

// While-style loop
for count < 10 {
    count++
}

// Infinite loop
for {
    // break to exit
    // continue to skip
}

// Range over slice/array
for index, value := range slice {
    fmt.Println(index, value)
}

// Range over map
for key, value := range myMap {
    fmt.Println(key, value)
}

// Range over string (iterates over runes)
for index, char := range "hello" {
    fmt.Printf("%d: %c\n", index, char)
}

// Ignore index/key with _
for _, value := range slice {
    fmt.Println(value)
}

// Only index
for index := range slice {
    fmt.Println(index)
}
```

---

## Functions

### Basic Functions

```go
func add(a int, b int) int {
    return a + b
}

// Same type parameters
func multiply(a, b int) int {
    return a * b
}

// Multiple return values
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

// Named return values
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return  // naked return
}

// Variadic functions
func sum(nums ...int) int {
    total := 0
    for _, num := range nums {
        total += num
    }
    return total
}

// Call variadic function
sum(1, 2, 3, 4)
nums := []int{1, 2, 3, 4}
sum(nums...)  // spread operator
```

### Anonymous Functions & Closures

```go
// Anonymous function
func() {
    fmt.Println("Anonymous")
}()

// Assign to variable
add := func(a, b int) int {
    return a + b
}
result := add(3, 4)

// Closure
func counter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}

c := counter()
fmt.Println(c())  // 1
fmt.Println(c())  // 2
```

### Defer

```go
// Defer executes at end of function
func example() {
    defer fmt.Println("World")
    fmt.Println("Hello")
}
// Prints: Hello, then World

// Multiple defers (LIFO order)
func example() {
    defer fmt.Println("1")
    defer fmt.Println("2")
    defer fmt.Println("3")
}
// Prints: 3, 2, 1

// Common use case: cleanup
func readFile() {
    f, err := os.Open("file.txt")
    if err != nil {
        return
    }
    defer f.Close()
    
    // work with file
}
```

---

## Pointers

```go
// Basic pointers
x := 42
p := &x         // pointer to x
fmt.Println(*p) // dereference: 42
*p = 21         // change value through pointer
fmt.Println(x)  // 21

// Pointer to struct
type Person struct {
    Name string
    Age  int
}

p1 := &Person{Name: "Alice", Age: 25}
p1.Name = "Bob"  // automatic dereferencing
(*p1).Age = 26   // explicit dereferencing

// New function (allocates memory)
p2 := new(Person)
p2.Name = "Charlie"
```

---

## Structs

### Define & Use

```go
// Define struct
type Person struct {
    Name string
    Age  int
    City string
}

// Create struct
p1 := Person{Name: "Alice", Age: 25, City: "NYC"}
p2 := Person{"Bob", 30, "LA"}  // positional
var p3 Person                   // zero values

// Access fields
fmt.Println(p1.Name)
p1.Age = 26

// Anonymous struct
p := struct {
    Name string
    Age  int
}{
    Name: "Alice",
    Age:  25,
}
```

### Methods

```go
type Rectangle struct {
    Width, Height float64
}

// Value receiver
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Pointer receiver (can modify)
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

// Usage
rect := Rectangle{Width: 10, Height: 5}
fmt.Println(rect.Area())
rect.Scale(2)
```

### Embedded Structs

```go
type Address struct {
    Street string
    City   string
}

type Person struct {
    Name    string
    Age     int
    Address // embedded struct
}

p := Person{
    Name: "Alice",
    Age:  25,
    Address: Address{
        Street: "123 Main St",
        City:   "NYC",
    },
}

// Access embedded fields
fmt.Println(p.Street)  // promotes Address.Street
fmt.Println(p.City)    // promotes Address.City
```

---

## Interfaces

### Define & Implement

```go
// Define interface
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Implement interface (implicit)
type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return 3.14 * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * 3.14 * c.Radius
}

type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

// Use interface
func printArea(s Shape) {
    fmt.Println("Area:", s.Area())
}

c := Circle{Radius: 5}
r := Rectangle{Width: 3, Height: 4}
printArea(c)
printArea(r)
```

### Empty Interface

```go
// Empty interface (any type)
var i interface{}
i = 42
i = "hello"
i = true

// Type assertion
s := i.(string)
s, ok := i.(string)  // safe type assertion
if ok {
    fmt.Println(s)
}

// Type switch
switch v := i.(type) {
case int:
    fmt.Println("Integer:", v)
case string:
    fmt.Println("String:", v)
case bool:
    fmt.Println("Boolean:", v)
default:
    fmt.Println("Unknown type")
}
```

### Common Interfaces

```go
// Stringer interface
type Stringer interface {
    String() string
}

type Person struct {
    Name string
    Age  int
}

func (p Person) String() string {
    return fmt.Sprintf("%s (%d)", p.Name, p.Age)
}

// Error interface
type error interface {
    Error() string
}
```

---

## Error Handling

### Basic Error Handling

```go
// Return error
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

// Check error
result, err := divide(10, 0)
if err != nil {
    fmt.Println("Error:", err)
    return
}
fmt.Println(result)
```

### Custom Error Types

```go
// Custom error type
type MyError struct {
    Msg  string
    Code int
}

func (e *MyError) Error() string {
    return fmt.Sprintf("Error %d: %s", e.Code, e.Msg)
}

// Return custom error
func doSomething() error {
    return &MyError{
        Msg:  "something went wrong",
        Code: 500,
    }
}

// Type assertion on error
if err := doSomething(); err != nil {
    if e, ok := err.(*MyError); ok {
        fmt.Printf("Code: %d, Message: %s\n", e.Code, e.Msg)
    }
}
```

### Error Wrapping

```go
import "errors"

// Wrap error
err := doSomething()
if err != nil {
    return fmt.Errorf("failed to do something: %w", err)
}

// Unwrap error
originalErr := errors.Unwrap(err)

// Check if error is specific type
if errors.Is(err, os.ErrNotExist) {
    fmt.Println("File does not exist")
}
```

### Panic and Recover

```go
// Panic (unrecoverable error)
panic("something bad happened")

// Recover (catch panic)
func safe() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered:", r)
        }
    }()
    panic("oops")
}
```

---

## Goroutines & Channels

### Goroutines

```go
// Start goroutine
go doSomething()

// Anonymous function
go func() {
    fmt.Println("Running concurrently")
}()

// With parameters
go func(msg string) {
    fmt.Println(msg)
}("hello")
```

### Channels

```go
// Create channel
ch := make(chan int)
buffered := make(chan int, 10)

// Send to channel
ch <- 42

// Receive from channel
value := <-ch

// Close channel
close(ch)

// Range over channel
for value := range ch {
    fmt.Println(value)
}

// Check if channel is closed
value, ok := <-ch
if !ok {
    fmt.Println("Channel closed")
}
```

### Select Statement

```go
select {
case msg := <-ch1:
    fmt.Println("Received from ch1:", msg)
case msg := <-ch2:
    fmt.Println("Received from ch2:", msg)
case ch3 <- value:
    fmt.Println("Sent to ch3")
case <-time.After(1 * time.Second):
    fmt.Println("Timeout")
default:
    fmt.Println("No communication ready")
}
```

### WaitGroup

```go
import "sync"

var wg sync.WaitGroup

for i := 0; i < 5; i++ {
    wg.Add(1)
    go func(id int) {
        defer wg.Done()
        fmt.Println("Worker", id)
    }(i)
}

wg.Wait()  // Wait for all goroutines
```

### Mutex

```go
import "sync"

var (
    counter int
    mutex   sync.Mutex
)

func increment() {
    mutex.Lock()
    counter++
    mutex.Unlock()
}

// Or use defer
func increment() {
    mutex.Lock()
    defer mutex.Unlock()
    counter++
}

// RWMutex for read-heavy workloads
var rwMutex sync.RWMutex

rwMutex.RLock()    // read lock
defer rwMutex.RUnlock()

rwMutex.Lock()     // write lock
defer rwMutex.Unlock()
```

---

## Packages

### Import

```go
// Single import
import "fmt"

// Multiple imports
import (
    "fmt"
    "math"
    "strings"
)

// Aliased import
import (
    f "fmt"
    m "math"
)

// Blank import (for side effects)
import _ "image/png"
```

### Package Declaration

```go
// File: mypackage/utils.go
package mypackage

// Exported (starts with capital letter)
func PublicFunction() {
    // ...
}

// Unexported (starts with lowercase)
func privateFunction() {
    // ...
}
```

### Init Function

```go
func init() {
    // Runs automatically before main()
    // Used for initialization
}
```

### Common Standard Library Packages

```go
// fmt - Formatting
fmt.Println("Hello")
fmt.Printf("Name: %s, Age: %d\n", name, age)
fmt.Sprintf("Formatted: %v", value)

// strings - String operations
strings.Contains("hello", "ll")
strings.Split("a,b,c", ",")
strings.Join([]string{"a", "b"}, ",")

// strconv - String conversion
i, err := strconv.Atoi("42")
s := strconv.Itoa(42)

// time - Time operations
now := time.Now()
fmt.Println(now.Format("2006-01-02 15:04:05"))
time.Sleep(2 * time.Second)

// io/ioutil - I/O operations
data, err := ioutil.ReadFile("file.txt")
err := ioutil.WriteFile("file.txt", []byte("content"), 0644)

// os - Operating system
os.Getenv("PATH")
os.Setenv("KEY", "value")
os.Exit(1)

// net/http - HTTP client/server
resp, err := http.Get("https://example.com")
http.ListenAndServe(":8080", handler)

// encoding/json - JSON
json.Marshal(data)
json.Unmarshal([]byte(jsonStr), &data)
```

---

## Testing

### Basic Tests

```go
// File: math_test.go
package main

import "testing"

func TestAdd(t *testing.T) {
    result := add(2, 3)
    expected := 5
    if result != expected {
        t.Errorf("Expected %d, got %d", expected, result)
    }
}

func TestDivide(t *testing.T) {
    result, err := divide(10, 2)
    if err != nil {
        t.Fatalf("Unexpected error: %v", err)
    }
    if result != 5 {
        t.Errorf("Expected 5, got %f", result)
    }
}
```

### Table-Driven Tests

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive", 2, 3, 5},
        {"negative", -1, -1, -2},
        {"zero", 0, 5, 5},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := add(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("Expected %d, got %d", tt.expected, result)
            }
        })
    }
}
```

### Benchmarks

```go
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        add(2, 3)
    }
}
```

### Examples

```go
func ExampleAdd() {
    result := add(2, 3)
    fmt.Println(result)
    // Output: 5
}
```

---

## Useful Commands

```bash
# Run program
go run main.go

# Build executable
go build
go build -o myapp

# Run tests
go test
go test -v              # verbose
go test -cover          # coverage
go test -bench=.        # benchmarks

# Module management
go mod init myproject   # initialize module
go mod tidy             # clean up dependencies
go get package          # download package
go get -u package       # update package

# Code quality
go fmt                  # format code
go vet                  # check for errors
go doc package          # view documentation

# Install tools
go install package

# List dependencies
go list -m all

# Clean build cache
go clean -cache
```

---

## Best Practices

### Code Organization

- Use meaningful package names
- Keep packages focused and cohesive
- Avoid circular dependencies
- Use internal packages for implementation details

### Error Handling

- Always check errors
- Provide context when wrapping errors
- Use custom error types when needed
- Don't panic unless absolutely necessary

### Concurrency

- Don't communicate by sharing memory; share memory by communicating
- Use channels to pass ownership
- Use sync primitives for protecting shared state
- Always use WaitGroups to wait for goroutines

### Performance

- Use buffered channels when appropriate
- Preallocate slices with known capacity
- Use pointers for large structs
- Profile before optimizing

---

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This cheat sheet is available under the MIT License.

## Resources

- [Official Go Documentation](https://golang.org/doc/)
- [A Tour of Go](https://tour.golang.org/)
- [Effective Go](https://golang.org/doc/effective_go)
- [Go by Example](https://gobyexample.com/)
- [Go Standard Library](https://pkg.go.dev/std)

---

**Last Updated:** January 2026
