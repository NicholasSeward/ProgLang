# Programming Languages
## Week 12 — Functional Programming

Sebesta, Chapter 15 (selected)

---

# Today

1. What functional programming is after
2. Mathematical functions — and why that matters
3. Fundamentals: immutability, first-class functions, recursion
4. A short Scheme tour
5. FP ideas inside imperative languages
6. FP vs imperative — and your interpreter
7. Lab 4

---

# Where this fits

| Week | Focus |
| --- | --- |
| 7–8 | Subprograms and how calls run |
| 11 | OOP, exceptions, concurrency survey |
| **12** | Functional programming — another paradigm |
| 13 | Logic programming (survey); Lab 4 due |
| Lab 4 | Functions are required; first-class / closures are stretch |

Imperative code says *how* to change state. Functional code says *what* a value is, built from other values. Your interpreter can lean either way.

---

# Section 15.1 — The idea

A **functional program** is mostly:

- expressions that produce values
- functions that take values and return values
- little or no assignment that overwrites a name

```scheme
(define (square x) (* x x))
(square 4)          ; 16
```

There is no `x = x + 1`. You bind a name once (or shadow it), and you build a new value instead of mutating an old one.

| Style | Typical question |
| --- | --- |
| Imperative | What commands update the variables? |
| Functional | What is the value of this expression? |

---

# Section 15.2 — Mathematical functions

In math, a function maps each input in its domain to exactly one output.

```text
f(x) = x²
f(3) = 9
```

Calling `f(3)` twice always gives `9`. Nothing else in the world changes. That property is **referential transparency**: an expression can be replaced by its value without changing the meaning of the program.

| Math / FP | Imperative surprise |
| --- | --- |
| Same inputs → same output | A function may read or write globals |
| No hidden effects | I/O, mutation, random — the result depends on *when* you call |

Real languages allow some effects (print, file I/O). The FP ideal still guides design: keep the pure core large, push effects to the edges.

---

# Section 15.3 — Fundamentals

## First-class functions

Functions are values. You can:

- pass a function as an argument
- return a function from a function
- store a function in a data structure

```scheme
(define (apply-twice f x)
  (f (f x)))

(apply-twice add1 3)    ; 5
```

Week 7 called this "subprograms as parameters." FP makes it normal, not a special case.

## Higher-order functions

A function that takes or returns a function. Classic trio:

| Name | Idea |
| --- | --- |
| `map` | apply `f` to every element |
| `filter` | keep elements where `p` is true |
| `fold` / `reduce` | combine elements with an operator |

```scheme
(map add1 '(1 2 3))           ; (2 3 4)
(filter even? '(1 2 3 4))     ; (2 4)
(foldl + 0 '(1 2 3 4))        ; 10
```

Loops become data + a small function. The Racket mini assignment built these with recursion on purpose.

## Immutability (as a habit)

Prefer new values over changing old ones.

```text
xs with 4 on the front   — not   insert 4 into xs
```

| Payoff | Cost |
| --- | --- |
| Easier reasoning; no aliasing surprises | May allocate more; need persistent structures or careful sharing |
| Safer concurrency (no shared mutable race) | Some algorithms feel less natural |

## Recursion instead of loops

Without mutable loop variables, repetition is usually recursion (or a higher-order function that hides the recursion).

```scheme
(define (sum lst)
  (if (null? lst)
      0
      (+ (car lst) (sum (cdr lst)))))
```

Tail recursion (the recursive call is the last thing) can be implemented as a jump, so deep recursion need not blow the stack. Scheme requires proper tail calls. Python does not — that is a language design choice you already hit in Lab thinking.

## Closures

A function that remembers the environment where it was **created**.

```scheme
(define (make-adder n)
  (lambda (x) (+ x n)))

(define add5 (make-adder 5))
(add5 3)    ; 8
```

`n` is not a global. It lives in the closure. Week 8 said that value may have to leave the stack (heap allocation). Your Lab 4 can store a function as "code + defining environment" if you want closures; nested functions that only see globals are a smaller start.

---

# Section 15.5 — Scheme, briefly

Scheme is a Lisp dialect: prefix calls, lists as the main structure, a tiny core.

| Form | Meaning |
| --- | --- |
| `(f a b)` | call `f` with `a` and `b` |
| `' (1 2 3)` or `(quote (1 2 3))` | the list itself, not a call |
| `(define name expr)` | bind a name |
| `(lambda (x) …)` | an anonymous function |
| `(if test then else)` | expression-oriented conditional |
| `(cond …)` | multi-branch |

Everything is an expression. `if` returns a value. There is no separate "statement language" and "expression language" the way C splits them.

**Lists.** `car` / `first` is the head. `cdr` / `rest` is the tail. `cons` builds a pair. Recursion on lists is the default control structure.

You do not need to become a Scheme programmer this week. You need to recognize: expression-oriented evaluation, first-class `lambda`, and lists as data **and** as a way to write programs (code is data — homoiconicity, lightly).

---

# Section 15.10 — FP in imperative languages

You already use FP features without living in Haskell.

| Feature | Where you have seen it |
| --- | --- |
| Lambdas / closures | Python `lambda`, Java lambdas, JS arrows, C++ lambdas |
| `map` / `filter` | Python, JS, Java streams |
| Immutability options | Python tuples; `final` / `const`; persistent libraries |
| Recursion | Every language that allows a function to call itself |

```python
list(map(lambda x: x + 1, [1, 2, 3]))
[x for x in xs if x % 2 == 0]
```

The host language is still imperative. Assignment, loops, and mutable objects are normal. The FP tools are available when they fit.

---

# Section 15.11 — FP vs imperative (short)

| | Functional leaning | Imperative leaning |
| --- | --- | --- |
| State | New values | Update variables |
| Control | Recursion, higher-order functions | Loops, sequenced statements |
| Composition | Pipe small functions | Procedures that share memory |
| Reasoning | Referential transparency | Trace the timeline of mutations |
| Concurrency | Easier when little is shared and mutable | Locks / careful sharing |

Neither "wins." Scientific computing, UIs, and systems code often mix both. The point of the chapter is to know what you are choosing.

---

# Tie this to Lab 4

Your mini interpreter is a program that processes programs. Design knobs that are FP-shaped:

| Knob | Imperative-ish | Functional-ish |
| --- | --- | --- |
| Environment | Mutable map; assign overwrites | `extend` returns a new env; lookup walks parents |
| Functions | Name → code only (globals for free vars) | Closure: code + defining env |
| `if` | Statement that runs a branch | Expression that yields a value |
| Program shape | Sequence of statements | Nest of expressions |

Lab 4 does **not** require a purely functional language. It requires `if`, a loop or recursion, and function calls with a real call scope. If you add first-class functions or closures, say so in the README — that is stretch aligned with this chapter.

```text
# mutable env (fine)
env.define("x", 1)
env.assign("x", 2)

# or extend (also fine)
child = env.extend({"x": 1})
```

Week 8's parent pointer works for both. Closures need the function value to keep that parent after the defining call returns.

---

# In class (10 minutes)

1. Rewrite a three-line imperative average (sum in a loop, divide) as `fold` + length, or as a Scheme recursive function.
2. Does your Lab 4 language treat `if` as a statement, an expression, or both? What would change in the AST?
3. If `make-adder` is evaluated in your interpreter, what must the function value store so `add5(3)` still sees `n`?

---

# Lab 4

Lab 4 is in progress. Functions are required for the Week 13 due date.

Order that usually works:

1. `if` / `else` on values you already compare
2. One loop (`while` / `for`) **or** show recursion via a user function
3. Function definition + call with a fresh environment for parameters

Sample programs for the Week 14 demo should exercise all three.

---

# Week 12 wrap

You should be able to:

- Explain referential transparency and why assignment breaks it
- Define first-class function, higher-order function, and closure
- Read a small Scheme `define` / `lambda` / `if` / list recursion example
- Point to FP features in Python/Java/JS
- Say how your interpreter's environment and function values lean imperative or functional

---

# Before next week

**Read:** Chapter 16, Sections 16.1–16.6 (logic programming survey)

**Lab 4:** feature freeze approaches Week 13 — prefer a solid `if` + loop/recursion + calls over half-built stretch goals

---

# Quick check

1. Why is `f(x) = x + 1` closer to math than a function that increments a global counter?
2. What makes a function *higher-order*?
3. In `(lambda (x) (+ x n))` created by `make-adder`, where does `n` live after `make-adder` returns?
4. Scheme's `if` returns a value. How is that different from an `if` statement in C?
5. Name one FP feature you already use in an imperative language.
6. For Lab 4, what is the minimum your "function value" must contain if you do *not* support closures? What extra if you do?
