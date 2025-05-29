
# 📘 `this` Keyword Cheat Sheet in JavaScript

## ✅ Quick Summary

| Function Type       | `this` is determined by...                               |
|---------------------|-----------------------------------------------------------|
| Regular function    | How it's **called** (runtime binding)                     |
| Arrow function      | Where it's **defined** (lexical binding)                  |
| Method call         | The object before the `.`                                 |
| Class method        | The instance (`this` refers to the object created)        |
| `bind`, `call`, `apply` | Explicit binding of `this`                             |

---

## 🔁 Regular Function

```js
function show() {
  console.log(this);
}

show(); // browser: window, strict mode: undefined
```

- `this` defaults to global object (`window`) or `undefined` in strict mode.

---

## 🏠 Method in Object

```js
const obj = {
  name: "Ranjan",
  greet() {
    console.log(this.name);
  }
};

obj.greet(); // ✅ "Ranjan"
```

- `this` refers to the **object calling the method**.

---

## ❌ Arrow Function as Method (Don't Do This)

```js
const obj = {
  name: "Ranjan",
  greet: () => {
    console.log(this.name);
  }
};

obj.greet(); // ❌ undefined
```

- Arrow functions don't bind their own `this`.
- `this` here refers to the **surrounding lexical scope**, which is **global**.

---

## ✅ Arrow Function Inside a Method

```js
const obj = {
  name: "Ranjan",
  greet: function () {
    const inner = () => {
      console.log(this.name);
    };
    inner();
  }
};

obj.greet(); // ✅ "Ranjan"
```

- Arrow function inherits `this` from the **`greet` method**, which has `this = obj`.

---

## ❌ Regular Function Inside a Method

```js
const obj = {
  name: "Ranjan",
  greet: function () {
    function inner() {
      console.log(this.name);
    }
    inner(); // ❌ undefined
  }
};

obj.greet();
```

- Regular `inner()` function has its own `this`, which defaults to global or `undefined`.

---

## ✅ Explicit Binding: `bind`, `call`, `apply`

```js
function greet() {
  console.log(this.name);
}

const user = { name: "Ranjan" };

greet.call(user);  // ✅ Ranjan
greet.apply(user); // ✅ Ranjan
const boundGreet = greet.bind(user);
boundGreet();      // ✅ Ranjan
```

- `call`/`apply` **invoke immediately**.
- `bind` **returns a new function with `this` bound**.

---

## ✅ `this` in Classes

```js
class User {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    console.log(this.name);
  }
}

const u = new User("Ranjan");
u.sayHi(); // ✅ "Ranjan"
```

- Inside class methods, `this` refers to the class instance.

---

## ✅ `this` in Arrow Functions (Lexical Scope)

### Global Scope
```js
const fn = () => {
  console.log(this);
};

fn(); // browser: window, Node strict: {}
```

### In `setTimeout`
```js
const obj = {
  count: 10,
  start: function () {
    setTimeout(() => {
      console.log(this.count); // ✅ 10
    }, 1000);
  }
};

obj.start();
```

- Arrow function inherits `this` from `start`, which is bound to `obj`.

---

## 🧠 Mental Model

- Arrow functions **capture `this` from where they are defined**, not how they're called.
- Regular functions **get `this` from how they’re called**.
