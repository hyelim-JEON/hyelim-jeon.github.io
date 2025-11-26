---
title: "Map/Set vs Object/Array — Practical & Coding Interview Guide"
date: 2025-11-26
categories: [TIL]
tags: [JavaScript, Map, Set, Object, Array, Algorithm]
---

## 🧱 1) Map vs Object (Advanced Comparison)

### 🔎 Why Map is Powerful

- Keys are **not limited to strings** → can use objects, arrays, functions.
- Better performance when data is frequently added or removed.
- `size` is O(1) and expresses intent clearly.
- Easier iteration (`for...of`, `entries()`, `keys()`, `values()`).
- Maintains insertion order.

```js
const user = { id: 1 };
const cache = new Map();

cache.set(user, "userdata");
console.log(cache.get(user)); // "userdata" (object key supported)
```

### ⚠️ Weaknesses of Map

- Not straightforward to convert to/from JSON.
- Less intuitive for structured data modeling (especially with TypeScript).

---

### 🧾 Why Object Is Still Important

- JSON (API requests/responses) is essentially an Object.
- Ideal for defining data structures and models.
- Natural for nested and hierarchical data.

```ts
interface User {
  id: number;
  profile: { name: string; age: number };
}
```

### ⚠️ Weaknesses of Object

- Keys must be **string or symbol**.
- No built-in `size` → size check is O(n).
- Can be slow for frequent insert/delete operations.
- Requires helper functions to act like a hash map.

---

### 🏁 Practical Conclusion (Object vs Map)

| Use Case | Best Choice |
|----------|-------------|
| JSON, data modeling, typed structures | **Object** |
| Caching, quick lookups, dynamic keys, using objects as keys | **Map** |

---

## 🧮 2) Set vs Array (Advanced Comparison)

### 🔎 Why Set Is Useful

- Automatically removes duplicates.
- Fast membership check (`has()`) → almost O(1).
- Great for **visited states** (DFS/BFS), selection states, and flags.

```js
const visited = new Set();
function visit(n) {
  if (visited.has(n)) return;
  visited.add(n);
}
```

#### 📌 Natural Support for Math Operations (Intersection Example)

```js
const A = new Set([1,2,3]);
const B = new Set([2,3,4]);

const intersection = new Set([...A].filter(x => B.has(x)));
```

---

### 🧾 When Array Is Better

- Order matters.
- Sorting is needed.
- Need index-based access.
- Rendering UI lists (React) with predictable ordering.

### ⚠️ Weaknesses of Array

- `includes`, `filter`, `find` are O(n).
- Needs extra work to remove duplicates.

---

### 🏁 Practical Conclusion (Array vs Set)

| Use Case | Best Choice |
|----------|-------------|
| Fast lookup, duplicate removal, visited tracking | **Set** |
| Ordered lists, sorting, index access, UI rendering | **Array** |

---

## 💼 Real-World Examples

### 📌 React Multiple Selection  
#### ❌ Array — requires repeated `includes` + `filter`

```js
setSelected(prev => {
  if (prev.includes(id)) return prev.filter(v => v !== id);
  return [...prev, id];
});
```

#### ✅ Set — simpler and faster

```js
setSelected(prev => {
  const next = new Set(prev);
  next.has(id) ? next.delete(id) : next.add(id);
  return next;
});
```

---

### 📌 Server Cache  
#### ❌ Object — cannot use objects as keys

```js
let cacheObj = {};
```

#### ✅ Map — supports object keys + efficient lookup

```js
const cache = new Map();
function fetchUser(u) {
  if (cache.has(u)) return cache.get(u);
}
```

---

### 📌 Graph Traversal (DFS/BFS)

```js
const visited = new Set();
function dfs(node) {
  if (visited.has(node)) return;
  visited.add(node);
}
```

---

## 🧪 Coding Interview Summary

| Problem Type | Best Data Structure |
|--------------|--------------------|
| Frequency count, Two Sum, sliding window | **Map** |
| Duplicate check, visited tracking, BFS/DFS | **Set** |
| JSON processing, structured data | **Object + Array** |
| Caching, anagrams, optimized lookups | **Map + Set** |

---

## 🎯 TIL Summary

> **Object/Array → great for structured data (JSON).**  
> **Map/Set → great for fast lookup, duplicates, dynamic keys, caching & state management.**

🔥 Choosing the right data structure often determines the performance *and* the clarity of your code.
