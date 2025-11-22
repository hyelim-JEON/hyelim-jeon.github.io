---
title: "JavaScript: this vs Arrow Functions"
date: 2025-02-21
categories: [TIL]
tags: [JavaScript, this, arrow function]
---

In JavaScript, `this` works differently in **normal functions** and **arrow functions**.

---

## 🔵 Normal Function & `this`

In a normal function, `this` depends on **how the function is called**, not where it is written.

```javascript
const obj = {
  a: 1,
  getA() {
    console.log(this.a);
  }
};

obj.getA(); // 1
```

> 📌 `this` refers to **obj** because the function is called as `obj.getA()`.

---

## 🟣 Arrow Function & `this`

Arrow functions **do not have their own `this`**.  
They inherit `this` from the **outer (lexical) scope**.

```javascript
const obj = {
  a: 1,
  getA: () => console.log(this.a)
};

obj.getA(); // undefined
```

> ⛔ Even inside an object, an arrow function **does not bind `this` to the object**.

---

## 🥇 Key Differences

| Feature | Normal Function | Arrow Function |
|--------|----------------|----------------|
| `this` | Depends on caller | Inherits from lexical scope |
| Best for object methods | ✔ Yes | ❌ No |
| Best for callbacks | ⚠ Sometimes | ✔ Yes |
| Can be used as constructor | ✔ Yes | ❌ No |

---

## 💬 Summary

- Normal functions get `this` from **how they are called**.
- Arrow functions get `this` from **where they are created**.

> ✨ **Use normal functions for object methods.**  
> ⚡ **Use arrow functions for callbacks.**
