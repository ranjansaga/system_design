# z-index Interview Cheatsheet

## What is `z-index`?

`z-index` is a CSS property that controls the **stacking order** of elements along the z-axis (depth). Elements with a higher `z-index` appear above those with a lower `z-index`—but only within the same **stacking context**.

```css
.element {
  position: relative; /* or absolute, fixed, sticky */
  z-index: 10;
}
```

---

## Rules of `z-index`

1. Only works on elements with a `position` value other than `static`.
2. A higher `z-index` appears above a lower one.
3. Works **within** the same stacking context.

---

## What Creates a Stacking Context?

A new stacking context is created by:

| CSS Property     | Condition                                                       |
| ---------------- | --------------------------------------------------------------- |
| `position`       | `relative`, `absolute`, `fixed`, or `sticky` + `z-index ≠ auto` |
| `opacity`        | Less than `1`                                                   |
| `transform`      | Any value other than `none`                                     |
| `filter`         | Any value other than `none`                                     |
| `perspective`    | Any value other than `none`                                     |
| `will-change`    | With `transform`, `opacity`, etc.                               |
| `mix-blend-mode` | Any value other than `normal`                                   |
| `contain`        | With `layout`, `paint`, or `strict`                             |
| `isolation`      | Set to `isolate`                                                |

---

## Tricky Cases of `z-index`

### 1. Stacking Context Isolation

If a parent element creates a stacking context, its child **cannot escape** it—even with a high `z-index`.

```css
.parent {
  position: relative;
  z-index: 1;
  transform: translateZ(0); /* New stacking context */
}
.child {
  position: absolute;
  z-index: 9999;
}
.overlay {
  position: relative;
  z-index: 10;
}
```

Despite `.child` having a `z-index` of 9999, it **cannot overlap** `.overlay` because `.child` is isolated inside `.parent`'s stacking context.

---

### 2. Nested Stacking Contexts

Child elements are limited by their parent’s stacking context. You can't layer child elements above unrelated parents.

### 3. Negative z-index

You can use negative values to push elements behind, but they can become unclickable or appear behind the body background.

```css
.backdrop {
  position: absolute;
  z-index: -1;
}
```

---

## z-index Doesn’t Work? Check This:

* Is the element positioned (not `static`)?
* Is it inside a stacking context?
* Is any parent creating a new stacking context?
* Are there conflicting `transform`, `opacity`, or `filter` properties?

---

## Debugging Tips

* Use Chrome DevTools → Layers panel.
* Remove `transform`, `opacity`, `filter` temporarily.
* Test z-index values like 1, 10, 1000 to spot layering changes.

---

## Best Practices

* Avoid unnecessary z-index stacking.
* Define a `z-index scale` in your design system (e.g., modal > tooltip > dropdown > header).
* Keep UI layers flat and well-documented.
* Be cautious using `transform`, `opacity`, etc. that isolate stacking contexts.

---

## Example z-index Scale

```scss
$z-indexes: (
  base: 0,
  dropdown: 1000,
  sticky: 1100,
  fixed: 1200,
  modal: 1300,
  toast: 1400,
);
```

Use it consistently across your app to avoid z-index conflicts.

---

This cheatsheet helps prepare for CSS layering and rendering behavior questions in front-end interviews.
