# 🔁🔁 Nested Loops in JavaScript — A Beginner's Guide

## What is a nested loop?

A **nested loop** is simply **a loop inside another loop**. We already know
that a loop repeats a block of code. A nested loop takes this one step
further: for **every single repetition** of the outer loop, the entire
inner loop runs from start to finish.

```js
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(`i=${i}, j=${j}`)
  }
}
```

Think of it like this: the **outer loop** is like the hours on a clock, and
the **inner loop** is like the minutes. For every 1 hour that passes, all 60
minutes have to pass first. Nested loops work the same way — the inner loop
must fully finish before the outer loop moves to its next step.

---

## Visual schema — how nested loops flow

```
 OUTER LOOP (i)                    INNER LOOP (j)

 ┌─────────────────┐
 │   i = 0           │
 └─────────┬─────────┘
           │
           ▼
 ┌─────────────────┐        ┌─────────────────────────┐
 │  is i < 3 ?       │  yes  │  j = 0                   │
 │                   ├──────▶│  is j < 3 ?  ──yes──▶ run│
 └─────────┬─────────┘       │  code, j++, repeat       │
      no   │                 │  until j < 3 is false     │
           ▼                 └────────────┬─────────────┘
        STOP                              │
           ▲                              ▼ (inner loop fully done)
           │                    ┌─────────────────────┐
           └────────────────────┤   i++  (go back up)  │
                                 └─────────────────────┘
```

**Key idea:** for `i = 0`, the inner loop runs completely (`j` goes
`0, 1, 2`). Only THEN does `i` become `1`, and the inner loop restarts
completely again (`j` goes `0, 1, 2` again). This repeats until the outer
loop is also done.

---

## Example 1 — Tracing through it step by step

```js
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(`i=${i}, j=${j}`)
  }
}
```

Output:
```
i=0, j=0
i=0, j=1
i=0, j=2
i=1, j=0
i=1, j=1
i=1, j=2
i=2, j=0
i=2, j=1
i=2, j=2
```

Notice the pattern: `i` stays the same while `j` counts all the way up
(`0, 1, 2`), and only after `j` is finished does `i` move to its next value.
This gives us `3 x 3 = 9` total lines — because for every 1 value of `i`,
we get 3 values of `j`.

```
     j=0  j=1  j=2
i=0   ✓    ✓    ✓      <- inner loop runs fully for i=0
i=1   ✓    ✓    ✓      <- then fully again for i=1
i=2   ✓    ✓    ✓      <- then fully again for i=2
```

---

## Example 2 — A multiplication table (a very common use case)

```js
for (let i = 1; i <= 3; i++) {
  for (let j = 1; j <= 3; j++) {
    console.log(`${i} x ${j} = ${i * j}`)
  }
}
```

Output:
```
1 x 1 = 1
1 x 2 = 2
1 x 3 = 3
2 x 1 = 2
2 x 2 = 4
2 x 3 = 6
3 x 1 = 3
3 x 2 = 6
3 x 3 = 9
```

For every row `i`, we go through every column `j` — exactly like a
multiplication table on paper.

```
        j=1     j=2     j=3
i=1 →  1x1=1   1x2=2   1x3=3
i=2 →  2x1=2   2x2=4   2x3=6
i=3 →  3x1=3   3x2=6   3x3=9
```

---

## Example 3 — Drawing a shape (a classic beginner exercise)

Nested loops are perfect for drawing patterns, because we control **rows**
with the outer loop and **columns** with the inner loop.

```js
for (let i = 1; i <= 5; i++) {
  let ligne = ""
  for (let j = 1; j <= i; j++) {
    ligne += "*"
  }
  console.log(ligne)
}
```

Output:
```
*
**
***
****
*****
```

**How it works:**
- The outer loop `i` controls which **row** we are on (1 to 5)
- The inner loop `j` controls how many `*` we add on that row — and since
  the inner loop only goes up to `i` (not a fixed number), each row gets
  **one more star than the row before it**.

```
i=1 -> inner loop runs 1 time  -> "*"
i=2 -> inner loop runs 2 times -> "**"
i=3 -> inner loop runs 3 times -> "***"
i=4 -> inner loop runs 4 times -> "****"
i=5 -> inner loop runs 5 times -> "*****"
```

---

## How many times does the code inside run?

This is one of the most important things to understand about nested loops:

> **Total repetitions = (number of times the outer loop runs) x (number of
> times the inner loop runs)**

```js
for (let i = 0; i < 4; i++) {      // runs 4 times
  for (let j = 0; j < 5; j++) {    // runs 5 times, for EACH i
    console.log("hello")
  }
}
// "hello" is printed 4 x 5 = 20 times
```

```
 outer loop:  i=0        i=1        i=2        i=3
                │          │          │          │
                ▼          ▼          ▼          ▼
 inner loop:  j:0-4      j:0-4      j:0-4      j:0-4
              (5 times)  (5 times)  (5 times)  (5 times)

              4 groups x 5 repetitions each = 20 total
```

⚠️ **Careful with performance:** if the outer loop runs 100 times and the
inner loop also runs 100 times, that's `100 x 100 = 10,000` repetitions in
total! Nested loops can get "expensive" very quickly as the numbers grow, so
always think about how many total repetitions you are creating.

---

## ⚠️ Common mistakes beginners make

### Mistake 1 — Reusing the same variable name for both loops

```js
// ❌ WRONG — both loops use "i", this creates confusing bugs
for (let i = 0; i < 3; i++) {
  for (let i = 0; i < 3; i++) {   // this "i" is NOT the same as the outer "i"!
    console.log(i)
  }
}
```

**Rule:** always give the inner loop a different variable name, usually
`j` (and `k` if you ever need a third nested loop). This is just a
convention, but a very important one to avoid confusion:

```js
// ✅ CORRECT
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(`i=${i}, j=${j}`)
  }
}
```

### Mistake 2 — Forgetting that the inner loop resets every time

A beginner might expect `j` to just keep counting up forever across all the
outer repetitions. In reality, **the inner loop starts fresh from its
initialization every single time** the outer loop runs.

```js
for (let i = 0; i < 2; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(`i=${i}, j=${j}`)
  }
}
```

Output:
```
i=0, j=0
i=0, j=1
i=0, j=2
i=1, j=0   <- j resets back to 0 here, it does NOT continue from 3!
i=1, j=1
i=1, j=2
```

---

## ✅ Summary

| Concept | Explanation |
|---|---|
| Nested loop | A loop placed inside another loop |
| Outer loop | Controls the "big" repetitions (often rows) |
| Inner loop | Runs completely for every single step of the outer loop (often columns) |
| Total repetitions | outer repetitions × inner repetitions |
| Variable names | Use a different letter for each loop (`i`, then `j`, then `k`) |
| Typical uses | Multiplication tables, shapes/patterns, grids, comparing every element of one list with every element of another |

---

## 🧠 Quick Self-Check Questions

1. In a nested loop, which one finishes completely first for each step: the
   outer loop or the inner loop?
2. If the outer loop runs 6 times and the inner loop runs 4 times, how many
   total repetitions happen inside the innermost code?
3. Why is it a bad idea to name both loop variables `i`?
4. Does the inner loop's variable keep increasing across outer loop
   repetitions, or does it reset each time?
5. In the star triangle example, why does each row get one more `*` than
   the row before it?

---

🎉 Nested loops might look confusing at first, but once you understand that
the inner loop simply "restarts fully" for every step of the outer loop,
you'll be able to build tables, shapes, grids, and much more. Now it's time
to practice!
