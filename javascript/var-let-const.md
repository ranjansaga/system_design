
# JavaScript `var`, `let`, and `const` Cheatsheet

## 📌 Scope and Global Object Attachment

| Feature               | `var`                     | `let`                     | `const`                   |
|-----------------------|---------------------------|---------------------------|----------------------------|
| Scope                 | Function or Global        | Block                     | Block                      |
| Hoisting              | ✅ Yes (initialized as `undefined`) | ✅ Yes (in Temporal Dead Zone) | ✅ Yes (in Temporal Dead Zone) |
| Re-declaration        | ✅ Allowed                | ❌ Not Allowed             | ❌ Not Allowed              |
| Re-assignment         | ✅ Allowed                | ✅ Allowed                 | ❌ Not Allowed              |
| Attaches to `window` (global object) | ✅ Yes (if global)        | ❌ No                      | ❌ No                       |
| Use in ES6 Modules    | ✅ Avoid                  | ✅ Preferred               | ✅ Preferred                |

---

## 🧠 Key Behaviors

### 1. Global Scope Attachment

```js
var name = "Ranjan";
console.log(window.name); // "Ranjan"

let age = 30;
console.log(window.age); // undefined
```

### 2. Arrow Function `this` Context

```js
let name = "Lexical";
var obj = {
  name: "Ranjan",
  print: () => console.log(this.name)
};
obj.print(); // "" or undefined (depends on environment)
```

```js
var name = "GlobalVar";
obj.print(); // "GlobalVar" (from window.name)
```

### 3. Regular Function `this` Context

```js
var obj = {
  name: "Ranjan",
  print: function () {
    console.log(this.name);
  }
};
obj.print(); // "Ranjan"
```

---

## 🔥 Temporal Dead Zone (TDZ)

```js
function test() {
  console.log(a); // ❌ ReferenceError
  let a = 10;
}
```

---

## ✅ Best Practices

- Prefer `let` and `const` over `var` in modern JavaScript.
- Use `const` by default; use `let` if you need to reassign.
- Avoid global `var` to reduce accidental global pollution.

---

## ✅ Key Takeaways: `var`, `let`, and Global Scope Behavior

### 🔹 `var x = 10` in Global Scope
- Declares `x` in the global scope.
- `x` becomes a **non-configurable** property of the `window` object.
- Accessible via `window.x` ✅
- Cannot be deleted:  
  ```js
  delete x; // false
  ```
- Hoisted with an initial value of `undefined`.

---

### 🔹 `x = 10` (Implicit Global)
- If no `var`, `let`, or `const` is used, JavaScript implicitly creates a global variable.
- Becomes a **configurable** property on the `window` object.
- Can be deleted:  
  ```js
  delete x; // true
  ```
- Considered bad practice (can accidentally leak variables to global scope).
- Not hoisted — runs only when the line executes.

---

### 🔹 `let x = 10` in Global Scope
- Creates a variable in the global scope, but **not** attached to `window`.
- `window.x` is `undefined`.
- Hoisted but in **Temporal Dead Zone (TDZ)** until declaration.
- Safer and recommended over `var` or implicit globals.
- Cannot be re-declared in the same scope.

---

### 🔹 Why `var` Shows Up on `window` but `let` Doesn’t
- `var` declarations at global scope become properties of `window`.
- `let` and `const` create bindings in the global **lexical environment**, but **not as `window` properties**.
- `var` is part of the old spec (ES3/ES5), `let`/`const` were introduced in ES6.

---

### 🔹 Example Recap
```js
var a = 1;
x = 2;
let b = 3;

console.log(window.a); // 1 ✅
console.log(window.x); // 2 ✅
console.log(window.b); // undefined ❌
```

---

### 🔹 Table Comparison

| Feature                          | `var x = 10`       | `x = 10` (undeclared) | `let x = 10`     |
|----------------------------------|---------------------|------------------------|-------------------|
| Becomes `window.x`              | ✅ Yes              | ✅ Yes                 | ❌ No             |
| Configurable                    | ❌ No               | ✅ Yes                | ✅ (non-window)   |
| Can be deleted                  | ❌ No               | ✅ Yes                | ✅ (non-window)   |
| Hoisting                        | ✅ Yes              | ❌ No                 | ✅ (with TDZ)     |
| Block Scoped                    | ❌ No (function)    | ❌ No                 | ✅ Yes            |
| Re-declarable in same scope     | ✅ Yes              | ✅ Yes                | ❌ No             |

---

### 🔹 Best Practices
- ✅ Prefer `let` and `const` in modern JavaScript.
- 🚫 Avoid using undeclared variables (`x = 10`).
- 🚫 Avoid `var` unless necessary for legacy support.


© Ranjan Prabhu — JavaScript Cheatsheet
