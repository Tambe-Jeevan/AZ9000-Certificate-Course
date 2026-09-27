## 🚀 AZ-900 — Day 5

**Internet Architecture: Routers, Switches, and Firewalls**

Welcome to **Day 5** of your AZ-900 journey! Over the last few days, you learned about computers, servers, virtualization, and local networking basics (IP, DNS, DHCP).

Today, we look at the physical and logical hardware that holds the internet and corporate networks together: **Routers, Switches, and Firewalls**. Understanding how these devices route and filter traffic is critical, because Azure replicates these exact concepts inside its cloud environment (as Virtual Networks and Network Security Groups).

---

### 📂 GitHub Repository Folder Name & Social Media Ready Tag

* **Folder Name:** `Day-05-Internet-Routers-Switches-Firewalls`
* **LinkedIn Post Headline Suggestion:** `Day 5 of my AZ-900 Cloud Journey: Mastering Routers, Switches, and Firewalls 🛡️`

---

### 🕑 Today's 2-Hour Plan

| Time | Activity |
| --- | --- |
| 0–20 min | What is a Switch? (Connecting devices *inside* a network) |
| 20–40 min | What is a Router? (Connecting different networks / Internet traffic director) |
| 40–60 min | What is a Firewall? (The security guard at the gate) |
| 60–80 min | Tracing how a request leaves your home and reaches a website |
| 80–105 min | Practical Lab: Running `tracert` and inspecting network hops on your PC |
| 105–120 min | AZ-900 Exam Points, Interview Questions & Quiz |

---

### 1. What is a Switch? (Connecting Inside a Network)

Imagine an office with 15 desktop computers and 2 printers. How do they all talk to each other without plugging cables into each other directly?

* **A Switch** connects devices **together within the same local network (LAN)**.
* **Analogy:** Think of a switch like a **multi-plug power strip** or an internal office intercom system. If Computer A wants to send a file to Computer B in the same room, the switch receives the data packet and forwards it *only* to Computer B, keeping internal office traffic fast and organized.
* **Key takeaway:** Switches operate **inside** a single network. They don't know anything about the outside internet; they just connect local neighbors.

---

### 2. What is a Router? (Connecting Different Networks)

What happens when your office computer wants to open `google.com`? Google is not on your local network—it lives on a server halfway across the world.

* **A Router** connects **different networks together** (e.g., connecting your local home/office network to the global Internet).
* **Analogy:** Think of a router like a **postal sorting office or a traffic cop at a major highway intersection**. When your computer sends a packet destined for the outside world, it hands it to the router. The router looks at the destination IP address and says, *"Ah, this needs to go out to the internet,"* and forwards it to the next network hop.
* **Key takeaway:** Routers bridge the gap between local private networks and the public internet.

---

### 3. What is a Firewall? (The Security Guard)

Not all traffic is good traffic. Hackers, malicious bots, and unauthorized users constantly try to break into corporate networks and servers.

* **A Firewall** is a security device (hardware or software) that **monitors and filters incoming and outgoing network traffic** based on a set of security rules.
* **Analogy:** Think of a firewall like a **security guard at a gated corporate building**. The guard checks every person's ID badge at the gate. If you are an employee (allowed traffic), you enter. If you are an unknown stranger trying to sneak in through a restricted door (unauthorized port/IP), the guard blocks you instantly.
* **Cloud Connection:** In Microsoft Azure, the physical equivalent of a firewall rule is called a **Network Security Group (NSG)**, which decides whether traffic can enter or leave your cloud virtual machines.

---

### 4. How a Request Travels (Putting It All Together)

Let's trace the exact path when you type `youtube.com` into your browser:

```text
Your Laptop 
    │ (Ethernet / Wi-Fi)
    ▼
Switch (Connects all devices in your house/office)
    │
    ▼
Router (Takes your packet, translates your private IP to public IP, and sends it to ISP)
    │
    ▼
Firewall (Checks if the traffic matches safety rules)
    │
    ▼
The Global Internet (Routers passing packets across undersea cables and global datacenters)
    │
    ▼
YouTube's Data Center Server

```

---

### 🛠️ 5. Practical Lab: Tracing Network Hops (`tracert`)

Let's see the routers and network hops in action using your Windows Command Prompt.

#### Lab Steps:

1. Press `Win + R`, type `cmd`, and press Enter.
2. Run the trace route command to Google's public DNS server:
```cmd
tracert 8.8.8.8

```


3. Watch the output scroll down line by line.
* Each line represents a **router hop** (a physical router your packet passes through as it leaves your home network, travels through your Internet Service Provider (ISP), and journeys across the global internet backbone).
* Notice the millisecond (ms) response times increasing as the distance grows!



---

### 🎯 AZ-900 Exam & Foundation Points From Day 5

* **Q1. What is the main difference between a switch and a router?**
* *Answer:* A switch connects devices *within* the same local network (LAN), while a router connects *different* networks together (like a LAN to the Internet).


* **Q2. What is the role of a firewall in a network?**
* *Answer:* To filter incoming and outgoing network traffic based on security rules, blocking unauthorized access while allowing legitimate traffic.


* **Q3. Why are routers necessary for internet communication?**
* *Answer:* Because individual local networks use private IP addresses that cannot be routed directly across the global public internet without translation and packet forwarding.



---

### 📝 Day 5 Quiz

*Try answering these out loud or in your notes:*

1. If three computers inside a small office need to share files locally without internet access, what networking device do they need?
2. If your home laptop wants to load a website hosted in the US, which device handles sending that packet out to the global internet?
3. If an unauthorized attacker tries to flood your server with malicious connection requests, what device or filter stops them?
4. What command can you run in Windows command prompt to see the exact routers your data passes through to reach a destination?

---

### 📝 LinkedIn & Social Media Post Template (Day 5)

> **🚀 Day 5 of my AZ-900 & Cloud Journey!**
> Today, I explored core internet architecture: **Routers, Switches, and Firewalls**. Understanding how these hardware and security components route and filter data is foundational before moving into Azure cloud networking.
> **What I learned & practiced today:**
> 🔀 **Switches:** Connecting and organizing devices *inside* a local network (LAN).
> 🌐 **Routers:** Acting as traffic directors to bridge different networks together and connect us to the global internet.
> 🛡️ **Firewalls:** Serving as the digital security guards at the gate to filter and block malicious traffic.
> **🛠️ Hands-on Labs Done:**
> * Ran `tracert 8.8.8.8` in the command prompt to trace the physical router hops packets take across the global internet.
> 
> 
> I am documenting my entire 0-to-hero journey. Check out my GitHub repo for daily notes and labs:
> 👉 **GitHub Repo:** `AZ900-Certificate-Course` (`Day-05-Internet-Routers-Switches-Firewalls`)
> #MicrosoftAzure #AZ900 #CloudComputing #Networking #SysAdmin #LearningEveryday #DevOpsJourney

---

*Great job completing Day 5! Let me know when you are ready for **Day 6: Virtualization & Hypervisors (Deep Dive)**!*