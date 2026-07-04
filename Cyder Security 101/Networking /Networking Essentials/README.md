# Lab Room: Networking Essentials
### Date Completed: 2 July 2026
### Tools: tryhackme attack box

---

## 📁 TASK 2: DHCP (Dynamic Host Configuration Protocol)

### 1. Architectural Definition & Core Purpose

DHCP is an **Application Layer** protocol designed to automate the assignment of IP addresses and network configuration parameters to devices on an Internet Protocol (IP) network. It operates on a **Client-Server model** utilizing the unreliable transport capabilities of the **User Datagram Protocol (UDP)**.

* **DHCP Server Port:** Listens on **UDP Port 67**.
* **DHCP Client Port:** Originates and listens on **UDP Port 68**.

Without DHCP, network administrators would have to manually input static IP addresses, subnet masks, default gateways, and DNS servers for every endpoint. This leads to administrative overhead and **IP Address Conflicts** (where two hosts are assigned identical IP addresses, completely disrupting network stack operations for both local and Internet traffic).

### 2. The DORA Process (Deep Packet Analysis)

When a client device initializes its network interface card (NIC), it possesses no Layer 3 configuration. It undergoes a four-step handshake colloquially known as **DORA**:

```
Client (Port 68)                                     Server (Port 67)
     |                                                      |
     | ------------ DHCPDISCOVER (Broadcast) -------------> |
     |                                                      |
     | <------------ DHCPOFFER (Unicast) ------------------ |
     |                                                      |
     | ------------ DHCPREQUEST (Broadcast) --------------> |
     |                                                      |
     | <------------ DHCPACK (Unicast) -------------------- |

```

* **DHCP Discover (D):** The unconfigured client broadcasts a packet to discover any active DHCP server within its local broadcast domain. Because it has no IP, the **Source IP is set to `0.0.0.0**` and the **Destination IP is `255.255.255.255**` (Limited Broadcast). At Layer 2, it targets the broadcast MAC address `ff:ff:ff:ff:ff:ff`.
* **DHCP Offer (O):** An active DHCP server intercepts the broadcast, selects an available IP from its designated address pool (scope), and unicasts a proposal back to the client. This packet includes the proposed IP address, subnet mask, lease duration, default gateway, and primary/secondary DNS servers.
* **DHCP Request (R):** The client accepts the offer. It broadcasts a `DHCPREQUEST` packet back to the network. It is explicitly broadcasted so that if multiple DHCP servers provided offers, the other servers realize their offers were declined and can safely reclaim those IP addresses into their active pools.
* **DHCP Acknowledge (A):** The server finalizes the transaction by sending a unicast `DHCPACK` packet, confirming that the network configuration parameters are now officially leased to that specific client MAC address for a predefined time frame.

---

## 📁 TASK 3: ARP (Address Resolution Protocol)

### 1. The Protocol Hybrid Nature & Purpose

ARP acts as the critical bridge mapping **Layer 3 (Network Layer) Network Addresses (IP)** to **Layer 2 (Data Link Layer) Physical Addresses (MAC)**. While software and applications rely on IP logic to determine end-to-end data paths, physical network hardware (like Ethernet switches or WiFi access points) requires 48-bit MAC addresses encoded in hexadecimal notation to physically move frames across a local area network (LAN).

> **Note on OSI Layer Placement:** ARP is a unique protocol. Because it formats headers that work directly over Layer 2 frames but serves the explicit requirements of Layer 3 IP mapping, it is technically categorized as a **Layer 2 protocol** (or Layer 2.5 by some engineers) since it does not utilize IP encapsulation headers.

### 2. Operation Mechanics: Request and Reply

Devices maintain a local, volatile cache table known as the **ARP Table** or **ARP Cache**. When Host A (`192.168.66.89`) needs to transmit an IP packet to Host B (`192.168.66.1`), it checks its local ARP cache. If the MAC address is absent, it initiates an ARP transaction:

* **ARP Request:** Host A encapsulates an ARP request *directly inside an Ethernet Frame*. The frame contains Host A's source MAC, but the destination MAC is set to the **Broadcast MAC address (`ff:ff:ff:ff:ff:ff`)**. The payload explicitly asks: *"Who has IP 192.168.66.1? Tell 192.168.66.89."* Every host in the local layer 2 broadcast domain processes this frame.
* **ARP Reply:** Host B recognizes its own IP inside the payload. It discards the broadcast wrapper and directly constructs a **Unicast ARP Reply** targeting Host A's specific MAC address. The payload explicitly declares: *"`192.168.66.1` is at `44:df:65:d8:fe:6c`."*

Host A receives the frame, extracts the MAC address, updates its local ARP Cache table, and successfully constructs the regular data frames to proceed with standard network communications.

---

## 📁 TASK 4: ICMP (Internet Control Message Protocol)

### 1. Protocol Architecture

Unlike TCP or UDP, ICMP does not carry upper-layer application user data. It is a **Layer 3 companion protocol** encapsulated directly within IP packets, strictly utilized for out-of-band **network diagnostics, error reporting, and control operations**.

### 2. Deep Dive: The Ping Utility

`ping` verifies end-to-end Layer 3 reachability between two hosts and calculates the **Round-Trip Time (RTT)**. It operates using two fundamental ICMP message structures:

* **ICMP Echo Request (Type 8, Code 0):** Sent from the source host to the destination host.
* **ICMP Echo Reply (Type 0, Code 0):** Sent from the destination host back to the source if it is alive and not restricted by security parameters (like stateless/stateful firewalls dropping ICMP traffic).

#### Packet Payload Analysis

* Linux environments typically inject a default payload of **56 bytes** into the ICMP data field, yielding an 84-byte IP packet ($56 \text{ data} + 8 \text{ ICMP header} + 20 \text{ IPv4 header}$).
* Windows environments commonly inject a default payload of **32 bytes** into the ICMP data field.
* As established in our lab analysis, custom configurations can tailor the payload size (e.g., **40 bytes** of raw payload data).

### 3. Deep Dive: The Traceroute/Tracert Utility

`traceroute` maps the exact chronological path of Layer 3 hops (routers) a packet traverses to reach its destination. It relies on a clever manipulation of the **Time-to-Live (TTL)** field inside the IPv4 header.

#### The TTL Mechanism

TTL is an 8-bit field designed to prevent packets from looping infinitely on the Internet. Each time an IP packet passes through a router hop, that router decrements the TTL value by **1**. If the TTL value hits **0**, the router drops the packet and transmits an **ICMP Time Exceeded message (Type 11, Code 0)** back to the original source.

#### Traceroute Execution Process:

1. **Probe 1 (TTL=1):** The source sends a packet with TTL=1. The very first router decrements it to 0, drops it, and replies with an *ICMP Type 11*. The source logs Router 1's IP and measures the RTT.
2. **Probe 2 (TTL=2):** The source sends a packet with TTL=2. Router 1 decrements it to 1 and passes it on. Router 2 decrements it to 0, drops it, and replies with an *ICMP Type 11*. The source logs Router 2's IP.
3. This incrementation continues ($TTL=3, 4, 5...$) until the packet successfully reaches the intended target host, which will respond with a standard port unreachable or echo reply message, concluding the route discovery.

---

## 📁 TASK 5: Routing

### 1. Functional Definition

Routing is the process performed by Layer 3 devices (routers) to forward packets across interconnected networks from a source to a remote destination. Routers maintain a **Routing Table** containing a list of known networks, interfaces, and metric costs. They run complex mathematical **Routing Algorithms** to determine the optimal mathematical pathway for data transmission.

### 2. Major Routing Protocols Explored

#### A. OSPF (Open Shortest Path First)

* **Category:** Interior Gateway Protocol (IGP) / Link-State.
* **Mechanism:** Every router running OSPF shares the state of its directly connected links with all other routers within its area. This ensures every router possesses an identical, comprehensive **topology map** of the network. It calculates the absolute shortest path to a destination using **Dijkstra’s Algorithm**, prioritizing bandwidth speed as its primary metric cost.

#### B. EIGRP (Enhanced Interior Gateway Routing Protocol)

* **Category:** Interior Gateway Protocol (IGP) / Advanced Distance-Vector (Hybrid).
* **Mechanism:** A Cisco proprietary protocol (now open-standard but predominantly Cisco-centric). It shares routing tables only with direct neighbors but utilizes a composite metric consisting of **bandwidth, delay, reliability, and load** to determine optimal pathways. It uses the **DUAL (Diffusing Update Algorithm)** to guarantee loop-free backup routes for ultra-fast convergence times.

#### C. BGP (Border Gateway Protocol)

* **Category:** Exterior Gateway Protocol (EGP) / Path-Vector.
* **Mechanism:** Known explicitly as the **"Protocol of the Internet."** Unlike IGPs that route traffic inside an organization, BGP routes traffic *between* different ISP networks and mega-corporations, known as **Autonomous Systems (AS)**. It makes routing decisions based on network policies, path attributes, and organizational rules rather than raw technical speeds.

#### D. RIP (Routing Information Protocol)

* **Category:** Interior Gateway Protocol (IGP) / Distance-Vector.
* **Mechanism:** A legacy, highly simplified routing protocol. It measures the value of a route based strictly on **Hop Count** (the raw number of routers a packet must cross). It has a hard architectural limitation of a **maximum of 15 hops** (16 is deemed completely unreachable). It ignores path quality, congestion, and bandwidth speeds.

---

## 📁 TASK 6: NAT (Network Address Translation)

### 1. Core Purpose & The IPv4 Exhaustion Crisis

The theoretical maximum capacity of the 32-bit IPv4 address scheme sits at $2^{32} \approx 4.29 \text{ billion}$ unique addresses. Given the exponential growth of Internet-reliant devices, IPv4 address depletion became an immediate threat. NAT was engineered as a primary mitigation tool.

NAT allows an entire private intranet network utilizing **RFC 1918 Private IP Spaces** (e.g., `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) to communicate out to the public Internet by sharing a singular, or small pool of, globally unique **Public IP Addresses**.

### 2. Technical Operations: PAT & The NAT Translation Table

The most common implementation of NAT is **PAT (Port Address Translation)** or NAT Overload.

When an internal host initiates an outbound TCP/IP connection, the NAT-enabled perimeter router intercepting the packet modifies both the Layer 3 and Layer 4 headers dynamically:

* **Outbound Translation:** The router swaps out the internal host's private Source IP and replaces it with the router's public-facing WAN IP. Simultaneously, it maps the connection to a unique source TCP/UDP port chosen from its **Ephemeral Port Pool**.
* **The NAT Table:** The router writes this precise state mapping directly into its internal allocation table:

$$\text{Private IP : Private Port} \Longleftrightarrow \text{Public IP : Translated Public Port}$$


* **Inbound Translation:** When the remote web server replies, it directs its packets to the router's public IP and the designated translated port. The router performs a reverse look-up inside its NAT Table, swaps the public destination details back to the host's original private IP and private port, and forwards the frame seamlessly.

### 3. Theoretical Port Limits

Because PAT relies on mapping unique Layer 4 port allocations, a single public IPv4 address is mathematically constrained by the 16-bit size of the TCP/UDP port field. There are exactly **65,536 available ports** ($2^{16}$). Subtracting system-reserved well-known ports ($0 - 1023$), a single public IP address processed by a router with infinite performance capability can theoretically handle approximately **64,000 parallel concurrent outbound TCP connections**.

---

https://share.gemini.google/YI4srmSP08wn
