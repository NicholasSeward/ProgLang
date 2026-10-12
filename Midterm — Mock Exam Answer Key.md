# Programming Languages — Midterm (Mock) — Answer Key

**Instructor only.** Point values match the exam. Accept equivalent answers with correct reasoning.

---

## Question 1 — Language evaluation criteria (10)

5 points per feature: 2 for the improved criterion with explanation, 1 for the harmed criterion, 2 for a concrete example.

| Feature | Improves | Harms | Example of harm |
| --- | --- | --- | --- |
| Implicit declaration | Writability — less to type | Reliability — typos create new variables silently | `total = 0` then `totl = total + 5`; `total` never changes |
| `+` for add / concat / union | Writability — one symbol to remember | Readability — the meaning depends on operand types; reliability — wrong types still "work" | `x + y` where `x` is accidentally a string: `"5" + 3` concatenates instead of adding |
| Optional semicolons | Writability — less noise | Readability / reliability — statement boundaries can be guessed wrong | A line break inside an expression ends the statement early (as in JavaScript's `return` followed by a newline) |

Other reasonable criteria (e.g. cost of implementation for the parser) are fine if justified.

---

## Question 2 — Grammars (10)

**(a) (1)** `@` is higher. It is generated lower in the grammar (in `<term>`), so it ends up lower in the parse tree and is applied first.

**(b) (2)** `#` is **left**-associative (left-recursive rule `<expr> → <expr> # <term>`). `@` is **right**-associative (right-recursive rule `<term> → <factor> @ <term>`).

**(c) (4)**

```text
<stmt> ⇒ <id> = <expr>
       ⇒ a = <expr>
       ⇒ a = <expr> # <term>
       ⇒ a = <term> # <term>
       ⇒ a = <factor> # <term>
       ⇒ a = <id> # <term>
       ⇒ a = b # <term>
       ⇒ a = b # <factor> @ <term>
       ⇒ a = b # <id> @ <term>
       ⇒ a = b # c @ <term>
       ⇒ a = b # c @ <factor>
       ⇒ a = b # c @ <id>
       ⇒ a = b # c @ a
```

−1 for each step that expands a nonterminal other than the leftmost one (max −2). −2 if the derivation does not reach the sentence.

**(d) (3)** Grouping is `(a # b) # c`. The lower-left `#` (combining `a` and `b`) is applied first.

```text
<stmt>
├── <id> → a
├── =
└── <expr>
    ├── <expr>
    │   ├── <expr> → <term> → <factor> → <id> → a
    │   ├── #          ← applied first
    │   └── <term> → <factor> → <id> → b
    ├── #
    └── <term> → <factor> → <id> → c
```

---

## Question 3 — Ambiguity (10)

**(a) (5)** 2 points per tree, 1 point for the two meanings.

```text
Tree 1: -(x ^ y)              Tree 2: (-x) ^ y

    <E>                           <E>
   ├── -                         ├── <E>
   └── <E>                       │   ├── -
       ├── <E> → x               │   └── <E> → x
       ├── ^                     ├── ^
       └── <E> → y               └── <E> → y
```

**(b) (5)** One correct answer:

```text
<E>  → - <E> | <P>
<P>  → <id> ^ <E> | <id>
<id> → x | y
```

- `- x ^ y` can only be `- (x ^ y)` — a leading `-` must use `- <E>`
- `x ^ y ^ x` groups as `x ^ (y ^ x)` — right recursion
- This version also allows `x ^ - y`, which is fine

Also accept: `<P> → <id> ^ <P> | <id>` (no unary minus in exponents).  
Grading: −2 if `^` is left-associative; −2 if `-` binds tighter than `^`; −2 if still ambiguous; −1 if a required string is missing.

---

## Question 4 — Recursive descent (8)

**(a) (4)**

```python
def parse_args(self):
    args = [self.parse_expr()]
    while self.current() == "COMMA":
        self.consume("COMMA")
        args.append(self.parse_expr())
    return args
```

| Element | Points |
| --- | --- |
| Parses the first `<expr>` | 1 |
| `{ ',' <expr> }` written as a loop checking for a comma | 2 |
| Consumes the comma and collects each argument | 1 |

**(b) (2)** The rule is **left-recursive**: `parse_seq` would call `parse_seq` first without consuming a token, recursing forever (1). Rewrite (1):

```text
<seq> → <stmt> { ; <stmt> }
```

**(c) (2)**

```text
<if> → if <cond> then <stmt> [ else <stmt> ]
```

---

## Question 5 — Lexical analysis (8)

**(a) (6)** 1 point per token.

```text
IF if
ID x2
GE >=
FLOAT 10.5
AND and
ID ifx
```

**(b) (2)** Any two, 1 point each:

| Spot | Longest match | Wrong shorter answer |
| --- | --- | --- |
| `x2` | `ID x2` | `ID x` then `INT 2` |
| `>=` | `GE >=` | `GT >` then `ASSIGN =` |
| `10.5` | `FLOAT 10.5` | `INT 10`, then an error on `.` |
| `ifx` | `ID ifx` | `IF if` then `ID x` |

---

## Question 6 — Semantics (10)

**(a) (6)**

```text
Postcondition:            y > 10
Back through y = x + 3:   x + 3 > 10   →  x > 7
Back through x = 2*y - 1: 2*y - 1 > 7  →  2*y > 8  →  y > 4

Weakest precondition: { y > 4 }
```

3 points per assignment step. −1 for arithmetic errors.

**(b) (4)**

- (i) (1) `count` is the number of `a`s in the string.
- (ii) (1) **Synthesized** — each parent's value is computed from its children (`<A>[1]` from `<A>[2]`).
- (iii) (2) Accepted: `abab` (2 a's), `aab` (2 a's). Rejected: `aaba` (3), `ba` (1). −1 per mistake.

---

## Question 7 — Referencing environments (12, +1)

4 points per implementation: 1 for the output, 3 for explaining which environment `show` uses.

| Output | What the implementation does | Approach (+1 for naming all three) |
| --- | --- | --- |
| `1` | `show` uses the environment where it was **defined** — inside `main`, so it sees `main`'s `n` | **Deep** binding (what static-scoped languages like JavaScript actually do) |
| `2` | `show` uses the environment where it is **called** — inside `runner`, so it sees `runner`'s `n` | **Shallow** binding (natural for dynamic scope) |
| `3` | `show` uses the environment of the statement that **passed** it — inside `setup`, so it sees `setup`'s `n` | **Ad hoc** binding |

Full credit for each row needs both the number and the reason. "It's static scope" alone for `1` earns 2 of 3 explanation points; they should say the environment is where `show` was **defined**.

---

## Question 8 — Data types (8)

**(a) (4)**

- Name equivalence: **illegal** — different type names (1)
- Structural equivalence: **legal** — same structure (1)
- Name equivalence is better here: it stops Celsius and Fahrenheit values from being mixed, which is exactly the unit bug the two type names exist to prevent (2)

**(b) (4)**

- `n` is a stack-dynamic local. Its **lifetime** ends when `make_value` returns, but the returned pointer still refers to that memory → a **dangling pointer**. Using it reads or writes memory that may now belong to something else (3).
- Prevention (1), any one:
  - Rust — the borrow checker rejects returning a reference to a local
  - Java / Python — no address-of operator for locals; objects are on the heap with garbage collection
  - C++ — return by value or a smart pointer (accept if explained)
  - Tombstones or lock-and-key (accept from the textbook)

---

## Question 9 — Expressions and side effects (8, +1)

4 points per output: 2 for the value, 2 for the explanation.

| Output | What happens |
| --- | --- |
| `3` | **Left operand first.** `k` is read while it's still 2. Then `bump()` changes `k` to 6 and returns 1. `2 + 1 = 3`. |
| `7` | **Right operand first.** `bump()` runs first and changes `k` to 6, then returns 1. `k` is read as 6. `6 + 1 = 7`. |

The difference comes from the **functional side effect** in `bump` (it changes the nonlocal `k`) combined with an unspecified **operand evaluation order**.

**(+1)** Java prints `3`. Java guarantees **left-to-right** operand evaluation, so `k` is read before `bump()` runs. (C and C++ leave the order unspecified, so either could happen.)

---

## Question 10 — Dangling else (8, +1)

4 points per output: 2 for the output, 2 for the interpretation.

| Interpretation | Meaning | Output |
| --- | --- | --- |
| `else` belongs to the **inner** `if (b > 0)` | `a > 0` is true; `b > 0` is false → its `else` runs | `other` then `done` |
| `else` belongs to the **outer** `if (a > 0)` | `a > 0` is true → the inner `if` runs and prints nothing; the `else` is skipped | `done` |

There are two outputs because the grammar is **ambiguous**: `if … if … else` has two parse trees, and nothing in the syntax says which `if` the `else` belongs to.

**(+1)** Java and C print `other` then `done`. Their rule is that an `else` matches the **nearest** unmatched `if`.

---

## Question 11 — Parameter passing (8)

| Scenario | Value | Reference | Value-result |
| --- | --- | --- | --- |
| `update(a, b)` | `a = 3`, `b = 4` (1) | `a = 7`, `b = 14` (1) | `a = 7`, `b = 14` (1) |
| `update(a, a)` | `a = 3` (1) | `a = 12` (2) | `a = 12` (2) |

Work for `update(a, a)`:

- **Reference:** `p`, `q`, and `a` are one variable. `p = 3 + 3 = 6` → `a = 6`. `q = 6 * 2 = 12` → `a = 12`.
- **Value-result:** local copies `p = 3`, `q = 3`. `p = 6`, `q = 12`. Copy back `p` → `a = 6`, then `q` → `a = 12`.

Give 1 of 2 for the aliasing cells if the work is right but there's an arithmetic slip.

---

## Grading notes

- This exam is open printed resources, so reward **application** over recall. A copied definition with no connection to the given code or grammar earns at most half credit.
- For traces, give full credit for a correct answer with clear work, and partial credit for a correct method with one arithmetic slip.
- Bonus points can raise the score above 100.
- Rough time budget: Questions 1–6 about 45 minutes, Questions 7–11 about 30 minutes.
