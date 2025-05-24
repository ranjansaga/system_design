
# Webpack Config Interview Quick Guide

## 📁 File Types and Module Systems

### CommonJS (`require`)
Used in Node.js environments like `webpack.config.js`:
```js
const path = require("path");
```

### ES Modules (`import`)
Used in React component files:
```js
import React from "react";
```

### Mixing CJS and ESM
Webpack allows using ES Modules in your code and CommonJS in config. When needed, use `.mjs` for ESM-style config files.

---

## 📦 Babel Configuration

### `babel.config.js` or `.babelrc`
```js
module.exports = {
  presets: ['@babel/preset-env', '@babel/preset-react'],
};
```
Used by `babel-loader` in Webpack to transpile modern JavaScript and JSX for browser compatibility.

---

## ⚙️ Webpack Output Configuration

```js
output: {
  path: path.resolve(__dirname, "dist"),
  filename: "bundle.[contenthash].js",
  clean: true,
  publicPath: "auto",
}
```

- `path`: Output directory
- `filename`: Output bundle name with hash for caching
- `clean`: Clears old files in `dist` before build
- `publicPath: "auto"`: Ensures Module Federation works correctly in dynamic hosting setups

---

## 📁 Output Files

- `main.js`: Entry bundle
- `remoteEntry.js`: Metadata manifest for Module Federation
- `vendors-*.js`: Vendor chunks for shared dependencies
- `src_*.js`: Chunk files for lazy-loaded components

---

## 🧠 Module Federation

### Remote App Config
```js
new ModuleFederationPlugin({
  name: "remote_app",
  filename: "remoteEntry.js",
  exposes: {
    "./HelloWorld": "./src/HelloWorld.jsx",
  },
})
```

- `remoteEntry.js` contains metadata (not the code itself)
- Lazy-loaded chunks like `src_HelloWorld_jsx.js` are loaded dynamically

---

## ⏱️ Load Timing

- `remoteEntry.js`: Loaded when the host starts rendering the remote component
- Component chunks: Loaded **only when** the component is imported/lazy-loaded

---

## 🧩 Code Splitting (Optional)

```js
optimization: {
  splitChunks: {
    chunks: 'all',
  },
}
```
Manually splits vendor and component-specific code. Webpack may do this automatically in development.

---

## 🖼️ HTML Injection

Generated via `HtmlWebpackPlugin`:
```html
<script defer src="main.js"></script>
```
Chunk scripts are injected automatically during build based on the Webpack config.

---

## ✅ Summary for Interview

- Use `babel-loader` to transpile JSX
- `webpack.config.js` can be CommonJS or `.mjs` for ESM
- `remoteEntry.js` is a manifest, not actual code
- Lazy-loaded components get their own chunk files
- `publicPath: "auto"` is key for Module Federation
- `HtmlWebpackPlugin` injects scripts into HTML
