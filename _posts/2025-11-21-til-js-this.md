
---
title: "JavaScript: Understanding `this` vs Arrow Functions"
date: 2025-02-21
categories: [TIL]
tags: [JavaScript, this, arrow function]
---

Today I learned the difference between how `this` works in **normal functions** and **arrow functions** in JavaScript.

---

## 🔵 Normal Function & `this`

In a normal function, `this` depends on **how the function is called**, not where it’s written.

```javascript
const obj = {
  a: 1,
  getA() {
    console.log(this.a);
  }
};

obj.getA(); // 1
📌 Here, this refers to obj because obj.getA() calls the function.

🟣 Arrow Function and this

Arrow functions do not have their own this.
They inherit this from their surrounding (outer) scope.

const obj = {
  a: 1,
  getA: () => console.log(this.a)
};

obj.getA(); // undefined


⛔ Even inside an object, an arrow function does not bind this to the object.

🥇 Key Differences
Feature	Normal Function	Arrow Function
this	Depends on caller	Inherits from outer scope
Object methods	✔ Good	❌ Not recommended
Callbacks	⚠ Sometimes	✔ Best choice

💬 Summary

Normal functions get this from how they are called.

Arrow functions get this from where they are created.

Use normal functions for object methods and arrow functions for callbacks.
