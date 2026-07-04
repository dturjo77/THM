# Lab Room: Networking Core Protocols
### Date Completed: 4 July 2026
### Tools: tryhackme attack box

---

## 📌 Task 1: Introduction & Lab Environment Setup

### 1. Architectural Setup

The practical foundation of this module relies on a dual-machine virtual architecture hosted within a browser-based sandboxed environment (commonly found on platforms like TryHackMe).

* **Attacker Machine (AttackBox):** A pre-configured Linux instance acting as the network auditor or client. It comes loaded with analytical CLI utilities (`telnet`, `ftp`, `tshark`, `nslookup`).
* **Lab Machine (Target VM):** A remote server instance running daemon services corresponding to core protocols (such as an Nginx/Apache web server, vsFTPd file server, and Exim/Dovecot mail servers).

### 2. Conceptual Progression Map

This lab represents the third tier in a four-part systematic networking curriculum. The learning path is structured to move from baseline abstractions to programmatic text-based execution:

1. **Networking Concepts:** Fundamental comprehension of OSI and TCP/IP structural layers.
2. **Networking Essentials:** Deep dive into foundational Layer 2 (Ethernet) and Layer 3/4 (IP, TCP/UDP) handshakes and framing.
3. **Networking Core Protocols (Current Module):** Deep dive into Layer 7 text-based application protocols (`DNS`, `HTTP`, `FTP`, `SMTP`, `POP3`, `IMAP`).
4. **Networking Secure Protocols:** Implementing cryptography, transport layer security (TLS/SSL), and cryptographic key exchanges.

---

## 📌 Task 2: DNS (Domain Name System) — Remembering Addresses

### 1. In-Depth Operational Mechanics

The Domain Name System (DNS) is a hierarchical, decentralized naming system for computers, services, or any resource connected to the Internet. It operates at **Layer 7 (Application Layer)** of the OSI model.

```
[Client Web Browser] 
       │
       │ (1) DNS Query: www.example.com (UDP Port 53)
       ▼
[DNS Resolver / Server] ───(2) Checks Resource Records (e.g., A/AAAA)
       │
       ▼ (3) DNS Response: 93.184.215.14
[Client establishes TCP Handshake with Target Web Server on Port 80/443]

```

* **Transport Layer Port Dynamics:** By default, DNS uses **UDP port 53** because it is connectionless and fast, minimizing latency during the query-response phase. However, DNS falls back to **TCP port 53** under two primary conditions:
* When the response payload exceeds the standard UDP packet constraint (**512 bytes**), triggering a truncation flag (`TC`).
* During zone transfers between primary and secondary authoritative DNS servers to guarantee data integrity.



### 2. Granular Breakdown of Resource Records (RR)

Every domain maintains a zone file containing specific records that tell the DNS resolver how to handle incoming queries:

* **A Record (Address Record):** Maps a human-readable hostname (FQDN) directly to a **32-bit IPv4 address** (e.g., `example.com` $\rightarrow$ `172.17.2.172`).
* **AAAA Record (Quad-A Record):** Performs the exact same architectural function as the A record but maps hostnames to a **128-bit IPv6 address** (e.g., `2606:2800:21f:cb07:6820:80da:af6b:8b2c`).
* **CNAME Record (Canonical Name Record):** Maps an alias name to the true canonical domain name. It routes traffic internally without needing a dedicated IP binding for every subdomain variation (e.g., mapping `www.example.com` $\rightarrow$ `example.com`).
* **MX Record (Mail Exchange Record):** Specifies the authoritative mail server responsible for receiving email infrastructure on behalf of the domain. It includes a priority preference value if multiple mail servers are configured.

### 3. Packet Analysis via Command Line (`nslookup` & `tshark`)

When executing `nslookup www.example.com`, a specific 4-packet transaction happens under the hood, captureable by packet analyzers like `tshark` or `Wireshark`:

* **Packet 1 (Standard Query - A):** Client sends an outbound UDP request to the resolver asking for the IPv4 representation (`A`) of the target domain.
* **Packet 2 (Standard Query Response - A):** Server returns the matching IPv4 string (`93.184.215.14`).
* **Packet 3 (Standard Query - AAAA):** Client checks for modern routing capacities by requesting the IPv6 address.
* **Packet 4 (Standard Query Response - AAAA):** Server returns the matching Hexadecimal IPv6 string.

---

## 📌 Task 3: WHOIS — Domain Ownership Analysis

### 1. Definition and Legal Context

**WHOIS** (pronounced literal text: *"Who Is"*) is a query and response transaction protocol used for querying databases that store the registered users or assignees of an Internet resource, such as a domain name or an IP address block.

It is maintained globally by registrars authorized by **ICANN** (Internet Corporation for Assigned Names and Numbers). It is a public ledger requiring registrants to provide accurate administrative and technical contact data to maintain domain accountability.

### 2. Core Metrics Extracted During Information Gathering

When running a `whois` query from a security perspective, specific fields provide crucial timeline context about a target network:

* **Creation Date / Expiration Date:** Indicates when the infrastructure was originally stood up and when it might expire. Older registration dates build structural reputational trust, whereas brand-new domains are highly correlated with phishing campaigns.
* **Registrar Abuse Contact:** The dedicated email pipeline (`abuse@godaddy.com`) used by security teams to report malicious payloads, command-and-control operations, or network attacks tied to that domain.
* **WHOIS Privacy Services:** To protect sensitive operational details from public scraping, registrars offer data masking mechanisms (e.g., *Domains By Proxy, LLC*). This substitutes real user identities with proxy registrar details to maintain privacy while complying with ICANN rules.

---

## 📌 Task 4: HTTP(S) — Accessing the Web

### 1. Structural Layer and Transmission Characteristics

Hypertext Transfer Protocol (HTTP) is an application layer standard designed to facilitate data exchange between a web client (browser) and a daemon web server. It relies completely on the connection-oriented **TCP protocol** to ensure no loss of packets occurs during asset rendering.

* **Port Allocations:** Cleartext **HTTP** operates natively on **TCP Port 80** (or testing ports like 8080). Securely encrypted **HTTPS** operates on **TCP Port 443** (or testing ports like 8443) using TLS/SSL wrappers to shield data streams.

### 2. Functional HTTP Methods Analysis

The architecture defines specific operational methods to direct server behavior:

* **GET:** Requests a transfer of a target resource's data representation from the server. It is idempotent (safe to run repeatedly without mutating server states).
* **POST:** Submits an arbitrary entity payload to the specified resource, frequently inducing a state change or side effects on the backend server database (e.g., processing forms, authentication attempts).
* **PUT:** Replaces all current representations of the target resource with the uploaded request payload. If the resource does not exist, it creates it.
* **DELETE:** Erases the designated target resource permanently from the active web directory tree.

### 3. Programmatic Raw Interaction via Telnet

Browsers conceal the plain-text nature of HTTP transactions. However, network engineers can use `telnet` to simulate an HTTP client request directly to a server on Port 80:

```bash
telnet MACHINE_IP 80

```

Upon establishing a TCP connection, a raw HTTP Request header must be cleanly entered:

```text
GET /flag.html HTTP/1.1
Host: localhost
[CRLF]
[CRLF]

```

* **Syntax Breakdown:** `GET` defines the action; `/flag.html` dictates the URI path; `HTTP/1.1` declares the protocol standard version. The `Host` header is mandatory in HTTP/1.1 to differentiate distinct web roots residing on a single server IP.
* **The Double-Enter Requirement:** A blank line consisting of a Carriage Return and Line Feed (`[CRLF][CRLF]`) is strictly required by the protocol specification to signal the explicit termination of the HTTP Request header array. Missing this causes the server to hang or reject the command as a `400 Bad Request`.

---

## 📌 Task 5: FTP (File Transfer Protocol) — Transferring Files

### 1. Dual-Channel Connection Architecture

File Transfer Protocol (FTP) operates distinctly from HTTP by segmenting its traffic architecture into two discrete communication pipelines across **Layer 4 (TCP)**:

* **Control Channel (TCP Port 21):** Handles authentication, session initiation, and raw text command traffic (`USER`, `PASS`, `LIST`, `QUIT`).
* **Data Channel (Dynamic Ports):** Handles the actual transport of files (`RETR`, `STOR`). This channel is spun up dynamically on a separate port connection only when a file transfer action is explicitly executed.

```
[Client FTP Engine] ═════════ Control Channel (Port 21) ═════════► [Remote FTP Daemon]
[Client FTP Engine] ◄══════════ Data Channel (Dynamic) ═══════════ [Remote FTP Daemon]

```

### 2. Underlying System FTP Commands

While human operators interact via high-level commands like `ls` or `get`, the client software translates these into lower-level protocol-specific keywords:

* `USER anonymous` $\rightarrow$ Passes the public account string to bypass strict credential validation.
* `ls` $\rightarrow$ Transmitted under the hood as **`LIST`** to pull server file indexing.
* `get flag.txt` $\rightarrow$ Transmitted under the hood as **`RETR flag.txt`** (Retrieve) to fetch the stream.
* `put localfile.txt` $\rightarrow$ Transmitted under the hood as **`STOR localfile.txt`** (Store) to write the file.

### 3. Data Transfer Modes

* **Binary Mode:** Transfers files bit-for-bit without alteration, essential for executables, compressed files, and media.
* **ASCII Mode:** Specifically formats text newline markers (translating between Windows `\r\n` and Linux `\n`) during text transfers. Misconfiguring text transfers can cause line-feed warning alerts.

---

## 📌 Task 6: SMTP (Simple Mail Transfer Protocol) — Sending Email

### 1. Core Mission and Port Designation

SMTP is an application protocol dedicated solely to the unidirectional **push transmission** of email traffic. It handles mail delivery from a local client to an outbound Mail Transfer Agent (MTA) server, and the hop-by-hop relay between distinct MTAs across the web. It uses **TCP Port 25** for server-to-server relaying.

### 2. Anatomy of an SMTP Session Dialogue

Using Telnet, a manual SMTP dialogue reveals the step-by-step transaction model:

1. **`HELO client.thm`** or **`EHLO`** (Extended HELO): Begins the session, identifying the sending domain.
2. **`MAIL FROM: <user@client.thm>`:** Establishes the envelope sender return path address.
3. **`RCPT TO: <linda@server.thm>`:** Establishes the envelope recipient destination address.
4. **`DATA`:** Prepares the server buffer to ingest the actual message text body.
5. **Headers and Body:** Content definitions (`From:`, `To:`, `Subject:`) are written, followed by the actual message content.
6. **`\r\n.\r\n` (A isolated period line):** Instructs the SMTP server parser that the message body compilation is complete and pushes it into the outbound mail queue.

---

## 📌 Task 7: POP3 (Post Office Protocol v3) — Downloading Email

### 1. Functional Mandate

Post Office Protocol Version 3 (POP3) is a unidirectional **pull protocol** operating on **TCP Port 110**. Unlike SMTP, which pushes mail outwards, POP3 contacts the remote server mail drop to retrieve and download stored emails locally to a single client machine.

### 2. State Operations & Critical Commands

During a POP3 connection, the session enters an *Authorization* state, transitions to a *Transaction* state, and concludes in an *Update* state:

* **`USER` / `PASS`:** Handles initial authentication mapping.
* **`STAT`:** Pulls global metadata showing total unread message counts and volume size.
* **`LIST`:** Prints a numeric array mapping individual message IDs to byte sizes.
* **`RETR <ID>`:** Commands the server to stream the full envelope structure, header details, and text content of a targeted message ID down to the local machine terminal.
* **`QUIT`:** Commits structural changes (such as purging items marked by `DELE`) and severs the active TCP port socket.

### 3. The Cleartext Security Flaw

POP3 is inherently insecure because all administrative and operational data packets are sent across the network wire in unencrypted, readable plain text. Anyone executing a packet capture (`Wireshark`/`tshark`) along the transmission path can instantly read user credentials (such as username strings and passwords like `Pa$$123`) and mail body data.

---

## 📌 Task 8: IMAP (Internet Message Access Protocol) — Synchronizing Email

### 1. Synchronization vs. Static Ingestion (IMAP vs. POP3)

Internet Message Access Protocol (IMAP), running natively on **TCP Port 143**, is a more robust alternative to POP3 designed for a multi-device era.

* **POP3 Deficiencies:** POP3 acts on a traditional "download-and-delete" mentality. Once an active email client retrieves mail data, it is pulled off the central server mail spire. This makes cross-device history checking across a phone, laptop, and desktop impossible.
* **IMAP Strengths:** IMAP leaves the true data master record on the remote server indefinitely. Multiple individual clients log into the server concurrently, caching temporary views locally. State modifications—such as marking a message as read (`\Seen`), moving an item to a folder, or deleting an asset—are globally synchronized back to the server. This ensures a consistent mailbox state across all devices.

### 2. Operational Complexity and Syntax Command Matrix

IMAP commands are structurally unique. They require an alphanumeric prefix tag (such as `A`, `B`, `C`) generated by the client to track asynchronous responses sent by the server:

* **`A LOGIN <user> <pass>`:** Authenticates the session.
* **`B SELECT inbox`:** Explicitly mounts an internal folder directory to parse targeting streams.
* **`C FETCH 4 body[]`:** Pulls the explicit body and header array of a targeted index entry. This command is highly customizable; it can request specific parameters, like `FETCH 4 (FLAGS)` or `FETCH 4 (BODY[HEADER])`, to minimize bandwidth.
* **`D LOGOUT`:** Ends the synchronized state machine session.

---

https://share.gemini.google/l3yejBRgH32f
