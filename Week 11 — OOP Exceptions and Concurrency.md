# Programming Languages
## Week 11 — OOP, Exceptions, and Concurrency (Survey)

Sebesta, Chapter 12 (selected), Chapter 14 (selected), Chapter 13 (survey)

---

# Today

1. What makes a language object-oriented
2. Design issues for OOP
3. Inheritance and dynamic binding
4. How dispatch is implemented (vtables, briefly)
5. Reflection (one slide)
6. Exception handling models
7. Concurrency survey — threads, monitors, message passing
8. Lab 4

---

# Where this fits

| Week | Focus |
| --- | --- |
| 10 | ADTs — one type, hidden representation, no inheritance required |
| **11** | Classes with inheritance and dynamic binding; exceptions; a short look at concurrency |
| 12 | Functional programming |
| Lab 4 | Control + functions on your interpreter |

Week 10 said a class used only as an ADT is not yet "object-oriented." Today adds the rest of Sebesta's definition.

---

# Section 12.2 — Three pillars

Sebesta counts a language as object-oriented when it has all three:

| Pillar | Meaning |
| --- | --- |
| **Abstract data types** | Classes as types with interface and hidden representation |
| **Inheritance** | A new class reuses and extends an existing one |
| **Dynamic binding** of method calls | Which method runs is chosen from the **object's** type at run time, not only from the variable's declared type |

A language can have classes and still miss the third. C with careful `.h` files can approximate ADTs and is not object-oriented. Java, C#, Python, and C++ (when used that way) have all three.

```text
Animal a = new Dog();
a.speak();          ← which speak? Dog's, if speak is dynamically bound
```

The variable's type is `Animal`. The object's type is `Dog`. Dynamic binding follows the object.

---

# Section 12.3 — Design issues (the short list)

Sebesta's chapter is long. These are the choices that show up again and again.

| Issue | The question |
| --- | --- |
| Exclusivity of objects | Is everything an object (Smalltalk, often Python), or are there also primitives (Java `int` vs `Integer`)? |
| Are subclasses subtypes? | Can a `Dog` always be used where an `Animal` is expected? |
| Single vs multiple inheritance | One parent, or several? |
| Allocation and deallocation | Stack? Heap? Who frees the object — programmer, GC, or ownership rules? |
| Dynamic and static binding | Is every method call dynamic, or can some be fixed at compile time? |
| Nested classes | Can a class live inside another? |
| Initialization | Constructors, default fields, order when a subclass is built |

Two that matter for tracing code:

**Subtype.** If every `Dog` is a legal `Animal`, the subclass is a subtype. That is the usual rule, but only if the subclass keeps the parent's contracts. A `Square` that overrides `setWidth` and also changes height can break callers that thought they had a `Rectangle`. Inheritance of **implementation** is not the same as a safe subtype.

**Single vs multiple inheritance.** Java and C# allow one class parent and many interfaces. C++ allows several class parents. Multiple inheritance is powerful and messy when two parents define the same name (the diamond problem). Interfaces avoid some of that by carrying no (or little) state.

---

# Inheritance

A **derived class** (subclass) gets the members of a **parent class** (superclass) and can add or override some of them.

```java
class Animal {
    void speak() { System.out.println("..."); }
}

class Dog extends Animal {
    void speak() { System.out.println("woof"); }
    void fetch() { /* ... */ }
}
```

| What the subclass can do | |
| --- | --- |
| Add fields and methods | `fetch` |
| Override a method | `speak` |
| Use inherited members | anything `Animal` made visible |
| Be used as the parent type | if it is a subtype |

**Access** still matters. `private` in the parent is not visible in the child. `protected` is. That is why Week 10 mentioned `protected` as a third level: it exists for inheritance, not only for ADTs.

**Abstract classes and interfaces.** An abstract class can leave some methods unimplemented. An interface (Java, C#) is a pure contract: method signatures, no (or minimal) code historically, more default methods in modern Java. A class that claims to implement an interface must supply the methods. That is how Java gets "multiple inheritance of type" without multiple inheritance of implementation.

---

# Dynamic binding

**Static binding:** which method runs is known from the call site's types at compile time.

**Dynamic binding** (dynamic dispatch): which method runs depends on the **run-time** type of the receiving object.

```java
Animal a = new Dog();
a.speak();    // Dog.speak — dynamic
```

| Language | Default for ordinary methods |
| --- | --- |
| Java | Dynamic (unless `final` / `static` / `private`) |
| Python | Dynamic |
| C++ | Static unless the method is marked `virtual` |
| C# | Static unless marked `virtual` / `override` |

C++ makes the cost opt-in. Java makes the flexible default opt-out. Same design trade-off as Week 1: reliability and flexibility of subtype polymorphism versus a cheaper, more predictable call.

Dynamic binding is what makes this useful:

```text
for each Animal a in zoo:
    a.speak()     // each animal's own version
```

The loop does not need a type test for every species. New subclasses plug in without editing the loop. That is the payoff. The cost is an extra level of indirection on each call, and a harder time reading "which code runs next" without knowing the object's real type.

---

# Section 12.5 — Implementation sketch (vtables)

How does the machine find `Dog.speak` from an `Animal` variable?

A common design:

1. Each object carries a pointer to a **virtual method table** (vtable) for its class.
2. The vtable is an array of function pointers, one slot per dynamically bound method.
3. A call `a.speak()` becomes: load the vtable from `a`, jump to the `speak` slot.

```text
Dog object
┌──────────┐
│ vptr  ──────►  Dog vtable
│ fields…  │     [0] Dog.speak
└──────────┘     [1] …
```

A `Dog` subclass that overrides `speak` gets its own vtable with a different pointer in that slot. The call site is the same machine code either way. Only the table changes.

You do not need to draw compiler internals on an exam. You need the idea: **the object knows its class's table**, and the call indexes that table.

Multiple inheritance makes several tables or more complex layouts. That is one reason many languages prefer single inheritance plus interfaces.

---

# Reflection (brief)

**Reflection** means a program can inspect and sometimes modify its own structure at run time: list a class's methods, build an instance from a name string, call a method by name.

| Language | Flavor |
| --- | --- |
| Java | `Class`, `Method`, used by frameworks and serializers |
| Python | `getattr`, `type`, everything is already open |
| C++ | Very little in the core language |

Useful for tools and frameworks. Easy to abuse: typos become run-time failures, and the compiler cannot check a name stored in a string. One awareness slide, not a project.

---

# Chapter 14 — Exception handling

An **exception** is an unusual event that the current code cannot finish through its normal path: divide by zero, missing file, failed network call, a violated precondition.

Without language support, people return error codes. Callers forget to check. The real work and the error path tangle.

```text
ok = open(file)
if not ok: return ERR_OPEN
ok = read(...)
if not ok: return ERR_READ
```

An exception facility lets one block **raise** (throw) and another **handle** (catch), often several calls away.

---

# What an exception facility must decide

| Design issue | Choices you will see |
| --- | --- |
| What can be thrown? | Any object? Only subtypes of `Throwable` / `Exception`? |
| How is it handled? | `try` / `catch` / `finally` (or `except` / `else` / `finally`) |
| Binding | Which handler catches this throw? Nearest matching type, often |
| Continuable? | After the handler, does the raise site resume? (Usually no — handler finishes, then continues after the `try`) |
| Checked or not? | Must the caller declare or catch it? (Java checked exceptions) |

```java
try {
    risky();
} catch (IOException e) {
    // handle this kind
} catch (Exception e) {
    // handle anything else that is still an Exception
} finally {
    // always runs — cleanup
}
```

```python
try:
    risky()
except ValueError as e:
    ...
finally:
    ...
```

```cpp
try {
    risky();
} catch (const std::exception& e) {
    ...
}
```

| Language | Note |
| --- | --- |
| Java | Checked exceptions must be declared or caught; `RuntimeException` is unchecked. `finally` for cleanup. Later: try-with-resources. |
| Python | Everything is unchecked. `except` matches by type. `else` runs if no exception. |
| C++ | Any type can be thrown historically; modern code throws `std::exception` types. No `finally` — RAII (destructors) for cleanup. Destructors during unwind are the cleanup story. |

**Propagation.** If a method does not catch, the exception goes to the caller, then that caller's caller, and so on. That is the point: the detection site and the recovery site can be far apart. If nothing catches it, the program typically terminates with a stack trace.

**User-defined exceptions.** A class that extends the language's exception base type. Throw that when your ADT's contract is broken (`pop` on an empty stack) instead of returning a magic value.

---

# Tracing exceptions (in class)

```text
main
  calls bank.withdraw(amount)
    calls account.debit(amount)
      throws InsufficientFunds
```

Questions:

1. If `debit` does not catch, who can?
2. If `withdraw` catches and logs, does `main` see the exception?
3. If there is a `finally` in `withdraw`, when does it run — before or after the handler body finishes transferring control?

---

# Chapter 13 — Concurrency (survey)

This is **one lecture survey**, not a parallel-programming course. You need vocabulary and two or three models. Lab work does not depend on writing multithreaded code.

**Why languages care.** Several tasks at once: overlapping I/O and computation, using multiple cores, or structuring a program as cooperating agents. The hard part is not starting tasks. The hard part is **shared state**.

| Danger | |
| --- | --- |
| Race | Two tasks update the same variable; result depends on timing |
| Deadlock | Each waits for a lock the other holds |
| Starvation | A task never gets a turn |

---

# Unit of concurrency

| Unit | Picture |
| --- | --- |
| **Process** | Heavyweight; own address space |
| **Thread** | Lighter; threads in one process **share** memory |

Most language-level concurrency you meet first is **thread-style** sharing inside one program. Erlang (and similar) prefers **processes** that do not share memory and talk by messages — that shows up in the mini assignment.

---

# Three classic mechanisms

## 1. Semaphores (brief)

A semaphore is a counter with **wait** (P) and **release** (V). Used to build locks and to limit how many tasks enter a section. Powerful, low-level, easy to get wrong. Languages often hide them under nicer forms.

## 2. Monitors

A **monitor** packages shared data with the operations that touch it. At most one task runs inside the monitor at a time. Waiting for a condition is done with condition variables (wait / signal), not with busy loops.

Think: an ADT whose methods are automatically mutually exclusive.

Java's `synchronized` methods and blocks are monitor-style. The object is the lock.

```java
synchronized void deposit(int n) {
    balance += n;
}
```

Only one thread at a time executes synchronized methods on that object. That prevents a lost update on `balance`. It does not by itself prevent deadlock if you lock two objects in different orders in different threads.

## 3. Message passing

Tasks do **not** share memory. They send messages. The receiver has a mailbox (queue). Synchronization is built into send/receive: you wait for a message instead of spinning on a flag.

```text
Account process:
  loop
    receive deposit(n) → balance := balance + n
    receive withdraw(n) → …
```

Erlang's model is this idea. Your bank-account mini assignment is a process with a balance and a message loop — no shared mutable variable across processes.

| Model | Shared memory? | Sync idea |
| --- | --- | --- |
| Threads + locks / monitors | Yes | Only one holder of the lock runs the critical section |
| Message passing | No (ideally) | Wait for a message; state lives inside one process |

Trade-off: shared-memory threads can be fast and familiar (and race-prone). Message passing pushes you to design clear protocols and copies (or ownership transfer) of data.

---

# How the three topics connect

| Topic | Connection |
| --- | --- |
| ADT / class | Hides representation; monitor methods are that idea plus mutual exclusion |
| Dynamic dispatch | Same call site, different object behavior — including "what happens on error" in subclass overrides |
| Exceptions | Separate normal path from failure path across call boundaries |
| Message passing | Failure and requests become messages; another way to separate concerns |

---

# In class

**Dispatch.** Given `Animal a = new Dog(); a.speak();` with `speak` overridden in `Dog`, which method runs in Java? In C++ if `speak` is not `virtual`?

**Exceptions.** Trace a throw from a nested call. Who catches? Does `finally` still run?

**Concurrency.** Two threads call `deposit` on the same account without synchronization. Invent an interleaving where money disappears. How does `synchronized` (or a single account process with messages) stop that?

---

# Lab 4

Continue Lab 4: `if`, a loop, and function definitions with **calls**.

A call is where a second environment frame earns its keep. Parameters live in the callee's scope. Exceptions in *your* language are optional stretch. Dynamic dispatch is not required unless your language has methods on objects by your own design.

---

# Week 11 wrap

You should be able to:

- List Sebesta's three requirements for OOP
- Explain inheritance vs subtype, and single vs multiple inheritance at a high level
- Trace a dynamically bound call and say what a vtable buys you
- Describe try / catch / finally (or the local spelling) and how an exception propagates
- Contrast monitors (shared memory + mutual exclusion) with message passing (no shared memory)

---

# Before next week

**Read:** Chapter 15, Sections 15.1–15.5 and 15.10–15.11 (functional programming)

**Lab 4:** keep building; functions are required for the Week 13 due date

---

# Quick check

1. A language has classes and inheritance, but every method call is fixed at compile time from the variable's type. Which of Sebesta's three OOP pillars is missing?
2. Why might `Square extends Rectangle` be a bad subtype even though it reuses code?
3. In Java, `Animal a = new Dog(); a.speak();` — who decides that `Dog.speak` runs, the compiler or the run-time type?
4. What does the vptr in an object point at?
5. What does `finally` (or a destructor in C++) give you that a `catch` alone does not?
6. Name one way checked exceptions in Java differ from exceptions in Python.
7. Two threads increment the same counter. What can go wrong, and what does a monitor-style lock change?
8. How does a message-passing account process avoid that race without a lock on a shared variable?
