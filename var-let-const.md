
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

© Ranjan Prabhu — JavaScript Cheatsheet
