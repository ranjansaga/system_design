
# 🧠 CORS (Cross-Origin Resource Sharing) – Cheat Sheet

---

## 🔍 1. What is CORS?

- CORS is a **browser security feature** that restricts web pages from making **requests to a different origin** (domain, protocol, or port).
- It prevents **unauthorized cross-origin access** to a server's resources.

> **Example of Cross-Origin:**  
> Your site: `https://myfrontend.com`  
> API server: `https://api.othersite.com`

---

## 🚦 2. How does CORS Work?

- Browser adds `Origin` header to cross-origin requests.
- Server must respond with **`Access-Control-Allow-Origin`** (and possibly other headers).
- If the browser doesn’t get a valid response, it blocks the request.

---

## ⚡ 3. Simple Request vs Preflight Request

| Feature                     | Simple Request                          | Preflight Request                         |
|----------------------------|------------------------------------------|-------------------------------------------|
| Triggered By               | Basic `GET`, `POST`, or `HEAD` methods   | Non-simple methods (`PUT`, `DELETE`, etc.) or custom headers |
| Content-Type allowed       | `text/plain`, `application/x-www-form-urlencoded`, `multipart/form-data` | Any type (e.g., `application/json`) |
| Custom headers             | ❌ Not allowed                            | ✅ Allowed, but triggers preflight         |
| `OPTIONS` sent first?      | ❌ No                                     | ✅ Yes (sent before actual request)        |
| Server CORS Headers needed | Only `Access-Control-Allow-Origin`       | Must include `Access-Control-Allow-Methods`, `Allow-Headers`, etc. |

---

## ✅ Simple Request – Example

### Request
```http
POST /api/message HTTP/1.1
Origin: https://client.com
Content-Type: text/plain
```

### Response
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://client.com
```

✅ No preflight triggered.

---

## 🚨 Preflight Request – Example

### Preflight Request (OPTIONS)
```http
OPTIONS /api/user HTTP/1.1
Origin: https://client.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header, Content-Type
```

### Server Response
```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://client.com
Access-Control-Allow-Methods: PUT
Access-Control-Allow-Headers: X-Custom-Header, Content-Type
```

---

## 📦 Actual PUT Request – After Preflight

```http
PUT /api/user HTTP/1.1
Origin: https://client.com
Content-Type: application/json
X-Custom-Header: demo
```

### Response
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://client.com
```

---

## 💥 Common CORS Errors and Fixes

| Error Message | Cause | Fix |
|---------------|-------|-----|
| CORS policy: No 'Access-Control-Allow-Origin' | Server didn’t include CORS headers | Server must return `Access-Control-Allow-Origin` |
| Preflight fails with 403 | Server rejected the OPTIONS request | Ensure server handles `OPTIONS` and allows requested method & headers |
| Credentialed requests blocked | `credentials: 'include'` used without proper headers | Add `Access-Control-Allow-Credentials: true` and don’t use `*` for origin |

---

## 🛡️ CORS Headers Quick Reference

| Header | Purpose |
|--------|---------|
| `Access-Control-Allow-Origin` | Allows specific origin (`*`, or a domain) |
| `Access-Control-Allow-Methods` | Allowed methods: `GET`, `POST`, `PUT`, etc. |
| `Access-Control-Allow-Headers` | Which headers the client can send |
| `Access-Control-Allow-Credentials` | Allow cookies/auth to be sent |
| `Access-Control-Max-Age` | Cache preflight response (in seconds) |

---

## 🔁 Server-side (Node.js + Express) Example

```js
const express = require("express");
const cors = require("cors");
const app = express();

app.use(cors()); // <-- handles all CORS headers
app.use(express.json());

app.put("/api/user", (req, res) => {
  res.json({ success: true, message: "User updated" });
});
```

---

## 🎯 Pro Tips

- Use browser **Network tab** → filter `OPTIONS` to see preflights.
- Don't use `*` for `Access-Control-Allow-Origin` if you're sending cookies (`credentials: 'include'`).
- Set `Access-Control-Max-Age` to avoid frequent preflights (e.g., `600` seconds).
- If debugging locally, consider using a proxy or local server with proper CORS setup.

---
