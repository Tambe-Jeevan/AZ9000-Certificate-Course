# 🚀 AZ-900 — Day 1

## IT + Cloud Foundations: From Absolute Zero

Today we are **not going deep into Azure yet**. First, you need to understand the basic building blocks that Azure is built on.

By the end of today, you should be able to explain:

> **Computer → Network → Server → Virtual Machine → IP → DNS → Data Center → Cloud → Azure**

And you'll also get a first understanding of:

> **Azure Subscription → Resource Group → Azure Resource**

---

# 🕑 Today's 2-Hour Plan

| Time        | Activity                              |
| ----------- | ------------------------------------- |
| 0–15 min    | What is a computer?                   |
| 15–30 min   | Client, server & data center          |
| 30–45 min   | Networking + IP address               |
| 45–60 min   | DNS, DHCP, router, switch             |
| 60–75 min   | Virtualization & Virtual Machines     |
| 75–90 min   | What is Cloud Computing?              |
| 90–105 min  | Azure + Subscription + Resource Group |
| 105–120 min | Practical lab + quiz                  |

---

# 1. First: What is a Computer?

Let's start extremely basic.

A computer is a machine that:

**Takes input → processes it → stores data → produces output**

For example, when you open Chrome and visit YouTube:

```text
You click YouTube
       ↓
Computer processes your request
       ↓
Internet connection
       ↓
YouTube server
       ↓
Video data comes back
       ↓
Chrome displays video
```

A computer has several important components.

### CPU

**CPU = Central Processing Unit**

It performs calculations and executes instructions.

Think:

> CPU = Brain of the computer

Example:

When you open Chrome, CPU executes the instructions required to run Chrome.

---

### RAM

**RAM = Random Access Memory**

Temporary working memory.

Example:

You open:

* Chrome
* VS Code
* Teams
* Spotify

All these applications use RAM while running.

If RAM is insufficient, your computer may become slow.

Think:

> RAM = Work table

---

### Storage

HDD/SSD stores data permanently.

Examples:

```text
Windows
Applications
Documents
Photos
Videos
Projects
```

Think:

> SSD/HDD = Cupboard

---

### Operating System

The OS manages the computer hardware and software.

Examples:

* Windows
* Linux
* macOS

For your future SysAdmin/Azure career, **Windows and Linux** are particularly important.

---

# 2. Client vs Server

This is **very important**.

## Client

A client is a device/application that **requests a service**.

Examples:

* Your laptop
* Your phone
* Web browser

## Server

A server is a computer/system that **provides a service**.

For example:

```text
Your Laptop
(Client)
    │
    │ Request
    ↓
Web Server
(Server)
    │
    │ Response
    ↓
Your Laptop
```

### Real-world example

You open:

`google.com`

Your browser is acting as the **client**.

Google's systems are acting as **servers**.

The client asks:

> "Give me Google's webpage."

The server responds:

> "Here is the webpage."

---

# 3. What is a Server?

Don't think a server is a completely different type of computer.

A server can basically be a computer configured to provide services to other computers.

For example:

### File Server

Stores files.

```text
Employee PC
     ↓
 File Server
     ↓
Company Documents
```

### Web Server

Hosts websites.

```text
Browser
   ↓
Web Server
   ↓
Website
```

### Database Server

Stores application data.

```text
Application
     ↓
Database Server
     ↓
Customer Data
```

### Domain Controller

In a traditional Windows enterprise environment, a Domain Controller provides **Active Directory Domain Services**.

This should sound familiar from your IT Support experience.

---

# 4. What is a Data Center?

A **data center** is a facility containing IT infrastructure such as:

* Servers
* Storage
* Networking equipment
* Power systems
* Cooling
* Security systems

Imagine a huge building:

```text
              DATA CENTER

 ┌─────────────────────────────────┐
 │ Server  Server  Server  Server  │
 │                                 │
 │ Server  Server  Server  Server  │
 │                                 │
 │ Storage Storage Storage         │
 │                                 │
 │ Switches / Routers / Firewalls  │
 │                                 │
 │ Power + Cooling + Security      │
 └─────────────────────────────────┘
```

Companies can build their own data centers.

Or they can use a cloud provider such as Azure.

---

# 5. What is Networking?

Networking simply means:

> **Connecting computers/devices so they can communicate with each other.**

Example:

```text
Laptop ───┐
          │
Desktop ──┼── Switch ── Router ── Internet
          │
Printer ──┘
```

Your laptop needs a way to identify and communicate with other devices.

That's where an **IP address** comes in.

---

# 6. What is an IP Address?

### IP = Internet Protocol

An IP address is an address used to identify a device/interface on a network.

Think about your house.

Your house has an address:

> House No. 25, XYZ Road

A network device has an IP address:

> `192.168.1.10`

So conceptually:

```text
House address
     ↓
Physical location

IP address
     ↓
Network location
```

---

# 7. IPv4

You'll commonly see IPv4 addresses like:

```text
192.168.1.10
```

An IPv4 address consists of **four numbers separated by dots**.

Example:

```text
192 . 168 . 1 . 10
```

Each section can range from:

```text
0 – 255
```

---

# 8. Private vs Public IP

This is extremely important for Azure.

## Private IP

Used inside a private network.

Example:

```text
192.168.1.10
192.168.1.20
10.0.0.5
```

Your home/office devices commonly use private IP addresses.

Example:

```text
Laptop → 192.168.1.10
Printer → 192.168.1.20
Router → 192.168.1.1
```

---

## Public IP

Used for communication over the public Internet.

Conceptually:

```text
Your laptop
    ↓
Private IP
    ↓
Router
    ↓
Public IP
    ↓
Internet
```

In Azure, you'll frequently encounter **public IP** and **private IP** when working with virtual machines and networking.

---

# 9. Find Your Computer's IP

This is your **first practical lab**.

Open:

**Command Prompt**

Run:

```cmd
ipconfig
```

You'll see something similar to:

```text
IPv4 Address. . . . . . : 192.168.1.10
Subnet Mask . . . . . . : 255.255.255.0
Default Gateway . . . . : 192.168.1.1
```

Don't worry if your numbers are different.

### Understand these three:

**IPv4 Address**

Your computer's address on that network.

**Subnet Mask**

Defines which portion represents the network and which portion represents the host.

**Default Gateway**

Usually your router; it provides the path to other networks/Internet.

---

# 10. What is a Router?

A router connects **different networks**.

Example:

```text
Your Home Network
192.168.1.0/24
       ↓
    Router
       ↓
    Internet
```

Think:

> **Router = Traffic director between networks**

---

# 11. What is a Switch?

A switch connects devices within a network/LAN.

Example:

```text
PC ─────┐
Laptop ─┼── Switch
Printer ┘
```

Think:

> **Switch = Connects devices inside a network**

---

# 12. What is DNS?

This is one of the **most important concepts** you'll use throughout your Azure career.

### DNS = Domain Name System

Computers communicate using IP addresses.

Humans prefer names.

You don't want to remember:

```text
142.250.xxx.xxx
```

You prefer:

```text
google.com
```

DNS translates a domain name into an IP address.

Conceptually:

```text
You type:

google.com
     ↓
    DNS
     ↓
IP address
     ↓
Google server
```

Think:

> **DNS = Internet phonebook**

---

# 13. Practical DNS Lab

Open Command Prompt.

Run:

```cmd
nslookup google.com
```

You'll get information about the DNS server and the IP address returned for the domain.

You can also try:

```cmd
nslookup microsoft.com
```

This is a useful command to remember for your SysAdmin career.

---

# 14. What is DHCP?

### DHCP = Dynamic Host Configuration Protocol

Instead of manually assigning an IP address to every computer, DHCP can automatically provide network configuration.

When your laptop connects to Wi-Fi:

```text
Laptop
   ↓
"Give me network configuration"
   ↓
DHCP
   ↓
IP address
Subnet mask
Gateway
DNS
```

So:

> **DHCP automatically provides network configuration to clients.**

---

# 15. DNS vs DHCP

Don't confuse these.

| Technology | Main job                                  |
| ---------- | ----------------------------------------- |
| **DNS**    | Name → IP                                 |
| **DHCP**   | Automatically gives network configuration |
| **Router** | Connects networks                         |
| **Switch** | Connects devices within a network         |

Easy memory:

**DNS = Name**

**DHCP = IP configuration**

---

# 16. What is Virtualization?

Now we're getting closer to Azure.

Imagine you have one physical server:

```text
Physical Server
CPU: 16 cores
RAM: 64 GB
Storage: 1 TB
```

Instead of using it as one server, virtualization allows you to create multiple **Virtual Machines (VMs)**.

```text
             Physical Server
          CPU: 16 cores / RAM 64GB
                    │
              Hypervisor
        ┌───────────┼───────────┐
        ↓           ↓           ↓
       VM1         VM2         VM3
    Windows      Linux       Windows
```

Each VM behaves like an independent computer.

---

# 17. What is a Virtual Machine?

A VM is basically a **software-based computer**.

It can have:

* Virtual CPU
* Virtual RAM
* Virtual disk
* Virtual network card
* Operating system

Example:

```text
Physical Computer
       ↓
Virtualization
       ↓
┌──────────────┐
│ Windows VM   │
├──────────────┤
│ Linux VM     │
├──────────────┤
│ Windows VM   │
└──────────────┘
```

This concept is **fundamental to cloud computing**.

---

# 18. What is a Hypervisor?

A **hypervisor** is software that creates and manages virtual machines.

Examples include:

* VMware ESXi
* Microsoft Hyper-V
* KVM

Conceptually:

```text
Physical Hardware
       ↓
   Hypervisor
       ↓
 ┌─────┼─────┐
 VM1   VM2   VM3
```

You don't need deep hypervisor administration for AZ-900.

You need to understand **what virtualization is and why cloud providers use it**.

---

# 19. From Virtualization to Cloud

Now connect everything.

Traditionally, a company might buy:

```text
Physical Server
      ↓
Install Windows Server
      ↓
Configure networking
      ↓
Install application
      ↓
Maintain hardware
```

This requires:

* Hardware purchase
* Data center
* Power
* Cooling
* Networking
* Maintenance
* Hardware replacement

Cloud changes the model.

Instead:

```text
Company
   ↓
Azure
   ↓
Request Virtual Machine
   ↓
Azure provides VM
```

You don't physically purchase the server.

---

# 20. What is Cloud Computing?

Simple definition:

> **Cloud computing is the delivery of computing resources over the Internet on demand.**

Resources can include:

* Compute
* Storage
* Networking
* Databases
* Applications
* Security services

Instead of owning everything yourself, you consume services from a cloud provider.

---

# 21. What is Microsoft Azure?

**Microsoft Azure** is Microsoft's cloud computing platform.

Think:

```text
Microsoft
    ↓
Azure
    ↓
Data Centers
    ↓
Cloud Services
```

Azure provides services such as:

* Virtual Machines
* Storage
* Databases
* Networking
* Identity
* Security
* Monitoring
* AI
* Application hosting

You don't need to memorize all services now.

We'll learn them gradually.

---

# 22. Very Important: Azure Region

Before understanding subscriptions, understand **where Azure resources physically run**.

An Azure **Region** is a geographical area containing Azure datacenters.

For example:

```text
Azure
 │
 ├── India
 │    ├── Region
 │    └── Region
 │
 ├── US
 │    ├── Region
 │    └── Region
 │
 └── Europe
      ├── Region
      └── Region
```

When creating an Azure resource, you will often need to choose a **region**.

Why?

Because location affects:

* Latency
* Availability
* Compliance
* Data residency
* Cost

---

# 23. What is an Azure Resource?

This word is extremely important.

A **resource** is something you create/use in Azure.

Examples:

```text
Virtual Machine
Storage Account
Virtual Network
Public IP
Database
```

Each is an Azure resource.

For example:

```text
Create VM
   ↓
Azure creates a VM resource
```

---

# 24. What is an Azure Resource Group?

This is one of the concepts you specifically asked about.

A **Resource Group (RG)** is a logical container for related Azure resources.

Think of it like a **project folder**.

Example:

```text
Resource Group: ECommerce-Production

        ├── Web VM
        ├── Database
        ├── Storage Account
        ├── Virtual Network
        └── Public IP
```

Instead of having resources scattered everywhere, you organize related resources into a resource group.

### Important:

A resource group is **not a physical server**.

It is a logical management container.

---

# 25. Real-World Resource Group Example

Imagine you work for a company:

**ABC E-Commerce**

They have an online shopping application.

You might organize it like:

```text
Azure Subscription
       │
       ├── RG: ECommerce-Production
       │       ├── Web VM
       │       ├── Database
       │       ├── Storage
       │       └── Network
       │
       └── RG: ECommerce-Testing
               ├── Test VM
               ├── Test Database
               └── Test Storage
```

This makes management easier.

---

# 26. What is an Azure Subscription?

This is another **very important AZ-900 concept**.

An Azure Subscription provides a boundary for:

* Billing
* Resource organization
* Access control
* Resource usage

Think of it as an **Azure account/billing boundary**, not simply your Microsoft login.

For example:

```text
Company
   │
   ├── Production Subscription
   │
   ├── Development Subscription
   │
   └── Testing Subscription
```

Each subscription can contain many resource groups.

---

# 27. Subscription vs Resource Group

Remember this:

```text
Subscription
      │
      ├── Resource Group
      │       ├── VM
      │       ├── Storage
      │       └── Network
      │
      └── Resource Group
              ├── VM
              └── Database
```

### Subscription

Think:

> **Billing + management boundary**

### Resource Group

Think:

> **Logical container for related resources**

### Resource

Think:

> **Actual Azure service/object**

---

# 28. Azure Hierarchy — Memorize This

For AZ-900, remember:

```text
Management Group
       ↓
Subscription
       ↓
Resource Group
       ↓
Resource
```

Example:

```text
Company
   ↓
Management Group
   ↓
Production Subscription
   ↓
ECommerce Resource Group
   ↓
Virtual Machine
```

Don't worry about Management Groups deeply today. We'll study them later.

---

# 🧪 DAY 1 PRACTICAL LAB

Now let's make today's theory practical.

## Lab 1 — Check your computer

Open CMD:

```cmd
hostname
```

Then:

```cmd
ipconfig
```

Then:

```cmd
ipconfig /all
```

Identify:

* Computer name
* IPv4 address
* Subnet mask
* Default gateway
* DNS server

---

## Lab 2 — Test connectivity

Run:

```cmd
ping 127.0.0.1
```

This is the **loopback address**.

Then:

```cmd
ping google.com
```

You're testing connectivity and name resolution.

---

## Lab 3 — Test DNS

Run:

```cmd
nslookup google.com
```

Then:

```cmd
nslookup microsoft.com
```

Observe:

**Which DNS server responded?**

**Which IP addresses were returned?**

---

## Lab 4 — Trace the path

Run:

```cmd
tracert google.com
```

This shows the network hops between your computer and the destination.

Don't worry if some hops show `*`.

We'll study `tracert` properly during networking fundamentals.

---

# ☁️ Optional Azure Practical

If you already have an Azure account, **don't start creating expensive resources randomly**.

For today, simply open the Azure Portal and identify:

```text
Home
Subscriptions
Resource Groups
Virtual Machines
Storage Accounts
Virtual Networks
```

**Do not create a VM today just for practice.**

VMs can generate charges depending on your account/configuration.

We'll do controlled hands-on labs when we reach Azure Compute.

---

# 🧠 DAY 1 — One Big Picture

You should now understand this:

```text
                    INTERNET
                       │
                    ROUTER
                       │
                 ┌─────┴─────┐
                 │           │
              Laptop      Desktop
                 │
             IP Address
                 │
             DNS Lookup
                 │
              Website
                 │
              Server
                 │
            Data Center
                 │
           Virtualization
                 │
            Virtual Machine
                 │
                AZURE
                 │
           Subscription
                 │
           Resource Group
                 │
              Resource
```

This is the foundation for everything we'll study later.

---

# 🎯 AZ-900 Exam Points From Day 1

You should be able to answer:

### Q1. What is cloud computing?

**Answer:** Delivery of computing resources/services over the Internet on demand.

### Q2. What is an IP address?

**Answer:** An address used to identify a device/network interface for network communication.

### Q3. What does DNS do?

**Answer:** Resolves domain names to IP addresses.

### Q4. What does DHCP do?

**Answer:** Automatically provides network configuration such as IP address, subnet mask, gateway and DNS information.

### Q5. What is virtualization?

**Answer:** Creating virtual computing environments such as VMs on physical hardware.

### Q6. What is an Azure resource?

**Answer:** An individual Azure service/object, such as a VM, storage account or virtual network.

### Q7. What is a Resource Group?

**Answer:** A logical container used to organize and manage related Azure resources.

### Q8. What is an Azure Subscription?

**Answer:** A boundary used for Azure resource management, access and billing.

---

# 📝 Day 1 Quiz

Try answering **without looking above**.

**1.** What is the difference between a client and a server?

**2.** What is the purpose of an IP address?

**3.** What does DNS do?

**4.** What is the difference between DNS and DHCP?

**5.** What is a router?

**6.** What is a switch?

**7.** What is virtualization?

**8.** What is a virtual machine?

**9.** What is an Azure resource?

**10.** What is the difference between an Azure Subscription and Resource Group?

**11.** Give three examples of Azure resources.

**12.** Why do companies use cloud computing instead of buying every physical server themselves?

---

# ✅ Day 1 Completion Checklist

Before moving to Day 2, make sure you can explain these **in your own words**:

* [ ] Computer
* [ ] CPU
* [ ] RAM
* [ ] Storage
* [ ] Operating System
* [ ] Client
* [ ] Server
* [ ] Data Center
* [ ] Network
* [ ] IP Address
* [ ] Private IP
* [ ] Public IP
* [ ] Router
* [ ] Switch
* [ ] DNS
* [ ] DHCP
* [ ] Virtualization
* [ ] Virtual Machine
* [ ] Hypervisor
* [ ] Cloud Computing
* [ ] Azure
* [ ] Azure Region
* [ ] Azure Resource
* [ ] Azure Resource Group
* [ ] Azure Subscription

**Most important:** Don't try to memorize all of this today. Run the CMD labs, understand the relationships, and then answer the 12 questions. **Day 2 will build directly on this foundation** and we'll go deeper into **networking, IP addresses, subnet masks, ports, TCP/UDP, HTTP/HTTPS and how a real user's request travels from their PC to an Azure server.**
