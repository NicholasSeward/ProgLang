# Lab 1 — Lexer

**Course:** Programming Languages  
**Accept assignment:** [classroom50.org — Lab 1](https://classroom50.org/Seward-Classes/programming-languages/assignments/lab-1/accept)  
**Submit:** link to your Git repository (from the accept link above)

---

## Goal

**Name** your language (not “TinyLang” — pick something of your own), design its lexical rules, and implement a **lexer**:

```text
source text  →  stream of tokens
```

This is the first of four labs. Later labs will parse and evaluate **your** language, so invent something you are willing to live with.

```text
Lab 1 Lexer → Lab 2 Parser → Lab 3 Evaluator → Lab 4 Interpreter
```

You are **not** told a fixed grammar. You **are** required to cover certain token categories and to publish an **EBNF** for the lexical / early syntax shape of your language.

---

## Name your language

Give the language a real **name**. Use that name in:

- the README title  
- your EBNF / grammar file  
- sample program filenames or headers  

“My language” or “TinyLang” as the final name is not enough.

---

## What you must support (categories, not a dictated dialect)

Your lexer must recognize at least:

| Category | Requirement | Your call |
| --- | --- | --- |
| **Integers** | Whole numbers | Digits, optional leading `+`/`-` as part of the number **or** as separate operator tokens — document it |
| **Floats** | Fractional / scientific forms you define | e.g. `3.14`, `0.5`, `1e-3` — pick a rule and stick to it |
| **Strings** | Quoted string literals | Quote style, escapes (`\n`, `\"`, …) — your design |
| **Reserved words** | At least a small set of keywords | Which words (`if`, `fun`, `print`, …) — your design |
| **Identifiers** | Names that are not reserved | Allowed characters — your design |
| **Operators / punctuation** | Enough for expressions and structure | e.g. `+ - * / = ( )` and whatever else you need |
| **NEWLINE** | Emit a token **if** newlines matter in your language | Statement separators, significant line breaks, etc. |
| **INDENT / DEDENT** (or equivalent) | Emit tokens **if** indentation matters | Python-style layout — only if you choose that design |

Also decide and document:

- How you treat ordinary spaces/tabs (usually skip)
- Comments (recommended; style is up to you)
- What happens on illegal input (clear error)

**Do not** invent a language that omits ints, floats, strings, reserved words, ids, or operators.

---

## EBNF required

Your repo must include an **EBNF** (or BNF + EBNF extensions) that describes at least:

1. Lexical structure: how ints, floats, strings, ids, reserved words, and operators are formed  
2. Enough program / statement / expression shape that a reader can see what you intend for Labs 2–4  

Put it in the README or a separate `GRAMMAR.md` / `ebnf.txt`.

The EBNF is part of the **spec of your language**. The lexer must be consistent with it.

---

## Tokens

Each token must have at least:

| Field | Required? | Meaning |
| --- | --- | --- |
| **label** (type) | yes | Token kind (`ID`, `INT`, `FLOAT`, `STRING`, `PLUS`, … — names are yours) |
| **lexeme** | yes | Exact source text for that token |

**Line number** (and column) on each token is **optional**. Helpful for errors and later labs; not required for full credit if your error reporting is still clear.

Ending with an `EOF` token (or equivalent “end” marker) is recommended so Lab 2 is easier.

---

## Print output format (required)

Your runnable lexer must print **one token per line** to stdout, in order:

```text
LABEL lexeme
```

- `LABEL` — token type / kind  
- then a single space  
- then the `lexeme`  
- then a newline  

### Example

If the source is:

```text
x = 2
```

and your tokens are id `x`, assign `=`, int `2`, output might be:

```text
ID x
ASSIGN =
INT 2
```

(Use your own label names; they must match your EBNF / README.)

### Notes

- For `STRING` tokens, print the lexeme as you define it (usually including quotes).  
- For `EOF`, `NEWLINE`, `INDENT`, or `DEDENT`, print the label and whatever lexeme you documented (empty lexeme is fine if stated in the README).  
- Do **not** print only lexemes, JSON, or multi-field dumps unless you *also* provide this `LABEL lexeme` stream as the default run output.

---

## Test inputs (required) — we will not run a hidden test suite

We are **not** grading you with our own automated tests.

You **must** provide a **good set of test input files** (and show expected output or how to produce it) that together exercise **all parts of your spec**, including at least:

| Cover | Example ideas |
| --- | --- |
| Integers | several forms you allow |
| Floats | forms you allow; edge cases you care about |
| Strings | normal, escapes, and a failing/unclosed case if you document errors |
| Reserved vs id | `if` vs `iffy` (or your equivalents) |
| Operators | each operator / punctuation you listed |
| NEWLINE / INDENT | only if your language uses them — show they appear correctly |
| Comments / whitespace | skipped as documented |
| A short “real” program | mixes categories in one file |
| Illegal input | at least one file that should error |

Put inputs under something like `tests/` or `examples/`. In the README, list what each file is meant to prove.

---

## Language / tooling

Any language is fine.

Your repo **README must** include:

1. **Language name**  
2. **Environment** — OS notes, language version, packages  
3. **How to run** the lexer on a file  
4. **How to run / view your test inputs**  
5. **AI disclaimer** (see below)  
6. Pointer to your **EBNF**

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
# <YourLanguageName> Lexer (Lab 1)

## Language name and summary
## EBNF (or link to GRAMMAR.md)
## Environment
## Run
## Test inputs (what each file covers)
## Output format (LABEL lexeme; EOF / NEWLINE / INDENT notes)
## AI / assistance
```

---

## Design tips (optional)

- Longest match for operators (`==` vs `=`) if you have multi-character ops  
- After reading a word, check a reserved-word table before emitting `ID`  
- Floats need careful rules so `1.2.3` and `1.` fail or parse the way your EBNF says  
- Strings need a clear policy on escapes and unclosed quotes  
- If indentation matters, study how Python tokenizes `INDENT`/`DEDENT`; if not, skip those tokens entirely  

---

## What to put in the repo

| Item | |
| --- | --- |
| Lexer source | your implementation |
| EBNF | README or separate file |
| README | name, environment, run, test inputs, AI disclaimer |
| Test inputs | files that cover the whole spec |
| Optional | expected output files next to each input |

---

## What to submit

1. Accept: https://classroom50.org/Seward-Classes/programming-languages/assignments/lab-1/accept  
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

- Language has a **name**  
- Required token categories present and demonstrated in your test inputs  
- EBNF present and consistent with the lexer  
- CLI prints `LABEL lexeme` one token per line  
- README has environment, run, test-input guide, and AI disclaimer  
- Reserved words are not emitted as plain identifiers  

### Examples of minor issues

- Thin test set that misses an edge of your own EBNF  
- README incomplete but project still runnable  
- Label names drift slightly from the EBNF write-up  

### Kick-back territory (more than three issues, or a major miss)

- No language name  
- Missing a required category (no strings, no floats, …)  
- No EBNF  
- Wrong output format  
- No meaningful test inputs covering the spec  
- Cannot run from README  
- No AI disclaimer  

---

## Academic honesty

You own your language design and your code. Discuss ideas freely; do not copy another student’s lexer. Cite non-trivial external code in the README.
