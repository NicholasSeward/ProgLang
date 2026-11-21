# Scheme — Mini Assignment

**Course:** Programming Languages  
**Accept assignment:** [classroom50.org — Scheme](https://classroom50.org/Seward-Classes/programming-languages/assignments/hello_scheme/accept)  
**Submit:** the Git repository link from that accept flow

**Time:** about 2 hours. A few small tasks, not a project.

Scheme is a small Lisp: prefix calls, lists, and functions as values. This assignment is about **functional tricks** — `lambda`, higher-order functions, and recursion.

We use **GNU Guile**. The accepted repo is already set up for a **GitHub Codespace** with Guile installed. Open a Codespace and work there. You do not install Guile yourself.
s
---

## Where to work

1. Accept the assignment (link at the top of this page).
2. Open the repo in a **Codespace**. Wait for setup the first time.
3. Use the terminal:

```sh
guile --version
guile              # REPL; (quit) or Ctrl-D to leave
guile -l task2.scm # load a file, then use the REPL
guile task2.scm    # run a file that prints demos
```

At the top of files that need `fold` or `filter`:

```scheme
(use-modules (srfi srfi-1))
```

| Read when a form is new | |
| --- | --- |
| Week 12 notes | Functional programming lecture |
| Guile reference | [Guile manual](https://www.gnu.org/software/guile/manual/) |
| Sebesta | Chapter 15 (Scheme sections) |

---

## A few features worth knowing

### Everything is a call (almost)

```scheme
(+ 2 3)           ; 5
(* (+ 1 2) 4)     ; 12
(define (sq x) (* x x))
(sq 5)            ; 25
```

`if` is an **expression**. Both branches are values.

```scheme
(if (< 3 4) "yes" "no")    ; "yes"
```

### Lists and `quote`

```scheme
'(1 2 3)          ; a list — not a call to 1
(list 1 2 3)      ; same idea, evaluates the elements
(car '(a b c))    ; a
(cdr '(a b c))    ; (b c)
(cons 1 '(2 3))   ; (1 2 3)
```

`'` / `quote` means “this list is data.” Without it, `(1 2 3)` tries to call `1`.

### `lambda` — functions as values

```scheme
(define add1 (lambda (x) (+ x 1)))
(add1 10)                            ; 11

((lambda (x y) (+ x y)) 2 3)         ; 5
```

`(define (add1 x) (+ x 1))` is sugar for the same binding.

### Higher-order: `map`, `filter`, `fold`

Guile’s left fold is SRFI-1 `fold`.

```scheme
(use-modules (srfi srfi-1))

(map add1 '(1 2 3))                  ; (2 3 4)
(map (lambda (x) (* x x)) '(1 2 3))  ; (1 4 9)
(filter even? '(1 2 3 4 5 6))        ; (2 4 6)
(fold + 0 '(1 2 3 4))                ; 10
(fold * 1 '(2 3 4))                  ; 24
```

No `for` loop. The list and a small function do the work.

### Closures

A `lambda` remembers bindings from where it was **created**.

```scheme
(define (make-adder n)
  (lambda (x) (+ x n)))

(define add10 (make-adder 10))
(add10 5)    ; 15
```

### Tail recursion

A call is **tail recursive** when the recursive call is the last thing the function does — nothing waits on its result except to return it. Guile can reuse the stack frame.

```scheme
(define (sum-tail lst)
  (define (loop xs acc)
    (if (null? xs)
        acc
        (loop (cdr xs) (+ acc (car xs)))))
  (loop lst 0))
```

`(loop …)` is in tail position. `(+ (car xs) (sum-tail (cdr xs)))` would not be.

---

## Files

| File | Task |
| --- | --- |
| `task1.scm` | Expressions + one comment block |
| `task2.scm` | Score pipeline |
| `task3.scm` | Tail recursion |
| `README.md` | Run notes, **Reflection**, **AI disclaimer** |

**No `for` / `while` / index loops** in Tasks 2–3.

---

## Task 1 — Expressions

**File:** `task1.scm`

Start with the following code.  Figure out what each line does.  Place a comment next to each line explaining what it does and how it works.

```scheme
(display (* (+ 1 2) (- 8 3)))
(newline)
(display (map (lambda (x) (* x 2)) '(1 2 3)))
(newline)
(display (filter odd? '(1 2 3 4 5)))
(newline)
(display (fold - 0 '(1 2 3)))
(newline)
(display ((lambda (f) (f 3)) (lambda (x) (* x x))))
(newline)
```

---

## Task 2 — Pipeline without a loop

**File:** `task2.scm`

Given a list of exam scores (integers 0–100), define:

| Function | Meaning | Example |
| --- | --- | --- |
| `average` | mean of a non-empty list | `(average '(70 80 90))` → `80` |
| `passing` | scores ≥ 60 | `(passing '(40 60 90))` → `'(60 90)` |
| `curve` | add 5 to each score, capped at 100 | `(curve '(96 50))` → `'(100 55)` |
| `report` | average of the **passing** scores **after** `curve` | see below |

```scheme
(report '(40 55 70 95))
```

Work it out by hand once: curve → pass filter → average. Implement `report` by **composing** the pieces (`compose`, nested calls, or a small pipeline helper).

`average` may use `fold` and `length`. Empty-list behavior for `average` / `report`: document it in a short comment.

At the bottom of the file, print a few demo calls so `guile task2.scm` shows output.

---

## Task 3 — Tail recursion

**File:** `task3.scm`

Write these with an inner helper that keeps an accumulator so the recursive call is in **tail position**. No `fold` / `map` for these two — the point is to see the tail-recursive shape.

| Function | Meaning | Example |
| --- | --- | --- |
| `sum-from-to` | sum of integers from `lo` to `hi` inclusive | `(sum-from-to 1 5)` → `15` |
| `countdown` | list counting down from `n` to `1` | `(countdown 4)` → `'(4 3 2 1)` |

`countdown` may build the list in the accumulator (reversed at the end is fine if the final result is descending). For `n < 1`, return `'()`.

Print demos at the bottom of the file.

---

## README.md (required layout)

Put **everything below in `README.md`**. Do not put the reflection or AI disclaimer only in chat or only in `.scm` comments.

```text
# Scheme Mini Assignment

## How to run
(Codespace + the guile commands you used)

## Sample output
(paste what each of task1.scm, task2.scm, task3.scm printed)

## Reflection

1. What surprised you about Scheme / this style of code?
2. Which Sebesta criterion (readability, writability, reliability, cost) does it favor, and what does it give up?
3. How would you write `report` or `sum-from-to` in a language you know, and what would be different?

## AI / assistance

State, in your own words:
- whether you used AI tools (ChatGPT, Copilot, Cursor, etc.)
- what you used them for
- that you understand and can explain every line you submit
```

Those two headings — **`## Reflection`** and **`## AI / assistance`** — must appear in `README.md` with answers under them.

---

## Grading

| Result | Score |
| --- | --- |
| Meets the spec | **100** |
| Each minor issue | **−10** |
| More than **3** issues | **Kicked back** |

### To spec

- `task1.scm` has one top comment block covering all expressions, then the code
- `task2.scm` implements `average`, `passing`, `curve`, and `report` without loops
- `task3.scm` implements `sum-from-to` and `countdown` with tail recursion
- `README.md` has How to run, Sample output, **Reflection**, and **AI / assistance**

### Minor issues

- Cap-at-100 off by one edge case
- Thin top comment block in Task 1
- `report` correct but does not reuse the pipeline pieces

### Kick-back

- Index / `for`-style loops instead of FP tools / tail recursion
- Task 3 not tail recursive (work left after the recursive call)
- Missing `task1.scm` / `task2.scm` / `task3.scm`
- Reflection or AI disclaimer missing from `README.md`
- Cannot run from the README
