Web Performance Optimization Checklist (LCP + TTFB)

🔹 Largest Contentful Paint (LCP)
1. Optimize When the LCP Resource Is Discovered
 - Place LCP-critical images directly in HTML (not lazy-loaded or JS-injected).
 - Avoid unnecessary HTML nesting around LCP element.
 - Inline critical CSS or load it with minimal blocking.

2. Optimize How the LCP Resource Is Discovered 
 - Add <link rel="preload"> for LCP images and fonts.
 - Use font-display: swap for fonts.
 - Don’t hide the LCP element behind JavaScript interactions or client-side routing delays.

3. Optimize How the LCP Resource Is Loaded
 - Serve images from a fast CDN or edge cache.
 - Use modern formats like WebP or AVIF.
 - Compress assets using gzip or brotli.
 - Leverage responsive images to avoid oversize payloads.

4. Optimize How the LCP Resource Is Rendered
 - Avoid layout shifts: define width, height, or aspect-ratio.
 - Eliminate unused CSS or JS to reduce render delay.
 - Minimize JavaScript execution that may delay layout.
 - Use system fonts or preload fonts to reduce FOIT (Flash of Invisible Text).

🔹 Time to First Byte (TTFB)
TTFB is the time it takes for the browser to receive the first byte of HTML from the server after a navigation.

🔧 Checklist to Reduce TTFB:
 - Use server-side caching for HTML (e.g., Redis, Varnish, CDN edge cache).
 - Minimize server processing time (optimize DB queries, avoid synchronous blocking).
 - Use CDNs for static and dynamic content if supported (e.g., Cloudflare, Fastly).
 - Avoid unnecessary redirects (301/302) before the final document.
 - Use HTTP/2 or HTTP/3 for faster handshakes and parallel requests.
 - Optimize DNS resolution (e.g., use fast DNS providers like Cloudflare DNS).
 - Reduce TLS handshake time (enable TLS session reuse and OCSP stapling).

