# Programming Languages
## Week 1 — Preliminaries

Sebesta, Chapter 1

---

# Today

- Why study programming languages?
- Programming domains
- Evaluation criteria
- What shapes language design
- Language categories
- Design trade-offs
- How languages are implemented
- Programming environments

---

# Course through-line

You will **compare languages by design**, not memorize one dialect.

By the end of the term you will also build a **mini interpreter**:

**source → tokens → AST → evaluate**

---

# Why study programming languages?

---

# Reason 1 — Choose better tools

Knowing language concepts helps you pick the right language for a problem.

Not every job wants the same trade-offs.

---

# Reason 2 — Learn languages faster

Once you know the *ideas* (scope, types, binding, evaluation…),

a new language is mostly new syntax for old concepts.

---

# Reason 3 — Understand implementation

Knowing how languages run (compile, interpret, manage memory)

makes you a better debugger and designer.

---

# Reason 4 — Better use of your languages

Even in a language you already know,

features you ignore (closures, generics, iterators…) become usable once you understand them.

---

# Reason 5 — Advance the field

Language design is still evolving.

Understanding criteria and trade-offs is how new languages (and features) get judged.

---

# Programming domains

Different domains push languages in different directions.

---

# Scientific computing

- Heavy floating-point math
- Arrays and matrices
- Historically: Fortran
- Today: also Python, Julia, C++, specialized libraries

---

# Business applications

- Reports, records, decimal money
- Lots of I/O and data formatting
- Historically: COBOL
- Today: also enterprise stacks (Java, C#, etc.)

---

# Artificial intelligence

- Symbolic computation, search, knowledge representation
- Historically: Lisp, Prolog
- Today: Python dominates practice; ideas still matter

---

# Systems programming

- OS, drivers, embedded, performance-critical code
- Need efficiency and low-level control
- C, C++, Rust, and friends

---

# Web software

- Markup + scripting + server languages
- JavaScript/TypeScript on the client
- Many options on the server

---

# Takeaway on domains

A language is rarely “best” in general.

It is often **strong in a domain** because of design choices that match that domain’s needs.

---

# Language evaluation criteria

Sebesta’s main yardsticks for judging languages and features.

---

# Four big criteria

1. **Readability** — easy to understand
2. **Writability** — easy to create programs
3. **Reliability** — behaves as intended under use
4. **Cost** — total cost of using the language

---

# Readability

Can a trained reader understand the program?

Helped by:

- Simple, consistent syntax
- Meaningful structure
- Good support for abstraction
- Limited weird special cases

---

# Writability

Can a programmer express solutions easily?

Related to readability, but not identical.

Helped by:

- Expressiveness
- Abstraction
- Orthogonality (combine features freely)

---

# Orthogonality (quick idea)

A small set of constructs that combine **cleanly**.

More orthogonal → fewer special rules → easier to learn and use.

Too much orthogonality can also create bizarre (but “legal”) combinations.

---

# Reliability

Does the program do what we mean, even under bad input / long use?

Helped by:

- Type checking
- Exception handling
- Restricted aliasing
- Readability + writability (bugs hide in confusing code)

---

# Cost

Not just runtime speed.

Also:

- Training programmers
- Writing the code
- Compiling
- Executing
- Maintaining / unreliable systems
- Poor language tools

---

# Other criteria (often discussed)

- **Portability** — move across platforms
- **Generality** — wide applicability
- **Well-definedness** — precise, complete language definition

---

# Influences on language design

What pushes designers toward certain choices?

---

# Computer architecture

Most popular languages still reflect the **von Neumann** model:

- Data and program in memory
- CPU fetches and executes instructions
- Variables ≈ memory cells
- Assignment is central

---

# Programming methodologies

How people build software changes language needs:

- Structured programming → better control structures
- Data abstraction → modules / ADTs
- Object orientation → classes, inheritance, dynamic binding
- Concurrency, security, etc. continue to push new features

---

# Language categories (paradigms)

Rough buckets — real languages often mix them.

---

# Imperative

- Statements change program state
- Variables, assignment, iteration
- Examples: C, Python (mostly), Java (mostly)

---

# Object-oriented

- Data + behavior bundled as objects
- Inheritance, dynamic method binding
- Examples: Java, C#, Smalltalk; also C++ / Python hybrids

---

# Functional

- Computation as evaluating functions
- Prefer immutable data, composition, higher-order functions
- Examples: Haskell, Scheme, ML, F#; features appear in many modern languages

---

# Logic

- State relations and rules; system searches for answers
- Examples: Prolog

---

# Design trade-offs

You rarely maximize every criterion at once.

---

# Classic tensions

- **Reliability vs cost of execution**  
  Extra checks slow programs down.

- **Readability vs writability**  
  Concise syntax can be cryptic.

- **Flexibility vs safety**  
  More power often means more ways to shoot yourself.

---

# Your job in this course

When you see a language feature, ask:

1. What problem does it solve?
2. What does it cost?
3. What criterion did the designers prioritize?

---

# Implementation methods

How does source code become running behavior?

---

# Compilation

Source → compiler → machine code → run on hardware

**Pros:** usually faster execution  
**Cons:** compile step; less portable machine code

---

# Pure interpretation

Source → interpreter executes it directly

**Pros:** great for flexibility, debugging, quick edits  
**Cons:** usually slower

---

# Hybrid implementation

Source → intermediate form → virtual machine / JIT

Examples: Java bytecode, .NET, many scripting languages

Balances portability and performance.

---

# Preprocessors (mention)

Some systems transform code before compilation

(e.g., C macros, other source-to-source tools).

Useful to know they exist; not our focus this week.

---

# Programming environments

A language is more than a grammar.

---

# What surrounds a language?

- Editors / IDEs
- Compilers / interpreters
- Debuggers
- Libraries and package ecosystems
- Build and test tools

These heavily affect **cost** and **writability** in practice.

---

# Week 1 wrap

You now have the course’s vocabulary:

- Domains
- Criteria (readability, writability, reliability, cost)
- Paradigms
- Trade-offs
- Implementation models

We will reuse these all term — including in your mini-interpreter design decisions.

---

# Before next class

**Read:** Chapter 1 (entire)

**Optional:** skim Chapter 2 for history

**Next week:** syntax — BNF, parse trees, ambiguity  
(Section 3.1-3.3)

---

# Discussion prompt

Pick a language you know well.

Name **one strength** and **one weakness** using Sebesta’s criteria.

Be ready to defend the trade-off.
