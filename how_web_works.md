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
