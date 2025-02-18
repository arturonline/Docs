# Pointers

A pointer holds the memory address of a value.

- `&` => We use the `&` ampersand operator to refer to a variable's memory address.
- `*` => We use the `*` operator to expose the underlying value of that memory address.

## Example

```go

// Declaring a pointer
var p *int

// Getting the Address of a Variable:
var a int = 42
p = &a

// Dereferencing a Pointer:
fmt.Println(*p) // Outputs: 42 
*p = 21 // Changes the value of a to 21

var q *int = &a
fmt.Println("address of a is", q)

// address of b is 0xc00002c008
```

```go
package main

import "fmt"

func main() {
    i, j := 42, 2701

    p := &i         // point to i
    fmt.Println(*p) // read i through the pointer
    // 43    

    *p = 21         // set i through the pointer
    fmt.Println(i)  // see the new value of i
    // 21

    p = &j         // point to j
    *p = *p / 37   // divide j through the pointer
    fmt.Println(j) // see the new value of j
    // 73
}
```

## Pointers to mutate values

When we call a function that takes an argument, that argument is copied to the function:

```go
func zero(x int) {
  x = 0
}

func main() {
  x := 5
  zero(x)
  fmt.Println(x) // x is still 5
}
```

In this program the zero function will not modify the original x variable in the main function. But what if we wanted to? One way to do this is to use pointers:

```go
func zero(xPtr *int) {
  *xPtr = 0
}

func main() {
  x := 5
  zero(&x)
  fmt.Println(x) // x is 0
}
```

>By using the '&' operator we can get a variable's address direction (x).
>
>By using the '*' operator we can get the value stored in an adress direction and change it (xPtr).

## Creating pointers using the new function

Another way to get a pointer is to use the built-in new function:

```go
func one(xPtr *int) {
  *xPtr = 1
}
func main() {
  xPtr := new(int)
  one(xPtr)
  fmt.Println(*xPtr) // x is 1
}
```

`new` takes a type as an argument, allocates enough memory to fit a value of that type and returns a pointer to it.

In some programming languages there is a significant difference between using `new` and `&`, with great care being needed to eventually delete anything created with new. Go is not like this, it's a garbage collected programming language which means memory is cleaned up automatically when nothing refers to it anymore.

Pointers are rarely used with Go's built-in types, but as we will see in the next chapter, they are extremely useful when paired with structs.

## Pointers in go

Pointers can be used to:

- Allow a function to directly mutate a value that is passed to it
- To increase performance in edge cases. Sometimes passing a large struct of data results in inefficient copying of data
- To signify the lack of a value. For example, when unmarshalling JSON data into a struct it can be useful to know if a key was absent rather than it being present with the zero value.