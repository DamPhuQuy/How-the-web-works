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

- Knows which Authoritative server holds the records for the domain.
- Example: `.com` TLD server says “the authoritative server for `example.com` is at X.”

### 1.2.4. Authoritative Nameserver

- Final source of truth; return the actual IP (A/AAAA record, etc).
- Only the authoritative nameserver gives the real IP. The others are just guides along the way.
- Example: “example.com = 93.184.216.34”.

### 1.3. How a DNS Query Works

1. Browser → Resolver

   - User types www.example.com in browser.
   - Browser asks the DNS recursive resolver (e.g., ISP’s resolver or public one like `8.8.8.8`).
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

### 1.4. DNS records

| Record    | Purpose                                           | Example                               |
| --------- | ------------------------------------------------- | ------------------------------------- |
| **A**     | Maps hostname → IPv4 address                      | `api.example.com → 192.0.2.1`         |
| **AAAA**  | Maps hostname → IPv6 address                      | `api.example.com → 2001:db8::1`       |
| **CNAME** | Alias (points one name to another)                | `www.example.com → example.com`       |
| **MX**    | Mail servers for a domain                         | `example.com → mail.example.com`      |
| **TXT**   | Arbitrary text (used for SPF, DKIM, verification) | `v=spf1 include:_spf.google.com ~all` |
| **NS**    | Delegates domain to name servers                  | `example.com → ns1.provider.com`      |
