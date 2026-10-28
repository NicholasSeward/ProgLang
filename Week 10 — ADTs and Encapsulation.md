# Programming Languages
## Week 10 — Abstract Data Types and Encapsulation

Sebesta, Chapter 11

---

# Today

1. Process abstraction vs data abstraction
2. What an ADT is — interface and hidden representation
3. Design issues
4. How a few languages enforce (or don't enforce) that hiding
5. Parameterized ADTs
6. Encapsulation constructs vs naming encapsulations
7. What this is not yet (Week 11)
8. Lab 4, briefly

---

# Where this fits

| Week | Focus |
| --- | --- |
| 5 | Types as sets of values and operations |
| 7–8 | Subprograms — hiding a **process** |
| **10** | Abstract data types — hiding **data** plus its operations |
| 11 | Classes, inheritance, dynamic dispatch |
| Lab 4 | Functions and control on your interpreter |

A function hides *how a computation is done*. An ADT hides *how data is stored*, and offers operations instead of fields.

---

# Section 11.1 — Abstraction

An **abstraction** is a view that keeps what the user needs and drops the rest.

| Kind | Hidden thing | User sees | Example |
| --- | --- | --- | --- |
| Process abstraction | The steps | A call | `sort(items)` |
| Data abstraction | The storage **and** the steps that touch it | A type and its operations | a `Stack` with `push` and `pop` |

Subprograms (Chapter 9) are process abstraction. They showed up early because a program is easier to read as named tasks than as one long list of steps.

Data abstraction comes later. A stack written as a bare array forces every caller to know the index, the length, and the "full" rule. Move the stack from an array to a linked list and every one of those callers breaks. Hide the array, and only `push` and `pop` change.

Two payoffs, both from Week 1's criteria:

| Payoff | Why |
| --- | --- |
| **Reliability** | Callers cannot put the structure into a state the operations would never create |
| **Writability of the rest of the program** | Callers write `push(x)`, not the representation's bookkeeping |
| **Cost of change** | The representation can be replaced without rewriting clients |

---

# Section 11.2 — What an ADT is

An **abstract data type** has three parts:

1. A **type name**
2. The **operations** that make up its interface
3. A **representation** that clients cannot use

A stack, as a client sees it:

| Operation | Meaning |
| --- | --- |
| `push(x)` | put `x` on top |
| `pop()` | remove and return the top |
| `is_empty()` | true if there is nothing to pop |

The array, the top index, and any helper that fixes the array stay behind the interface.

This is only an ADT if that last sentence is true. A class with a public array is a record with extra methods. Clients will use the array, and then you cannot change it.

```text
s.data[s.top] = 4     ← reaches past the interface
s.top = s.top + 1     ← the object can now lie about its own size
```

After that, `pop` and `is_empty` are guesses. The type stopped being abstract at the moment the field became reachable.

---

# Section 11.3 — Design issues

Sebesta's design questions for an ADT facility:

| Question | What you are choosing |
| --- | --- |
| What **container** holds the interface? | A class, a package, a module, a file |
| Can the type be **parameterized**? | One stack definition for many element types |
| What **access controls** exist, and where are they written? | `public` / `private`, or a convention, or a language with no wall at all |
| Is the **specification separate** from the implementation? | Clients compile against a header or a package spec; the body can change later |

A few consequences worth saying out loud:

- **Access has to be the language's job** if you care about reliability. A comment that says "please don't touch `top`" is a hope.
- **Partial hiding** is common and useful. Some operations are public, some are for the implementation only, and some are visible to the same package but not to the rest of the program (Java's default access).
- **A separate specification** means a reader can learn the type without reading the representation. Ada packages and C++ headers do this. A Java class usually puts the method signatures and the private fields in the same file, so the split is by keyword, not by file.
- **Initialization** belongs on the interface too. If a client can build a stack and forget to set `top` to 0, the representation has leaked into the startup rule. Constructors (or Ada's default field values) keep that rule inside the type.

---

# Section 11.4 — Language examples

Same stack. The interesting difference is how hard it is to cheat.

## C++

`private` is enforced. A caller cannot write `s.data[i]`.

```cpp
class Stack {
public:
    Stack();
    void push(int x);
    int pop();
    bool is_empty() const;
private:
    int data[100];
    int top;
};
```

The class declaration often lives in a header. Method bodies can live in a `.cpp` file. Clients include the header, so they *see* the private fields in the text, but the compiler rejects any use of them. Seeing is not the same as accessing.

`protected` is a third level: visible to derived classes. That matters next week, when inheritance shows up. `friend` is a deliberate hole — a named function or class may touch the private part. It is useful for operators that are not methods. It is also a way to throw the hiding away, so it belongs on a short list, not on every helper.

## Java

Same shape: public methods, private fields. The compiler enforces it.

Java has no standard separate specification file. The class source is both the interface and the implementation; privacy is marked member by member.

| Modifier | Who can use it |
| --- | --- |
| `public` | Anyone |
| `protected` | The class, its subclasses, and the same package |
| *(none)* | The same package only |
| `private` | The class only |

Package-private (no modifier) is easy to miss. Two classes in one package can share details that the rest of the program cannot see. That is already a small encapsulation, not just one type.

## Python

A class with methods. A leading underscore is a **convention**: `_data` means "internal." Another module can still read `s._data`.

Name mangling (`__data` becomes `_Stack__data` on the object) makes accidents less likely. It is not a wall. `s._Stack__data` still works.

| Language | Hiding |
| --- | --- |
| C++, Java | The compiler rejects access to private members |
| Python | Privacy is a naming habit |

That is the reliability vs writability trade-off from Week 1. A wall catches mistakes. A convention is less to type, and it will not stop a determined caller.

## Ada (the textbook's clean split)

Ada puts the **specification** and the **body** in two places. Clients can be shown only the spec.

```ada
package Stack is
   type Stack_Type is limited private;
   procedure Push(S : in out Stack_Type; X : in Integer);
   procedure Pop(S : in out Stack_Type; X : out Integer);
   function Is_Empty(S : Stack_Type) return Boolean;
private
   type Stack_Type is record
      Data : array (1 .. 100) of Integer;
      Top  : Integer := 0;
   end record;
end Stack;
```

The package **body** (not shown) holds the procedure code. `limited private` means clients may declare variables of the type and pass them to these operations, and they may not even assign one stack to another unless you provide that operation. The representation is in the spec's `private` part so the compiler can lay out the record, but client code still cannot name `Data` or `Top`.

You do not need to write Ada. You need the design point: the interface can be a separate artifact from the code that implements it.

---

# Section 11.5 — Parameterized ADTs

A stack of `int` and a stack of `string` should not be two pasted copies. The operations are the same. Only the element type changes.

| Language | Mechanism | When the element type is fixed |
| --- | --- | --- |
| Ada | Generic packages | Compile time; you instantiate the package |
| C++ | Templates: `Stack<int>` | Compile time; the compiler can generate a version per type |
| Java | Generics: `Stack<String>` | Checked at compile time; one implementation is shared (erasure) |
| Python | One class | Not fixed. Elements can be anything; mistakes show up at run time |

The interface stays `push` / `pop`. Callers write the element type at the point of use instead of maintaining `IntStack` and `StringStack` by hand.

C++ can specialize a template when one element type needs different code. Java's generics mostly refuse operations the shared implementation could not perform on every type (you do not get to call `String` methods on an unbounded `T` without saying so). Python checks nothing until a method actually runs. Same surface idea, different moment of checking — the static vs dynamic trade-off from Week 5, applied to a container.

---

# Section 11.6 — Encapsulation constructs

An ADT packages **one** type with its operations. Large programs need a bigger box: several types that cooperate and should be compiled, named, and sometimes hidden as a group.

That bigger box is an **encapsulation**.

| Construct | What it groups | Limit |
| --- | --- | --- |
| Nested subprograms | Helpers inside one routine | Hides code, not a data representation shared by several types |
| A class | One type | Awkward when `Token` and `Lexer` need to share details |
| A package, module, or assembly | Several types and functions | Can hide some of them from outside the group |

Examples of the third row: a Java package, a Python module, an Ada package holding more than one type, a C# assembly.

A lexer is the example to keep. `Token`, `Lexer`, and a small helper for characters belong together. Clients should construct a lexer and ask for tokens. They should not rebuild tokens by hand if that can break the lexer's invariants. The package is the encapsulation. Each of those types may also be an ADT on its own.

---

# Section 11.7 — Naming encapsulations

Grouping code does not by itself stop **name collisions**. Two libraries can both define `List` or `Stack`.

A **naming encapsulation** gives the group a qualified name so the simple names can coincide:

| Language | Qualified name |
| --- | --- |
| C++ | `std::vector` |
| Java | `java.util.List` and `java.awt.List` |
| Ada | `Ada.Text_IO` |
| Python | `json.dumps` after `import json` |
| Ruby | A module used as a namespace |

```text
java.util.List     a sequence
java.awt.List      a UI widget
```

Same simple name, two types. `import java.util.List` brings one of them into unqualified use. Without a naming scheme, the compiler or the linker has two definitions of `List` and no rule for which one a given call means.

Chapter 11 separates the two jobs on purpose:

| Job | Question it answers |
| --- | --- |
| Encapsulation construct | What is grouped, and what can code outside the group see? |
| Naming encapsulation | How do we mention that group so its names do not collide with someone else's? |

One language feature often does both. A Java package is a directory of classes (encapsulation) and a dotted name (naming). A Python module is a file of definitions and also the prefix on those names. C++ splits them more often: a class hides members, and a namespace qualifies names. You still have to be able to say which job you mean.

---

# Not yet: objects

A class used only as an ADT — private data, public operations, no inheritance — is Chapter 11.

Week 11 adds what makes a language object-oriented in Sebesta's sense:

- Classes as types (already useful for ADTs)
- **Inheritance**
- **Dynamic binding** of method calls

You can have ADTs with none of the last two. Do not treat "it has classes" as "it is object-oriented." Python and Java are. A C program with careful `.h` files can approximate an ADT and is not object-oriented. C++ can be used either way.

---

# In class (10 minutes)

Pick a small type: a counter, a point, or a queue.

Write three short lists:

1. **Public** — operations a caller needs, including how a new value starts life
2. **Hidden** — fields they should not touch
3. **Who enforces it** — compiler, or convention

Then one naming question: another library ships a type with the same name. How would Java, C++, or Python tell the two apart in client code?

---

# Lab 4

Lab 3 runs expressions and names. One global environment is enough for that lab, as long as a function scope can be attached later.

Lab 4 adds the rest of the language you already **parsed**:

- `if` / `else`
- a loop
- function definitions and **calls**

Each call is the natural place for a second scope. Parameters live there. The caller's names are the outer frame.

Kickoff is choosing the order you will add those three, and one sample program you want the final demo to run. No new lab writeup today.

---

# Week 10 wrap

You should be able to:

- Say what an ADT hides, and what the caller still sees
- Explain why a public field breaks the abstraction even if the methods are correct
- List the design choices: container, parameters, access control, separate specification
- Separate "the language enforces hiding" from "the programmer agrees to hide it"
- Give one reason to parameterize a container, and say when the element type actually gets checked
- Distinguish one type, an encapsulation of several types, and a naming encapsulation that prevents collisions

---

# Before next week

**Read:** Chapter 12, Sections 12.1–12.5 (object-oriented programming)

**Also on the syllabus for Week 11:** exceptions (Ch. 14, selected) and a short concurrency survey (Ch. 13, selected)

---

# Quick check

1. Why is a public array inside a "stack class" not an ADT?
2. Name one operation that belongs on the public side of a queue, and one thing that should stay hidden.
3. A client sets `top` to 50 on a stack whose array only has 10 slots. Which ADT payoff just failed?
4. Python's `_data` and Java's `private` solve the same problem differently. What is the difference?
5. C++ clients can read `data` in the header and still cannot use it. Why is that still hiding?
6. Why write `Stack<String>` instead of a separate `StringStack` class? When does Java check that the elements really are strings, compared with Python?
7. What does a separate package specification (Ada) or header (C++) give you that a single Java source file does not?
8. A Java package holds `Token` and `Lexer`. Is that one ADT, or an encapsulation of several types?
9. `java.util.List` and `java.awt.List` share a simple name. Which Chapter 11 idea keeps that from being a collision?
