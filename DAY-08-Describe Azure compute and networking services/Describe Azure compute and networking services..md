# AZ-900 Certification — Day 08

Microsoft Azure Fundamentals • Compute Services: Virtual Machines, Containers and Serverless

Today we begin the Azure Compute and Networking Services section of your official Microsoft AZ-900 study guide. This domain carries 35–40% of the exam, so understanding these services is important.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn



The goal is not just to memorize Azure service names. By the end of today's lesson, you should be able to explain where an application runs in Azure, which compute service to choose, and why.

## Today's learning plan

Part 1 — Compute fundamentals

Physical servers, virtual machines, operating systems, CPU, RAM and storage.

Part 2 — Three ways to run applications

Virtual machines vs containers vs serverless Functions.

Part 3 — Azure services and practical scenarios

Azure Virtual Machines, VM Scale Sets, Azure Functions and safe hands-on practice.

Part 4 — Exam practice

Common traps, interview questions, quiz and GitHub documentation.

## Part 1: What is compute?

In cloud computing, compute means the processing resources used to run software and applications.

For example, think about your workplace IT environment. A company may run:

- An Active Directory server to manage domain users and computers.
- A file server to store shared documents.
- A web server to host an internal website.
- An application server to run business software.

All of these workloads need compute resources.

### Understand a server from zero

Application or service

Active Directory, website, database, etc.

CPU

Processes instructions

RAM

Temporary working memory

Storage

Files and operating system

Networking

Communication with other systems

A server is a computer that provides services or resources to other systems. It can be a physical machine or a virtual machine.

For example, your Windows Server 2025 lab machine `LAB-DC01` is a virtual machine that runs server software. The same principle applies to an Azure virtual machine.

### Physical server vs virtual machine

[Meet Taiwan’s “Queen Of Chassis” Who’s Making Customized Server Casings For Global Tech Giants](https://images.openai.com/static-rsc-4/o5LpL1xyH2ooPLTkAHjH1rSor1MnuGBOJzZyK8mm2l0zC9Pduh3XUvO6LCIDKeu_mtu_DprMCLQhZ9kLe_AI_0RjyDVfyF0zQ7FiXR55DJ_ndHbaqtw7s1dDBg8X0UIK3VSad6aDb_Gukw92pF0jf5AxjBp7_duPCfPpTcOa5tc?purpose=inline)

Physical server

A real hardware machine with its own physical CPU, RAM, storage and network interfaces.

[Computer Hardware Abstraction: Virtual Machines vs Containers](https://images.openai.com/static-rsc-4/lN0ptMP7o3Gpr9vqyNv6QQZaEklPqnY7U5PSf-AObLlM-XXYEXs4DnNrpHyZeh4WUJ4PRYjsEuqG9vJ3QrARHiexo9rCtzdoXxC2wynVY4Y8Yw9rsvnPyhPmIVq4ujXZ2JtB6rHvrabYTTegiM9hKgwN4J5LrKFgRJw7rNTy9SU?purpose=inline)

Virtual machine (VM)

A software-based computer that receives virtual CPU, RAM, storage and networking from a physical host.

Imagine one powerful physical server running a hypervisor. That hypervisor can host several VMs, each with its own operating system and configuration.

For example:

- VM 1: Windows Server for Active Directory.
- VM 2: Windows Server for file sharing.
- VM 3: Linux for web hosting.

Each VM behaves like a separate computer, even though they share the underlying physical hardware.

Important: A VM is not the same thing as a physical server. A VM uses virtualized resources provided by its host infrastructure.

## Part 2: What is an Azure Virtual Machine?

An Azure Virtual Machine (Azure VM) is a computer hosted in Microsoft's Azure cloud infrastructure.

Instead of buying a physical server, you rent computing resources from Azure and configure a virtual computer.

Microsoft Azure datacenter

Physical servers managed by Microsoft

Your Azure virtual machine

Virtual CPU • RAM • Disk • Network interface

Windows Server or Linux

Your chosen operating system

Your applications

AD DS, IIS, scripts, business software, etc.

### What resources does a VM need?

| Resource                | Meaning                             | Example             |
| ----------------------- | ----------------------------------- | ------------------- |
| CPU / vCPU              | Processing power                    | 2 vCPUs             |
| RAM                     | Temporary working memory            | 8 GB                |
| OS disk                 | Stores Windows or Linux             | 128 GB              |
| Data disk               | Stores application or business data | 256 GB              |
| Network interface (NIC) | Connects the VM to a network        | Private IP address  |
| Virtual network (VNet)  | Provides network connectivity       | `10.0.0.0/16`       |
| Operating system image  | Starting template for the VM        | Windows Server 2025 |

These are examples, not required default values. Actual sizes and available options depend on the workload and Azure region.

### Real-world scenario: A company needs an AD server

Suppose your company wants an additional Windows Server for a lab.

With an Azure VM, the general process is:

1. Select a Windows Server image.
2. Choose a VM size based on CPU and memory requirements.
3. Configure the OS disk.
4. Select the virtual network and subnet.
5. Configure administrator access and network security.
6. Create the VM and install/configure the required roles.

Important for your SysAdmin career: Creating the VM only provides the computer. You still need to install and configure services such as Active Directory Domain Services if that is the goal.

## Part 3: Virtual machines vs containers vs Functions

This is one of the most important comparisons in today's AZ-900 lesson. Microsoft explicitly includes these compute types in the official exam objectives.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn

+1



### A. Virtual machines

[Understanding virtualization & containers in the simplest way – ██FR█████ █INTELL███████████](https://images.openai.com/static-rsc-4/pjQ3BMjBOp6lu0WY3LIveOntQJh7xiW5SHhonS2YaPz33G_jWzE_LSytptR4rdtWhtfYbMbEBv6dbbYUlJeyGjOrS3nTIFJHo1DJcCCLswrtoxkMUKU1YkDQ_HJlwORTj70y3RnnLb7X20o5gYOXxjZ2-VPVsNRTS_g2iPnB_90?purpose=inline)

[22.frenchintelligence.org](https://22.frenchintelligence.org/2025/06/22/understanding-virtualization-containers-in-the-simplest-way/?utm_source=chatgpt.com)

A VM behaves like a complete computer with its own guest operating system.

- You can usually manage the OS, install software and configure services.
- You are responsible for many OS-level tasks, such as patching and configuration.
- It is useful when software requires a specific OS or extensive system-level control.

Example: Hosting a Windows Server application that requires particular Windows services.

### B. Containers

[Enterprise IT Glossary | HPE | HPE](https://images.openai.com/static-rsc-4/GAqZ7e9DOHGRX2_due7-bHJ2pEX6trJr0SZc9HkhX0Ca5r6QM0hZ9V7haBhyqjXTKa8o9THCjGv9iiKwnmu3l_tQrMaPtHLOfhehEFLLpRb8Y3N9pJNyd_6FPHU3pDhD-k1gJaItUCSZ6Voa14DibpubhrTm5jLXpiL5oP_kQs4?purpose=inline)

[juniper.net](https://www.juniper.net/us/en/products-services/what-is/docker-container/?utm_source=chatgpt.com)

A container packages an application and its required dependencies so it can run consistently in different environments.

Unlike a typical VM, a container generally shares the host operating system's kernel rather than running a complete separate guest OS.

Think of it this way:

- VM: a complete virtual computer.
- Container: an isolated package for running an application.

Example: A developer packages a web application into a container, tests it on a laptop, and deploys the same container to Azure.

Azure container services include:

- Azure Container Instances (ACI): run containers without managing the underlying virtual machines.
- Azure Kubernetes Service (AKS): managed Kubernetes for deploying and coordinating containerized applications.

You don't need to learn Docker commands or Kubernetes administration in depth for today's lesson. First understand when containers are the right choice.

### C. Serverless computing and Azure Functions

[A Comprehensive Guide to Azure Functions with Node.js | by Isuru Ariyarathna | Medium](https://images.openai.com/static-rsc-4/szTdgj9r6cXKhlYq8M2NW98VXUkf4_icOE-YdgarlVY5MfA6WBqBeDOjeX4xtbC993BGVURJQcsEr-EMsQHV7oU86JUMpLtnLVlBBYsHzLS2-FgqiXV2uod9Jl-Rx_OzQaId2ZBQn9Hg0ABMOnGqwkrdUx-1SqrpKH1ayGpgnUU?purpose=inline)

[medium.com](https://medium.com/%40isuruariyarathna2k00/a-comprehensive-guide-to-azure-functions-with-node-js-3ebfbd2d0341?utm_source=chatgpt.com)

Serverless does not mean that physical servers do not exist. It means Azure manages the underlying infrastructure so you can focus more on your code.

Azure Functions runs code in response to triggers, such as:

- An HTTP request.
- A timer schedule.
- A message arriving in a supported queue.
- An event from another service.

Example: Every time a file is uploaded, run a function to validate the file and send a notification.

You don't generally need to create and maintain a dedicated VM just to run that small piece of event-driven code.

Billing depends on the selected hosting plan and its pricing rules; serverless does not automatically mean free.

### Compare the three

| Feature                    | Virtual machine                 | Container                    | Azure Functions                    |
| -------------------------- | ------------------------------- | ---------------------------- | ---------------------------------- |
| Main purpose               | Run a complete virtual computer | Run packaged applications    | Execute code in response to events |
| Full guest OS per workload | Typically yes                   | Typically no                 | No VM OS to manage                 |
| OS-level control           | High                            | More limited                 | Very limited                       |
| Infrastructure management  | More responsibility             | Depends on container service | Mostly managed by Azure            |
| Typical use case           | Legacy app or custom server     | Portable application         | Event-driven automation            |

## Part 4: Which Azure compute service should you choose?

Use this scenario selector to practise selecting the right service.

Choose a requirement:

Windows Server with custom settings

Package a web application

Run code after a file upload

Host a website without managing its VM

Provide employees with cloud desktops

## Azure Virtual Machines

Best when you need control over the operating system, Windows Server roles, or software that requires a full VM.

## Part 5: Other Azure VM options you must recognize

The exam also tests VM options beyond a single VM.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn



[Virtual Machine Scale Sets In Microsoft Azure - Scaler Topics](https://images.openai.com/static-rsc-4/AdYzNC3UsZR8myGvJDHwapLJVK-B7Zq4eD-7Kh5AbH6lmpsHKFDzMHkpN5mX6wNz7evAoURaGPfgEOjGCVyaEcJt6IQ8Yt8-Hd3lYg7uLi9reTDiHXXcZfyqvDZ-GUpQ1beBfT7WKI6qbSv8RyC41gBxwGa8XfsDHnlhD-61Ukg?purpose=inline)

1\. Azure Virtual Machine Scale Sets (VMSS)

Manage a group of similar VMs and scale the number of instances to match demand.

Example: An online store gets extra traffic during a sale and needs additional VM instances.

[Describe core Azure services | Microsoft Press Store](https://images.openai.com/static-rsc-4/3iVtNBCDIwNNarPjXvhohhvQpSgZq20hrbIhtEze3CtWSMGGQf409e6PWM61z8yI97d78GZYyaaX8tEhIpgrt40XUuxl7UwGaRVO2iTuLEvHEONWUE4y7Qp0cTb-AfB4yUwLH_n7EHnwyawdVTSS_JphzqIA_pQggpqZAubP9hE?purpose=inline)

2\. Availability Sets

Organize VMs across fault domains and update domains to reduce the impact of certain hardware failures and planned maintenance.

Exam distinction: An availability set is not the same as an Availability Zone. Zones are physically separate locations within an Azure region.

[Introduction | Azure Virtual Desktop Succinctly | Syncfusion®](https://images.openai.com/static-rsc-4/me1_CCqmKfnXxGpCn5T0ef29AVh2peH1m7Sqeudfu3_xZ49w9yd2cUuoXAbsPNKylwbU6PuxNca667GusNuXFV9VheyDTB0npFpy878vvh8pVKiBvJYsF2sbIbiWfKmM6Sia30SSFyLMF2C9Y1BXY9oZ6OQ5zPR2XPW3PRx5I64?purpose=inline)

3\. Azure Virtual Desktop (AVD)

Delivers virtualized Windows desktops and applications over the network.

Example: Employees access a managed work desktop from different devices.

## Part 6: Practical lab — understand your own VM

Today's lab uses your existing Windows Server lab. You do not need to create an Azure resource or pay for cloud services.

### Lab A — Inspect your local computer

Open Command Prompt or PowerShell on your Windows 11 laptop.

Run these commands one at a time:

```
systeminfo
```

Shows information about your Windows installation and hardware.

```
Get-CimInstance Win32_ComputerSystem |
  Select-Object Name, Manufacturer, Model, TotalPhysicalMemory
```

Shows your computer's model and physical memory.

```
Get-CimInstance Win32_Processor |
  Select-Object Name, NumberOfCores, NumberOfLogicalProcessors
```

Shows processor and logical-processor information.

```
Get-Volume
```

Shows local volumes, drive letters and file-system information.

What you should learn: Your physical laptop has actual hardware. A VM uses virtual resources presented to its operating system. Azure lets you select those virtual resources according to your requirements.

### Lab B — Inspect your Hyper-V lab VM

On your host laptop, open PowerShell. If Hyper-V management permissions are available, run:

```
Get-VM
```

This lists the virtual machines registered on your host.

Then run:

```
Get-VM -Name "LAB-DC01" |
  Select-Object Name, State, Generation, MemoryStartup
```

This checks the VM's name, running state, generation and startup memory.

To inspect its virtual processors:

```
Get-VMProcessor -VMName "LAB-DC01"
```

To inspect its virtual network adapters:

```
Get-VMNetworkAdapter -VMName "LAB-DC01"
```

If the VM name differs on your machine, replace `LAB-DC01` with the actual name returned by `Get-VM`.

These are inspection commands. They do not create a new VM or intentionally change its configuration.

### Lab C — Think like an Azure administrator

For each resource below, decide what you would need to configure if you created a VM in Azure.

VM deployment checklist

0 of 6

Select a Windows Server or Linux image

Choose VM size: CPU and RAM

Choose OS disk and any required data disks

Configure VNet, subnet and network interface

Plan secure administrator access and network rules

Review pricing and stop/delete resources when finished

For your first Azure practice session, simply explore the Azure Portal and look at the VM creation options without clicking Review + create or deploying anything. Actual Azure resources can incur charges, including disks and networking resources, and stopping a VM does not necessarily stop every charge.

## Part 7: Common exam traps

Memorize the reason for each answer rather than just the service name.

- Need full OS control? Usually an Azure VM.
- Need to run a packaged application? Consider containers.
- Need code to run when an event occurs? Consider Azure Functions.
- Need a managed platform for a website or API? Consider Azure App Service.
- Need many similar VM instances that can scale? Consider VM Scale Sets.
- Need virtual desktops for employees? Consider Azure Virtual Desktop.
- Need to reduce maintenance-related and hardware-failure risks across VMs? Understand Availability Sets and Availability Zones, which provide different mechanisms.

One key distinction: containers still run on infrastructure, and serverless still uses servers. These approaches mainly change how much infrastructure you need to manage.

## Part 8: Interview questions

Try to answer these aloud before reading the suggested answers.

Q1. What is an Azure VM?

A virtualized computer running on Azure infrastructure. You choose its operating system, size, disks and networking configuration.

Q2. What is the difference between a VM and a container?

A VM typically runs a complete guest OS. A container packages an application and dependencies and usually shares the host OS kernel.

Q3. What is Azure Functions used for?

Running event-driven code, such as responding to HTTP requests, timers or supported storage events, without requiring you to manage a dedicated VM for each function.

Q4. What is the difference between a VM and a VM Scale Set?

A VM is an individual virtual computer. A VM Scale Set manages a group of VM instances and supports scaling that group.

Q5. Why would a company use Azure App Service instead of a VM?

If the company wants to host a supported web application or API while Azure manages much of the underlying server infrastructure.

## Part 9: Test yourself

1\. A company needs Windows Server with custom OS settings. Which service is the best fit?

Azure Functions

Azure Virtual Machines

Azure DNS

2\. A developer wants to package an application with its dependencies. What should they consider?

Containers

Availability Sets

Resource groups

3\. A script must run automatically when a supported event occurs. Which service is designed for this?

Azure Virtual Desktop

Azure Functions

Azure VM Scale Sets

4\. Which service helps manage a group of similar VMs and scale instance count?

Azure DNS

Azure VM Scale Sets

Azure App Service

5\. A business wants to host a website without managing the guest OS of a VM. Which is a suitable option?

Azure App Service

A physical server

An availability set

Check my answers

## Part 10: Day 08 GitHub documentation

For your `AZ900-Certificate-Course` repository, use today's structure only.

### Day-08-Azure-Compute-Services/

Day-08-Azure-Compute-Services/\
├── README.md\
├── notes/\
│   ├── 01-Compute-Fundamentals.md\
│   ├── 02-Azure-VMs.md\
│   ├── 03-VMs-vs-Containers-vs-Functions.md\
│   └── 04-VM-Scale-Sets-and-Availability.md\
├── practical-labs/\
│   └── Day-08-Local-VM-Inspection.md\
├── screenshots/\
│   ├── Day-08-01-SystemInfo.png\
│   ├── Day-08-02-HyperV-VM-List.png\
│   └── Day-08-03-VM-Resources.png\
└── quiz/\
&#x20;   └── Day-08-AZ900-Quiz.md

Copy folder structure

README title: `Day 08: Azure Compute Services — VMs, Containers and Azure Functions`

LinkedIn headline: `AZ-900 Day 08 | Understanding Azure Compute Services: Virtual Machines, Containers and Serverless`

Short social headline: `AZ-900 Day 08 — Azure Compute Fundamentals`

Evidence to upload: Only screenshots you actually captured from your own system. Never claim an Azure VM was deployed if you only explored the portal or used your local Hyper-V lab. Avoid screenshots containing passwords, tokens, public IPs you don't intend to share, or other sensitive information.

## Day 08 completion checklist

- I can explain CPU, RAM, storage and networking in a server.
- I understand the difference between a physical server and a VM.
- I can explain VMs vs containers vs Azure Functions.
- I can identify Azure App Service, VM Scale Sets, Availability Sets and Azure Virtual Desktop.
- I completed the local VM inspection lab.
- I attempted the five exam questions.
- I documented today's notes in GitHub.

Official Microsoft learning resource: [Describe Azure compute services](https://learn.microsoft.com/en-us/training/modules/describe-azure-compute-networking-services/?utm_source=chatgpt.com). It covers VMs, containers, Functions, VM options and application hosting.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn



Today's guiding principle: Understand → Practice → Troubleshoot → Explain → Document → Repeat. You do not need to rush into creating cloud resources to learn Azure effectively.