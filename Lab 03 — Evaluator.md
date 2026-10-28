# Lab 3 — Evaluator

**Course:** Programming Languages  
**Accept assignment:** [classroom50.org — Lab 3](https://classroom50.org/Seward-Classes/programming-languages/assignments/lab-3/accept)  
**Submit:** link to your Git repository (from the accept link above)

---

## Goal

Extend **your** language from Lab 2 so programs **run**:

```text
source text  →  tokens  →  AST  →  values
```

```text
Lab 1 Lexer → Lab 2 Parser → Lab 3 Evaluator → Lab 4 Interpreter
```

Lab 2 built the tree. Lab 3 walks that tree and produces results. Names live in an **environment**.

You keep your language name and design. You are **not** told a fixed set of operators. You **are** required to evaluate the expression and binding constructs below, and to show that with tests.

`if`, loops, and function **calls** are Lab 4. Do not crash if a program contains them: either run them, or stop with a clear message that says the construct is not evaluated until Lab 4.

---

## Start from Lab 2

Copy your Lab 2 lexer and parser into the new repo and build on them.

- If Lab 2 had issues or was kicked back, fix them here. A wrong tree evaluates to a wrong value.
- You may change your language. If you do, update the EBNF and list the changes in the README under **Changes since Lab 2**.

The **default** run must **execute** the program and print its output. Printing the AST is no longer the default. Keeping a `--ast` flag is fine for debugging.

---

## What you must evaluate

### Expressions

| Category | Requirement | Your call |
| --- | --- | --- |
| **Literals** | Integers, floats, and strings produce values | How each is printed |
| **Variables** | A name looks up a value using your binding rule | What happens if it was never bound — document it |
| **Arithmetic** | `+ - * /` (or your equivalents) on numbers | Integer vs float division — document it |
| **Precedence and associativity** | Same rules as your Lab 2 grammar | Already in the tree; evaluation must respect the tree |
| **Parentheses** | Already gone from the AST; the tree is the grouping | — |
| **Comparison** | At least `<` and `==` (or your equivalents) produce a boolean (or your boolean stand-in) | How `true` / `false` print |
| **Unary** | The unary operator you parsed in Lab 2 | — |

### Statements

| Category | Requirement | Your call |
| --- | --- | --- |
| **Binding / assignment** | Store a value under a name | When a name is created, whether it can be assigned again, and what a use before binding does — **your rules, written in the README** |
| **Output** | A print (or your output statement) produces the program’s output | Any number of arguments. Any layout. **Document what it prints** |
| **Program** | Statements run in order | — |

Top-level names may all live in one **global** scope. Block scope is not required. See **Environment** for the one constraint that looks ahead to functions.

---

## Not required until Lab 4

These were **parsed** in Lab 2. You do not have to **run** them yet:

- `if` / `else`
- loops
- function calls (and executing function bodies)

If the source contains one of these, do one of the following:

- evaluate it correctly, or
- stop with a clear message, for example `Runtime error: 'if' is not evaluated until Lab 4`

A function **definition** may be stored and skipped. Do not run its body at the point of the definition.

---

## Environment

Names live in an environment. For this lab, that environment can be a single global scope. You do not need block scope, and you do not need to enter a second scope in any test.

**Lookahead.** Lab 4 adds function calls. A call needs its own scope and must still see (or deliberately not see) the caller’s names when it returns. Build the environment so that scope can be **bolted on** later: an object or map you own, with a place for a parent or an outer frame. This lab may leave that parent empty.

A pile of host-language globals, with no environment value you can wrap, will not take a function scope later. A class, a dict plus an unused parent pointer, or a stack that currently holds one map are all fine.

Document your binding rule in the README: where a write goes, and what a lookup does when the name was never bound. You do not have to specify shadowing until you actually have two scopes.

---

## Print output

Running your program on a file prints **what your print does** to stdout. The shape is yours:

- one argument or many
- one line, several lines, or a format of your own
- numbers, strings, and booleans rendered however you like

Say that in the README. The expected files must match it.

Do not print the AST, a token dump, or a debug trace on the default run.

### Example

Source, in some hypothetical language:

```text
x = 2 + 3 * 4
print x
y = x - 1 - 1
print y
```

One possible output, if `print` writes one value per line:

```text
14
12
```

`3 * 4` is grouped under `+` by the tree, so the value is `14`, not `20`.  
`x - 1 - 1` is left-associative, so the value is `12`, not `2`.

`print x, y` on one line, a label in front of the value, or any other layout is fine when the README and the expected file agree.

---

## Runtime errors

On a bad program, print one clear error and stop. The message must say **what** went wrong. Include the name if the problem is a name. Include a line number if you have one.

```text
Runtime error: undefined variable 'x'
```

```text
Runtime error: division by zero
```

A raw stack trace from your implementation language is not enough.

Document what you do for:

- undefined names
- division by zero
- operations on the wrong kinds of values (e.g. `1 + "hi"`), if those can occur

Stopping at the first error is fine.

---

## Implementation rules

- Write the evaluator yourself. **No** `eval` of the source text, and no interpreter library that runs the program for you.
- The lexer and parser remain your own (no parser generators).
- Walk the AST. Do not re-parse strings inside the evaluator.

---

## Test inputs (required) — we will not run a hidden test suite

We are **not** grading you with our own automated tests.

You **must** provide a **good set of tests** that together exercise **all parts of your spec**.

### Test files: input + expected output

Every test is a **pair of plain text files**:

| File | Contents |
| --- | --- |
| `name.input.txt` | Source code in your language |
| `name.expected.txt` | Exactly what your program prints (output lines, or the runtime error) |

```text
tests/
├── 01_arith.input.txt
├── 01_arith.expected.txt
├── 02_precedence.input.txt
├── 02_precedence.expected.txt
├── ...
├── 12_undefined.input.txt
└── 12_undefined.expected.txt
```

- Every input file has a matching expected file, and every expected file has a matching input file.
- Running your evaluator on `name.input.txt` must print exactly what is in `name.expected.txt`. We will spot-check this.
- Work out the expected values by hand. Pasting whatever your code prints copies its bugs into the expected file.

A script that runs every pair and reports pass/fail is nice but not required.

### What your tests must cover

At least:

| Cover | Example idea |
| --- | --- |
| Arithmetic | `2 + 3`, including a float if you have them |
| Precedence | `2 + 3 * 4` prints `14` (or your language’s equivalent) |
| Associativity | `10 - 3 - 2` prints `5`, not `9` |
| Parentheses | `(2 + 3) * 4` prints `20` |
| Comparison | a true case and a false case |
| Unary | the unary operator from Lab 2 |
| Strings | a string prints as you documented |
| Assignment | bind a name, then print it |
| Your binding rule | a second assignment, or a use before binding, prints what the README says (a new value or your runtime error) |
| A short “real” program | several bindings and prints in one file |
| One deferred construct (if you do not run it yet) | `if` / loop / call prints your Lab 4 message instead of crashing |

A test that enters a second scope is **optional**. It is a good check that the parent pointer (or stack) actually works, and it is recommended if you already have block scope. It is not required. You may have no block scope at all in this lab.

Put the pairs under `tests/`. In the README, list what each test is meant to prove.

---

## Language / tooling

Any language is fine.

Your repo **README must** include:

1. **Language name**
2. **Environment** — OS notes, language version, packages
3. **How to run** a program file
4. **How to run your tests** and compare the output with the expected files
5. **Print** — how many arguments, and exactly what a print writes
6. **Names** — your binding rule, and where a function scope will attach in Lab 4 (even if this lab never opens one)
7. **Runtime errors** — undefined name, division by zero, bad operands
8. **What is deferred to Lab 4**
9. **Changes since Lab 2** (or “none”)
10. **AI disclaimer** (see below)
11. Pointer to your **EBNF**

---

## AI disclaimer (required in README)

Include a short section titled **AI / assistance** that states, in your own words:

- Whether you used AI tools (ChatGPT, Copilot, Cursor, etc.) on this lab
- What you used them for (ideas, debugging, boilerplate, …)
- That you understand and can explain every line you submit

Using AI is allowed as a tool. Submitting work you cannot explain is not. False statements in the disclaimer are an academic honesty issue.

---

## Suggested README outline

```text
# <YourLanguageName> Evaluator (Lab 3)

## Language name and summary
## EBNF (or link to GRAMMAR.md)
## Changes since Lab 2
## Names (and where a function scope will attach)
## Print
## Runtime errors
## Deferred to Lab 4
## Run
## Tests (how to run, what each pair covers)
## AI / assistance
```

---

## Design tips (optional)

- One eval function per AST node kind. `BinOp` looks at the operator and evaluates both children first.
- Evaluate **the tree you already built**. If precedence is wrong, fix the parser, not the evaluator.
- Give the environment a parent slot, or keep scopes on a stack, even if Lab 3 only ever uses the global frame. Lab 4 should be able to push a function scope without rewriting lookup.
- Write the binding rule and the print format down before you code them. The tests should be copied from that writeup, not the other way around.
- Catch your implementation language’s divide-by-zero and print your own runtime error.

---

## What to put in the repo

| Item | |
| --- | --- |
| Lexer + parser + evaluator source | your implementation |
| EBNF | README or separate file |
| README | name, environment, run, tests, value format, runtime errors, deferred features, changes, AI disclaimer |
| `tests/*.input.txt` | programs covering the spec |
| `tests/*.expected.txt` | exact expected output for each input |
| Optional | a script that runs every pair and reports pass/fail |

---

## What to submit

1. Accept: https://classroom50.org/Seward-Classes/programming-languages/assignments/lab-3/accept
2. Push to the repo that creates
3. Submit that **repository link** as directed on classroom50 / LMS

---

## Grading

| Result | Score |
| --- | --- |
| Meets the spec | **100** |
| Each minor issue | **−10** |
| More than **3** issues | **Kicked back** for another attempt (not scored until resubmitted cleanly) |

### What counts as “to spec”

- Expressions evaluate with the same precedence and associativity as your Lab 2 trees
- Names follow the binding rule in the README. The environment is a value you can extend with a function scope later; this lab may use only the global scope
- Print output matches the README and your expected files
- A missing name does what the README says; division by zero is a runtime error, not a crash
- Deferred constructs do not crash the run
- Every test has an input file and an expected-output file, and the run matches
- README has environment, run, test guide, value format, runtime errors, Lab 4 deferrals, changes, and AI disclaimer

### Examples of minor issues

- One operator edge case wrong (for example integer division rounding)
- Error message missing the variable name
- Thin test set for precedence
- A test missing its expected file, or an expected file that doesn't match the actual output
- Print’s arguments or layout are not described, so the expected files cannot be checked against a rule

### Kick-back territory (more than three issues, or a major miss)

- Does not evaluate expressions, or ignores the AST and evals source text
- Names are loose host-language globals, with no environment a function scope can attach to
- Crashes on undefined names, division by zero, or an `if` / loop / call
- Default run still prints only the AST
- Cannot run from README
- No AI disclaimer

---

## Academic honesty

You own your language design and your code. Discuss ideas freely; do not copy another student’s evaluator. Cite non-trivial external code in the README.
