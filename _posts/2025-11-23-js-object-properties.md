---
title: "JavaScript: Arrays, Object Properties & Custom Data Structures"
date: 2025-11-23
categories: [TIL]
tags: [JavaScript, array, object, data structure]
---

Today I learned how arrays, object properties, and custom data structures work internally in JavaScript.

---

## 🔷 1. Object Literal `{}` vs `class { }`

| Feature | Object Literal `{}` | Class |
|--------|---------------------|-------|
| Purpose | Stores data once (one-time object) | Blueprint to create multiple objects |
| Creation | `const a = {}` | `new MyClass()` |
| Methods | Defined as properties | Defined inside the class body |
| Not a Class | `{}` **is not** a class | `class` defines a reusable structure |

> 📝 **Conclusion:** `{}` creates an *object literal*, not a class.

---

## 🔷 2. Property Keys and Access: `a[1]` vs `a.1`

When a property key is numeric:

```javascript
const obj = { 1: "hello" };
```

📌 Stored as a **string internally**.

✔ **Valid access:**
```javascript
obj[1];      // "hello"
obj["1"];    // "hello"
```

❌ **Invalid access:**
```javascript
obj.1; // SyntaxError
```

> 🔑 Keys that are not valid variable names must use **bracket notation**.

---

## 🔷 3. Why `push()` Returns the Length?

`push()` adds an item and returns the **new array length**, e.g.:

```javascript
const index = arr.push("apple") - 1; // index of newly added item
```

> ➡ Enables **chaining** and **automatic index calculation**.

---

## 🔷 4. Why `delete arr[i]` Is Wrong for Arrays

```javascript
delete arr[1];
```

✔ Removes the property  
❌ Does **not reduce length**  
❌ Leaves a **hole (empty slot)**

Example:

```javascript
console.log([10, , 30]); // hole remains
```

➡ **Correct deletion uses `splice()`:**

```javascript
arr.splice(1, 1);
```

---

## 🔷 5. Removing by Index Requires Shifting

To properly remove an element inside an array:

1. Move elements `i+1 → i`
2. Delete the duplicate at the end
3. Decrease `length`

> 🧠 This is how JavaScript arrays behave internally.

---

## 🔷 6. 🧱 Implementing a Custom Array: `MyArray`

This class does **not** use built-in array methods (`push`, `pop`, `shift`, `unshift`, `splice`, etc.).  
It manually manages length and properties.

```javascript
class MyArray {
  constructor() {
    this.length = 0;
  }

  push(value) {
    this[this.length] = value;
    this.length++;
    return this.length;
  }

  pop() {
    if (this.length === 0) return undefined;

    const idx = this.length - 1;
    const value = this[idx];
    delete this[idx];
    this.length--;
    return value;
  }

  shift() {
    if (this.length === 0) return undefined;

    const value = this[0];

    for (let i = 0; i < this.length - 1; i++) {
      this[i] = this[i + 1];
    }

    delete this[this.length - 1];
    this.length--;
    return value;
  }

  unshift(value) {
    for (let i = this.length; i > 0; i--) {
      this[i] = this[i - 1];
    }

    this[0] = value;
    this.length++;
    return this.length;
  }

  insertAt(index, value) {
    if (index < 0) index = 0;
    if (index > this.length) index = this.length;

    for (let i = this.length; i > index; i--) {
      this[i] = this[i - 1];
    }

    this[index] = value;
    this.length++;
  }

  removeAt(index) {
    if (index < 0 || index >= this.length) return undefined;

    const removed = this[index];

    for (let i = index; i < this.length - 1; i++) {
      this[i] = this[i + 1];
    }

    delete this[this.length - 1];
    this.length--;
    return removed;
  }

  get(index) {
    return index < 0 || index >= this.length ? undefined : this[index];
  }

  print() {
    const items = [];
    for (let i = 0; i < this.length; i++) items.push(this[i]);
    console.log(`[${items.join(", ")}] (length: ${this.length})`);
  }
}
```

---

## ✨ Key Takeaways

- `{}` creates **object literals**, not class instances.
- Numeric object keys are automatically converted into **strings**.
- `push()` returns **new length** for convenience and chaining.
- **Never use `delete` on arrays** → use shifting logic or `splice`.
- Understanding **internal shifting** helps build custom structures.

---

## 🧩 Optional Study Tasks

Try implementing inside `MyArray`:

🔸 `indexOf(value)` → return the index or `-1`  
🔸 `forEach(callback)` → loop and execute the callback

Questions to think about:

- Why must `insertAt()` iterate **backwards**?
- How are `shift()` and `removeAt(0)` logically identical?

