# 🔁 Loops in JavaScript — A Beginner's Guide

## Why do we need loops?

Most of the things we do in life are full of repetition. Imagine someone asks
you to print every number from 0 to 100 using `console.log()`. Doing that by
hand, one line at a time, would take forever and be extremely boring.

```js
console.log(0)
console.log(1)
console.log(2)
// ... and 98 more lines like this 😩
```

A **loop** is a programming tool that repeats a block of code automatically,
as many times as you need, without you having to write it over and over.

```
┌─────────────────────────────────────┐
│   WITHOUT A LOOP                     │
│   console.log(0)                     │
│   console.log(1)                     │
│   console.log(2)                     │
│   console.log(3)     <- you write    │
│   console.log(4)        every line   │
│   console.log(5)        by hand      │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│   WITH A LOOP                        │
│   for (let i = 0; i <= 5; i++) {     │
│     console.log(i)                   │
│   }                                  │
│   -> the computer repeats this       │
│      automatically for you           │
└─────────────────────────────────────┘
```

In this guide, we will cover the loops every JavaScript beginner must know:

1. `for` loop
2. `while` loop
3. `do...while` loop
4. `break`
5. `continue`

---

## 1. The `for` Loop

The `for` loop is the most commonly used loop. It is perfect when you **know
in advance** how many times you want to repeat something.

### Structure

```js
for (initialization; condition; increment/decrement) {
  // code goes here
}
```

A `for` loop has **3 parts**, separated by semicolons `;`:

| Part | What it does | When it runs |
|---|---|---|
| `initialization` | Creates the counter variable (usually `i`) | Runs **once**, at the very start |
| `condition` | Checks if the loop should keep going | Checked **before every** repetition |
| `increment/decrement` | Changes the counter (`i++`, `i--`, etc.) | Runs **after every** repetition |

### Visual schema — how it flows

```
        ┌─────────────────────┐
        │  let i = 0           │  <- runs ONCE (initialization)
        └──────────┬───────────┘
                   │
                   ▼
        ┌─────────────────────┐
   ┌───▶│   is i <= 5 ?        │  <- condition checked EVERY time
   │    └──────────┬───────────┘
   │          yes  │  no
   │               │        └────────────▶ STOP, exit the loop
   │               ▼
   │    ┌─────────────────────┐
   │    │  console.log(i)      │  <- the code inside { } runs
   │    └──────────┬───────────┘
   │               │
   │               ▼
   │    ┌─────────────────────┐
   └────┤  i++                 │  <- increment runs, THEN we go back up
        └─────────────────────┘
```

### Example 1 — Counting up

```js
for (let i = 0; i <= 5; i++) {
  console.log(i)
}

// Output: 0 1 2 3 4 5
```

**Step by step:**
- `let i = 0` → i starts at 0
- Is `0 <= 5`? Yes → print `0`
- `i++` → i becomes 1
- Is `1 <= 5`? Yes → print `1`
- ...and so on, until `i` becomes 6, where `6 <= 5` is `false`, so the loop stops.

### Example 2 — Counting down

```js
for (let i = 5; i >= 0; i--) {
  console.log(i)
}

// Output: 5 4 3 2 1 0
```

Here we start at `5` and use `i--` (decrement) to go down instead of up.

### Example 3 — Doing a calculation inside the loop

```js
for (let i = 0; i <= 5; i++) {
  console.log(`${i} * ${i} = ${i * i}`)
}
```

Output:
```
0 * 0 = 0
1 * 1 = 1
2 * 2 = 4
3 * 3 = 9
4 * 4 = 16
5 * 5 = 25
```

### ⚠️ The #1 mistake beginners make: the infinite loop

If you **forget** the increment (`i++`), the condition will **always stay
true**, and the loop will run forever. This can freeze or crash your program.

```js
// ❌ DANGER — infinite loop, i never changes!
for (let i = 0; i <= 5; ) {
  console.log(i)
  // i++ is missing here!
}
```

```
        ┌─────────────────────┐
   ┌───▶│   is i <= 5 ?        │
   │    └──────────┬───────────┘
   │           yes │
   │               ▼
   │    ┌─────────────────────┐
   │    │  console.log(i)      │
   │    └──────────┬───────────┘
   │               │
   └───────────────┘   <- i never changes, so this loops FOREVER 🔁💀
```

**Golden rule:** always double-check that whatever you use in the condition
is actually being updated inside the loop.

---

## 2. The `while` Loop

The `while` loop is used when you want to repeat something **as long as a
condition is true**. Unlike `for`, it does not have a built-in place for
initialization or increment — you have to write those yourself.

### Structure

```js
while (condition) {
  // code goes here
}
```

### Visual schema

```
        ┌─────────────────────┐
        │  let i = 0           │  <- you write this BEFORE the loop
        └──────────┬───────────┘
                   │
                   ▼
        ┌─────────────────────┐
   ┌───▶│   is i <= 5 ?        │
   │    └──────────┬───────────┘
   │          yes  │  no
   │               │        └────────────▶ STOP
   │               ▼
   │    ┌─────────────────────┐
   │    │  console.log(i)      │
   │    └──────────┬───────────┘
   │               │
   │               ▼
   │    ┌─────────────────────┐
   └────┤  i++                 │  <- you write this INSIDE the loop
        └─────────────────────┘
```

### Example

```js
let i = 0
while (i <= 5) {
  console.log(i)
  i++
}

// Output: 0 1 2 3 4 5
```

### When to use `while` instead of `for`?

Use `while` when you **don't know in advance** how many times the loop will
run. For example: "keep asking the user for input until they type the
correct password" — you don't know if that will take 1 try or 20 tries.

### ⚠️ Same danger as `for`: don't forget to update your variable!

```js
// ❌ DANGER — infinite loop, i never changes!
let i = 0
while (i <= 5) {
  console.log(i)
  // i++ is missing here!
}
```

If you forget `i++` inside a `while` loop, the condition `i <= 5` will
**always** be true, and your program will loop forever.

---

## 3. The `do...while` Loop

`do...while` is very similar to `while`, with **one key difference**:

> `do...while` always runs the code **at least once**, even if the condition
> is false from the very beginning.

### Structure

```js
do {
  // code goes here
} while (condition)
```

Notice that the condition is checked **at the end**, not at the beginning.

### Visual schema — the key difference

```
   WHILE LOOP                          DO...WHILE LOOP
   (checks condition FIRST)            (checks condition LAST)

   ┌─────────────┐                     ┌─────────────┐
   │  condition?  │                     │  run code    │
   └──────┬───────┘                     └──────┬───────┘
     yes  │  no                                │
          │   └──▶ STOP                        ▼
          ▼                            ┌─────────────┐
   ┌─────────────┐                     │  condition?  │
   │  run code    │                     └──────┬───────┘
   └──────┬───────┘                       yes  │  no
          │                                    │   └──▶ STOP
          └──▶ (back to condition)             │
                                                └──▶ (back to run code)
```

### Example — normal case (both loops behave the same)

```js
let i = 0
do {
  console.log(i)
  i++
} while (i <= 5)

// Output: 0 1 2 3 4 5
```

### Example — the case that shows the REAL difference

```js
let x = 100

// while: condition is false from the start, so the code NEVER runs
while (x < 10) {
  console.log("while:", x)
}
// (nothing is printed)

// do...while: the code runs ONE TIME, even though the condition is false
do {
  console.log("do while:", x)
} while (x < 10)
// Output: "do while: 100"
```

**Why does this matter?** Sometimes you need something to happen at least
once no matter what — for example, showing a menu to a user for the first
time, before you even know if they want to continue or stop.

---

## 4. `break` — Stopping a Loop Early

`break` is used to **immediately stop** a loop, even if the condition is
still true.

### Example

```js
for (let i = 0; i <= 5; i++) {
  if (i == 3) {
    break
  }
  console.log(i)
}

// Output: 0 1 2
```

### What happens step by step

```
i = 0 -> is i == 3? no -> print 0
i = 1 -> is i == 3? no -> print 1
i = 2 -> is i == 3? no -> print 2
i = 3 -> is i == 3? YES -> break! -> loop stops completely 🛑
```

Even though the loop's condition (`i <= 5`) would allow it to continue up to
5, `break` cuts it short as soon as `i` reaches `3`.

```
        ┌─────────────────────┐
        │   is i <= 5 ?        │
        └──────────┬───────────┘
                   │ yes
                   ▼
        ┌─────────────────────┐
        │   is i == 3 ?        │
        └──────┬──────────┬────┘
           yes │          │ no
               ▼          ▼
        ┌───────────┐  ┌─────────────┐
        │  break 🛑  │  │ console.log │
        │ (exit loop)│  └─────────────┘
        └───────────┘
```

---

## 5. `continue` — Skipping One Repetition

`continue` is used to **skip the current repetition** and jump straight to
the next one. Unlike `break`, it does NOT stop the whole loop — it only
skips one round.

### Example

```js
for (let i = 0; i <= 5; i++) {
  if (i == 3) {
    continue
  }
  console.log(i)
}

// Output: 0 1 2 4 5
```

Notice that `3` is missing from the output, but the loop still continues
all the way to `5`.

### What happens step by step

```
i = 0 -> is i == 3? no  -> print 0
i = 1 -> is i == 3? no  -> print 1
i = 2 -> is i == 3? no  -> print 2
i = 3 -> is i == 3? YES -> continue -> SKIP console.log, go to next i
i = 4 -> is i == 3? no  -> print 4
i = 5 -> is i == 3? no  -> print 5
```

### `break` vs `continue` — the key difference

```
  BREAK                              CONTINUE
  "Stop everything now"              "Skip just this one, keep going"

  0                                  0
  1                                  1
  2                                  2
  🛑 (loop ends here)                (3 is skipped)
                                     4
                                     5
```

---

## ✅ Summary Table

| Loop / Keyword | When to use it |
|---|---|
| `for` | You know exactly how many times to repeat (e.g. from 0 to 10) |
| `while` | You don't know in advance how many repetitions you'll need, and you want to check the condition BEFORE running the code |
| `do...while` | Like `while`, but you need the code to run AT LEAST ONCE, even if the condition is false |
| `break` | Stop the loop completely and immediately |
| `continue` | Skip the current repetition only, and move to the next one |

---

## 🧠 Quick Self-Check Questions

Before moving on, make sure you can answer these:

1. What are the 3 parts of a `for` loop, and in what order do they run?
2. What happens if you forget to increment your counter variable?
3. What is the ONE key difference between `while` and `do...while`?
4. If you use `break` when `i == 3`, will `i == 4` ever run? Why or why not?
5. If you use `continue` when `i == 3`, will `i == 4` still run? Why or why not?

---

🎉 Congratulations — you now understand how to repeat tasks automatically in
JavaScript instead of writing the same line of code over and over. This is
one of the most powerful tools you will use as a developer. Now it's time to
practice with exercises!
