# Concepts of Programming Languages (Sebesta, 12th Edition)

**Class planning outline** — chapters, sections, and teaching notes.

## Course goals (from the book)

1. Introduce the fundamental constructs of contemporary programming languages.
2. Give students tools for critical evaluation of existing and future languages.
3. Prepare students for compiler design (syntax description, lexical/syntax analysis).

**Approach:** Design issues for language constructs → choices in common languages → compare alternatives.

**12th edition notes:** Lua and Objective-C largely removed; Swift added in several chapters; optional types added in Ch. 6; Python iterators expanded in §8.3.4.

---

## Instructor guidance (sequencing)

| Chapters | Notes |
| --- | --- |
| **1, 3** | Cover in detail |
| **2** | Interesting reading; little lecture time; **skippable** (nothing later depends on it) |
| **4** | Skip if a compiler course is required; depends on Ch. 3; otherwise standalone |
| **5–9** | Easier for students with C++/Java/C# experience |
| **10–14** | Harder; need more detailed lectures |
| **15–16** | New to most juniors; ideally provide Scheme and Prolog processors; undergrad courses may not finish both; grad courses can cover fully by trimming early imperative chapters |

**Core construct sequence (Ch. 5–14):** variables → data types → expressions/assignment → control → subprograms (+ implementation) → ADTs → OOP → concurrency → exceptions/events.

**Paradigms (Ch. 15–16):** functional and logic programming (some FP ideas also appear in Ch. 6 and 8).

---

## Chapter 1 — Preliminaries

**Focus:** Why study languages; evaluation criteria; design influences and trade-offs; implementation approaches.

| Section | Topic |
| --- | --- |
| 1.1 | Reasons for Studying Concepts of Programming Languages |
| 1.2 | Programming Domains |
| 1.3 | Language Evaluation Criteria |
| 1.4 | Influences on Language Design |
| 1.5 | Language Categories |
| 1.6 | Language Design Trade-Offs |
| 1.7 | Implementation Methods |
| 1.8 | Programming Environments |

**Planning notes:** Typically covered thoroughly. Sets vocabulary for the rest of the course (readability, writability, reliability, cost, etc.).

---

## Chapter 2 — Evolution of the Major Programming Languages

**Focus:** History and contributions of major languages (not full language tutorials). Optional / independent reading.

| Section | Topic |
| --- | --- |
| 2.1 | Zuse’s Plankalkül |
| 2.2 | Pseudocodes |
| 2.3 | The IBM 704 and Fortran |
| 2.4 | Functional Programming: Lisp |
| 2.5 | The First Step Toward Sophistication: ALGOL 60 |
| 2.6 | Computerizing Business Records: COBOL |
| 2.7 | The Beginnings of Timesharing: Basic |
| 2.8 | Everything for Everybody: PL/I |
| 2.9 | Two Early Dynamic Languages: APL and SNOBOL |
| 2.10 | The Beginnings of Data Abstraction: SIMULA 67 |
| 2.11 | Orthogonal Design: ALGOL 68 |
| 2.12 | Some Early Descendants of the ALGOLs |
| 2.13 | Programming Based on Logic: Prolog |
| 2.14 | History’s Largest Design Effort: Ada |
| 2.15 | Object-Oriented Programming: Smalltalk |
| 2.16 | Combining Imperative and Object-Oriented Features: C++ |
| 2.17 | An Imperative-Based Object-Oriented Language: Java |
| 2.18 | Scripting Languages |
| 2.19 | The Flagship .NET Language: C# |
| 2.20 | Markup-Programming Hybrid Languages |

**Planning notes:** Good motivation and context; assign as reading rather than lecture if time is tight. Can be skipped entirely.

---

## Chapter 3 — Describing Syntax and Semantics

**Focus:** BNF; attribute grammars (syntax + static semantics); intro to operational, denotational, and axiomatic semantics.

| Section | Topic |
| --- | --- |
| 3.1 | Introduction |
| 3.2 | The General Problem of Describing Syntax |
| 3.3 | Formal Methods of Describing Syntax |
| 3.4 | Attribute Grammars |
| 3.5 | Describing the Meanings of Programs: Dynamic Semantics |

**Planning notes:** Cover in detail. Prerequisite for Ch. 4. Core “formal methods” chapter for the course.

---

## Chapter 4 — Lexical and Syntax Analysis

**Focus:** Lexers; parsing problem; recursive-descent; bottom-up parsing. Aimed at curricula without a required compilers course.

| Section | Topic |
| --- | --- |
| 4.1 | Introduction |
| 4.2 | Lexical Analysis |
| 4.3 | The Parsing Problem |
| 4.4 | Recursive-Descent Parsing |
| 4.5 | Bottom-Up Parsing |

**Planning notes:** Depends on Ch. 3. Standalone otherwise. Skip if students take a dedicated compiler course.

---

## Chapter 5 — Names, Bindings, and Scopes

**Focus:** Names, variables, binding, scope, lifetime, referencing environments, named constants.

| Section | Topic |
| --- | --- |
| 5.1 | Introduction |
| 5.2 | Names |
| 5.3 | Variables |
| 5.4 | The Concept of Binding |
| 5.5 | Scope |
| 5.6 | Scope and Lifetime |
| 5.7 | Referencing Environments |
| 5.8 | Named Constants |

**Planning notes:** Start of the core “constructs” block (Ch. 5–14). Usually manageable for students with imperative OOP background.

---

## Chapter 6 — Data Types

**Focus:** Primitive types through pointers/references; optional types; type checking, strong typing, type equivalence.

| Section | Topic |
| --- | --- |
| 6.1 | Introduction |
| 6.2 | Primitive Data Types |
| 6.3 | Character String Types |
| 6.4 | Enumeration Types |
| 6.5 | Array Types |
| 6.6 | Associative Arrays |
| 6.7 | Record Types |
| 6.8 | Tuple Types |
| 6.9 | List Types |
| 6.10 | Union Types |
| 6.11 | Pointer and Reference Types |
| 6.12 | Optional Types *(new in 12e)* |
| 6.13 | Type Checking |
| 6.14 | Strong Typing |
| 6.15 | Type Equivalence |
| 6.16 | Theory and Data Types |

**Planning notes:** Some functional data structures appear here (lists, etc.) before Ch. 15. §6.12 is a 12e addition—worth calling out when comparing modern languages (e.g. Swift/optional patterns).

---

## Chapter 7 — Expressions and Assignment Statements

**Focus:** Arithmetic expressions, overloading, conversions, relational/boolean expressions, short-circuit evaluation, assignment.

| Section | Topic |
| --- | --- |
| 7.1 | Introduction |
| 7.2 | Arithmetic Expressions |
| 7.3 | Overloaded Operators |
| 7.4 | Type Conversions |
| 7.5 | Relational and Boolean Expressions |
| 7.6 | Short-Circuit Evaluation |
| 7.7 | Assignment Statements |
| 7.8 | Mixed-Mode Assignment |

**Planning notes:** Relatively straightforward for experienced programmers; good place for design-trade-off discussions (side effects, coercion, etc.).

---

## Chapter 8 — Statement-Level Control Structures

**Focus:** Selection, iteration (incl. Python iterators), unconditional branching, guarded commands.

| Section | Topic |
| --- | --- |
| 8.1 | Introduction |
| 8.2 | Selection Statements |
| 8.3 | Iterative Statements *(§8.3.4 expanded for Python iterators in 12e)* |
| 8.4 | Unconditional Branching |
| 8.5 | Guarded Commands |
| 8.6 | Conclusions |

**Planning notes:** Also touches control ideas used later in functional languages. Emphasize design issues (e.g. goto debate, multiple loop exits).

---

## Chapter 9 — Subprograms

**Focus:** Subprogram design issues; parameter passing; indirect calls; overloading; generics; closures; coroutines.

| Section | Topic |
| --- | --- |
| 9.1 | Introduction |
| 9.2 | Fundamentals of Subprograms |
| 9.3 | Design Issues for Subprograms |
| 9.4 | Local Referencing Environments |
| 9.5 | Parameter-Passing Methods |
| 9.6 | Parameters That Are Subprograms |
| 9.7 | Calling Subprograms Indirectly |
| 9.8 | Design Issues for Functions |
| 9.9 | Overloaded Subprograms |
| 9.10 | Generic Subprograms |
| 9.11 | User-Defined Overloaded Operators |
| 9.12 | Closures |
| 9.13 | Coroutines |

**Planning notes:** End of the “easier” Ch. 5–9 stretch. Closures and generics bridge well into OOP and FP later.

---

## Chapter 10 — Implementing Subprograms

**Focus:** Call/return semantics; activation records / stack-dynamic locals; nested subprograms; blocks; implementing dynamic scoping.

| Section | Topic |
| --- | --- |
| 10.1 | The General Semantics of Calls and Returns |
| 10.2 | Implementing “Simple” Subprograms |
| 10.3 | Implementing Subprograms with Stack-Dynamic Local Variables |
| 10.4 | Nested Subprograms |
| 10.5 | Blocks |
| 10.6 | Implementing Dynamic Scoping |

**Planning notes:** More challenging; budget extra lecture time. Strong link to compiler/runtime systems material from the secondary course goal.

---

## Chapter 11 — Abstract Data Types and Encapsulation Constructs

**Focus:** Abstraction; ADT design issues; language examples; parameterized ADTs; encapsulation and naming.

| Section | Topic |
| --- | --- |
| 11.1 | The Concept of Abstraction |
| 11.2 | Introduction to Data Abstraction |
| 11.3 | Design Issues for Abstract Data Types |
| 11.4 | Language Examples |
| 11.5 | Parameterized Abstract Data Types |
| 11.6 | Encapsulation Constructs |
| 11.7 | Naming Encapsulations |

**Planning notes:** Bridge into OOP (Ch. 12). Expect more lecture depth than Ch. 5–9.

---

## Chapter 12 — Support for Object-Oriented Programming

**Focus:** OOP concepts; design issues; language support (inheritance, dynamic binding); implementation; reflection.

| Section | Topic |
| --- | --- |
| 12.1 | Introduction |
| 12.2 | Object-Oriented Programming |
| 12.3 | Design Issues for Object-Oriented Languages |
| 12.4 | Support for Object-Oriented Programming in Specific Languages |
| 12.5 | Implementation of Object-Oriented Constructs |
| 12.6 | Reflection |

**Planning notes:** Challenging chapter; prioritize inheritance and dynamic method binding as the book’s main OOP themes.

---

## Chapter 13 — Concurrency

**Focus:** Subprogram-level concurrency; semaphores, monitors, message passing; Ada/Java/C#; FP concurrency; statement-level concurrency.

| Section | Topic |
| --- | --- |
| 13.1 | Introduction |
| 13.2 | Introduction to Subprogram-Level Concurrency |
| 13.3 | Semaphores |
| 13.4 | Monitors |
| 13.5 | Message Passing |
| 13.6 | Ada Support for Concurrency |
| 13.7 | Java Threads |
| 13.8 | C# Threads |
| 13.9 | Concurrency in Functional Languages |
| 13.10 | Statement-Level Concurrency |

**Planning notes:** Dense and language-specific—select a subset of language case studies if the term is short.

---

## Chapter 14 — Exception Handling and Event Handling

**Focus:** Exception models (C++, Java, Python, Ruby); event handling (Java, C#).

| Section | Topic |
| --- | --- |
| 14.1 | Introduction to Exception Handling |
| 14.2 | Exception Handling in C++ |
| 14.3 | Exception Handling in Java |
| 14.4 | Exception Handling in Python and Ruby |
| 14.5 | Introduction to Event Handling |
| 14.6 | Event Handling with Java |
| 14.7 | Event Handling in C# |

**Planning notes:** Last of the imperative/OOP construct chapters. Can trim event handling if time is short and keep exceptions as the priority.

---

## Chapter 15 — Functional Programming Languages

**Focus:** Math/FP fundamentals; Lisp roots; Scheme (main lab language); Common Lisp, ML, Haskell, F#; FP features in imperative languages; paradigm comparison.

| Section | Topic |
| --- | --- |
| 15.1 | Introduction |
| 15.2 | Mathematical Functions |
| 15.3 | Fundamentals of Functional Programming Languages |
| 15.4 | The First Functional Programming Language: Lisp |
| 15.5 | An Introduction to Scheme |
| 15.6 | Common Lisp |
| 15.7 | ML |
| 15.8 | Haskell |
| 15.9 | F# |
| 15.10 | Support for Functional Programming in Primarily Imperative Languages |
| 15.11 | A Comparison of Functional and Imperative Languages |

**Planning notes:** Provide a Scheme environment for programming exercises. Undergrads may only get through Scheme + selected comparisons; ML/Haskell/F# can be survey-level. Some related material already in Ch. 6 and 8.

---

## Chapter 16 — Logic Programming Languages

**Focus:** Predicate calculus; logic programming overview; Prolog elements, deficiencies, and applications.

| Section | Topic |
| --- | --- |
| 16.1 | Introduction |
| 16.2 | A Brief Introduction to Predicate Calculus |
| 16.3 | Predicate Calculus and Proving Theorems |
| 16.4 | An Overview of Logic Programming |
| 16.5 | The Origins of Prolog |
| 16.6 | The Basic Elements of Prolog |
| 16.7 | Deficiencies of Prolog |
| 16.8 | Applications of Logic Programming |

**Planning notes:** Provide a Prolog processor. Often the chapter most likely to be shortened in an undergrad schedule; graduate courses can cover fully.

---

## 14-week rough layout

Assumptions: one undergrad course, students already know an imperative/OOP language (Java/C#/C++/Python). No required compilers course, so Ch. 4 is a light treatment, not a parser course. Ch. 2 is reading, not lecture. Concurrency and events are survey-level so FP/logic still fit.

### Week-by-week

| Week | Book | Lecture focus | Student work (ideas) |
| --- | --- | --- | --- |
| 1 | Ch. 1 | Why languages; evaluation criteria; paradigms; implementation models | Short write-up: evaluate a language they know against Sebesta’s criteria |
| 2 | Ch. 3 | Syntax: BNF/EBNF, parse trees, ambiguity | Homework: write grammars; disambiguate; draw parse trees. Ch. 2 as background reading |
| 3 | Ch. 3–4 | Semantics survey (static vs dynamic); lexer + recursive-descent *idea* only | Small lab: recursive-descent parser for a tiny expression language *or* skip coding and stay on paper if time is tight |
| 4 | Ch. 5 | Names, binding, scope, lifetime | Problem set + “what does this print?” scope/lifetime quizzes in 2–3 languages |
| 5 | Ch. 6 | Types: primitives through pointers/optionals; type checking / equivalence | Type-design critique (e.g. unions vs optionals vs null) |
| 6 | Ch. 7–8 | Expressions, assignment, control (incl. iterators, goto debate, guarded commands) | Homework: design-issue short answers; optional tiny language comparison table |
| 7 | Ch. 9 | Subprograms: parameter passing, overloading, generics, closures | Midterm around end of week 7 (Ch. 1, 3–9). Parameter-passing tracing exercises |
| 8 | Ch. 10 | Runtime: activation records, nested subs, implementing dynamic scope | Diagram/homework: draw AR stacks; no large coding required |
| 9 | Ch. 11–12 | ADTs/encapsulation → OOP design issues (inheritance, dynamic binding) | Language-feature comparison (Java vs C++ vs C# vs Python/Swift), short paper or slides |
| 10 | Ch. 12–14 | Finish OOP implementation/reflection; exceptions (events optional) | Problem set on dispatch / exception models. Start **term project** if using one |
| 11 | Ch. 13 (survey) | Concurrency models: threads vs monitors vs message passing — pick 1–2 case studies | Reading + discussion or a small Java/C# threading demo, not a full concurrent systems unit |
| 12 | Ch. 15 | Functional programming; Scheme as the lab language | Scheme lab 1 (recursion, lists, higher-order functions) |
| 13 | Ch. 15–16 | Finish FP comparison; start Prolog / logic programming | Scheme lab 2 *or* small FP-in-Java/Python exercise; Prolog lab 1 (facts, rules, queries) |
| 14 | Ch. 16 + wrap | Prolog deficiencies/applications; course synthesis (criteria applied to all paradigms) | Prolog lab 2 (search/puzzle). Project due. Final exam (cumulative, heavier on 10–16) |

If something has to slip: drop Ch. 4 coding, trim Ch. 13 to one lecture, and/or make Ch. 16 a one-week survey instead of two labs.

---

## Assessment mix (high level)

Typical PL courses mix **design critique** (the book’s real point) with **a little coding in unfamiliar paradigms**. A workable split:

| Component | Weight (ballpark) | Format |
| --- | --- | --- |
| Problem sets | 20–25% | Book-style short answers + grammar/tracing exercises; 5–7 over the term |
| Programming labs | 20–25% | 4–6 small labs (see below), not a software-engineering project |
| Midterm | 15–20% | Week 7: written, closed- or open-book; definitions + “compare these designs” + tracing |
| Final | 20–25% | Cumulative; more implementation/OOP/FP/logic than the midterm |
| Term project *or* paper | 15–20% | One substantial artifact (choose one model below) |
| Participation / quizzes | 0–10% | Optional weekly 10-minute “what does this bind to?” quizzes |

**What most courses do *not* do:** a large team app in one language. The learning target is *concepts and comparison*, so assessments should force students to explain *why* a design exists.

---

## Assignment / lab ideas (pick a subset)

Keep labs short (1–2 weeks). Rotate languages rather than deepening one stack.

1. **Grammar & mini-parser** (weeks 2–3) — EBNF for a tiny language; optional recursive-descent or use a parser generator. Ties to Ch. 3–4.
2. **Scope/binding tracer** (week 4) — Given snippets in two languages, predict output and name the binding/scope rule in play.
3. **Type / memory design memo** (week 5) — Compare pointers vs references vs optionals; or implement a tiny “unsafe” vs “safe” type example.
4. **Parameter-passing lab** (week 7) — Same function in C++/Java/Python; show pass-by-value, reference, name/sharing; write a one-page analysis.
5. **Scheme labs** (weeks 12–13) — Recursion + lists; then map/filter/closures. This is the standard FP block with this book.
6. **Prolog labs** (weeks 13–14) — Family tree / graph search / a small puzzle (e.g. coloring, scheduling). Standard logic block.
7. **Exception or OOP dispatch worksheet** (week 10) — Predict which handler/method runs; explain static vs dynamic binding.

---

## Term project options (choose one model)

**A. Language comparison paper/presentation** (most common, lightest logistics)  
Pick two languages and 3–4 constructs (e.g. types, inheritance, exceptions, concurrency). Evaluate with Ch. 1 criteria. 6–8 pages or a 10-minute talk.

**B. “Design a little language” spec** (good for design-focused courses)  
Write a language proposal: BNF, type rules, scoping, a few example programs. No full implementation required. Pair with a short “why these choices” rationale.

**C. Mini interpreter** (heavier; use if Ch. 4 was taught with code)  
Implement a tiny language (arithmetic + lets + functions, or a Scheme subset). Deliverable: interpreter + grammar + write-up of binding/types. Solo or pairs.

**D. Paradigm ports** (good lab-heavy variant instead of a paper)  
Same small problem (e.g. expression evaluator, search) in an imperative language, Scheme, and Prolog, plus a reflection on which features helped.

For a first offering, **A or D** is the safest; **C** only if you want a programming-heavy course and can support it.

---

## Exam / quiz style (what works in this subject)

- **Tracing:** scope, parameter passing, dynamic dispatch, exception propagation.
- **Design issues:** “give two alternatives and a trade-off” (the book’s section structure maps directly to this).
- **Grammar:** write or fix BNF; show two parse trees for an ambiguous grammar.
- **Short definitions:** binding time, strong typing, activation record, referential transparency, unification.
- **Avoid:** asking them to write large programs on exams; save that for labs.

Open-book concept exams work well because the hard part is applying criteria, not recalling a C# keyword.

---

## Supplements (book resources)

- Lecture slides and figures: [pearson.com/cs-resources](https://www.pearson.com/cs-resources)
- Companion site mini-manuals (~100-page language tutorials)
- Instructor solutions via Pearson Instructor Resource Center
- Language processors: JavaScript (browsers), PHP (web servers); other processors linked from companion site
