# 0. Introduction flow:

You have got the big picture of website in <a href="website_in_general.md">website_in_general</a>. Now we deeply dive into concepts.

**Step-by-step flow with layered architecture**

**1. User enters a URL**

- The browser receives the domain (e.g., example.com) from the user.

**2. DNS Resolution (Domain → IP)**

- Layer: DNS / Application Layer

- The domain name is translated into an IP address using DNS servers (Recursive → Root → TLD → Authoritative).

- Result: Browser knows which server to contact.

**3. Network Communication (TCP/IP, Ports, Routing)**

- Layer: Networking Layer

- The browser establishes a TCP connection to the server’s IP via port 80 (HTTP) or 443 (HTTPS).

- If HTTPS is used, a TLS handshake encrypts the communication.

**4. HTTP/HTTPS Request (Client → Server)**

- Layer: Application Layer (HTTP)

- The browser sends an HTTP request (GET, POST, etc.) to the web server, containing headers, cookies, and optional body data.

**5. Server Backend **Processing\*\*\*\*

- Layer: Server / Backend Application

- The web server (e.g., Nginx, Apache) receives the request and forwards it to the backend application (e.g., Node.js, Java, Python).

- The backend executes business logic, interacts with the database, and prepares a response (HTML, JSON, etc.).

**6. Response Delivery**

- Layer: Application + Networking Layers

- The server sends back an HTTP response with status codes, headers, and content (HTML, CSS, JSON, images…).

- his data travels through the same TCP/TLS connection to the browser.

**7. Browser Rendering (Client-side)**

- Layer: Browser Rendering Engine

- The browser parses HTML → builds DOM

- Parses CSS → builds CSSOM

- Executes JavaScript → updates DOM dynamically

- Finally, renders the visual web page to the screen.

**8. ptimization Layer (Performance Enhancements)**

- Layer: Caching & Delivery Layer

- CDNs, proxies, and browser caching store static resources for faster subsequent loads.

- Compression (gzip, Brotli), HTTP/2 multiplexing, and SSL/TLS session reuse further reduce latency and bandwidth.

---

```pgsql
You type:  example.com
           |
           v
+------------------+
|   DNS Lookup     |   🔎 "What is the IP of example.com?"
+------------------+
           |
           v
+------------------+
|   TCP/IP Layer   |   📦 Use the IP to establish a connection (TCP handshake)
+------------------+
           |
           v
+------------------+
|   HTTP/HTTPS     |   🌐 Send/receive web data over the TCP connection
+------------------+
```

---

```pgsql
[ You / Browser ]
        |
        | 1. Type "http://example.com"
        v
+-------------------+
|   DNS Resolver    |  <-- asks "What is example.com?"
+-------------------+
        |
        | 2. DNS Query (UDP/TCP 53)
        v
+-------------------+       +---------------------+
| Root DNS Server   | --->  | .com DNS Server     |
+-------------------+       +---------------------+
                                   |
                                   v
                           +---------------------+
                           | example.com DNS     |
                           | -> 93.184.216.34    |
                           +---------------------+

        |
        | 3. DNS Response: IP = 93.184.216.34
        v
[ You / Browser ]
        |
        | 4. Open TCP connection (port 80/443)
        v
+--------------------------------------------------+
|   Web Server at 93.184.216.34                    |
|                                                  |
|   <--- HTTP Request: GET / HTTP/1.1 ------------ |
|   ---> HTTP Response: 200 OK + HTML ------------ |
+--------------------------------------------------+
```

---

Breakdown:

- DNS: resolves name -> IP.
- TCP/IP: makes sure two machines can talk reliably using that IP.
- HTTP/HTTPs: the actual web protocol to exchange requests & responses.
