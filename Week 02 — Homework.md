# Week 2 Homework — Grammar Design

## Task

Write a **BNF or EBNF** grammar that:

- accepts **Program A**
- rejects **Program B**

Turn in your grammar and **one sentence** saying why B is invalid under it.

---

## Program A — must be **valid**

```
n = 10
sum = 0
i = 1
if i then
  sum = sum + i
  i = i + 1
print sum
if n then
  print n
  print sum
x = (sum + n) * 2
y = x - 1
print y
```

---

## Program B — must be **invalid**

```
n = 10
sum = 0
i = 1
while i do
  sum = sum + i
  i = i + 1
print sum
if n then
  print n
else
  print sum
fun double x =
  x * 2
z = double n
print z
a = b = c
```

