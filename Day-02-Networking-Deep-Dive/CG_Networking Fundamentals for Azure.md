# Day 2 — Networking Fundamentals for Azure

Today we’ll build directly on Day 1. **Do not worry if networking feels completely new.** We’ll start from zero and then connect every concept to Azure and real-world SysAdmin work.

## 🎯 Day 2 Goal

By the end of today, you should understand:

* IPv4 address
* Network / Host portion
* Subnet Mask
* Default Gateway
* Private vs Public IP
* MAC Address
* Ports
* TCP vs UDP
* Common ports
* HTTP vs HTTPS
* How a PC communicates with a server
* Basic network troubleshooting

**Study time: ~2 hours**

| Time   | Activity            |
| ------ | ------------------- |
| 40 min | Concepts            |
| 20 min | Real-world examples |
| 45 min | Hands-on lab        |
| 15 min | Revision + quiz     |

---

# Part 1 — What is a Network?

Very simply:

> **A network is a group of devices that can communicate with each other.**

Example at home:

```text
Laptop ─────┐
Phone ──────┤
TV ─────────┤── Wi-Fi Router ─── Internet
Printer ────┘
```

In an office:

```text
PC ───┐
PC ───┤
Laptop┤
Printer── Switch ─── Router ─── Internet
Server┤
AP ───┘
```

Your SysAdmin job will frequently involve answering:

> "Why can't this computer communicate with that server?"

To answer that, you need IP, subnet, gateway, DNS, ports, TCP/UDP, etc.

---

# Part 2 — IPv4 Address

You learned yesterday that an IP address identifies a device on a network.

Example:

```text
192.168.1.10
```

An IPv4 address contains **4 numbers**, called octets.

```text
192 . 168 . 1 . 10
 ↑     ↑    ↑    ↑
Octet Octet Octet Octet
```

Each octet can be:

```text
0 → 255
```

So:

```text
192.168.1.10     ✅
10.0.0.25        ✅
172.16.5.100     ✅
192.168.1.300    ❌
```

Why maximum 255?

Because each octet contains **8 bits**:

```text
8 bits = 2⁸ = 256 values

0 through 255 = 256 values
```

You don't need to memorize binary deeply today.

---

# Part 3 — Private IP Address

Private IP addresses are normally used **inside internal networks**.

The important private IPv4 ranges are:

| Range                         | Example      |
| ----------------------------- | ------------ |
| 10.0.0.0 – 10.255.255.255     | 10.10.1.20   |
| 172.16.0.0 – 172.31.255.255   | 172.16.5.20  |
| 192.168.0.0 – 192.168.255.255 | 192.168.1.20 |

Example:

```text
Office PC
192.168.10.25
       ↓
Office Router
       ↓
Internet
```

The PC's `192.168.10.25` is a **private IP**.

---

# Part 4 — Public IP

A **public IP** is an address used for communication over the public Internet.

For example:

```text
Your PC
192.168.1.20
     ↓
Router
Public IP
     ↓
Internet
```

Your home/office devices can have private IPs while the router communicates with the Internet using a public IP.

### Important

Do NOT think:

> Private IP = bad
> Public IP = good

They have different purposes.

---

# Part 5 — Subnet Mask ⭐

This is one of today's most important concepts.

Suppose you have:

```text
IP Address:
192.168.1.10

Subnet Mask:
255.255.255.0
```

The subnet mask helps determine:

> **Which part identifies the network and which part identifies the host/device.**

Think of an apartment building.

```text
Building = Network
Flat      = Host
```

For:

```text
192.168.1.10
255.255.255.0
```

you can conceptually think:

```text
Network       Host
192.168.1     .10
```

So:

```text
192.168.1.10
192.168.1.20
192.168.1.30
```

are normally in the same `/24` network:

```text
192.168.1.0/24
```

---

# Part 6 — What is `/24`?

You will see this frequently in Azure and networking.

Instead of writing:

```text
IP:          192.168.1.10
Subnet Mask: 255.255.255.0
```

we can write:

```text
192.168.1.10/24
```

`/24` means:

```text
24 bits = network portion
8 bits  = host portion
```

For now remember:

```text
255.255.255.0 = /24
```

This is enough for today's lesson.

---

# Part 7 — Default Gateway ⭐

Imagine your laptop wants to communicate with a server on another network.

Who should it send the traffic to?

👉 **Default Gateway**

Usually this is your router.

Example:

```text
Laptop
192.168.1.20
      |
      | 
      v
Gateway / Router
192.168.1.1
      |
      v
Internet
```

Your PC essentially says:

> "I don't know how to reach that external network, so I'll give the packet to my gateway."

### Simple definition

> **Default Gateway = device that provides a path from your local network to other networks.**

---

# Part 8 — MAC Address

IP address works at the network layer, but your network card also has a hardware address called a:

**MAC Address**

Example:

```text
A4-BB-6D-12-34-56
```

or:

```text
A4:BB:6D:12:34:56
```

Think:

```text
IP address → network identity/address
MAC address → network interface hardware identity
```

You don't need to go deeply into OSI today.

---

# Part 9 — IP vs MAC

Very important for interviews.

| IP Address                     | MAC Address                                   |
| ------------------------------ | --------------------------------------------- |
| Logical address                | Hardware/interface address                    |
| Used for network communication | Used for local network communication          |
| Can change                     | Usually associated with the network interface |
| Example `192.168.1.10`         | Example `A4-BB-6D-12-34-56`                   |

---

# Part 10 — Ports ⭐⭐⭐

Now an extremely important SysAdmin concept.

Suppose your computer has one IP:

```text
192.168.1.10
```

But many applications are running:

```text
Web
SSH
DNS
RDP
Database
```

How does the computer know which application should receive the traffic?

👉 **Port number**

Think of:

```text
IP address = Building address
Port       = Apartment/department number
```

Example:

```text
192.168.1.10:443
```

means:

```text
IP   = 192.168.1.10
Port = 443
```

---

# Part 11 — Important Ports for SysAdmin/Azure

You don't need hundreds of ports.

Start with these:

|  Port | Protocol | Common use       |
| ----: | -------- | ---------------- |
| 20/21 | TCP      | FTP              |
|    22 | TCP      | SSH              |
|    25 | TCP      | SMTP             |
|    53 | TCP/UDP  | DNS              |
|    80 | TCP      | HTTP             |
|   110 | TCP      | POP3             |
|   143 | TCP      | IMAP             |
|   443 | TCP      | HTTPS            |
|  3389 | TCP      | RDP              |
|   445 | TCP      | SMB/File Sharing |
|  5985 | TCP      | WinRM HTTP       |
|  5986 | TCP      | WinRM HTTPS      |

### Most important for you

Remember these first:

```text
22   → SSH
53   → DNS
80   → HTTP
443  → HTTPS
3389 → RDP
445  → SMB
```

---

# Part 12 — TCP vs UDP ⭐⭐⭐

Two important transport protocols.

## TCP

TCP is:

> **Connection-oriented and reliable.**

It makes sure data is delivered properly and in order.

Examples:

```text
HTTPS
SSH
RDP
SMB
```

Think:

> "Before talking, let's establish a reliable connection."

---

## UDP

UDP is:

> **Connectionless and faster, but doesn't provide TCP-style delivery guarantees.**

Examples include:

```text
DNS queries (commonly)
DHCP
VoIP
Streaming
Online gaming
```

Think:

> "Send the data quickly without establishing a full connection first."

### Simple comparison

| TCP                     | UDP                                           |
| ----------------------- | --------------------------------------------- |
| Reliable delivery       | No TCP-style reliability                      |
| Connection-oriented     | Connectionless                                |
| More overhead           | Less overhead                                 |
| Ordering/retransmission | No built-in TCP-style ordering/retransmission |
| HTTPS, SSH, RDP         | DNS, DHCP, VoIP                               |

---

# Part 13 — HTTP vs HTTPS

You use websites every day.

### HTTP

```text
http://example.com
```

Uses:

```text
Port 80
```

### HTTPS

```text
https://example.com
```

Uses:

```text
Port 443
```

HTTPS provides encrypted communication using TLS.

For SysAdmin/Azure, **443 is extremely important**.

---

# Part 14 — How Your Browser Reaches a Website

This is where everything starts connecting.

You type:

```text
https://www.microsoft.com
```

### Step 1 — DNS

Your computer asks:

> "What IP address belongs to [www.microsoft.com](http://www.microsoft.com)?"

DNS responds with an IP address.

```text
www.microsoft.com
       ↓
     DNS
       ↓
   IP address
```

### Step 2 — Destination port

Because you're using HTTPS:

```text
Port = 443
```

So conceptually:

```text
Your PC
192.168.1.20
      |
      | HTTPS
      | destination port 443
      ↓
Internet
      ↓
Microsoft Web Server
```

This is the basic flow you should understand.

---

# Part 15 — DHCP

You saw DHCP yesterday.

Let's connect it with today's concepts.

When your laptop joins a network, it needs things like:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
```

DHCP can automatically provide these settings.

Example:

```text
Laptop
   |
   | "I need network configuration"
   ↓
DHCP Server
   |
   ↓
IP:       192.168.1.25
Mask:     255.255.255.0
Gateway:  192.168.1.1
DNS:      192.168.1.1
```

Without proper configuration, network communication may fail.

---

# Part 16 — Practical Lab 🧪

Now let's actually inspect your Windows computer.

## Lab 1 — Check your IP

Open **CMD**.

Run:

```cmd
ipconfig
```

You'll see something similar to:

```text
IPv4 Address. . . . . . : 192.168.1.25
Subnet Mask . . . . . . : 255.255.255.0
Default Gateway . . . . : 192.168.1.1
```

### Your task

Identify:

```text
IPv4:
Subnet Mask:
Default Gateway:
```

Don't worry if yours looks different.

---

# Lab 2 — Get complete network information

Run:

```cmd
ipconfig /all
```

Find:

```text
Physical Address
DHCP Enabled
IPv4 Address
Subnet Mask
Default Gateway
DNS Servers
```

This is a **very useful command for a SysAdmin**.

---

# Lab 3 — Find your MAC address

Run:

```cmd
getmac
```

Or:

```cmd
ipconfig /all
```

Look for:

```text
Physical Address
```

Example:

```text
Physical Address. . . . . : 3C-52-82-AA-BB-CC
```

That's your network adapter's MAC address.

---

# Lab 4 — Test your local TCP/IP stack

Run:

```cmd
ping 127.0.0.1
```

You should normally receive:

```text
Reply from 127.0.0.1
```

Why?

`127.0.0.1` is the **localhost/loopback address**.

It tests communication with your own computer.

```text
Your PC
  ↕
127.0.0.1
```

---

# Lab 5 — Test your Gateway

First find your gateway:

```cmd
ipconfig
```

Suppose it says:

```text
Default Gateway : 192.168.1.1
```

Then run:

```cmd
ping 192.168.1.1
```

If you get replies:

```text
Reply from 192.168.1.1
```

your PC can communicate with the gateway.

---

# Lab 6 — Test Internet connectivity

Run:

```cmd
ping 8.8.8.8
```

If you get replies, your machine can reach Google's public DNS IP.

Then:

```cmd
ping google.com
```

Now you're testing both:

1. Network connectivity
2. DNS name resolution

This gives us a useful troubleshooting idea.

---

# ⭐ SysAdmin Troubleshooting Logic

Suppose:

```cmd
ping 8.8.8.8
```

works.

But:

```cmd
ping google.com
```

fails.

What might be wrong?

👉 **DNS could be the problem.**

Because:

```text
8.8.8.8
```

is already an IP.

But:

```text
google.com
```

requires DNS resolution.

---

# Lab 7 — Test DNS directly

Run:

```cmd
nslookup google.com
```

You should see information including an IP address.

Example:

```text
Name:    google.com
Address: xxx.xxx.xxx.xxx
```

This is one of the commands you should become comfortable with as a SysAdmin.

---

# Lab 8 — Check a port

Windows has PowerShell's:

```powershell
Test-NetConnection
```

Open PowerShell and run:

```powershell
Test-NetConnection google.com -Port 443
```

Look for:

```text
TcpTestSucceeded : True
```

If true:

> Your computer successfully established a TCP connection to that destination port.

Try:

```powershell
Test-NetConnection google.com -Port 80
```

And:

```powershell
Test-NetConnection google.com -Port 3389
```

Don't worry if 3389 fails. A public website normally doesn't expose RDP to you.

---

# Part 17 — Real-World SysAdmin Example

Imagine an employee says:

> **"I can't access the company's application."**

Don't immediately reinstall Windows. 😄

A SysAdmin thinks systematically.

### Step 1 — Check IP

```cmd
ipconfig
```

Does the computer have a valid IP?

---

### Step 2 — Check gateway

```cmd
ping <gateway>
```

Can the PC reach the local network gateway?

---

### Step 3 — Check Internet/network

```cmd
ping 8.8.8.8
```

---

### Step 4 — Check DNS

```cmd
nslookup application.company.com
```

---

### Step 5 — Check application port

For HTTPS:

```powershell
Test-NetConnection application.company.com -Port 443
```

If the application uses another port, test that port.

---

# Part 18 — Azure Connection ⭐

Everything you're learning today appears in Azure.

Imagine an Azure VM.

```text
Azure VM
   |
   ├── Private IP
   |
   ├── Network Interface
   |
   ├── Subnet
   |
   ├── Virtual Network
   |
   └── NSG / Firewall rules
```

For example:

```text
VNet
10.0.0.0/16
       |
       ├── Web Subnet
       |      10.0.1.0/24
       |
       └── App Subnet
              10.0.2.0/24
```

A VM could have:

```text
Private IP:
10.0.1.10
```

Later you'll learn how Azure networking uses:

* Virtual Network (VNet)
* Subnet
* Network Interface
* Private IP
* Public IP
* NSG
* Routing
* DNS

Today's concepts are the **foundation** for all of that.

---

# 🧠 Day 2 — What You Must Remember

Don't try to memorize everything.

Focus on this:

```text
IP Address
    ↓
Identifies a device/interface on a network

Subnet Mask
    ↓
Helps define network and host portions

Default Gateway
    ↓
Path to other networks

MAC Address
    ↓
Network interface hardware address

Port
    ↓
Identifies a network service/application endpoint

TCP
    ↓
Reliable, connection-oriented transport

UDP
    ↓
Connectionless, lightweight transport

DNS
    ↓
Name → IP

DHCP
    ↓
Automatically provides network configuration

HTTP
    ↓
Port 80

HTTPS
    ↓
Port 443

RDP
    ↓
Port 3389

SSH
    ↓
Port 22

SMB
    ↓
Port 445
```

---

# 🎯 AZ-900 Exam Connection

For AZ-900, don't spend huge amounts of time becoming a network engineer.

You mainly need to understand Azure networking concepts and their purpose.

Today's foundation will make these later topics much easier:

```text
IP
 ↓
Subnet
 ↓
Network
 ↓
Azure VNet
 ↓
Azure Subnet
 ↓
Azure VM
 ↓
NSG
 ↓
Private/Public connectivity
```

---

# 📝 Day 2 Interview Questions

Try answering these **without looking above**.

### Q1. What is an IP address?

### Q2. What is a private IP?

### Q3. What is a subnet mask?

### Q4. What is the purpose of a default gateway?

### Q5. What is a MAC address?

### Q6. What is a port?

### Q7. What is the difference between TCP and UDP?

### Q8. What port does HTTPS use?

### Q9. What port does RDP use?

### Q10. What port does SSH use?

### Q11. What is DNS?

### Q12. What is DHCP?

### Q13. What is the difference between `ping 8.8.8.8` and `ping google.com`?

### Q14. What does `Test-NetConnection` help you check?

---

# 🧪 Day 2 Mini Task

On your Windows laptop, run:

```cmd
ipconfig /all
```

Then:

```cmd
ping 127.0.0.1
```

Then:

```cmd
ping <your-default-gateway>
```

Then:

```cmd
ping 8.8.8.8
```

Then:

```cmd
nslookup google.com
```

Finally in PowerShell:

```powershell
Test-NetConnection google.com -Port 443
```

Write down the results.

### Your Day 2 target:

You should be able to explain this diagram:

```text
             INTERNET
                 |
          Default Gateway
                 |
          ┌──────┴──────┐
          │   Network   │
          │192.168.1.0/24
          │             │
       Laptop         Server
   192.168.1.10    192.168.1.20
          |
       DNS Server
```

And explain:

**IP → Subnet → Gateway → DNS → Port → TCP/UDP**

---

## ✅ Day 2 Completion Checklist

* [ ] Understand IPv4
* [ ] Understand private/public IP
* [ ] Understand subnet mask
* [ ] Understand `/24`
* [ ] Understand default gateway
* [ ] Understand MAC address
* [ ] Understand ports
* [ ] Know TCP vs UDP
* [ ] Know HTTP/HTTPS
* [ ] Know ports 22, 53, 80, 443, 445, 3389
* [ ] Run `ipconfig /all`
* [ ] Run `ping`
* [ ] Run `nslookup`
* [ ] Run `Test-NetConnection`
* [ ] Understand basic network troubleshooting

**Day 3** will move into **subnetting without making it complicated**: network address, host address, broadcast address, `/24`, `/25`, `/26`, how many devices fit in a subnet, and—most importantly—how this directly maps to **Azure VNet + Subnet design**.
