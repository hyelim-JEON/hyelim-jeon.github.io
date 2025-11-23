---
title: "JavaScript: Class Inheritance & super() Explained"
date: 2025-11-22
categories: [TIL]
tags: [JavaScript, class, inheritance, super]
---

JavaScript classes support **inheritance**, allowing one class to extend another and reuse its functionality.  
The keyword `super()` plays a key role when working with inherited constructors.

---

## 🏗️ Basic Class Inheritance

A class can **extend** another using the `extends` keyword:

```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }
}

class Child extends Parent {}

const c = new Child("Hailey");
console.log(c.name); // "Hailey"
```

> 📌 The `Child` class automatically inherits the `Parent` constructor when no constructor is defined in `Child`.

---

## 🔑 What is `super()`?

When you add a constructor to the child class, the first thing you **must** do is call `super()`.

```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }
}

class Child extends Parent {
  constructor(name, grade) {
    super(name);   // 👈 calls Parent constructor
    this.grade = grade;
  }
}

const c = new Child("Hailey", "A");
console.log(c.name);  // "Hailey"
console.log(c.grade); // "A"
```

> 📌 `super(name)` passes the value to the parent constructor.

---

## ⚠️ Child Without Passing All Parent Parameters

You **don’t have to pass every parent parameter**, but any missing one becomes `undefined`.

```javascript
class Parent {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

class Child extends Parent {
  constructor(name) {
    super(name); // age not passed!
  }
}

const c = new Child("Hailey");
console.log(c.age); // undefined
```

> ❗ The field exists, but with no value → **undefined**.

---

## 🎁 Default Values Can Prevent undefined

To avoid undefined values, give parent parameters **default values**:

```javascript
class Parent {
  constructor(name, age = 0) {
    this.name = name;
    this.age = age;
  }
}
```

> 👉 Now `age` is `0` when not provided.

---

## 🧠 Summary

- `extends` allows a class to inherit from another.
- `super()` must be called **before using `this`** in child constructors.
- Missing parent parameters become **undefined**.
- Default values in parent constructors help avoid undefined.

> 🚀 **Use inheritance to reuse logic, and use `super()` to initialize the parent properly.**

