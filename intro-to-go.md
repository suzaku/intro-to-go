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
* Interface
* Error Handling
* Goroutines
* Channels
* Resources 

---

## Hello World

https://play.golang.org/p/dl55zgXA-p9

```go
package main

import "fmt"

func main() {
    fmt.Println("你好，世界！")
}
```

---

## More Hello to World

https://play.golang.org/p/5mfImEW2PzV

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

https://play.golang.org/p/AAJYNrivEIP

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

https://play.golang.org/p/TzoKtWJqc_y

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

https://play.golang.org/p/gKRdBgR9Cto

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

https://play.golang.org/p/qUc1_3pfV9y

```go
package main

import "fmt"

type User struct {
    name string
    age int
}

func (u *User) SetName(name string) {
    u.name = name
}

func (u User) String() string {
    return fmt.Sprintf("User %s (age: %d)", u.name, u.age)
}

func main() {
    jay := User{name: "Jay Chou", age: 42}
    fmt.Println(jay)
    var baby User
    baby.SetName("Jim Green")
    fmt.Println(baby)
    baby.SetName("Jack")
    fmt.Println(baby)
}
```

---

## Interface

https://play.golang.org/p/-mFVwK-rQ7j

```go
package main

import "fmt"

type Talker interface {
    talk() string
}

type Robot struct {}

func (r Robot) talk() string {
    return ".-.-...--.-.-"
}

type Fairy struct {}

func (f Fairy) talk() string {
    return "BabalaBabala"
}

func main() {
    talkers := []Talker{Robot{}, Fairy{}}
    for _, t := range talkers {
        fmt.Println(t.talk())
    }
}
```

---

## Error Handling

https://play.golang.org/p/_jagIL3eECL

```go
package main

import "fmt"

func fab(n int) (int, error) {
    if n < 1 {
        return 0, fmt.Errorf("invalid n: %d", n)
    }
    x, y := 1, 1
    for i := 1; i < n; i++ {
        x, y = y, x+y 
    }
    return x, nil
}

func main() {
    x, err := fab(3)
    fmt.Println(x, err)
    if _, err := fab(0); err != nil {
        fmt.Printf("Failed to calculate fab: %s\n", err)
    }
}
```

---

## Goroutines

https://play.golang.org/p/S8vd-1iGApl

```go
package main

import (
    "fmt"
    "time"
    "sync"
)

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 16; i++ {
        wg.Add(1)
        go func(n int) {
            defer wg.Done()
            time.Sleep(time.Second)
            fmt.Printf("Done: %d\n", n)
        }(i)
    }
    wg.Wait()
}
```

---

## Channels - Multi-Workers Pattern

https://play.golang.org/p/3q7fVIDfn_F

```go
package main

import "fmt"

func worker(jobs <-chan int, result chan<- string) {
    for j := range jobs {
        result <- fmt.Sprintf("Processed %d", j)
    }
}

func main() {
    jobs := make(chan int, 1)
    const nJobs = 9
    go func() {
        for i := 0; i < nJobs; i++ {
            jobs <- i
        }
        close(jobs)
    }()
    const nWorkers = 2
    result := make(chan string, nWorkers)
    defer close(result)
    for i := 0; i < nWorkers; i++ {
        go worker(jobs, result)  
    }
    for i := 0; i < nJobs; i++ {
        r := <-result
        fmt.Print(r)
        fmt.Print(" ")
    }
}
```

---

## Channels - Merging-Stream Pattern

https://play.golang.org/p/fyuWxII2hN9

```go
package main

import "fmt"

func numGenerator(init int, limit int) <-chan int {
    out := make(chan int, 1)
    go func () {
        for i := 0; i < limit; i++ {
            out <- init+i
        }
        close(out)
    }()
    return out
}

func main() {
    s1 := numGenerator(0, 7)
    s2 := numGenerator(100, 3)
    s3 := numGenerator(1000, 2)
    for s1 != nil || s2 != nil || s3 != nil {
        var x int
        var ok bool
        select {
        case x, ok = <-s1:
            if !ok {
                s1 = nil
            }
        case x, ok = <-s2:
            if !ok {
                s2 = nil
            }
        case x, ok = <-s3:
            if !ok {
                s3 = nil
            }
        }
        fmt.Printf("%d ", x)
    }
}
```

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
