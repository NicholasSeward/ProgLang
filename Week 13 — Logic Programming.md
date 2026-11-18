# Programming Languages
## Week 13 — Logic Programming (Survey) + Lab Freeze

Sebesta, Chapter 16 (selected)

---

# Today

1. Logic languages as **domain-specific languages** (DSLs)
2. Declarative vs imperative — the abstraction trade-off
3. Queries are not free — search has a cost
4. Predicate calculus, lightly
5. Prolog basics — facts, rules, queries
6. Unification and backtracking
7. Why efficient search is hard to own once you abstract it
8. Deficiencies and applications (survey)
9. Course synthesis — Ch. 1 criteria across paradigms
10. Lab 4 due / feature freeze; Week 14 presentation

---

# Where this fits

| Week | Focus |
| --- | --- |
| 12 | Functional — compute with expressions and functions |
| **13** | Logic as a **relational / search DSL**; Lab 4 freeze |
| 14 | Presentations and live demos |
| Lab 4 | **Due this week** — freeze features; polish demo |

Three paradigms in the book: imperative/OOP (most of the term), functional (15), logic (16). Today also frames Prolog as a **DSL**: a language shaped for one kind of problem (relations and search), not a general-purpose replacement for your Lab language.

Your mini interpreter is a tiny general-purpose language. Prolog is specialized. SQL, regex, CSS, and make are other DSLs you already know — each buys expressiveness in a domain and pays for it somewhere else.

---

# Logic programming as a DSL

A **domain-specific language** is designed so that programs in a narrow domain look like the problem, not like loops and arrays.

| DSL | Domain | What you write |
| --- | --- | --- |
| SQL | Relational data | `SELECT … WHERE …` |
| Regex | String patterns | `/[A-Z]+\\d*/` |
| CSS | Document style | `h1 { font-size: … }` |
| **Prolog** | Relations, rules, search | facts + rules + queries |

```prolog
parent(ann, bob).
parent(bob, cara).

grandparent(G, C) :- parent(G, P), parent(P, C).
```

You did not write a loop over people. You stated what `grandparent` means in the **domain of kinship / graphs**. A query asks whether (or for whom) it holds.

```prolog
?- grandparent(ann, cara).
true.

?- grandparent(ann, X).
X = cara.
```

That is the DSL payoff: **writability** and often **readability** inside the domain. The bill shows up under **cost** and control — next sections.

| General-purpose (your Lab 4) | Relational DSL (Prolog-like) |
| --- | --- |
| You choose the algorithm | The engine chooses a search |
| Cost is visible in your loops | Cost is buried in the inference procedure |
| Harder to say "what is true" | Harder to say "how expensive is this query" |

---

# Section 16.1 — Another way to mean "program"

| Style | You write | The system |
| --- | --- | --- |
| Imperative | Steps that change state | Executes the steps |
| Functional | Expressions and function definitions | Evaluates to a value |
| Logic / relational DSL | Facts and rules about relations | **Searches** for answers that fit |

Same idea as other declarative DSLs: you describe *what* should hold; something else decides *how* to find it.

---

# Section 16.2–16.3 — Predicate calculus (light)

**Propositional** logic deals with whole statements that are true or false.

**Predicate** calculus adds:

| Piece | Meaning | Example |
| --- | --- | --- |
| Constant | A specific thing | `ann`, `bob` |
| Variable | A stand-in | `X`, `P` |
| Predicate | A relation or property | `parent(ann, bob)` |
| Connectives | and, or, not, implies | `parent(G,P) ∧ parent(P,C)` |
| Quantifiers | for all / there exists | "every person has a parent" (in the math, not always in Prolog the same way) |

A **Horn clause** is a useful restricted form: one positive conclusion, zero or more conditions.

```text
grandparent(G, C)  ←  parent(G, P) ∧ parent(P, C)
```

Prolog programs are essentially Horn clauses. Theorem proving in full first-order logic is harder; Prolog picks a tractable subset and a concrete search strategy.

You do not need to prove theorems on the board. You need: **relations + variables + "if these hold, then that holds."**

---

# Section 16.4 — Logic programming overview

A logic program is:

1. A **database** of facts and rules
2. A **query**
3. An **inference** process that tries to satisfy the query

| Term | Meaning |
| --- | --- |
| Fact | Always true: `parent(ann, bob).` |
| Rule | Conditionally true: head `:-` body |
| Goal / query | What we ask the system to prove or instantiate |
| Closed-world | If it cannot be proved, treat it as false |

**Declarative reading:** the clauses describe what is true.  
**Procedural reading:** Prolog tries clauses in order, left to right in the body — that order matters for termination and **cost**.

Both readings matter. Beginners write declarative rules and then discover that clause order and recursion shape can loop forever — or finish, but only after exploring a huge tree.

---

# Queries are not free

A one-line query can look cheap. It is a request to **search**.

```prolog
?- requires(cs480, P).
```

If `requires` is a recursive transitive closure over a dense graph, that line may touch far more work than an imperative nested loop you wrote on purpose. The DSL hid the algorithm. It did not delete the work.

| Illusion | Reality |
| --- | --- |
| "I only wrote a rule" | The engine may try many clause orders and bindings |
| "`;` gives another answer for free" | Each answer is more search from a choice point |
| "Relations work both ways" | Some modes are cheap; others scan everything |
| "Declarative means I ignore performance" | Someone still chose a search strategy — often a naive one |

**Abstraction trade-off (Week 1 cost):**

- Raising the level (facts + rules + query) improves writability in the domain.
- It also moves **algorithm selection** into the language runtime.
- When the default search is wrong for your data, you fight the abstraction: reorder goals, add cuts, index predicates, switch engines, or rewrite the hot path in a general-purpose language.

Developing **efficient search strategies** is then harder, not easier: the "program" no longer *is* the strategy. The strategy is an invisible interpreter (depth-first, left-to-right, backtracking) plus whatever hints the DSL lets you leak back in.

Same pattern outside Prolog:

| DSL | Looks free | Actually costs |
| --- | --- | --- |
| SQL | A short `SELECT` | Joins, full scans, missing indexes |
| Regex | One pattern | Catastrophic backtracking on some inputs |
| ORM / query builders | `filter(...).all()` | N+1 queries, accidental cross products |

Logic programming is the clean classroom version of that lesson.

---

# Section 16.5–16.6 — Prolog basics

## Syntax (enough to read)

| Form | Example |
| --- | --- |
| Fact | `prereq(cs101, cs102).` |
| Rule | `requires(C, P) :- prereq(P, C).` |
| Query | `?- requires(cs102, X).` |
| Variables | Start with uppercase: `X`, `Course` |
| Atoms | Start with lowercase: `cs101`, `ann` |
| Conjunction | Comma means **and** |
| End | Period ends a clause |

```prolog
prereq(cs101, cs102).
prereq(cs102, cs201).
prereq(cs101, cs150).

requires(C, P) :- prereq(P, C).
requires(C, P) :- prereq(Mid, C), requires(Mid, P).
```

`requires` is the transitive closure of prerequisites — same recursive pattern as the Prolog mini assignment.

## Queries

```prolog
?- prereq(cs101, X).
X = cs102 ;
X = cs150.
```

`;` asks for **another** answer. Prolog **backtracks** and tries the next matching clause or binding.

Ask "backward" as easily as "forward":

```prolog
?- prereq(X, cs201).
X = cs102.
```

That is a DSL design point: a relation is not a one-directional function. The same `prereq/2` serves both directions when the mode fits.

**Modes still have costs.** `?- prereq(cs101, X)` may use an index on the first argument. `?- prereq(X, cs201)` may walk every fact. Same relation, different price. The query syntax will not warn you.

---

# Unification

**Unification** finds bindings that make two terms the same.

| Pair | Result |
| --- | --- |
| `cs101` and `cs101` | succeed, no new binding |
| `X` and `cs101` | `X = cs101` |
| `prereq(X, cs102)` and `prereq(cs101, cs102)` | `X = cs101` |
| `cs101` and `cs102` | fail |

Function symbols (compound terms) unify if the functors match and the arguments unify pairwise. Occurs-check and advanced details stay in the book; the classroom idea is **pattern match + bind variables**.

A failed unification makes Prolog abandon that branch and backtrack.

---

# Backtracking (the search — and its bill)

When a goal has several candidate clauses, Prolog tries them **top to bottom**. Inside a rule body, goals run **left to right**. On failure, it undoes bindings and tries the next option.

That fixed strategy *is* the language's default algorithm. You did not write it. You also do not fully control it without stepping outside the pure declarative story.

```text
?- requires(cs201, P).

uses rule 1: prereq(P, cs201)  →  P = cs102   (one answer)
uses rule 2: prereq(Mid, cs201), requires(Mid, P)
             Mid = cs102
             requires(cs102, P) → … eventually P = cs101
```

| Strength | Cost / weakness |
| --- | --- |
| Multiple answers via `;` | Each answer resumes search — not a free list comprehension |
| Relations work in several modes | Some modes explore far more of the tree |
| Short rules match the domain | Wrong clause/goal order → blowups or nontermination |
| Engine owns the search | Harder to invent a custom efficient strategy in-language |

```prolog
% dangerous shape if this is the only rule and nothing shrinks
bad(X) :- bad(X).
```

**Goal order is a performance tool.**  
`popular(P), linked(P, Q)` vs `linked(P, Q), popular(P)` can differ by orders of magnitude if `popular` is rare and `linked` is huge. You are hand-tuning search while still pretending the program is "just logic." That tension is normal in declarative DSLs.

---

# Trace a tiny example (in class)

Facts:

```prolog
parent(ann, bob).
parent(bob, cara).
parent(ann, dan).
```

Query: `?- parent(ann, X).`

1. First fact matches → `X = bob`. User types `;`
2. Third fact matches → `X = dan`. User types `;`
3. No more → false

Query: `?- grandparent(ann, cara).` with the rule from the top of today's notes — walk unification on `G`, `C`, `P`.

---

# Section 16.7–16.8 — Deficiencies and applications (skim)

## Rough edges (especially around cost)

| Issue | Why it hurts |
| --- | --- |
| Opaque search cost | A small query can dominate runtime; profiling means thinking like the engine |
| Hard to own the strategy | Custom algorithms fight DFS + backtracking; you add cuts, tabling, or leave the DSL |
| Closed-world assumption | "Not in the DB" means false — bad for incomplete knowledge |
| Negation as failure | `not` / `\+` is not classical logic negation |
| The cut `!` | Prunes search on purpose — admits the declarative reading was not enough |
| Occurs-check often off | Incorrect edge-case unifications traded for speed |

The cut is the honesty moment: when abstraction will not give you the search you need, the language lets you **punch a hole** back to procedural control. That is a DSL smell and a DSL survival mechanism.

## Where the DSL still earns its keep

| Domain | Why a logic / rules DSL fits |
| --- | --- |
| Expert systems / policies | Encode "when these hold, that follows" |
| Query languages | SQL family: declare the result shape; optimizer owns the plan (same trade-off) |
| Grammars (DCGs) | Parse as proof — stretch on the Prolog mini |
| Config / constraint search | The problem *is* search over relations |
| Teaching | Makes unification and backtracking visible |

Use a relational DSL when the domain is rules and search **and** you accept that performance work becomes "teach the engine" (indexes, goal order, limited cuts) instead of "write the nested loops." When you need a carefully engineered strategy, a general-purpose language (or a hybrid) often wins on **cost** and controllability.

Survey only for applications. The mini assignment stays in facts, rules, and queries — but notice when a query feels "slow for how short it is."

---

# Course synthesis — Ch. 1 criteria

Same four criteria you used in Week 1. Different paradigms stress different ones.

| Criterion | Imperative / OOP | Functional | Logic / relational DSL |
| --- | --- | --- | --- |
| **Readability** | Familiar control flow; large OO designs can hide dispatch | Dense expressions; clear if you know the idioms | High in-domain; opaque once you must see the search tree |
| **Writability** | Everything expressible with enough code | High for transforms and pipelines | High when the problem *is* a relation |
| **Reliability** | Types, tests, encapsulation help; mutation and races hurt | Referential transparency helps; effects still exist | Nontermination; closed world; mode mistakes |
| **Cost** | You own the algorithm (and its bugs) | Learning curve; GC / allocation | Queries look free; search and strategy are hard to manage |

**DSL lesson for the criteria:** abstraction that improves writability often **moves cost into the runtime**. Managing that cost means either learning the engine's strategy or escaping the DSL.

Your **Lab language** is almost certainly a small general-purpose imperative language. That was a design choice:

| Your interpreter | Criteria angle |
| --- | --- |
| Lexer → parser → AST → eval | Clear pipeline (writability of the *implementation*) |
| Mutable vs extend-only env | Reliability of scoping vs simplicity |
| Dynamic typing (typical) | Writability now; fewer static guarantees |
| Evaluate, do not search | You pay in code size; you keep a grip on *how* work happens |

For Week 14, pick **one** real trade-off you made and score it with these criteria — not a generic "Python is readable" slide. If you added any "mini DSL" surface (query-like forms, pattern rules), say what it bought and what it hid.

---

# Lab 4 — due / feature freeze

**Lab 4 is due.** Control (`if`) + loop or recursion + function calls with a per-call scope.

| Do | Do not |
| --- | --- |
| Freeze features | Start closures the night before the demo |
| Fix crashing demos | Add a third loop form "for show" |
| Write the README and expected tests so they match | Leave "works on my machine" undocumented |
| Rehearse the two demo programs | Demo a feature you cannot explain |

The Week 14 artifact **is** this interpreter. Polish beats novelty.

---

# Prolog mini assignment

Assigned this week; due end of Week 14 (with presentations).

Course catalog → prerequisites as facts and rules. Queries forward and backward; at least one multi-answer backtracking query. Handout: the Prolog section in `Course Plan — Language Mini Assignments.md` (and any dedicated handout if posted on classroom50).

Tools: [SWISH](https://swish.swi-prolog.org) or SWI-Prolog.

---

# Week 14 — presentation shape (prep now)

About **8–12 minutes** + live demo + Q&A.

Suggested arc:

1. Language name and one-sentence design goal  
2. Grammar highlights (what you can write)  
3. Architecture: lexer → parser → AST → evaluator  
4. **Live demo** of two programs (happy path + one that shows `if` / call / loop-or-recursion)  
5. **One** design trade-off with Sebesta criteria  
6. Limitations / what you would add next  

Bring a demo script (exact commands / file names). Failure during the talk is normal if you recover; unrehearsed installs are not.

---

# Week 13 wrap

You should be able to:

- Explain Prolog (and logic programming) as a **DSL** for relations and search, with analogs like SQL
- Contrast logic programming with imperative and functional styles
- Argue why a short query is **not** free work
- Say how raising abstraction can make **cost** and **search strategy** harder to manage
- Read a Prolog fact, rule, and query; explain unification and backtracking
- Name one deficiency (cut, closed world, opaque cost) and one domain where the DSL still fits
- Map one Lab 4 design choice onto readability / writability / reliability / cost

---

# Before next week

**Lab 4:** submitted and frozen  
**Prolog mini:** started (facts in a file; a few queries on paper)  
**Presentation:** outline + demo file list  

**Reading:** none new for Week 14

---

# Quick check

1. Name one DSL you already use. What domain does it specialize in, and what does it hide?
2. Why is Prolog usefully described as a domain-specific language rather than only as "another paradigm"?
3. Why is `?- requires(cs480, P).` not obviously cheap just because it is one line?
4. How does abstracting over search make it harder to develop an efficient strategy?
5. In `requires(C, P) :- prereq(Mid, C), requires(Mid, P).` what does the comma mean?
6. Why might `?- prereq(X, cs201).` cost more than `?- prereq(cs101, X).` even though both use the same relation?
7. User presses `;` after an answer. What is Prolog doing, and why is that not "free"?
8. What does the cut `!` admit about the declarative DSL story?
9. Name one Lab 4 choice and which Ch. 1 criterion it favored or hurt.
