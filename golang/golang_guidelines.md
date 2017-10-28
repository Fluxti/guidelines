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

Both can be obtained using len(s) , cap(s)

Length can be increased by reslicing it .

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



