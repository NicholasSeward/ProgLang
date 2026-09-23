# Week 4 Practice — Static vs Dynamic Binding

**Reading:** Sebesta, Chapter 5 (binding, scope)  
**Time:** ~15–20 minutes  
**Submit:** LMS / discussion (short answers)

---

## Idea

A **binding** ties a name to something (here: a variable / value).

- **Static (lexical) binding** of a free name: decided from where the function is **written**.  
- **Dynamic binding** of a free name: decided from where the function is **called**.

You will predict what a small payroll helper computes under each rule, then rewrite so both rules agree.

---

## The program (meaningful, still small)

A shop computes **take-home pay** after tax.  
`net` uses a free variable `tax_rate` (not a parameter).

Do **not** run it yet. Treat as pseudocode.

```text
tax_rate = 0.10          // company default: 10%

function net(gross)
    // take-home = gross minus tax
    return gross - gross * tax_rate

function contractor_paycheck(gross)
    tax_rate = 0.25      // contractors taxed at 25% in this block
    return net(gross)

print(contractor_paycheck(1000))
```

What should a contractor take home on **$1000** gross?

That depends on **which `tax_rate` the name inside `net` is bound to**.

---

## Tasks

### 1) Static binding

If free names use **static (lexical) binding**, what does the program print?

**Why?** Which `tax_rate` is `net` bound to, and when was that binding established?

---

### 2) Dynamic binding

If free names use **dynamic binding**, what does the program print?

**Why?** Which `tax_rate` is `net` bound to when `contractor_paycheck` calls it?

---

### 3) Rewrite so both bindings agree

Rewrite the program so **static and dynamic binding produce the same take-home amount**.

Include:

- your rewritten code  
- the shared numeric result  
- **why** the rewrite removed the disagreement (parameters? nesting? renaming? no local `tax_rate`?)

More than one solution is fine — pick one and explain the bindings.

---

## Optional stretch

Translate your version into **Python** (static/lexical name binding).  
Does the result match task (1)?  
What does Python need (`global`, a parameter, a nested function) to match the “contractor 25%” story under static binding?
