# 0. Introduction flow:

You have got the big picture of website in <a href="website_in_general.md">website_in_general</a>. Now we deeply dive into concepts.

## **Step-by-step Web Request Flow (Layered Architecture – Extended Version)**

### **1. User Enters a URL**

- **Layer:** Browser Interface
- The user types a URL (e.g., `https://example.com`) in the address bar.
- **Browser checks locally first:**

  - **Browser cache:** If the resource was recently loaded.
  - **Service Worker cache:** (if PWA) may intercept the request.
  - **DNS cache:** Local or OS-level cache for domain resolution.

---

### **2. DNS Resolution (Domain → IP)**

- **Layer:** Application Layer (DNS over UDP/TCP port 53, or DNS over HTTPS/QUIC for security)
- The browser asks the OS resolver to resolve `example.com` → IP.
- **Resolution chain:**
  Recursive Resolver → Root DNS → TLD (.com) → Authoritative DNS → IP Address.
- **Optimization:**

  - DNS responses are cached (local OS, browser, ISP, or recursive resolver).
  - Reduces latency for future requests.

- **Result:** Browser gets the IP address of the target server (e.g., `93.184.216.34`).

---

### **3. Network Communication (TCP/IP, Ports, Routing)**

- **Layer:** Transport & Internet Layers
- Browser opens a **TCP connection** to the IP via:

  - Port **80** for HTTP
  - Port **443** for HTTPS

- **Process:**

  1. **TCP Three-Way Handshake:** SYN → SYN-ACK → ACK.
  2. If HTTPS: **TLS Handshake** begins — exchanging keys, verifying certificate, establishing encryption.
  3. For modern web: **HTTP/3** uses **QUIC over UDP** (faster, less handshake overhead).

- **Routing:**
  IP packets travel through multiple routers, possibly NAT gateways, to reach the server.

---

### **4. HTTP/HTTPS Request (Client → Server)**

- **Layer:** Application Layer
- The browser constructs an **HTTP request**:

  ```
  GET /index.html HTTP/1.1
  Host: example.com
  User-Agent: Chrome/141.0
  Accept: text/html
  Cookie: session_id=xyz
  ```

- **If HTTPS:** All content is encrypted via TLS.
- **Special cases:**

  - **CORS:** May trigger a preflight `OPTIONS` request.
  - **Persistent connections:** Keep-Alive allows multiple requests over one TCP connection.

---

### **5. Server Backend Processing**

- **Layer:** Application & Business Logic Layers
- **Flow inside the server:**

  1. **Web Server Layer:** Nginx / Apache receives the request.

     - May serve static files directly (HTML, CSS, JS).
     - Or forward (reverse proxy) to backend app (e.g., Node.js, Spring Boot, Django).

  2. **Backend Application Layer:**

     - Parses the request (routes, controllers).
     - Executes business logic.
     - Interacts with:

       - **Database (SQL/NoSQL)** via ORM or raw queries.
       - **Cache Layer (Redis, Memcached)** for performance.
       - **External APIs or microservices**.

  3. **Response constructed:**

     - Usually in **JSON, HTML, or binary (images, files)**.
     - Includes status code, headers, and body.

---

### **6. Response Delivery**

- **Layer:** Application + Networking
- The backend sends back the **HTTP response**:

```
  HTTP/1.1 200 OK
  Content-Type: text/html; charset=UTF-8
  Content-Length: 5123
```

_(Response body follows)_

- **Possible routes before reaching the client:**

  - Reverse proxy → Load balancer → CDN → ISP → Browser.

- **Performance features:**

  - **Compression:** gzip, Brotli.
  - **Persistent connections:** Reuse TCP/TLS session.
  - **Multiplexing (HTTP/2, HTTP/3):** Send multiple responses in one connection.

---

### **7. Browser Rendering (Client-side)**

- **Layer:** Browser Rendering Engine (e.g., Blink, WebKit)
- **Rendering pipeline:**

  1. Parse **HTML** → Build **DOM Tree**.
  2. Parse **CSS** → Build **CSSOM Tree**.
  3. Combine → **Render Tree**.
  4. **Layout**: Calculate positions & sizes.
  5. **Paint**: Draw pixels.
  6. **Composite**: GPU-accelerated final render.

- **JavaScript Execution:**

  - Executes via JS Engine (V8, SpiderMonkey).
  - May modify DOM dynamically (e.g., React, Vue).
  - Asynchronous operations via event loop.

- **Modern frontends:**

  - **Hydration:** For SSR (Next.js, Nuxt).
  - **Lazy loading**, **code splitting** improve performance.

---

### **8. Optimization & Delivery Layer**

- **Layer:** Performance Enhancement / Delivery
- **Caching:**

  - **Browser cache:** Stores static assets.
  - **CDN (Content Delivery Network):** Edge servers serve content closer to users.
  - **Reverse proxy caching:** e.g., Nginx, Cloudflare.

- **Compression:** gzip, Brotli.
- **Connection optimization:** HTTP/2 multiplexing, HTTP/3 over QUIC.
- **Security:** SSL/TLS session reuse, HSTS, CSP, secure cookies.
- **Other optimizations:**

  - Minification (HTML/CSS/JS).
  - Image optimization (WebP, AVIF).
  - Prefetching / preloading resources.

---

 <img src="images/Screenshot from 2025-10-10 21-16-44.png">

---

Breakdown:

- DNS: resolves name -> IP.
- TCP/IP: makes sure two machines can talk reliably using that IP.
- HTTP/HTTPS: the actual web protocol to exchange requests & responses.
