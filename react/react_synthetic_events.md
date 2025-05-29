# React Synthetic Events: Cheat Sheet & Guide

## 📌 What Are Synthetic Events?

React uses a **SyntheticEvent** system as a wrapper around the browser’s native events to ensure consistent behavior across different browsers. This system provides a normalized interface and adds features like event pooling, delegation, and cross-browser compatibility.

---

## 🔁 Event Delegation in React

### ✅ Default Behavior
- React **does not attach individual event listeners** to each element.
- Instead, it **delegates** all event handling to a **single root DOM node** (usually the document or root container).
- This is more performant, especially when dealing with many elements.

### 📋 Example

```jsx
function App() {
  const handleClick = (event) => {
    console.log('Clicked:', event.target.textContent);
  };

  return (
    <div onClick={handleClick}>
      <button>One</button>
      <button>Two</button>
    </div>
  );
}
```

- A single `onClick` on `<div>` handles clicks from all child buttons.
- `event.target` identifies the actual clicked button.

---

## 🔄 Event Bubbling vs Capturing

| Phase         | Direction         | React Support |
|---------------|-------------------|---------------|
| Capturing     | Top → Target      | ✅ `onClickCapture` |
| Bubbling      | Target → Top      | ✅ `onClick` |

### 👇 Bubbling (Default)
```jsx
<div onClick={handleClick}></div>
```

### 👆 Capturing
```jsx
<div onClickCapture={handleClick}></div>
```

---

## 🧠 Synthetic Event Pooling

### What Is It?
React reuses (`pools`) event objects for performance. After the event callback completes, the event is **nullified**—meaning all properties like `event.target` become inaccessible.

### Problem:
```js
const handleClick = (e) => {
  setTimeout(() => {
    console.log(e.target.value); // ❌ Might be null
  }, 1000);
};
```

### Fix: Persist or copy the value

#### ✅ Option 1: `e.persist()`
```js
const handleClick = (e) => {
  e.persist(); // Prevent pooling
  setTimeout(() => {
    console.log(e.target.value);
  }, 1000);
};
```

#### ✅ Option 2: Cache value immediately
```js
const handleClick = (e) => {
  const value = e.target.value;
  setTimeout(() => {
    console.log(value); // ✅ Safe
  }, 1000);
};
```

---

## 🧾 Summary

| Feature              | Context API      | Redux w/ Middleware |
|----------------------|------------------|----------------------|
| Delegation           | ❌ No             | ✅ Yes (via React)    |
| Bubbling & Capturing | ✅ Supported      | ✅ Supported          |
| Event Pooling        | ✅ Yes (React 17−) | ✅ Yes               |
| Performance          | ✅ Efficient via delegation | ✅ Scalable |

---

## 📌 Best Practices

- Use `event.target.value` immediately or cache it.
- Use `onClickCapture` if you need to intercept events early.
- Prefer event delegation for dynamic lists.
- Use `e.persist()` if you must access events asynchronously.

---

## 🔗 References
- [React SyntheticEvent Docs](https://reactjs.org/docs/events.html)
- [MDN Event Bubbling and Capturing](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_bubbling_and_capture)
