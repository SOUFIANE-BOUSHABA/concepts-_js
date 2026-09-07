# 🔁 for vs for...in vs for...of vs forEach — Choosing the Right Loop

## Why so many loops?

JavaScript gives you **4 different ways** to repeat code over a collection
of values, and each one exists for a specific situation. Beginners often
mix them up, so this guide focuses entirely on **when to use which one**,
and what each one can and cannot do.

1. `for` (the classic loop)
2. `for...in` (loops over keys)
3. `for...of` (loops over values)
4. `.forEach()` (an array method)

---

## 1. The Classic `for` Loop

You already know this one — it uses a counter (usually `i`) and gives you
full control: the starting point, the condition, and how the counter
changes.

```js
const fruits = ["pomme", "banane", "orange"]

for (let i = 0; i < fruits.length; i++) {
  console.log(i, fruits[i])
}

// Output:
// 0 pomme
// 1 banane
// 2 orange
```

### What makes `for` special

```
   for gives you BOTH:
   - the INDEX (i)          -> 0, 1, 2...
   - the VALUE (fruits[i])  -> "pomme", "banane", "orange"

   AND it supports break/continue freely.
```

**Use `for` when:** you need the index for something (numbering, comparing
neighboring elements, going backwards), or you need fine control over how
the counter changes (skipping by 2, going in reverse, etc.).

---

## 2. `for...in` — Loops Over Keys

`for...in` is designed for **objects**. It gives you each **key** (the
property name), one at a time.

```js
const apprenant = {
  prenom: "Sara",
  age: 21,
  ville: "Nador",
}

for (const cle in apprenant) {
  console.log(cle, ":", apprenant[cle])
}

// Output:
// prenom : Sara
// age : 21
// ville : Nador
```

### Visual schema

```
   for (const cle in apprenant)

   round 1: cle = "prenom"  -> apprenant[cle] -> "Sara"
   round 2: cle = "age"     -> apprenant[cle] -> 21
   round 3: cle = "ville"   -> apprenant[cle] -> "Nador"
```

⚠️ **`for...in` also works on arrays, but it gives you INDEXES (as
strings), not values:**

```js
const fruits = ["pomme", "banane", "orange"]

for (const i in fruits) {
  console.log(i, typeof i) // "0" string, "1" string, "2" string
}
```

**Golden rule:** even though `for...in` technically works on arrays, it's
designed for **objects**. Using it on arrays is considered bad practice —
use `for`, `for...of`, or `.forEach()` instead for arrays.

**Use `for...in` when:** you need to loop through the **properties of an
object**, and you don't know their names in advance.

---

## 3. `for...of` — Loops Over Values

`for...of` is designed for **arrays** (and other "iterable" things like
strings). It gives you each **value** directly, one at a time — no index,
no key, just the value.

```js
const fruits = ["pomme", "banane", "orange"]

for (const fruit of fruits) {
  console.log(fruit)
}

// Output:
// pomme
// banane
// orange
```

### Visual schema

```
   for (const fruit of fruits)

   round 1: fruit = "pomme"
   round 2: fruit = "banane"
   round 3: fruit = "orange"

   -> no index available directly, just the value
```

`for...of` also works on strings, going character by character:

```js
for (const lettre of "JS") {
  console.log(lettre)
}
// J
// S
```

⚠️ **`for...of` does NOT work directly on a plain object:**

```js
const apprenant = { prenom: "Sara", age: 21 }

for (const valeur of apprenant) {
  console.log(valeur) // ❌ TypeError: apprenant is not iterable
}
```

**Use `for...of` when:** you just need the **values** of an array (or the
characters of a string), and you don't care about the index.

---

## 4. `.forEach()` — An Array Method

`forEach()` isn't a loop keyword like the others — it's a **method**
available directly on arrays. It runs a function once for every element.

```js
const fruits = ["pomme", "banane", "orange"]

fruits.forEach((fruit) => {
  console.log(fruit)
})

// Output:
// pomme
// banane
// orange
```

It can also give you the index, as a second parameter:

```js
fruits.forEach((fruit, index) => {
  console.log(`${index + 1}. ${fruit}`)
})

// 1. pomme
// 2. banane
// 3. orange
```

### Visual schema

```
   fruits.forEach((fruit, index) => { ... })

   call 1: fruit="pomme",  index=0
   call 2: fruit="banane", index=1
   call 3: fruit="orange", index=2

   -> forEach calls your function ONCE per element, automatically
```

**Use `.forEach()` when:** you're working with an array, you just want to
**display or use** each value (not build a new array), and you don't need
`break`.

---

## 5. The Big One: `break` and `continue` Support

This is the single most important practical difference between these four
options, and it decides which one you MUST use in certain situations.

| Loop type | Supports `break` / `continue`? |
|---|---|
| `for` | Yes |
| `for...in` | Yes |
| `for...of` | Yes |
| `.forEach()` | **NO** |

```js
const fruits = ["pomme", "banane", "orange", "kiwi"]

// This throws a SyntaxError — break is NOT allowed inside forEach
fruits.forEach((fruit) => {
  if (fruit === "orange") {
    break // Illegal break statement
  }
  console.log(fruit)
})
```

```js
// Use for...of (or classic for) instead, if you need to stop early
for (const fruit of fruits) {
  if (fruit === "orange") {
    break
  }
  console.log(fruit)
}
// pomme
// banane
```

**Why does `forEach` not support `break`?** Because `forEach` isn't
actually a loop — it's a method that calls your function separately for
each element, like making several independent phone calls. You can't
"break" out of a series of phone calls the same way you can stop walking
through a hallway.

```
   for / for...of / for...in          forEach()

   ONE continuous loop,                 SEPARATE function calls,
   break exits it directly              one per element - nothing
                                        to "exit" from midway
```

**Golden rule:** if there's any chance you'll need `break` or `continue`
later, don't start with `forEach` — use `for` or `for...of` from the
beginning.

---

## 6. Side-by-Side Comparison

```js
const fruits = ["pomme", "banane", "orange"]

// 1. Classic for - gives you index AND value, supports break
for (let i = 0; i < fruits.length; i++) {
  console.log(i, fruits[i])
}

// 2. for...in - gives you the INDEX (as a string), meant for objects
for (const i in fruits) {
  console.log(i, fruits[i])
}

// 3. for...of - gives you the VALUE directly, meant for arrays
for (const fruit of fruits) {
  console.log(fruit)
}

// 4. forEach - gives you the VALUE (and optionally the index), no break
fruits.forEach((fruit, index) => {
  console.log(index, fruit)
})
```

---

## Summary Table

| | Designed for | Gives you | Supports `break`? | Typical use case |
|---|---|---|---|---|
| `for` | Arrays (or any counted repetition) | Index + value | Yes | Need the index, or full control over the counter |
| `for...in` | Objects | Keys (property names) | Yes | Loop through an object's properties |
| `for...of` | Arrays, strings | Values directly | Yes | Just need the values, no index needed |
| `.forEach()` | Arrays | Value (+ optional index) | No | Simple display/processing, no early exit needed |

### Decision guide

```
   Is it an OBJECT?
        |
       YES --> use for...in
        |
        NO (it's an array/string)
        |
        v
   Do you need to stop early with break?
        |
       YES --> use for  or  for...of
        |
        NO
        |
        v
   Do you need the index?
        |
       YES --> use for  or  forEach (with index param)
        |
        NO --> use for...of  or  forEach
```

---

## Quick Self-Check Questions

1. Which loop type is specifically designed for objects, and which one is
   designed for array values?
2. What happens if you try to use `break` inside a `.forEach()` call?
3. If you use `for...in` on an array instead of an object, what do you get
   back - the values, or the indexes?
4. If you need to stop looping as soon as you find a specific element, which
   of the 4 options can you use, and which one should you avoid?
5. What's the difference between what `for...of` gives you and what
   `.forEach()` gives you, when you don't ask for the index?

---

Choosing the right loop isn't about which one is "better" - it's about
matching the tool to the situation: object vs. array, needing the index or
not, and whether you might need to stop early. Now it's time to practice
with exercises!
