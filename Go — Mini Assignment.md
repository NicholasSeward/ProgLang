# Go — Mini Assignment

**Course:** Programming Languages  
**Accept assignment:** [classroom50.org — Go](https://classroom50.org/Seward-Classes/programming-languages/assignments/hello_go/accept)  
**Submit:** link to your Git repository (from the accept link above)

**Time:** about 2 hours. A few small tasks, not a project.

Go (Golang) is a statically typed language from Google, aimed at simple concurrency and clear systems code. You will not learn the whole language. You will try the features that show up most in Programming Languages discussions: packages and `go` routines, channels, multiple return values, and interfaces.

---

## Where to work

Accept the assignment and open the repo.

| Option | How |
| --- | --- |
| **Codespace** (preferred if the template provides one) | Create a Codespace, wait for setup, use the terminal |
| **Local Go** | [go.dev/dl](https://go.dev/dl/) — then `go version` should print 1.21+ |
| **Browser** | [go.dev/play](https://go.dev/play/) for short snippets; submit real files in the repo |

```sh
go run ./...
# or
go run main.go
go test ./...
```

| Read when stuck | |
| --- | --- |
| **A Tour of Go** | [go.dev/tour](https://go.dev/tour/) — especially concurrency and methods |
| **Effective Go** | [go.dev/doc/effective_go](https://go.dev/doc/effective_go) — skim as needed |

---

## A few features worth knowing

### Packages and `main`

Executable programs live in `package main` with `func main()`. Imports are explicit. Unused imports are a **compile error** — Go favors tools catching dead code over silent leftovers.

```go
package main

import "fmt"

func main() {
    fmt.Println("hello")
}
```

### Multiple return values and errors

Functions often return a result and an `error`. The idiom is to check `err` immediately. There is no exception facility in the core language.

```go
func div(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

func main() {
    q, err := div(10, 2)
    if err != nil {
        fmt.Println("error:", err)
        return
    }
    fmt.Println(q)
}
```

`:=` declares and assigns. `_` discards a value you must acknowledge but do not use.

### Structs and methods

A **struct** groups fields. Methods have a **receiver**.

```go
type Counter struct {
    N int
}

func (c *Counter) Inc() {
    c.N++
}
```

Pointer receivers (`*Counter`) can modify the struct. Value receivers get a copy. That choice is a tiny Week 5 / Week 7 echo: what is shared vs copied.

### Interfaces — satisfied implicitly

An interface is a set of method signatures. A type **implements** it by having those methods — no `implements` keyword.

```go
type Speaker interface {
    Speak() string
}

type Dog struct{}

func (Dog) Speak() string { return "woof" }
```

`Dog` is a `Speaker` without announcing it. Useful for ADTs and for small seams in tests. Easy to miss in reading: the contract lives in the interface type, not next to the struct.

### Goroutines

`go f()` starts `f` concurrently. It is not a heavyweight OS process API in your source — the runtime multiplexes goroutines onto OS threads.

```go
go fmt.Println("later, maybe")
```

Without synchronization, you have races. Prefer channels (or other sync) over shared mutable variables when learning.

### Channels

A **channel** is a typed pipe. Send with `ch <- v`. Receive with `v := <-ch`. This is message passing (Week 11 survey), not a monitor on a shared counter.

```go
ch := make(chan int)
go func() {
    ch <- 7
}()
fmt.Println(<-ch)
```

Unbuffered channels synchronize sender and receiver. Close a channel when no more values will be sent if receivers range over it.

### `defer`

`defer f()` runs `f` when the surrounding function returns — good for cleanup (close files, unlock). Deferred calls run in LIFO order.

```go
f, err := os.Open("notes.txt")
if err != nil { /* … */ }
defer f.Close()
```

---

## Tasks

Put your code in the accepted repo (for example `main.go`, or a small `tasks/` package). Include a short README with how you ran it, sample output, the reflection, and the AI disclaimer.

### Task 1 — Errors as values

Write `parsePositive(s string) (int, error)` that:

- returns the integer if `s` is a decimal integer **strictly greater than zero**
- returns a non-nil error otherwise (non-number, or ≤ 0)

In `main` (or a test), call it on at least `"42"`, `"0"`, `"-3"`, and `"nope"`. Print either the value or the error message for each. Use `strconv.Atoi` (or equivalent); do not panic on bad input.

### Task 2 — Interface

Define a `Shape` interface with `Area() float64`.

Implement at least two types (for example `Rect` with width/height and `Circle` with radius) with `Area` methods.

Write `totalArea(shapes []Shape) float64` that sums areas. In `main`, build a slice of `Shape` with both kinds and print the total. Use the interface type in the slice — not `[]any` with type switches for the sum.

### Task 3 — Goroutine + channel

Write a function:

```go
func squares(nums []int) []int
```

that returns a slice of the same length where each result is the square of the corresponding input.

**Constraint:** start **one goroutine per input element**. Each goroutine sends its index and squared value on a channel (you choose the message type — a small struct is fine). The original `squares` function receives all results and builds the output slice in the correct order.

Do not square everything in the main goroutine and only pretend to use `go`. A race detector is optional; correctness of ordering is required.

Show `squares([]int{1, 2, 3, 4})` printing something equivalent to `[1 4 9 16]`.

---

## What to put in the repo

| Item | |
| --- | --- |
| Go source | Tasks 1–3, runnable with `go run` or `go test` |
| README | how to run, sample output, reflection, AI disclaimer |
| Optional | `go.mod` if you initialized a module |

### Reflection

1. What surprised you about Go?
2. Which Sebesta criterion (readability, writability, reliability, cost) does it favor, and what does it give up?
3. How would you write Task 3 in a language you know without goroutines, and what would be different?

### AI disclaimer

**AI / assistance:** whether you used AI tools, what for, and that you can explain every line you submit.

---

## What to submit

1. Accept: https://classroom50.org/Seward-Classes/programming-languages/assignments/hello_go/accept
2. Push to the repo that creates
3. Submit that **repository link** as directed on classroom50 / LMS

---

## Grading

| Result | Score |
| --- | --- |
| Meets the spec | **100** |
| Each minor issue | **−10** |
| More than **3** issues | **Kicked back** |

### To spec

- `parsePositive` returns errors for bad / non-positive input; demos all four cases
- `Shape` + two types + `totalArea` on a `[]Shape`
- `squares` uses one goroutine per element and a channel; order matches the input
- README has run instructions, reflection, and AI disclaimer

### Minor issues

- Error messages unclear but errors exist
- `totalArea` works but the demo slice has only one shape kind
- Goroutines present but ordering bugs on one case

### Kick-back

- Task 3 does the work without goroutines/channels
- Panic instead of `error` for Task 1's normal failure cases
- Cannot run from README
- No AI disclaimer
