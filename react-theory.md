

# 🔄 React Reconciliation & Fiber Internals – Summary

## 🔹 1. What is Reconciliation?
Reconciliation is the process React uses to update the **Real DOM** efficiently.
- It compares the current Virtual DOM with the previous one (diffing).
- Computes the minimal set of changes needed to update the DOM.

## 🔹 2. Virtual DOM & Fiber Tree
- React creates a **Virtual DOM** using `React.createElement()`.
- Internally, this becomes the **Fiber Tree**:
  - Each **Fiber node** represents a React element (component or DOM element like `<div>`, `<p>`, etc.).
  - Contains metadata: type, props, state, parent/child/sibling references, effects, etc.

## 🔹 3. Rendering Flow (via `createRoot`)
`ReactDOM.createRoot()` boots the app and triggers reconciliation.

### ⚙️ Render Phase (diffing)
- React builds a **work-in-progress Fiber Tree**.
- Compares new React elements to the old Fiber tree (diffing).
- No changes to the DOM are made in this phase.
- Supports **scheduling**, **interruption**, and **prioritization** using **lanes**.

### ✅ Commit Phase
- Applies changes from the work-in-progress tree to the **Real DOM**.
- Executes DOM mutations, `useEffect`, lifecycle methods, etc.
- Cannot be interrupted.

## 🔹 4. Component Re-rendering on Updates
- On state or prop change:
  - React marks the corresponding Fiber subtree for update.
  - Re-evaluates JSX for that subtree.
  - Updates the virtual DOM (Fiber tree) and commits changes.

## 🔹 5. Role of `React.memo`
- `React.memo` wraps functional components to **avoid unnecessary re-renders**.
- Skips re-rendering if props haven't changed (shallow comparison).
- Reduces work during the render phase (skips new Fiber node creation).
- Best for **pure** and **stable** components with frequent unchanged props.

---

## 📌 Summary Flow

```plaintext
React.createElement() → Fiber Tree (Virtual DOM)
→ Render Phase (diffing) → Commit Phase (DOM mutation)
→ Updated Real DOM

React.memo() → skips re-renders → fewer Fiber updates
```

![Alt text](https://github.com/ranjansaga/system_design/blob/main/react-fibre.png)


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

---

## ✅ When to Use `useCallback`

### Use it when:

* You pass a function as a prop to a **memoized child component** (via `React.memo`).
* You want to **avoid re-creating a function on every render** when dependencies are stable.

### Example (Good Case):

```js
const handleClick = useCallback(() => {
  console.log(count * 2);
}, [count]);

<Child onClick={handleClick} />
```

---

## ❌ When NOT to Use `useCallback`

### Avoid it when:

* The dependencies include **functions declared inside the component** (which are recreated every render).
* The function is **not passed down as a prop** or **used in performance-critical logic**.

### Example (Bad Case):

```js
const getValue = () => count * 2; // recreated every render

const handleClick = useCallback(() => {
  console.log(getValue());
}, [getValue]); // new ref every time → defeats useCallback
```

---


## 🔍 Memoization Internals

### 1. `useMemo`

* **Purpose**: Memoizes a computed value.
* **Stored in**: Fiber node's `memoizedState`.
* **Internal Flow**:

  ```js
  {
    memoizedValue: <cached value>,
    deps: [/* dependency array */]
  }
  ```
* **Behavior**: If dependencies are the same, the cached value is reused.

### 2. `useCallback`

* **Purpose**: Memoizes a function reference.
* **Stored in**: Fiber node's `memoizedState`.
* **Internal Flow**:

  ```js
  {
    memoizedValue: () => { /* cached function */ },
    deps: [/* dependency array */]
  }
  ```
* **Behavior**: If dependencies haven't changed, the same function reference is returned.

### 3. `React.memo`

* **Purpose**: Prevents re-renders of a component if its props haven't changed.
* **Stored in**: Fiber node's `memoizedProps` and `pendingProps`.
* **Behavior**:

  * Compares previous and new props using `Object.is` or a custom comparison.
  * Skips render if props are the same.



## 🧠 Summary Table

| Hook/Feature  | Purpose                      | Stored In             | Skip Re-render? | Use Case                               |
| ------------- | ---------------------------- | --------------------- | --------------- | -------------------------------------- |
| `useMemo`     | Cache computed value         | `Fiber.memoizedState` | ❌ (value reuse) | Expensive calculations                 |
| `useCallback` | Cache function ref           | `Fiber.memoizedState` | ❌ (ref reuse)   | Stable callbacks for memoized children |
| `React.memo`  | Skip re-render on same props | `Fiber.memoizedProps` | ✅               | Prevent child component updates        |

---


