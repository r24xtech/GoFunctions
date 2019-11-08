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
* evenGenerator returns a function that generates even numbers. Each time it’scalled, it adds 2 to the local i variable, which—unlike normal local variables—persists between calls.
```go
func evenGenerator() func() uint {
  i := uint(0)
  return func() (ret uint) {
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
