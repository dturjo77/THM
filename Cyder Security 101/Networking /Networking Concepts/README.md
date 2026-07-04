# Lab Room: Networking Concepts
### Date Completed: 2 July 2026
### Tools: tryhackme attack box

---

# Comprehensive Networking Reference Notebook

## Task 1: Introduction to Networking & Core Learning Vectors

Task 1 establishes the foundational requirements for tracking how data flows across the internet and understanding data routing.

### Core Concepts Defined

* **IP Address (Internet Protocol Address):** A unique logical identifier assigned to every device connected to a network utilizing the TCP/IP protocol suite. It serves two primary functions: host or network interface identification and location addressing.
* **TCP Port Number:** A 16-bit logical address used at the Transport Layer to direct data to specific applications or processes running on a host.

---

## Task 2: The ISO OSI Reference Model

The **Open Systems Interconnection (OSI)** model is a theoretical 7-layer framework developed by the International Organization for Standardization (ISO). It standardizes network communications by dividing them into isolated, functional layers.

### Layer-by-Layer In-Depth Analysis

#### Layer 7: Application Layer

* **Function:** Directly interfaces with and provides network services to end-user software applications. It recognizes application requirements and initiates requests.
* **Key Protocols:** * `HTTP/HTTPS` (Hypertext Transfer Protocol / Secure) – Web browsing.
* `FTP` (File Transfer Protocol) – File transfers.
* `DNS` (Domain Name System) – Resolves domain names to IP addresses.
* `SMTP/POP3/IMAP` – Email routing and retrieval.



#### Layer 6: Presentation Layer

* **Function:** Acts as the data translator. It handles data formatting, syntax transformation, compression, and encryption/decryption, ensuring that data sent from Layer 7 of one system can be read by Layer 7 of another.
* **Standards/Examples:** `MIME` (encodes binary files into 7-bit ASCII for email), `JPEG`, `PNG`, `ASCII`, `Unicode`, and SSL/TLS encryption mapping.

#### Layer 5: Session Layer

* **Function:** Establishes, manages, synchronizes, and terminates continuous dialogues (sessions) between applications on separate hosts. It handles authentication and checkpointing to recover from mid-stream failures.
* **Examples:** `NFS` (Network File System), `RPC` (Remote Procedure Call).

#### Layer 4: Transport Layer

* **Function:** Responsible for end-to-end communication, flow control, error checking, and data segmentation between specific application processes.
* **Protocols:** `TCP` (reliable, connection-oriented) and `UDP` (fast, connectionless).

#### Layer 3: Network Layer

* **Function:** Manages logical addressing and routing across diverse, interconnected networks. It determines the optimal physical path for data packets based on network topography.
* **Protocols & Hardware:** `IP` (IPv4/IPv6), `ICMP` (ping/error messages), `IPSec` (VPN encryption). **Routers** operate at this layer.

#### Layer 2: Data Link Layer

* **Function:** Facilitates node-to-node data transfer over a shared local medium (network segment). It handles physical addressing, framing, and media access control.
* **Addressing:** Uses a 6-byte (48-bit) **MAC (Media Access Control) Address** written in hexadecimal (e.g., `cc:5e:f8:02:21:a7`). The first 3 bytes designate the Organizationally Unique Identifier (OUI/Vendor ID).
* **Protocols & Hardware:** Ethernet (`802.3`), Wi-Fi (`802.11`). Network **Switches** operate at this layer.

#### Layer 1: Physical Layer

* **Function:** Defines the physical, mechanical, and electrical specifications for transmitting raw binary streams (1s and 0s) over a physical medium.
* **Mediums:** Copper cables (Cat5e/Cat6 Ethernet), Fiber-optic cables, and Radio Frequency bands (2.4 GHz, 5 GHz, 6 GHz Wi-Fi).

---

## Task 3: The TCP/IP Implementation Model

Developed by the US Department of Defense (DoD) in the 1970s, the **TCP/IP model** is the functional architectural model implemented across the global internet. It prioritizes resilience, allowing routing paths to dynamically shift if portions of the network infrastructure fail.

### Architectural Mapping (RFC 1122 vs. 5-Layer Modern Framework)

The classic TCP/IP model condenses the 7 OSI layers into 4 distinct operational zones:

1. **Application Layer:** Absorbs OSI Layers 5, 6, and 7. Application developers handle session state and data formatting within the application itself.
2. **Transport Layer:** Directly maps to OSI Layer 4 (`TCP`/`UDP`).
3. **Internet Layer:** Maps to OSI Layer 3. This layer focuses entirely on global routing and logical tracking via IP.
4. **Link Layer (Network Interface):** Absorbs OSI Layers 1 and 2, dealing with local physical media and MAC addressing configurations.

> **Note on Textbooks (e.g., Kurose & Ross):** Modern curricula often expand the TCP/IP model into **5 layers** by splitting the Link Layer back out into distinct **Link** (Layer 2) and **Physical** (Layer 1) categories to simplify hardware vs. framing concepts.

---

## Task 4: IP Addresses, Subnetting, and Routing

IPv4 addresses provide unique network tracking, enabling predictable global packet delivery.

### Technical Breakdown of IPv4

* **Bit Depth:** 32 bits total, split into **4 octets** (8 bits per octet).
* **Value Range:** $0$ to $255$ per octet ($2^8 = 256$ possibilities).
* **Total Address Space:** $2^{32} \approx 4.29\text{ billion}$ theoretical configurations.

### Subnet Masks and Classless Inter-Domain Routing (CIDR)

A subnet mask distinguishes between the **Network portion** and the **Host portion** of an IP address.

* **Example:** `/24` notation equals a subnet mask of `255.255.255.0`.
* The first 24 bits are masked out for the Network identity, leaving 8 bits ($2^8 = 256$ slots) for local host assignments.

#### Structural Address Restrictions in a `/24` Subnet:

* **Network Address (`.0`):** Reserved to uniquely identify the entire subnet (e.g., `192.168.66.0`). Non-assignable to hosts.
* **Broadcast Address (`.255`):** Reserved to transmit data packets to all devices residing on the subnet concurrently (e.g., `192.168.66.255`).
* **Usable Host Space:** $256 - 2 = 254$ usable IP addresses (`.1` through `.245`).

### RFC 1918 Private IP Address Spaces

Private IP ranges are explicitly designated for local area network (LAN) internal environments and are dropped by default by public internet routers. They require **Network Address Translation (NAT)** to cross into the public internet.

| Class Space | Private Network Range | CIDR Block |
| --- | --- | --- |
| **Class A** | `10.0.0.0` to `10.255.255.255` | `10.0.0.0/8` |
| **Class B** | `172.16.0.0` to `172.31.255.255` | `172.16.0.0/12` |
| **Class C** | `192.168.0.0` to `192.168.255.255` | `192.168.0.0/16` |

### System Network Configuration Commands

* **Windows Command Line:** `ipconfig`
* **Linux/UNIX Terminal:** `ifconfig` or `ip address show` (shorthand: `ip a s`)

---

## Task 5: Transport Layer Protocols (TCP vs. UDP)

Layer 4 protocols leverage a 16-bit address block yielding **65,535 usable logical ports** (Port 0 is reserved) to separate concurrent application data streams entering a host.

### User Datagram Protocol (UDP)

* **Operational Mode:** Connectionless, low-overhead transport.
* **Reliability:** Unreliable. It features no delivery verifications, flow controls, or retransmission handshakes ("fire-and-forget").
* **Usecases:** Real-time video streaming, online gaming, VoIP, and DNS queries where speed takes precedence over minor packet drops.

### Transmission Control Protocol (TCP)

* **Operational Mode:** Connection-oriented, stateful transport.
* **Reliability:** Complete reliability. Employs byte-level sequence numbers to track packet ordering, handles error correction, and requires acknowledgement receipts (`ACK`) from the destination.

#### The TCP Three-Way Handshake Process

Before any Layer 7 application data can cross a TCP socket, a formal connection session must occur:

1. **`SYN` (Synchronize):** The client transmits a packet containing a randomly generated initial sequence number ($ISN_C$) to the server port.
2. **`SYN-ACK` (Synchronize-Acknowledge):** The server returns a packet verifying receipt of the client's sequence number while sending its own random initial sequence number ($ISN_S$).
3. **`ACK` (Acknowledge):** The client sends a final confirmation packet back to the server. The connection state shifts to **ESTABLISHED**.

---

## Task 6: Encapsulation & The Lifecycle of a Packet

Data crossing a network relies on a strict architectural wrapping and unwrapping routine.

### The Encapsulation Protocol Pipeline

1. **Application Data Generation:** The user executes an action (e.g., submitting an HTTPS query).
2. **Layer 4 Segment/Datagram Construction:** The Transport layer splits the application data payload and appends a TCP or UDP header containing the source/destination target port parameters.
3. **Layer 3 Packet Injection:** The Internet layer wraps the segment inside an IP header containing the absolute Source IP and Destination IP.
4. **Layer 2 Frame Wrapping:** The local Link protocol wraps the IP packet inside a header and trailer containing localized source/destination MAC addresses.
5. **Layer 1 Serialization:** The complete frame is serialized into physical electrical voltages, light pulses, or radio waves.

### Decapsulation and Routing Traversal

* As the payload encounters a router along its path, the router peels away the Layer 2 frame information to inspect the Layer 3 IP header.
* The router references its internal routing table, matches the target IP destination network, applies a *brand new Layer 2 frame header/trailer* matching the physical topology of the next hop, and queues transmission.
* This process repeats across multiple hops until the destination host accepts the transmission and systematically strips away all headers layer by layer to extract the underlying raw application data.

---

## Task 7: Telnet and Practical Layer 7 Diagnostics

The **TELNET (Teletype Network)** protocol is a legacy text-based networking framework historically utilized for remote command-line system access.

> **Security Warning:** Telnet does not incorporate encryption mechanisms. Credentials and communications pass over the wire in cleartext, making them highly susceptible to network sniffing. Modern operations utilize **SSH (Secure Shell, Port 22)**.

### Telnet as a Connectivity Diagnostic Tool

Because Telnet opens a raw, interactive TCP connection to any specified port, it is an effective tool for manual validation of Layer 7 protocol states.

#### Diagnostic Lab Scenarios:

1. **Interacting with an Echo Server (Port 7):**
* Command: `telnet MACHINE_IP 7`
* Outcome: The server accepts strings and echoes them back. The connection remains open until manually escaped via `CTRL + ]` then typing `quit`.


2. **Interacting with a Daytime Server (Port 13):**
* Command: `telnet MACHINE_IP 13`
* Outcome: The handshake completes, the server reads its local clock string, transmits it to the console, and automatically initiates a connection termination.


3. **Manually Constructing a Web Request (Port 80):**
* Command: `telnet MACHINE_IP 80`
* Execution Inputs:
```http
GET / HTTP/1.1
Host: telnet.thm
[Press Enter Key Twice]

```


---

https://share.gemini.google/UwxoYpiNGtWw
