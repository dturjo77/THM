Networking
Networking Secure Protocols

---

## Task 1: Environment Initialization & Opening PCAPNG Files

The analysis began within an enterprise Linux environment running a packet analysis engine to inspect historical network traffic captured during a user session.

### 1. Packet Capture (PCAP/PCAPNG) Structure

* **Concept:** PCAP (Packet Capture) and PCAPNG (Packet Capture Next Generation) are standard file formats used to record network traffic passing through a network interface card (NIC).
* **Mechanism:** As network frames move across the wire or through the air, sniffing tools copy the raw binary data, attach a highly accurate timestamp, and write them sequentially to a file. PCAPNG improves on the older PCAP format by supporting multi-interface captures, explicit OS/hardware metadata block fields, and custom packet annotations.
* **Lab Context:** The target network capture file for this session was located natively as `randy-chromium.pcapng`. Opening this file into the analyzer initialized the dissection engine, loading the raw hexadecimal sequences into a readable, tabular frame format.

---

## Task 2: Addressing Cleartext Protocols and Port Mapping

Before analyzing the encrypted traffic, the foundational layer of transport and application security mapping was analyzed to differentiate vulnerable protocols from secure alternatives.

### 1. Transport Layer Security (TLS) vs. Cleartext Deficiencies

* **Concept:** Legacy application layer protocols transmit data natively in plain text, making them completely vulnerable to passive eavesdropping, sniffing, and man-in-the-middle (MitM) credential harvesting. Secure variations wrap these protocols within an encrypted TLS session.
* **Mechanism & Port Standard Mapping:**
* **File Transfer Protocol (FTP - Port 21) vs. FTPS (Port 990):** FTP sends authentication commands across an unencrypted control channel. FTPS uses TLS negotiation immediately upon connection or via explicit command commands to obscure commands and data blocks.
* **Telnet (Port 23) vs. SSH (Secure Shell - Port 22):** Telnet sends terminal interactions, commands, and passwords in clear text. SSH establishes a cryptographically secure tunnel via asymmetric handshake mechanics, ensuring all remote terminal payloads are completely unreadable to an interstitial observer.
* **Post Office Protocol 3 (POP3 - Port 110) vs. IMAPS (Internet Message Access Protocol Secure - Port 993):** POP3 retrieves email natively over an open stream. Modern implementations protect these email architectures by enforcing IMAP over an explicit TLS connection.



---

## Task 3: Identifying Non-TLS Network Traffic (DNS Analysis)

Initial packet navigation encountered control-plane protocols that run unencrypted outside of typical TLS web configurations.

### 1. Domain Name System (DNS) Mechanics

* **Concept:** DNS maps human-readable domain strings to machine-routable IP addresses. By default, standard DNS queries and responses run over UDP port 53 without native encryption layer controls.
* **Mechanism:** Before a browser can establish a secure web socket with a host, it sends a `Standard query` requesting explicit record maps (such as an `A` record for IPv4 or an `AAAA` record for IPv6).
* **Lab Observations:** In `image_ebac86.jpg`, initial packet lines explicitly captured standard UDP queries tracking names like `accounts.google.com` alongside response streams routing to their corresponding server infrastructure. Because DNS packets do not negotiate transport layer encryption, right-clicking them lacks any protocol preferences mapping to cryptographic handshakes or master keys.

---

## Task 4: Transport Layer Security (TLS) Handshake Dissection

To extract information from protected application channels, the architecture of the secure tunnel generation process was analyzed.

### 1. TLS v1.3 Handshake Architecture

* **Concept:** TLS 1.3 is the latest standard for cryptographic transport security, heavily optimizing the multi-step handshakes found in older TLS 1.2 implementations down to a single round-trip time (1-RTT).
* **Mechanism & Frame Progression:**
* **Client Hello:** The browser initiates the request by broadcasting its supported cipher suites, key exchange parameters, and the Server Name Indication (SNI) string (e.g., `Client Hello (SNI=accounts.google.com)`).
* **Server Hello:** The web server responds by selecting the strongest mutually supported cipher suite and sharing its cryptographic key share parameters (e.g., `Server Hello, Change Cipher Spec`).
* **Change Cipher Spec / Encrypted Extensions:** The connection transitions from cleartext negotiation directly into an encrypted operational state (e.g., `Encrypted Extensions, Certificate, Certificate Verify, Finished`). Subsequent application traffic (such as HTTP data payloads) is packaged completely inside protected `Application Data` wrappers.



---

## Task 5: Injecting TLS Pre-Master Secrets for Decryption

Because the application traffic inside the PCAPNG file was fully protected by TLS 1.3, an external cryptographic key log was injected to decrypt and read the hidden payload data.

### 1. `(Pre-)Master-Secret` Key Logging Mechanism

* **Concept:** Perfect Forward Secrecy (PFS) ensures that leaking a server's long-term private key does not compromise past sessions. To decrypt captured traffic, an analyst must supply the ephemeral keys generated specifically for that unique session.
* **Mechanism:** Modern web browsers (like Chromium or Firefox) can be configured to dump these ephemeral key structures into an external debugging log file (typically named `ssl-key.log` or similar) utilizing the system environment variable `SSLKEYLOGFILE`.
* **Application in Wireshark:** By navigating through global protocol preferences (`Edit -> Preferences -> Protocols -> TLS`) or modifying right-click preferences on a valid `TLSv1.3` packet, the key file can be manually fed into the parsing engine. Once loaded, Wireshark maps the random session identifiers against the log entries to decrypt the protected payload layers on-the-fly, revealing underlying application layers like HTTP/2.

---

## Task 6: HyperText Transfer Protocol 2 (HTTP/2) Dissection

Once decrypted, the traffic transformed from generic TLS Application Data streams directly into highly structured HTTP/2 binary components.

### 1. HTTP/2 Multiplexing and Binary Framing Architecture

* **Concept:** HTTP/1.1 relies on cleartext strings sent sequentially over individual TCP sockets. HTTP/2 introduces a binary framing layer that breaks down communications into separate, concurrent, bidirectional logical streams managed over a single persistent TCP connection.
* **Mechanism & Frame Types:**
* **`HEADERS` Frame:** Carries clean HTTP header components such as request methods (`POST`, `GET`), targeting paths, cookie strings, and authorization settings (e.g., `HEADERS[1]: POST /ListAccounts`).
* **`DATA` Frame:** Carries the actual underlying core payload code, binary components, web assets, or form submittals tied to the request (e.g., `DATA[1] (application/x-www-form-urlencoded)`).
* **`SETTINGS` & `WINDOW_UPDATE` Frames:** Manage transport layer configurations, flow control boundaries, and maximum concurrent stream capabilities.



---

## Task 7: Advanced Object Reassembly and Data Extraction

With HTTP/2 streams fully visible, advanced data extraction techniques were used to locate and rebuild file objects directly from the capture file.

### 1. HTTP/HTTP2 Object Exporting

* **Concept:** When web browsers request resources across network packets, files are split and transmitted across many fragments. Wireshark provides a built-in feature called **Export Objects** that acts as an automated carver, reconstructing files back into their original format.
* **Mechanism:** By navigating to `File -> Export Objects -> HTTP / HTTP2`, Wireshark aggregates all reassembled objects transferred over decrypted web protocols. This summary details explicit network locations:
* **Packet:** The baseline index identifying exactly where the resource interaction initialized (e.g., Packets 24, 34, 366).
* **Hostname:** The destination server targets (e.g., separating `accounts.google.com` tracks from `[www.facebook.com](https://www.facebook.com)` domains).
* **Content-Type & Size:** Flags the specific data blueprint (`application/json`, `text/html`, or `application/x-www-form-urlencoded`) and file footprint size (e.g., 132 bytes for packet 366) to help analysts identify high-value packets.



---

## Task 8: Stream Targeting & Form Payload Extraction

The final objective concentrated on parsing the exact application parameters inside the isolated frame to retrieve the target authentication token.

### 1. `application/x-www-form-urlencoded` Deep Inspection

* **Concept:** When web authentication forms are submitted, inputs are encoded into standardized key-value parameter pairs.
* **Mechanism:** Isolating the targeting vector via explicit display syntax (`frame.number == 366`) allows an analyst to bypass complex object structures and look directly at the dissected field mapping tree.
* **Dissection & Final Extraction (`image_ed025f.jpg`):** Expanding the **HTML Form URL Encoded** tree structures reveals the exact backend variables passed along the HTTP/2 stream payload:
* `jazoest` = `"2877"`
* `lsd` = `"AVPeRE3H6tE"`
* `email` = `"strategos@networking.thm"`
* `pass` = `"THM{B8WM6P}"`



### 2. Analytical Conclusion

* **Extracted Solution:** The authentication field contained the secure laboratory token: **`THM{B8WM6P}`**.
* **Core Cybersecurity Takeaway:** This end-to-end extraction process underscores why modern security standards require total protection of network sessions. If an administrative analyst or a malicious actor captures a packet session and accesses the ephemeral TLS log secrets, even multiplexed HTTP/2 application layers can be completely broken down, exposing backend authentication data, session cookies, and sensitive user payloads.
