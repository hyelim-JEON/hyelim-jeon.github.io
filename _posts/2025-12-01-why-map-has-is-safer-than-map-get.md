---
title: "Why map.has() Is Safer Than map.get() !== undefined in JavaScript"
date: 2025-12-01
categories: [TIL, JavaScript, Algorithm]
tags: [Map, HashTable, DuplicateCheck, JS]
---

## 🎯 Topic
Understanding why checking duplicates with `map.get(value) !== undefined` can be risky, and why `map.has(value)` is the safer and recommended approach in JavaScript.

---

## 🧠 1. How Map.get() Works

`Map.get(key)` returns:
- the stored value if the key exists
- `undefined` if the key does NOT exist

Example:

    const m = new Map();
    console.log(m.get(5)); // undefined (key does not exist)

This is why many beginners write:

    if (seen.get(value) !== undefined) {
      return value; // duplicate found
    }

This logic works *only* because:
- First time: `get(value)` → undefined
- Second time: `get(value)` → true (or some value)

---

## ⚠️ 2. Why This Is Dangerous

If someone stores an undefined value:

    seen.set(value, undefined);

Then:

    seen.get(value) !== undefined  // false

→ The key **exists**,  
→ But the returned value is still undefined,  
→ So your logic fails and **misses the duplicate**.

---

## 🟩 3. Correct Approach: Use map.has()

`map.has(key)` checks **only key existence**, not the stored value.

    if (seen.has(value)) {
      return value; // duplicate found
    }
    seen.set(value, true);

### ✔ Why this is safer?
- Works even if value is undefined, false, 0, "", or null
- Avoids confusing “missing key” vs “existing key with undefined value”
- Communicates intent clearly: you are checking existence, not value

---

## 🧑‍💻 4. Full Safe Implementation (Duplicate Finder)

    function findDuplicate(nums) {
      const seen = new Map();

      for (let n of nums) {
        if (seen.has(n)) return n;
        seen.set(n, true);
      }

      return undefined;
    }

Example:

    findDuplicate([2, 5, 1, 2, 3]); // 2
    findDuplicate([1, 2, 3]);      // undefined

---

## 📌 5. Summary Table

| Method                     | Meaning                     | Safe? | Recommended? |
|---------------------------|-----------------------------|-------|--------------|
| `map.get(key) !== undefined` | checks value to infer existence | ❌ No | ❌ Avoid      |
| `map.has(key)`           | checks key existence         | ✔ Yes | ⭐ Best       |

---

## 🚀 Final Insight

When using `Map` as a hash table in algorithms:

**Always use `map.has()` for existence checks.**  
It avoids edge cases, prevents subtle bugs, and expresses intent clearly.
