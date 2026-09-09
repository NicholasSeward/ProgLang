# Week 2 — In-Class Activities & Homework

**Topics:** BNF, parse trees, derivations, ambiguity, precedence, associativity  
**Reading:** Section 3.1-3.3 (Sebesta)  
**Due:** Homework due before Week 3 class (adjust date on syllabus)

---

## In-class activities

Use these between lecture chunks or as a full “workshop” block (~25–35 min total). Mix and match based on time.

### Activity 1 — Syntax or semantic? (5 min, whole class)

**Goal:** Reinforce that grammar only describes *shape*.

Read each item aloud. Students hold up **S** (syntax) or **M** (semantic):


| #   | Statement                                                       | Answer                                                                        |
| --- | --------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| 1   | `x = y +` is illegal because the expression is incomplete       | S                                                                             |
| 2   | `x = y + z` adds `y` and `z`                                    | M                                                                             |
| 3   | `if 3 then x = 1` is illegal because `3` is not a boolean       | M *(type)* — accept **S** if you treat “boolean required” as static semantics |
| 4   | `if x then y =` is illegal because `=` has nothing on the right | S                                                                             |
| 5   | `++x` is illegal in Python                                      | S *(in Python)*                                                               |
| 6   | `x = y` binds `x` to the value of `y`                           | M                                                                             |


**Debrief:** Grammar catches #1 and #4; a type checker catches #3; meaning is #2, #5, #6.

---

### Activity 2 — Read the grammar (8 min, pairs)

**Goal:** Practice reading BNF with `→` and `|`.

Given:

```
<stmt>   → <assign> | print id
<assign> → id = <expr>
<expr>   → id + id | id
```

Pairs mark each string **legal (L)** or **illegal (I)**:


| String      |                  |
| ----------- | ---------------- |
| `a = b`     | L                |
| `a = b + c` | L                |
| `print x`   | L                |
| `print`     | I                |
| `a + b`     | I *(not a stmt)* |
| `a = b = c` | I                |
| `x = y +`   | I                |


**Debrief:** Call on pairs for one **illegal** string and which rule fails first.

---

### Activity 3 — Write one rule (5 min, individual → share)

**Goal:** Produce a minimal grammar extension.

**Prompt:** Extend the grammar from Activity 2 so that **subtraction** is allowed in expressions (same shape as addition: `id - id` only — no chaining yet).

Expected:

```
<expr> → id + id | id - id | id
```

Share 2–3 on the board. Check: did anyone use `id` on both sides of `-`?

---

### Activity 4 — Leftmost derivation race (10 min, pairs)

**Goal:** Step through rewrites mechanically.

Grammar:

```
<assign> → id = <expr>
<expr>   → id + <term>
<term>   → id
```

**Task:** Write the **leftmost derivation** for `x = a + b`.


| Step | Sentence           |
| ---- | ------------------ |
| 1    | `<assign>`         |
| 2    | `id = <expr>`      |
| 3    | `id = id + <term>` |
| 4    | `id = id + id`     |
| 5    | `x = a + b`        |


First pair to finish **correctly** explains step 3 to the class.

**Stretch:** Same grammar — is `x = a + b + c` legal? *(No — no rule for chaining.)*

---

### Activity 5 — Draw the tree (8 min, individual)

**Goal:** Connect derivation to parse tree.

Use the grammar from Activity 4 and string `x = a + b`.

Draw a parse tree with:

- Root `<assign>`
- Every nonterminal labeled
- Leaves as terminals (`id`, `+`, `=`)

Compare with neighbor. Instructor sketches canonical tree on board.

---

### Activity 6 — Spot the ambiguity (12 min, pairs)

**Goal:** See two trees for one string.

Grammar:

```
<expr> → <expr> + <expr> | <expr> * <expr> | id
```

**Task A:** For `a + b * c`, draw **two** different parse trees.

**Task B:** In one sentence each, what value would you get if you evaluated tree 1 vs tree 2? *(Assume normal math: `*` before `+` — which tree matches that? Neither is forced by this grammar!)*

**Debrief:** Ambiguity = grammar problem. Math “convention” is **not** the same as an unambiguous grammar.

---

### Activity 7 — Fix precedence (10 min, pairs)

**Goal:** Layer nonterminals.

**Starting point** (ambiguous):

```
<expr> → <expr> + <expr> | <expr> * <expr> | id
```

**Task:** Rewrite using `<expr>`, `<term>`, `<factor>` so `a + b * c` has **only one** tree ( `*` tighter than `+` ).

Share answer:

```
<expr>   → <expr> + <term> | <term>
<term>   → <term> * <factor> | <factor>
<factor> → id
```

Pairs sketch the single tree for `a + b * c`.

---

### Activity 8 — Left or right? (5 min, think-pair-share)

**Goal:** Associativity from grammar shape.


| Grammar                             | String      | Grouping           |
| ----------------------------------- | ----------- | ------------------ |
| `<expr> → <expr> - <term> | <term>` | `a - b - c` | Left: `((a-b)-c)`  |
| `<assign> → id = <assign> | <expr>` | `a = b = c` | Right: `(a=(b=c))` |


**Prompt:** Without drawing, predict grouping for `a - b - c` with the first grammar. Pair check. Then draw tree.

---

### Activity 9 — TinyLang brainstorm (10 min, small groups)

**Goal:** Start the homework / Lab 1–2 target language.

Groups of 3 list on one page:

1. **Keywords** they want (e.g. `if`, `then`, `else`, `while`, `print`, `fun`)
2. **Operators** (`+`, `-`, `*`, `/`, `=`, `==`, …)
3. **One statement form** in English → rough BNF (one rule)

Post sheets or photo for “TinyLang gallery walk.”

**Instructor:** Collect ideas; publish a **starter TinyLang EBNF** on LMS before Lab 1.

---

### Suggested timing (one 75-min session)


| Block    | Time   | Activity                     |
| -------- | ------ | ---------------------------- |
| Lecture  | 15 min | Steps 0–3 (id, assign, expr) |
| Activity | 8 min  | 2 + 3                        |
| Lecture  | 10 min | Lists, if, parse trees       |
| Activity | 10 min | 4 + 5                        |
| Lecture  | 10 min | Ambiguity                    |
| Activity | 12 min | 6 + 7                        |
| Lecture  | 8 min  | Associativity, EBNF intro    |
| Activity | 5 min  | 8                            |
| Wrap     | 5 min  | 9 (assign homework)          |


---

## Homework — Grammar problem set

**Submit:** PDF or typed markdown on LMS  
**Points:** 25 (adjust to your scale)  
**Policy:** Individual work; cite any collaboration

### Part A — Reading check (4 pts)

1. **(2 pts)** In your own words, what is the difference between **syntax** and **semantics**? Give one example of each kind of error.
2. **(2 pts)** Sebesta uses `→` in grammars. What does `|` mean? What is the difference between `id` and `<expr>` in a rule?

---

### Part B — Legality and derivations (8 pts)

Grammar:

```
<program> → <stmt>
<stmt>    → <assign> | print <id-list>
<assign>  → id = <expr>
<expr>    → <expr> + <term> | <term>
<term>    → <term> * <factor> | <factor>
<factor>  → id | number | ( <expr> )
<id-list> → id | <id-list> , id
```

1. **(2 pts)** Mark each **legal (L)** or **illegal (I)**:
  - `x = 3 + 4 * 5`
  - `print a, b`
  - `print`
  - `(x + y) * 2`
  - `x = y = z`
2. **(4 pts)** Give a **complete leftmost derivation** for:
  `x = 3 + 4 * 5`
3. `y = 3 * x`
4. **(2 pts)** Draw the **parse tree** for the same string (or attach a clear photo/diagram).

---

### Part C — Write grammars in BNF (6 pts)

Use `→` and `|`. Terminals: `id`, `number`, keywords in plain text.

1. **(2 pts)** Write rules for a `while` loop:
  `while <expr> do <stmt>`
2. **(2 pts)** Write rules for an **if-with-optional-else** using **two separate rules** (pure BNF, no `[ ]`):
  `if <expr> then <stmt>` and `if <expr> then <stmt> else <stmt>`
3. **(2 pts)** Write a two-rule BNF for a comma-separated `<id-list>` with **one or more** ids (same pattern as lecture).

---

### Part D — Ambiguity and design (7 pts)

1. **(3 pts)** Consider:
  ```
   <expr> → <expr> + <expr> | id
  ```
   Draw **two different parse trees** for `a + b + c`. Is this grammar ambiguous? Explain in one sentence.
2. **(4 pts)** Rewrite the expression grammar using `<expr>`, `<term>`, and `<factor>` so that:
  - `*` binds tighter than `+`
    - `+` is **left-associative**
    - parentheses override precedence
    Test: your grammar should yield **one** tree for `a + b * c` and left-assoc grouping for `a + b + c`.

---

### Part E — TinyLang draft (required for Lab 1–2) (0 pts graded / completion)

1. **TinyLang syntax draft** — one page:
  - Language name
    - List of **terminals** (token types your lexer will emit)
    - BNF or EBNF for: assignment, `if`, at least one loop **or** function call, expressions with `+` `-` `*` and parentheses
    - **Two** sample legal programs (3–5 lines each)
    - **One** sample **illegal** program with a sentence explaining which rule it violates
    This document is the spec for Labs 1–2. Instructor may publish a merged class version.

---

## Optional challenge (extra credit +3)

**Dangling else:** Show two parse trees for:

```
if a then if b then s1 else s2
```

using a naive single-rule `<if-stmt>`. Then write **matched/unmatched** BNF (Section 3.3 / lecture) so only **one** tree is possible, with `else` matching the **inner** `if`.

---

## Instructor answer key (quick reference)

### Homework 3 — legality


| String          |                                     |
| --------------- | ----------------------------------- |
| `x = 3 + 4 * 5` | L                                   |
| `print a, b`    | L                                   |
| `print`         | I                                   |
| `(x + y) * 2`   | L                                   |
| `x = y = z`     | I *(no chaining assign in grammar)* |


### Homework 4 — derivation sketch

```
<program> → <stmt> → <assign> → id = <expr>
→ id = <expr> + <term> → id = <term> + <term>
→ id = <term> + <term> * <factor> → …
→ id = <factor> + <term> * <factor>
→ id = number + <term> * <factor>
→ x = 3 + 4 * 5
```

### Homework 9

Ambiguous — two trees: `(a+b)+c` vs `a+(b+c)`.

### Homework 10 — sample answer

```
<expr>   → <expr> + <term> | <term>
<term>   → <term> * <factor> | <factor>
<factor> → id | number | ( <expr> )
```

---

## Book cross-reference (Sebesta Ch. 3)

If you want extra practice from the text, assign a subset of **Chapter 3 Review Questions / Problem Set** after Section 3.3. Typical fits:

- Writing BNF for selection and iteration
- Identifying ambiguity in a grammar
- Drawing parse trees for arithmetic expressions
- Explaining precedence and associativity in a given grammar

Pair book problems with **Part E (TinyLang)** so written work feeds the semester project.

---

## Grading rubric (homework)


| Part | Pts        | Criteria                                                      |
| ---- | ---------- | ------------------------------------------------------------- |
| A    | 4          | Clear definitions; correct use of terms                       |
| B    | 8          | Correct L/I; derivation steps valid; tree matches grammar     |
| C    | 6          | Rules well-formed; `while` / if / list patterns correct       |
| D    | 7          | Two trees for #9; layered grammar fixes precedence + left `+` |
| E    | completion | Spec is complete enough to start Lab 1 lexer                  |


Partial credit: reward correct **reasoning** on derivations and trees even if notation slips.