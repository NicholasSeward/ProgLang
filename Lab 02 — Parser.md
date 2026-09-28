# Lab 2 — Parser

**Course:** Programming Languages  
**Accept assignment:** [classroom50.org — Lab 2](https://classroom50.org/Seward-Classes/programming-languages/assignments/lab-2/accept)  
**Submit:** link to your Git repository (from the accept link above)

---

## Goal

Extend **your** language from Lab 1 with a **parser** that turns tokens into an **abstract syntax tree (AST)**:

```text
source text  →  tokens  →  AST
```

```text
Lab 1 Lexer → Lab 2 Parser → Lab 3 Evaluator → Lab 4 Interpreter
```

You keep your language name and design. You are **not** told a fixed grammar. You **are** required to parse certain kinds of constructs, to publish a **complete EBNF**, and to print your AST in the format below.

Nothing is evaluated yet. `print 2 + 3` should produce a tree, not `5`.

---

## Start from Lab 1

Copy your Lab 1 lexer into the new repo and build on it.

- If Lab 1 had issues or was kicked back, fix them here. The parser is only as good as the tokens it gets.
- You may change your language (new keywords, operators, token labels). If you do, update the EBNF and list the changes in the README under **Changes since Lab 1**.

---

## What you must parse (categories, not a dictated dialect)

Your parser must handle at least:

### Expressions

| Category | Requirement | Your call |
| --- | --- | --- |
| **Literals** | Integers, floats, and strings | Same forms as your Lab 1 lexer |
| **Identifiers** | Variable references | — |
| **Arithmetic** | At least two precedence levels (e.g. `+ -` below `* /`) | Which operators, which levels |
| **Associativity** | Left-associative for `-` and `/` (or your equivalents) | Right-assoc operators (e.g. `^`) are optional — document them |
| **Parentheses** | Override precedence | — |
| **Comparison** | At least `<` and `==` (or your equivalents) | Full set is up to you |
| **Unary operator** | At least one (e.g. `-x` or `not x`) | Which ones, and their precedence |
| **Function call** | Calling a named function with arguments | Syntax is yours (`f(a, b)`, `f a b`, `call f with a`, …) |

### Statements

| Category | Requirement | Your call |
| --- | --- | --- |
| **Binding / assignment** | Give a name a value | `x = 1`, `let x = 1`, `x <- 1`, … |
| **Output** | A print-style statement | Keyword or built-in call |
| **Conditional** | `if` with an optional `else` branch | Syntax, and how `else` is attached |
| **Loop** | At least one loop form | `while`, `for`, `repeat`, … |
| **Block** | A sequence of statements as a body | Braces, `end`, indentation, … |
| **Function definition** | Named function with parameters and a body | Syntax is yours; **parsed now, evaluated in Lab 4** |
| **Program** | A sequence of statements | Separators: newlines, `;`, … |

Also decide and document:

- How statements are separated (and whether blank lines / trailing separators are allowed)
- How blocks end
- How the dangling `else` is resolved, if your syntax allows it
- Anything in your language that is **not** required above but that you chose to add

---

## EBNF required (complete)

Your repo must include a **complete EBNF** for the language:

1. Lexical rules (carried over from Lab 1, updated as needed)
2. Program, statement, and expression rules for everything your parser accepts

Requirements for the grammar:

- **Unambiguous.** Precedence and associativity must be visible in the rules (layered nonterminals, as in lecture), not just described in prose.
- **Consistent with the parser.** If a program is valid under the EBNF, your parser accepts it. If the parser accepts it, the EBNF allows it.
- **Parseable by your technique.** If you write recursive descent, remove left recursion (use `{ … }` repetition) and resolve common prefixes (lookahead or left factoring).

Put it in the README or a separate `GRAMMAR.md` / `ebnf.txt`.

---

## The AST

Build an **abstract** syntax tree, not a concrete parse tree.

| Keep | Drop |
| --- | --- |
| Operators, names, literal values | Parentheses |
| Structure (which operand goes where) | Separators (`;`, commas, newlines) |
| Statement kinds and their parts | Keywords that only mark structure (`then`, `do`, `end`) |

`(2 + 3) * 4` and `((2 + 3)) * 4` must produce the **same** AST.

Node kinds and names are yours (`BinOp`, `Add`, `Plus`, …). Pick a consistent set and document it.

---

## Print output format (required)

Running your parser on a file must print the AST to stdout: **one node per line**.

### Line format

```text
KIND
KIND value
```

- `KIND` — the node kind
- optionally a single space and a `value` — operator, name, or literal lexeme
- children appear on the lines below their parent, in order, indented one level deeper

### Indentation

Use one of these and say which in the README:

- **Box-drawing** (recommended, as in Week 3 Part 3): `├── `, `└── `, `│   `
- **Plain**: two spaces per level

### Example

Given this source in some hypothetical language:

```text
x = 2 + 3 * 4
print x - 1 - 1
```

output might be:

```text
Program
├── Assign x
│   └── BinOp +
│       ├── Int 2
│       └── BinOp *
│           ├── Int 3
│           └── Int 4
└── Print
    └── BinOp -
        ├── BinOp -
        │   ├── Var x
        │   └── Int 1
        └── Int 1
```

Notice:

- `*` sits below `+` — precedence
- `(x - 1) - 1` — left associativity
- No parentheses, `=` token, or newline nodes appear

### Syntax errors

On invalid input, print a clear error instead of a tree and stop. The error must say:

- **what was expected**
- **what was found** (the offending token)
- **where** — line number if your tokens carry one, otherwise the token position

```text
Syntax error at line 3: expected ')' but found NEWLINE
```

Stopping at the first error is fine. Error recovery is not required.

### Lexer mode (optional)

Keeping a flag that prints your Lab 1 `LABEL lexeme` stream (e.g. `--tokens`) is helpful for debugging but not required. The **default** run must print the AST.

---

## Implementation rules

- Write the parser yourself. **No parser generators or parsing libraries** (ANTLR, yacc/bison, PLY, Lark, parsimonious, …).
- Any technique is fine. **Recursive descent** is recommended and matches lecture.
- Your lexer from Lab 1 must still be your own.

---

## Test inputs (required) — we will not run a hidden test suite

We are **not** grading you with our own automated tests.

You **must** provide a **good set of tests** that together exercise **all parts of your spec**.

### Test files: input + expected output

Every test is a **pair of plain text files**:

| File | Contents |
| --- | --- |
| `name.input.txt` | Source code in your language |
| `name.expected.txt` | Exactly what your parser prints for that input (the AST, or the syntax error message) |

```text
tests/
├── 01_precedence.input.txt
├── 01_precedence.expected.txt
├── 02_left_assoc.input.txt
├── 02_left_assoc.expected.txt
├── ...
├── 20_missing_paren.input.txt
└── 20_missing_paren.expected.txt
```

- Every input file has a matching expected file, and every expected file has a matching input file.
- Error tests follow the same rule: the expected file holds the error message.
- Running your parser on `name.input.txt` must print exactly what is in `name.expected.txt`. We will spot-check this.
- Write the expected output by working out the tree first. Pasting whatever your parser prints just copies its bugs into the expected file.

A script that runs every pair and reports pass/fail is nice but not required.

### What your tests must cover

At least:

| Cover | Example ideas |
| --- | --- |
| Every expression category | literals, ids, calls, unary, comparison |
| Precedence | `2 + 3 * 4` puts `*` below `+` |
| Associativity | `10 - 3 - 2` groups to the left |
| Parentheses | `(2 + 3) * 4` overrides precedence; redundant parens give the same tree |
| Every statement category | binding, print, if, if/else, loop, block, function definition |
| Nesting | loop inside if, if inside a function body, calls as arguments |
| Dangling `else` | if your syntax allows it — show which `if` it attaches to |
| A short “real” program | mixes everything in one file |
| Syntax errors | several files: missing `)`, missing operand, bad statement start, unterminated block, … |

Put the pairs under `tests/`. In the README, list what each test is meant to prove.

---

## Language / tooling

Any language is fine.

Your repo **README must** include:

1. **Language name**
2. **Environment** — OS notes, language version, packages
3. **How to run** the parser on a file
4. **How to run your tests** and compare the output with the expected files
5. **AST format notes** — node kinds, indentation style
6. **Changes since Lab 1** (or “none”)
7. **AI disclaimer** (see below)
8. Pointer to your **EBNF**

If you use Python with only the standard library, the environment section can be short.  
If you use another language or any extra libraries, setup must be clear enough for a clean machine.

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
# <YourLanguageName> Parser (Lab 2)

## Language name and summary
## EBNF (or link to GRAMMAR.md)
## Changes since Lab 1
## Environment
## Run
## Tests (how to run, what each pair covers)
## AST format (node kinds, indentation style)
## AI / assistance
```

---

## Design tips (optional)

- One function per nonterminal. Precedence falls out of which function calls which.
- `{ op term }` in the EBNF becomes a `while` loop that folds into `left`. That keeps left associativity without left recursion.
- Use lookahead (`peek`) to choose a statement kind before consuming anything.
- A small `expect(label)` helper that raises a good error message will pay for itself.
- Unary operators usually sit between `term` and `factor` in the precedence ladder.
- If newlines are tokens, decide where they are allowed (inside parentheses? after an operator?) and put that in the EBNF.
- If indentation matters, treat `INDENT` / `DEDENT` like `{` / `}` in your block rule.
- Write the AST printer early. You will use it to debug everything else.

---

## What to put in the repo

| Item | |
| --- | --- |
| Lexer + parser source | your implementation |
| EBNF | README or separate file |
| README | name, environment, run, tests, AST format, changes, AI disclaimer |
| `tests/*.input.txt` | valid and invalid programs covering the whole spec |
| `tests/*.expected.txt` | exact expected output for each input |
| Optional | a script that runs every pair and reports pass/fail |

---

## What to submit

1. Accept: https://classroom50.org/Seward-Classes/programming-languages/assignments/lab-2/accept
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

- Every required expression and statement category parses and is demonstrated in your tests
- Every test has an input file and an expected-output file, and the parser's output matches
- Precedence and associativity are correct in the printed trees
- Output is an **AST** (no parentheses or separator nodes) in the required line format
- Syntax errors report expected, found, and where
- EBNF is complete, unambiguous, and matches the parser
- README has environment, run, test guide, AST notes, changes, and AI disclaimer

### Examples of minor issues

- One edge case in your own grammar that the parser gets wrong
- Error message missing the location for one kind of error
- Thin test set for one category
- A test missing its expected file, or an expected file that doesn't match the actual output
- EBNF drifts slightly from what the parser actually accepts

### Kick-back territory (more than three issues, or a major miss)

- A required category missing (no loops, no function definitions, …)
- Wrong precedence or associativity
- Prints a concrete parse tree or a raw data dump instead of the AST format
- Crashes or prints nothing useful on invalid input
- Used a parser generator or parsing library
- No EBNF, or EBNF that does not describe the language
- Cannot run from README
- No AI disclaimer

---

## Academic honesty

You own your language design and your code. Discuss ideas freely; do not copy another student’s parser. Cite non-trivial external code in the README.
