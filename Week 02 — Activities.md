# Week 2 — In-Class Activities

**Topics:** BNF, parse trees, derivations, ambiguity, precedence, associativity  
**Reading:** Section 3.1-3.3 (Sebesta)

Work with a partner unless your instructor says otherwise. Write answers on paper or a shared doc.

---

## Activity 1 — Syntax or semantic?

For each item, mark **S** (syntax) or **M** (semantic / meaning).

| # | Statement |
| --- | --- |
| 1 | `x = y +` is illegal because the expression is incomplete |
| 2 | `x = y + z` adds `y` and `z` |
| 3 | `if 3 then x = 1` is illegal because `3` is not a boolean |
| 4 | `if x then y =` is illegal because `=` has nothing on the right |
| 5 | `++x` is illegal in Python |
| 6 | `x = y` binds `x` to the value of `y` |

---

## Activity 2 — Read the grammar

Given:

```
<stmt>   → <assign> | print id
<assign> → id = <expr>
<expr>   → id + id | id
```

Mark each string **L** (legal) or **I** (illegal).

| String | L or I |
| --- | --- |
| `a = b` | |
| `a = b + c` | |
| `print x` | |
| `print` | |
| `a + b` | |
| `a = b = c` | |
| `x = y +` | |

For one illegal string, write which rule fails (or why no rule applies).

---

## Activity 3 — Write one rule

Extend the grammar from Activity 2 so **subtraction** is allowed in expressions — same shape as addition (`id - id` only; no chaining yet).

Write your new `<expr>` rule below:

```
<expr> → 
```

---

## Activity 4 — Leftmost derivation

Grammar:

```
<assign> → id = <expr>
<expr>   → id + <term>
<term>   → id
```

Write a **leftmost derivation** for `x = a + b`.

| Step | Sentence |
| --- | --- |
| 1 | `<assign>` |
| 2 | |
| 3 | |
| 4 | |
| 5 | |

**Stretch:** Is `x = a + b + c` legal under this grammar? Why or why not?

---

## Activity 5 — Draw the tree

Using the grammar from Activity 4 and the string `x = a + b`, draw a parse tree:

- Root is `<assign>`
- Label every nonterminal
- Leaves are terminals (`id`, `+`, `=`)

Compare with your partner.

---

## Activity 6 — Spot the ambiguity

Grammar:

```
<expr> → <expr> + <expr> | <expr> * <expr> | id
```

**A.** For `a + b * c`, draw **two** different parse trees.

**B.** If you evaluated each tree with normal arithmetic values for `a`, `b`, and `c`, would the two trees give the same number? Explain in one sentence.

---

## Activity 7 — Fix precedence

Start from the ambiguous grammar in Activity 6.

Rewrite it using `<expr>`, `<term>`, and `<factor>` so that:

- `*` binds tighter than `+`
- `a + b * c` has **only one** parse tree

Then sketch that single tree for `a + b * c`.

```
<expr>   → 
<term>   → 
<factor> → 
```

---

## Activity 8 — Left or right?

| Grammar | String | Your predicted grouping |
| --- | --- | --- |
| `<expr> → <expr> - <term> \| <term>` | `a - b - c` | |
| `<assign> → id = <assign> \| <expr>` | `a = b = c` | |

Predict the grouping first (no drawing). Check with your partner. Then draw one of the trees.

---

## Activity 9 — TinyLang brainstorm

In groups of 3, on one page list:

1. **Keywords** you want (examples: `if`, `then`, `else`, `while`, `print`, `fun`)
2. **Operators** (`+`, `-`, `*`, `/`, `=`, `==`, …)
3. **One statement form** in English, then as a rough BNF rule

Be ready to share with the class.
