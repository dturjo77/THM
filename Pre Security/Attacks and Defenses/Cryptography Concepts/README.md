
# Lab Room: Cryptography Concepts
### Date Completed: 20 June 2026
### Tools: tryhackme attack box

---

## ─── TASK 1 & 2: FOUNDATIONS & SYMMETRIC ENCRYPTION ───

### 1. The Core Objective of Cryptography

Cryptography is the practice and study of techniques for secure communication in the presence of adversarial third parties. Within the framework of the **CIA Triad** (Confidentiality, Integrity, and Availability), cryptography provides the mathematical enforcement mechanisms to guarantee two specific pillars:

* **Confidentiality:** Preventing unauthorized disclosure of information by scrambling readable data into an unreadable state.
* **Integrity:** Preventing unauthorized alteration of data by providing mechanisms to detect whether data was modified during transit.

### 2. Fundamental Data States

Data handled by cryptographic systems exists in one of two distinct structural forms:

* **Plaintext:** The raw, unencrypted, baseline data that is human-readable and instantly actionable. Example: `"Patient John Doe has a penicillin allergy."`
* **Ciphertext:** The unreadable, randomized string of characters produced after plaintext passes through an encryption mechanism. Example: `“x7!R9#mQ*pL2”`. It must look like absolute noise to anyone lacking authorization.

### 3. Cryptographic Components

Every functional encryption scheme relies on the interaction between two elements:

* **The Algorithm (The Cipher):** The explicit mathematical formulas, logic steps, or structural recipes used to transform plaintext into ciphertext and vice versa. Modern security architecture dictates that algorithms are publicly disclosed and rigorously vetted by global analysts.
* **The Key:** A secret string of bits or an integer parameter passed into the algorithm to customize the mathematical operation. The security of the data relies entirely on the uniqueness and secrecy of this key.

> **Kerckhoffs's Principle:** A cryptographic system must remain secure even if everything about the design, rules, and algorithm is completely known to the public, provided that the specific key is kept secret.

### 4. Symmetric Cryptography Mechanics

In a symmetric encryption architecture, a single shared secret key is utilized for both the encryption of plaintext and the decryption of ciphertext.

#### Mathematical Blueprint:

$$\text{Encryption Process: } \text{Plaintext} + \text{Algorithm} + \text{Key} \longrightarrow \text{Ciphertext}$$

$$\text{Decryption Process: } \text{Ciphertext} + \text{Algorithm} + \text{Key} \longrightarrow \text{Plaintext}$$

#### The Physical Lockbox Analogy:

* **The Algorithm:** The inner mechanical design of a padlock. Anyone can examine how the pins align or watch the shackle close.
* **The Key:** The specific physical metal key cut to open that single lock.
* **The Plaintext:** The private physical document placed inside the box.
* **The Ciphertext:** The locked iron box traveling through a public delivery route.

### 5. Technical Focus: The Caesar Cipher

The Caesar Cipher is a historic **substitution cipher** where each alphabet character in the plaintext is shifted forward down the alphabet structure by a fixed number of spaces determined by the key value.

#### Mathematical Wrap-around Rules:

If a shift exceeds the final boundary character 'Z', the algorithm performs a modulo operation ($\pmod{26}$) to loop back to the beginning of the alphabet loop:


$$\text{Character Index} = (\text{Current Index} + \text{Key}) \pmod{26}$$

#### Operational Demonstration (Key = 3):

* `H` $\rightarrow$ (+3) $\rightarrow$ `K`
* `E` $\rightarrow$ (+3) $\rightarrow$ `H`
* `L` $\rightarrow$ (+3) $\rightarrow$ `O`
* `L` $\rightarrow$ (+3) $\rightarrow$ `O`
* `O` $\rightarrow$ (+3) $\rightarrow$ `R`
* **Result:** `HELLO` becomes `KHOOR`

#### Shift Key Examples Completed in Lab:

* **Level 3 Interception:** `ESP DJDEPX TD LE CTDV` decrypted with a backward shift of **11** yielded `THE SYSTEM IS AT RISK`.
* **Level 4 Interception:** `XLMW MW XLI JMREP GSHI` decrypted with a backward shift of **4** yielded `THIS IS THE FINAL CODE`.
* **Encryption Test:** `CYBER` encrypted with a forward shift of **5** yielded `HDGJW`.
* **ROT13 Variant:** `FVZCYR PNRFNE PVCURE` decrypted with a shift of **13** yielded `SIMPLE CAESAR CIPHER`.

### 6. Vulnerabilities & Constraints of Symmetric Architecture

* **The Brute-Force Vulnerability:** Because ciphers like Caesar only possess 25 valid shifting keys, a threat actor can test every permutation in milliseconds. While modern symmetric ciphers like **AES (Advanced Encryption Standard)** use massively complex mathematical operations to eliminate this threat, they still suffer from a critical logical flaw.
* **The Key Distribution Problem:** Symmetric encryption requires both communicating parties to possess the identical key. If they have never met, they must transmit this key across an untrusted network (the Internet). If an eavesdropper captures the key during transit, all future confidentiality is instantly broken.

---

## ─── TASK 3 & 4: ASYMMETRIC ENCRYPTION & SYSTEMS INTEGRATION ───

### 1. Asymmetric Cryptography Mechanics (Public-Key Crypto)

To break the key distribution deadlock, asymmetric cryptography introduces an architecture utilizing two distinct, mathematically paired keys:

* **The Public Key:** Made entirely public. It can be openly broadcasted, embedded in web files, or shared on directory servers. Anyone can use this key to **encrypt** data intended for the owner.
* **The Private Key:** Kept strictly secret by the owner. It never traverses the network. Only this key can **decrypt** data that was locked by its matching public key.

#### The Street Mailbox Analogy:

* **The Public Key:** The drop-slot on the exterior of a street mailbox. Anyone walking past can drop a confidential letter inside.
* **The Private Key:** The master physical key held exclusively by the postal worker to open the locked storage door at the back to collect the mail.

### 2. The Identity Crisis: Digital Certificates & Trust Infrastructure

While asymmetric encryption ensures that only the holder of the private key can read a message, it introduces a secondary threat: **Impersonation**. An attacker could broadcast their own public key while claiming to be a trusted entity like `google.com`.

To fix this, the internet relies on a **Public Key Infrastructure (PKI)**:

* **Digital Certificate:** A electronic passport that binds a validated entity name (e.g., `tryhackme.com`) directly to their specific Public Key.
* **Certificate Authority (CA):** Globally trusted third-party corporations (like Let's Encrypt or DigiCert) that verify the identity of websites and digitally sign their certificates to guarantee validity.
* **Root Store:** A pre-compiled list of trusted CAs embedded directly inside web browsers and operating systems to instantly verify certificates during web navigation.

### 3. Structural Comparison Matrix

| Technical Feature | Symmetric Encryption | Asymmetric Encryption |
| --- | --- | --- |
| **Key Count per Session** | One single key shared by both parties. | Two separate, mathematically linked keys. |
| **Operational Speed** | Highly optimized, ultra-fast data throughput. | Computationally heavy, significantly slower. |
| **Primary Deployment** | Encrypting large files, hard drives, active database tables. | Secure key exchanges, digital identities, signatures. |
| **Primary Weakness** | Difficult key distribution over public lines. | High processing overhead for large data streams. |

### 4. Real-World Integration: The HTTPS Hybrid Cryptosystem

Modern secure web browsing (**HTTPS/TLS**) does not pick one format over the other. Instead, it deploys a **Hybrid Architecture** that exploits the unique structural advantage of each system:

```
[Web Browser]                                                    [Web Server]
      │                                                               │
      │ Step 1: Request Secure Connection                             │
      ├──────────────────────────────────────────────────────────────>│
      │                                                               │
      │ Step 2: Present Signed Certificate (Contains Public Key)       │
      │<──────────────────────────────────────────────────────────────┤
      │                                                               │
      │ Step 3: Browser verifies Certificate via Trusted Root CA.      │
      │ Step 4: Browser generates a random Symmetric Session Key.     │
      │ Step 5: Browser encrypts Session Key with Server's Public Key. │
      │                                                               │
      │ Step 6: Send Encrypted Session Key                            │
      ├──────────────────────────────────────────────────────────────>│
      │                                                               │
      │                               Step 7: Server uses its Private │
      │                                       Key to decrypt the data │
      │                                       and extract the Session │
      │                                       Key.                    │
      │                                                               │
      │═══════════════════════════════════════════════════════════════│
      │ Step 8: Both parties switch to fast Symmetric Encryption      │
      │         using the established Session Key for all bulk data.  │
      │═══════════════════════════════════════════════════════════════│

```

---

## ─── SUMMARY ARCHITECTURE OF CYBER DEFENSE ───

While cryptography effectively isolates and protects data data-in-transit or data-at-rest, it must be supported by foundational administrative and environmental defensive controls to keep a company secure:

* **Secure Key Storage:** Ensuring private keys are kept away from exposed configuration files or public code repos.
* **Software Lifecycle Updates:** Ensuring patches are regularly applied to prevent attackers from bypassing encryption layers entirely via software flaws.
* **User Training:** Teaching personnel to recognize social engineering tricks, as an attacker will simply trick a user into handing over keys if the math cannot be bypassed.

---

<img width="1281" height="532" alt="image" src="https://github.com/user-attachments/assets/6c9cf0cb-5a4b-4442-834f-92cc4a722916" />

<img width="1257" height="497" alt="image" src="https://github.com/user-attachments/assets/bc7dd6bd-1d92-495b-af61-7fc5ba25e6d5" />

