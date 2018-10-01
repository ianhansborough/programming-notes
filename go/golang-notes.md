# Go Notes
#code #cs

## File Structure
### Package Main   

When you build reusable pieces of code, you will develop a package as a shared library. But when you develop executable programs, you will use the package “main” for making the package as an executable program. The package “main” tells the Go compiler that the package should compile as an executable program instead of a shared library. The main function in the package “main” will be the entry point of our executable program. When you build shared libraries, you will not have any main package and main function in the package.

## Literals
go is statically typed

#### Integers
#### float32 (single)
#### float64 (double) (generally use)
#### complex64
#### complex128

#### Strings
may be created with double quotes or backticks.  double quotes cannot contain new line’s. Use JS-style escape syntax `\n \t`

#### Booleans - &&, ||, !, ==, 

## Variables
variables need to have the type declared `var x string = "foo bar"`
however, you can use `:=` to assert the type.  `var x := "foo bar"`
`x := "foo bar"`
**Naming convention: use snake case**

## Constants 
`const x string = "Hello World"`
be default, constants can either be typed or untyped. this is important, as Go will not allow you to do operations on two values that are of different types.

## Iota
iota is used as a sort of self incrementing index. Within a given block, the first time iota is referenced, it will be 0, then 1, then 2, and so on. 

## Runes
a rune is a character
an integer value identifying a unicode code point
a numeric type that is an alias for int32
` 'a' //this is a rune. use single quotes`

## Control Structures
### For loops: 
similar to C for loops (no parens needed)
`for i := 0; i < 100; i++ { // do something }`
`for i < 10 { // do something }`

### For Range Loops
`for _, value := range x { total += value }`
note here that the `_` denotes a placeholder for a variable that is never used. Go will throw an error if you create a variable that is never used

### If Statements
**If:** similar to JS (no parens needed)
```
if true {
	fmt.Println("this will print")
}
if false {
	fmt.Println("this will not print")
}
else if !false {
	fmt.Println("this will print")
}

```

#### Initialization statement
```
if err := file.Chmod(0664); err != nil {
	fmt.Println("there was an error")
}
```

### Switch cases
no default fall through for switch cases in Go (no need for break statements after each case)
```
switch some_val {
case "Tim", "Jenny":
	fmt.Println("Yo T Dawg or J Dawg")
case "FallThrough":
	fmt.Println("will fall through")
	fallthrough
case "FallsToHere":
	fmt.Println("this is for fallthrough")
```

#### Switching on Type
```
switch x.(type) {
case bool:
	fmt.Println("is BOOL")
case string:
	fmt.Println("is STRING")
default:
	fmt.Println("something else")
}
```

## Arrays
`var x [5]int`
`x := [5]float64 { 98, 95, 49, 53, 92, }`
Note the trailing `,`  on the last element of the array is required by Go.
Arrays are not dynamic in Go
All elements must be of the same type
You won’t ever really use these by themselves

## Slices
A slice is a segment of an array. Unlike arrays, slices can have variable length
`var x []float64` initialize a slice with length 0
you should use the built-in make function to create a slice
`make([]T, length, capacity)`
`x := make([]float64, 5, 10)`  
this creates a slice of length 5 that is associated with an underlying float64 array of capacity 10. Slices can be shorter than their associated array, but never longer. When a slice exceeds the capacity of it’s underlying array, Go with throw out the old array, create a new array in memory with double the capacity, and move the contents of the slice to the new underlying array.

### make
used to initialize any of the reference types. These types have headers that must be initialized by the `make()` function

### append
``` 
func main() {
	slice1 := []int{1,2,3,}
	slice2 := append(slice1, 4, 5,)
	slice3 := append(slice1, slice2...)
	fmt.Println(slice1, slice2, slice3)
	// slice1 = [1,2,3]
	// slice2 = [1,2,3,4,5]
	// slice3 = [1,2,3,1,2,3,4,5]
}
```

### delete 
```
slice1 := []int{1,2,3,4,5}
slice2 := append(slice1[:2], slice[3:]...)
// slice2 = [1,2,4,5]
```

### copy
```
func main() {
	slice1 := []int{1,2,3}  // [1,2,3]
	slice2 := make([]int,2) // [0,0]
	copy(slice2, slice1) 
	fmt.Println(slice1, slice2)
	// slice2 = [1,2] since it only had room for two elements
}	
```

## Maps
* Definition: an unordered group of elements of one type, called the element type
* the value of an initialized map is nil, b/c it is also a reference type
```
x := make(map[string]int) // x is a map of strings to ints
x["key"] = 10
fmt.Println(x["key"])
// >>> 10

// shorter way to create a map (composite literal)
elements := map[string]string{
	"F": "foo",
	"B": "bar",
}

```

you can delete an item from a map using `delete(x, "key")`  

if a map value lookup is not assigned, it will return the zero value for that value type. you can check for this using this syntax:
```
if name, ok := elements["Un"]; ok {
	fmt.Println(name, ok)
} 
// a map lookup returns up to two values. the second one tells whether or not the lookup was sucessful
// first try to get a value from the map for "Un", if succcessful, execute block
```

you can nest maps to form model nested data
```
elements := map[string]map[string]string {
	"H": map[string]string{
      "name":"Hydrogen",
      "state":"gas",
    },
    "He": map[string]string{
      "name":"Helium",
      "state":"gas",
    },
		...
		...
}
```

### Map Range Loops
```
myMap := map[string]string{
	"zero": "hello"
	"one": "there"
}
for key, val := range myMap {
	fmt.Println(key, " - ", val)
}
```

## Functions (procedures, subroutines)
```
func average(foo []float64) float64 {
	panic("Not Implemented")
}
```

functions can return multiple values
```
func f() (int, int) {
	return 5, 6
}
func main() {
	x, y := f() // x == 5 and y == 6
}
```

### Functions with Variadic Params
the last parameter in a Go function can be variadic, meaning that it can accept one or more of this argument. _This is what  fmt.Println uses_
```
func add (args ...int) int {
	total := 0
	for _, val := range args {
		total += val
	}
	return total
}
```

### Variadic  Args
```
func main() {
	data := []float64{43,56,76,92,42}
	n := average(data...)
	// this deconstructs data as a variadic argument
}

func average(sf ...float64) {
	// calc the average
}

```

### Closures
A function inside of another function with a that utilizes variables found in the scope of the parent function is called a **Closure**
```
func makeEvenGenerator() func() uint {
  i := uint(0)
  return func() (ret uint) {
    ret = i
    i += 2
    return
  }
}
func main() {
  nextEven := makeEvenGenerator()
  fmt.Println(nextEven()) // 0
  fmt.Println(nextEven()) // 2
  fmt.Println(nextEven()) // 4
}
```

### Callbacks
```
func visit (numbers []int, callback func(int)) {
	for _, n := range numbers {
		callback(n)
	}
}
```

### Recursion
A recursive function is one that calls itself. A good example of this is finding the factorial
```
func factorial(x int) int {
	if x == 0 {
		return 1
	}
	return x * factorial(x-1)
}
```

### Defer
a keyword that schedules a function call to be run after a function completes
```
func main() {
	defer second() // called second
	first() // called first
}
```

useful for taking care of something at the end no matter where the function returns / what happens in the rest of the function (i.e closing a file)

### Panic + Recover
panic causes a runtime error. recover takes the argument that you pass to panic and executes a block to recover from the panic. Must be used like so:
```
func main() {
	defer func() {
		str := recover()
		fmt.Println(str)
	}
	panic("PANIC")
}
```

## Pointers
_Go always passes by value by default. must use pointers to pass a reference_

`&` gets the memory address associated with a variable
`*` when *used on a type* like  `*int` means the type is a memory address that points to that type of variable (pointer)
`*` when *used on a pointer* dereferences a memory address to produce the value stored there.
```
func zero(xPtr *int) {
	*xPtr = 0
}
func main() {
	x := 5
	zero(&x)
}
```
in Go, you can also use `xPtr := new(int)` to return a pointer to an address in memory that has been allocated for an int. this works fine too

## Structs
* A sequence of named elements, called fields. Each field has a name and a type

```
type Circle struct {
	x float64
	y float64
	r float64
}
type Circle struct {
	x, y, r, float64 // more terse way of writing the above.
}
```

initialize an empty struct as follows:
```
var c Circle
c := new(Circle)
```

initialize a struct with values as follows: 
```
c := Circle{x: 0, y: 0, r: 5}
c := Circle{0, 0, 5} // if you know what order the fields were defined in
```

access fields using the `.` operator

### Tags
can be used in a struct definition to describe how the data in that struct get’s turned into JSON and read in from JSON

### Methods 
a method is  a special type of function that also has a receiver. this tells Go to only make that method callable from a certain type of struct, and automatically pass in a pointer reference to the struct that invokes the method.
```
type rectangle struct {x1, y1, x2, y2, float64}
func (r *Rectangle) area() float64 {
	return (r.x2 - r.x1) * (r.y2 - r.y1)
}
```

### Embedded Types
by including just the type of another struct with no name, you are embedding the Person type into the Android type. this means that Android has access to all of the methods present on Person.
```
type Android struct {
	Person
	Model string
}
```

### JSON + Structs
Marshal - takes an interface and marshal’s it to a JSON encoded string
* struct fields MUST be capitalized (exported) if you want them to show up when you marshal the struct to a JSON string.
Unmarshal - takes a JSON encoded string and unmarshals it to an interface
Encode - takes an interface and encodes it to a JSON stream
Decode - takes a JSON stream and decodes it to an interface

## Interfaces
Interfaces allow us to do polymorphisms “substituability”
An interface is like a protocol. it defines a set of functionality that a given type must have if it is to implement that interface.

* This allows you to introduce flexibility into your code. A function that prints area accepts an interface of type Shape as it’s sole argument. 

* The shape interface implements an area function. 

* Both the types circle and square have access to an area method, but these methods are DIFFERENT. But, since both types have access to an area function, they both implement the Shape interface and thus can both be used as arguments to the function mentioned above.

## Concurrency 
### Goroutines
A groutine is a function that is capable of running concurrently with other functions.
```
func main() {
	go f0()
	f1()
}
```

### Channels
channels let multiple goroutines communicate with one another and synchronize their execution


## Packages
* Packages are not executable and do not have a `func main() {}`  declaration. 
* When you import the package, you must use the entire qualified path to the package inside of your workspace (GitHub.com/GoesToEleven/…)
* import packages with 
```
import (
	"fmt"
	"github.com/GoesToEleven/GolangTraining/someFile"
)
```

* The folder that houses a package must match the name of the package declaration in the file
* Files inside the same package folder are all accessible to one another. no need to import
* camel-case methods within a package are not visible on export
* Pascal-case methods within a package are visible on export 

## Commands
`go run file.go`  runs an executable go file
`go build` builds an executable binanry file from an executable go file.
`go clean` removes old binaries/builds and other junk that doesn’t need to be there
`go install` builds an executable binary file from an executable go file and puts it in $GOPATH/bin

## Scope
universe - 
package
file
block
Keep your scope tight

## Blank Identifier