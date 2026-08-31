# Programming Languages — 14-Week Course Plan

**Textbook:** Sebesta, *Concepts of Programming Languages*, 12th ed.  
**Implementation note:** Class examples and starter code (when provided) are in **Python, Java, or C++**. Students may implement labs in any language they prefer. The mini interpreter is the through-line for all four labs.

**Final assessment:** Presentation + live demo of the mini interpreter (no traditional final exam).  
**Midterm:** One off week (no new topics / no new reading load beyond optional review).

---

## Course learning objectives (ACM-aligned)

Adapted from ACM/IEEE-CS **Computer Science Curricula 2023** knowledge area **Foundations of Programming Languages (FPL)** (formerly **PL** in CS2013). ACM outcomes are *illustrative*, not mandated verbatim—below are course-level objectives suited to this syllabus (Sebesta + mini interpreter). Primary FPL units covered: **Translation**, **Abstraction/Representation**, **Syntax** (elective depth via labs), **Types**, **Systems/Runtime**, **OOP**, **Functional**, **Logic** (survey), **Design**.

Upon successful completion of this course, students will be able to:

1. **Evaluate language designs** using principles such as readability, writability, reliability, cost, and orthogonality, and justify design trade-offs across languages. *(FPL-Design; Sebesta Ch. 1)*

2. **Distinguish** language definition (syntax and semantics) from a particular implementation (compiler, interpreter, JIT), and **differentiate** syntax/parsing from semantics/evaluation. *(FPL-Translation)*

3. **Specify syntax** with BNF/EBNF, identify ambiguity/precedence/associativity issues, and illustrate parse trees for simple constructs. *(FPL-Translation, FPL-Syntax)*

4. **Explain and illustrate** names, binding, scope (static vs dynamic), lifetime, side effects, and common parameter-passing methods. *(FPL-Abstraction)*

5. **Describe** type-system concepts (static vs dynamic checking, strong typing, type equivalence) and explain how types constrain legal operations and catch errors. *(FPL-Types)*

6. **Explain** run-time organization for subprograms (activation records, stack/heap) and relate it to recursion, environments, and language features. *(FPL-Systems, FPL-Translation)*

7. **Compare** imperative, object-oriented, functional, and logic approaches to expressing computation (including inheritance/dynamic dispatch and FP ideas such as immutability and higher-order functions). *(FPL-OOP, FPL-Functional, FPL-Logic)*

8. **Implement a program that processes programs**: build a mini language pipeline (lexer → parser/AST → evaluator/interpreter) and demonstrate its behavior. *(FPL-Abstraction LO: interpreter / program-as-data; Labs 1–4 + final demo)*

**Source:** ACM/IEEE-CS *Computer Science Curricula 2023* (CS2023), Knowledge Area FPL — [csed.acm.org](https://csed.acm.org/). Cross-walk also consistent with CS2013 PL units (Program Representation, Language Translation and Execution, Syntax Analysis, Basic Type Systems, OOP, Functional Programming, Logic Programming, Language Pragmatics/Design).

---

## Syllabus excerpt — weekly topics and projects

**Textbook:** Sebesta, *Concepts of Programming Languages*, 12th ed.  
Labs build one **mini interpreter** (source → tokens → AST → evaluate). Starter code, when provided, is Python, Java, or C++; students may use another language if they document how to run it.

| Week | Topics | Project / assessment |
| --- | --- | --- |
| 1 | Why study programming languages; evaluation criteria; paradigms; design trade-offs; implementation methods | Mini-interpreter project arc introduced |
| 2 | Syntax: BNF/EBNF, parse trees, ambiguity, precedence and associativity | TinyLang syntax published (Lab 1–4 target) |
| 3 | Semantics overview; lexical analysis; parsing and recursive descent | **Lab 1 due — Lexer** (source → tokens) |
| 4 | Names, bindings, scope, and lifetime | — |
| 5 | Data types; type checking and type equivalence | **Lab 2 due — Parser** (tokens → AST) |
| 6 | Expressions, assignment, and control structures | Midterm review materials released |
| 7 | Midterm (no new topics) | **Midterm exam** (Weeks 1–6) |
| 8 | Subprograms: parameter passing, overloading, generics, closures | — |
| 9 | Implementing subprograms: activation records and environments | **Lab 3 due — Evaluator** (AST → values + environments) |
| 10 | Abstract data types and encapsulation | Lab 4 kickoff (functions + control) |
| 11 | Object-oriented programming; exception handling; concurrency survey | Lab 4 continues |
| 12 | Functional programming | Lab 4 continues |
| 13 | Logic programming (survey); course synthesis | **Lab 4 due — Mini interpreter** (control + functions); feature freeze |
| 14 | Final presentations and demos | **Final:** presentation + live demo of mini interpreter |

---

## Course arc

| Block | Weeks | Focus |
| --- | --- | --- |
| Foundations | 1–3 | Criteria, syntax/semantics, lexing & parsing ideas |
| Core constructs | 4–6 | Names, types, expressions, control |
| Midterm | 7 | Review + exam (off week) |
| Subprograms & runtime | 8–9 | Calls, activation records, environments |
| Abstraction & OOP | 10–11 | ADTs, OOP, exceptions (concurrency survey) |
| Paradigms + finish interpreter | 12–13 | FP (and light logic); polish interpreter |
| Final | 14 | Presentations & demos |

---

## Assessment (high level)

| Component | Role |
| --- | --- |
| Problem sets / short homework | Design issues, tracing, grammars |
| Labs 1–4 | Build a mini language end-to-end |
| Midterm (Week 7) | Written exam on Weeks 1–6 (+ Ch. 1, 3–8) |
| Final (Week 14) | Mini-interpreter presentation + demo |

---

## Mini-interpreter target (what “done” looks like)

Students implement a **small language** (name it as you like; e.g. *TinyLang*) with roughly:

- Numbers, booleans, strings (optional)
- Variables / `let` (or assignment) with an **environment**
- Arithmetic / relational / boolean expressions
- `if` and a simple loop or recursion
- **User-defined functions** and calls
- Clear pipeline: **source → tokens → AST → evaluate**

Stretch (optional, for stronger students): closures, lists, or a tiny standard library.

**Deliverable for Week 14:** working interpreter + short slide deck (design choices tied to Sebesta criteria) + live demo of sample programs.

---

## Four labs (pipeline)

| Lab | Due ~ | Builds | Ties to book |
| --- | --- | --- | --- |
| **Lab 1 — Lexer** | End of Week 3 | Source string → token stream | Ch. 4 lexical analysis; Ch. 3 token/lexeme ideas |
| **Lab 2 — Parser / AST** | End of Week 5 | Tokens → AST | Ch. 3 grammars; Ch. 4 recursive-descent |
| **Lab 3 — Expression evaluator** | End of Week 9 | AST → values; environments for names | Ch. 5 binding/scope; Ch. 7 expressions; Ch. 10 AR / env intuition |
| **Lab 4 — Full mini interpreter** | End of Week 13 | Control + functions on top of Lab 3 | Ch. 8 control; Ch. 9–10 subprograms |

Starter code, when given, will be **Python, Java, or C++**. Students may port or rewrite in another language if they document how to run it.

---

## Week-by-week topics and reading

### Week 1 — Preliminaries

| | |
| --- | --- |
| **Topics** | Why study PLs; evaluation criteria; domains; paradigms; design trade-offs; implementation methods (compiled / interpreted / hybrid) |
| **Reading** | Ch. 1 (all) |
| **Optional** | Skim Ch. 2 for historical flavor (not examined in depth) |
| **Work** | Short homework: evaluate a language you know against Ch. 1 criteria; announce Lab 1–4 / mini-interpreter arc |

---

### Week 2 — Describing syntax

| | |
| --- | --- |
| **Topics** | Lexemes vs tokens; BNF/EBNF; parse trees; ambiguity; precedence/associativity via grammar design |
| **Reading** | Section 3.1-3.3 |
| **Work** | Grammar problem set; publish **TinyLang** draft syntax (even informal) so Lab 1 has a target |

---

### Week 3 — Semantics intro + lexical / syntax analysis

| | |
| --- | --- |
| **Topics** | Attribute grammars (light); dynamic semantics survey (operational / denotational / axiomatic — overview only); lexical analysis; parsing problem; recursive-descent idea |
| **Reading** | Section 3.4-3.5 (can skim proofs/detail); Section 4.1-4.4 |
| **Work** | **Lab 1 due:** lexer for TinyLang (keywords, ids, numbers, operators, punctuation; skip whitespace/comments) |

---

### Week 4 — Names, bindings, scopes

| | |
| --- | --- |
| **Topics** | Names; variables; binding & binding times; static vs dynamic scope; scope vs lifetime; referencing environments; named constants |
| **Reading** | Ch. 5 (all) |
| **Work** | Scope/binding tracing homework (“what does this print?” in 2 languages) |

---

### Week 5 — Data types

| | |
| --- | --- |
| **Topics** | Primitive types; arrays/records/tuples/lists; unions; pointers vs references; optional types; type checking, strong typing, type equivalence |
| **Reading** | Section 6.1-6.5, 6.10-6.15 (skim the rest as needed) |
| **Work** | **Lab 2 due:** recursive-descent (or equivalent) parser → AST; reject bad programs with useful errors |

---

### Week 6 — Expressions, assignment, control

| | |
| --- | --- |
| **Topics** | Operator overloading & coercions; short-circuit evaluation; assignment; selection & iteration; unconditional branching; guarded commands (brief) |
| **Reading** | Ch. 7; Ch. 8 |
| **Work** | Design-issue homework; midterm review sheet released; Lab 3 preview (evaluator + environments) |

---

### Week 7 — Midterm off week

| | |
| --- | --- |
| **Topics** | No new lecture topics |
| **Reading** | Review Ch. 1, 3–8 (and Lab 1–2 ideas: tokens, grammars, AST) as needed |
| **Work** | **Midterm exam**; office hours / review session only |

---

### Week 8 — Subprograms

| | |
| --- | --- |
| **Topics** | Subprogram design issues; parameter-passing methods; subprograms as parameters; overloading & generics (survey); closures; coroutines (brief) |
| **Reading** | Ch. 9 |
| **Work** | Parameter-passing tracing homework; connect “environment / activation” ideas to Lab 3 |

---

### Week 9 — Implementing subprograms

| | |
| --- | --- |
| **Topics** | Call/return semantics; stack-dynamic locals & activation records; nested subprograms; blocks; implementing dynamic scoping |
| **Reading** | Ch. 10 |
| **Work** | **Lab 3 due:** evaluate expressions + variable bindings (environment / symbol table); `if` optional here or pushed to Lab 4 |

---

### Week 10 — ADTs and encapsulation

| | |
| --- | --- |
| **Topics** | Abstraction; ADT design issues; language examples; parameterized ADTs; encapsulation & naming |
| **Reading** | Ch. 11 |
| **Work** | Short problem set; Lab 4 kickoff (functions + control + polish) |

---

### Week 11 — OOP + exceptions (concurrency survey)

| | |
| --- | --- |
| **Topics** | OOP design issues; inheritance & dynamic binding; implementation sketch (vtables idea); reflection (brief); exception handling models; concurrency as **one lecture survey** (threads / monitors / message passing — pick 1–2 examples) |
| **Reading** | Section 12.1-12.5; Section 14.1-14.4; Section 13.1-13.5 *(survey — not deep)* |
| **Work** | Dispatch / exception tracing worksheet; continue Lab 4 |

---

### Week 12 — Functional programming

| | |
| --- | --- |
| **Topics** | Math functions & FP fundamentals; Scheme overview; FP features in imperative languages; compare FP vs imperative — **tie to interpreter design** (immutable env, expression-oriented language, first-class functions) |
| **Reading** | Section 15.1-15.5, 15.10-15.11 (skim ML/Haskell/F# as interest) |
| **Work** | Optional small Scheme/Python-FP exercises; Lab 4 in progress (functions required) |

---

### Week 13 — Logic programming (light) + interpreter freeze

| | |
| --- | --- |
| **Topics** | Predicate calculus & logic programming overview; Prolog basics; deficiencies & applications (survey); course synthesis using Ch. 1 criteria |
| **Reading** | Section 16.1-16.6 (skim 16.7-16.8) |
| **Work** | **Lab 4 due:** complete mini interpreter (control + functions); prepare presentation & demo script; freeze features |

---

### Week 14 — Final presentations & demos

| | |
| --- | --- |
| **Topics** | Student presentations (no new book content) |
| **Reading** | None |
| **Work** | **Final:** ~8–12 min presentation + live demo of TinyLang programs; Q&A on design trade-offs (syntax, scope, types, evaluation) |

**Presentation expectations (suggested):** language overview → grammar highlights → architecture (lexer/parser/eval) → demo → one design trade-off analyzed with Sebesta criteria → limitations / future work.

---

## Suggested problem-set cadence (optional)

| After week | Focus |
| --- | --- |
| 1 | Evaluation criteria |
| 2–3 | BNF / ambiguity / parse trees |
| 4 | Scope & binding tracing |
| 6 | Expressions & control design issues |
| 8 | Parameter passing |
| 11 | OOP dispatch / exceptions |

---

## Lab details (instructor checklist)

### Lab 1 — Lexer
- Input: source file or string  
- Output: list/stream of tokens `(kind, lexeme, location)`  
- Must handle: identifiers, keywords, numbers, operators, delimiters  
- Starter option: Python skeleton with a `Token` type and failing tests  

### Lab 2 — Parser → AST
- Input: tokens from Lab 1  
- Output: typed/untyped AST nodes (e.g. `BinOp`, `Literal`, `Var`, `Let`, `If`, `Call`)  
- Grammar: provide a frozen EBNF so everyone targets the same language  
- Errors: report line/column on syntax failure  

### Lab 3 — Evaluator (expressions + environment)
- Interpret AST for expressions and name bindings  
- Environment API: `extend`, `lookup` (prep for nested scopes / functions)  
- Tests: arithmetic, nested lets, shadowing  

### Lab 4 — Mini interpreter
- Add: `if`, at least one of `while` / recursion, function definitions & calls  
- Sample programs + README (how to run; language choice if not Python)  
- Becomes the Week 14 demo artifact  

---

## Trim / stretch levers

| If behind | Cut |
| --- | --- |
| Time | Ch. 13 almost entirely; Ch. 16 to one lecture; Ch. 4 bottom-up parsing |
| Lab load | Make Lab 3 include only expressions + lets; push `if` fully into Lab 4 |
| Stronger students | Closures, lists, type checker stub, or bytecode VM stretch |

---

## Quick reference — reading map

| Week | Primary reading |
| --- | --- |
| 1 | Ch. 1 |
| 2 | Ch. 3.1–3.3 |
| 3 | Ch. 3.4–3.5, Ch. 4.1–4.4 |
| 4 | Ch. 5 |
| 5 | Ch. 6 (selected) |
| 6 | Ch. 7–8 |
| 7 | Review only |
| 8 | Ch. 9 |
| 9 | Ch. 10 |
| 10 | Ch. 11 |
| 11 | Ch. 12, 14 (exc.), 13 (survey) |
| 12 | Ch. 15 (selected) |
| 13 | Ch. 16 (selected) |
| 14 | — |
