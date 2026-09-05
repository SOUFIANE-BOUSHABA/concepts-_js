# 📦 Array Manipulation in JavaScript — A Beginner's Guide

## What is an array?

An **array** is a single variable that can hold **multiple values**,
ordered in a list. Instead of creating 5 separate variables for 5 fruits,
you can store them all in one array.

```js
// Without an array (annoying!)
const fruit1 = "pomme"
const fruit2 = "banane"
const fruit3 = "orange"

// With an array (much better!)
const fruits = ["pomme", "banane", "orange"]
```

In this guide, we'll cover everything you need to **create, read, modify,
loop through, and transform** arrays:

1. Creating an array and accessing elements
2. Length and index
3. Adding and removing elements (`push`, `pop`, `unshift`, `shift`)
4. Looping through an array (`for` and `forEach`)
5. Searching inside an array (`includes`, `indexOf`, `find`)
6. Modifying with `splice`
7. Extracting a portion with `slice`
8. Sum, average, min, max
9. Arrays are mutable (the opposite of strings!)
10. A quick look at `map`, `filter`, `sort`

---

## 1. Creating an Array and Accessing Elements

Arrays are written between square brackets `[ ]`, with values separated by
commas.

```js
const fruits = ["pomme", "banane", "orange"]
```

Just like strings, each element has a **position**, called its **index**,
starting at `0`.

### Visual schema — indexing an array

```
   fruits = [ "pomme" ,  "banane" ,  "orange" ]
                │            │           │
              index 0     index 1     index 2
```

```js
console.log(fruits[0]) // "pomme"
console.log(fruits[1]) // "banane"
console.log(fruits[2]) // "orange"
console.log(fruits[5]) // undefined -> index does not exist!
```

---

## 2. Length and Index

```js
console.log(fruits.length) // 3
```

Just like with strings, the **last** index is always `length - 1`:

```js
console.log(fruits[fruits.length - 1]) // "orange" (the last element)
```

```
   [ "pomme" , "banane" , "orange" ]
       0          1          2
                              ↑
                      length - 1  (3 - 1 = 2)
                      -> this is always the LAST valid index
```

---

## 3. Adding and Removing Elements

There are 4 essential methods to add or remove elements — 2 for the END of
the array, and 2 for the BEGINNING.

```js
const fruits = ["pomme", "banane", "orange"]

fruits.push("fraise")   // add "fraise" at the END
console.log(fruits) // ["pomme", "banane", "orange", "fraise"]

fruits.pop()             // remove the LAST element
console.log(fruits) // ["pomme", "banane", "orange"]

fruits.unshift("kiwi")   // add "kiwi" at the BEGINNING
console.log(fruits) // ["kiwi", "pomme", "banane", "orange"]

fruits.shift()            // remove the FIRST element
console.log(fruits) // ["pomme", "banane", "orange"]
```

### Visual schema — remembering which is which

```
              unshift()                    push()
         (add at the START)          (add at the END)
                  │                          │
                  ▼                          ▼
         ┌─────┬─────┬─────┬─────┬─────┐
         │  ?  │pomme│banane│orange│  ?  │
         └─────┴─────┴─────┴─────┴─────┘
                  ▲                          ▲
                  │                          │
              shift()                     pop()
       (remove from START)          (remove from END)
```

**Memory trick:** `push`/`pop` work at the end (like a stack of plates you
push onto and pop off the top). `unshift`/`shift` "shift" everything to
make room at the beginning.

---

## 4. Looping Through an Array

### Option 1 — classic `for` loop (gives you the index too)

```js
const notes = [12, 15, 8, 17, 10]

for (let i = 0; i < notes.length; i++) {
  console.log(`Note ${i + 1} : ${notes[i]}`)
}
```

### Option 2 — `forEach` (simpler when you don't need to build something new)

```js
notes.forEach((note) => {
  console.log(note)
})
```

### Visual schema — how `forEach` works

```
   notes = [12, 15, 8, 17, 10]
             │
             ▼
   forEach calls your function ONCE for EACH element:

   call 1: note = 12  -> console.log(12)
   call 2: note = 15  -> console.log(15)
   call 3: note = 8   -> console.log(8)
   call 4: note = 17  -> console.log(17)
   call 5: note = 10  -> console.log(10)
```

`forEach` also gives you the index if you need it:

```js
notes.forEach((note, index) => {
  console.log(`Position ${index} : ${note}`)
})
```

---

## 5. Searching Inside an Array

### `includes()` — does the array contain something? (true/false)

```js
const fruits = ["pomme", "banane", "orange"]

console.log(fruits.includes("banane")) // true
console.log(fruits.includes("kiwi"))   // false
```

### `indexOf()` — WHERE is it? (returns the index, or -1)

```js
console.log(fruits.indexOf("banane")) // 1
console.log(fruits.indexOf("kiwi"))   // -1
```

### `find()` — search using a condition instead of an exact value

```js
const notes = [12, 15, 8, 17, 10]

const resultat = notes.find((note) => note > 14)
console.log(resultat) // 15  (the FIRST value that matches the condition)
```

### Visual schema — how `find` works

```
   notes.find(note => note > 14)

   check element 0: 12 > 14 ? -> false, continue
   check element 1: 15 > 14 ? -> TRUE! STOP HERE

   returns: 15
```

⚠️ If nothing matches, `find()` returns `undefined` (not `-1` like
`indexOf`).

```js
const resultat2 = notes.find((note) => note > 100)
console.log(resultat2) // undefined -> no value matches
```

---

## 6. Modifying an Array with `splice()`

`splice()` is a powerful (and slightly tricky) method that can **remove**,
**insert**, or **replace** elements anywhere in the array — not just at the
start or end.

### Structure

```js
array.splice(startIndex, deleteCount, item1, item2, ...)
```

### Removing elements

```js
const fruits = ["pomme", "banane", "orange", "kiwi"]

fruits.splice(1, 2) // starting at index 1, remove 2 elements
console.log(fruits) // ["pomme", "kiwi"]
```

```
   [ "pomme" , "banane" , "orange" , "kiwi" ]
       0           1          2        3

   splice(1, 2):
   start at index 1, remove 2 elements ("banane" and "orange")

   result: [ "pomme" , "kiwi" ]
```

### Inserting elements (without removing anything)

```js
const fruits = ["pomme", "orange"]

fruits.splice(1, 0, "banane") // at index 1, remove 0, insert "banane"
console.log(fruits) // ["pomme", "banane", "orange"]
```

### Replacing an element

```js
const fruits = ["pomme", "banane", "orange"]

fruits.splice(1, 1, "kiwi") // at index 1, remove 1, insert "kiwi"
console.log(fruits) // ["pomme", "kiwi", "orange"]
```

---

## 7. Extracting a Portion with `slice()`

Just like with strings, `slice(start, end)` extracts a portion **without
modifying** the original array. `end` is NOT included.

```js
const fruits = ["pomme", "banane", "orange", "kiwi", "fraise"]

console.log(fruits.slice(1, 3)) // ["banane", "orange"]
console.log(fruits)             // unchanged! still 5 elements
```

### ⚠️ `slice` vs `splice` — beginners mix these up constantly!

| | `slice()` | `splice()` |
|---|---|---|
| Changes the original array? | ❌ No, returns a new array | ✅ Yes, modifies it directly |
| Purpose | Extract/copy a portion | Remove/insert/replace elements |

```
   slice()  -> "give me a COPY of this part, don't touch the original"
   splice() -> "actually CUT/CHANGE the original array"
```

---

## 8. Sum, Average, Min, Max

These are classic exercises that combine a loop with a few variables.

```js
const notes = [12, 15, 8, 17, 10]

let somme = 0
let min = notes[0]
let max = notes[0]

for (let i = 0; i < notes.length; i++) {
  somme += notes[i]
  if (notes[i] < min) min = notes[i]
  if (notes[i] > max) max = notes[i]
}

const moyenne = somme / notes.length

console.log(`Somme: ${somme}`)     // 62
console.log(`Moyenne: ${moyenne}`) // 12.4
console.log(`Min: ${min}`)         // 8
console.log(`Max: ${max}`)         // 17
```

### Visual schema — tracing the min/max logic

```
   notes = [12, 15, 8, 17, 10]
   start: min = 12, max = 12

   i=0: notes[0]=12 -> already min/max
   i=1: notes[1]=15 -> 15 > max(12)? yes -> max becomes 15
   i=2: notes[2]=8  -> 8 < min(12)?  yes -> min becomes 8
   i=3: notes[3]=17 -> 17 > max(15)? yes -> max becomes 17
   i=4: notes[4]=10 -> no change

   final: min = 8, max = 17
```

---

## 9. Arrays Are Mutable (Unlike Strings!)

This is an important difference from strings: arrays **CAN** be changed
directly, without needing to reassign the variable.

```js
const fruits = ["pomme", "banane", "orange"]

fruits[0] = "kiwi" // this WORKS, even though fruits is a const!
console.log(fruits) // ["kiwi", "banane", "orange"]
```

⚠️ **Wait, isn't `fruits` a `const`?** Yes — but `const` only prevents you
from reassigning the **whole variable** to a completely different array. It
does NOT prevent you from changing what's **inside** the array.

```js
const fruits = ["pomme", "banane"]

fruits[0] = "kiwi"           // ✅ OK - modifying content
fruits.push("orange")        // ✅ OK - modifying content
fruits = ["autre", "tableau"] // ❌ ERROR - reassigning the whole variable
```

```
┌────────────────────────────────────┐
│  const fruits = [...]                │
│                                       │
│  ✅ allowed: change what's INSIDE     │
│     fruits[0] = "kiwi"                │
│     fruits.push(...)                  │
│                                       │
│  ❌ NOT allowed: replace the WHOLE    │
│     array with a different one        │
│     fruits = [...]                    │
└────────────────────────────────────┘
```

---

## 10. A Quick Look at `map`, `filter`, and `sort`

You don't need to master these yet, but it's good to recognize them.

### `map()` — transform every element into something new

```js
const nombres = [1, 2, 3, 4]
const doubles = nombres.map((n) => n * 2)
console.log(doubles) // [2, 4, 6, 8]
console.log(nombres) // [1, 2, 3, 4]  <- unchanged, map returns a NEW array
```

### `filter()` — keep only elements that pass a condition

```js
const nombres = [1, 2, 3, 4, 5, 6]
const pairs = nombres.filter((n) => n % 2 === 0)
console.log(pairs) // [2, 4, 6]
```

### `sort()` — sort the array (⚠️ modifies the original!)

```js
const nombres = [5, 2, 9, 1, 7]
nombres.sort((a, b) => a - b) // ascending order
console.log(nombres) // [1, 2, 5, 7, 9]
```

```
   map    -> "transform each element into something new"
   filter -> "keep only some elements, based on a condition"
   sort   -> "reorder the elements"
```

---

## ✅ Summary Table

| Method | What it does | Changes the original? |
|---|---|---|
| `.length` | Number of elements | — |
| `array[i]` | Access element at index `i` | — |
| `.push(x)` | Add `x` at the END | ✅ Yes |
| `.pop()` | Remove the LAST element | ✅ Yes |
| `.unshift(x)` | Add `x` at the BEGINNING | ✅ Yes |
| `.shift()` | Remove the FIRST element | ✅ Yes |
| `.includes(x)` | Does it contain `x`? (true/false) | ❌ No |
| `.indexOf(x)` | Position of `x` (-1 if not found) | ❌ No |
| `.find(condition)` | First element matching a condition | ❌ No |
| `.splice(start, count, ...)` | Remove/insert/replace elements | ✅ Yes |
| `.slice(start, end)` | Extract a portion (copy) | ❌ No |
| `.map(fn)` | Transform every element | ❌ No |
| `.filter(fn)` | Keep elements matching a condition | ❌ No |
| `.sort(fn)` | Reorder elements | ✅ Yes |
| `.forEach(fn)` | Run code for every element | ❌ No |

---

## 🧠 Quick Self-Check Questions

1. What is the difference between `push`/`pop` and `unshift`/`shift`?
2. Why does `fruits[fruits.length - 1]` always give you the last element?
3. What is the key difference between `slice()` and `splice()`?
4. If `fruits` is declared with `const`, why can you still do
   `fruits.push("kiwi")` without an error?
5. What's the difference between `find()` and `indexOf()` — when would you
   use one over the other?

---

🎉 Arrays are everywhere in real JavaScript programs — lists of users,
products, messages, scores, you name it. Mastering how to add, remove,
search, and transform them is one of the most valuable skills you'll build
as a developer. Now it's time to practice with exercises!
