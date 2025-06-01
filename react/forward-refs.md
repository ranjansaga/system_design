# 🔄 React `forwardRef` and `useImperativeHandle` — Summary & Example

---

## 🧠 Overview

React provides `forwardRef` and `useImperativeHandle` to manage **refs and imperative APIs** between parent and child components. These tools help you expose only the required functionality to parent components, improving encapsulation and reusability.

---

## 🔁 `forwardRef`

### What It Does

* Forwards a `ref` from a parent to a specific child DOM node or component inside a custom component.

### When To Use

* You wrap a native DOM element (e.g., `<input>`) inside your own component and want the parent to control it.
* Needed for building reusable input components, modals, design systems, etc.

### Example

```jsx
const MyInput = React.forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});

const ref = useRef();
<MyInput ref={ref} />;
ref.current.focus(); // ✅ Works
```

---

## 🔧 `useImperativeHandle`

### What It Does

* Customizes the value exposed to the parent via a `ref`.
* Lets you expose **only specific methods or values**.
* Helps **block direct access** to internal DOM or logic.

### When To Use

* You want to expose a limited API from a child (e.g., `focus`, `clear`).
* You’re building reusable components and want to enforce contracts.

### Example

```jsx
const FancyInput = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus(),
    clear: () => inputRef.current.value = ''
  }));

  return <input ref={inputRef} />;
});

// Parent usage
const ref = useRef();
<FancyInput ref={ref} />;
ref.current.focus(); // ✅ Exposed
ref.current.clear(); // ✅ Exposed
ref.current.value = 'hack'; // ❌ Not allowed
```

---

## 🔒 Benefits of Using Both Together

| Benefit         | Description                                             |
| --------------- | ------------------------------------------------------- |
| Encapsulation   | Prevents parent from messing with internal DOM or logic |
| Controlled APIs | Expose only safe, intended methods                      |
| Abstraction     | Hides complexity inside reusable components             |
| Predictability  | Clean contract between parent and child                 |

---

## ✅ Summary

* Use `forwardRef` to forward a `ref` to a child DOM element.
* Use `useImperativeHandle` to expose a **custom interface** to the parent.
* Perfect for inputs, modals, tooltips, sliders, and other **controlled components**.

Let me know if you'd like to add modal/canvas examples or a styled-components version!
