# 🚀 AZ-900 — Day 04

## DNS Deep Dive + Azure DNS Basics

Today we will understand **DNS properly from zero**.

You already used `nslookup` on Day 1 and Day 2. Today we'll understand **what actually happens behind the command** and then connect it to **Azure DNS**.

---

# 🎯 Day 4 Learning Objectives

By the end of today, you should understand:

* What DNS is
* Why DNS is required
* Domain name vs hostname
* FQDN
* DNS resolver
* DNS server
* DNS query
* DNS records
* A record
* AAAA record
* CNAME record
* MX record
* NS record
* TTL
* Recursive vs authoritative DNS
* Forward lookup
* Reverse lookup
* DNS caching
* `nslookup`
* `ipconfig /flushdns`
* `ipconfig /displaydns`
* Azure DNS
* Azure Public DNS
* Azure Private DNS
* Azure DNS Private Resolver — basic awareness
* Real-world enterprise DNS troubleshooting

---

# ⏱️ Your 2-Hour Schedule

| Time        | Activity                         |
| ----------- | -------------------------------- |
| 0–35 min    | DNS concepts                     |
| 35–55 min   | DNS records + resolution process |
| 55–100 min  | Windows practical lab            |
| 100–115 min | Azure DNS concepts               |
| 115–120 min | Quiz + revision                  |

---

# PART 1 — Why Do We Need DNS?

Let's start with something very simple.

Computers communicate using IP addresses.

For example:

```text
142.250.195.14
```

Imagine you had to remember an IP address for every website:

```text
Google       → 142.x.x.x
Microsoft    → 20.x.x.x
Amazon       → 18.x.x.x
GitHub       → 140.x.x.x
```

That's difficult.

Humans prefer names:

```text
google.com
microsoft.com
github.com
```

So DNS solves this problem.

# DNS = Domain Name System

The simplest definition:

> **DNS translates names into IP addresses.**

For example:

```text
google.com
     ↓
DNS
     ↓
142.x.x.x
```

Think of DNS as the **phonebook of the internet**.

---

# 🧠 Real-Life Example

Your phone has:

```text
Contact:
Rahul
```

But internally, the phone needs:

```text
+91XXXXXXXXXX
```

You don't normally remember the number.

Similarly:

```text
You type:

www.google.com
```

DNS finds the corresponding IP address.

---

# PART 2 — What Happens When You Type Google.com?

You open your browser:

```text
https://www.google.com
```

What happens?

Simplified:

```text
You
 │
 │ "What is the IP of google.com?"
 ▼
DNS Resolver
 │
 ▼
DNS Servers
 │
 ▼
IP address
 │
 ▼
Browser connects to Google
```

More practically:

```text
Browser
   │
   ▼
Operating System
   │
   ▼
DNS Cache
   │
   ├── Found? → Use cached answer
   │
   └── Not found
          │
          ▼
      DNS Resolver
          │
          ▼
      DNS infrastructure
          │
          ▼
      IP address
```

Then your browser can connect to the destination.

---

# PART 3 — Domain Name

Consider:

```text
google.com
```

This is a **domain name**.

A domain is a human-readable name used to identify an internet namespace.

Examples:

```text
microsoft.com
amazon.com
github.com
openai.com
```

---

# PART 4 — Subdomain

Suppose we have:

```text
example.com
```

We can create:

```text
www.example.com
mail.example.com
portal.example.com
vpn.example.com
```

Here:

```text
example.com
```

is the domain.

And:

```text
www
mail
portal
vpn
```

are subdomains/host labels under that domain.

---

# PART 5 — Hostname

A **hostname** identifies a particular host/device within a network or DNS namespace.

For example:

```text
server01.company.com
```

could represent:

```text
server01
```

as the hostname.

In your own Windows environment you might have:

```text
LAB-DC01
```

That's a hostname.

If it's registered in DNS as:

```text
LAB-DC01.lab.local
```

then that is a fully qualified DNS name.

---

# PART 6 — FQDN

You will hear this frequently in SysAdmin interviews.

## FQDN = Fully Qualified Domain Name

Example:

```text
server01.company.com
```

It identifies the complete DNS name.

Another example:

```text
dc01.lab.local
```

Break it down:

```text
dc01       → host
lab        → domain
local      → top-level/domain suffix
```

Conceptually:

```text
FQDN
 │
 ├── Host
 │
 └── Domain
```

---

# PART 7 — What Is a DNS Server?

A DNS server is a server/service that handles DNS queries.

You ask:

> "What IP address belongs to `example.com`?"

The DNS infrastructure provides the answer if it can resolve it.

Example:

```text
Client
192.168.1.20
   │
   │ DNS Query
   ▼
DNS Server
192.168.1.1
   │
   ▼
Answer
93.184.216.34
```

---

# PART 8 — DNS Resolver

This term is important.

A **DNS resolver** is responsible for obtaining DNS answers on behalf of a client.

Your computer may be configured to use:

```text
DNS Server:
192.168.1.1
```

Your router may then forward DNS requests to an upstream DNS service.

Conceptually:

```text
Your PC
   ↓
Local DNS / Router
   ↓
Recursive DNS Resolver
   ↓
DNS hierarchy
   ↓
Answer
```

Don't worry about memorizing every implementation detail yet.

---

# PART 9 — DNS Records

This is one of today's most important topics.

DNS doesn't just store "name → IP".

It stores different **types of records**.

Think of a DNS record as an entry in a database.

---

# ⭐ 1. A Record

A record maps a hostname to an **IPv4 address**.

Example:

```text
server01.example.com
        ↓
192.168.1.10
```

DNS:

```text
server01.example.com → A → 192.168.1.10
```

Remember:

> **A = IPv4**

---

# ⭐ 2. AAAA Record

AAAA maps a hostname to an **IPv6 address**.

Example:

```text
server01.example.com
        ↓
2001:db8::10
```

Remember:

```text
A     → IPv4
AAAA  → IPv6
```

---

# ⭐ 3. CNAME

CNAME = **Canonical Name**

It creates an alias for another DNS name.

Example:

```text
portal.company.com
       ↓
CNAME
       ↓
webserver01.company.com
```

So:

```text
portal.company.com
```

is an alias.

Think:

```text
Official name → Alias
```

---

# ⭐ 4. MX Record

MX = **Mail Exchange**

It tells DNS which mail servers handle email for a domain.

Example:

```text
company.com
     ↓
MX
     ↓
mail.company.com
```

When mail systems need to deliver email for `company.com`, MX records help identify the appropriate mail servers.

---

# ⭐ 5. NS Record

NS = **Name Server**

It identifies the authoritative DNS servers for a DNS zone/domain.

Example conceptually:

```text
example.com
     ↓
NS
     ↓
ns1.example-dns.com
ns2.example-dns.com
```

---

# ⭐ 6. TXT Record

TXT records can store text information associated with a DNS name.

They are commonly used for things such as:

* Domain verification
* Email security mechanisms
* SPF-related information
* Other service configuration

Example:

```text
example.com
     ↓
TXT
     ↓
"some verification/configuration text"
```

---

# ⭐ 7. PTR Record

PTR is associated with **reverse DNS**.

It maps:

```text
IP → hostname
```

instead of:

```text
hostname → IP
```

Normal DNS:

```text
server01.company.com
        ↓
192.168.1.10
```

Reverse DNS:

```text
192.168.1.10
        ↓
server01.company.com
```

---

# 🧠 Important DNS Record Table

| Record | Purpose                              |
| ------ | ------------------------------------ |
| A      | Hostname → IPv4                      |
| AAAA   | Hostname → IPv6                      |
| CNAME  | Alias → another hostname             |
| MX     | Mail server                          |
| NS     | Authoritative name server            |
| TXT    | Text/configuration/verification data |
| PTR    | Reverse DNS                          |

For AZ-900, **A, AAAA, CNAME, MX, NS and TXT** are especially useful concepts to recognize.

---

# PART 10 — Forward vs Reverse Lookup

## Forward Lookup

Name → IP

```text
server01.company.com
       ↓
192.168.1.10
```

This is the most common DNS operation you encounter.

---

## Reverse Lookup

IP → Name

```text
192.168.1.10
       ↓
server01.company.com
```

This typically uses a PTR record.

---

# PART 11 — DNS Port

You learned ports on Day 2.

DNS commonly uses:

```text
UDP 53
```

and DNS can also use:

```text
TCP 53
```

### Why both?

UDP is commonly used for ordinary DNS queries because it has lower overhead.

TCP can be used in situations such as larger DNS responses and certain DNS operations.

For AZ-900:

> **DNS = Port 53**

Remember that.

---

# PART 12 — DNS Cache

Suppose you visit:

```text
google.com
```

Your computer may remember the DNS result temporarily.

This is called **DNS caching**.

Why?

To avoid asking DNS servers repeatedly for the same information.

Example:

```text
First request:

PC → DNS → IP
```

Later:

```text
PC
 ↓
DNS Cache
 ↓
IP
```

This can make resolution faster and reduce unnecessary DNS traffic.

---

# PART 13 — TTL

DNS records have a **TTL**.

TTL = **Time To Live**

It determines how long a DNS response can generally be cached before it needs to be refreshed.

Example:

```text
google.com
TTL = 300 seconds
```

A resolver may cache that response for the specified period.

Think:

```text
DNS answer
   ↓
Cache
   ↓
TTL countdown
   ↓
Expires
   ↓
Fresh DNS lookup
```

---

# PART 14 — Practical Windows DNS Lab 🧪

Now let's actually test DNS.

Open **Command Prompt**.

---

## Lab 1 — Check Your DNS Configuration

Run:

```cmd
ipconfig /all
```

Find:

```text
DNS Servers
```

For example:

```text
DNS Servers . . . . . . . . . . : 192.168.1.1
```

Your actual address may be different.

### Understand:

```text
Your PC
   │
   └── DNS Server configured here
```

---

# Lab 2 — Use nslookup

Run:

```cmd
nslookup google.com
```

You may see something similar to:

```text
Server:  router
Address: 192.168.1.1

Non-authoritative answer:
Name:    google.com
Addresses: ...
```

Don't worry if your exact output is different.

Look for:

```text
Server
Address
Name
Addresses
```

---

# Lab 3 — Query Microsoft

```cmd
nslookup microsoft.com
```

Observe the returned IP address.

---

# Lab 4 — Query GitHub

```cmd
nslookup github.com
```

Again, observe the result.

The important lesson:

```text
github.com
    ↓
DNS
    ↓
IP address
```

---

# Lab 5 — Query a Specific DNS Server

You can explicitly tell `nslookup` which DNS server to use.

For example:

```cmd
nslookup google.com 8.8.8.8
```

Here:

```text
google.com
```

is the name you're asking about.

And:

```text
8.8.8.8
```

is the DNS server you're asking.

---

# Lab 6 — Query Cloudflare DNS

Try:

```cmd
nslookup google.com 1.1.1.1
```

Now you're asking a different DNS resolver.

Conceptually:

```text
Your PC
   │
   │ "Resolve google.com"
   ▼
1.1.1.1
   │
   ▼
IP address
```

---

# Lab 7 — Reverse Lookup

Try:

```cmd
nslookup 8.8.8.8
```

You're asking:

> "Does this IP have a DNS name?"

This is a reverse lookup.

---

# Lab 8 — View DNS Cache

Run:

```cmd
ipconfig /displaydns
```

Windows may show many cached entries.

You'll see information associated with names your computer has recently resolved.

---

# Lab 9 — Flush DNS Cache

Run:

```cmd
ipconfig /flushdns
```

You should receive a message indicating that the DNS Resolver Cache was successfully flushed.

### Why is this useful?

Suppose:

```text
Old DNS information
       ↓
Cached
       ↓
Website changed IP
       ↓
PC still has old information
```

Clearing the local cache can help eliminate stale cached DNS data as a troubleshooting factor.

---

# PART 15 — DNS Troubleshooting Scenario

Imagine a user says:

> "Internet is working, but I can't open `portal.company.com`."

Don't immediately reinstall anything.

Troubleshoot logically.

### Step 1 — Check IP configuration

```cmd
ipconfig /all
```

Check:

* IP
* subnet mask
* gateway
* DNS server

### Step 2 — Test internet connectivity

```cmd
ping 8.8.8.8
```

If this works, basic IP connectivity may be available.

### Step 3 — Test DNS

```cmd
nslookup portal.company.com
```

If resolution fails, investigate DNS.

### Step 4 — Test the actual service port

PowerShell:

```powershell
Test-NetConnection portal.company.com -Port 443
```

Now you're testing HTTPS connectivity.

---

# 🔥 Very Important Troubleshooting Logic

Suppose:

```text
ping 8.8.8.8
```

works.

But:

```text
ping google.com
```

fails.

One possible cause is:

```text
DNS problem
```

Because:

```text
8.8.8.8
```

is already an IP.

Whereas:

```text
google.com
```

requires name resolution.

This is an extremely useful SysAdmin troubleshooting concept.

---

# PART 16 — Azure DNS

Now let's connect DNS to Azure.

Azure provides DNS-related services.

At AZ-900 level, understand these major concepts:

### Azure DNS

A DNS hosting service for domains using Microsoft's Azure infrastructure.

### Azure Private DNS

Used for DNS name resolution within private Azure environments and connected networks.

### Azure DNS Private Resolver

Provides DNS resolution capabilities between Azure and external/on-premises DNS environments without requiring traditional DNS server VMs for the resolver function.

For now, don't go too deep into Private Resolver. We will revisit it later when we study Azure networking.

---

# PART 17 — Azure Public DNS

Suppose your company owns:

```text
company.com
```

And wants:

```text
www.company.com
```

to resolve to a public service.

Conceptually:

```text
Internet user
      │
      ▼
www.company.com
      │
      ▼
Public DNS
      │
      ▼
Public IP / Azure endpoint
      │
      ▼
Application
```

Azure DNS can host the DNS zone and records.

---

# PART 18 — Azure Private DNS

Now imagine an internal company application:

```text
database.internal.company
```

It should **not** be publicly resolvable.

You can use private DNS concepts.

Example:

```text
Azure VNet
10.0.0.0/16
       │
       ├── App VM
       │
       └── DB VM
             │
             └── private DNS name
```

The goal is internal name resolution.

For example:

```text
db01.internal
      ↓
10.0.2.10
```

Instead of users remembering:

```text
10.0.2.10
```

they can use a meaningful name.

---

# 🏢 Real-World Enterprise Example

Imagine your company has:

```text
Application Server
10.10.2.20

Database Server
10.10.3.20
```

Instead of configuring applications with:

```text
10.10.3.20
```

you could use a DNS name such as:

```text
database.company.internal
```

Then:

```text
Application
    │
    │ "database.company.internal"
    ▼
DNS
    │
    ▼
10.10.3.20
    │
    ▼
Database
```

If the database server's IP changes later, DNS can be updated without necessarily changing the application's configured hostname.

That's one reason DNS is so important in enterprise environments.

---

# PART 19 — DNS + Active Directory

This is particularly important for your **System Administrator path**.

Active Directory heavily depends on DNS.

A Windows domain such as:

```text
lab.local
```

uses DNS for locating domain services.

Your lab:

```text
LAB-DC01
    │
    └── lab.local
```

The domain controller provides important DNS functionality in a typical AD-integrated setup.

For example, domain clients need to locate services such as domain controllers.

Conceptually:

```text
Client
   │
   │ DNS query
   ▼
DNS
   │
   ▼
Domain Controller
   │
   ▼
Authentication / AD services
```

This is why **DNS problems can cause Active Directory login and domain-related problems**.

---

# 🧠 Day 4 — AZ-900 Exam Memory Sheet

Remember these:

```text
DNS
↓
Domain Name System
```

```text
A
↓
IPv4
```

```text
AAAA
↓
IPv6
```

```text
CNAME
↓
Alias
```

```text
MX
↓
Mail server
```

```text
NS
↓
Name server
```

```text
PTR
↓
Reverse DNS
```

```text
DNS
↓
Port 53
```

```text
Azure DNS
↓
DNS hosting
```

```text
Azure Private DNS
↓
Private name resolution
```

---

# 🧪 Day 4 Practical Assignment

Run these commands and save your outputs:

```cmd
ipconfig /all
```

```cmd
nslookup google.com
```

```cmd
nslookup microsoft.com
```

```cmd
nslookup github.com
```

```cmd
nslookup google.com 8.8.8.8
```

```cmd
nslookup google.com 1.1.1.1
```

```cmd
nslookup 8.8.8.8
```

```cmd
ipconfig /displaydns
```

```cmd
ipconfig /flushdns
```

Then:

```powershell
Test-NetConnection google.com -Port 443
```

### Don't just run them.

For every command, write:

```text
Command:
Purpose:
What I observed:
What it proves:
```

This will turn your GitHub repository into a **real hands-on learning portfolio**, rather than just copied notes.

---

# 📝 Day 4 Quiz

Try answering before checking your notes.

### Q1

What does DNS stand for?

### Q2

Why do we need DNS?

### Q3

What does an A record contain?

### Q4

What is an AAAA record?

### Q5

What is a CNAME?

### Q6

What is an MX record used for?

### Q7

What is a PTR record?

### Q8

What is forward DNS lookup?

### Q9

What is reverse DNS lookup?

### Q10

What is DNS caching?

### Q11

What does TTL mean?

### Q12

Which port is commonly associated with DNS?

### Q13

What command displays the Windows DNS cache?

### Q14

What command clears the Windows DNS cache?

### Q15

Why is DNS especially important for Active Directory?

---

# 📁 Day 4 Documentation Structure

Your daily portfolio should continue with today's exact naming.

### GitHub folder

```text
Day-04-DNS-and-Azure-DNS/
```

### Structure

```text
Day-04-DNS-and-Azure-DNS/
│
├── README.md
│
├── Notes/
│   └── Day-04-DNS-Fundamentals.md
│
├── Practical-Labs/
│   └── Windows-DNS-Troubleshooting.md
│
├── Commands/
│   └── DNS-Commands.md
│
└── Screenshots/
```

### README title

```text
# Day 04 — DNS Fundamentals and Azure DNS Basics
```

### LinkedIn headline

**AZ-900 Day 04: DNS Deep Dive & Azure DNS — DNS Records, Name Resolution, Caching and Troubleshooting**

### Short social headline

**AZ-900 Day 04 🚀 | DNS Fundamentals + Azure DNS | Hands-on DNS Troubleshooting**

### Practical lab title

**Day 04 Practical Lab — Windows DNS Resolution & Troubleshooting**

### Screenshot names

```text
01-ipconfig-all-dns.png
02-nslookup-google.png
03-nslookup-microsoft.png
04-nslookup-github.png
05-nslookup-google-8.8.8.8.png
06-nslookup-google-1.1.1.1.png
07-reverse-dns-lookup.png
08-display-dns-cache.png
09-flush-dns-cache.png
10-test-https-443.png
```

### Evidence to upload

```text
✅ ipconfig /all
✅ DNS server information
✅ nslookup results
✅ Public DNS resolver test
✅ Reverse lookup
✅ DNS cache
✅ Flush DNS result
✅ Test-NetConnection 443
✅ Your explanations/observations
```

---

# 🎯 Day 4 Final Mental Model

Keep this picture in your mind:

```text
                    USER
                      │
                      │
               google.com
                      │
                      ▼
                    DNS
                      │
             "What is its IP?"
                      │
                      ▼
                DNS Resolver
                      │
                      ▼
                 IP Address
                      │
                      ▼
                TCP/HTTPS
                  Port 443
                      │
                      ▼
                  SERVER
```

And inside Azure:

```text
                    Azure
                      │
                  Company
                      │
                 Public DNS
                      │
                company.com
                      │
               ┌──────┴──────┐
               │             │
           Public app    Private app
               │             │
            Public DNS    Private DNS
                             │
                            VNet
                             │
                          Subnet
                             │
                             VM
```

**Day 3 taught you where resources live in the network.
Day 4 teaches you how humans and applications find those resources by name.**

Next, **Day 5** should build on this with **Azure Regions, Availability Zones, Region Pairs, Datacenters, and Azure's global infrastructure**—a very important AZ-900 area.
