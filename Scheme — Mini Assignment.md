# Scheme — Mini Assignment

**Course:** Programming Languages  
**Accept assignment:** [classroom50.org — Scheme](https://classroom50.org/Seward-Classes/programming-languages/assignments/hello_scheme/accept)  
**Submit:** link to your Git repository (from the accept link above)

**Time:** about 2 hours. A few small tasks, not a project.

Scheme is a small Lisp: prefix calls, lists, and functions as values. This assignment is about **functional tricks** — `lambda`, higher-order functions, closures, and recursion — not about building an interpreter (that is Lab 3–4 and the separate Racket mini).

We use **Racket** as the Scheme implementation. Write Scheme-shaped code; `#lang racket` at the top of the file is fine.

---

## Where to work

| Option | How |
| --- | --- |
| **DrRacket** | [racket-lang.org](https://racket-lang.org) — create a file, pick **Determine language from source**, run |
| **Command line** | `racket yourfile.rkt` |
| **Codespace** | If the accepted repo provides one, use that |

Start every file with:

```racket
#lang racket
```

| Read when a form is new | |
| --- | --- |
| Week 12 notes | Functional programming lecture |
| Racket guide (functions) | [docs.racket-lang.org/guide/lambda.html](https://docs.racket-lang.org/guide/lambda.html) |
| Sebesta | Chapter 15 (Scheme sections) |

In the interactions window (REPL), you can try one expression at a time. Submit a `.rkt` file that defines the required functions and shows the sample calls (either printed, or with `rackunit` / comments of expected results).

---

## A few features worth knowing

### Everything is a call (almost)

```racket
(+ 2 3)           ; 5
(* (+ 1 2) 4)     ; 12
(define (sq x) (* x x))
(sq 5)            ; 25
```

`if` is an **expression**. Both branches are values.

```racket
(if (< 3 4) "yes" "no")    ; "yes"
```

### Lists and `quote`

```racket
'(1 2 3)          ; a list — not a call to 1
(list 1 2 3)      ; same idea, evaluates the elements
(car '(a b c))    ; a
(cdr '(a b c))    ; (b c)
(cons 1 '(2 3))   ; (1 2 3)
```

`'` / `quote` means “this list is data.” Without it, `(1 2 3)` tries to call `1`.

### `lambda` — functions as values

```racket
(define add1 (lambda (x) (+ x 1)))
(add1 10)                            ; 11

((lambda (x y) (+ x y)) 2 3)         ; 5
```

`(define (add1 x) (+ x 1))` is sugar for the same binding.

### Higher-order: `map`, `filter`, `foldl`

```racket
(map add1 '(1 2 3))                  ; (2 3 4)
(map (lambda (x) (* x x)) '(1 2 3))  ; (1 4 9)
(filter even? '(1 2 3 4 5 6))        ; (2 4 6)
(foldl + 0 '(1 2 3 4))               ; 10
(foldl * 1 '(2 3 4))                 ; 24
```

No `for` loop. The list and a small function do the work.

### Closures

A `lambda` remembers bindings from where it was **created**.

```racket
(define (make-adder n)
  (lambda (x) (+ x n)))

(define add10 (make-adder 10))
(add10 5)    ; 15
```

`n` is not global. Each `make-adder` call can freeze a different `n`.

### Composition

Build a pipeline of functions:

```racket
(define (compose f g)
  (lambda (x) (f (g x))))

(define add1-then-sq (compose sq add1))
(add1-then-sq 3)    ; 16  — (3+1)²
```

### Recursion on lists

When `map` / `foldl` are not enough, recurse on `car` / `cdr`:

```racket
(define (sum lst)
  (if (null? lst)
      0
      (+ (car lst) (sum (cdr lst)))))
```

Prefer higher-order helpers when they fit. Use recursion when the structure is irregular (nested lists, trees).

---

## Tasks

Put definitions in `tasks.rkt` (or similar). **No `for` / `while` / index loops.** Use `lambda`, recursion, and/or `map` / `filter` / `foldl`.

Built-in `map`, `filter`, and `foldl` **are allowed** here (unlike the Racket mini, where you reimplemented them).

### Task 1 — Predict, then run

Write your prediction **before** running. Then record the actual result. One sentence on each miss.

```racket
(* (+ 1 2) (- 8 3))
(map (lambda (x) (* x 2)) '(1 2 3))
(filter odd? '(1 2 3 4 5))
(foldl - 0 '(1 2 3))
((lambda (f) (f 3)) (lambda (x) (* x x)))
```

For `(foldl - 0 '(1 2 3))`, say in one line what order the subtractions ran (left fold).

### Task 2 — Pipeline without a loop

Given a list of exam scores (integers 0–100), define:

| Function | Meaning | Example |
| --- | --- | --- |
| `average` | mean of a non-empty list | `(average '(70 80 90))` → `80` |
| `passing` | scores ≥ 60 | `(passing '(40 60 90))` → `'(60 90)` |
| `curve` | add 5 to each score, capped at 100 | `(curve '(96 50))` → `'(100 55)` |
| `report` | average of the **passing** scores **after** `curve` | see below |

```racket
(report '(40 55 70 95))
```

Work it out by hand once: curve → pass filter → average. Implement `report` by **composing** the pieces (`compose`, nested calls, or a small pipeline helper). Do not walk the list with a hand-rolled index loop.

`average` may use `foldl` and `length` (or an equivalent fold). Empty-list behavior for `average` / `report`: document it (error message is fine).

### Task 3 — Closures and a curry

**3a.** `(make-multiplier k)` returns a function of one argument that multiplies by `k`.

```racket
(define double (make-multiplier 2))
(double 7)    ; 14
```

Two multipliers from different `k` values must stay independent.

**3b.** `(curry2 f)` turns a two-argument function into a one-argument function that returns another one-argument function:

```racket
(define add (curry2 +))
((add 3) 4)    ; 7
```

Show `((curry2 *) 6) 7)` → `42`.

### Task 4 — Nested lists (recursion)

`(flatten lst)` takes a list that may contain nested lists and returns a flat list of the non-list atoms **in order**.

```racket
(flatten '(1 (2 (3 4) 5) 6))    ; (1 2 3 4 5 6)
(flatten '((a) b (c (d))))      ; (a b c d)
```

Use recursion. `list?` / `pair?` and `append` or `cons` are fine. Do not call a built-in flatten if your Racket version has one — write it yourself.

---

## What to put in the repo

| Item | |
| --- | --- |
| `tasks.rkt` | Tasks 2–4 definitions and demonstrated calls |
| Task 1 notes | predictions + results (in the README or comments) |
| README | how to run (`racket tasks.rkt` or DrRacket), sample output, reflection, AI disclaimer |

Optional: `(require rackunit)` and `check-equal?` for the examples — nice, not required.

### Reflection

1. What surprised you about Scheme / this style of code?
2. Which Sebesta criterion (readability, writability, reliability, cost) does it favor, and what does it give up?
3. How would you write `report` or `flatten` in a language you know, and what would be different?

### AI disclaimer

**AI / assistance:** whether you used AI tools, what for, and that you can explain every line you submit.

---

## What to submit

1. Accept: https://classroom50.org/Seward-Classes/programming-languages/assignments/hello_scheme/accept
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

- Task 1 has predictions written before results
- `average`, `passing`, `curve`, and `report` work without loops; `report` uses the pipeline pieces
- `make-multiplier` and `curry2` work as specified
- `flatten` handles nesting and is your own recursion
- README has run instructions, reflection, AI disclaimer

### Minor issues

- Cap-at-100 off by one edge case
- Task 1 missing a note on a wrong prediction
- `report` correct but written as one giant recursive function with no reuse of `curve` / `passing` / `average`

### Kick-back

- Index / `for`-style loops instead of FP tools
- `flatten` only works on depth-1 lists
- Closures that share one global `k`
- Cannot run from README
- No AI disclaimer
