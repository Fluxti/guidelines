# Golang guidelines

## Go style guide 

Follow the [effective go](https://golang.org/doc/effective_go.html) style guide and use as a supplement the [go code review annotations](https://github.com/golang/go/wiki/CodeReviewComments)

## API documentation

Use the [swagger](https://github.com/go-swagger/go-swagger) implementation to document the APIS.

## Quick reference 

# Packages variables and functions

### Basic data types 

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

### Default values

```
0 for numeric types,
false for the boolean type, and
"" (the empty string) for strings.
nil for empty pointers
```

## Constants 

Constants can be character , string , boolean or numeric values.

```
const constantName = value 
```

## Declaration blocks

variables ,constants and imports can be declared using a declararion block using \n as a separation
var()
const()
import()

## Imports 

Programs start running in package ```main```
Package name is the same as the last element of the import path e.g. ```math/rand```

## Exported names 

Names are exported to use outside the package   start with capital letter.

## Functions

The program runs on the ```main``` function.

The syntax for creating functions is the following 

```
func functionName(paramNames type ) ( [returnVariables] returnTypes){
	statements
}
```

## Brackets

{} brackets are mandatory for each control statement

## Variables

Declaration of list of variables 
```var variable1,variablen type```

Initializers
```var variable1,variablen = value1,value2```

Short variable declarations
Inside a function the assignment := can be used as an declaration with an implicit type .

## Type conversions

The expression T(variable) converts the variable to the type T.

var variable type = type(value) //explicit conversion
variable := type(value) //implicit conversion

# Flow control 

## For syntax

	for i:= 0; sum < 1000;i++ {
		sum += sum
	}

init and post statement are optional , condition is mandatory

## While syntax

	for condition {
		statements
	}

## If statements 

	if condition {
		statements
	}



## If initializator

    	if statement ; condition {
		statement
	}else {
        statement
    }

## switch 

switch case ends automatically unless it finds a ```fallthroug``` statement

```
switch statement ; variable {
    case :
    default : 
}
```

Switch with no condition evaluates as switch true this is used to write long if-then-else 

```
switch {
	case condition :
	case condition:
	default:
}
```

## Defer

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns. 

```defer function(params)```

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order. 

## Pointer

The type *T is a pointer to a T value. Its zero value is nil. 

```
var p *int
```

The & operator generates a pointer to its operand. 

```
i := 42
p = &i
``` 
The * operator denotes the pointer's underlying value.

```
fmt.Println(*p) // read i through the pointer p
*p = 21   
```

## Structs

A struct is a collection of fields

```
	type StructName struct{
		VariableName type = value 
		VariableName type = value 
	}
```

### Initialization 

```
StructName{value,value,...}
```

Struct fields are accesed using a dot.

```
v  := Square{Vertex{1,2},Vertex{3,4}}
fmt.Print(v.First.X)
```

### Pointers to structs

Structs fields can be accesed through a struct pointer. Instead of doing *p.X , do p.X.

```
	v := Vertex{1,2}
	p := &v
	p.X = 2 
```

### Struct literals

A struct literal denotes a newly allocated struct value by listing the values of its fields. 

```
Vertex{X:1} // Y:0
Vertex{Y:2} // X:0
Vertex{} // X:0 , Y:0
```

## Arrays

The type [n]T is an array of n values of type T. The size is mandatory to declare.

```
var a [10]int
```

Initialization 
```
array := [2]int{1,2}
array[0] = 1

var array = [2]string {"hi","lol"}
```

## Slices

A slice is a dinamically-sized , flexible view into the elements of an array. It creates a subarray reference to an array.

The type []T is an slice with element of type T

A slice is formed by specifying two indices a low and high bound , separated with a colon :

``` a[low:high]```

This selects a half-open rage which includes first element , but excludes the last one .

Slices are references to arrays , it does not store any data .

### Slice literals

Slice literals is like an array without the length

```[]bool{true,false,true}```

### Slice defaults

When slicing you can omit the high or low bounds to use their defaults .
Default for lower is 0 and for upper the length of the array . 

Following expressing are equivalent

```
var a[10]int
a[0:10]
a[:10]
a[0:]
a[:]
```

### Slice length and capacity

The length of the slice is the number of elements it contains. 
The capacity is the number of elements in the underlying array , counting from the first element in the slice 

Both can be obtained using ```len(s) , cap(s)```

Length can be increased by reslicing it .

To increase a slice length to the original capacity

```s = s[:cap(s)]```



### Nil slices 

A nil slice has a length and capacity of 0 and has no underlying array

var a  []int

### Creating a slice with make

Make creates a dynamically-sized arrays , allocates a zeroed array and returns an slice that refers to that array.
```
a := make([]int, 5)  // len(a)=5 cap(a)=5
b := make([]int, 0, 5) // len(b)=0, cap(b)=5
``` 

### Slices of slices

Slices can contain any type including slices

### Appending to a slice 

```
func append(s []T, vs ...T) []T
```
The first parameter s of append is a slice of type T, and the rest are T values to append to the slice. 

```
	s = append(s, 2, 3, 4)
```

## Copying a slice from one part to another

To copy the contents of one slice to another use the ```copy(dst, src)``` function. Source cannot be bigger than destination in capacity.

You can copy from certain ranges of the slice the data ```copy(dst[:],src)``` 

### Range 

 The range form of the for loop iterates over a slice or map.

When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index. 

```
for i, v := range s {
}
```

You can skip the index or value by assigning to _

```
for _, v 
for i, _ 
```

## Maps 

Map maps references from key to values  . The zero value of a map is nil
A nil map has no key , nor keys can be added 

```var map map[type]type```

```make``` returns a map initialized and ready to use ```make(map[type]type)```

### Map literals

Map literals are like struct literals but keys must be provided  ```var map[type]type { key: value ,...,keyn:valuen} ``` 

### Mutating maps

Insert an element ```map[key] = element```
Retreive an element ```element = map[ley]```
delete an element ```delete(map,key)```
test if key is present with a two value assignment = ```element , isPresent = map[key]```
	If key is key is present , elements get assigned and isPresent turns true , otherwise , element is 0 and isPresent is false

## Function values 

Functions are values too  , they can be passed around like other values . They can be used as function arguments or return values . 

```
functionVariable := func(x,y type) type{
	
}

func functionName(fn func(x,y type) type ) type {
	value := x
	value2 := y
	fn(value1,value2)
}

functionName(functionVariable)
```

## Function closures

Go functions may be closures. A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables. 

```
func adder() func(int) int {
	sum := 0 //Sum is static to the scope of the function value
	return func(x int) int {
		sum += x
		return sum
	}
}

func main(){
	closure := adder()
	closure(1)
}
```

## Methods

Go does not have classes. However, you can define methods on types.

A method is a function with a special receiver argument.

The receiver appears in its own argument list between the func keyword and the method name.

```

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
	fmt.Println(Abs(v))
}

```

You can declare a method on non-struct types, too.
You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a ```receiver``` whose type is defined in another package (which includes the built-in types such as int).

```
type TypeName float64

func (f TypeName) sum(){
	if f 
	...
}

func main(){
	f := TypeName(1)
	f.sum()
}

``` 

Methods operate on a copy of the original receiver value . To change the reference , you must use pointer receivers.

## Pointer receivers

 The receiver type has the literal syntax *T for some type T. (Also, T cannot itself be a pointer such as *int.)

Pointer receivers are more common to than value receivers because methods often need to modify their receiver.

```
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```

## Pointers and functions

Methods can be written as functions that receive methods .

```
func Function(v *Vertex){
	...
}

func main(){
	Function(&v)
}

```

## Methods and pointer indirection

Functions with pointer argument must take a pointer

```
var v Vertex
ScaleFunc(v, 5)  // Compile error!
ScaleFunc(&v, 5) // OK
``` 

Methods with pointer receivers take either a value or a pointer as the recevier when they are called.

```
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```

Even tough v is a value not a pointer , go interprets the statement v. as (&v).

Functions that take a value argument must not be passed a pointer

## Choosing a value or pointer receiver

A pointer receiver is used for :

* A method can modify the value its receiver points to.

* Avoid copying the value on each method call , this can be more efficient if the receiver is a large struct.

## Interfaces

The interface type is defined as a set of method signatures .

A value of interface type can hold any value that implements those methods .

```
type InterfaceTypeName interface {
	Function/MethodName(arguments) returnType
}

var a InterfaceTypeName
b := TypeConstructor()

a = b // Because b implements Interface

fmt.Printf(a.methodName())

func (f Type) Function/MethodName(arguments) returnType {

}

```

## Interfaces are implemented implicitly

A type implements an interface by implementing its methods , there is no explicit declaration of intent , no "implements" keyword.

Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement. 

type I interface {
	M()
}

type T struct {
	S string
}

func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"hello"}
	i.M()
}


## Interface values

Under the covers, interface values can be thought of as a tuple of a value and a concrete type: 

```(value, type)```

An interface value holds a value of a specific underlying concrete type. 

Calling a method on an interface value executes the method of the same name on its underlying type. 

type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	fmt.Println(t.S)
}

i = &T{"Hello"}
fmt.Printf("(%v, %T)\n", i, i)

Result : 
(&{Hello}, *main.T)

## Interface values with nill underlying values

 If the concrete value inside the interface itself is nil, the method will be called with a nil receiver.

In some languages this would trigger a null pointer exception, but in Go it is common to write methods that gracefully handle being called with a nil receiver (as with the method M in this example.) 

Note that an interface value that holds a nil concrete value is itself non-nil. 

func (t *T) M() {
	if t == nil {
		fmt.Println("<nil>")
		return
	}
	fmt.Println(t.S)
}

func main() {
	var i I

	var t *T
	i = t
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}

Result(<nil>,*mainT)
<nil>

## Nil interface values

 A nil interface value holds neither value nor concrete type.

Calling a method on a nil interface is a run-time error because there is no type inside the interface tuple to indicate which concrete method to call.

type I interface {
	M()
}

func main() {
	var i I
	describe(i)
	i.M()
}

func describe(i I) {
	fmt.Printf("(%v, %T)\n", i, i)
}

Result : invalid memory address

## The empty interface

The interface type that specifies zero methos is know as the empty interface 

``` interface{} ```

 An empty interface may hold values of any type. (Every type implements at least zero methods.)

Empty interfaces are used by code that handles values of unknown type. For example, fmt.Print takes any number of arguments of type interface{}. 

var i interface{}
i=42
describe(i)

i="hello"
describe(i)

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}

Result 
(42,int)
(hello,string)

## Type assetions

 A type assertion provides access to an interface value's underlying concrete value.

t := i.(T)

 To test whether an interface value holds a specific type, a type assertion can return two values: the underlying value and a boolean value that reports whether the assertion succeeded.

t, ok := i.(T)

 If i holds a T, then t will be the underlying value and ok will be true.

If not, ok will be false and t will be the zero value of type T, and no panic occurs. 

	var i interface{} = "hello"

	s := i.(string)
	fmt.Println(s)

	s, ok := i.(string)
	fmt.Println(s, ok)

	f, ok := i.(float64)
	fmt.Println(f, ok)

	f = i.(float64) // panic
	fmt.Println(f)
}

## Type switches

 A type switch is a construct that permits several type assertions in series.

 A type switch is like a regular switch statement, but the cases in a type switch specify types (not values), and those values are compared against the type of the value held by the given interface value. 

 switch v := i.(type) {
case T:
    // here v has type T
case S:
    // here v has type S
default:
    // no match; here v has the same type as i
}

 The declaration in a type switch has the same syntax as a type assertion i.(T), but the specific type T is replaced with the keyword type.

This switch statement tests whether the interface value i holds a value of type T or S. In each of the T and S cases, the variable v will be of type T or S respectively and hold the value held by i. In the default case (where there is no match), the variable v is of the same interface type and value as i. 

## Stringers 

 One of the most ubiquitous interfaces is Stringer defined by the fmt package.

type Stringer interface {
    String() string
}

A Stringer is a type that can describe itself as a string. The fmt package (and many others) look for this interface to print values

## Errors

Go programs express error state with error values.

The error type is a built-in interface similar to fmt.Stringer:

type error interface {
    Error() string
}

(As with fmt.Stringer, the fmt package looks for the error interface when printing values.)

Functions often return an error value, and calling code should handle errors by testing whether the error equals nil.

i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
    return
}
fmt.Println("Converted integer:", i)

A nil error denotes success; a non-nil error denotes failure.

## Readers

 The io package specifies the io.Reader interface, which represents the read end of a stream of data.

The Go standard library contains many implementations of these interfaces, including files, network connections, compressors, ciphers, and others.

The io.Reader interface has a Read method:

func (T) Read(b []byte) (n int, err error)

Read populates the given byte slice with data and returns the number of bytes populated and an error value. It returns an io.EOF error when the stream ends.

The example code creates a strings.Reader and consumes its output 8 bytes at a time. 

func main() {
	r := strings.NewReader("Hello, Reader!")
	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}

## Images

 Package image defines the Image interface:

package image

type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}

Note: the Rectangle return value of the Bounds method is actually an image.Rectangle, as the declaration is inside package image.

(See the documentation for all the details.)

The color.Color and color.Model types are also interfaces, but we'll ignore that by using the predefined implementations color.RGBA and color.RGBAModel. These interfaces and types are specified by the image/color package 

func main() {
	m := image.NewRGBA(image.Rect(0, 0, 100, 100))
	fmt.Println(m.Bounds())
	fmt.Println(m.At(0, 0).RGBA())
}

# Concurrency

 A goroutine is a lightweight thread managed by the Go runtime.

go f(x, y, z)

starts a new goroutine running

f(x, y, z)

The evaluation of f, x, y, and z happens in the current goroutine and the execution of f happens in the new goroutine.

Goroutines run in the same address space, so access to shared memory must be synchronized. The sync package provides useful primitives, although you won't need them much in Go as there are other primitives. (See the next slide.) 


## Channels

Channels are a typed conduit through which you can send and receive values with the channel operator, <-.

ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.

(The data flows in the direction of the arrow.)

Like maps and slices, channels must be created before use:

ch := make(chan int)

By default, sends and receives block until the other side is ready. This allows goroutines to synchronize without explicit locks or condition variables.

The example code sums the numbers in a slice, distributing the work between two goroutines. Once both goroutines have completed their computation, it calculates the final result.


## Buffered Channels

Channels can be buffered. Provide the buffer length as the second argument to make to initialize a buffered channel:

ch := make(chan int, 100)

Sends to a buffered channel block only when the buffer is full. Receives block when the buffer is empty.

Modify the example to overfill the buffer and see what happens.


## Range and Close

A sender can close a channel to indicate that no more values will be sent. Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression: after

v, ok := <-ch

ok is false if there are no more values to receive and the channel is closed.

The loop for i := range c receives values from the channel repeatedly until it is closed.

Note: Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic.

Another note: Channels aren't like files; you don't usually need to close them. Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a range loop.

package main

import (
	"fmt"
)

func fibonacci(n int, c chan int) {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}

func main() {
	c := make(chan int, 10)
	go fibonacci(cap(c), c)
	for i := range c {
 		fmt.Println(i)
	}
 }


# Select

The select statement lets a goroutine wait on multiple communication operations.

A select blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple are ready. 

package main

import "fmt"

func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}

##  Default Selection

The default case in a select is run if no other case is ready.

Use a default case to try a send or receive without blocking:

package main

import (
	"fmt"
	"time"
)

func main() {
	tick := time.Tick(100 * time.Millisecond)
	boom := time.After(500 * time.Millisecond)
	for {
		select {
		case <-tick:
			fmt.Println("tick.")
		case <-boom:
			fmt.Println("BOOM!")
			return
		default:
			fmt.Println("    .")
			time.Sleep(50 * time.Millisecond)
		}
	}
}

# Where to start


# Configuration

Installing go
https://golang.org/dl/

Go documentation
https://golang.org/doc/

Screen cast of writing , building , installing and testing go code . 
https://www.youtube.com/watch?v=XCsL89YtqCs

How to write go code 
https://golang.org/doc/code.html

Package reference 
https://golang.org/pkg/

Language spec
https://golang.org/ref/spec

Go concurrency patterns
https://www.youtube.com/watch?v=f6kdp27TYZs

Avanced Go concurrency patterns
https://www.youtube.com/watch?v=QDDwwePbDtw

Share memory by communicating
https://golang.org/doc/codewalk/sharemem/

A simple programming environment
https://vimeo.com/53221558

Writing Web applications
https://vimeo.com/53221558

First class functions
https://golang.org/doc/codewalk/functions/

Go blog
https://blog.golang.org/



## Environment variables

Define a GOPATH variable on ~/.bash_profile or ~/.bashrc that references the go workspace .

export GOPATH="$HOME/go"

Export the bin directory to the path
export PATH="$HOME/go/bin:$PATH"

