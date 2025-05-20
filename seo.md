# SEO Interview Checklist

## ✅ 1. What is Indexing?

**Indexing** is the process by which search engines discover, analyze, and add web pages to their database so they can appear in search results.

### Example:

If you search `site:ranjansaga.com/blog`, and the page appears in results — it is **indexed**.

---

## ✅ 2. What Makes a Page Indexable?

To be indexable, a page must:

* ✅ Not be blocked in `robots.txt`

  ```
  User-agent: *
  Disallow: /admin/
  ```

* ✅ Not contain `<meta name="robots" content="noindex">`

  ```html
  <meta name="robots" content="index, follow">
  ```

* ✅ Return a `200 OK` HTTP status (avoid 404, 500, 301 loops, etc.)

* ✅ Be linked from at least one crawlable page

* ✅ Be included in the XML sitemap

* ✅ Use correct canonical tags if there are duplicate pages

---

## ✅ 3. How to Check If a Page is Indexed

* Use Google search:

  ```
  site:yourdomain.com/page
  ```

* Use **Google Search Console > URL Inspection Tool**

  * Shows if the page is indexed
  * Explains crawl and index issues

---

## ✅ 4. Structured Data and Rich Snippets

Structured data helps search engines understand your content.

### Example: Breadcrumb Structured Data

```html
<script type="application/ld+json">
{
  "@context": "http://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://www.example.com/"
    }
  ]
}
</script>
```

# Sitemap Interview Notes

**Date:** 2025-05-20

---

## ✅ Who Creates Sitemaps?

Sitemaps are typically created by:

- Developers (manual or scripted)
- Backend servers (dynamic generation)
- CMS tools (WordPress, Shopify, etc.)

---

## ❓ Do All Websites Have Sitemaps?

Not all, but all **should**. They help:

- Search engines crawl pages
- Especially useful for:
  - Large websites
  - Dynamic content
  - Poor internal linking

---

## 🔍 How Does Google Know About Your Sitemap?

1. **Google Search Console**: Add under "Sitemaps"
2. **robots.txt**:
   ```
   Sitemap: https://yourdomain.com/sitemap.xml
   ```

---

## 🧩 How Does `/sitemap.xml` Work If the Backend Creates It?

### ✅ Case 1: Dynamic Sitemap via Backend Route
```js
app.get('/sitemap.xml', async (req, res) => {
  const xml = generateSitemap(); // build XML dynamically
  res.header('Content-Type', 'application/xml');
  res.send(xml);
});
```
- Sitemap is **not a file**, just served when requested.
- Search engines can still access `/sitemap.xml`.

---

### ✅ Case 2: Pre-generated at Build Time
```js
const fs = require('fs');
const sitemap = buildXML(); 
fs.writeFileSync('public/sitemap.xml', sitemap);
```
- Static sitemap written to `public/` directory.
- Hosted at `https://yourdomain.com/sitemap.xml`.

---

### ✅ Case 3: S3 / CDN Hosting

- Upload sitemap to root of S3/static site.
- Or configure a rewrite/redirect to dynamic route.

---

## 🧠 Summary

| Approach                     | Source                          | URL Mapping Logic                         |
|-----------------------------|----------------------------------|--------------------------------------------|
| Dynamic (backend)           | Generated at request             | Route: `/sitemap.xml` returns XML          |
| Static (build-time)         | Scripted into `public/` folder   | Served from root as static file            |
| CDN/S3                      | Uploaded or routed               | Configured via CDN or redirects            |

---

> **Tip:** Always test sitemap in Google Search Console for visibility.

