## 6. Browser Rendering

### 6.1. Rendering Pipeline (Simplified)

1. Browser receives HTML, CSS, JS.
2. Builds **DOM** (Document Object Model).
3. Builds **CSSOM** (CSS Object Model).
4. Combines both → **Render Tree**.
5. Paints pixels on screen.

### 6.2. Frontend Technologies

- **HTML**: Structure
- **CSS**: Style
- **JavaScript**: Interactivity

### 6.3. Importance for Backend Developers

Understanding how browsers interpret your backend responses helps optimize API response structure, performance, and caching.

---

## 7. Caching, CDN, and Proxy Layers

### 7.1. Caching

Stores frequently accessed data to improve speed.

- **Browser cache**
- **Server cache**
- **Reverse proxy cache (e.g., Nginx, Varnish)**

### 7.2. CDN (Content Delivery Network)

Distributes static assets across global servers to minimize latency for users worldwide.

### 7.3. Proxy and Load Balancer

- **Forward proxy:** Acts on behalf of the client.
- **Reverse proxy:** Acts on behalf of the server (common in backend).
- **Load balancer:** Distributes requests among multiple servers.

### 7.4. Compression & Optimization

Techniques like Gzip, Brotli, and image compression reduce bandwidth and speed up load time.
