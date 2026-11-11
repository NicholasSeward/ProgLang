# Lab 4 — Mini Interpreter

**Course:** Programming Languages  
**Accept assignment:** [classroom50.org — Lab 4](https://classroom50.org/Seward-Classes/programming-languages/assignments/lab-4/accept)  
**Submit:** link to your Git repository (from the accept link above)

---

## Goal

Finish **your** language. Lab 3 evaluated expressions and names. Lab 4 runs the control and functions you already **parsed** in Lab 2:

```text
source text  →  tokens  →  AST  →  run
```

```text
Lab 1 Lexer → Lab 2 Parser → Lab 3 Evaluator → Lab 4 Interpreter
```

This repo is the artifact you demo in Week 14. Keep the design you can explain.

You keep your language name. Binding rules, print layout, and syntax remain **your** choices, documented in the README. You **are** required to run conditionals, repetition (loop or recursion), and function definitions with calls.

---

## Start from Lab 3

Copy your Lab 3 lexer, parser, and evaluator into the new repo and build on them.

- Fix anything kicked back from Lab 3. Wrong values stay wrong under `if` and calls.
- You may change the language. If you do, update the EBNF and list the changes under **Changes since Lab 3**.

The **default** run executes a program file and prints the program's output. `--ast` or `--tokens` flags are fine for debugging.

---

## What you must run

Everything Lab 3 required still works. On top of that:

### Control

| Category | Requirement | Your call |
| --- | --- | --- |
| **Conditional** | `if` with a condition and a then-branch; `else` if your Lab 2 grammar has it | How truthiness works (only booleans? nonzero? non-empty?) |
| **Repetition** | At least one of: a **loop** (`while` / `for` / …), or **recursion** through a user-defined function that calls itself | Syntax and termination |

If you choose recursion instead of a loop, your tests must show a recursive function that bottoms out and returns (or prints) a clear result. Document a recursion limit if your host language will hit one.

### Functions

| Category | Requirement | Your call |
| --- | --- | --- |
| **Definition** | Bind a name to a function (parameters + body) | Syntax you already parse |
| **Call** | Evaluate arguments, bind them to parameters, run the body, return to the caller | Positional args; default/keyword args optional |
| **Scope** | Each call gets its **own** environment for parameters (and locals you allow) | Whether the body sees enclosing names (static chain / globals only / …) — **document it** |
| **Return** | The call produces a value the caller can use, **or** your language is statement-only and the body uses print / assignment for results — **document which** | Expression-bodied vs explicit `return` |

A function value is at least "parameter names + body AST." Closures (body + defining environment) are **stretch**, aligned with Week 12. If you skip closures, say whether nested functions see only globals or also enclosing locals.

### Still required from Lab 3

Literals, variables, arithmetic, comparisons, unary ops, binding/assignment, print, and your documented binding rules.

---

## Environment (now used for real)

A call must push a scope and pop it (or restore the caller) when the call finishes.

| Operation | Role in Lab 4 |
| --- | --- |
| **define / assign** | Bind parameters in the callee's scope; your rules for outer names |
| **lookup** | Find a name in the current scope, then outward per your rule |
| **extend / push** | Enter the callee (and blocks, if you have block scope) |

Top-level / global scope for program-level definitions is fine. One flat map for the whole run, with parameters overwriting globals and never restoring them, is **not**.

---

## Print and errors

Same spirit as Lab 3:

- Print does whatever you documented (any arity, any layout)
- Runtime errors are clear messages, not host stack traces alone
- Division by zero and unbound names (if your rule is "error") still apply
- Wrong number of arguments on a call should be a clear runtime error

---

## Not required (stretch)

| Stretch | |
| --- | --- |
| Closures / first-class functions | Function values that capture their defining env; passing functions as arguments |
| Nested functions with static scoping | |
| Lists or other compound values | |
| Exceptions in your language | |
| Objects / methods | |

A solid `if` + loop-or-recursion + calls beats a half-finished stretch feature. Feature freeze is Week 13.

---

## Test inputs (required)

Same pair format as Labs 2–3:

| File | Contents |
| --- | --- |
| `name.input.txt` | Source in your language |
| `name.expected.txt` | Exact stdout (or your runtime error text) |

```text
tests/
├── 01_if_true.input.txt
├── 01_if_true.expected.txt
├── …
```

We are **not** running a hidden suite. We spot-check that your runs match your expected files.

### What your tests must cover

| Cover | Example idea |
| --- | --- |
| Lab 3 basics | At least one arithmetic + binding + print program still works |
| `if` then | condition true → then-branch runs |
| `if` else (if you have else) | condition false → else-branch runs |
| Loop **or** recursion | countdown / sum 1..n / factorial — clear output |
| Function with parameters | `f(a, b)` uses the arguments, not globals of the same name by accident |
| Nested or sequential calls | `f` calls `g`, or two separate calls |
| Local vs outer | a parameter or local does not permanently destroy an outer name of the same spelling (unless your documented rule says it does) |
| Arity error | too few or too many arguments → your runtime error |
| A short “demo” program | mixes binding, `if`, repetition, and at least one call — suitable to show in Week 14 |

Put the pairs under `tests/`. List in the README what each test is meant to prove.

---

## Sample programs for the demo

Include at least **two** readable programs (under `examples/` or `tests/`) you would run live in Week 14. One may be the demo test above. Prefer clarity over cleverness.

---

## Language / tooling

Any implementation language is fine.

Your repo **README must** include:

1. **Language name** and a one-paragraph summary
2. **Environment** — how to run on a clean machine
3. **How to run** a program file
4. **How to run tests** against the expected files
5. **Print** — arguments and layout
6. **Names and functions** — binding rule; what a call's environment can see; whether you have closures
7. **Control** — truthiness; loop vs recursion choice
8. **Runtime errors** — unbound name, division by zero, bad arity, …
9. **Changes since Lab 3**
10. **AI disclaimer**
11. Pointer to the **EBNF**
12. **Week 14 demo** — which example files you plan to run (can be tentative)

---

## AI disclaimer (required in README)

**AI / assistance:** whether you used AI tools, what for, and that you can explain every line you submit. Using AI is allowed. Submitting work you cannot explain is not.

---

## Suggested README outline

```text
# <YourLanguageName> Interpreter (Lab 4)

## Language name and summary
## EBNF (or link to GRAMMAR.md)
## Changes since Lab 3
## Names, scopes, and functions
## Control (if, loop or recursion)
## Print
## Runtime errors
## Run
## Tests
## Demo programs
## AI / assistance
```

---

## Design tips (optional)

- Evaluate conditions to your boolean (or truthy) rule once; both branches of `if` should not run.
- For `while`, rethink the condition each iteration; avoid infinite loops in tests.
- On a call: evaluate args in the **caller** env → make a child env → bind parameters → evaluate body → discard the child.
- If you support `return`, decide whether it exits only the function or also breaks out of nested blocks; document it.
- Recursion in *your* language is several interpreter frames per user call. Catch host `RecursionError` (or equivalent) and print a clear message, or document a small limit.
- Closures: store the environment that was current at `define` / `lambda` time; look up free variables there, not in the caller's env (static vs dynamic — Week 4 / Week 7).

---

## What to put in the repo

| Item | |
| --- | --- |
| Full pipeline source | lexer + parser + evaluator/interpreter |
| EBNF | README or separate file |
| README | as above |
| `tests/*.input.txt` / `*.expected.txt` | covering the Lab 4 list |
| Demo / example programs | at least two you can run live |
| Optional | test runner script |

---

## What to submit

1. Accept: https://classroom50.org/Seward-Classes/programming-languages/assignments/lab-4/accept
2. Push to the repo that creates
3. Submit that **repository link** as directed on classroom50 / LMS

---

## Grading

| Result | Score |
| --- | --- |
| Meets the spec | **100** |
| Each minor issue | **−10** |
| More than **3** issues | **Kicked back** for another attempt |

### To spec

- Lab 3-style programs still run
- `if` takes the correct branch
- Loop or recursion works and is demonstrated
- Function definitions and calls use a per-call scope for parameters
- Tests cover the required rows; output matches expected files
- README documents binding, calls, control, print, errors, changes, AI, and demo files

### Minor issues

- One edge case on truthiness or off-by-one in a loop
- Thin tests for calls or for `if`/`else`
- Expected file drift for one test
- Recursion limit undocumented when the host blows up on a moderate input

### Kick-back

- No working function calls, or calls share one global map with no restore
- `if` / loop / functions still print "not until Lab 4" or crash
- Default run is still AST-only
- Cannot run from README
- No AI disclaimer

---

## Academic honesty

You own your language and your code. Discuss ideas freely; do not copy another student's interpreter. Cite non-trivial external code in the README.

---

## Week 14 (reminder)

The final is a short presentation plus a **live demo** of this interpreter. Design trade-offs (syntax, scope, evaluation) should be ones you actually implemented. Freeze features in Week 13 so the demo is boring in the best way — it just works.
