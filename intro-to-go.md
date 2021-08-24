---
author: satoru
theme: ./theme.json
---
# Introduction to Go by Examples

> Official Playground: http://play.golang.org/
> or
> The Unofficial Alternative with Syntax Highlighting: https://goplay.space/

## Content
* Hello World
* More Hello to World
* Collections: array
* Collections: slices
* Collections: map
* Struct
* Error Handling
* Goroutines
* Channels
* Resources 

---

## Hello World

```go
package main

import "fmt"

func main() {
    fmt.Println("你好，世界！")
}
```

---

## More Hello to World

```go
package main

import (
    "fmt"
)

// factorial calculates "n!"
func factorial(n int) int {
    if n == 0 {
        return 1
    }
    f := 1
    for i := 2; i <= n; i++ {
        f *= i
    }
    return f
}

func main() {
    fmt.Printf("0! = %4d\n", factorial(0))
    fmt.Printf("5! = %4d\n", factorial(5))
}
```

---

## Collections: array

```go
package main

import (
    "fmt"
)

func main() {
    x := [4]int{1, 9, 8, 4}
    for i, v := range x {
        fmt.Printf("%d: %d\n", i, v)
    }
    var name [2]string
    name[0] = "Jim"
    name[1] = "Green"
    fmt.Println(name[0] + " " + name[1])
}
```

---

## Collections: slices

```go
package main

import (
    "fmt"
)

func main() {
    nums := []int{1, 9, 8, 4}
    fmt.Println(nums)

    nums = append(nums, 0, 1)
    nums = append(nums, []int{4, 2}...)
    fmt.Println(nums)

    prefix := nums[:4]
    fmt.Printf("Prefix: %v\n", prefix)
    fmt.Printf("Whole: %v\n", nums)

    prefix[0] = 2
    fmt.Printf("Whole: %v\n", nums)
}
```

---

## Collections: map


```go
package main

import "fmt"

func main() {
    prices := make(map[string]float64)
    prices["apple"] = 4.2
    prices["orange"] = 3.3
    for k, v := range prices {
        fmt.Printf("The price of %s is %.2f\n", k, v)
    }

    if p, ok := prices["x"]; ok {
        fmt.Printf("The price of x is %.2f\n", p)
    } else {
        fmt.Println("x not found")
    }
    
    delete(prices, "apple")
    fmt.Println(prices)
}
```

---

## Struct

---

## Error Handling

---

## Goroutines

---

## Channels

---

## Resources

### Source of the slides
* https://github.com/suzaku/intro-to-go

### Tutorial
* [A Tour of Go](https://tour.golang.org/welcome/1)

### Books
* [Learning Go](https://learning.oreilly.com/library/view/learning-go/9781492077206/)
* [The Go Programming Language](https://learning.oreilly.com/library/view/the-go-programming/9780134190570/)

### Reading Source Code
* [slides](https://github.com/maaslalani/slides)
* [shonenjump](https://github.com/suzaku/shonenjump)
* [juicefs](https://github.com/juicedata/juicefs)
* [Golang Standard Library](https://github.com/golang/go/tree/master/src)

### Feel Free to Ask Me
