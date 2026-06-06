# Lab Room: Inside a Computer System
### Date Completed: 5 June 2026
### Tools: tryhackme box

---

## Part 1: Core Computer Hardware Components

### 1. Motherboard

* **Analogy:** The human skeleton and nervous system.
* **Detailed Explanation:** The motherboard is the main printed circuit board (PCB) of a computer. It serves as the central hub that physically holds the entire system together and allows all other hardware components to communicate with one another.
* **Key Features & Connections:** It contains dedicated slots and sockets for major components, including the CPU socket, RAM slots (DIMM slots), expansion slots (PCIe), and various storage/power ports. Every single component in a computer system either plugs directly into the motherboard or connects to it via cables.

### 2. CPU (Central Processing Unit)

* **Analogy:** The human brain (specifically the execution of tasks).
* **Detailed Explanation:** Often referred to as the processor, the CPU is the primary component that executes instructions and processes data for the computer. It continuously carries out arithmetic, logical, control, and input/output operations specified by the instructions.
* **Key Features & Connections:** Modern CPUs feature multiple "cores," allowing them to process multiple streams of instructions simultaneously (parallel processing). The CPU is mounted directly onto the motherboard via a specialized CPU socket.

### 3. RAM (Random Access Memory)

* **Analogy:** Short-term or working memory.
* **Detailed Explanation:** RAM is the ultra-fast, temporary workspace where the computer stores data it needs to access immediately. When you open a program or file, it is loaded into RAM so the CPU can work with it without delay.
* **Key Features & Connections:** RAM is **volatile memory**, meaning it requires constant electrical power to maintain its data; the moment the computer is turned off, everything stored in RAM is lost. Modern systems utilize advanced standards like DDR5 or DDR6 to achieve incredibly fast data transfer speeds.

### 4. Storage (SSD / HDD)

* **Analogy:** Long-term memory.
* **Detailed Explanation:** Storage devices permanently retain your data, files, applications, and operating system, even when the computer is completely powered down (**non-volatile memory**).
* **Types of Storage:**
* **HDD (Hard Disk Drive):** An older technology that relies on mechanical spinning platters and moving magnetic heads to read/write data. While slower, they offer very large storage capacities at a lower cost.
* **SSD (Solid State Drive):** A modern technology with no moving parts. It utilizes flash memory chips to read and write data at exponentially faster speeds than HDDs.


* **Key Features & Connections:** Storage devices connect to the motherboard using SATA cables or slot directly into high-speed PCI Express (PCIe/NVMe) slots.

### 5. Network Adapter

* **Analogy:** Human vocal cords and ears (communication tools).
* **Detailed Explanation:** A network adapter—also known as a Network Interface Card (NIC)—enables the computer to connect and communicate with other computers, local networks, and the internet.
* **Key Features & Connections:** These adapters come in both wired (Ethernet) and wireless (Wi-Fi) variants. While they are frequently integrated directly into modern motherboards, they can also be installed as standalone expansion cards via PCI Express ports.

### 6. Power Supply Unit (PSU)

* **Analogy:** The human heart (pumping blood/energy to organs).
* **Detailed Explanation:** The PSU is the lifeblood of the computer. It draws alternating current (AC) power from an electrical wall outlet and converts it into regulated direct current (DC) power required by the computer’s internal components.
* **Key Features & Connections:** Choosing a PSU requires careful power calculation; if the components demand more wattage than the PSU can safely deliver, the system will become unstable or fail to boot. It distributes power through a network of cables, including the main 24-pin motherboard connector and legacy Molex connectors.

### 7. Graphics Card (GPU)

* **Analogy:** The visual cortex of the brain.
* **Detailed Explanation:** Dedicated to processing visual data, the graphics card (Graphics Processing Unit) takes instructions from the operating system and software applications, renders them into images, and outputs those images onto a display monitor.
* **Key Features & Connections:** It handles complex 3D rendering, video playback, and visual tasks so that the main CPU doesn't get overwhelmed. The graphics card plugs directly into the high-bandwidth PCI Express slots on the motherboard.

### 8. Input/Output (I/O) Devices

* **Analogy:** The human senses (eyes, ears, touch) and expressions.
* **Detailed Explanation:** These peripheral devices allow users to interact with the computer system, providing a means to input data or receive processed results.
* **Categories:**
* **Input Devices:** Tools used to feed data into the computer (e.g., keyboards, mice, microphones, and scanners).
* **Output Devices:** Tools used to project info out to the user (e.g., monitors, speakers, and printers).


* **Key Features & Connections:** These peripherals interface with the computer via standard external ports such as USB, HDMI, and DisplayPort.

---

## Part 2: The Boot-Up Process (What Happens When You Press Power)

When you turn on a computer, it goes through a strict 5-step sequence to safely transition from a powered-down state into a fully operational system.

```
[Power Button] ➔ [Firmware (UEFI)] ➔ [POST Test] ➔ [Select Boot Device] ➔ [Bootloader (OS Loaded)]

```

### Step 1: Press the Power Button

* **What Happens:** Pressing the physical power button completes an electrical circuit, sending an electronic signal directly to the Power Supply Unit (PSU).
* **Result:** The PSU wakes up, begins converting electricity, and allows operational power to flow through the motherboard to wake up the core hardware components.

### Step 2: Firmware Starts

* **What Happens:** With initial power established, the hardware components turn on, but the CPU does not yet know how to communicate with them. The system immediately initializes its built-in firmware.
* **Key Concept:** This central system is called **UEFI (Unified Extensible Firmware Interface)**.

> **Note:** You will frequently hear this called **BIOS (Basic Input/Output System)**. While BIOS is the older legacy standard, modern computers have replaced it with the faster, more secure UEFI, though they serve the exact same core purpose.

### Step 3: Power-On Self Test (POST)

* **What Happens:** The UEFI firmware executes a critical diagnostic routine known as the **Power-On Self Test (POST)**.
* **Result:** The system checks to ensure that all critical components (CPU, RAM, Storage, Graphics Card, etc.) are present, properly configured, and functioning normally. If a required component is missing or broken, the motherboard will flag an error (often through distinct audio beeps or error lights) and halt the boot process.

### Step 4: Select Boot Device

* **What Happens:** Once the POST successfully verifies that all hardware is safe to use, the UEFI searches for a place to load the computer's "consciousness" (the Operating System).
* **Result:** The UEFI reads a pre-configured, ordered priority list stored in its settings. It checks the devices on this list one by one (e.g., NVMe SSD, SATA SSD, HDD, or a USB drive) to find where the bootable files reside.

### Step 5: Initiate Bootloader

* **What Happens:** Once the UEFI successfully identifies the correct boot device, it locates a small, specialized piece of software called the **bootloader**.
* **Final Action:** The bootloader takes over and begins transferring the Operating System files from the permanent storage device (SSD/HDD) into the fast, temporary working memory (RAM). Once the OS is completely loaded into the RAM, the UEFI officially hands over control of the computer hardware to the Operating System, presenting you with your desktop interface.

---
Lab Task
---

## Cunnect other parts into Motherboard:
<img width="1252" height="682" alt="image" src="https://github.com/user-attachments/assets/0733c027-f305-4ec5-a5f9-083bb811e0b4" />

<img width="1347" height="877" alt="image" src="https://github.com/user-attachments/assets/172fae8e-2155-4438-ad22-8bced8449759" />

<img width="1277" height="662" alt="image" src="https://github.com/user-attachments/assets/a8d90213-5535-44b6-bcab-269154e7a805" />
