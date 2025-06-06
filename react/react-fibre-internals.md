## React Fiber Node

### A Fiber Node is a JavaScript object that holds all the necessary metadata for rendering and updating that component.

```js
FiberNode = {
  type,               // Component type (function/class/div etc.)
  key,                // Key for list reconciliation
  pendingProps,       // New props for upcoming render
  memoizedProps,      // Props used during last render
  memoizedState,      // 🌟 Component state or hook state list
  return,             // Parent fiber
  child,              // First child fiber
  sibling,            // Next sibling fiber
  stateNode,          // Host DOM node or class component instance
  alternate,          // Link to old fiber (for diffing)
  updateQueue,        // 🌟 Queued updates (like setState calls)
  flags,              // Side-effect tracking
}
```

---

## Where Are Hooks and States Stored?

### 🔗 1. Hooks: Stored as a linked list on the fiber

On function components, React keeps a `memoizedState` on the fiber which points to the first hook node.

Each hook node is an object like:

```js
hook = {
  memoizedState,   // current state value
  baseState,       // base state for useReducer
  queue,           // update queue (pending setState calls)
  next,            // next hook in the chain
}
```

They form a linked list like:

```txt
fiber.memoizedState → hook1 → hook2 → hook3 → null
```

---

## Component Fiber Visual Representation

```txt
Component Fiber
|
|-- memoizedState (points to first hook)
     |
     ├─ Hook 1 (useState)
     |     ├─ memoizedState: 42
     |     └─ next →
     |
     ├─ Hook 2 (useEffect)
     |     ├─ memoizedState: { deps: [...] }
     |     └─ next →
     |
     └─ Hook 3 (useRef)
           └─ memoizedState: { current: ... }
```

---

## Summary

- Each function component gets a **Fiber Node**
- Its `memoizedState` holds a **linked list of hook objects**
- Each hook object stores its own `memoizedState` (actual state) and an `updateQueue`
- React walks this list during every render in order to **keep state consistent**
