
# 🛡️ Rate Limiting Cheatsheet

---

## ✅ What is a Rate Limiter?

A **rate limiter** controls how many requests a client can make to a server within a specific time window.

Think of it like a bouncer letting only a few people into a club per minute — even if 100 show up.

---

## 🎯 Why Use Rate Limiting?

| Reason              | Purpose                                                             |
|---------------------|---------------------------------------------------------------------|
| **Prevent Abuse**   | Stops users from spamming APIs (login, form submission, etc.)       |
| **Ensure Fair Usage**| Allocates fair resources in multi-tenant systems                   |
| **Mitigate DDoS**   | Protects server from traffic floods                                 |
| **Improve Reliability**| Avoids performance degradation under high load                   |
| **Control Cost**    | Avoids excessive backend load and unnecessary processing            |

---

## 🧰 Types of Rate Limiting Strategies

| Strategy           | Description                                           |
|--------------------|-------------------------------------------------------|
| **Fixed Window**    | Count reset at every interval (e.g., per minute)     |
| **Sliding Window**  | Tracks rolling time period instead of fixed one      |
| **Token Bucket**    | Tokens are added at a rate; request consumes tokens  |
| **Leaky Bucket**    | Ensures a fixed outflow of requests per unit time    |

---

## 🛠️ Custom Rate Limiter in Express (Fixed Window Example)

```js
// rateLimiter.js
const hashMap = new Map();

function myRateLimiter(req, res, next) {
  const currentTime = Date.now();
  const maxRequests = 10;
  const timeWindow = 1 * 60 * 1000; // 1 minute in ms

  if (!hashMap.has(req.ip)) {
    hashMap.set(req.ip, []);
  }

  const timestamps = hashMap
    .get(req.ip)
    .filter(ts => currentTime - ts < timeWindow);

  if (timestamps.length >= maxRequests) {
    return res
      .status(429)
      .json({ message: "Too many requests, please try again later." });
  }

  timestamps.push(currentTime);
  hashMap.set(req.ip, timestamps);

  next();
}

module.exports = myRateLimiter;
```

---

## ✅ How to Use in Express App

```js
const express = require('express');
const app = express();
const myRateLimiter = require('./rateLimiter');

app.use(myRateLimiter);

app.get('/', (req, res) => {
  res.send('Rate limited route');
});

app.listen(3000, () => console.log('Server running'));
```

---

## ⚠️ Limitations of In-Memory Rate Limiting

- **Not shared across servers**
- **No TTL cleanup**
- **Not persistent** (cleared on server restart)

---

## 🧱 Improvements for Production

| Enhancement         | Benefit                                    |
|---------------------|--------------------------------------------|
| Use **Redis**        | Centralized and persistent limit tracking |
| Add **TTL cleanup**  | Prevent memory leak                       |
| Use **user ID**      | More accurate than IP (esp. for auth APIs)|
| Allow **custom limits per route** | Flexibility                   |
