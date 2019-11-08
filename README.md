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
* 
```go
func main() {
  add := func(x, y int) int {
    return x + y
  }
  fmt.Println(add(1,1))
}
```

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

