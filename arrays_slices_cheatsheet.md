# Cheatsheet for arrays and slices in Go
When I was learning Go, with [Finnian A.](https://github.com/developius), we found that there were a lot of different ways to initialise arrays and slices.

Until I become even slightly good at Go, this guide will be a personal reference.

## General array/slice concepts
* An array in Go is fixed length. Once initialised, you can't change the element type or number of elements.
* A slice is an abstraction on top of an array. __Slices don't store any data__, they just refer to the underlying array.
* In most circumstances it's unnecessary to deal with arrays; slices are far easier.
* A slice can refer to any section of the underlying array, it doesn't have to refer to the whole thing

A slice has a length and a capacity.
* Length of the slice is, well, the length of the slice. _Not_ the length of the underlying array.
* Capacity of the slice refers to the **maximum size your slice can grow to before it won't fit** in the array it refers to.
* If the slice does run out of space, a new array will automatically be allocated behind the scenes by `append()`.

Here's an example, where our slice has a length of 2, but a capacity of 7
```
Slice:    | - | - |
Array:    | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
Capacity: < ------------------------- >
```
The capacity of 7 signifies that there are seven spaces in the underlying array which can be used.

Here's another example, where our slice starts at element 3:
```
Slice:             | - | - |
Array: | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
Capacity:          < ------------- >
```
The slice now has a length of 2, and a _capacity of 4_.

## Array initialisation
### Using `var`
To create an empty array, use the following syntax:
```go
var arr [3]string
```
This zero-values all the elements of the array.
Default values for different types:
```
integer -> 0
float -> 0.0
string -> ""
boolean -> false
everything else -> nil
```
### Using an assignment
To create an empty array:
```go
arr := [3]string{}
```
To create an array containing starting values:
```go
arr := [3]string{
  "foo",
  "bar",
  "baz",
}
```
This is just an example of assigning an _array literal_ to a variable.

Here's another example of an array literal:
```go
[5]int{58, 89, 1, -56, 182}
```
*Note*: Though the length of the array must be fixed, there's no reason why you should have to count it yourself!

Use `...` to make the compiler count for you, like so:
```go
arr := [...]int{58, 89, 1, -56, 182}
```

## Slice initialisation
### On top of an existing array
In the situation that an array already exists, it's dead easy to add a slice on top of it.

Of course, a slice can sit on top of the whole array, or just a section of it.
```go
// Our array
arr := [5]int{0, 1, 2, 3, 4}

// Our slice on top of the array (all of it)
slice := arr[:]
// Or, on just a section
slice := arr[1:3]
slice := arr[:2]
slice := arr[2:]
```
### In one go
Of course, you can't strictly initialise a slice by itself, because *slices don't store any data.*

However, it is possible to create a slice without creating an array first, because Go will automatically allocate a corresponding array in the background.

There are three ways to do this:
1. Using `var`

  To create an empty slice, use the following syntax:
  ```go
  var arr []string
  ```
2. A slice literal assignment

  We talked about array literals earlier. Here's the corresponding slice literal. The syntax is exactly the same as an array, just without specifying the length.
  ```go
  // Array literal
  [5]int{58, 89, 1, -56, 182}
  // Slice literal
  []int{58, 89, 1, -56, 182}
  ```
  Creating a slice literal automatically creates the underlying array.

  So to create a slice with starting values, simply assign a slice literal.
  ```go
  slice := []string{
    "foo",
    "bar",
    "baz",
  }
  ```
  You can also use this to create empty slices:
  ```go
  slice := []string{}
  ```
3. Using `make()`

  `make()` allocates an array and returns a slice which refers to that array.

  To make a slice of ints with a length of 10 (which refers to an array of length 10):
  ```go
  slice := make([]int, 10)
  ```
  An optional third parameter is the capacity of the slice.

  So, you could create a slice of length 0, which refers to an array of length 10:
  ```go
  slice := make([]int, 0, 10)
  ```
  This can often be handy, as calling `append()` will add elements starting from index 0.
## Arrays/slices in 2D and beyond
### Slices in 2D
#### Using `var`
```go
var slice [][]string
```
#### Using an assignment
To assign an empty 2D slice:
```go
slice := [][]string{}
```
To assign a 2D slice with starting values:
```go
slice := [][]string{
  []string{"r1c1", "r1c2", "r1c3"},
  []string{"r2c1", "r2c2", "r2c3"},
}
```
### Arrays in 2D
Arrays in 2D are exactly the same as slices in 2D, just with the length specified.
Eg:
```go
arr := [3][3]string{
  [...]string{"r1c1", "r1c2", "r1c3"},
  [...]string{"r2c1", "r2c2", "r2c3"},
}
```
