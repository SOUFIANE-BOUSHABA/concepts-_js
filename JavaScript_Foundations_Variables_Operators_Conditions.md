# 🧠 JavaScript Foundations — Variables, Operators, Conditions

## How to use this document

This guide is built **step by step**: each part relies on the one before
it. Don't skip any section, even if it looks simple — everything you learn
here will be reused in later parts (and in the rest of the training).

1. **Variables** — store a value
2. **Types** — know what you stored
3. **Operators** — compare, calculate
4. **Conditions** — make a decision
5. **Nested Conditions** — make several linked decisions

---

# PART 1 — Variables

## 1.1 Why do we need variables?

A **variable** is a "box" where we store a piece of information, so we can
reuse it later without rewriting it every time.

```js
console.log("Hello Sara, you are 21 years old")
console.log("Sara got 15/20 on the exam")
// "Sara" and her age are repeated everywhere - if Sara changes,
// we'd have to rewrite everything by hand!
```

With variables, we store the information **once**:

```js
const firstName = "Sara"
const age = 21

console.log(`Hello ${firstName}, you are ${age} years old`)
console.log(`${firstName} got 15/20 on the exam`)
```

## 1.2 Declaring a variable: `let`, `const`, `var`

```js
let age = 20            // "let": the value CAN change later
const firstName = "Ali"  // "const": the value will NEVER change
var city = "Rabat"       // "var": the old way of writing this (avoid it)
```

### The essential test: will this value be reassigned?

```
   Will this value be reassigned later in the code?
             │
         YES │  NO
             │   │
             ▼   ▼
           let   const
```

```js
let age = 20
age = 21          // ✅ OK, let allows reassignment

const firstName = "Sara"
firstName = "Ali"  // ❌ ERROR: Assignment to constant variable.
```

### Golden rule to remember

**Always use `const` by default.** Only switch to `let` when you know the
value will need to change later. **Never** use `var` in modern code — it
still exists in older projects, but it causes subtle bugs that `let` and
`const` avoid (we'll see exactly why in the next section).

### Naming rules

```js
let myAge = 20            // ✅ camelCase (recommended)
let my_age = 20            // ⚠️ works, but not the JS convention
let 2ndName = "test"       // ❌ ERROR: cannot start with a digit
let my-age = 20            // ❌ ERROR: hyphen is forbidden (confused with subtraction)
```

---

# PART 2 — `var` vs `let` (and `const`) — A Closer Look

Since `var` still shows up in older code and tutorials, it's worth
understanding **exactly** why modern JavaScript avoids it. There are two
key differences: **scope** and **redeclaration**.

## 2.1 Difference #1 — Scope: block vs function

`let` and `const` are **block-scoped**: a variable only exists inside the
`{ }` block where it was declared (an `if`, a `for`, or any `{ }` pair).

`var` is **function-scoped** (or global if declared outside any function):
it completely ignores block boundaries like `if` or `for`.

```js
// WITH let
if (true) {
  let x = 10
  console.log(x) // 10, works fine - we're INSIDE the block
}
console.log(x) // ❌ ERROR: x is not defined - the block ended, x is gone


// WITH var
if (true) {
  var y = 10
  console.log(y) // 10
}
console.log(y) // 10 -> var "leaked" outside the if block!
```

### Visual schema

```
   if (true) {
     let x = 10     <- exists ONLY inside these { }
     var y = 10     <- "escapes" the block, exists outside too
   }

   console.log(x)   -> ❌ Error (x doesn't exist here)
   console.log(y)   -> ✅ 10 (var ignored the block entirely)
```

**Why this matters:** `var`'s behavior can cause confusing bugs, especially
inside loops and conditions, because a variable you thought was "local" to
a block is actually accessible (and modifiable) from far away in your
code.

## 2.2 Difference #2 — Redeclaration

`let` and `const` do **not** allow you to declare the same variable name
twice in the same scope. `var` does — silently.

```js
// WITH let
let age = 20
let age = 21   // ❌ ERROR: Identifier 'age' has already been declared


// WITH var
var age = 20
var age = 21   // ✅ no error at all - just silently overwrites the first one
console.log(age) // 21
```

**Why this matters:** with `let`, JavaScript immediately warns you if
you've accidentally reused a variable name (a common mistake in long
files). With `var`, the mistake goes completely unnoticed, and can quietly
overwrite data you needed.

## 2.3 Side-by-side summary

| | `var` | `let` | `const` |
|---|---|---|---|
| Scope | Function (ignores blocks) | Block (`{ }`) | Block (`{ }`) |
| Can be reassigned? | Yes | Yes | No |
| Can be redeclared in the same scope? | Yes (silently) | No (error) | No (error) |
| Recommended in modern code? | ❌ No | ✅ When the value changes | ✅ By default |

**Golden rule to remember:** you will occasionally see `var` in old
tutorials, Stack Overflow answers, or legacy codebases — recognize it and
understand it, but never write it yourself. Always reach for `const`
first, and `let` only when reassignment is genuinely needed.

---

# PART 3 — Data Types

## 3.1 Why types matter

A variable doesn't just hold "a value" — that value always has a **type**,
which determines what you're allowed to do with it (adding numbers works
differently than "adding" text, for example).

## 3.2 The primitive types

```js
let number = 42                // number    (integer or decimal)
let text = "Hello"              // string    (text, in quotes)
let isAdult = true              // boolean   (only true or false)
let nothing = null              // null      (absence of value, INTENTIONAL)
let notDefined                  // undefined (declared, but no value assigned yet)
```

### Visual schema

```
   let number = 42        →  type: number
   let text = "Hello"     →  type: string
   let isAdult = true     →  type: boolean
   let nothing = null     →  type: null (represents an intentional "empty")
   let notDefined         →  type: undefined (nothing was assigned)
```

## 3.3 Checking a variable's type with `typeof`

```js
console.log(typeof number)     // "number"
console.log(typeof text)       // "string"
console.log(typeof isAdult)    // "boolean"
console.log(typeof notDefined) // "undefined"
console.log(typeof nothing)    // "object"  <- a historical JS quirk!
```

⚠️ **Classic trap:** `typeof null` returns `"object"`, not `"null"`. This
is a well-known historical bug in JavaScript's design — memorize it as an
exception, not as something that needs to make logical sense.

## 3.4 `null` vs `undefined` — what's the difference?

```
   undefined  →  "nothing has been put here yet"  (default state)
   null       →  "I deliberately made this empty"  (explicit choice)
```

```js
let a           // undefined: the variable exists, but has no value yet
let b = null    // null: we explicitly said "this is empty"
```

---

# PART 4 — Operators

Now that we know how to store and identify values, let's see how to
**manipulate** and **compare** them.

## 4.1 Arithmetic operators

```js
console.log(5 + 3)   // 8    addition
console.log(5 - 3)   // 2    subtraction
console.log(5 * 3)   // 15   multiplication
console.log(5 / 2)   // 2.5  division
console.log(5 % 2)   // 1    modulo (remainder of the division)
```

### The modulo `%` — an operator that deserves its own explanation

Modulo gives you the **remainder** of a division, not the division's
result itself.

```
   5 % 2 = 1

   5 divided by 2 = 2, remainder 1
   (2 x 2 = 4, and 5 - 4 = 1 remaining)
```

**The most common use of modulo:** checking whether a number is even or
odd.

```js
console.log(10 % 2) // 0 -> even (divides exactly by 2)
console.log(7 % 2)  // 1 -> odd (1 left over)
```

## 4.2 Comparison operators

```js
console.log(5 > 3)   // true
console.log(5 < 3)   // false
console.log(5 >= 5)  // true
console.log(5 <= 4)  // false
```

### `==` vs `===` — the most important distinction in all of JavaScript

```js
console.log(5 == "5")   // true  -> compares only the VALUE (converts types)
console.log(5 === "5")  // false -> compares value AND type (no conversion)
```

```
   5 == "5"                          5 === "5"

   number 5   ↘                      number 5    ✗ different types
               converts "5" to 5                  (number vs string)
   string "5" ↗   -> compares 5 to 5  →  false
   → compares 5 to 5 → true
```

**Golden rule: always use `===` and `!==`**, never `==` or `!=`. Strict
operators avoid surprising bugs caused by automatic type conversion.

## 4.3 Logical operators

```js
console.log(true && false)  // false  AND: BOTH must be true
console.log(true || false)  // true   OR: AT LEAST ONE must be true
console.log(!true)          // false  NOT: flips the value
```

### Visual schema

```
   &&  (AND)                 ||  (OR)                  !  (NOT)

   true  && true  → true     true  || true  → true     !true  → false
   true  && false → false    true  || false → true      !false → true
   false && true  → false    false || true  → true
   false && false → false    false || false → false
```

**A concrete example combining several operators:**

```js
let age = 25
let goodEyesight = true

console.log(age >= 18 && goodEyesight)
// age >= 18 -> true (comparison)
// true && true -> true (logic)
// -> this person can take their driving test
```

---

# PART 5 — Conditions

Now that we can compare values, we can make the program **make
decisions** based on the result of those comparisons.

## 5.1 The `if` statement

```js
if (condition) {
  // code that runs ONLY if the condition is true
}
```

```js
let age = 20

if (age >= 18) {
  console.log("Adult")
}
```

### Visual schema

```
        ┌─────────────────┐
        │  age >= 18 ?      │
        └─────────┬─────────┘
             true  │  false
                   │    └──▶ nothing happens, execution continues after the block
                   ▼
        ┌─────────────────┐
        │  console.log(...) │
        └─────────────────┘
```

## 5.2 `if...else` — an alternative for the opposite case

```js
let age = 15

if (age >= 18) {
  console.log("Adult")
} else {
  console.log("Minor")
}
```

### Visual schema

```
        ┌─────────────────┐
        │  age >= 18 ?      │
        └─────────┬─────────┘
             true  │  false
                   ▼        ▼
        ┌──────────┐   ┌──────────┐
        │ "Adult"   │   │ "Minor"  │
        └──────────┘   └──────────┘
```

## 5.3 `if...else if...else` — several possible cases

When there are more than two possible outcomes, we add `else if` blocks
between the `if` and the final `else`.

```js
let grade = 14

if (grade >= 16) {
  console.log("Excellent")
} else if (grade >= 10) {
  console.log("Pass")
} else {
  console.log("Fail")
}
```

### Key point: evaluation order

JavaScript tests conditions **in order**, and stops as soon as one is
true — the remaining ones aren't even checked.

```
   grade = 14

   grade >= 16 ?  → 14 >= 16 → FALSE → move to the next condition
   grade >= 10 ?  → 14 >= 10 → TRUE  → "Pass" is printed, AND WE STOP HERE
   (the else is never checked)
```

⚠️ **Classic trap:** if you reverse the order of your conditions, the
result changes completely.

```js
// ❌ WRONG ORDER
if (grade >= 10) {
  console.log("Pass")       // 14 >= 10 is true -> STOPS HERE
} else if (grade >= 16) {
  console.log("Excellent")  // never checked, even if grade = 20 !
}
```

**Golden rule: when conditions overlap, always test the strictest (or most
specific) one first.**

## 5.4 Combining conditions and logical operators

```js
let age = 25
let goodEyesight = true

if (age >= 18 && goodEyesight) {
  console.log("Can take the driving test")
} else {
  console.log("Cannot take the driving test")
}
```

---

# PART 6 — Nested Conditions

## 6.1 What is a nested condition?

A **nested condition** is an `if` placed **inside** another `if`. It's
used when a second decision should only be made once the first condition
is already true.

```js
let age = 30
let isWeekend = true

if (age >= 18) {
  // we only get here if age >= 18 is true
  if (isWeekend) {
    console.log("Adult weekend rate")
  } else {
    console.log("Adult weekday rate")
  }
} else {
  console.log("Minor rate")
}
```

### Visual schema

```
        ┌─────────────────┐
        │  age >= 18 ?      │
        └─────────┬─────────┘
             true  │  false
                   │    └──▶ "Minor rate"
                   ▼
        ┌─────────────────┐
        │  isWeekend ?      │   <- this check only exists IF age >= 18 is true
        └─────────┬─────────┘
             true  │  false
                   ▼    ▼
      "Adult weekend    "Adult weekday
       rate"              rate"
```

**Why not just use `&&` here?** Because we have **two different
outcomes** depending on `isWeekend`, not a single yes/no answer. If we'd
only needed one question ("adult on the weekend, yes or no"), `&&` would
have been enough. But here, each branch gives its own detailed answer — so
we need a second level of decision-making.

## 6.2 Example with 3 levels of nested conditions

```js
let amount = 150
let promoCode = "SAS20"

if (amount >= 100) {
  if (promoCode === "SAS10") {
    console.log("10% discount")
  } else if (promoCode === "SAS20") {
    console.log("20% discount")
  } else {
    console.log("Invalid code")
  }
} else {
  console.log("Insufficient amount (minimum 100)")
}
```

### How to read code like this

```
   LEVEL 1: amount >= 100 ?
        │
        ├── NO → "Insufficient amount" (stops here, nothing else is checked)
        │
        └── YES → enter the block, then check LEVEL 2:
                │
                promoCode === "SAS10" ?
                     │
                     ├── YES → "10% discount"
                     │
                     └── NO → promoCode === "SAS20" ?
                                    │
                                    ├── YES → "20% discount"
                                    └── NO → "Invalid code"
```

**Crucial point to understand:** even if `promoCode` is perfectly valid
(`"SAS20"`), if `amount < 100`, we NEVER enter the block that checks the
code — the program stops directly at "Insufficient amount". The outer
condition "protects" everything inside it.

## 6.3 Watch your indentation

Indentation (the spaces at the start of each line) doesn't matter to
JavaScript — the code runs the same with or without it. But it's
**essential for humans** reading the code: it visually shows which block
is "inside" which other block.

```js
// ✅ Well indented - easy to read, levels are immediately visible
if (amount >= 100) {
  if (promoCode === "SAS20") {
    console.log("20% discount")
  }
}

// ❌ Poorly indented - runs the same, but unreadable
if (amount >= 100) {
if (promoCode === "SAS20") {
console.log("20% discount")
}
}
```

**Golden rule: every time you enter a new `{ }` block, add one level of
indentation (usually 2 spaces).**

---

## ✅ How Everything Fits Together

1. **Variables** — we store information (let/const)
2. **Types** — we know what we stored (number, string, boolean...)
3. **Operators** — we manipulate and compare that information (+, -, ===, &&...)
4. **Conditions** — we make a decision based on the result (if/else)
5. **Nesting** — we make several linked decisions

| Concept | Example | Remember this |
|---|---|---|
| `let` vs `const` | `const firstName = "Ali"` | `const` by default, `let` only if it must change |
| `var` vs `let` | `var` is function-scoped, `let` is block-scoped | Never use `var` in modern code |
| `typeof` | `typeof age` | Checks a variable's type |
| `%` (modulo) | `10 % 2 === 0` | Gives the remainder of a division, used to detect even/odd |
| `===` vs `==` | `5 === "5"` → `false` | Always use `===`, never `==` |
| `&&` / `||` / `!` | `age >= 18 && goodEyesight` | AND (both true) / OR (at least one true) / NOT (flips) |
| `if/else if/else` | Grading a test | The order of conditions matters a lot |
| Nested conditions | An `if` inside an `if` | An inner `if` is only checked in certain cases |

---

## 🧠 Quick Self-Check Questions

1. Why do we use `const` by default instead of `let`?
2. What's the difference in scope between `var` and `let`? Give a concrete
   example.
3. What does `typeof null` return, and why is that surprising?
4. What is the exact difference between `==` and `===`?
5. In an `if/else if/else`, what happens as soon as one condition is true?
6. Why is an `if` nested inside another `if` only checked in certain
   cases?

---

🎉 These six notions — variables, `var` vs `let`, types, operators,
conditions, and nested conditions — form the foundation that absolutely
everything else in JavaScript is built on (loops, functions, arrays,
objects). Take the time to really master them before moving forward: every
new concept you learn afterward will reuse them directly. Now it's time
for exercises!
