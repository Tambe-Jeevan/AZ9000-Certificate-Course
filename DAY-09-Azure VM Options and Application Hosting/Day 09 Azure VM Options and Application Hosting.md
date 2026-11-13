# **AZ-900 Certification — Day 09**

Azure Compute Services • VM Options, Application Hosting and Scaling

Yesterday, we learned the foundations of Azure compute: Virtual Machines, containers and Azure Functions.

Today, we'll go one level deeper. Imagine your company asks you to host an application in Azure. **How do you decide whether to use a VM, App Service, containers or another Azure service?**

This lesson continues the official Microsoft AZ-900 objective: **Describe Azure compute and networking services**, under the Azure architecture and services domain.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn



## **Today's learning objectives**

By the end of Day 09, you should understand:

1. Azure VM images, sizes and disks.
2. Availability Sets vs Availability Zones.
3. Virtual Machine Scale Sets.
4. Azure App Service and web application hosting.
5. Azure Container Instances vs Azure Kubernetes Service.
6. How to select the correct compute service for a business requirement.

## **Part 1 — Understand Azure VM configuration**

When you create an Azure VM, you are not simply creating a computer. You are selecting several components that determine how that computer operates.

Think of buying a laptop:

- Which operating system do you want?
- How much RAM do you need?
- Which processor do you need?
- How much storage do you need?
- How will you connect it to the network?

Azure VM configuration follows the same idea.

[Azure virtual machines | Azure](https://images.openai.com/static-rsc-4/M_6WNpMEJOGSBEl1hGwxatVJmXLzG153WSmqDwM5PsMyiq94k6aaF4mpTt0vPyxIzHlQiGSQWVpfvS9B5kUk2Z7iMI5E1gvQY7MZsMI1reaKhQwHMqSM28bAc5akZyFAgxxzMDue9E5cjMaLXfJ3eFRz6QKJOJ0GoNzs3bp6vLY?purpose=inline)

[campus.datacamp.com](https://campus.datacamp.com/courses/understanding-microsoft-azure-architecture-and-services/compute-and-networking-services?ex=8\&utm_source=chatgpt.com)

### **1. VM image**

An **image** is a template used to create a VM with an operating system and potentially preconfigured software.

Examples:

- Windows Server 2025
- Windows Server 2022
- Ubuntu Linux
- Red Hat Enterprise Linux

Real-world example:

Your company needs a Windows Server VM for an application that supports Windows Server.

You select a Windows Server image during VM creation. Azure uses that image to provision the operating system disk.

**Remember:** An image is the starting template; it is not the running VM itself.

### **2. VM size**

VM size determines the compute resources allocated to the VM.

For example, one VM might have 2 vCPUs and 8 GB RAM, while another might have 8 vCPUs and 32 GB RAM.

| VM resource         | What it determines                               |
| ------------------- | ------------------------------------------------ |
| vCPU                | Processing capacity                              |
| RAM                 | Available working memory                         |
| Temporary storage   | Local temporary data capacity, where supported   |
| Network performance | Available network throughput                     |
| Disk performance    | Depends on the selected disk and VM capabilities |

**Real-world scenario:**

A small internal application may work with a smaller VM. A database or resource-intensive application may need more CPU, RAM and disk performance.

Do not assume that choosing the largest VM is always best. It can increase cost without providing useful performance improvements.

### **3. VM disks**

A VM needs storage to hold its operating system and data.

**Azure Virtual Machine**

**OS disk**

Windows/Linux operating system and boot files.

**Data disk**

Application files, databases and business data.

**Temporary disk**

Local temporary storage available on certain VM sizes. Data can be lost during maintenance or redeployment. Do not use it for data that must persist.

For example:

- OS disk: Windows Server.
- Data disk: Company application database.
- Temporary disk: Temporary working files.

**Important exam point:** Azure managed disks are managed by Azure as storage resources. The VM uses these disks; you don't manage the underlying physical storage hardware.

## **Part 2 — High availability: Availability Sets vs Availability Zones**

First, understand the problem.

Imagine your company runs an application on one VM. What happens if the physical hardware hosting that VM fails?

The application may become unavailable.

Azure provides availability features to reduce the impact of infrastructure failures.

### **1. Availability Sets**

An Availability Set helps distribute VMs across:

- **Fault domains:** Separate groups of underlying hardware, power and network infrastructure.
- **Update domains:** Groups that can be restarted together during planned maintenance, with updates sequenced across groups.

[Microsoft Azure Fundamentals (AZ-900) Cert Prep: 3 Azure Services \[ o_oyao's Blog \]](https://images.openai.com/static-rsc-4/ElqpWQXKisqUV8aozi4wHKufQGv8BAnE4ujGO4By2dGOJEC__J1kUQ1I3sojgHxSdlXrEtoBafr-I3qX6onIfY7ofwB1y0LUivNtfszRxQwJWAl7PLjaQudLxthB02QD8R9LtZ-lhSozc9zDWVFV1cHHk9DB7htjDVVdTa3Opr0?purpose=inline)

[dyingdown.github.io](https://dyingdown.github.io/2025/04/12/Microsoft-Azure-Fundamentals-AZ-900-Cert-Prep-3-Azure-Services/?utm_source=chatgpt.com)

Example:

Your company runs two application VMs. Placing them in an Availability Set helps reduce the risk that a single hardware failure or planned maintenance event affects both at the same time.

**Remember:** Availability Sets help protect against certain hardware and planned maintenance failures within a datacenter. They do not provide the same physical separation as Availability Zones.

### **2. Availability Zones**

An Availability Zone is a physically separate location within an Azure region.

Each zone has independent infrastructure, including power, cooling and networking.

[Qu’est-ce que les zones de disponibilité Azure ? | Microsoft Learn](https://images.openai.com/static-rsc-4/B0GyrDglEurrMl4E-64n5owBSGqX8cDhi7iRaaBYdEbxHJzLCUWVPw7UugVxP5yf2Tq0D43bfoQzqtX2kHAdfEQ3SCLZeMCksezoEA6CLb_Ts86Zjdtb2LBi-bG05zAo_8MPjdes9kTNWIPj2femH1n4xym_ZZ60A4QQbVe0TQc?purpose=inline)

[learn.microsoft.com](https://learn.microsoft.com/fr-fr/azure/reliability/availability-zones-overview?utm_source=chatgpt.com)

Example:

A company deploys its application across two or more Availability Zones in the same Azure region.

If one zone experiences an outage, workloads in another zone may continue operating, provided the application is designed and configured for that resilience.

### **Availability Sets vs Availability Zones**

| Feature          | Availability Set                                           | Availability Zone                     |
| ---------------- | ---------------------------------------------------------- | ------------------------------------- |
| Main purpose     | Reduce impact of hardware and planned maintenance failures | Reduce impact of a zone-level failure |
| Separation       | Fault and update domains                                   | Physically separate locations         |
| Geographic scope | Within a datacenter-level infrastructure arrangement       | Within an Azure region                |
| Protection level | Different from zone-level protection                       | Stronger physical isolation           |



Exam essential

&#x20;Availability Sets and Availability Zones are not interchangeable.



## **Part 3 — What is Azure Virtual Machine Scale Sets?**

Imagine an e-commerce website.

On an ordinary day, 1,000 customers visit the website. During a major sale, 50,000 customers visit.

One VM might not be sufficient to handle the increased workload.

You could manually create more VMs, but that creates additional administration work.

This is where **Azure Virtual Machine Scale Sets (VMSS)** help.

[VM Scale Sets: Scaling 101 - Euron Daily | Euron Community](https://images.openai.com/static-rsc-4/RmJ8IN7P9UZB6PVxQuGhOhYtRVaNSxfsRpHo-cjg4TBchr0ipabMc4a5k_ED-KUlv8eeXlNRIk37NPgngS4gcGm5ex9-fC1xjTLMuYOYvN9sJf8CXydQ5Gh_llOJcCLEf4s0aVolykzvsFKZZfWFyp6ZQ9hEUQSDD2AT-sWmSMs?purpose=inline)

[euron.one](https://euron.one/community/posts/3949a06c-26fc-4968-98d1-a32917160d30?utm_source=chatgpt.com)

VM Scale Sets allow you to:

- Create and manage a group of VM instances.
- Scale out by increasing the number of instances.
- Scale in by reducing the number of instances.
- Configure automatic scaling based on supported metrics and rules.
- Manage groups of similar VMs together.

### **Scale out vs scale up**

This is a very important distinction.

[Software scalability: 6 dev and operations factors to consider for meeting high demands](https://images.openai.com/static-rsc-4/5H64v3TblYC0cX3U8OI5DqJc26moZosKmEgiuLSSFx7OTq7ATsGP1qjkqSVnu5JY05s0lZQ6lckPZhCLQD01LqRzg4Oj8fJ3Lpz-CzmyHxHnDEeb8waSnHz7Z3gmqK1wpcGtUNF1L83v5B_HYZ_dlrcyiNTHdBklCjR7PPMUCXc?purpose=inline)

**Scale out**

Add more VM instances to share the workload.

[Vertical vs. Horizontal Scaling: When to Choose Each](https://images.openai.com/static-rsc-4/rCdcC4sH9nVhM0aLJ6eD5_NtNCy-xdTpJc4q4UB6sw73kCS0hXDEAfR1um6XmzFqwDM_3XCMbU8jbN3VOOmu9U75yP9lkjIoNBsPuIGG0oBNIgNXz-DURjTYohOddtHfdplw-ihbmFYorOZ96BMx5E8xugOQSw9ELKmwobwdrYE?purpose=inline)

**Scale up**

Increase the resources of an existing VM, such as CPU or RAM.

Example:

- Scale up: Increase a VM from 2 vCPUs to 4 vCPUs.
- Scale out: Increase the number of application VMs from 2 to 5.

**AZ-900 tip:** If the question mentions automatically increasing or decreasing the number of VM instances based on demand, think about VM Scale Sets.

## **Part 4 — Azure App Service**

Now imagine a company wants to host a website.

You have two broad approaches:

1. Create an Azure VM, install an operating system, configure a web server such as IIS, deploy the website and maintain the OS.
2. Use Azure App Service to host a supported web application on a managed platform.

What if the company doesn't want to manage the underlying VM operating system?

That's where Azure App Service becomes useful.

[Elevate Business with Azure: Flexibility & Security Insights by Assemblysoft](https://images.openai.com/static-rsc-4/ewi1TIfhyqexWCWE5wux1VzdzCnYcE2CWMXg1Irs7c2o0YBjie9ZJOY7Z0p97tJTb9qrW92U0fB6H2MGdDzh3HUH7oxagt4AkK9Mruo-q7B4jcMovCROmPUG9ULEwkz97WuPPADMG68Xd-PKNZJnp3zLVz9mHE4HsxpDE9e4M0M?purpose=inline)

[assemblysoft.com](https://assemblysoft.com/blog/post/what-is-azure-and-why-should-i-use-it?utm_source=chatgpt.com)

**Azure App Service** is a managed platform for hosting web applications, web APIs and supported web workloads.

It supports application technologies such as:

- .NET
- Java
- Node.js
- Python
- PHP

### **Example: Company website**

Suppose your company has a website called:

`www.company.com`

The development team writes the website code and deploys it to Azure App Service.

Azure manages much of the underlying hosting infrastructure. The team can focus more on the application rather than maintaining a VM's operating system.

### **VM vs App Service**

| Requirement                      | Azure VM                 | Azure App Service                            |
| -------------------------------- | ------------------------ | -------------------------------------------- |
| Full OS control                  | Yes                      | No                                           |
| Install custom OS-level software | More control             | Limited to supported platform capabilities   |
| Manage guest OS patches          | Customer responsibility  | Azure manages underlying platform components |
| Host web applications            | Yes, after configuration | Yes, managed hosting                         |
| Best fit                         | Custom server workloads  | Supported managed web applications and APIs  |

**Remember:** App Service is not the same as a virtual machine. It is a platform service.

## **Part 5 — Azure Container Instances vs Azure Kubernetes Service**

Yesterday we introduced containers. Today, let's understand two Azure services that run them.

### **1. Azure Container Instances (ACI)**

Azure Container Instances lets you run containers in Azure without managing the underlying VMs directly.

[Run Containers by Using Azure Container Instances | by John Dhinakar Challa | Medium](https://images.openai.com/static-rsc-4/CjwRNphlkoqpf4Yj-HYLbILqeYCdXQPDt6Qq2UXd3iM3p3NPYTd_Q94byuyLO6UaNBZ3l0zOz2feYY91GdWUvs4d5u-98IZzqhxhdXlL8yI46T1Ytj_M9vh_-TraRDFnl73LHbU4LkBhwikA77pugNDnnx3-ojBRSCoiydVjptg?purpose=inline)

[medium.com](https://medium.com/%40dhinakarchalla234/run-containers-by-using-azure-container-instances-510c2619cffe?utm_source=chatgpt.com)

Example:

A developer wants to run a small containerized application or a short-lived task without managing a Kubernetes cluster.

ACI can be a suitable option.

### **2. Azure Kubernetes Service (AKS)**

Suppose a large company has 100 containerized application components.

It needs to coordinate how containers are deployed, scaled, recovered and managed across a cluster.

Kubernetes provides container orchestration.

**Azure Kubernetes Service (AKS)** is Azure's managed Kubernetes service.

[Concepts de base d’Azure Kubernetes Service (AKS) - Azure Kubernetes Service | Microsoft Learn](https://images.openai.com/static-rsc-4/N4r8HId4m5Nxs67oX-PtCvwQupo6gpIaZ3c_f-0z6tdpvtUTcoPdaT2nxncfSXr6y4n_Ad5WIH_iI5xfmyekLdA5BaY3tseIg7lS3bi4HS_H6az5Axy6W9n2ZQXYlRc89J4Tr9-R4XBONDCbuUCMVyZOFeC2zGLD7RXEhsdvxYc?purpose=inline)

[learn.microsoft.com](https://learn.microsoft.com/fr-fr/azure/aks/core-aks-concepts?utm_source=chatgpt.com)

AKS helps manage:

- Container deployment.
- Scaling workloads.
- Scheduling containers.
- Application recovery and orchestration.
- Cluster management.

### **ACI vs AKS**

| Feature             | ACI                                             | AKS                                     |
| ------------------- | ----------------------------------------------- | --------------------------------------- |
| Main purpose        | Run containers                                  | Orchestrate containerized applications  |
| Kubernetes required | No                                              | Yes                                     |
| Complexity          | Generally simpler                               | More complex                            |
| Suitable for        | Individual container workloads and simple tasks | Larger, coordinated container workloads |

**AZ-900 memory trick:**

- ACI = Run containers.
- AKS = Manage and orchestrate containers using Kubernetes.

## **Part 6 — Choose the right Azure compute service**

Let's put everything together.

| Business requirement                                        | Suitable Azure service     |
| ----------------------------------------------------------- | -------------------------- |
| Need a full Windows Server with custom configuration        | Azure Virtual Machines     |
| Need multiple similar VMs that scale with demand            | Virtual Machine Scale Sets |
| Need to host a supported website without managing its VM OS | Azure App Service          |
| Need to run a container without managing VMs or Kubernetes  | Azure Container Instances  |
| Need Kubernetes container orchestration                     | Azure Kubernetes Service   |
| Need event-triggered code execution                         | Azure Functions            |
| Need virtual Windows desktops for employees                 | Azure Virtual Desktop      |

### **Practical scenario**

A company is planning the following projects:

1. Host an internal Windows Server application requiring custom services.
2. Host its public-facing website.
3. Run a containerized background processing task.
4. Manage a large microservices-based application.
5. Run a script every night at 2 AM.

Think about which service you would choose for each.

Answers are at the end of the lesson.

## **Part 7 — Practical lab: Explore your Hyper-V VM and Azure Portal**

Today's practical work has two parts. You can complete the first part entirely on your laptop without Azure charges.

### **Lab A: Inspect your existing&#x20;**`LAB-DC01`

Open PowerShell as Administrator on your Windows 11 host.

**Step 1 — List your virtual machines**

```
Get-VM
```

**Step 2 — Inspect VM configuration**

```
Get-VM -Name "LAB-DC01" |
Format-List Name, State, Generation, MemoryStartup, ProcessorCount
```

**Step 3 — Inspect virtual disks**

```
Get-VMHardDiskDrive -VMName "LAB-DC01"
```

**Step 4 — Inspect virtual network adapters**

```
Get-VMNetworkAdapter -VMName "LAB-DC01" |
Format-List VMName, Name, SwitchName, MacAddress, Status
```

**Step 5 — Inspect virtual processor configuration**

```
Get-VMProcessor -VMName "LAB-DC01" |
Format-List Count, CompatibilityForMigrationEnabled
```

Your goal is to identify the VM's CPU, memory, virtual disks and network connection.

### **Lab B: Explore Azure VM options without deploying**

1. Open the [Azure Portal](https://portal.azure.com/?utm_source=chatgpt.com).
2. Sign in if you have an Azure account.
3. Search for **Virtual machines**.
4. Open the Virtual machines page.
5. Explore the VM creation wizard if available.
6. Observe the following configuration categories:
   - Basics: subscription, resource group, VM name, region, image and size.
   - Disks: OS disk and data disks.
   - Networking: VNet, subnet and network interface.
   - Management: monitoring and management settings.
7. Stop before selecting **Review + create** or deploying a VM.

No deployment required

You can learn the configuration process without creating billable Azure resources.

**Lab observation:** Write down what you see for the image, VM size, OS disk and network settings. Do not share passwords, keys or sensitive account details in GitHub screenshots.

## **Part 8 — Troubleshooting and practical understanding**

These are common situations a junior SysAdmin or cloud support engineer should recognize.

| Problem                                          | What to investigate                                                    |
| ------------------------------------------------ | ---------------------------------------------------------------------- |
| VM is slow                                       | CPU, memory, disk I/O and application performance                      |
| Application unavailable after a hardware failure | VM availability design and redundancy                                  |
| Website needs more capacity                      | App Service scaling or VM Scale Sets, depending on architecture        |
| Container deployment is difficult to coordinate  | Whether Kubernetes/AKS is appropriate                                  |
| Temporary VM disk data disappears                | Temporary storage is not persistent                                    |
| Azure VM cannot connect to a service             | VNet, subnet, network security rules, routing and service availability |

### **Understand this real-world distinction**

If an Azure VM is running but your website is not accessible, the VM's status alone does not prove that the application is healthy.

You may need to investigate:

- Is the operating system running?
- Is the web service running?
- Is the application listening on the expected port?
- Are network security rules allowing traffic?
- Is DNS resolving the correct address?

This connects today's compute lesson with your earlier networking lessons.

## **Part 9 — AZ-900 exam revision**

Remember these important distinctions:

| Exam wording                                  | Think of              |
| --------------------------------------------- | --------------------- |
| Full OS control                               | Azure VM              |
| Multiple VM instances with autoscaling        | VM Scale Sets         |
| Fault domains and update domains              | Availability Sets     |
| Physically separate locations within a region | Availability Zones    |
| Managed web application hosting               | App Service           |
| Run containers without managing VMs           | ACI                   |
| Kubernetes orchestration                      | AKS                   |
| Run code based on events                      | Azure Functions       |
| Virtualized Windows desktops                  | Azure Virtual Desktop |

### **Answers to the five business scenarios**

1. Internal Windows Server application → **Azure Virtual Machines**
2. Public-facing website → **Azure App Service**
3. Containerized background task → **Azure Container Instances**
4. Large microservices application requiring orchestration → **Azure Kubernetes Service**
5. Script running every night at 2 AM → **Azure Functions** (with a timer trigger)

These are suitable choices based on the requirements stated. In real projects, the final architecture also depends on application compatibility, scale, availability, security and cost.

## **Part 10 — Interview questions**

Try answering these without looking at the lesson.

1. What is the difference between an Azure VM image and a VM size?
2. What is the difference between an OS disk and a data disk?
3. What is the difference between Availability Sets and Availability Zones?
4. What is the difference between scale up and scale out?
5. What is Azure Virtual Machine Scale Sets?
6. What is Azure App Service?
7. What is the difference between ACI and AKS?
8. Why would a company choose App Service instead of an Azure VM?

**Important career connection:** These concepts are useful beyond AZ-900. They form the foundation for cloud support, infrastructure support and System Administrator work.

## **Part 11 — Day 09 quiz**

**Test your understanding**

8 questions

**1. Which component defines the operating system template used to create an Azure VM?**

VM image

VM size

Availability Set



**2. Which Azure feature uses fault domains and update domains?**

Availability Zones

Availability Sets

VM Scale Sets



**3. A company wants to increase the number of VM instances automatically when demand rises. Which service is suitable?**

Azure VM Scale Sets

Azure DNS

Azure Files



**4. Which service is designed for managed hosting of websites and web APIs?**

Azure App Service

Azure Virtual Network

Azure Availability Sets



**5. Which Azure service provides managed Kubernetes?**

ACI

AKS

Azure Functions



**6. Which option describes scale up?**

Adding more VM instances

Increasing CPU and RAM of an existing VM

Creating another region



**7. Which disk is intended for temporary data that may be lost?**

OS disk

Managed data disk

Temporary disk



**8. Which service is suitable for running code on a timer trigger?**

Azure Functions

Azure Virtual Desktop

Availability Sets



Submit quiz

## **Part 12 — GitHub documentation**

For your repository `AZ900-Certificate-Course`, create today's folder:

**Day-09-Azure-VM-Options-and-Application-Hosting/**

Day-09-Azure-VM-Options-and-Application-Hosting/\
├── README.md\
├── notes/\
│   ├── 01-Azure-VM-Configuration.md\
│   ├── 02-Availability-Sets-vs-Zones.md\
│   ├── 03-VM-Scale-Sets.md\
│   ├── 04-Azure-App-Service.md\
│   └── 05-ACI-vs-AKS.md\
├── practical-labs/\
│   └── Day-09-HyperV-VM-Inspection.md\
├── screenshots/\
│   ├── Day-09-01-VM-Configuration.png\
│   ├── Day-09-02-VM-Disk-Inspection.png\
│   └── Day-09-03-VM-Network-Inspection.png\
└── quiz/\
&#x20;   └── Day-09-AZ900-Quiz.md

Copy folder structure

**README title:** `Day 09: Azure VM Options and Application Hosting`

**LinkedIn headline:** `AZ-900 Day 09 | Azure VM Configuration, Availability, Scaling and Application Hosting`

**Social media headline:** `AZ-900 Day 09 — Understanding Azure Compute Options`

**Evidence to upload:**

- Hyper-V VM configuration output.
- Virtual disk inspection output.
- Virtual network adapter inspection output.
- Your written comparison of Availability Sets and Availability Zones.
- Quiz score and explanations.

## **Day 09 completion checklist**

**Learning progress**

## **0%**

Understand VM images, sizes and disks

Understand Availability Sets and Availability Zones

Explain scale up vs scale out

Understand VM Scale Sets

Explain Azure App Service

Understand ACI vs AKS

Complete local Hyper-V inspection

Attempt the Day 09 quiz

Document today's work in GitHub

**Your learning rule:** Don't move ahead just because you finished reading. Make sure you can explain the difference between a VM, App Service, ACI and AKS without looking at your notes.

Next lesson: **Day 10 — Azure Networking Services: Virtual Networks, Subnets, VNet Peering and Network Security Groups.**