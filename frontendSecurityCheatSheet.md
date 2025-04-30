
# 🛡️ Frontend Security Cheatsheet

A quick reference guide of common frontend security threats, examples, and solutions.

---

| # | 🔓 Threat | 💣 Example | 🛠️ Solution |
|--|----------|------------|-------------|
| 1 | **XSS (Cross Site Scripting)** | Attacker injects `<script>alert('Hacked')</script>` into a form input or query param. | - React auto-escapes content by default. <br> - Avoid `dangerouslySetInnerHTML`. <br> - Sanitize user input. <br> - Use strong Content Security Policy (CSP). |
| 2 | **CSRF (Cross Site Request Forgery)** | Logged-in user clicks `<img src="http://bank.com/transfer?amount=1000">` and unintentionally initiates actions. | - Use CSRF tokens in forms or headers. <br> - Store tokens in cookies with `SameSite=Strict`. <br> - Verify origin/referrer headers on the server. |
| 3 | **Insecure Local Storage** | Attacker accesses `localStorage.getItem('token')` via XSS. | - Do not store sensitive data (e.g., JWT) in `localStorage` or `sessionStorage`. <br> - Use HttpOnly, Secure cookies for JWT. <br> - Alternatively, store tokens in memory for short-lived sessions. |
| 4 | **No HTTPS** | User data (e.g., login credentials) sent via HTTP can be intercepted. | - Use HTTPS with a valid TLS certificate. <br> - Redirect all HTTP traffic to HTTPS. <br> - Enable HSTS headers. |
| 5 | **Clickjacking** | Your site is loaded in an iframe and users are tricked into clicking hidden elements. | - Add `X-Frame-Options: DENY` header. <br> - Use CSP: `frame-ancestors 'none'`. |
| 6 | **Unvalidated Redirects** | `https://yourapp.com?redirect=http://malicious.com` sends users to attacker site. | - Validate redirect URLs on the server. <br> - Only allow whitelisted domains or paths. |
| 7 | **Weak CSP (Content Security Policy)** | Inline JS or external malicious scripts are executed. | - Set CSP headers like: <br>   `Content-Security-Policy: default-src 'self'; script-src 'self';` <br> - Avoid `unsafe-inline`. <br> - Use nonces or hashes for inline scripts if necessary. |
| 8 | **Sensitive Data in URL** | `https://example.com/reset?token=abc123` exposes token in browser history. | - Avoid placing sensitive data in URLs. <br> - Use POST body or HTTP headers instead. |
| 9 | **Improper CORS Setup** | Backend allows requests from any origin: `Access-Control-Allow-Origin: *` | - Set CORS policy to trusted origins only. <br> - Use `credentials: true` only when necessary and configure `Access-Control-Allow-Credentials`. |
| 10 | **Exposed Stack Traces/Errors** | Error like `TypeError: Cannot read property 'x' of undefined` shown to user. | - Show user-friendly error messages. <br> - Log detailed errors only on the server side. |

---

## ✅ Recommended HTTP Security Headers

```http
Content-Security-Policy: default-src 'self';
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
Referrer-Policy: no-referrer
Permissions-Policy: geolocation=(), camera=()
```

---

## ✅ JWT Storage Best Practices

| Storage Method | XSS Protection | CSRF Protection | Use Case |
|----------------|----------------|------------------|----------|
| `localStorage` | ❌ No | ✅ Yes | ❌ Avoid |
| `sessionStorage` | ❌ No | ✅ Yes | ❌ Avoid |
| `HttpOnly Cookie` | ✅ Yes | ❌ Needs CSRF token | ✅ Recommended |
| Memory (JS variable) | ✅ Yes | ✅ Yes | ✅ Short sessions only |

---

## ✅ CSRF Token Flow Summary

1. Server generates a CSRF token per session.
2. Token is sent to frontend via cookie or endpoint.
3. Frontend includes it in request header or hidden form field.
4. Server validates token before processing the request.

---

## ✅ CSP (Content Security Policy) for React/Next.js

- Set in headers (via server like Nginx, Express, or next.config.js):

```http
Content-Security-Policy: default-src 'self'; script-src 'self';
```

- In Next.js, use `next.config.js` → `headers()` function.

---
