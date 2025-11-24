---
title: "JavaScript: split('') vs Spread (...) & Arrow Function Syntax"
date: 2025-11-24
categories: [TIL]
tags: [JavaScript, string, split, spread, arrow function]
---

Today I learned how JavaScript handles **string splitting**, how it compares with the **spread (`...`) operator**, and how arrow functions differ from normal functions.

---

## 🔹 `split('')` — Convert String to Character Array

`split()` splits a string using a separator.  
When using an **empty string (`''`)**, it splits into **individual characters**:

```javascript
"Hello".split(''); 
// ["H", "e", "l", "l", "o"]
```

📌 Other examples:

```javascript
"apple,banana,kiwi".split(",");
// ["apple", "banana", "kiwi"]

"2025-02-21".split("-");
// ["2025", "02", "21"]
```

---

## 🔸 Spread Operator `...` with Strings

The spread operator expands an iterable.  
Strings are iterable → spreading produces characters:

```javascript
[..."Hello"];
// ["H", "e", "l", "l", "o"]
```

### 🆚 `split('')` vs `[...str]`

| Feature | `split('')` | `[...str]` |
|--------|-------------|------------|
| Works only with strings | ✔ Yes | ❌ No (works on any iterable) |
| Ignores surrogate pairs | ❌ Sometimes | ✔ Better for emojis/Unicode |
| Requires separator | ✔ Yes | ❌ No |
| For emojis | ❌ Breaks | ✔ Correct |

🔍 Example with emojis:

```javascript
"👍🙂".split('');
// ["👍", "�", "�"]  🚫 broken!

[..."👍🙂"];
// ["👍", "🙂"]  ✅ correct
```

> 🎯 **Conclusion:** Spread is safer for Unicode/emojis.

---

## 🔵 Arrow Function vs Normal Function

### 📌 Normal Function

```javascript
function toChars(str) {
  return str.split('');
}
```

### 📌 Arrow Function (ES6)

```javascript
const toChars = str => str.split('');
```

### 🔑 Differences

| Feature | Normal Function | Arrow Function |
|--------|----------------|----------------|
| Has its own `this` | ✔ Yes | ❌ No (inherits `this`) |
| Can be used as constructor (`new`) | ✔ Yes | ❌ No |
| Short, concise syntax | ⚠ Longer | ✔ Very short |
| Good for object methods | ✔ Yes | ❌ No |
| Good for callbacks | ⚠ Sometimes | ✔ Best |

> 📝 **Arrow functions are shorter but do not have their own `this`.**

---

## ✨ Extra Spread Examples

```javascript
// Spread a string
console.log([... "ABC"]); 
// ["A", "B", "C"]

// Merge arrays
console.log([1, 2, ...[3, 4]]);
// [1, 2, 3, 4]

// Clone an array
const arr = [1, 2, 3];
const copy = [...arr];
```

---

## 🧠 Summary

- `split('')` splits a string into characters.
- Spread (`...`) also splits strings, but handles emojis correctly.
- Arrow functions are shorter but **don’t have their own `this`**.
- Spread works on any iterable, not just strings.

> ⚡ **Use `[...str]` for emojis and safe character splitting.**  
> ✨ **Use arrow functions for callbacks and short functions.**

