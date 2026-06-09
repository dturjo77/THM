# Lab Room: Client-Server Basics
### Date Completed: 7 June 2026
### Tools: tryhackme attack box

---

# Comprehensive Technical Notes: Computer Networking & Web Communications

## 📄 Task 1: Introduction to Computer Networking

### 1. Evolution of Networking (Standalone to Interconnected Systems)

* **Historical Context:** In the early days of computing, machines operated as isolated, standalone units. They stored data locally, executed programs locally, and had no native mechanism to communicate with external machines. Data sharing required physical media (like tapes or disks).
* **The Paradigm Shift:** As organizations grew globally, the necessity for rapid resource sharing and information exchange led to the concept of interconnection.
* **Precursors to the Modern Internet:**
* **ARPANET (Advanced Research Projects Agency Network):** The first network to implement the TCP/IP protocol suite. It introduced the concept of packet switching.
* **CYCLADES:** A French research network that pioneered the concept of host-to-host data transmission, shifting the responsibility of data reliability from the network itself to the connected computers.
* **NSFNET (National Science Foundation Network):** Acted as a major backbone for high-speed networking in the United States, transitioning academic research networks into the commercial internet infrastructure we use today.



### 2. Specialized System Roles

* Much like human societies where individuals specialize in specific trades (medicine, retail, mechanics), interconnected computers evolved to specialize in specific services. Rather than every computer trying to do everything, systems are categorized based on their roles: some are built to store vast amounts of data, while others are built to request and display that data.

---

## 📄 Task 2: The Client-Server Architecture (Analogy & Technical Breakdown)

The **Client-Server Model** is a distributed application structure that partitions tasks or workloads between the providers of a resource or service (servers) and service requesters (clients).

```
[ Client ]  --- (Request) --->  [ Server ]
[ Client ]  <-- (Response) ---  [ Server ]

```

### 1. The Client

* **Definition:** The device or software application that initiates a communication session by sending a request for data or a service.
* **Key Characteristic:** The client **always** initiates the connection. It does not sit and wait for incoming requests; it acts as the consumer.
* **Analogy Alignment:** Alice/Bob deciding they want pizza and actively placing the order.
* **Technical Real-World Example:** Web browsers (Chrome, Firefox), mobile apps (Facebook, Spotify), or command-line utilities (cURL).

### 2. The Server

* **Definition:** A high-performance computer or software process that constantly listens for incoming requests from clients and provides a corresponding service or data resource.
* **Key Characteristic:** It is passive until a request is received, at which point it processes the request and serves the result.
* **Analogy Alignment:** Luigi’s Pizza shop, which stays open, waiting for customers to order.
* **Technical Real-World Example:** Apache, Nginx (Web Servers), MySQL (Database Servers), or Microsoft Exchange (Mail Servers).

### 3. Request and Response Cycle

* **The Request:** The formal message sent by the client to the server asking for a specific action (e.g., "Give me the homepage").
* **The Response:** The message sent back by the server containing either the requested resource or an error message if the resource is unavailable or the request was malformed.
* **Analogy Alignment:** Bob handing the order to the cook (Request) and receiving the physical pizza box (Response).

### 4. Network Protocols

* **Definition:** A standardized set of rules, formats, and syntax that determines how data is transmitted and interpreted between distinct devices across a network. Without a protocol, two computers could send electrical signals to each other but would fail to comprehend the meaning of the data bits.
* **What a Protocol Governs:**
* **Commands:** Which specific words/methods are accepted (e.g., `GET`, `POST`).
* **Structure:** The exact arrangement of data headers and data bodies.
* **Syntax & Language:** The digital encoding standard used.
* **Error Handling:** What to do when a requested resource does not exist.


* **Analogy Alignment:** The printed menu that dictates what items can be ordered, and the English language used by Bob and the cook to communicate.

### 5. Network Ports

* **Definition:** A logical, virtual construct identifier used by operating systems to map incoming network traffic to specific software applications or services running on a device.
* **Why Ports Matter:** A single physical server can run multiple services simultaneously (e.g., hosting a website and running an email server). Ports ensure traffic doesn't get mixed up. Ports are numbered from `0` to `65535`.
* **Analogy Alignment:** Different dedicated entrance doors at Luigi's: Door A for Takeaway, Door B for Dining In, and Door C for Delivery drivers.
* **Common Technical Examples:** Port `80` (HTTP web traffic), Port `443` (HTTPS secure web traffic), Port `22` (SSH secure remote access).

### 6. DNS (Domain Name System)

* **Definition:** A hierarchical and decentralized naming system for computers, services, or other resources connected to the Internet. It translates human-readable domain names into machine-readable numerical identifiers.
* **IP Address (Internet Protocol Address):** The actual numerical label assigned to each device connected to a computer network (e.g., `142.250.190.46`). It functions exactly like a real-world physical mailing address.
* **The Function of DNS:** Humans excel at remembering names (`google.com`), whereas routers and computers require IP addresses to route data packets. DNS acts as the "Phonebook/GPS of the Internet" by looking up the name and resolving it to the IP address.
* **Analogy Alignment:** Bob typing the name "Luigi’s Pizza" into his GPS device, which translated the name into exact geographical coordinates.

---

## 📄 Task 3: Web Communication in Practice (HTTP Deep-Dive)

### 1. Stateless vs. Stateful Environments

* **Stateless Protocol (HTTP):** By default, HTTP is fundamentally stateless. This means the server treats every single incoming request as an isolated transaction completely unrelated to any previous request. The server retains zero memory of who you are or what you did a second ago.
* **Application-Level Statefulness:** Because modern web usage requires memory (e.g., staying logged in, keeping items in a shopping cart), developers implement mechanisms to mimic memory over a stateless protocol:
* **Session Identifiers:** When you log in with credentials, the server authenticates you once and generates a unique, temporary string called a Session ID.
* **Cookies:** The server sends this Session ID back to your browser, which stores it as a cookie. On every single subsequent click or request, the browser automatically attaches this cookie. The server reads the cookie, matches it in its database, and remembers, *"Ah, this is still Alice!"* Without this, you would have to re-enter your username and password for every single link you clicked on a website.



### 2. HTTP Methods (Commands)

The HTTP specification defines structural methods to indicate the desired action to be performed on a identified resource. There are 9 core methods, including:

* `GET`: Used exclusively to **retrieve** or read data from a server without modifying anything.
* `POST`: Used to **send/submit** data to the server to create a new resource (e.g., submitting a signup form).
* `PUT`: Used to update/replace an existing resource entirely.
* `DELETE`: Used to remove a resource from the server.
* `PATCH`: Used to make partial updates to a resource.

### 3. Deep Analysis of an HTTP GET Request Lifecycle

When a user types a URL like `[https://www.iamlearning.thm/contact](https://www.iamlearning.thm/contact)` into a browser, the following technical components are constructed under the hood:

```
[ Request Metadata ] -> Scheme: HTTPS | Host: www.iamlearning.thm | Path: /contact

```

* **Scheme:** Identifies the protocol layer used for transport. `HTTP` is unencrypted, whereas `HTTPS` adds an SSL/TLS encryption layer to secure data in transit.
* **Host:** Identifies the target domain name (`www.iamlearning.thm`) hosting the resource.
* **Filename/Path:** The specific file directory path targeted on the host server. A path of `/` translates to the default home page configuration (usually `index.html` or `index.php`).
* **Address:** The IP address resolved by DNS. The special IP address `127.0.0.1` is known as **Loopback** or **Localhost**, which points back to the internal network card of your own machine (used heavily for local testing).

### 4. HTTP Responses: Headers and Bodies

Once the server processes a `GET` request, it replies with a structured packet broken down into two distinct parts:

```
+--------------------------------------------+
| RESPONSE HEADER                            |
| (Status Code: 200 OK, Content-Type, etc.)  |
+--------------------------------------------+
| RESPONSE BODY                              |
| (Raw HTML / Website Content)               |
+--------------------------------------------+

```

* **HTTP Status Codes:** A 3-digit numerical code returned by the server indicating the status of the request:
* **`200 OK`**: The request was successful, and the resource is attached.
* **`404 Not Found`**: The server could not find the requested URL path.


* **Response Header:** Contains administrative metadata regarding the transmission. It details things like the server type, date, data encryption parameters, cookie configurations, and the `Content-Type` (telling the browser whether it is receiving an image, a video, or HTML code).
* **Response Body:** Contains the actual payloads requested by the client. In the context of web browsing, this is typically raw **HTML (Hypertext Markup Language)** code. The browser parses this raw layout text, downloads attached assets, and visually renders it into a fully interactive web page for the user.
