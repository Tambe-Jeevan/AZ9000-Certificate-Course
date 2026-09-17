# 🚀 AZ-900 — Day 03

## Subnetting & Azure Virtual Network (VNet) Basics

Today we connect yesterday's networking concepts to **Azure networking**. Don't worry—I'm going to teach subnetting from absolute zero and only the level you actually need for **AZ-900 + junior SysAdmin**.

### 🎯 Today's Goal

By the end of Day 3, you should understand:

* What a network address is
* What a host address is
* What a subnet is
* What subnetting means
* What `/24`, `/25`, `/26` mean
* Network address vs usable IP vs broadcast address
* How many devices a subnet can support
* What an Azure **VNet** is
* What an Azure **Subnet** is
* VNet vs Subnet
* How Azure VM networking is structured
* Basic Azure IP planning

---

# ⏱️ 2-Hour Study Plan

| Time        | Activity                 |
| ----------- | ------------------------ |
| 0–40 min    | Networking concepts      |
| 40–60 min   | Real-world examples      |
| 60–105 min  | Practical subnetting lab |
| 105–120 min | AZ-900 revision + quiz   |

---

# Part 1 — First Understand What a Network Is

Imagine a company has:

```text
Company Network
       |
       +---- PC-01
       +---- PC-02
       +---- PC-03
       +---- Printer
       +---- Server
```

All these devices need addresses so they can communicate.

For example:

```text
PC-01       192.168.10.10
PC-02       192.168.10.11
Printer     192.168.10.20
Server      192.168.10.50
```

Here:

```text
192.168.10
```

can represent the **network portion**, while the final number can represent the **host/device portion**.

---

# Part 2 — What Is a Subnet?

A **subnet** is a smaller logical network created inside a larger network.

Think of a company building:

```text
Company
│
├── HR
├── Finance
├── IT
├── Sales
└── Management
```

Instead of putting every device into one giant network, IT can separate them into different networks/subnets.

For example:

```text
192.168.10.0/24
```

could be divided into:

```text
HR       → 192.168.10.0/26
Finance  → 192.168.10.64/26
IT       → 192.168.10.128/26
Sales    → 192.168.10.192/26
```

This is **subnetting**.

---

# Part 3 — What Does `/24` Mean?

You saw this yesterday:

```text
192.168.10.0/24
```

The `/24` is called **CIDR notation**.

CIDR = **Classless Inter-Domain Routing**.

You don't need to memorize the complicated definition for AZ-900.

Just remember:

> `/24` tells us how many bits belong to the network portion.

IPv4 has **32 bits**.

So:

```text
/24
```

means:

```text
24 network bits
+
8 host bits
=
32 bits
```

Its subnet mask is:

```text
255.255.255.0
```

Therefore:

```text
192.168.10.0/24
```

is equivalent to:

```text
192.168.10.0
Subnet Mask: 255.255.255.0
```

---

# Part 4 — The Most Important `/24` Example

Consider:

```text
192.168.10.0/24
```

The addresses are:

```text
192.168.10.0
192.168.10.1
192.168.10.2
...
192.168.10.254
192.168.10.255
```

There are:

**256 total addresses**

But not all 256 are normally assigned to devices in a traditional IPv4 subnet.

### Network address

```text
192.168.10.0
```

Identifies the network itself.

### Usable host addresses

```text
192.168.10.1
-
192.168.10.254
```

These can normally be assigned to devices.

### Broadcast address

```text
192.168.10.255
```

Used to communicate with all hosts on that subnet in traditional IPv4 networking.

So:

```text
192.168.10.0/24

Network       → .0
Usable        → .1 – .254
Broadcast     → .255
```

### Usable hosts

```text
256 - 2 = 254
```

So `/24` traditionally provides **254 usable IPv4 host addresses**.

---

# Part 5 — Why Do We Need Subnetting?

Suppose your company has:

```text
1000 computers
```

Putting everything into one huge network isn't always ideal.

You can divide the network:

```text
Company Network
192.168.10.0/24
        │
        ├── IT
        ├── HR
        ├── Finance
        └── Sales
```

Benefits include:

* Better organization
* Smaller broadcast domains
* Easier IP management
* Better network segmentation
* Security design
* Easier troubleshooting

For a SysAdmin, **IP planning and subnet understanding are important**.

---

# Part 6 — `/25`

Now let's make the network smaller.

```text
192.168.10.0/25
```

`/25` means:

```text
25 network bits
7 host bits
```

Total addresses:

```text
2⁷ = 128
```

Traditional usable hosts:

```text
128 - 2 = 126
```

So:

```text
192.168.10.0/25
```

has:

```text
Network     → 192.168.10.0
Usable      → 192.168.10.1 – 192.168.10.126
Broadcast   → 192.168.10.127
```

---

# Part 7 — `/26`

Now:

```text
192.168.10.0/26
```

Host bits:

```text
32 - 26 = 6
```

Total addresses:

```text
2⁶ = 64
```

Usable:

```text
64 - 2 = 62
```

So:

```text
192.168.10.0/26
```

means:

```text
Network     → .0
Usable      → .1 – .62
Broadcast   → .63
```

The next `/26` subnet begins at:

```text
192.168.10.64
```

Then:

```text
192.168.10.64/26
```

Usable:

```text
192.168.10.65 – .126
```

Broadcast:

```text
192.168.10.127
```

---

# ⭐ Remember This Table

| CIDR  | Total IPs | Traditional Usable Hosts |
| ----- | --------: | -----------------------: |
| `/24` |       256 |                      254 |
| `/25` |       128 |                      126 |
| `/26` |        64 |                       62 |
| `/27` |        32 |                       30 |
| `/28` |        16 |                       14 |

For AZ-900, you don't need to become a subnetting expert.

You mainly need to understand **what a subnet is and how it relates to Azure VNets**.

---

# Part 8 — Very Important: VNet

Now we move into Azure.

## What is an Azure VNet?

**VNet = Virtual Network**

An Azure Virtual Network is a logically isolated network in Azure where Azure resources can communicate.

Think of it as your company's virtual network inside Azure.

Example:

```text
Azure
│
└── VNet
    │
    ├── Web Subnet
    │
    ├── App Subnet
    │
    └── Database Subnet
```

---

# Part 9 — VNet vs Subnet

This is very important.

### VNet

The overall virtual network.

### Subnet

A smaller network segment inside the VNet.

Think:

```text
VNet
│
├── Subnet 1
├── Subnet 2
└── Subnet 3
```

Real-world analogy:

```text
Apartment Complex
        ↓
       VNet

Buildings
        ↓
      Subnets

Apartments
        ↓
       VMs
```

---

# Part 10 — Example Azure VNet

Suppose we create:

```text
VNet Name:
Company-VNet

Address Space:
10.0.0.0/16
```

Inside it:

```text
Company-VNet
10.0.0.0/16
│
├── Web-Subnet
│   10.0.1.0/24
│
├── App-Subnet
│   10.0.2.0/24
│
└── DB-Subnet
    10.0.3.0/24
```

Visual:

```text
                 Azure
                   │
            Company-VNet
             10.0.0.0/16
                   │
       ┌───────────┼───────────┐
       │           │           │
       ▼           ▼           ▼
     Web          App          DB
   Subnet       Subnet       Subnet
 10.0.1.0/24  10.0.2.0/24  10.0.3.0/24
```

---

# Part 11 — Why Use Different Subnets?

Imagine:

```text
Web Server
Application Server
Database Server
```

You don't necessarily want them all in the same subnet.

A common design could be:

```text
VNet
│
├── Web Subnet
│     └── Web VM
│
├── App Subnet
│     └── App VM
│
└── DB Subnet
      └── Database
```

This helps create network segmentation and security controls.

---

# Part 12 — Azure VM Networking

An Azure VM doesn't simply "sit inside" a VNet by itself.

A simplified structure is:

```text
Azure VM
   │
   ▼
Network Interface (NIC)
   │
   ▼
Subnet
   │
   ▼
VNet
```

Example:

```text
VM01
 │
 └── NIC
      │
      └── Private IP: 10.0.1.4
             │
             ▼
        Web-Subnet
        10.0.1.0/24
             │
             ▼
        Company-VNet
        10.0.0.0/16
```

### Remember:

**NIC = Network Interface Card**

In Azure, a VM uses a **virtual network interface** to connect to the VNet.

---

# Part 13 — Private IP vs Public IP in Azure

Suppose:

```text
Azure VM
Private IP: 10.0.1.4
```

That private IP is used for communication inside the virtual network and connected networks.

If the VM needs direct internet-facing connectivity, a **public IP** may be associated with the relevant Azure resource, depending on the architecture.

Example:

```text
Internet
   │
   ▼
Public IP
   │
   ▼
Azure resource
   │
   ▼
Private IP
   │
   ▼
VM
```

Important:

> Don't assume every Azure VM needs a public IP.

In real enterprise environments, many servers are intentionally kept private.

---

# 🧪 Part 14 — Today's Practical Lab

You don't need to create an Azure VM today.

We can learn subnetting using Windows and a small calculation exercise first.

## Lab 1 — Check Your Current IP

Open CMD:

```cmd
ipconfig
```

Then:

```cmd
ipconfig /all
```

Find:

```text
IPv4 Address
Subnet Mask
Default Gateway
DNS Servers
```

Write them down.

Example:

```text
IPv4 Address : 192.168.1.10
Subnet Mask  : 255.255.255.0
Gateway      : 192.168.1.1
DNS          : 192.168.1.1
```

---

# Lab 2 — Convert `/24`

Take:

```text
192.168.10.0/24
```

Find:

**1. Subnet mask**

Answer:

```text
255.255.255.0
```

**2. Network address**

```text
192.168.10.0
```

**3. First usable**

```text
192.168.10.1
```

**4. Last usable**

```text
192.168.10.254
```

**5. Broadcast**

```text
192.168.10.255
```

---

# Lab 3 — Convert `/26`

Take:

```text
192.168.10.0/26
```

Remember:

```text
/26 → 64 total addresses
```

Therefore:

```text
Network:
192.168.10.0

First usable:
192.168.10.1

Last usable:
192.168.10.62

Broadcast:
192.168.10.63
```

Next subnet:

```text
192.168.10.64/26
```

---

# Lab 4 — Azure IP Planning Exercise

Imagine you're designing an Azure environment.

Requirement:

```text
Company needs:
Web servers
Application servers
Database servers
```

Create this design:

```text
VNet:
Company-VNet

Address Space:
10.0.0.0/16
```

Subnets:

```text
Web-Subnet
10.0.1.0/24

App-Subnet
10.0.2.0/24

DB-Subnet
10.0.3.0/24
```

Then draw:

```text
Company-VNet
10.0.0.0/16
       │
       ├── Web-Subnet
       │   10.0.1.0/24
       │
       ├── App-Subnet
       │   10.0.2.0/24
       │
       └── DB-Subnet
           10.0.3.0/24
```

This is **IP address planning**.

---

# ☁️ Part 15 — Connect This to AZ-900

You should now understand this hierarchy:

```text
Azure
 │
 └── Subscription
      │
      └── Resource Group
           │
           ├── VNet
           │    │
           │    ├── Subnet
           │    │    └── VM
           │    │
           │    ├── Subnet
           │    │    └── VM
           │    │
           │    └── Subnet
           │
           └── Other Resources
```

Don't memorize this as one giant structure.

Understand the relationship:

```text
VNet = overall Azure virtual network

Subnet = smaller network inside VNet

VM = compute resource connected to subnet through NIC
```

---

# 🔥 Part 16 — Real-World SysAdmin Scenario

Suppose a company tells you:

> "We have created an Azure VM, but another server cannot communicate with it."

As a junior SysAdmin, you should start thinking:

```text
VM running?
      ↓
NIC connected?
      ↓
Correct private IP?
      ↓
Correct subnet?
      ↓
VNet connectivity?
      ↓
Routing?
      ↓
Firewall / NSG?
      ↓
Correct port?
```

For example, application communication might require:

```text
App Server
10.0.2.10
      │
      │ TCP 443
      ▼
Web Server
10.0.1.10
```

You don't just ask:

> "Is the server ON?"

You start thinking about **network path + IP + subnet + port + security rules**.

That's the mindset we're building.

---

# 🧠 Part 17 — Easy Subnetting Formula

For IPv4:

```text
Total addresses = 2^(host bits)
```

Host bits:

```text
32 - CIDR
```

Therefore:

### `/24`

```text
32 - 24 = 8

2⁸ = 256
```

### `/25`

```text
32 - 25 = 7

2⁷ = 128
```

### `/26`

```text
32 - 26 = 6

2⁶ = 64
```

For traditional IPv4 subnet calculations:

```text
Usable hosts = Total addresses - 2
```

The `-2` accounts for the network and broadcast addresses.

---

# ⭐ Part 18 — What You Must Remember for AZ-900

Don't spend hours memorizing binary today.

Remember these:

### 1. VNet

```text
Virtual Network
```

Azure's virtual networking environment.

### 2. Subnet

Smaller network segment inside a VNet.

### 3. Address Space

Defines the IP range available to the VNet.

Example:

```text
10.0.0.0/16
```

### 4. Private IP

Used for internal communication.

Example:

```text
10.0.1.10
```

### 5. Public IP

Used when internet-facing connectivity is required.

### 6. NIC

Connects an Azure VM to a virtual network.

### 7. `/24`

```text
255.255.255.0
```

### 8. `/26`

```text
64 total addresses
62 traditional usable hosts
```

---

# 📝 Day 3 Quiz

Try answering without looking above.

### Q1

What is a subnet?

### Q2

What does `/24` represent?

### Q3

What is the subnet mask for `/24`?

### Q4

How many total IPv4 addresses are in a `/24`?

### Q5

How many traditional usable hosts are in `/24`?

### Q6

What is the broadcast address of:

```text
192.168.1.0/24
```

### Q7

What is an Azure VNet?

### Q8

Can a VNet contain multiple subnets?

### Q9

What is the relationship between VNet and subnet?

### Q10

What connects an Azure VM to a VNet?

### Q11

What is the difference between:

```text
10.0.0.0/16
```

and

```text
10.0.1.0/24
```

### Q12

Why might an organization separate Web, App and Database servers into different subnets?

---

# 📋 Day 3 Checklist

Before moving to Day 4, you should be able to explain:

* [ ] What is subnetting?
* [ ] What is a subnet?
* [ ] What is CIDR?
* [ ] What does `/24` mean?
* [ ] What does `/25` mean?
* [ ] What does `/26` mean?
* [ ] Network address
* [ ] Host address
* [ ] Broadcast address
* [ ] Total vs usable IP addresses
* [ ] Azure VNet
* [ ] Azure subnet
* [ ] VNet vs subnet
* [ ] VNet address space
* [ ] Azure VM NIC
* [ ] Private vs public IP
* [ ] Basic Azure IP planning

---

# 📁 Day 3 — Your Daily Documentation

Since you're maintaining the `AZ900-Certificate-Course` GitHub repository and posting your daily practice, use this structure today.

### GitHub folder

```text
Day-03-Subnetting-and-Azure-VNet/
```

### Folder structure

```text
Day-03-Subnetting-and-Azure-VNet/
│
├── README.md
│
├── Notes/
│   └── Day-03-Subnetting-and-VNet.md
│
├── Practical-Labs/
│   ├── Subnetting-Practice.md
│   └── Azure-VNet-IP-Planning.md
│
├── Commands/
│   └── Network-Commands.md
│
└── Screenshots/
```

### README title

```text
# Day 03 — Subnetting and Azure Virtual Network Basics
```

### LinkedIn headline

**AZ-900 Day 03: Subnetting & Azure VNet Basics — Network, Host, Subnet and IP Address Planning**

### Short social-media headline

**AZ-900 Day 03 🚀 | Subnetting + Azure VNet Basics | Hands-on IP Planning**

### Practical lab title

**Day 03 Practical Lab — Subnetting Practice & Azure VNet IP Address Planning**

### Screenshot names

```text
01-ipconfig-subnet-details.png
02-subnet-mask.png
03-subnetting-calculation.png
04-24-subnet-practice.png
05-26-subnet-practice.png
06-azure-vnet-concept.png
07-azure-vnet-ip-planning.png
```

### Evidence to upload

Capture your own work/results for:

```text
✅ ipconfig /all
✅ Your IPv4 + subnet mask
✅ /24 calculation
✅ /25 calculation
✅ /26 calculation
✅ Azure VNet + subnet design
✅ Notes/README
```

**Important:** For today's Azure exercise, a conceptual IP-planning document is enough. You don't need to create a paid Azure resource just to complete Day 3.

---

## 🎯 Day 3 Final Takeaway

If you remember only this, remember:

```text
VNet
 │
 ├── Subnet
 │    └── VM
 │         └── NIC
 │              └── Private IP
 │
 ├── Subnet
 │    └── VM
 │
 └── Subnet
```

**VNet = big virtual network**

**Subnet = smaller network inside VNet**

**NIC = network interface connecting a VM**

**IP = address used for communication**

**CIDR `/24`, `/25`, `/26` = defines the network size**

Tomorrow, the natural next step is **Day 4: DNS Deep Dive + Azure DNS basics**, where we'll go beyond simply knowing `nslookup` and understand how name resolution actually works in an enterprise/Azure environment.
