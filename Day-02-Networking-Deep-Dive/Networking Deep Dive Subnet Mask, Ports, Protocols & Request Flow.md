## 🚀 AZ-900 — Day 2

**Networking Deep Dive: Subnet Mask, Ports, Protocols & Request Flow**

Yesterday, we built your foundational vocabulary: Computer, Server, Data Center, IP, DNS, Virtualization, and Azure basics (Subscription, Resource Group, Resource).

Today, we go one layer deeper into **Networking**. To understand how Azure networking works later, you must first understand how computers talk to each other across a room and across the globe.

---

### 🕑 Today's 2-Hour Plan

| Time | Activity |
| --- | --- |
| 0–15 min | Subnet Mask & Network vs. Host ID |
| 15–30 min | What is a Port? (The "Doors" of a computer) |
| 30–45 min | TCP vs. UDP (Reliability vs. Speed) |
| 45–60 min | HTTP vs. HTTPS (Security on the Web) |
| 60–75 min | How a Real User Request Travels from PC to Azure |
| 75–105 min | Practical Labs (`netstat`, port checking, network tracing) |
| 105–120 min | Quiz & Completion Checklist |

---

### 1. What is a Subnet Mask?

Yesterday you learned about IPv4 addresses (like `192.168.1.10`). But how does your computer know if another computer is sitting in the same room on your local network, or if it is halfway across the world on the internet?

The **Subnet Mask** answers this question.

A subnet mask works like a pair of glasses that splits an IP address into two parts:

1. **Network ID:** Identifies the specific network/building.
2. **Host ID:** Identifies the specific device inside that building.

#### Real-World Analogy: Office Building

Imagine an apartment complex:

* **Network ID** = Building Number (e.g., Building 5)
* **Host ID** = Flat Number (e.g., Flat 402)

If you want to mail a letter to someone in the same building, you only need to know their flat number. If they are in another building, you need the building number too.

In computer networking, a standard home subnet mask looks like this:

```text
IP Address:  192.168.1.10
Subnet Mask: 255.255.255.0

```

* The `255.255.255` part means: *"Match the network ID (`192.168.1`)."*
* The `.0` part means: *"The final number (`10`) is the unique device on this network."*

---

### 2. What is a Port?

If an **IP address** is the street address of an apartment building, a **Port** is the specific apartment door or room number inside that building.

A single computer can run multiple services at the same time (e.g., a web server, a database server, a remote desktop connection). How does incoming traffic know where to go? Through **Ports**.

* Ports range from **0 to 65535**.
* Certain ports are reserved for standard services:
* **Port 80:** HTTP (Unencrypted web traffic)
* **Port 443:** HTTPS (Encrypted secure web traffic)
* **Port 3389:** RDP (Remote Desktop to log into Windows VMs)
* **Port 22:** SSH (Secure Shell to log into Linux VMs)



Think:

```text
IP Address 192.168.1.50 + Port 443 
       ↓
"Deliver this web request specifically to the secure web server software running inside this computer."

```

---

### 3. TCP vs. UDP

When computers send data over a network, they use different transport protocols. The two most important ones for your career are **TCP** and **UDP**.

#### TCP (Transmission Control Protocol)

* **How it works:** It establishes a strict connection first (called a *3-way handshake*). It checks if every single packet of data arrived safely. If a packet is lost, it asks for it to be resent.
* **Analogy:** Making a phone call. ("Hello?" "Hi!" "Can you hear me?" "Yes." -> Conversation starts. If you miss a word, you ask the person to repeat it).
* **Use case:** Web browsing (HTTP/S), sending emails, downloading files, database transactions where missing data causes corruption.

#### UDP (User Datagram Protocol)

* **How it works:** It fires data packets out rapidly without checking if the receiver is ready or if packets got lost. It prioritizes **speed** over perfection.
* **Analogy:** Broadcasting a radio station or throwing leaflets from a helicopter. You throw them out; whether people catch them or not, you keep flying.
* **Use case:** Live video streaming, online gaming, VoIP calls (where a missing millisecond of audio doesn't ruin the whole call).

---

### 4. HTTP vs. HTTPS

When you browse the web, how is your data protected?

* **HTTP (HyperText Transfer Protocol):**
* Data travels in **plain text**. Anyone tapping into your Wi-Fi router or network switch can read your passwords, credit card numbers, or messages. *(Port 80)*


* **HTTPS (HTTP Secure):**
* Data is **encrypted** using SSL/TLS certificates. Even if someone intercepts your network traffic, they only see scrambled gibberish. *(Port 443)*



**Azure Rule of Thumb:** In modern cloud architecture, you should almost always enforce HTTPS to secure data in transit.

---

### 5. How a Real Request Travels from PC to Azure

Let's put everything together. Imagine you deploy a website on an Azure Virtual Machine, and you type `mycompany.com` into your browser. Here is the exact path your request takes:

```text
1. You type "mycompany.com" in Chrome.
   │
2. DNS Lookup: Your PC asks a DNS server, "What is the IP address for mycompany.com?"
   │
3. DNS returns the Azure Public IP address (e.g., 20.45.10.5).
   │
4. Your PC packs your request into TCP packets, targeting IP `20.45.10.5` on Port `443`.
   │
5. Your Home Router sends the packet out to the global Internet.
   │
6. The packet hits Microsoft Azure's global network edge router.
   │
7. Azure's Network Security Group (NSG) checks: "Is Port 443 allowed?" (If yes, traffic passes).
   │
8. The packet reaches your Azure Virtual Machine, enters through Port 443, and your web server responds!

```

This exact journey happens in milliseconds. Understanding this flow makes configuring Azure VNets, firewalls, and load balancers much easier later.

---

### 🧪 DAY 2 PRACTICAL LAB

Let's inspect active ports and connections on your own computer right now.

#### Lab 1 — Check active network connections (`netstat`)

1. Open **Command Prompt** (CMD).
2. Run the following command to see all active network connections and ports your PC is using:
```cmd
netstat -ano

```


3. Look at the **Foreign Address** and **State** columns. Notice how many established connections your computer has open with various servers on the internet.

#### Lab 2 — Test a website port

1. In CMD, test if a specific server is listening on port 443 using PowerShell command or telnet (if enabled), or use a simple ping test:
```cmd
ping 8.8.8.8

```


2. For an advanced test, open PowerShell and run:
```powershell
Test-NetConnection google.com -Port 443

```


*Notice the output: `TcpTestSucceeded : True`. This proves Port 443 is open and responding!*

---

### 🎯 AZ-900 Exam & Foundation Points From Day 2

* **Q1. What is the role of a Subnet Mask?**
* *Answer:* It separates an IP address into a Network ID and a Host ID.


* **Q2. What is a network port used for?**
* *Answer:* It acts as a logical endpoint to direct incoming traffic to the correct application/service on a computer (e.g., Port 443 for HTTPS).


* **Q3. What is the difference between TCP and UDP?**
* *Answer:* TCP is connection-oriented and guarantees delivery (reliable); UDP is connectionless and sends data rapidly without checking delivery (fast/streaming).


* **Q4. Why is HTTPS preferred over HTTP?**
* *Answer:* HTTPS encrypts data in transit to protect it from being intercepted.



---

### 📝 Day 2 Quiz

*Try answering these without scrolling back up:*

1. What does a subnet mask help your computer determine?
2. What standard port number is used for secure web traffic (HTTPS)?
3. If you are streaming a live video game match, is the network protocol more likely to be TCP or UDP? Why?
4. What is the main security risk of using plain HTTP instead of HTTPS?
5. When a packet reaches an Azure data center, what security filter checks whether the port is allowed?

---

### ✅ Day 2 Completion Checklist

Before moving to Day 3, make sure you can explain these concepts clearly:

* Subnet Mask
* Network ID vs. Host ID
* Ports (80, 443, 3389, 22)
* TCP 3-Way Handshake & Reliability
* UDP Speed
* HTTP vs. HTTPS Encryption
* Request journey from client to cloud

---

*Great job finishing Day 2! Let me know when you are ready for **Day 3: Cloud Deployment Models (Public, Private, Hybrid) & Service Models (IaaS, PaaS, SaaS)**!*