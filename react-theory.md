
# 📌 React.memo — Summary

## ✅ What is `React.memo`?
- A **higher-order component** that prevents **unnecessary re-renders** of a **functional component**.
- It uses **shallow comparison** of props to determine if a re-render is needed.

---

## ✅ When is `React.memo` Useful?

| Scenario | Why it helps |
|----------|--------------|
| **Child gets same primitive props** | Skips re-render if props don’t change |
| **Parent re-renders but child’s props don’t change** | Isolates child from parent updates |
| **Child component is expensive to render** | Saves performance by skipping heavy work |
| **Multiple sibling components** | Prevents unrelated children from re-rendering |

---

## ❌ When `React.memo` is NOT Useful

| Scenario | Why it doesn’t help |
|----------|---------------------|
| **Child always receives new props (e.g., inline objects)** | Shallow compare fails, re-renders anyway |
| **Component is trivial (e.g., small button)** | Overhead of memoization may outweigh benefit |
| **Component depends on `useState`, `useContext`, etc.** | Internal changes still cause re-renders |

---

## ⚠️ Notes

- `React.memo` does **shallow comparison** of props.
- It does **not prevent re-renders** triggered by:
  - **Internal state changes**
  - **Context value changes**
- If props include **non-primitive values** (e.g., objects, arrays, functions), they must be memoized using `useMemo` or `useCallback` to avoid unnecessary renders — but we didn’t cover that here.

---

## 🧪 Example

```jsx
const Child = React.memo(({ value }) => {
  console.log("🔄 Child rendered");
  return <p>Value: {value}</p>;
});

function Parent() {
  const [count, setCount] = useState(0);
  return (
    <>
      <Child value={42} /> {/* Same prop every time */}
      <button onClick={() => setCount(count + 1)}>+</button>
    </>
  );
}
```

### 👉 Without `React.memo`: `Child` re-renders on every button click  
### ✅ With `React.memo`: `Child` skips re-render if `value` doesn't change
