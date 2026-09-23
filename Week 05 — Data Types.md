# Programming Languages
## Week 5 — Data Types

Sebesta, Chapter 6 (selected)

---

# Today

1. What a type is
2. Primitive types
3. Strings, enums, arrays (quick)
4. Associative arrays, records, tuples, lists
5. Unions
6. Pointers vs references
7. Optional types
8. Type checking, strong typing, type equivalence

Keep it conceptual — design issues, not every language detail.

---

# Where this fits

| Week | Focus |
| --- | --- |
| 4 | Names and bindings |
| **5** | What values **are** (types) |
| 6 | Expressions and control |
| Lab 2 | Parser → AST (types come later in Lab 3+) |

---

# What is a type?

A **type** describes:

- A set of possible **values**
- The **operations** that are allowed on them

```text
int     →  …, -1, 0, 1, 2, …    and  + - * / …
boolean →  true, false           and  and or not …
```

Types help **catch mistakes** and **document intent**.

---

# Design questions for every type

| Question | Example |
| --- | --- |
| What values are allowed? | `int` range |
| What operations? | Can you `/` two ints? |
| How stored? | 32-bit? heap object? |
| Checked when? | Compile time vs run time |

---

# Primitive types

Built-in, usually not defined in terms of other types.

| Kind | Typical languages |
| --- | --- |
| Integer | Almost all |
| Floating point | Almost all |
| Boolean | Java, Python, C++, C#; C uses ints historically |
| Character | C, Java, … |

---

# Numeric design issues (simple)

| Issue | Trade-off |
| --- | --- |
| Size / range | `int` vs `long` vs bigint |
| Signed vs unsigned | C/C++ have both; Java ints are signed |
| Overflow | Wrap, trap, or grow (Python int) |
| Float precision | Fast approx vs exact decimals (money!) |

---

# Character strings

| Style | Languages |
| --- | --- |
| Primitive / built-in | Python, Java, C#, JS |
| Array of char | Classic C |
| Immutable | Java, Python, JS strings |
| Mutable | C++ `string`, some builders |

Design issues: length limit, how concatenation works, Unicode support.

---

# Enumeration types

Named constants in a small set:

```text
enum Day { Mon, Tue, Wed, Thu, Fri, Sat, Sun }
```

| Benefit | |
| --- | --- |
| Readability | `Mon` vs `0` |
| Safety | Can't assign `42` if checked |

| Languages |
| --- |
| C, C++, Java, C#, Rust, Swift, Python `Enum` |

---

# Array types

Same-type elements, indexed access.

| Issue | Examples |
| --- | --- |
| Index range | 0-based (C, Java, Python) vs 1-based (some Fortran, Lua) |
| Fixed vs dynamic size | C array vs Java `ArrayList` / Python `list` |
| Bounds checking | Java yes; C traditionally no |

---

# Associative arrays

Collection of values indexed by **keys** (not only integers).

```text
ages["Ada"] = 36
```

| Also called | |
| --- | --- |
| Map, dictionary, hash | same idea |

| Languages |
| --- |
| Python `dict`, JS objects / `Map`, Ruby `Hash` |
| Java `HashMap`, C# `Dictionary` |
| Perl hashes, Lua tables, PHP arrays (dual nature) |

---

# Associative arrays — design issues

| Issue | Questions |
| --- | --- |
| Key type | Strings only? Any hashable type? |
| Missing key | Error? Default? Optional? |
| Ordering | Unordered hash vs insertion order |
| Mutability | Can you add/remove keys freely? |

Good for flexible data; less ideal when you need fixed fields and strong structure.

---

# Record types

A **record** groups **named fields**, possibly of different types.

```text
Person { name: string, age: int }
```

| Related idea | Languages |
| --- | --- |
| `struct` | C, C++, Go, Rust |
| `class` / object fields | Java, C#, Python |
| `record` | COBOL (historic), C# `record`, Java records |

Access is usually by **field name**: `p.age`.

---

# Records — design issues

| Issue | Notes |
| --- | --- |
| Field names | Part of the type’s interface |
| Nesting | Records inside records |
| Ellipsis / partial updates | Some languages allow copying with changes |
| vs class | Records often emphasize data; classes add methods / identity |

Compared to arrays: heterogeneous fields, **names** instead of numeric indices.

---

# Tuple types

Fixed-length product of types — fields often by **position**, not name.

```text
(string, int)      // e.g. ("Ada", 36)
```

| Languages |
| --- |
| Python tuples, ML / Haskell / F# / Rust / Swift tuples |
| C# `ValueTuple`, modern JS (via arrays / destructuring patterns) |

Useful for multiple return values and lightweight pairing.

---

# Tuples vs records

| | Tuple | Record |
| --- | --- | --- |
| Access | Usually index / pattern | Field name |
| Best when | Short, positional | Named, documented fields |
| Example | `(x, y)` point | `{ x: …, y: … }` |

Some languages blur them (named tuples, destructuring).

---

# List types

Sequence of elements — often **homogeneous**, often **growable**.

| Flavor | Languages |
| --- | --- |
| Linked list (classic FP) | Scheme, Lisp, Haskell, ML |
| Dynamic array “list” | Python `list`, Java `ArrayList` |
| Immutable list | Many FP libs; Clojure; Java `List.of` style |

FP languages put lists at the center (head/tail, recursion).

---

# Lists — design issues

| Issue | Notes |
| --- | --- |
| Mutable vs immutable | In-place update vs new list |
| Heterogeneous? | Python yes; Java generics usually no |
| Operations | Cons, append, slice, map/filter |
| Performance | Linked list vs array-backed |

Arrays: fixed random access.  
Lists: often flexible size / sequential processing.

---

# Aggregate types — one glance

| Type | Index / access | Field types |
| --- | --- | --- |
| Array | Integer index | Usually same |
| Associative array | Key | Usually same |
| Record | Field name | Can differ |
| Tuple | Position | Can differ |
| List | Sequence ops | Often same |

---

# Union types

Same memory, **different** interpretations.

```text
// idea: one cell, either an int OR a float
union Number { int i; float f; }
```

| Kind | Meaning |
| --- | --- |
| Free union | No tag — unsafe if you read the wrong view |
| Discriminated union | Tag remembers which case is live |

| Languages |
| --- |
| C/C++ `union` (often free) |
| Rust `enum`, Swift `enum` with associated values (safe) |
| ML / Haskell / F# discriminated unions |

---

# Why unions are a design hot spot

| Free unions | Discriminated |
| --- | --- |
| Compact, flexible | Safer |
| Easy to misuse | Extra tag storage |
| Hurts reliability | Helps reliability |

Sebesta ties this to **evaluation criteria** from Week 1.

---

# Pointers vs references

Both mean: “refers to another value / object.”

| | Pointer (classic) | Reference (managed) |
| --- | --- | --- |
| Arithmetic? | Often yes (`p + 1`) | Usually no |
| Null? | Often allowed | Sometimes banned / optional |
| Who frees memory? | Often you | Often GC / ownership |

| Languages |
| --- |
| Pointers: C, C++, Rust (`*`, raw) |
| References: Java objects, Python names, C# refs, JS objects |
| Both flavors: C++, Rust |

---

# Pointer problems (why Java skipped them)

| Problem | |
| --- | --- |
| Dangling pointer | Point to freed memory |
| Memory leak | Forgot to free |
| Wild pointer | Uninitialized |

References + GC (or ownership) trade **control** for **safety**.

---

# Optional types

A value that may be **present** or **absent** — without abusing `null`.

```text
Optional<User> findUser(id)   // maybe no user
```

| Approach | Languages |
| --- | --- |
| `null` / `None` | Java, C#, Python, JS |
| `Optional` / `option` | Java `Optional`, Rust `Option`, Swift `Optional` |
| Maybe type | Haskell `Maybe` |

Design goal: make “missing” **explicit** in the type.

---

# Type checking

**Type checking** = enforce type rules.

| When | Name | Languages |
| --- | --- | --- |
| Before run | Static | Java, C#, Rust, C++, Go |
| During run | Dynamic | Python, JS, Ruby |
| Mix | Gradual / optional | TypeScript, Python + hints |

---

# Strong typing (Sebesta sense)

A language is **strongly typed** if type errors are **always detected** (at compile time or run time).

| Stronger lean | Weaker lean |
| --- | --- |
| Java, Python, Rust, Ada | C (looser conversions, unchecked unions) |

“Strong” ≠ “static.” Python is dynamically checked but often called strongly typed; C is static-ish but weaker about some errors.

---

# Type equivalence — when are two types “the same”?

| Rule | Idea | Flavor |
| --- | --- | --- |
| **Name equivalence** | Same declaration / name | Stricter |
| **Structure equivalence** | Same shape of fields | More flexible |

```text
type Celsius = float
type Fahrenheit = float
```

Under **name** equivalence, these may be different types even if both are floats.

---

# Type equivalence — languages (rough)

| Lean name equivalence | Lean structural |
| --- | --- |
| C, C++, Java (mostly) | ML family (often structural for records) |
| Ada (strict) | Go (structural for some interfaces) |

Exact rules vary — know the **idea**, not every edge case.

---

# Coercion vs cast (preview for Week 6)

| | |
| --- | --- |
| **Coercion** | Implicit conversion (`2 + 3.5`) |
| **Cast** | Explicit conversion (`(int) x`) |

More coercion → easier to write, easier to hide bugs.

---

# Quick map

| Topic | One-line takeaway |
| --- | --- |
| Primitive | Built-in values + ops |
| String / enum / array | Common structured / named forms |
| Associative array | Key → value |
| Record | Named fields, mixed types |
| Tuple | Positional product type |
| List | Sequence; often growable / FP-friendly |
| Union | One storage, multiple views — tag if you want safety |
| Pointer vs ref | Control vs safety |
| Optional | Absence in the type system |
| Checking | Static vs dynamic |
| Strong typing | Type errors get caught |
| Equivalence | Name vs structure |

---

# Tie-back to your language lab

Your Lab 1 language already has ints, floats, strings, …

| Design choice | Ask yourself |
| --- | --- |
| Are strings mutable? | |
| Do you have `null` or an optional? | |
| Are types checked in Lab 3, or only at runtime? | |

You do not need a full type checker yet — but start noticing the issues.

---

# Week 5 wrap

You should be able to:

- Define what a type provides (values + operations)
- Contrast unions, pointers/references, and optionals at a high level
- Explain static vs dynamic type checking
- Explain strong typing and name vs structure equivalence briefly

---

# Before next class

**Read:** Chapter 7 and Chapter 8 (expressions, assignment, control)

**Lab 2:** parser → AST (due end of Week 5 per plan)
