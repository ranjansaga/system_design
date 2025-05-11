
# HTTP Caching vs React Query / SWR Caching (Cheat Sheet)

## 🌐 HTTP Caching (Browser-Level)

🔍 ETag (Entity Tag)
What it is: A unique identifier (hash/fingerprint) representing the content of a resource.

How it works:

Server responds with ETag: "abc123" in the response header.

On the next request, the browser sends If-None-Match: "abc123".

If the resource hasn't changed, the server replies with 304 Not Modified (no body).

Saves bandwidth and improves performance.

✅ Use case: Precise cache validation (more accurate than Last-Modified).

📆 Last-Modified
What it is: A timestamp indicating when the resource was last changed.

How it works:

Server responds with Last-Modified: Sat, 10 May 2025 10:00:00 GMT.

Browser sends If-Modified-Since: Sat, 10 May 2025 10:00:00 GMT on the next request.

If the resource hasn’t changed, the server sends 304 Not Modified.

✅ Use case: Simpler and readable cache validation, but may lack precision.

🧠 Summary
Feature	ETag	Last-Modified
Format	Unique string (hash)	Date string
Precision	High	Moderate
Server support	Slightly more compute-heavy	Simple to generate
When to use	When content changes often or dynamically	When timestamp is reliable and consistent

### Key Headers
| Header         | Purpose                                                                 |
|----------------|-------------------------------------------------------------------------|
| `Cache-Control`| Controls how and for how long the response can be cached.               |
| `ETag`         | A unique identifier for a version of the resource.                      |
| `Last-Modified`| Timestamp of when the resource was last changed.                        |

### Behavior
- Managed by the **browser** or **proxy**.
- Automatically used by `fetch()` or `XHR` unless disabled.
- Responses are stored in browser's cache storage (disk or memory).
- Works even **without any JavaScript logic**.

---

## ⚛️ JavaScript Runtime-Level Caching (React Query / SWR)

### How It Works
- Stores API responses in **JavaScript memory** (heap).
- Uses `Map`, `Object`, or internal data structures.
- Resides in React's runtime; **cleared on page reload** (unless persisted).
- Deduplication, retry, refresh intervals, etc., managed via JS logic.

### Example with SWR

```js
import useSWR from 'swr';
const fetcher = (url) => fetch(url).then(res => res.json());

function App() {
  const { data } = useSWR('/api/user', fetcher);
  return <div>{data?.name}</div>;
}
```

- Result is cached **in-memory**.
- If fetched again within `dedupingInterval`, cached data is reused.
- Re-fetching can be controlled via stale time, polling, etc.

---

## 🔍 Key Differences

| Feature               | HTTP Caching          | React Query / SWR                |
|-----------------------|-----------------------|----------------------------------|
| Storage               | Browser (disk/memory) | JS memory (heap)                 |
| Persistence           | Yes                   | No (unless persisted manually)   |
| Auto-managed          | Yes (by browser)      | No (app logic required)          |
| Deduplication         | Implicit              | Explicit with timers/keys        |
| Cache Control         | HTTP Headers          | Custom app logic                 |
| Control Flexibility   | Low                   | High                             |

---

## 💡 TL;DR

- **HTTP Caching** is passive, automatic, and persistent.
- **React Query / SWR** is active, in-memory, and customizable.

Use **both** together where appropriate — e.g., browser HTTP cache handles CDN/static content, and React Query handles dynamic data with cache invalidation and refetching strategies.
