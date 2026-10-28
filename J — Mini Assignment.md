# J — Mini Assignment

**Course:** Programming Languages  
**Accept assignment:** [classroom50.org — J](https://classroom50.org/Seward-Classes/programming-languages/assignments/j/accept)  
**Submit:** link to your Git repository (from the accept link above)

**Time:** about 2 hours. A few small tasks, not a project.

J is an array language. Kenneth Iverson designed APL, then he and Roger Hui designed J so the same ideas could be written in plain ASCII. You will not learn the whole language. You will run a handful of expressions, then write a few tiny verbs that use those ideas.

---

## Where to work

Accept the assignment. The repo is a J workspace. Open it in a **GitHub Codespace** and wait for the container setup to finish the first time. That install is J 9.7, syntax highlighting, and a Run Code play button for `.ijs` files. You do not install J yourself.

| Do this | How |
| --- | --- |
| Run the starter | Open `examples/features.ijs` and press the play button in the editor title bar. Output shows up in the terminal. |
| Run any script | Play button, or `bash .devcontainer/run-j.sh yourfile.ijs` |
| Try one line | In the terminal, run `jconsole`. Type an expression and press Enter. `exit 0` leaves it. |

A script does **not** echo each line the way `jconsole` does. Print with `smoutput`, as `examples/features.ijs` does. `":` turns a number into text you can put next to a label:

```j
smoutput 'average: ', ": avg 70 85 90 95
```

| Read this when a symbol is new | |
| --- | --- |
| **Learning J**, chapters 1–3 | [Basics](https://www.jsoftware.com/help/learning/01.htm), [lists](https://www.jsoftware.com/help/learning/02.htm), [defining functions](https://www.jsoftware.com/help/learning/03.htm) — Roger Stokes |
| **NuVoc** | [code.jsoftware.com/wiki/NuVoc](https://code.jsoftware.com/wiki/NuVoc) |

Examples below are `jconsole` sessions. The prompt is three spaces. You do not type the spaces.

---

## A few features worth knowing

### Nouns are arrays

A single number is an array. So is a list. You write a list by separating numbers with spaces.

```text
   5
5
   3 1 4 1 5
3 1 4 1 5
```

There is no scalar-versus-list split to manage. Arithmetic applies to whole arrays.

```text
   10 + 3 1 4
13 11 14
   1 2 3 * 10 20 30
10 40 90
```

### Right to left, one precedence

Every verb has the same precedence. Evaluation goes **right to left**. Parentheses still group.

```text
   2 * 3 + 4
14
```

That is `2 * (3 + 4)`, not `(2 * 3) + 4`. Same rule for power, where the order changes the answer:

```text
   2 ^ 3 ^ 2
512
```

`2 ^ (3 ^ 2)` is `2 ^ 9`. Left to right would have been `(2 ^ 3) ^ 2`, which is 64.

This is a language-design choice you already saw with APL: writability for array code, at a cost in readability if you expect C or Java precedence.

### The negative sign is `_`

`-` is always the verb "negate" or "subtract". A negative number is written with an underscore.

```text
   _5 + 2
_3
```

`_5` is the number. `- 5` is the verb `-` applied to `5`, which happens to produce the same number. Inside a list, use the underscore: `_1 0 1`.

### One symbol, two valences

A **verb** can be **monadic** (one argument, on the right) or **dyadic** (a left argument and a right argument). Same spelling. The arguments decide which meaning you get.

| Expression | Valence | Meaning | Result |
| --- | --- | --- | --- |
| `# 3 1 4 1 5` | monad | tally — how many items | `5` |
| `2 # 3 1 4` | dyad | copy — repeat each item | `3 3 1 1 4 4` |
| `i. 5` | monad | the integers `0 1 2 3 4` | `0 1 2 3 4` |

NuVoc lists both meanings on the same page. Read the one that matches the number of arguments you wrote.

### Adverbs build a new verb

J's parts of speech are **nouns**, **verbs**, **adverbs**, and **conjunctions**. You need the first three.

`+` is a verb. `/` is an adverb. It takes the verb on its left and produces a new verb, "insert between the items."

```text
   +/ 3 1 4 1 5
14
   */ 1 2 3 4
24
```

`+/` is sum. `*/` is product. You did not write a loop. The adverb did the repetition.

`>.` is "the larger of". `<.` is "the smaller of". Inserted, they are max and min:

```text
   >./ 3 9 4 1
9
   <./ 3 9 4 1
1
```

### A fork

A **fork** is three verbs in a row. On one argument `y`:

```text
(f g h) y    means    (f y) g (h y)
```

Average is sum divided by count. Sum is `+/`, divide is `%` (not `/` — slash is already the insert adverb), count is `#`.

```text
   avg =: +/ % #
   avg 70 85 90 95
85
```

`(=:)` assigns a name. Read `(+/ % #) y` as `(+/ y) % (# y)`.

That definition is **tacit**: the argument is not mentioned. You can also write an **explicit** verb, which is easier when the tacit form gets crowded. `3 :` means "monadic verb", and `y` is the argument:

```text
   range =: 3 : '(>./ y) - (<./ y)'
   range 3 9 4 1
8
```

Comments start with `NB.`

---

## Tasks

Run `examples/features.ijs` once so you have seen the starter. Then do the tasks in your own file, `tasks.ijs`, in the repo root. Leave the example in place.

Task 1 is a `jconsole` session: the console prints each result for you. Tasks 2 and 3 live in `tasks.ijs` and print with `smoutput`. Run that file with the play button.

### Task 1 — Predict, then run

Write the result you expect **before** you press Enter. Then run it. For any line you got wrong, add one sentence on which rule you missed (right-to-left, valence, or insert).

```text
2 * 3 + 4
10 - 4 - 3
(10 - 4) - 3
2 ^ 3 ^ 2
# 3 1 4 1 5
2 # 3 1 4
+/ 3 1 4 1 5
```

### Task 2 — Three verbs

Define each verb and show one call.

| Verb | Behavior | A call to show |
| --- | --- | --- |
| `avg` | average of a list | the fork `+/ % #` on `70 85 90 95` → `85` |
| `range` | maximum minus minimum | `range 3 9 4 1` → `8` |
| `evens` | keep the even numbers | `evens i. 10` → `0 2 4 6 8` |

`avg` must be the fork. `range` and `evens` may be explicit (`3 : '...'`).

For `evens`, residue is dyadic `|`, with the divisor on the left: `2 | y`. A boolean list can sit on the left of `#` to keep some items and drop others. Right-to-left reading matters. No `for.` and no `while.`

### Task 3 — Above average

Pick a list of at least **six** numbers and name it.

Define `above` so that `above scores` is how many items are **strictly greater** than `avg scores`. Use `avg`. Do not write a loop.

`smoutput` the list, `avg` of the list, and `above` of the list.

---

## What to put in the repo

Stay in the repo you accepted. Do not start a second one.

| Item | |
| --- | --- |
| `tasks.ijs` | `avg`, `range`, `evens`, and `above`, with `smoutput` for the calls |
| Task 1 notes | predictions and the `jconsole` results, in the README |
| README | how you ran J (Codespace), the reflection, the AI disclaimer |

### Reflection

1. What surprised you about J?
2. Which Sebesta criterion (readability, writability, reliability, cost) does it favor, and what does it give up?
3. How would you write `above` in a language you know, and what would be different?

### AI disclaimer

A short **AI / assistance** note: whether you used AI tools, what you used them for, and that you can explain every line you submit. Using AI is allowed. Submitting work you cannot explain is not.

---

## What to submit

1. Accept: https://classroom50.org/Seward-Classes/programming-languages/assignments/j/accept
2. Push to the repo that creates
3. Submit that **repository link** as directed on classroom50 / LMS

---

## Grading

| Result | Score |
| --- | --- |
| Meets the spec | **100** |
| Each minor issue | **−10** |
| More than **3** issues | **Kicked back** for another attempt |

### To spec

- Task 1 has a prediction written before the result, and a sentence on each miss
- `avg` is the fork `+/ % #`
- `range` and `evens` match the sample calls
- `above` uses the array, not a loop, and `tasks.ijs` prints the list, the average, and the count
- README has the reflection and the AI disclaimer

### Minor issues

- A prediction with no note about a wrong answer
- `evens` or `above` correct but only by an explicit loop
- `tasks.ijs` missing one of the verbs, or Task 1 with no `jconsole` results
- Reflection answers one question and skips the others

### Kick-back

- The verbs were not run (no `tasks.ijs`, or a script that prints nothing)
- `avg` is a hand-written loop or a call to a library average
- Cannot tell what you ran
- No AI disclaimer
