## 🚀 AZ-900 — Day 3

**Cloud Deployment Models (Public, Private, Hybrid) & Service Models (IaaS, PaaS, SaaS)**

Yesterday, you mastered how network traffic and ports work. Today, we move into the core architectural models of cloud computing. By the end of today, you will know how clouds are deployed (Public vs. Private vs. Hybrid), what levels of control you get (IaaS vs. PaaS vs. SaaS), and who is responsible for security (The Shared Responsibility Model).

---

### 📂 GitHub Repository Folder Name

* **Folder Name:** `Day-03-Cloud-Deployment-Models-and-Services`

---

### 🕑 Today's 2-Hour Plan

| Time | Activity |
| --- | --- |
| 0–20 min | Cloud Deployment Models: Public, Private, Hybrid, Multi-Cloud |
| 20–45 min | Cloud Service Models: IaaS, PaaS, SaaS (The Apartment Analogy) |
| 45–60 min | The Shared Responsibility Model (Who secures what?) |
| 60–90 min | Real-world mapping (How companies choose models) |
| 90–105 min | Practical Exploration: Exploring Azure pricing tiers & service models via Portal |
| 105–120 min | Quiz & LinkedIn Post Update |

---

### 1. Cloud Deployment Models

A deployment model defines *where* your data is stored and *who* manages the physical infrastructure. There are three main types:

#### A. Public Cloud

* **Definition:** Computing resources are owned and operated by a third-party provider (like Microsoft Azure, AWS, or Google Cloud) and delivered over the public internet. Multiple companies share the same physical hardware (called multi-tenancy), but your data is logically isolated and secure.
* **Analogy:** **Public Bus / Public Park.** Anyone can use it, you don't maintain the vehicle, and it's cost-effective.

#### B. Private Cloud

* **Definition:** Cloud infrastructure used exclusively by a single business or organization. It can be physically located at your company’s on-premises data center, or hosted by a third party dedicated solely to you.
* **Analogy:** **Your Personal Car.** Only you drive it, you customize it completely, but you pay for all maintenance and insurance yourself.
* **Use Case:** Banks, government agencies, or healthcare companies with strict regulatory compliance that cannot share hardware with others.

#### C. Hybrid Cloud

* **Definition:** A combination of Public and Private clouds that allows data and apps to be shared between them.
* **Analogy:** **Owning a House + Using a Public Bank.** You keep your cash and gold in a private locker at home (Private), but you use public roads and cloud apps to move around and communicate (Public).
* **Use Case:** A retail company keeps customer payment databases secure on-premises (Private), but hosts their temporary festival shopping website on Azure (Public) to handle sudden traffic spikes.

*(Note: There is also **Multi-Cloud**, which means using multiple public cloud providers simultaneously, like using both Azure and AWS).*

---

### 2. Cloud Service Models (IaaS, PaaS, SaaS)

When you rent computing resources, how much control do you want over the stack? This is broken down into three categories. Think of it like renting accommodation:

#### A. IaaS (Infrastructure as a Service)

* **Definition:** You rent the raw underlying hardware—virtual machines, storage, and networks—but you must install the operating system, security patches, and applications yourself.
* **Analogy:** **Renting an Empty Apartment.** The landlord builds the structure and provides water/electricity, but you must bring your own furniture, paint the walls, and clean the floors.
* **Azure Examples:** Azure Virtual Machines, Azure Virtual Networks.

#### B. PaaS (Platform as a Service)

* **Definition:** The cloud provider manages the operating system, servers, and storage. You only focus entirely on writing and deploying your application code.
* **Analogy:** **Renting a Fully Furnished Hotel Room.** The bed, TV, AC, and plumbing are all set up by the hotel. You just walk in with your suitcase and start working.
* **Azure Examples:** Azure App Service, Azure SQL Database.

#### C. SaaS (Software as a Service)

* **Definition:** A fully developed software application delivered over the internet on a subscription basis. You don't manage anything; you just log in and use it.
* **Analogy:** **Staying at an All-Inclusive Resort.** Everything is built, managed, and cooked for you. You just enjoy the service.
* **Examples:** Microsoft 365, Gmail, Salesforce, Microsoft Teams.

---

### 3. The Shared Responsibility Model

One of the most heavily tested concepts on the AZ-900 exam. Security and management are shared between **Microsoft** and **You**.

* **Rule of Thumb:**
* **Microsoft** is always responsible for the **Cloud (Security *of* the cloud)**: physical data centers, host servers, network cabling, and building security.
* **You (the customer)** are always responsible for your **Data and Access (Security *in* the cloud)**: who has permission to log in, data encryption, and configuration.



| Service Model | What Microsoft Manages | What YOU Manage |
| --- | --- | --- |
| **IaaS** | Physical Datacenters, Physical Servers, Network, Virtualization | OS installation, Patches, Application Code, Network Traffic Rules (Firewalls/NSGs) |
| **PaaS** | Physical Datacenters, Servers, OS, Runtime Environments | Application Code, Data stored in the database |
| **SaaS** | Everything (Servers, OS, App Code, Maintenance) | User Accounts, Device Access, Data Permissions |

---

### 🧪 DAY 3 PRACTICAL LAB

Let's look at how Azure packages these services:

1. Open the **Azure Portal** home dashboard.
2. Search for **App Services** in the top search bar. Click *Create*. Notice how Microsoft asks you for a subscription and resource group, but **does not ask you to configure CPU size or install Windows Server**—because it's **PaaS**!
3. Next, search for **Virtual Machines**. Click *Create*. Notice how here you *must* choose the operating system, disk size, and CPU cores—because it's **IaaS**!

---

### 🎯 AZ-900 Exam Points From Day 3

* **Q1. Which deployment model uses dedicated hardware exclusively for one organization?** *Answer: Private Cloud.*
* **Q2. If a company wants to combine on-premises security with public cloud flexibility, what model should they use?** *Answer: Hybrid Cloud.*
* **Q3. In which service model do you manage the Virtual Machine OS and updates yourself?** *Answer: IaaS (Infrastructure as a Service).*
* **Q4. Who is responsible for physical data center security in an IaaS setup?** *Answer: Microsoft.*

---

### 📝 Day 3 Quiz

1. Is Microsoft 365 an example of IaaS, PaaS, or SaaS?
2. If your manager tells you, *"We don't want to manage operating system patches, just let us deploy our website code directly,"* which service model should you choose?
3. In the Shared Responsibility Model, who owns and manages the user data regardless of whether it is IaaS, PaaS, or SaaS?
4. What is the primary difference between a Public Cloud and a Private Cloud?

---

### 📝 LinkedIn & Social Media Post Template (Day 3)

> **🚀 Day 3 of my AZ-900 Cloud Journey!**
> Today, I tackled the architectural and operational models of cloud computing. Understanding deployment models and service responsibilities is vital for designing enterprise cloud solutions.
> **What I learned & practiced today:**
> ☁️ **Deployment Models:** Public Cloud (shared multi-tenant infrastructure), Private Cloud (dedicated hardware), and Hybrid Cloud (combining both).
> 🏢 **Service Models (The Apartment Analogy):**
> * **IaaS:** Renting an empty apartment (Azure VMs).
> * **PaaS:** Renting a fully furnished hotel room (Azure App Service).
> * **SaaS:** Staying at an all-inclusive resort (Microsoft 365).
> 🔒 **Shared Responsibility Model:** How security is divided between Microsoft (Security *of* the cloud) and the customer (Security *in* the cloud).
> 
> 
> **🛠️ Hands-on Labs Done:**
> * Compared creation settings between Azure IaaS (Virtual Machines) and PaaS (App Services) to see how Microsoft abstracts infrastructure management.
> 
> 
> All notes, diagrams, and labs are documented in my GitHub repository:
> 👉 **GitHub Repo:** `AZ900-Certificate-Course` (`Day-03-Cloud-Deployment-Models-and-Services`)
> #MicrosoftAzure #AZ900 #CloudComputing #DevOpsJourney #CloudArchitecture #LearningEveryday

---

*Great job finishing Day 3! Let me know when you are ready for **Day 4: Azure Global Infrastructure (Regions, Availability Zones, and Region Pairs)**!*