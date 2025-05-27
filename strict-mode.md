| Feature                              | Behavior with Strict Mode             |
| ------------------------------------ | ------------------------------------- |
| ❌ Undeclared variables               | Throws `ReferenceError`               |
| ❌ Accidental globals                 | Prevented                             |
| ❌ Assigning to read-only properties  | Throws `TypeError`                    |
| ❌ Duplicates in object/function args | Throws `SyntaxError`                  |
| ❌ `this` in functions (non-methods)  | Defaults to `undefined`, not `window` |
| ❌ Deleting variables/functions       | Throws `SyntaxError`                  |
