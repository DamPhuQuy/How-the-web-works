# 3. IP Address

## 3.1. What is an IP Address ?

- Definition: An Internet Protocol (IP) address is a unique identifier for a device on a network.
- Acts like a postal address for computers — tells data where to go and where it came from.
- Written in dot-decimal (IPv4) or colon-hexadecimal (IPv6) form.

## 3.2. **Ipv4 vs Ipv6**

<img src="images/66afa856-5f56-4353-8376-ad114bbfefbc.png">

**IPv4 (32-bit)**

- Example: 18.154.227.99
- Format: Dotted-decimal (4 numbers separated by dots).
- Each number = 1 octet (8 bits).
- Example breakdown to bit:
- 18 → 00010010
- 154 → 10011010
- 227 → 11100011
- 99 → 01100011

→ IPv4 = total 32 bits (4 × 8 bits).

Features:

- ~4.3 billion possible addresses.
- Widely used, but running out of addresses.

**IPv6 (128-bit)**

- Example: 2001:0DB8:AC10:FE01::
- Format: Colon-hexadecimal (8 groups of 4 hex digits).
- Each hex digit = 4 bits, so each group = 16 bits.
- Example breakdown to bit:
- 2001 → 0010000000000001
- 0DB8 → 0000110110111000
- AC10 → 1010110000010000
- FE01 → 1111111000000001
- Remaining groups are zeros (compressed as ::).

→ IPv6 = total 128 bits (8 × 16 bits).

Features:

- Almost unlimited addresses (3.4×10³⁸).
- Built to replace IPv4.

## 3.3. Public vs Private IP

**Public IP:**

- Unique across the internet.
- Assigned by ISPs (e.g., 203.0.113.25).
- Used for web servers, APIs, etc.

**Private IP:**

- Used inside local networks (e.g., 192.168.x.x, 10.x.x.x).
- Not directly accessible from the internet.
- Backend devs often configure databases or internal services with private IPs.

## 3.4. Static vs Dynamic IP

- Static IP: Fixed, doesn’t change (used for servers).
- Dynamic IP: Assigned temporarily by DHCP (common for home devices).

## **3.5. Subnetting**

### 3.5.1. IP Address Structure

<img src="images/fefefefee.png">

An IP address has two parts:

- Network ID → Identifies the network.
- Host ID → Identifies a device inside the network.

Example:
`192.168.1.10/24`

- Network part = `192.168.1`
- Host part = `10`
- `/24` = **Subnet Mask length** (24 bits for network, 8 bits for hosts).

### 3.5.2. Subnet Mask

<img src="images/weqeqwe.png">

A subnet mask tells us how many bits are for the network and how many for hosts.

Example:

`/24` = `255.255.255.0`

→ Network = first 24 bits.

→ Host = last 8 bits (2⁸ – 2 = 254 usable hosts).

`/16` = `255.255.0.0`

→ 2¹⁶ – 2 = 65,534 hosts.

`/30`= `255.255.255.252`

→ 2² – 2 = 2 usable hosts.

## 3.6. Key Formulas and Backend Dev Relevance

**Key Formulas:**

- Number of hosts per subnet = 2^(host bits) – 2
  (subtract 2 for network address & broadcast address).

- Number of subnets = 2^(borrowed bits).

**Backend Dev Relevance:**

- Deploying apps: Need to know your server’s public IP.
- Connecting to databases: Often use private IPs inside a VPC.
- Scaling apps: Must understand subnetting + routing when setting up clusters.
- Security: Configure firewalls to allow/deny specific IP ranges.

## 3.7. Summary and example

Example of Subnetting

Suppose you have:
`192.168.1.0/24` (256 IP addresses: 0–255).

Split into 2 subnets (/25):

- `192.168.1.0/25` → Hosts: 0–127 (126 usable).
- `192.168.1.128/25` → Hosts: 128–255 (126 usable).
