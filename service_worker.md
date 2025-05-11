
# Service Workers – Interview Prep Guide

## ✅ What is a Service Worker?

A **Service Worker** is a script that your browser runs in the background, separate from a web page. It enables features like:
- Offline capabilities
- Background sync
- Push notifications
- Network request interception and caching

## 🔁 Service Worker Lifecycle

1. **Register** – Registers the worker script (`navigator.serviceWorker.register`).
2. **Install** – Runs once on installation, can pre-cache files.
3. **Activate** – Runs after installation, used for cache cleanup.
4. **Fetch** – Intercepts and handles network requests.

### Code Example

```js
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open("notes-cache").then((cache) => {
      return cache.addAll([
        "/",
        "/index.html",
        "/notes.json",
        "/static/js/bundle.js"
      ]);
    })
  );
});
```

## 🧠 What Happens in the Code Above?

- On `install`, it opens a cache named `notes-cache`.
- It pre-caches essential app files (`index.html`, static JS, etc.).
- `event.waitUntil()` ensures service worker doesn't finish installation until caching completes.

## 🧲 Fetch Handling for Offline Support

```js
self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((cachedResponse) => {
      return cachedResponse || fetch(event.request);
    })
  );
});
```

- First checks cache, falls back to network if not found.
- Enables offline experience.

## 🧪 How to Register a Service Worker

In your main JS file (e.g. `index.js` in React):

```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js')
    .then(() => console.log("SW registered"))
    .catch((err) => console.error("SW registration failed:", err));
}
```

## 🚀 Benefits of Service Workers

- Offline access to content
- Faster repeat visits with cached assets
- Efficient background tasks like sync and notifications
- Acts as a programmable network proxy

## 💡 Real-world Use Cases

- Google Docs (offline editing)
- Twitter PWA
- Flipkart Lite
- News apps caching headlines
- E-commerce carts that work offline

## 📁 Additional Enhancements

- Combine with **Workbox** for easier setup
- Use **IndexedDB** for storing structured data
- Pair with **Web Push API** for notifications

## 📘 Bonus: Checklist for PWA

- [x] Manifest.json
- [x] Service Worker registered
- [x] HTTPS-only
- [x] Responsive UI
- [x] Offline fallback

---

For more info: [MDN Service Worker Docs](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
