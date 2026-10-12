# Programming Languages — Midterm (Mock)

**Covers:** Weeks 1–7 — Sebesta Chapters 1, 3–9  
**Time:** 75 minutes  
**Points:** 100 (+3 bonus)

---

## Rules

| Allowed | Not allowed |
| --- | --- |
| Textbook (print) | Phones, laptops, tablets, e-readers |
| Printed slides, notes, handouts | Smartwatches, earbuds |
| Handwritten notes, any amount | Calculators (none needed) |
| Printed copies of your own lab code | Talking with anyone except the instructor |

- Show your work. Partial credit is given for clear reasoning.
- Code answers may be in Python, Java, C++, or clear pseudocode.
- Questions use **new** grammars and code on purpose. Copying a definition from the book will not earn full credit — apply it to the problem in front of you.

---

## Point summary

| Question | Topic | Points |
| --- | --- | --- |
| 1 | Language evaluation criteria | 10 |
| 2 | Grammars, derivations, parse trees | 10 |
| 3 | Ambiguity | 10 |
| 4 | Recursive descent | 8 |
| 5 | Lexical analysis | 8 |
| 6 | Semantics | 10 |
| 7 | Referencing environments | 12 (+1) |
| 8 | Data types | 8 |
| 9 | Expressions and side effects | 8 (+1) |
| 10 | Dangling else | 8 (+1) |
| 11 | Parameter passing | 8 |
| | **Total** | **100 (+3)** |

---

## Question 1 — Language evaluation criteria (10 points)

A new language, **Bolt**, proposes three features:

1. Variables are declared automatically the first time they are assigned.
2. `+` means numeric addition, string concatenation, **and** set union.
3. Semicolons at the end of statements are optional.

Pick **two** of the features. For each one:

- (a) Name one evaluation criterion (readability, writability, reliability, cost) the feature **improves**, and explain how.
- (b) Name one criterion it **harms**, and give a short code example of a mistake or confusion it could cause.

---

## Question 2 — Grammars, derivations, parse trees (10 points)

```text
<stmt>   → <id> = <expr>
<expr>   → <expr> # <term> | <term>
<term>   → <factor> @ <term> | <factor>
<factor> → ( <expr> ) | <id>
<id>     → a | b | c
```

- (a) (1 pt) Which operator has **higher** precedence, `#` or `@`? How can you tell from the grammar?
- (b) (2 pts) What is the associativity of `#`? Of `@`? How can you tell?
- (c) (4 pts) Write a **leftmost derivation** of `a = b # c @ a`.
- (d) (3 pts) Draw the **parse tree** for `a = a # b # c`. Circle or label which `#` is applied first.

---

## Question 3 — Ambiguity (10 points)

```text
<E>  → <E> ^ <E> | - <E> | <id>
<id> → x | y
```

- (a) (5 pts) Show that this grammar is **ambiguous** by drawing two different parse trees for `- x ^ y`. Under each tree, write the meaning with parentheses.
- (b) (5 pts) Rewrite the grammar so it is **unambiguous**, `^` binds **tighter** than unary `-`, and `^` is **right**-associative. It must still generate `x`, `- x`, `x ^ y`, `- x ^ y`, and `x ^ y ^ x`.

---

## Question 4 — Recursive descent (8 points)

**(a) (4 pts)** Write `parse_args()` for this rule. Return a list of argument nodes.

```text
<args> → <expr> { ',' <expr> }
```

You may use `current()` (type of the current token), `consume(type)`, and `parse_expr()`.

**(b) (2 pts)** Why does this rule break a recursive-descent parser? Rewrite it in EBNF so it works.

```text
<seq> → <seq> ; <stmt> | <stmt>
```

**(c) (2 pts)** Left factor these into one rule:

```text
<if> → if <cond> then <stmt>
     | if <cond> then <stmt> else <stmt>
```

---

## Question 5 — Lexical analysis (8 points)

A lexer has these rules:

| Label | Pattern |
| --- | --- |
| `IF`, `AND` | reserved words `if`, `and` |
| `ID` | a letter followed by letters or digits |
| `INT` | one or more digits |
| `FLOAT` | digits `.` digits |
| `GT`, `GE`, `ASSIGN` | `>`, `>=`, `=` |

Whitespace separates tokens but is not required between them.

- (a) (6 pts) List the tokens (label and lexeme, one per line) for:

  ```text
  if x2>=10.5and ifx
  ```

- (b) (2 pts) Identify **two** places in that input where the longest-match rule decides the answer. For each, say what the wrong (shorter) answer would have been.

---

## Question 6 — Semantics (10 points)

**(a) (6 pts)** Compute the **weakest precondition**. Show each step.

```text
{ ? }
x = 2 * y - 1;
y = x + 3;
{ y > 10 }
```

**(b) (4 pts)** An attribute grammar:

```text
1. <S> → <A>          <S>.count ← <A>.count
                      predicate: <A>.count = 2
2. <A> → a <A>        <A>[1].count ← <A>[2].count + 1
3. <A> → b <A>        <A>[1].count ← <A>[2].count
4. <A> → a            <A>.count ← 1
5. <A> → b            <A>.count ← 0
```

- (i) In plain words, what does `count` compute?
- (ii) Is `count` synthesized or inherited? How can you tell?
- (iii) Which of these strings are accepted (syntax **and** predicate)? `abab`, `aab`, `aaba`, `ba`

---

## Question 7 — Referencing environments (12 points, +1 bonus)

```javascript
function main() {
    var n = 1;
    function show() { print(n); }
    function runner(f) { var n = 2; f(); }
    function setup() { var n = 3; runner(show); }
    setup();
}
main();
```

`show` is passed as a parameter and called somewhere other than where it was defined. Different language implementations could make this program print different things.

- (a) Describe **at least three** different outputs this program could have.
- (b) For each output, describe what is different in that implementation — which `n` does `show` see, and why?
- **(+1)** Name the three approaches.

---

## Question 8 — Data types (8 points)

**(a) (4 pts)**

```text
type Celsius    = record { value: float }
type Fahrenheit = record { value: float }

var c: Celsius
var f: Fahrenheit
c = f
```

Is `c = f` legal under **name** type equivalence? Under **structural** type equivalence? Which rule is better for this program, and why?

**(b) (4 pts)**

```c
int *make_value(void) {
    int n = 42;
    return &n;
}
```

What problem does a caller of `make_value` face? Use the words **lifetime** and **dangling pointer**. Name one language and the mechanism it uses to prevent this.

---

## Question 9 — Expressions and side effects (8 points, +1 bonus)

```text
k = 2

function bump()
    k = k * 3
    return 1

result = k + bump()
print(result)
```

- (a) Give **two** possible outputs of this program.
- (b) For each output, explain what happens to produce it.
- **(+1)** Which output does Java produce, and why?

---

## Question 10 — Dangling else (8 points, +1 bonus)

```text
a = 5
b = -1

if (a > 0)
if (b > 0)
    print("both")
else
    print("other")
print("done")
```

- (a) Give **two** possible outputs of this program.
- (b) Explain why there are two, and what each interpretation does.
- **(+1)** Which output do Java and C produce, and why?

---

## Question 11 — Parameter passing (8 points)

```text
procedure update(p, q)
    p = p + q
    q = p * 2
```

Fill in the table. Each scenario starts fresh. For value-result, copy back **left to right** (`p` first, then `q`).

| Scenario | Pass-by-value | Pass-by-reference | Pass-by-value-result |
| --- | --- | --- | --- |
| `a = 3`, `b = 4`, `update(a, b)` — give `a` and `b` afterward | | | |
| `a = 3`, `update(a, a)` — give `a` afterward | | | |

---

*End of exam.*
