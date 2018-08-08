# Cheatsheet for arrays and slices in Go
When learning Go, with [Finnian A.](https://github.com/developius), we found that there were a lot of different ways to initialise arrays and slices.

Until I become even slightly good at Go, this guide will be a personal reference.

## General array/slice concepts
TODO:
<!-- In most circumstances it's unnecessary to deal with arrays; slices are far easier.
_Slices don't store any data._
Explain capacity with ascii art -->

## Array initialisation
### Empty
To declare an empty array, use the following syntax:
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
### With starting values
To create an array containing starting values, use curly braces:
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
##Slice initialisation
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

There are two ways to do this:
#### A slice literal assignment (with starting values)
We talked about array literals earlier. Here's the corresponding slice literal.
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
#### Using `make()` (empty)
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
## 2D arrays/slices
