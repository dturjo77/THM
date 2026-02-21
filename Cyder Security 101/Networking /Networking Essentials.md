## 🤔 প্রথমে প্রশ্নগুলো সহজ করে বুঝি

### 1️⃣ কম্পিউটার অন করলেই Internet কেন কাজ করে?

👉 কারণ **DHCP** তোমার ডিভাইসকে নিজে নিজে IP address দেয়।

### 2️⃣ আমার data কোন কোন দেশ/ডিভাইস ঘুরে যায়?

👉 এটা জানতে সাহায্য করে **Traceroute**।

### 3️⃣ ISP একটাই IP দেয়, কিন্তু বাসার সব ডিভাইস Internet পায় কীভাবে?

👉 এখানে কাজ করে **NAT**।

এই রুমে এসব প্রশ্নের উত্তরই শেখানো হবে 👍

---

## 🎯 এই রুমের Learning Objectives (মূল শেখার বিষয়)

### 1️⃣ **DHCP (Dynamic Host Configuration Protocol)**

🧠 কাজ:
👉 ডিভাইস অন করলেই **নিজে নিজে IP address দেয়**

📌 সহজভাবে:

> তুমি নতুন বাসায় গেলে মালিক তোমাকে রুম নম্বর দেয়
> ঠিক তেমনি DHCP তোমার ডিভাইসকে IP দেয়

---

### 2️⃣ **ARP (Address Resolution Protocol)**

🧠 কাজ:
👉 IP address → MAC address খুঁজে বের করা

📌 সহজভাবে:

> “এই IP কার?”
> ARP বলে দেয় — “এই MAC address-এর”

---

### 3️⃣ **NAT (Network Address Translation)**

🧠 কাজ:
👉 **একটা Public IP ব্যবহার করে অনেক ডিভাইস Internet ব্যবহার করতে দেয়**

📌 সহজভাবে:

> বাসার একটাই main gate
> কিন্তু ভেতরে অনেক রুম
> বাইরে সবাই gate-এর address দেখে, ভেতরে কে কোন রুমে থাকে NAT জানে

---

### 4️⃣ **ICMP (Internet Control Message Protocol)**

🧠 কাজ:
👉 Network error ও status message পাঠানো

📌 সহজভাবে:

> “পথ বন্ধ”, “গন্তব্য পাওয়া যায়নি”
> এসব message ICMP পাঠায়

---

### 5️⃣ **Ping**

🧠 কাজ:
👉 অন্য ডিভাইস alive আছে কিনা চেক করা

📌 সহজভাবে:

> “Hello, শুনতে পাচ্ছ?”
> Reply এলে → alive ✅
> Reply না এলে → problem ❌

---

### 6️⃣ **Traceroute**

🧠 কাজ:
👉 তোমার data **কোন কোন router পার হয়ে যাচ্ছে** তা দেখানো

📌 সহজভাবে:

> ঢাকা → ভারত → সিঙ্গাপুর → USA
> এই পথটা Traceroute দেখায়

---

## 🖼️ পুরো বিষয়টা চোখে দেখলে আরও ক্লিয়ার হবে

![Image](https://campus.barracuda.com/resources/attachments/image/98210074/1/dhcp_enterprise_conf.png)

![Image](https://www.researchgate.net/publication/320322146/figure/fig1/AS%3A548239512018944%401507721893582/Typical-configuration-of-a-home-network-using-NAT.png)

![Image](https://marvel-b1-cdn.bc0a.com/f00000000310757/www.fortinet.com/content/dam/fortinet/images/cyberglossary/what-is-arp.jpg)

![Image](https://www.fourfaith.com/uploadfile/2022/0808/20220808105025195.jpg)

![Image](https://www.networkacademy.io/sites/default/files/inline-images/how-arp-works.gif)

---

