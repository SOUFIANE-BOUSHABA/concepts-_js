# 🧱 Objects in JavaScript — A Beginner's Guide

## What is an object?

An **object** is a way to group related information together, using
**named properties** instead of numbered positions (like an array does).

```js
const produit = {
  nom: "Clavier",
  prix: 150,
  quantite: 10,
};
```

Think of the difference like this: an **array** is like a numbered list
("item #0, item #1, item #2..."), while an **object** is like a form with
labeled fields ("Name: ___, Price: ___, Quantity: ___"). When each piece
of information has a clear *meaning* (a name, an age, a price...), an
object is usually a better fit than an array.

```
┌─────────────────────────────┐   ┌─────────────────────────────┐
│         ARRAY                 │   │        OBJECT                 │
│                               │   │                               │
│   ["Clavier", 150, 10]        │   │   {                            │
│      0        1    2          │   │     nom: "Clavier",            │
│                               │   │     prix: 150,                 │
│   -> positions have           │   │     quantite: 10               │
│      no built-in meaning      │   │   }                             │
│                               │   │   -> each value has a NAME     │
└─────────────────────────────┘   └─────────────────────────────┘
```

In this guide, we'll cover:

1. Creating an object and accessing its properties
2. Modifying, adding, and deleting properties
3. Methods (functions inside an object)
4. Looping through an object's properties
5. Nested objects (an object inside an object)
6. Arrays of objects
7. Objects containing arrays
8. Objects of objects
9. Array manipulation vs. object manipulation — the key differences

---

## 1. Creating an Object and Accessing Properties

An object is written between curly braces `{ }`, with `key: value` pairs
separated by commas.

```js
const apprenant = {
  prenom: "Sara",
  age: 21,
  ville: "Nador",
  estActif: true,
};
```

Each `key` (also called a **property name**) is like a label. You access
the value behind a label in two ways:

### Dot notation (the most common way)

```js
console.log(apprenant.prenom) // "Sara"
console.log(apprenant.age)    // 21
```

### Bracket notation (useful when the key is stored in a variable)

```js
console.log(apprenant["prenom"]) // "Sara"

const cle = "age"
console.log(apprenant[cle]) // 21 -> bracket notation lets you use a VARIABLE as the key
```

### Visual schema

```
   apprenant = {
     prenom: "Sara",   ─┐
     age: 21,           │  these are PROPERTIES (key: value pairs)
     ville: "Nador"    ─┘
   }

   apprenant.prenom       -> "Sara"   (dot notation)
   apprenant["prenom"]    -> "Sara"   (bracket notation, same result)
```

⚠️ **When to use bracket notation instead of dot notation:** dot notation
only works when you know the exact property name while writing the code.
Bracket notation is required when the property name is stored in a
variable, or contains special characters/spaces.

```js
const cleChoisie = "ville"
console.log(apprenant.cleChoisie)   // ❌ undefined - looks for a property literally named "cleChoisie"
console.log(apprenant[cleChoisie])  // ✅ "Nador" - uses the VALUE of cleChoisie as the key
```

---

## 2. Modifying, Adding, and Deleting Properties

```js
const produit = {
  nom: "Clavier",
  prix: 150,
};

produit.prix = 130          // MODIFY an existing property
produit.enPromo = true       // ADD a new property
delete produit.enPromo       // DELETE a property

console.log(produit) // { nom: "Clavier", prix: 130 }
```

### Visual schema

```
   produit = { nom: "Clavier", prix: 150 }
                              │
                produit.prix = 130
                              ▼
   produit = { nom: "Clavier", prix: 130 }
                              │
              produit.enPromo = true
                              ▼
   produit = { nom: "Clavier", prix: 130, enPromo: true }
                              │
              delete produit.enPromo
                              ▼
   produit = { nom: "Clavier", prix: 130 }
```

⚠️ Just like arrays, objects declared with `const` **can still be
modified** — `const` only prevents you from reassigning the whole variable
to a completely different object.

```js
const produit = { nom: "Clavier" }

produit.prix = 100          // ✅ OK - modifying content
produit = { nom: "Souris" }  // ❌ ERROR - reassigning the whole variable
```

---

## 3. Methods — Functions Inside an Object

A property can also be a **function**. When a function lives inside an
object, it's called a **method**.

```js
const apprenant = {
  prenom: "Karim",
  age: 24,
  sePresenter() {
    console.log(`Je m'appelle ${this.prenom} et j'ai ${this.age} ans`)
  },
}

apprenant.sePresenter() // "Je m'appelle Karim et j'ai 24 ans"
```

### What is `this`?

Inside a method, `this` refers to **the object the method belongs to**.
It's how the method can access the object's own properties.

```
   apprenant.sePresenter()
             │
             ▼
   inside sePresenter(), "this" = apprenant
             │
             ▼
   this.prenom  ->  apprenant.prenom  ->  "Karim"
```

---

## 4. Looping Through an Object's Properties

Since an object doesn't have numbered indexes like an array, you loop
through it differently — using `for...in`.

```js
const apprenant = {
  prenom: "Sara",
  age: 21,
  ville: "Nador",
}

for (const cle in apprenant) {
  console.log(`${cle} : ${apprenant[cle]}`)
}

// Output:
// prenom : Sara
// age : 21
// ville : Nador
```

### Visual schema

```
   for (const cle in apprenant)

   round 1: cle = "prenom"  -> apprenant["prenom"] -> "Sara"
   round 2: cle = "age"     -> apprenant["age"]    -> 21
   round 3: cle = "ville"   -> apprenant["ville"]  -> "Nador"
```

⚠️ Notice that inside the loop, you must use **bracket notation**
(`apprenant[cle]`), not dot notation. `cle` is a variable that changes
value each round, so `apprenant.cle` would look for a property literally
named `"cle"` (which doesn't exist) instead of using the variable's value.

---

## 5. Nested Objects — An Object Inside an Object

A property's value can itself be another object.

```js
const apprenant = {
  prenom: "Ilyas",
  age: 23,
  adresse: {
    ville: "Safi",
    codePostal: "46000",
  },
}

console.log(apprenant.prenom)         // "Ilyas"
console.log(apprenant.adresse.ville)  // "Safi"
```

### Visual schema

```
   apprenant = {
     prenom: "Ilyas",
     age: 23,
     adresse: {              <- this whole thing is ANOTHER object
       ville: "Safi",
       codePostal: "46000"
     }
   }

   apprenant.adresse         -> { ville: "Safi", codePostal: "46000" }
   apprenant.adresse.ville   -> "Safi"    (you "walk down" one level at a time)
```

---

## 6. Arrays of Objects

This is one of the most common patterns in real programs: a **list of
similar items**, where each item is represented as an object.

```js
const apprenants = [
  { prenom: "Sara", note: 15 },
  { prenom: "Ali", note: 9 },
  { prenom: "Karim", note: 17 },
]
```

### Visual schema

```
   apprenants[0]         -> { prenom: "Sara",  note: 15 }
   apprenants[1]         -> { prenom: "Ali",   note: 9 }
   apprenants[2]         -> { prenom: "Karim", note: 17 }

   apprenants[0].prenom  -> "Sara"
   apprenants[2].note    -> 17
```

### Looping through an array of objects

```js
for (let i = 0; i < apprenants.length; i++) {
  console.log(`${apprenants[i].prenom} : ${apprenants[i].note}`)
}

// Output:
// Sara : 15
// Ali : 9
// Karim : 17
```

Or with `for...of`, which is often cleaner here:

```js
for (const apprenant of apprenants) {
  console.log(`${apprenant.prenom} : ${apprenant.note}`)
}
```

### Searching inside an array of objects

```js
function trouverApprenant(tableau, prenom) {
  for (let i = 0; i < tableau.length; i++) {
    if (tableau[i].prenom === prenom) {
      return tableau[i]
    }
  }
  return null
}

console.log(trouverApprenant(apprenants, "Karim"))
// { prenom: "Karim", note: 17 }
```

---

## 7. Objects Containing Arrays

The opposite combination is just as common: an object where one of its
properties is an array.

```js
const apprenant = {
  prenom: "Yassine",
  notes: [12, 15, 9, 17],
}

console.log(apprenant.notes)      // [12, 15, 9, 17]
console.log(apprenant.notes[0])   // 12  (first note)
console.log(apprenant.notes.length) // 4  (number of notes)
```

### Visual schema

```
   apprenant = {
     prenom: "Yassine",
     notes: [12, 15, 9, 17]    <- this property's VALUE is an array
   }

   apprenant.notes       -> [12, 15, 9, 17]
   apprenant.notes[2]    -> 9     (third element of that array)
```

### A full example — combining a method with an array property

```js
const apprenant = {
  prenom: "Yassine",
  notes: [12, 15, 9, 17],
  calculerMoyenne() {
    let somme = 0
    for (let i = 0; i < this.notes.length; i++) {
      somme += this.notes[i]
    }
    return somme / this.notes.length
  },
}

console.log(apprenant.calculerMoyenne()) // 13.25
```

Notice `this.notes` inside the method — since `this` refers to
`apprenant`, `this.notes` reaches into the array stored on that object.

---

## 8. Array of Objects, Where Each Object Contains an Array

This combines everything above: a list of items (array), where each item
is an object (object), and one of that object's properties is itself a
list (array).

```js
const apprenants = [
  {
    prenom: "Sara",
    skills: ["HTML", "CSS", "JavaScript"],
  },
  {
    prenom: "Ali",
    skills: ["Python", "SQL"],
  },
  {
    prenom: "Karim",
    skills: ["JavaScript", "React", "Node", "MongoDB"],
  },
]
```

### Visual schema — reading the structure level by level

```
   apprenants                          <- ARRAY (a list of apprenants)
      │
      ├── apprenants[0]                <- OBJECT (Sara's data)
      │      ├── prenom: "Sara"
      │      └── skills: [...]         <- ARRAY (Sara's list of skills)
      │             ├── "HTML"
      │             ├── "CSS"
      │             └── "JavaScript"
      │
      ├── apprenants[1]                <- OBJECT (Ali's data)
      │      └── ...
      │
      └── apprenants[2]                <- OBJECT (Karim's data)
             └── ...
```

### Accessing a specific piece of data

```js
console.log(apprenants[0].prenom)      // "Sara"
console.log(apprenants[0].skills)      // ["HTML", "CSS", "JavaScript"]
console.log(apprenants[0].skills[1])   // "CSS"
console.log(apprenants[2].skills.length) // 4
```

### Looping through everything (nested loops)

```js
for (const apprenant of apprenants) {
  console.log(`${apprenant.prenom} :`)
  for (const skill of apprenant.skills) {
    console.log(`  - ${skill}`)
  }
}

// Output:
// Sara :
//   - HTML
//   - CSS
//   - JavaScript
// Ali :
//   - Python
//   - SQL
// Karim :
//   - JavaScript
//   - React
//   - Node
//   - MongoDB
```

**How to read this pattern:** the outer loop walks through the **list of
apprenants** (the array), and for each one, the inner loop walks through
**that apprenant's skills** (the array inside the object). This is exactly
the same nested-loop logic you've already used for grids and patterns —
just applied to real, named data instead of numbers.

---

## 9. Object of Objects

Instead of a numbered array, you can also group multiple objects inside
**one big object**, where each one is identified by a unique key (a name)
instead of a numeric index.

```js
const apprenants = {
  apprenant1: {
    prenom: "Sara",
    skills: ["HTML", "CSS", "JavaScript"],
  },
  apprenant2: {
    prenom: "Ali",
    skills: ["Python", "SQL"],
  },
}
```

### Accessing a specific apprenant

```js
console.log(apprenants.apprenant1.prenom)     // "Sara"
console.log(apprenants["apprenant2"].prenom)  // "Ali"
```

### Looping through an object of objects: use `for...in`, not `for...of`

```js
for (const cle in apprenants) {
  console.log(`${apprenants[cle].prenom} :`)
  for (const skill of apprenants[cle].skills) {
    console.log(`  - ${skill}`)
  }
}
```

⚠️ `for...of` does **not** work directly on a plain object — it's built
for arrays (and other "iterable" structures). Use `for...in` when the
outer structure is an object, and `for...of` when it's an array.

---

## 10. Array Manipulation vs. Object Manipulation — Key Differences

Even though arrays and objects can both hold multiple values, the tools
you use to work with them are different, because arrays are **ordered by
position** and objects are **organized by name**.

| Task | Array | Object |
|---|---|---|
| Access a value | `array[0]` (by numeric index) | `objet.cle` or `objet["cle"]` (by name) |
| Get the size | `array.length` | `Object.keys(objet).length` (no `.length` directly!) |
| Add something | `array.push(valeur)` | `objet.nouvelleCle = valeur` |
| Remove something | `array.splice(index, 1)` or `.pop()` | `delete objet.cle` |
| Loop through it | `for`, `for...of`, `.forEach()` | `for...in` |
| Typical use case | An ordered LIST of similar items | A single "record" with named fields |

```
   ARRAY                              OBJECT

   const fruits = ["pomme", "kiwi"]   const produit = { nom: "Clavier", prix: 150 }

   fruits[0]        -> "pomme"        produit.nom        -> "Clavier"
   fruits.push("x")  -> adds at end    produit.stock = 5   -> adds new property
   fruits.length     -> 2              Object.keys(produit).length -> 2
   for (const f of fruits)            for (const cle in produit)
```

**The one-sentence rule to remember:** if you're thinking "item #1, item
#2, item #3..." (order matters, same kind of thing repeated), use an
**array**. If you're thinking "this piece of data has a name" (like
`prenom`, `prix`, `age`), use an **object**. Very often, as you saw above,
you'll combine both: an array of objects, or an object with an array
inside it.

---

## ✅ Summary Table

| Concept | Example | What it does |
|---|---|---|
| Create an object | `{ nom: "Ali", age: 20 }` | Groups named properties together |
| Dot notation | `objet.nom` | Access a property by its exact name |
| Bracket notation | `objet["nom"]` or `objet[variable]` | Access a property, useful with a variable key |
| Add/modify | `objet.cle = valeur` | Adds the property if new, updates it if it exists |
| Delete | `delete objet.cle` | Removes a property completely |
| Method | `objet.faireQuelqueChose()` | A function stored as a property |
| `this` | Used inside a method | Refers to the object the method belongs to |
| `for...in` | Loops over an object's keys | The correct way to loop through an object |
| Array of objects | `[{...}, {...}, {...}]` | A list of similar "records" |
| Object containing an array | `{ notes: [12, 15, 9] }` | A record with a list-type field |
| Object of objects | `{ cle1: {...}, cle2: {...} }` | A named collection instead of a numbered one |

---

## 🧠 Quick Self-Check Questions

1. What's the main conceptual difference between an array and an object?
2. When would you need bracket notation (`objet[cle]`) instead of dot
   notation (`objet.cle`)?
3. Inside a method, what does `this` refer to?
4. Why doesn't `for...of` work directly on a plain object?
5. In `apprenants[0].skills[1]`, what does each part of that expression
   represent?

---

🎉 Objects are everywhere in real JavaScript code — user profiles, product
catalogs, API responses, configuration settings, you name it. Combining
arrays and objects (an array of objects, an object with arrays inside it)
is exactly how most real-world data is structured. Now it's time to
practice with exercises!
