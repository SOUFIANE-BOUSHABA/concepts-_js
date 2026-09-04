# 🔤 String Manipulation in JavaScript — A Beginner's Guide

## What is a string?

A **string** is simply text — a sequence of characters wrapped in quotes.

```js
const prenom = "Sara"
const message = 'Bonjour tout le monde'
const phrase = `JavaScript est genial`
```

You can use single quotes `' '`, double quotes `" "`, or backticks `` ` ` ``
— they all create a string. Backticks have a special superpower we'll see
later (template literals).

In this guide, we'll cover everything you need to **read, extract, search,
transform, and rebuild** strings:

1. Length and index
2. Accessing and looping through characters
3. Changing case (`toUpperCase`, `toLowerCase`)
4. Searching inside a string (`includes`, `indexOf`)
5. Extracting parts of a string (`slice`)
6. Cleaning up a string (`trim`)
7. Splitting and joining (`split`, `join`)
8. Replacing text (`replace`)
9. Template literals (building strings dynamically)
10. Strings are immutable

---

## 1. Length and Index

Every string has a **length** (how many characters it contains), and each
character has a **position**, called its **index**. Indexes always start
at `0`, not `1`.

```js
const mot = "Youcode"
console.log(mot.length) // 7
```

### Visual schema — indexing a string

```
   Y    o    u    c    o    d    e
   │    │    │    │    │    │    │
   0    1    2    3    4    5    6    <- indexes (start at 0!)

   mot.length = 7  (7 characters total)
```

```js
console.log(mot[0])              // "Y"  (the first character)
console.log(mot[3])              // "c"
console.log(mot[mot.length - 1]) // "e"  (the LAST character)
```

⚠️ **Why `mot.length - 1` for the last character?** Because indexes start
at `0`, the last valid index is always **one less** than the length.

```
length = 7, but the valid indexes are only 0, 1, 2, 3, 4, 5, 6
                                                            ↑
                                              this is length - 1
```

---

## 2. Looping Through a String

Since a string is a sequence of characters, you can loop through it just
like an array, using its `length` and index.

```js
const mot = "JS"

for (let i = 0; i < mot.length; i++) {
  console.log(mot[i])
}

// Output:
// J
// S
```

### Visual schema

```
   i=0 -> mot[0] -> "J" -> printed
   i=1 -> mot[1] -> "S" -> printed
   i=2 -> loop stops (i < mot.length is now false, since length is 2)
```

---

## 3. Changing Case: `toUpperCase()` and `toLowerCase()`

These two methods return a **new** string, converted to uppercase or
lowercase.

```js
const phrase = "Bonjour Youcode"

console.log(phrase.toUpperCase()) // "BONJOUR YOUCODE"
console.log(phrase.toLowerCase()) // "bonjour youcode"
console.log(phrase)               // "Bonjour Youcode"  <- unchanged!
```

### Visual schema

```
   "Bonjour Youcode"
          │
          ├── .toUpperCase() ──▶ "BONJOUR YOUCODE"  (a NEW string)
          │
          └── .toLowerCase() ──▶ "bonjour youcode"  (a NEW string)

   the original "phrase" variable is NEVER modified
```

**Why is this useful?** Mostly for comparing text without worrying about
capitalization:

```js
const reponseUtilisateur = "OUI"

if (reponseUtilisateur.toLowerCase() === "oui") {
  console.log("Reponse reconnue !")
}
// works whether the user typed "oui", "OUI", "Oui", or "OuI"
```

---

## 4. Searching Inside a String

### `includes()` — does the string contain something?

Returns `true` or `false`.

```js
const phrase = "Bienvenue a la SAS Youcode"

console.log(phrase.includes("SAS"))     // true
console.log(phrase.includes("Python"))  // false
```

### `indexOf()` — WHERE does something start?

Returns the index (position) where the text starts, or `-1` if not found.

```js
const phrase = "Bienvenue a la SAS Youcode"

console.log(phrase.indexOf("SAS"))      // 15
console.log(phrase.indexOf("Python"))   // -1  (not found)
```

### Visual schema

```
   B  i  e  n  v  e  n  u  e     a     l  a     S  A  S     Y  o  u  c  o  d  e
   0  1  2  3  4  5  6  7  8  9  10 11 12 13 14 15 16 17 18 19 ...

   "SAS" starts at index 15  ->  indexOf("SAS") returns 15
   "SAS" exists somewhere    ->  includes("SAS") returns true
```

---

## 5. Extracting Parts of a String: `slice()`

`slice(start, end)` cuts out a piece of the string, from index `start` up
to (but **not including**) index `end`.

```js
const phrase = "Bonjour Youcode"

console.log(phrase.slice(0, 7))  // "Bonjour"
console.log(phrase.slice(8))     // "Youcode"  (no end = goes to the end)
```

### Visual schema — this is the part beginners find trickiest

```
   B  o  n  j  o  u  r     Y  o  u  c  o  d  e
   0  1  2  3  4  5  6  7  8  9  10 11 12 13 14

   slice(0, 7):
   ├──────────────────┤
   0                   7 <- NOT included, cutting stops right before it
   result: "Bonjour"


   slice(8):
                        ├───────────────────────┤
                        8                        (until the end)
   result: "Youcode"
```

⚠️ **Golden rule:** the character AT the `end` index is never included —
`slice` stops **right before** it. This trips up almost every beginner at
first, so always double-check your end index by counting carefully.

```js
const mot = "JavaScript"
console.log(mot.slice(0, 4)) // "Java"  (indexes 0,1,2,3 — not 4!)
```

---

## 6. Cleaning Up a String: `trim()`

`trim()` removes extra spaces from the **beginning and end** of a string
(but not spaces in the middle).

```js
const saisie = "   Sara   "

console.log(saisie)          // "   Sara   "
console.log(saisie.trim())   // "Sara"
console.log(saisie.trim().length) // 4 (just "Sara")
```

**Why is this useful?** Very common when handling user input — someone
might accidentally type extra spaces before or after their answer, and
`trim()` cleans that up before you compare or store the value.

---

## 7. Splitting and Joining

### `split()` — turn a string into an array

```js
const phrase = "Bonjour Youcode SAS"
const mots = phrase.split(" ") // split wherever there's a space

console.log(mots) // ["Bonjour", "Youcode", "SAS"]
console.log(mots.length) // 3
```

### Visual schema

```
   "Bonjour Youcode SAS"
            │
            │  .split(" ")  -> cut at every space
            ▼
   ["Bonjour", "Youcode", "SAS"]
     [0]        [1]        [2]
```

You can split on any character, not just spaces:

```js
const date = "31-08-2026"
const parties = date.split("-")
console.log(parties) // ["31", "08", "2026"]
```

### `join()` — turn an array BACK into a string

`join()` does the opposite of `split()`.

```js
const mots = ["Bonjour", "Youcode", "SAS"]
console.log(mots.join(" "))  // "Bonjour Youcode SAS"
console.log(mots.join("-"))  // "Bonjour-Youcode-SAS"
console.log(mots.join(""))   // "BonjourYoucodeSAS"
```

```
   ["Bonjour", "Youcode", "SAS"]
            │
            │  .join(" ")  -> glue pieces together with a space
            ▼
   "Bonjour Youcode SAS"
```

---

## 8. Replacing Text: `replace()`

```js
const phrase = "J'aime Python"

console.log(phrase.replace("Python", "JavaScript"))
// "J'aime JavaScript"

console.log(phrase) // "J'aime Python"  <- unchanged, replace() returns a NEW string
```

⚠️ By default, `replace()` only replaces the **first** occurrence found:

```js
const phrase = "chat chat chat"
console.log(phrase.replace("chat", "chien"))
// "chien chat chat"  <- only the FIRST "chat" was replaced!
```

To replace **all** occurrences, use `replaceAll()`:

```js
console.log(phrase.replaceAll("chat", "chien"))
// "chien chien chien"
```

---

## 9. Template Literals — Building Strings Dynamically

Template literals use backticks `` ` ` `` and let you insert variables
directly inside a string using `${ }`.

```js
const prenom = "Sara"
const age = 21

// The old way (string concatenation with +):
console.log("Bonjour " + prenom + ", tu as " + age + " ans")

// The modern way (template literal):
console.log(`Bonjour ${prenom}, tu as ${age} ans`)

// Both print: "Bonjour Sara, tu as 21 ans"
```

### Visual schema

```
   `Bonjour ${prenom}, tu as ${age} ans`
             │                 │
             ▼                 ▼
        replaced by       replaced by
        the VALUE of      the VALUE of
        "prenom"          "age"
             │                 │
             ▼                 ▼
   "Bonjour Sara, tu as 21 ans"
```

You can even put small calculations inside `${ }`:

```js
const a = 5
const b = 3
console.log(`${a} + ${b} = ${a + b}`) // "5 + 3 = 8"
```

Template literals are almost always preferred over `+` concatenation
because they're easier to read, especially with multiple variables.

---

## 10. Strings Are Immutable

This is an important concept: once a string is created, **it can never be
changed**. Every method we've seen (`toUpperCase`, `slice`, `replace`...)
does NOT modify the original string — it always returns a **brand new**
string.

```js
let mot = "bonjour"

mot.toUpperCase()   // this creates a new string, but throws it away!
console.log(mot)    // still "bonjour" — unchanged!

mot = mot.toUpperCase() // you must REASSIGN it to actually keep the change
console.log(mot)    // now "BONJOUR"
```

### Visual schema

```
   let mot = "bonjour"
          │
          │  mot.toUpperCase()
          ▼
   creates "BONJOUR" somewhere in memory...
          │
          │  ...but if you don't store it anywhere, it's lost!
          ▼
   mot is STILL "bonjour"


   mot = mot.toUpperCase()
          │
          ▼
   NOW mot is reassigned to point to "BONJOUR"
```

⚠️ You also cannot change a single character directly:

```js
let mot = "bonjour"
mot[0] = "B" // ❌ this does NOTHING — strings can't be edited this way
console.log(mot) // still "bonjour"
```

To "change" a string, you always need to build a **new** string and
reassign it to the variable (or a new variable).

---

## ✅ Summary Table

| Method / Concept | What it does | Example |
|---|---|---|
| `.length` | Number of characters | `"abc".length` → `3` |
| `str[i]` | Character at index `i` | `"abc"[0]` → `"a"` |
| `.toUpperCase()` | Converts to UPPERCASE | `"abc".toUpperCase()` → `"ABC"` |
| `.toLowerCase()` | Converts to lowercase | `"ABC".toLowerCase()` → `"abc"` |
| `.includes(x)` | Does it contain `x`? (true/false) | `"abc".includes("b")` → `true` |
| `.indexOf(x)` | Position where `x` starts (-1 if not found) | `"abc".indexOf("b")` → `1` |
| `.slice(start, end)` | Extract part of the string | `"abcdef".slice(1, 4)` → `"bcd"` |
| `.trim()` | Remove spaces at start/end | `"  abc  ".trim()` → `"abc"` |
| `.split(x)` | Turn string into an array | `"a-b-c".split("-")` → `["a","b","c"]` |
| `.join(x)` (on arrays) | Turn array back into a string | `["a","b"].join("-")` → `"a-b"` |
| `.replace(x, y)` | Replace first occurrence of `x` with `y` | `"aa".replace("a","b")` → `"ba"` |
| `.replaceAll(x, y)` | Replace ALL occurrences | `"aa".replaceAll("a","b")` → `"bb"` |
| `` `${variable}` `` | Insert a variable inside a string | `` `Hello ${name}` `` |

---

## 🧠 Quick Self-Check Questions

1. Why is the last character of a string at index `length - 1`, not `length`?
2. What is the difference between `slice(0, 3)` and `slice(0, 4)`? Which one
   includes the character at index 3?
3. If you call `phrase.toUpperCase()` but don't store the result anywhere,
   what happens to `phrase`?
4. What's the difference between `replace()` and `replaceAll()`?
5. Why are template literals (`` ` ` ``) usually preferred over `+`
   concatenation?

---

🎉 String manipulation is something you'll use constantly as a developer —
validating user input, formatting text for display, searching through data,
and much more. Now it's time to practice with exercises!
