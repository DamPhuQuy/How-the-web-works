# Basic web concepts

# Note for learning basic networking

# Table of contents

- [Basic web concepts](#basic-web-concepts)
- [Note for learning basic networking](#note-for-learning-basic-networking)
- [Table of contents](#table-of-contents)
- [1. DNS (Domain Name System)](#1-dns-domain-name-system)
  - [1.1. What is DNS?](#11-what-is-dns)
  - [1.2. The 4 DNS Servers in web loading](#12-the-4-dns-servers-in-web-loading)
    - [1.2.1. DNS Recursor (or DNS Resolver)](#121-dns-recursor-or-dns-resolver)
    - [1.2.2. Root Nameserver](#122-root-nameserver)
    - [1.2.3. TLD Nameserver](#123-tld-nameserver)
    - [1.2.4. Authoritative Nameserver](#124-authoritative-nameserver)
  - [1.3. How a DNS Query Works](#13-how-a-dns-query-works)
  - [1.4. DNS records](#14-dns-records)
  - [1.5. Summary:](#15-summary)
- [2. OSI Model -\> TCP/IP Model (TCP/IP focused)](#2-osi-model---tcpip-model-tcpip-focused)
  - [2.1. Introduction: What is TCP/IP?](#21-introduction-what-is-tcpip)
    - [2.2. Application Layer (OSI: Application + Presentation + Session → TCP/IP: Application)](#22-application-layer-osi-application--presentation--session--tcpip-application)
    - [2.3. Transport Layer (OSI: Transport → TCP/IP: Transport)](#23-transport-layer-osi-transport--tcpip-transport)
    - [2.4. Network Layer (OSI: Network → TCP/IP: Internet)](#24-network-layer-osi-network--tcpip-internet)
    - [2.5. Link/Physical Layers (OSI: Data Link + Physical → TCP/IP: Network Interface)](#25-linkphysical-layers-osi-data-link--physical--tcpip-network-interface)
    - [2.6. Summary and example](#26-summary-and-example)

# 1. DNS (Domain Name System)

## 1.1. What is DNS?

- The Domain Name System (DNS) is the `phonebook of the Internet`.
- Humans access information online through `domain names`, like `nytimes.com` or `espn.com`.
- Web browsers interact through `Internet Protocol (IP) addresses`.
- DNS translates domain names to `IP addresses` so browsers can load Internet resources.

- Each device connected to the Internet has a unique IP address which other machines use to find the device.
- DNS servers eliminate the need for humans to memorize IP addresses such as 192.168.1.1 (in IPv4), or more complex newer alphanumeric IP addresses such as 2400:cb00:2048:1::c629:d7a2 (in IPv6).

## 1.2. The 4 DNS Servers in web loading

### 1.2.1. DNS Recursor (or DNS Resolver)

- First server your device asks; it hunts down the answer by querying others.
- Acts like a middleman: it doesn’t know the answer but knows where to ask.
- Usually provided by your ISP, Google DNS `(8.8.8.8)`, or Cloudflare `(1.1.1.1)`.

### 1.2.2. Root Nameserver

- Knows which `Top-Level Domain (TLD)` server at to contact (e.g: `.com`, `.org`)
- Example: for `example.com`, the root says “ask the `.com` server.”

### 1.2.3. TLD Nameserver

- Stands for top-level domain (TLD) is the final segment of a domain name, appearing after the last dot, such as .com, .org, or .vn.
- Knows which Authoritative server holds the records for the domain.
- Example: `.com` TLD server says “the authoritative server for `example.com` is at X.”

### 1.2.4. Authoritative Nameserver

- Final source of truth; return the actual IP (A/AAAA record, etc).
- Only the authoritative nameserver gives the real IP. The others are just guides along the way.
- Example: “example.com = 93.184.216.34”.

## 1.3. How a DNS Query Works

1. Browser → Resolver

   - User types www.example.com in browser.
   - Browser asks the DNS recursive resolver (e.g., ISP’s resolver or public one like `8.8.8.8`).
   - ISP stands for Internet Service Provider, a company that provides users with access to the internet, acting as a "bridge" between a device and the global network.
   - Resolver checks its cache. If not found → continues the query.

2. Resolver → Root Nameserver (.)

   - Resolver queries the root nameserver.
   - Root replies: “I don’t know the IP, but the `.com` TLD server does.”

3. Resolver → TLD Nameserver (`.com`)

   - Resolver asks the `.com` TLD server.
   - TLD replies: “I don’t have the IP, but here’s the authoritative nameserver for `example.com`.”

4. Resolver → Authoritative Nameserver (example.com)

   - Resolver queries the authoritative server.
   - Authoritative server responds: www.example.com = `93.184.216.34`.

5. Resolver → Browser

   - Resolver caches the result (for faster future lookups).
   - Resolver sends the IP back to the browser.
   - Browser can now connect directly to the web server at `93.184.216.34`.

The browser is able to make the request for the web page:

- The browser makes a HTTP request to the IP address.
- The server at that IP returns the webpage to be rendered in the browser.

**In short**: Browser → Resolver → Root → TLD → Authoritative → Resolver → Browser → Website.

<img src="images/complete-dns-lookup-and-webpage-query.png">
<img src="images/6793ff79-193f-4c1d-a0c9-77fbdf4f6002.png">

## 1.4. DNS records

| Record    | Purpose                                           | Example                               |
| --------- | ------------------------------------------------- | ------------------------------------- |
| **A**     | Maps hostname → IPv4 address                      | `api.example.com → 192.0.2.1`         |
| **AAAA**  | Maps hostname → IPv6 address                      | `api.example.com → 2001:db8::1`       |
| **CNAME** | Alias (points one name to another)                | `www.example.com → example.com`       |
| **MX**    | Mail servers for a domain                         | `example.com → mail.example.com`      |
| **TXT**   | Arbitrary text (used for SPF, DKIM, verification) | `v=spf1 include:_spf.google.com ~all` |
| **NS**    | Delegates domain to name servers                  | `example.com → ns1.provider.com`      |

## 1.5. Summary:

- DNS = Internet's phonebook
- Purpose: Translates human-readable names (`example.com`) -> machine IP address (e.g: `93.184.216.34`)
- Without DNS -> we would have to memorize IPs instead of names.
- 4 DNS Server in action:
  - Recursive Resolver: first stop, usually your ISP, Google (`8.8.8.8`) or Cloudflare (`1.1.1.1`). Acts as a middleman to find the answer.
  - Root Nameserver (.): Knows which TLD server to ask (e.g: `.com`, `.org`)
  - TLD Nameserver: Knows which authorities server stores the domain's records (e.g: `example.com`)
  - Authoritative Nameserver: The source of truth - returns the real IP (A/AAAA record) (e.g: `www.example.com` = `93.184.216.34`).
- Flow: Browser -> Resolver -> Root -> TLD -> Authoritative -> back to Resolver -> Browser -> Server.

# 2. OSI Model -> TCP/IP Model (TCP/IP focused)

<img src="images/The-logical-mapping-between-OSI-basic-reference-model-and-the-TCP-IP-stack.png">

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

### 2.2. Application Layer (OSI: Application + Presentation + Session → TCP/IP: Application)

This is the layer you work with the most.

- Work with interfaces, protocols, software.
- Protocols: HTTP/1.1, HTTP/2, HTTP/3, HTTPS, DNS, SMTP, FTP, gRPC, WebSocket.
- This is where APIs live.
- Examples in backend:
  - When you build a REST API, you’re designing at the Application Layer.
  - When your backend calls an external API (e.g., GET /users), that’s also Application Layer.
  - TLS encryption (HTTPS) also belongs here.

As a backend dev, when debugging a failing request, you usually check:

- Headers (Authorization, Content-Type, etc.)
- Response codes (200, 404, 500…)
- Certificates (TLS/SSL)

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

### 2.5. Link/Physical Layers (OSI: Data Link + Physical → TCP/IP: Network Interface)

This is about hardware and direct connections.

- Transmit bits across the network
- Examples: Ethernet cables, Wi-Fi, MAC addresses.

As a backend developer, you almost never touch this directly, but it matters when:

- Debugging why a server NIC (network card) is misconfigured.
- Dealing with Docker/Kubernetes networking (bridge, overlay networks).

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

- Application Layer: Browser sends `GET /users request(HTTP).
- Transport Layer: Wrapped in TCP segment, reliable connection established.
- Network Layer: Packet gets destination IP (server's public IP)
- Link/Physical Layer: Transmitted over Ethernet/Wi-Fi until it reaches server.
  When server responds, the flow goes back up the layers to the client.

**EXAMPLE:** TCP Example - REST API / Database

Suppose you build a REST API:

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
