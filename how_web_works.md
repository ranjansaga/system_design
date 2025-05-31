# 🌐 How the Web Works – Interview Cheat Sheet

## 1. High-Level Flow of a Website Visit

```
User types URL → DNS Lookup → TCP/TLS Connection → HTTP Request → Server Response → Browser Renders Page
```

---

## 2. DNS Resolution

- Translates domain name (e.g., google.com) into an IP address.
- Resolved via:
  - Browser cache
  - OS cache
  - Router cache
  - ISP DNS server
  - Authoritative DNS server

---

## 3. TCP & HTTP Relationship

- **HTTP is an application layer protocol** that runs on top of **TCP**.
- TCP ensures reliable, ordered, and error-checked delivery.
- Every HTTP(S) request establishes a TCP connection (with TLS for HTTPS).

---

## 4. HTTP Request Lifecycle

1. Browser resolves DNS to IP.
2. Opens TCP connection (with TLS for HTTPS).
3. Sends HTTP request (method, headers, cookies, etc.).
4. Server responds with HTML/CSS/JS/images.
5. Connection kept alive or closed (via `Connection: keep-alive`).

---

## 5. Browser Rendering Pipeline

1. **Download HTML** (first).
2. **Parse HTML** to build the **DOM** tree.
3. Discover and download:
   - **CSS** → Build **CSSOM**.
   - **JS** → Executed (can modify DOM).
   - **Images, fonts, etc.**
4. **Construct Render Tree** (DOM + CSSOM).
5. **Layout** – Calculate positions/sizes.
6. **Paint** – Convert elements to pixels.
7. **Composite** – Final screen render.

---

## 6. HTML vs CSS vs JS Loading Order

| Resource | Discovered When | Loaded |
|----------|------------------|--------|
| HTML     | First            | Immediately |
| CSS      | When `<link>` is parsed | In parallel (but blocks rendering) |
| JS       | When `<script>` is parsed | In parallel (may block if not `defer/async`) |

> ✅ Tip: Use `defer` for non-blocking JS execution after HTML parsing.

---

# 🔍 What is Render-Blocking?

Render-blocking refers to any resource that **prevents the browser from rendering content to the screen** until it is fully downloaded and processed.

### 🚫 Common Render-Blocking Resources:

| Resource Type                   | Render-Blocking? | Reason                                                               |
| ------------------------------- | ---------------- | -------------------------------------------------------------------- |
| **CSS (`<link>` tags)**         | ✅ Yes            | Needed to build the CSSOM before the render tree can be constructed. |
| **Synchronous JS (`<script>`)** | ✅ Yes            | Blocks HTML parsing while script downloads and executes.             |
| **Fonts**                       | ❌ Not directly   | They delay text paint but do not block the entire page render.       |
| **Images**                      | ❌ No             | Loaded after render, don’t block paint.                              |
| **Async/Defer JS**              | ❌ No             | Do not block HTML parsing or rendering.                              |

---

## 📜 HTML Parsing & CSS

When the browser encounters a `<link rel="stylesheet">`:

* ✅ **HTML continues parsing** in parallel.
* 🛑 **Rendering is paused** until the CSS file is downloaded and parsed (i.e., CSSOM is ready).

### Critical Rendering Path

1. **HTML parsed** → DOM built
2. **CSS downloaded & parsed** → CSSOM built
3. **Render Tree = DOM + CSSOM**
4. **Layout**
5. **Paint**
6. **Composite**

---

## 🕵️ Fonts and Rendering

* Fonts are **not technically render-blocking**.
* BUT: Text that uses web fonts may **not appear** until the font loads.

### Paint Behavior:

| Phase     | Fonts Needed?     | Notes                                         |
| --------- | ----------------- | --------------------------------------------- |
| Reflow    | ❌                 | Can use fallback font metrics.                |
| Paint     | ✅ (for web fonts) | Text won't be painted if fonts aren't loaded. |
| Composite | ✅                 | Depends on successful paint.                  |

### FOIT vs FOUT

* **FOIT**: Flash of Invisible Text — default behavior, delays text paint.
* **FOUT**: Flash of Unstyled Text — with `font-display: swap`, shows fallback immediately.

### Optimization:

```css
@font-face {
  font-family: 'CustomFont';
  src: url('/fonts/CustomFont.woff2') format('woff2');
  font-display: swap;
}
```

```html
<link rel="preload" href="/fonts/CustomFont.woff2" as="font" type="font/woff2" crossorigin="anonymous">
```

---

## ✅ Key Takeaways

* CSS and synchronous JS are primary render blockers.
* Fonts don’t block HTML or layout, but can delay **text paint**.
* HTML continues parsing even if CSS or fonts are downloading.
* Use `font-display: swap` and `preload` to optimize font delivery.
* Minimize render-blocking resources to reduce **TTFB** and improve **FCP/TTI**.

---

## 7. SSR vs CSR

- **Server-Side Rendering (SSR)**:
  - HTML is fully generated on server and sent.
  - Faster First Paint, better SEO.
- **Client-Side Rendering (CSR)**:
  - JS builds UI in browser (SPA).
  - Blank HTML + JS bundle (e.g., React).
  - Needs JS to function.

---

## 8. CloudFront + S3 Setup

- No backend server (static hosting).
- HTML/CSS/JS files served from **S3 buckets** via **CloudFront CDN**.
- Suitable for static or pre-rendered (e.g., Next.js `export`) sites.

---

## 9. Caching Layers

| Cache Type    | Location                | Purpose                           |
|---------------|-------------------------|------------------------------------|
| Browser Cache | User’s browser          | Avoid redundant fetches            |
| OS Cache      | Local machine           | DNS entries, connection reuse      |
| Router Cache  | Home/office router      | DNS cache                          |
| ISP Cache     | ISP DNS or CDN          | DNS or static asset caching        |
| CDN (e.g., CloudFront) | Edge servers   | Speed up delivery close to user    |

---

## 10. Performance Tips

- Minimize render-blocking CSS/JS.
- Use `async`/`defer` for JS.
- Compress images, enable GZIP/Brotli.
- Lazy load non-critical resources.
- Cache wisely with `Cache-Control`, `ETag`.

---

## 11. DevTools Pro Tips

- **Network tab** – Inspect load order, waterfall.
- **Performance tab** – Visualize render timeline.
- **Lighthouse** – Audit performance, SEO, PWA.

---

## 12. Bonus: TLS Handshake (for HTTPS)

1. Client → Server: Hello (with supported ciphers).
2. Server → Client: Certificate + public key.
3. Client verifies, sends encrypted key.
4. Secure symmetric session starts.
