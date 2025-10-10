# 2. OSI Model -> TCP/IP Model (TCP/IP focused)

<img src="images/The-logical-mapping-between-OSI-basic-reference-model-and-the-TCP-IP-stack.png">

## 2.0. What is the OSI Model?

The OSI (Open Systems Interconnection) model is a conceptual framework that breaks down the functions of a networking system into seven distinct layers. Its main purpose is to standardize communication and provide a common language for networking, but the modern internet is based on the simpler TCP/IP model.

**The 7 Layers**

- Application: The layer closest to the user; interacts with software like browsers and email clients (HTTP, FTP).

- Presentation: Handles data translation, encryption, and compression to prepare it for the application.

- Session: Manages the communication sessions between two devices, including opening, managing, and closing them.

- Transport: Ensures reliable data delivery, managing flow - control and error correction. Key protocols are TCP and UDP.

- Network: Responsible for addressing and routing data packets across different networks, finding the best path. The main - protocol is IP.

- Data Link: Manages reliable data transmission between two - nodes on the same network (e.g., Ethernet).
- Physical: Refers to the physical hardware that transmits data, like cables and switches.

**Is it Still Used Today?**

Yes, but mostly as a theoretical and diagnostic tool. The OSI model is not directly implemented in most modern networks.

- Educational Tool: It's excellent for teaching and understanding complex networking concepts in a structured way.

- Troubleshooting: Network professionals use it to isolate problems by identifying which layer is failing.

- Reference Model: It provides a standard framework and vocabulary for developing new networking technologies.

---

## 2.1. Introduction: What is TCP/IP?

- Protocol Suite: A set of protocols that enable end-to-end communication.
- Network Model: Provides a layered framework for how data flows across networks.
- Foundation of the Internet: Every interaction on the Internet and most private networks relies on TCP/IP.

**TCP (Transmission Control Protocol)**

- Reliable, ordered delivery.
- Handles retransmission, error checking, flow control.
- Used in: HTTP/HTTPS, gRPC, database connections.

**IP (Internet Protocol)**

- Addressing and routing of packets.
- Ensures data gets to the correct device.
- IP = “where to send,” TCP = “how to deliver correctly.”

**How TCP/IP Works**

- Packetization: Application data is split into packets.
- Addressing: Each packet is labeled with source + destination IP addresses.
- Routing: Routers forward packets toward the destination.
- Reliable Delivery (TCP): Packets are reassembled in order, with error checking.

**TCP/IP merges some OSI layers:**

- Application + Presentation + Session → Application
- Data Link + Physical → Network Interface

---

### 2.2. Application Layer (OSI: Application + Presentation + Session → TCP/IP: Application)

This is the layer you work with the most.

- Work with interfaces, protocols, software.
- Protocols: HTTP/1.1, HTTP/2, HTTP/3, HTTPS, DNS, SMTP, FTP, gRPC, WebSocket.
- This is where APIs live.
- Examples in backend:
  - When you build an REST API, you’re designing at the Application Layer.
  - When your backend calls an external API (e.g., GET /users), that’s also Application Layer.
  - TLS encryption (HTTPS) also belongs here.

As a backend dev, when debugging a failing request, you usually check:

- Headers (Authorization, Content-Type, etc.)
- Response codes (200, 404, 500…)
- Certificates (TLS/SSL)

---

### 2.3. Transport Layer (OSI: Transport → TCP/IP: Transport)

This layer makes sure data arrives correctly.

- Error-free data delivery between host -> destination nodes.
- Protocols: TCP, UDP.
- TCP:
  - Connection-oriented (3-way handshake: SYN → SYN-ACK → ACK).
  - Reliable (guarantees order & delivery).
  - Used by HTTP, HTTPS, gRPC, database connections.
- UDP:
  - Connectionless, faster, no guarantee of delivery.
  - Used for DNS queries, video streaming, gaming.
- Example for backend:
  - When your API is slow because of many half-open TCP connections → you might hit TIME_WAIT or socket exhaustion.
  - If you deploy a WebSocket or gRPC service, you’re still on TCP.

---

### 2.4. Network Layer (OSI: Network → TCP/IP: Internet)

This layer decides where the packet goes (routing).

- Package data into IP packets; transmit packets across the network.
- Protocols: IP (IPv4, IPv6), ICMP (ping).
- Key ideas:
  - IP address identifies devices.
  - Routers move packets across networks.
  - TTL (Time To Live) prevents infinite loops.
- Example for backend:
  - When your server can’t reach a database in another subnet → likely a routing issue.
  - When DNS resolves correctly but your request still times out → maybe firewall or routing problem.

---

### 2.5. Link/Physical Layers (OSI: Data Link + Physical → TCP/IP: Network Interface)

This is about hardware and direct connections.

- Transmit bits across the network
- Examples: Ethernet cables, Wi-Fi, MAC addresses.

As a backend developer, you almost never touch this directly, but it matters when:

- Debugging why a server NIC (network card) is misconfigured.
- Dealing with Docker/Kubernetes networking (bridge, overlay networks).

---

### 2.6. Summary and example

**SUMMARY**

- TCP/IP = protocol suite + network model → foundation of the Internet.
- TCP (Transmission Control Protocol)
  - Reliable, ordered delivery of data.
  - Handles retransmission, error checking, flow control.
  - Used in: HTTP/HTTPS, APIs, databases.
- IP (Internet Protocol)
  - Handles addressing + routing.
  - Each packet has source + destination IP.
  - IP = “where to send”, TCP = “how to deliver correctly.”
- How it works:
  - Data split into packets.
  - IP adds addressing.
  - Routers forward packets.
  - TCP reassembles + checks integrity.
- TCP/IP vs OSI:
  - OSI (7 layers) → TCP/IP (4 layers).
  - Application (HTTP, DNS), Transport (TCP/UDP), Internet (IP), Network Interface.
- Backend developer view:
  - You mostly touch Application (HTTP, gRPC, DNS),
  - sometimes Transport (TCP vs UDP decisions),
  - and need to understand IP addressing/routing basics.

| **Layer**     | **What You Do as Backend Dev**                           |
| ------------- | -------------------------------------------------------- |
| Application   | Build APIs (HTTP, gRPC, WebSocket), use TLS, DNS lookups |
| Transport     | Handle TCP connections, sockets, connection pooling      |
| Network       | Deal with IPs, routing, firewall rules, VPNs             |
| Link/Physical | Mostly invisible, but relevant in container networking   |

**EXAMPLE:** How a client calls your api

- Application Layer: Browser sends `GET /users request(HTTP)`.
- Transport Layer: Wrapped in TCP segment, reliable connection established.
- Network Layer: Packet gets destination IP (server's public IP)
- Link/Physical Layer: Transmitted over Ethernet/Wi-Fi until it reaches server.
  When server responds, the flow goes back up the layers to the client.

**EXAMPLE:** TCP Example - REST API / Database

Suppose you build an REST API:

```http
GET /users HTTP/1.1
Host: api.example.com
```

Behind the scenes:

- HTTP request = Application layer.
- Runs on top of TCP (port 443 for HTTPS).
- TCP ensures the request and response arrive in order and intact.
- If one packet is lost, TCP retransmits it.
- Without TCP, your API might return broken JSON (half the response missing).
- Without TCP, Connection pooling in databases (like PostgreSQL/MySQL) is managing TCP sockets.

**EXAMPLE:** UDP Example: DNS Query

- Your backend calls an external API at api.stripe.com.
- Before sending the HTTP request, your server asks DNS:

```csharp
What is the IP of api.stripe.com?
```

- DNS runs on UDP (port 53) → fast, no need for reliability.
- If a packet is lost, client just retries (instead of waiting).

Why it matters?

- If DNS resolution fails, your backend can’t even start the HTTP request.
- This is why misconfigured DNS often causes “server unreachable” errors.
