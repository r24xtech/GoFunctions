# GoFunctions
All about functions in Go.

```go
func main(){
	xzx := []float64{98,93,77,82,83}
	fmt.Println(average(xzx))
}
// The average function will need to take in a slice of float64s and return one float64.
func average(xs []float64)float64{
	total := 0.0
	for _, v := range xs{
		total += v
	}
	return total/float64(len(xs))
}
```


**Variadic Functions**
* In this example, add is allowed to be called with multiple integers. This is known as avariadic parameter.
* By using an ellipsis (...) before the type name of the last parameter, you can indicatethat it takes zero or more of those parameters.

```go
func main() {
  fmt.Println(add(1,2,3))
}

func add(args ...int) int {
  total := 0
  for _, v := range args {
    total += v
  }
  return total
}
/*
xs := []int{1,2,3}    
fmt.Println(add(xs...))
*/
```


**Closure**
* It is possible to create functions inside of functions.

```go
func main() {
  add := func(x, y int) int {
    return x + y
  }
  fmt.Println(add(1,1))
}
```


* When  you  create  a  local  function  like  this,  it  also  has access to other local variables.
* increment adds 1 to the variable x, which is defined in the main function’s scope. This x variable can be accessed and  modified by the increment function. This is why the first time we call increment we see 1 displayed, but the second time we call it we see 2 displayed
* A function like this together with the nonlocal variables it references is known as aclosure. In this case, increment and the variable x form the closure.
```go
func main() {
  x := 0
  increment := func() int {
    x++
    return x
  }
  fmt.Println(increment())
  fmt.Println(increment())
}
// Output: 1
// OUtput: 2
```


* `uint` means "unsigned integer" while `int` means "signed integer". Unsigned integers only contain positive numbers (or zero).
* One way to use closure is by writing a function that returns another function, which when called, can generate a sequence of numbers.
* `evenGenerator` returns a function that generates even numbers. Each time it’scalled, it adds 2 to the local i variable, which—unlike normal local variables—persists between calls.
* This is what is called a naked return. I have removed the arguments from the return statement. The Go compiler automatically returns the current values in the return arguments local variables. Though this is really cool you need to watch for shadowing.
* Go's return values may be named. If so, they are treated as variables defined at the top of the function. These names should be used to document the meaning of the return values. 
* A return statement without arguments returns the named return values. This is known as a "naked" return. 
* Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions. 
```go
func evenGenerator() func() uint {
  i := uint(0) // 0, 2, 4,... the functions below is able to change i's value due to the difference in their scope
  return func() (ret uint) { // this func will return ret because its return is empty and ret is part of its argument (naked return)
    ret = i
    i += 2
    return
  }
}

func main() {
  nextEven := evenGenerator()
  fmt.Println(nextEven())
  fmt.Println(nextEven())
  fmt.Println(nextEven())
}
// Output: 0
// Output: 2
// Output: 4
```
```go
func fibonacci() func() int{
  first, second := 0, 1
  return func() int {
    ret := first
    first, second = second, first + second
    return ret
  }
}

func main() {
  f := fibonacci()
  for i := 0; i < 10; i++ {
    fmt.Println(f())
  }
}
```

**Recursion**
* A function is able to call itself, and that makes it recursive.
```go
func Factorial(x int) int {
  if x == 1 {
    return 1
  }
  return x*Factorial(x-1)
}

func Power(x float64, n int) float64 {
  if n == 0 {
    return 1
  }
  return x*Power(x, n-1)
}
```


**defer, panic, and recover**
* Go has a special statement called `defer` (defer = delay, put off, postpone) that schedules a function call to be run afterthe function completes.
* This program prints 1st followed by 2nd. Basically, `defer` moves the call to second to the end of the function.

```go
func first() {
  fmt.Println("1st")
}

func second() {
  fmt.Println("2nd")
}

func main() {
  defer second()
  first()
}
// Output: 1st
// Output: 2nd
```
* defer is often used when resources need to be freed in some way. For example, when we open a file, we need to make sure to close it later.
* It keeps our Close call near our Open call so it’s easier to understand
* Deferred functions are run even if a runtime panic occurs.
* Deferred function calls are executed in Last In First Out order after the surrounding function returns
```go
f, _ := os.Open(filename)
defer f.Close()
```

```go
func main() {
  panic("PANIC")
  str := recover() // this will never happen
  fmt.Println(str)
}
```
* But the call to recover will never happen in this case, because the call to panic immediately stops execution of the function. Instead, we have to pair it with defer.
```go
func main() {
  defer func() {
    str := recover()
    fmt.Println(str)
  }()
  panic("PANIC")
}
```

**Pointers**
* Pointers reference a location in memory where a value is stored rather than the value itself.
* In Go, a pointer is represented using an asterisk (*) followed by the type of the stored value. In the zero function, xPtr is a pointer to an int.
* An asterisk is also used to dereference pointer variables. Dereferencing a pointer gives us access to the value the pointer points to.
* Finally, we use the & operator to find the address of a variable. &x returns a *int(pointer to an int) because x is an int.
* &x in main and xPtr in zero refer to the same memory location.
```go
func zero(xPtr * int) {
  *xPtr = 0 // store the int 0 in the memory location xPtr refers to!
}
/* If we try xPtr = 0 instead, we will get a compile-time error because xPtr is not an int; it’s a *int, which can only be given another *int. */
func main() {
  x := 5
  zero(&x)
  fmt.Println(x)
}
```

* Another way to get a pointer is to use the built-in new function.
* New takes a type as an argument, allocates enough memory to fit a value of that type,and returns a pointer to it.
* In some programming languages, there is a significant difference between using new and &, with great care being needed to  eventually delete anything created with new. You don’t have to worry about this with Go—it’s a garbage-collected  programming language, which means memory is cleaned up automatically when nothing refers to it anymore.
* Pointers are rarely used with Go’s built-in types, but they are extremely useful when paired with `structs`.
```go
func one(xPtr *int) {
  *xPtr = 1
}

func main() {
  xPtr := new(int)
  one(xPtr)
  fmt.Println(*xPtr)
}
```

```go
y := new(int)
fmt.Println(y)
// Output: 0xc0000100a0
fmt.Println(*y)
// Output: 0
```
