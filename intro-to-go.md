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
* Learning Resources 

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

---

## Collections: slices

---

## Collections: map

---

## Struct

---

## Error Handling

---

## Goroutines

---

## Learning Resources

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
