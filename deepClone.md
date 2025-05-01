## 🧠 JavaScript Deep Clone Cheat Sheet

### Comparison Table

| Feature/Aspect                    | `structuredClone()` ✅ | `JSON.parse(JSON.stringify())` ⚠️ | `_.cloneDeep()` (Lodash) ⚠️ |
|----------------------------------|----------------------------|-------------------------------------------|----------------------------------|
| Deep clone support               | ✅ Yes                  | ✅ Yes                                   | ✅ Yes                        |
| Circular references              | ✅ Supported           | ❌ Throws error                          | ✅ Supported                  |
| Functions                        | ❌ Not cloned          | ❌ Removed                                | ❌ Removed                    |
| Date                             | ✅ Preserved           | ❌ Converted to string                   | ✅ Preserved                  |
| RegExp                           | ✅ Preserved           | ❌ Removed                                | ✅ Preserved                  |
| Map / Set                        | ✅ Preserved           | ❌ Removed                                | ✅ Preserved                  |
| Typed Arrays / Buffers           | ✅ Preserved           | ❌ Removed                                | ⚠️ Partial              |
| SharedArrayBuffer, File, Blob    | ✅ Supported           | ❌ Not supported                          | ❌ Not supported              |
| Undefined in array               | ✅ Preserved           | ❌ Converted to `null`                   | ✅ Preserved                  |
| Symbol keys                      | ✅ Preserved           | ❌ Lost                                   | ✅ Preserved                  |
| Performance                      | ⚡️ Fast (native) | 🙃 Fast, but limited                | 🙃 Slow for nested objs |
| Browser/Node support             | ✅ Node 17+, modern browsers | ✅ Everywhere                        | ✅ Everywhere                |
| Dependency                       | ❌ None                | ❌ None                                  | ✅ Needs lodash               |

---

### 🔥 Examples

#### 1. Circular Reference
```js
const a = {};
a.self = a;

structuredClone(a); // ✅ Works
JSON.parse(JSON.stringify(a)); // ❌ TypeError
_.cloneDeep(a); // ✅ Works
```

#### 2. Date Object
```js
const obj = { d: new Date() };

structuredClone(obj); // ✅ Date preserved
JSON.parse(JSON.stringify(obj)); // ❌ Becomes string
_.cloneDeep(obj); // ✅ Date preserved
```

#### 3. Functions
```js
const obj = { fn: () => {} };

structuredClone(obj); // ❌ fn removed
JSON.parse(JSON.stringify(obj)); // ❌ fn removed
_.cloneDeep(obj); // ❌ fn removed
```

---

### ✅ When to Use `structuredClone()`

| Use Case                                         | Go For It? |
|--------------------------------------------------|------------|
| Native deep cloning in modern environments       | ✅ Yes     |
| Handling circular refs, Date, RegExp, Map, Set   | ✅ Yes     |
| Cloning without external dependencies            | ✅ Yes     |
| Need to clone File, Blob, ArrayBuffer, etc.      | ✅ Yes     |
| Want predictable and consistent cloning          | ✅ Yes     |
| Need to support legacy browsers (IE, old Node)   | ❌ Use Lodash |

---

### ✍️ TL;DR

- ✅ Use **`structuredClone()`** for modern, spec-compliant, native deep cloning.
- ⚠️ Use **Lodash `cloneDeep()`** for legacy or large-scale compatibility.
- ❌ Avoid `JSON.parse(JSON.stringify(...))` for cloning — it's **lossy and risky**.

---

Let me know if you need code snippets or test cases to accompany this cheat sheet!

