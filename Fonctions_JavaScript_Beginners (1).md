# 🧩 Functions in JavaScript — A Beginner's Guide

## Why do we need functions?

Imagine you need to calculate the sum of two numbers in five different
places in your program. Without functions, you would have to rewrite the
same calculation over and over again.

```js
console.log(5 + 3)
console.log(10 + 20)
console.log(7 + 2)
// ... same logic repeated everywhere
```

A **function** is a reusable block of code that performs a specific task.
You write the logic **once**, give it a name, and then you can **call** it
as many times as you want, with different values, without rewriting it.

```
┌─────────────────────────────────────┐
│   WITHOUT A FUNCTION                 │
│   console.log(5 + 3)                 │
│   console.log(10 + 20)   <- you      │
│   console.log(7 + 2)        repeat   │
│                              the same│
│                              logic   │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│   WITH A FUNCTION                    │
│   function addition(a, b) {          │
│     return a + b                     │
│   }                                  │
│                                       │
│   console.log(addition(5, 3))        │
│   console.log(addition(10, 20))      │
│   console.log(addition(7, 2))        │
│   -> the logic is written ONCE       │
└─────────────────────────────────────┘
```

In JavaScript, there are **3 main ways** to write a function:

1. Function declaration (the classic way)
2. Function expression (a function stored in a variable)
3. Arrow function (a shorter, modern syntax)

We will also cover **parameters, arguments, `return`**, and **scope**,
because you need these to fully understand how functions work.

---

## 1. Function Declaration (the classic way)

This is the most traditional way to write a function in JavaScript, using
the `function` keyword.

### Structure

```js
function functionName(parameter1, parameter2) {
  // code goes here
  return someValue
}
```

### Example

```js
function addition(a, b) {
  return a + b
}

console.log(addition(5, 3)) // 8
```

### Visual schema — what happens when you call a function

```
                 DEFINITION (written once)
        ┌─────────────────────────────────┐
        │  function addition(a, b) {       │
        │    return a + b                  │
        │  }                                │
        └─────────────────────────────────┘


                 CALL #1: addition(5, 3)
        ┌─────────────────────────────────┐
        │  a = 5, b = 3                     │
        │  return 5 + 3                     │
        │  -> gives back 8                  │
        └─────────────────────────────────┘


                 CALL #2: addition(10, 20)
        ┌─────────────────────────────────┐
        │  a = 10, b = 20                   │
        │  return 10 + 20                   │
        │  -> gives back 30                 │
        └─────────────────────────────────┘
```

Notice how the **same function** is reused with **different values** each
time it is called.

---

## 2. Parameters, Arguments, and `return`

These three words are used a lot when talking about functions, and
beginners often mix them up. Here is the difference:

| Term | What it means |
|---|---|
| **Parameter** | The name used INSIDE the function definition (like a placeholder) |
| **Argument** | The REAL value you send in when you call the function |
| **`return`** | Sends a value back out of the function, and immediately stops the function |

```js
//        parameters
//        ↓  ↓
function addition(a, b) {
  return a + b   // <- return sends the result back
}

console.log(addition(5, 3))
//                    ↑  ↑
//                arguments (the real values)
```

### ⚠️ Important: `return` vs `console.log`

A very common beginner mistake is confusing `return` with `console.log`.

```js
// ❌ This function does NOT give back a usable value
function addition(a, b) {
  console.log(a + b)  // only prints, does not return anything
}

let result = addition(5, 3)
console.log(result) // undefined !! because nothing was returned


// ✅ This function correctly returns a value
function addition(a, b) {
  return a + b
}

let result = addition(5, 3)
console.log(result) // 8, because the value was returned
```

**Rule of thumb:** use `return` when you want to **use the result later**
in your code. Use `console.log` only when you just want to **display**
something immediately.

### `return` also stops the function immediately

```js
function verifierAge(age) {
  if (age < 18) {
    return "Mineur" // the function stops HERE if this runs
  }
  return "Majeur" // this line only runs if the code above didn't return
}

console.log(verifierAge(15)) // "Mineur"
console.log(verifierAge(25)) // "Majeur"
```

---

## 3. Function Expression (a function stored in a variable)

Instead of using the `function` keyword directly, you can store a function
inside a variable (`const` or `let`).

### Structure

```js
const functionName = function (parameter1, parameter2) {
  // code goes here
  return someValue
}
```

### Example

```js
const addition = function (a, b) {
  return a + b
}

console.log(addition(5, 3)) // 8
```

### Function declaration vs function expression — what's the difference?

```
FUNCTION DECLARATION                FUNCTION EXPRESSION

function addition(a, b) {           const addition = function (a, b) {
  return a + b                        return a + b
}                                    }

✅ Can be called BEFORE it is        ❌ Can only be called AFTER
   written in the file (hoisting)       it is defined
```

```js
// This works fine with a function declaration:
console.log(addition(2, 3)) // 5
function addition(a, b) {
  return a + b
}


// This causes an ERROR with a function expression:
console.log(addition(2, 3)) // ❌ ReferenceError: Cannot access 'addition' before initialization
const addition = function (a, b) {
  return a + b
}
```

This behavior (being usable before it's written) is called **hoisting**,
and it's the main practical difference between the two. For beginners, the
simple rule is: **always define your function before you call it**, and
you'll avoid this problem entirely no matter which style you use.

---

## 4. Arrow Functions (the modern, shorter syntax)

Arrow functions are a more recent and shorter way to write functions,
using the `=>` symbol ("arrow").

### Structure (long version, same as a function expression)

```js
const functionName = (parameter1, parameter2) => {
  // code goes here
  return someValue
}
```

### Example

```js
const addition = (a, b) => {
  return a + b
}

console.log(addition(5, 3)) // 8
```

### The short version (implicit return)

If your function body is just **one single expression**, you can remove the
curly braces `{ }` and the `return` keyword — the value is returned
automatically.

```js
const addition = (a, b) => a + b

console.log(addition(5, 3)) // 8
```

### Visual schema — turning a classic function into an arrow function

```
STEP 1: classic function declaration
┌───────────────────────────────────┐
│ function addition(a, b) {          │
│   return a + b                     │
│ }                                  │
└───────────────────────────────────┘
                │
                ▼  store it in a variable, remove "function" keyword,
                   add "=>" after the parameters
┌───────────────────────────────────┐
│ const addition = (a, b) => {       │
│   return a + b                     │
│ }                                  │
└───────────────────────────────────┘
                │
                ▼  if the body is ONE line, remove { } and "return"
┌───────────────────────────────────┐
│ const addition = (a, b) => a + b   │
└───────────────────────────────────┘
```

### ⚠️ Careful: the short version ONLY works for a single expression

```js
// ✅ OK - single expression, implicit return works
const carre = (n) => n * n

// ❌ NOT OK - multiple lines need { } and an explicit "return"
const verifierAge = (age) => {
  if (age < 18) {
    return "Mineur"
  }
  return "Majeur"
}
```

### One parameter only? Parentheses are optional

```js
// Both of these work exactly the same way:
const carre = (n) => n * n
const carre2 = n => n * n
```

For beginners, it's recommended to **always keep the parentheses**, even
with one parameter — it makes the code more consistent and easier to read.

---

## 5. Scope — Where Does a Variable "Exist"?

A variable created **inside** a function only exists **inside** that
function. This is called the variable's **scope**.

```js
function exemple() {
  let message = "Bonjour"
  console.log(message) // works fine, we are INSIDE the function
}

exemple()
console.log(message) // ❌ ERROR: message is not defined
```

### Visual schema

```
┌─────────────────────────────────────────┐
│  OUTSIDE the function                     │
│  (global scope)                           │
│                                            │
│    ┌─────────────────────────────────┐   │
│    │  INSIDE the function              │   │
│    │  (local scope)                    │   │
│    │                                    │   │
│    │   let message = "Bonjour"          │   │
│    │   -> only visible in here          │   │
│    │                                    │   │
│    └─────────────────────────────────┘   │
│                                            │
│   console.log(message) ❌                 │
│   -> "message" does NOT exist out here    │
└─────────────────────────────────────────┘
```

**Why does this matter?** It means different functions can use the same
variable name without any conflict — each function has its own private
"workspace".

```js
function fonctionA() {
  let x = 10
  console.log(x) // 10
}

function fonctionB() {
  let x = 999 // a completely different "x", no conflict!
  console.log(x) // 999
}

fonctionA()
fonctionB()
```

---

## 6. Comparing All Three Styles Side by Side

```js
// 1. Function declaration
function addition(a, b) {
  return a + b
}

// 2. Function expression
const addition2 = function (a, b) {
  return a + b
}

// 3. Arrow function (long version)
const addition3 = (a, b) => {
  return a + b
}

// 3bis. Arrow function (short version)
const addition4 = (a, b) => a + b

// All 4 versions do exactly the same thing:
console.log(addition(5, 3))  // 8
console.log(addition2(5, 3)) // 8
console.log(addition3(5, 3)) // 8
console.log(addition4(5, 3)) // 8
```

---

## ✅ Summary Table

| Style | Syntax | When to use it |
|---|---|---|
| Function declaration | `function name() { }` | Classic, readable, can be called before it's defined in the file |
| Function expression | `const name = function() { }` | Useful when you want to store a function in a variable explicitly |
| Arrow function (long) | `const name = () => { return x }` | Modern syntax, same behavior as a function expression |
| Arrow function (short) | `const name = () => x` | Best for short, one-line functions (like simple calculations) |

| Concept | Explanation |
|---|---|
| Parameter | The placeholder name in the function definition |
| Argument | The real value passed in when calling the function |
| `return` | Sends a value back and stops the function immediately |
| Scope | A variable only exists inside the function where it was created |

---

## 🧠 Quick Self-Check Questions

Before moving on, make sure you can answer these:

1. What is the difference between a parameter and an argument?
2. What happens if a function uses `console.log` instead of `return`, and
   you try to store its result in a variable?
3. What is the main practical difference between a function declaration and
   a function expression?
4. When can you use the short (implicit return) version of an arrow
   function, and when can't you?
5. If you create a variable inside a function, can you use that variable
   outside the function afterward? Why or why not?

---

🎉 Functions are one of the most important building blocks in programming —
they let you organize your code, avoid repetition, and build more complex
programs step by step. Now it's time to practice with exercises!
