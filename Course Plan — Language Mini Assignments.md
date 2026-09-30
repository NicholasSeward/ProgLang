# Programming Languages — Language Mini Assignments

**Textbook:** Sebesta, *Concepts of Programming Languages*, 12th ed.  
**Languages:** COBOL, Rust, APL, Racket, Erlang, Prolog

Six short programming assignments, one per language. Each one is placed in the week whose topic that language shows off best, and in a week where no lab is due.

**Size:** about 2–3 hours each. Small enough to finish in a week, big enough to write real code.

---

## Where they fit

Lab deadlines are Weeks 3, 5, 9, and 13. The midterm is Week 7. Mini assignments go in the gaps.

| Week | Topic | Lab / exam | Mini assignment |
| --- | --- | --- | --- |
| 1 | Preliminaries | — | **COBOL** assigned |
| 2 | Describing syntax | — | **COBOL** due |
| 3 | Semantics, lexing, parsing | Lab 1 due | — |
| 4 | Names, bindings, scopes | — | **Rust** |
| 5 | Data types | Lab 2 due | — |
| 6 | Expressions and control | — | **APL** |
| 7 | — | Midterm | — |
| 8 | Subprograms | — | **Racket** |
| 9 | Implementing subprograms | Lab 3 due | — |
| 10 | ADTs and encapsulation | — | — (buffer) |
| 11 | OOP, exceptions, concurrency | — | **Erlang** |
| 12 | Functional programming | — | — (Lab 4 push) |
| 13 | Logic programming | Lab 4 due | **Prolog** assigned |
| 14 | Presentations | Final | **Prolog** due |

```mermaid
flowchart LR
  C["Wk 1–2 COBOL"] --> R["Wk 4 Rust"] --> A["Wk 6 APL"] --> K["Wk 8 Racket"] --> E["Wk 11 Erlang"] --> P["Wk 13–14 Prolog"]
```

---

## This term (starting from Week 6)

Weeks 1–5 have passed, so here is a catch-up placement that still avoids lab weeks:

| Week | Mini assignment | Why it still fits |
| --- | --- | --- |
| 6 | **APL** | Expressions — right on topic |
| 8 | **COBOL** | Light restart after the midterm; reviews Week 5 records and decimal types |
| 10 | **Rust** | Follows Week 9 stack, heap, and lifetimes; traits tie to ADTs |
| 11 | **Erlang** | Concurrency and exceptions — right on topic |
| 12 | **Racket** | Functional programming — right on topic |
| 13–14 | **Prolog** | Logic programming — right on topic |

---

## Common rules (all six)

| Item | Requirement |
| --- | --- |
| Repo | One repo for all six, one folder per language (`cobol/`, `rust/`, …) |
| Folder README | How to run (tool + version), sample output, reflection, AI disclaimer |
| Reflection | Three short answers (below) |
| Submit | Repo link after each assignment |
| Grading | Same as labs: **100** if to spec, **−10** per minor issue, more than **3** issues → kicked back |

**Reflection questions (same every time):**

1. What surprised you about this language?
2. Which Sebesta criterion (readability, writability, reliability, cost) does it favor, and what does it give up?
3. How would you write the same program in a language you know, and what would be different?

---

## COBOL — Payroll report

**When:** Weeks 1–2 (assigned Week 1, due end of Week 2)  
**Ties to:** Chapter 1 language domains and readability; Chapter 2 history; previews records and decimal types (Week 5)

### Task

Print a payroll report for a small team.

- Store at least **four** employees in a table (`OCCURS`), each a **record** with name, hours, and hourly rate
- Pay is `hours × rate`, with **overtime** at 1.5× for hours over 40
- Print one line per employee and a **total** line
- Money uses exact decimal types (`PIC 9(5)V99`) and a formatted output picture (`$$$,$$9.99`)

### Sample output

```text
NAME                  HOURS       GROSS
GRACE HOPPER           40.0     $840.00
JEAN SAMMET            45.0   $1,187.50
...
TOTAL                         $4,012.25
```

### Must use

| COBOL feature | Why |
| --- | --- |
| Level-numbered record (`01`, `05`) | Record types |
| `PIC` with `V` | Exact decimal arithmetic |
| Edited output picture | Formatting built into the type |
| `IF` | Overtime rule |
| `PERFORM VARYING` | Loop over the table |

**Tools:** GnuCOBOL (`cobc -x payroll.cob`) or an online COBOL compiler.

---

## Rust — Ownership and `Option`

**When:** Week 4  
**Ties to:** Binding, lifetime, immutability (Chapter 5); previews references, dangling pointers, and optional types (Week 5)

### Task

Write a small **grade-stats** program.

- `fn average(scores: &[f64]) -> Option<f64>` — `None` for an empty list
- `fn highest(scores: &[f64]) -> Option<f64>`
- `fn longest_name(names: &[String]) -> Option<&String>`
- `main` calls each one on a normal input **and** an empty input and handles both cases with `match`

### Break it on purpose

In the README, show **three** compiler errors you caused on purpose. For each: the code, the error message, and one sentence on what the compiler protected you from.

| Error to trigger | Concept |
| --- | --- |
| Use a `String` after it was moved | Ownership |
| Assign to a variable declared without `mut` | Immutability by default |
| Return a reference to a local variable | Lifetime / dangling pointer |

### Must use

- Borrowed parameters (`&[T]`), not ownership transfers
- `Option` + `match` — no `unwrap()` in the final code
- At least one `let mut`

**Tools:** [Rust Playground](https://play.rust-lang.org) or `rustup` + `cargo run`.

---

## APL — Array one-liners

**When:** Week 6  
**Ties to:** Precedence and associativity (Chapter 7); array operations (Chapter 6); writability vs readability (Chapter 1)

### Part 1 — Predict, then run

Write your prediction **before** running each expression, then record the actual result. Explain any you got wrong.

```apl
2×3+4
10-4-3
(10-4)-3
-/1 2 3 4
÷/8 4 2
2*3*2
```

### Part 2 — Functions (no explicit loops)

Write each as a one-line function (dfn):

| Function | Example | Result |
| --- | --- | --- |
| `avg` — average | `avg 70 85 90 95` | `85` |
| `range` — max minus min | `range 3 9 4 1` | `8` |
| `above` — how many are above the average | `above 70 85 90 95` | `2` |
| `evens` — keep only the even numbers | `evens ⍳10` | `2 4 6 8 10` |
| `pal` — is a word a palindrome? (1 or 0) | `pal 'racecar'` | `1` |

For each function, add a comment reading it **right to left** in plain English.

**Tools:** [TryAPL](https://tryapl.org) or Dyalog APL. Submit a `.apl` file plus a session transcript.

---

## Racket — Higher-order toolkit + a tiny evaluator

**When:** Week 8  
**Ties to:** Subprograms as parameters, closures, recursion (Chapter 9); previews functional programming (Week 12); connects to Labs 3–4

### Part 1 — Build your own higher-order functions

Using **recursion only** (no built-in `map`, `filter`, `foldl`, and no loops):

| Function | Example | Result |
| --- | --- | --- |
| `my-map` | `(my-map add1 '(1 2 3))` | `'(2 3 4)` |
| `my-filter` | `(my-filter even? '(1 2 3 4))` | `'(2 4)` |
| `my-foldl` | `(my-foldl + 0 '(1 2 3 4))` | `10` |

### Part 2 — A closure

`(make-counter)` returns a function; each call returns the next number: `1`, `2`, `3`, … Two counters must be independent.

### Part 3 — Extend an evaluator

Start from this:

```racket
(define (calc e)
  (match e
    [(? number?) e]
    [(list '+ a b) (+ (calc a) (calc b))]
    [(list '* a b) (* (calc a) (calc b))]))
```

Add `-`, `/`, comparison `<`, and `(if c t e)`. Division by zero should produce a clear error.

```racket
(calc '(if (< 1 2) (+ 10 5) 0))   ; 15
```

### Must use

- `rackunit` tests (`check-equal?`) for every function
- A README note: how is `calc` like your Lab 3 evaluator, and what did you not need that Lab 2 required?

**Tools:** [DrRacket](https://racket-lang.org).

---

## Erlang — Bank account process

**When:** Week 11  
**Ties to:** Message-passing concurrency (Chapter 13); exception handling (Chapter 14); single assignment; guards (Chapter 8 guarded commands)

### Task

A bank account is a **process** that holds a balance. There are no mutable variables — the balance is passed along through recursion.

| Message | Reply |
| --- | --- |
| `{deposit, Amount, From}` | `{ok, NewBalance}` |
| `{withdraw, Amount, From}` | `{ok, NewBalance}` or `{error, insufficient_funds}` |
| `{balance, From}` | `{balance, Balance}` |
| `stop` | process ends |

- Provide client functions: `open/1`, `deposit/2`, `withdraw/2`, `balance/1`, `close/1`
- Use a **guard** to reject non-positive amounts
- A `demo/0` function opens **two** accounts and moves money between them

### Show in the README

- A shell transcript running `demo/0`
- What happens when you type `X = 1.` then `X = 2.` in the shell, and why single assignment helps concurrency

**Tools:** Erlang/OTP (`erl`, then `c(bank).`).

---

## Prolog — Course prerequisites

**When:** Weeks 13–14 (assigned Week 13, due end of Week 14)  
**Ties to:** Facts, rules, unification, backtracking (Chapter 16); grammars as rules (Chapter 3)

### Task

Model a department's course catalog.

- At least **8** `prereq(Before, After)` facts
- At least **2** students, each with `taken(Student, Course)` facts
- Rules:

| Rule | Meaning |
| --- | --- |
| `requires(C, P)` | `P` is a direct **or indirect** prerequisite of `C` (recursive) |
| `can_take(S, C)` | `S` has taken every direct prerequisite of `C`, and has not taken `C` |
| `missing(S, C, P)` | `P` is a prerequisite of `C` that `S` has not taken |

### Queries

Show at least **five** queries and their answers in the README, including:

- One that returns several answers through backtracking (`;`)
- One asked "backward" (which courses need `cs101`?)

### Stretch (optional)

Write a definite clause grammar (`-->`) that accepts arithmetic expressions from **your** Lab language, and compare it to your Lab 2 EBNF.

**Tools:** [SWISH](https://swish.swi-prolog.org) or SWI-Prolog.

---

## Summary — what each assignment teaches

| Language | Big idea students take away | Sebesta chapter |
| --- | --- | --- |
| COBOL | Types built for a domain (money, records, formatted output) | 1, 2, 6 |
| Rust | Lifetime and ownership checked at compile time | 5, 6, 10 |
| APL | Precedence is a design choice; arrays instead of loops | 6, 7 |
| Racket | Functions as values; programs as data | 9, 15 |
| Erlang | Concurrency through message passing, not shared memory | 13, 14 |
| Prolog | Declare what is true; let the system search | 16 |

---

## If time is short

| Keep | Drop or merge |
| --- | --- |
| Racket and Prolog (they back up the Chapter 15 and 16 lectures) | COBOL → make it a Week 1 discussion reading instead of code |
| APL (fast, and fits Week 6 perfectly) | Rust or Erlang — keep one |
