# TypeScript Generics Cheat Sheet

## 📘 What Are Generics?

Generics are a way to **write reusable and type-safe code** that works with multiple types **without losing type information**.

---

## 🎯 Why Use Generics?

✅ Reusable logic  
✅ Type inference and type safety  
✅ Prevent use of `any`  
✅ Cleaner APIs and components  

---

## 🔧 Syntax

```ts
function identity<T>(arg: T): T {
  return arg;
}
```

- `T` is a **type variable** (placeholder for actual type).
- You provide the type when you call the function.

---

## ✅ Examples

### 1. Generic Function

```ts
function firstItem<T>(arr: T[]): T {
  return arr[0];
}

firstItem<number>([1, 2, 3]); // returns 1
firstItem(['a', 'b']);        // returns "a"
```

---

### 2. Generic Interface

```ts
interface ApiResponse<T> {
  data: T;
  status: number;
}

const userRes: ApiResponse<{ name: string }> = {
  data: { name: 'Ranjan' },
  status: 200
};
```

---

### 3. Generic Class

```ts
class Box<T> {
  content: T;
  constructor(value: T) {
    this.content = value;
  }
}

const numBox = new Box<number>(42);
```

---

### 4. Generics with Constraints

```ts
function logLength<T extends { length: number }>(item: T): number {
  return item.length;
}

logLength('hello');   // 5
logLength([1, 2, 3]);  // 3
// logLength(42);     // ❌ Error: number has no 'length'
```

---

### 5. Default Generic Types

```ts
function wrap<T = string>(value: T): T[] {
  return [value];
}

wrap();        // defaults to string[]
wrap(10);      // returns number[]
```

---

## 🧠 Naming Convention

| Name | Meaning |
|------|---------|
| `T`  | Type (general) |
| `K`  | Key (in objects) |
| `V`  | Value (in key-value pairs) |
| `U`, `S` | Secondary types |
| `ElementType` | Array/list element type |

---

## 🧪 Real-World Example: Fetch Wrapper

```ts
async function fetchData<T>(url: string): Promise<T> {
  const res = await fetch(url);
  return await res.json() as T;
}

type User = { id: number; name: string };
const user = await fetchData<User>('/api/user');
```

---

## 🧵 TL;DR

- Use generics to keep types flexible and safe
- Helps build reusable utility functions, APIs, and components
- Use constraints to guide which types are allowed