## 🚀 AZ-900 — Day 7

**On-Premises Infrastructure & Designing a Small Company IT Environment**

Welcome to **Day 7** of your AZ-900 journey! Over the past six days, you built a complete foundation: you understand how computers work, what servers and hypervisors do, how local networking works, and how traffic moves through routers, switches, and firewalls.

Today, we bring all these pieces together. We will look at **On-Premises Infrastructure**—how traditional companies build and manage their IT environments locally—and design a small company network from scratch. Understanding this traditional setup is critical because it highlights *why* businesses eventually move to the cloud.

---

### 📂 GitHub Repository Folder Name & Social Media Ready Tag

* **Folder Name:** `Day-07-On-Premises-Infrastructure-Design`
* **LinkedIn Post Headline Suggestion:** `Day 7 of my AZ-900 Cloud Journey: Designing a Traditional On-Premises IT Environment 🏢`

---

### 🕑 Today's 2-Hour Plan

| Time | Activity |
| --- | --- |
| 0–20 min | What is On-Premises Infrastructure? (The physical reality) |
| 20–40 min | The Anatomy of a Server Room (Power, Cooling, Racks, Hardware) |
| 40–60 min | The Hidden Challenges & Costs of On-Premises IT |
| 60–80 min | Real-World Scenario: Designing an IT network for a 50-person company |
| 80–105 min | Practical Lab: Creating a technical blueprint for a small office network |
| 105–120 min | AZ-900 Exam Points, Interview Questions & Quiz |

---

### 1. What is On-Premises Infrastructure?

**On-Premises (On-Prem)** refers to an IT deployment model where all the hardware, software, networking equipment, and data storage are physically located **inside the organization's own building or office space**.

* Instead of renting resources over the internet from Microsoft or Google, the company owns everything outright.
* **Real-World Analogy:** Buying and building your own house from scratch, buying your own water tank, generator, and security system. You have total control, but if something breaks in the middle of the night, *you* have to fix it.

---

### 2. The Anatomy of an On-Premises Server Room

To host an on-premises IT environment, a company needs more than just a computer. A typical on-prem server room includes:

1. **Server Racks:** Metal cabinets where physical server hardware, switches, and patch panels are stacked vertically.
2. **Cooling Systems:** Specialized air conditioning units running 24/7 because servers generate massive amounts of heat. If the AC fails, servers overheat and shut down automatically to prevent hardware damage.
3. **Uninterruptible Power Supply (UPS) & Generators:** Large battery backups that keep servers running during a power outage until a backup generator kicks in.
4. **Networking Gear:** Core switches, enterprise routers, and hardware firewalls managing internal and external traffic.

---

### 3. The Hidden Challenges of On-Premises IT

Why are companies migrating away from on-prem? Consider the everyday friction points:

* **Heavy Upfront Cost (CapEx):** Buying physical servers, server racks, network gear, and software licenses requires a massive cash layout before the business even opens its doors.
* **Long Deployment Times:** If a company needs a new server to launch an app, ordering the hardware, shipping it, racking it, cabling it, and installing the OS can take **weeks or months**.
* **Maintenance Burden:** Hardware fails. Hard drives crash, power supplies blow out, and fans stop working. IT staff must spend hours troubleshooting physical parts instead of building business features.
* **Scaling Limitations:** If your business suddenly grows from 50 to 500 employees, your physical server room runs out of physical floor space, power capacity, and cooling capacity.

---

### 4. Real-World Scenario: Designing an IT Environment for "ABC Logistics"

Imagine you are hired as the IT Administrator for **ABC Logistics**, a growing shipping company with **50 employees**. They need a local office IT environment that includes:

* Employee workstations (Laptops/Desktops)
* Internal file sharing for documents
* Employee user accounts & security policies (Active Directory Domain Controller)
* A secure connection to the outside internet

#### How an On-Prem Architecture Looks:

```text
                  [ THE INTERNET ]
                         │
                 (ISP Fiber Line)
                         │
                         ▼
             [ Hardware Firewall ]  <--- (Blocks hackers/attacks)
                         │
                         ▼
             [ Core Enterprise Router ] <--- (Directs traffic)
                         │
                         ▼
            [ 24-Port Managed Switch ] <--- (Connects office devices)
             /           │           \
            /            │            \
    [ Employee ]    [ Employee ]    [ Physical Server Rack ]
    PC Workstation  PC Workstation      │
                                   ┌────┴────┐
                                   ▼         ▼
                              Domain      File & DB
                            Controller     Server

```

---

### 🛠️ 5. Practical Lab: Blueprinting Your On-Premises Office Infrastructure

Let's practice architecture design by mapping out an on-prem environment in your notes.

#### Lab Steps:

1. Open a notepad or document on your computer.
2. Create a section titled **"ABC Logistics On-Prem Architecture Design"**.
3. Write down and answer these four design questions:
* **Hardware Inventory:** What physical hardware do we need to buy upfront? (Hint: 1 Rack, 1 Firewall, 1 Switch, 1 Physical Server running a Hypervisor, 50 Client PCs).
* **Networking:** How will the server connect to the client PCs? (Hint: Via Ethernet cables plugged into the switch, assigned private IPs via DHCP).
* **Security:** Where should the firewall be placed in the connection chain? (Hint: Right between the ISP router and the internal switch).
* **Risk Assessment:** What happens if the local office experiences a major power grid failure and the backup generator fails? (Hint: All services go offline until power is restored).



---

### 🎯 AZ-900 Exam & Foundation Points From Day 7

* **Q1. What is On-Premises infrastructure?**
* *Answer:* A deployment model where all computing hardware, software, and data storage are physically hosted and managed within an organization's own facility.


* **Q2. What financial model is typically associated with traditional on-premises setups?**
* *Answer:* **CapEx (Capital Expenditure)**, because companies must invest heavily in physical hardware upfront before using it.


* **Q3. What is a major limitation of physical on-premises infrastructure when a company experiences rapid growth?**
* *Answer:* Physical constraints such as lack of server room space, power limits, cooling capacity, and long lead times required to purchase and install new hardware.



---

### 📝 Day 7 Quiz

*Try answering these out loud or in your notes:*

1. In an on-premises environment, who is physically responsible for replacing a broken hard drive or power supply?
2. Why is specialized air conditioning critical in a traditional corporate server room?
3. What does CapEx stand for, and why is it a challenge for startup companies?
4. If an on-premises company runs out of server rack space, what must they do to expand their computing power?

---

### 📝 LinkedIn & Social Media Post Template (Day 7)

> **🚀 Day 7 of my AZ-900 & Cloud Journey!**
> Today, I wrapped up Phase 1 (IT & Cloud Foundations) by studying **On-Premises Infrastructure** and designing a traditional enterprise server room and office network from scratch.
> **What I learned & practiced today:**
> 🏢 **On-Premises Architecture:** Understanding how physical hardware, server racks, cooling, and power systems operate inside a corporate building.
> 💰 **The CapEx Dilemma:** Why heavy upfront hardware costs and long procurement cycles slow down traditional businesses.
> 🗺️ **Network Design:** Mapped out an enterprise environment including firewalls, routers, switches, domain controllers, and client workstations for a 50-person company.
> Understanding the constraints of on-prem infrastructure makes it crystal clear *why* modern enterprises are aggressively migrating to cloud platforms like Microsoft Azure!
> I am documenting my entire 0-to-hero journey. Check out my GitHub repo for daily notes and labs:
> 👉 **GitHub Repo:** `AZ900-Certificate-Course` (`Day-07-On-Premises-Infrastructure-Design`)
> #MicrosoftAzure #AZ900 #CloudComputing #SysAdmin #EnterpriseArchitecture #LearningEveryday #DevOpsJourney

---

*Fantastic job completing Phase 1 of your journey! You now have a rock-solid grasp of IT fundamentals. Let me know when you are ready for **Day 8: What is Cloud Computing? (Transitioning from On-Prem to Azure)**!*