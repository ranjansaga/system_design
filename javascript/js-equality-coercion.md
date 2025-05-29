
# 🧠 JavaScript Equality & Type Coercion Cheat Sheet

## 🔍 1. `===` (Strict Equality)

| Expression                | Result | Explanation                                   |
|---------------------------|--------|-----------------------------------------------|
| `undefined === undefined` | `true` | Same type and value                           |
| `null === null`           | `true` | Same type and value                           |
| `undefined === null`      | `false`| Different types (`undefined` vs `object`)     |
| `5 === "5"`               | `false`| Number vs String                              |
| `"abc" === "abc"`         | `true` | Both are strings with same value              |

---

## 🤏 2. `==` (Loose Equality - allows coercion)

| Expression                | Result | Explanation                                   |
|---------------------------|--------|-----------------------------------------------|
| `undefined == null`       | `true` | Both are treated as "empty values"            |
| `5 == "5"`                | `true` | `"5"` is coerced to number `5`                |
| `"0" == false`            | `true` | `"0"` → 0 → `false`                           |
| `0 == false`              | `true` | Both become `false`                           |
| `"" == 0`                 | `true` | `""` coerces to `0`                           |
| `"" == false`             | `true` | Both are falsy                                |
| `null == false`           | `false`| `null` only equals `undefined`, not `false`   |

---

## 🔢 3. String and Number Coercion

| Expression      | Result | Explanation                       |
|-----------------|--------|-----------------------------------|
| `"2" + 3`       | `"23"` | `+` with string = string concat   |
| `"2" * 3`       | `6`    | `*` coerces both to numbers       |
| `"10" - 4`      | `6`    | `-` coerces both to numbers       |
| `"abc" * 2`     | `NaN`  | `"abc"` is not a number           |
| `"5" ** 2`      | `25`   | `"5"` coerces to number           |

---

## 🧠 Summary Rules

- `===` → strict, **no type conversion**
- `==` → loose, **type coercion allowed**
- `+` → if **any operand is a string**, does **string concatenation**
- `-`, `*`, `/`, `%`, `**` → coerce to numbers

---

## ✅ Best Practices

- Always use `===` unless you explicitly want coercion.
- Don’t rely on coercion in complex logic – be **explicit** about types.
