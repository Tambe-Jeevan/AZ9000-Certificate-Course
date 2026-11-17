# **AZ-900 Certification — Day 10**

Azure Networking • Virtual Networks, Subnets, VNet Peering and Network Security Groups

Practical-focused lesson

Today we are moving from simply understanding Azure services to thinking like a **Cloud Support Engineer / Junior Azure Administrator**.

We'll build a small company network in Azure, configure subnets, apply network security rules, understand how two virtual networks communicate, and troubleshoot common connectivity problems.

This follows the official AZ-900 networking objectives, including virtual networks, subnets, peering, VPN Gateway, ExpressRoute, Azure DNS and public/private endpoints. We'll focus on VNet, subnet, peering and NSG hands-on today.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn



## **1. Today's real-world project**

Imagine you join an IT company as a Junior Cloud Support Engineer.

The company has:

- 50 employees.
- A public-facing website.
- An internal application.
- A backend database.
- A requirement to separate public-facing and internal workloads.

Your manager asks you:

> "Create an Azure network where the website and internal application are separated, and control which network traffic is allowed."

This is exactly the kind of problem we will practise.

## **Today's company network design**

**Azure**

Company subscription

**Virtual Network**

VNet

10.10.0.0/16

**Frontend subnet**

10.10.1.0/24

Website workload

**Backend subnet**

10.10.2.0/24

Internal application / database

Each subnet can have its own network security rules.

**Network Security Group (NSG)**

Controls permitted and denied network traffic

By the end of the practical, you will have created this network structure in Azure without deploying a VM.

## **2. First understand what a VNet is**

### **What is a network?**

A network is a group of devices that can communicate with each other.

For example, your office might have:

- Employee laptops.
- Printers.
- Servers.
- Switches.
- Routers.
- Firewalls.

They communicate over a network using IP addresses.

### **What is an Azure Virtual Network?**

An **Azure Virtual Network (VNet)** is a logically isolated private network in Azure.

It allows Azure resources, such as virtual machines, to communicate with each other and, when configured, with other networks or the internet.

Think about your home Wi-Fi:

Your router may provide a private network such as:

`192.168.1.0/24`

Your laptop might have:

`192.168.1.10`

Your mobile might have:

`192.168.1.11`

In Azure, a VNet provides a similar private networking foundation, but it is software-defined and managed through Azure.

### **VNet vs physical router**

| Home / office network | Azure                                              |
| --------------------- | -------------------------------------------------- |
| Private LAN           | Virtual Network                                    |
| Network segment       | Subnet                                             |
| Device IP             | VM private IP                                      |
| Router routes traffic | Azure virtual network routing                      |
| Firewall rules        | Network Security Group and other security controls |

**Important:** An Azure VNet is not a physical router or a physical switch. It is a logical network.

## **3. Understand CIDR and address ranges before creating a VNet**

You have already studied IPv4 and subnetting. Let's connect that knowledge to Azure.

Suppose we create this VNet:

`10.10.0.0/16`

What does `/16` mean?

It means the first 16 bits are the network portion of the IPv4 address.

For our practical, you don't need to calculate every binary address. Understand these examples:

| CIDR           | Total IPv4 addresses |
| -------------- | -------------------- |
| `10.10.0.0/16` | 65,536               |
| `10.10.1.0/24` | 256                  |
| `10.10.2.0/24` | 256                  |
| `10.10.3.0/24` | 256                  |

An Azure subnet must use an address range within its parent VNet address space.

For example:

**Parent VNet:&#x20;**`10.10.0.0/16`

Subnet 1 — `10.10.1.0/24`

Frontend

Subnet 2 — `10.10.2.0/24`

Backend

Subnet 3 — `10.10.3.0/24`

Future expansion

Azure reserves five IP addresses in each IPv4 subnet, so a `/24` subnet has 251 usable IPv4 addresses for Azure resources, rather than 256.

**Why do companies create multiple subnets?**

For logical separation, IP address planning, routing, and applying different security rules to different workloads.

A subnet is not automatically a complete security boundary. Use NSGs and other security controls to define allowed communication.

## **4. What is an NSG?**

NSG stands for **Network Security Group**.

It contains security rules that allow or deny inbound and outbound network traffic to Azure resources.

Think of it like a security guard at an office entrance.

- Employee with valid access → allowed.
- Unauthorized visitor → denied.
- Specific department access → controlled by rules.

An NSG works with:

- Source IP address.
- Source port.
- Destination IP address.
- Destination port.
- Protocol.
- Direction: inbound or outbound.
- Action: allow or deny.

### **Understand inbound and outbound**

[What do you mean by a Firewall?](https://images.openai.com/static-rsc-4/fH2TqijIlmmbQ7HM84wbcTtmUjblJulS93UrTosHuZ0wxv8Sp6WnDJMQ2QxUrMlSYezRV71kptdSyOYJBciXy5dPHnbfYJKI_UH0bg5vyopLVEwn0G2cNT3IUiI5B6K8MHoQZCMw1NcB2_j1ZLW0gZaxLrh8Ox0wiJbukmTNZGY?purpose=inline)

**Inbound**

Traffic coming towards your resource.

[Inbound, Outbound 개념](https://images.openai.com/static-rsc-4/eum6XxZKDfbyVre9AMgIcvHFCStDXzYmXmF0h4zte6yn9NTKAKGtegl6lwxTkFAKchVoPx8VWYy7ddXcL8ahpN67N5QQZYBK1TwgWTbFUaqF76VvTt0Mvnl8nV39AgL9eGbJks_H28P_UkeQeafTH0oKLBs8O17Xf_mo5kSmLGM?purpose=inline)

**Outbound**

Traffic leaving your resource.

Example:

An employee's browser sends an HTTP request to a website hosted on an Azure VM.

- The request enters the VM: inbound traffic.
- The response leaves the VM: outbound traffic.

### **Common network ports**

| Port | Protocol | Common purpose         |
| ---- | -------- | ---------------------- |
| 80   | TCP      | HTTP                   |
| 443  | TCP      | HTTPS                  |
| 3389 | TCP      | Windows Remote Desktop |
| 22   | TCP      | SSH                    |
| 53   | TCP/UDP  | DNS                    |
| 445  | TCP      | SMB file sharing       |

Remember: a port identifies a network service endpoint. It is not a physical USB or charging port.

### **How NSG rules are evaluated**

An NSG rule has a priority number.

- Lower number = higher priority.
- Rules are evaluated from the lowest number upwards.
- The first matching rule determines the action.
- Azure also provides default rules.

For example:

| Priority | Rule                     | Action |
| -------- | ------------------------ | ------ |
| 100      | Allow HTTP from Internet | Allow  |
| 200      | Deny HTTP from Internet  | Deny   |

If traffic matches both, priority 100 is evaluated first and wins.

An NSG also has default rules, including allowing VNet traffic and denying other inbound traffic unless a higher-priority custom rule changes the outcome.

**Important:** NSGs are stateful. Return traffic for an allowed connection is handled automatically.

## **5. Hands-on Lab 1 — Create your first Azure VNet**

No VM deployment

We will create the following resources:

| Resource        | Name              | Configuration        |
| --------------- | ----------------- | -------------------- |
| Resource group  | `AZ900-Day10-RG`  | Central India        |
| VNet            | `AZ900-VNet01`    | `10.10.0.0/16`       |
| Frontend subnet | `Frontend-Subnet` | `10.10.1.0/24`       |
| Backend subnet  | `Backend-Subnet`  | `10.10.2.0/24`       |
| NSG             | `Frontend-NSG`    | Custom inbound rules |

Creating these networking resources does not require a VM. VNet and NSG resources generally have no direct hourly charge, but peering data transfer and other connected services may incur charges. Check the pricing shown for your subscription and region.

### **Step 1 — Open Azure Portal**

Go to [Microsoft Azure Portal](https://portal.azure.com/?utm_source=chatgpt.com).

1. Sign in.
2. Search for **Resource groups**.
3. Select **Create**.
4. Select your subscription.
5. Enter resource group name: `AZ900-Day10-RG`.
6. Select region: `Central India`, if available in your subscription.
7. Select **Review + create**.
8. Select **Create**.

Why create a resource group?

It keeps today's practice resources together so you can manage and delete them together later.

### **Step 2 — Create a Virtual Network**

1. Search for **Virtual networks**.
2. Select **Create**.
3. Choose your subscription.
4. Resource group: `AZ900-Day10-RG`.
5. VNet name: `AZ900-VNet01`.
6. Region: same region as your resource group.
7. Open the **IP addresses** tab.
8. Set the IPv4 address space to `10.10.0.0/16`.

Now configure the subnets.

**Subnet 1**

- Name: `Frontend-Subnet`
- Starting address: `10.10.1.0`
- Size: `/24`

**Subnet 2**

- Name: `Backend-Subnet`
- Starting address: `10.10.2.0`
- Size: `/24`

Depending on the Portal version, you may add subnets from the IP addresses tab or create them after the VNet is deployed.

9. Select **Review + create**.
10. Verify the configuration.
11. Select **Create**.

Do not create a VM, public IP address or NAT Gateway as part of this lab.

### **Step 3 — Verify your VNet and subnets**

Open:

`Virtual networks → AZ900-VNet01 → Subnets`

You should see:

| Subnet          | Address range  |
| --------------- | -------------- |
| Frontend-Subnet | `10.10.1.0/24` |
| Backend-Subnet  | `10.10.2.0/24` |

[M01-Unit 4 Design and implement a Virtual Network in Azure | Azure-Advanced-Networking](https://images.openai.com/static-rsc-4/uI0Rn7i2_-vgHHCsAvnCbWx9s0eifciHd63YsudZb6UG4KaX1jgMMjfpRNYXfcy7T3G4Iq-TH5EsTpUeEOzvoTZM5Gz6gAxqjshnE-s53PyM93DVPS8ottVeN5X9nqic95TjXxJWnDegQ57HYDy49JPy5QE5qaxgTRch50jmBbQ?purpose=inline)

[cloudklass.github.io](https://cloudklass.github.io/Azure-Advanced-Networking/Instructions/Labs/M01_Unit%204%20Design%20and%20implement%20a%20Virtual%20Network%20in%20Azure.html?utm_source=chatgpt.com)

**Your practical task:** Capture a screenshot of your actual VNet and subnet configuration.

Save it as:

`Day-10-01-Azure-VNet-and-Subnets.png`

## **6. Hands-on Lab 2 — Create an NSG and configure a security rule**

Now we will create a network security policy for the frontend subnet.

### **Step 1 — Create the NSG**

1. Search for **Network security groups** in Azure Portal.
2. Select **Create**.
3. Resource group: `AZ900-Day10-RG`.
4. Name: `Frontend-NSG`.
5. Region: `Central India`.
6. Select **Review + create**.
7. Select **Create**.

### **Step 2 — Add an inbound HTTP rule**

Open:

`Frontend-NSG → Inbound security rules → Add`

Enter these values:

| Field              | Value                 |
| ------------------ | --------------------- |
| Source             | Any                   |
| Source port ranges | `*`                   |
| Destination        | IP Addresses          |
| Destination IP     | `10.10.1.0/24`        |
| Service            | Custom                |
| Destination port   | `80`                  |
| Protocol           | TCP                   |
| Action             | Allow                 |
| Priority           | `100`                 |
| Name               | `Allow-HTTP-Frontend` |

Select **Add**.

What did we configure?

We created a rule that allows inbound TCP traffic destined for port 80 on the frontend subnet address range.

**Important security lesson:** This is an exercise in understanding NSG rules, not a recommendation to expose a production website over plain HTTP. Production web traffic should normally use HTTPS, with appropriate application and network security.

### **Step 3 — Associate the NSG with the frontend subnet**

Creating an NSG alone does not automatically apply it to your subnet.

1. Open `AZ900-VNet01`.
2. Select **Subnets**.
3. Select `Frontend-Subnet`.
4. Find **Network security group**.
5. Select `Frontend-NSG`.
6. Save.

[Create, Change, or Delete Azure Network Security Groups | Microsoft Learn](https://images.openai.com/static-rsc-4/kMVO4mPyQlxVxhAmLcikfTEkvMGqH-EAPpZijArCLcre0wUelffyL50MG9elElKtIBBcu59gPGtUurCEiPtpG3Ud6003fq-nlqE1ZIFqxJ9mqXpY7a_ZDE1tTnNZUr3ASQxxNcOlmplOSpZK7gxfLkqFITzjxPpiLxHZgtmzSc0?purpose=inline)

[learn.microsoft.com](https://learn.microsoft.com/en-us/azure/virtual-network/manage-network-security-group?utm_source=chatgpt.com)

Now your frontend subnet has an NSG associated with it.

**Do not associate this NSG with the backend subnet yet.** We want to understand how separate subnets can have separate policies.

### **Step 4 — Verify the association**

Navigate to:

`AZ900-VNet01 → Subnets → Frontend-Subnet`

Confirm that the NSG shows:

`Frontend-NSG`

Capture:

`Day-10-02-Frontend-NSG-Association.png`

## **7. Hands-on Lab 3 — Explore NSG rules like a support engineer**

Imagine an employee reports:

> "I cannot access the company website hosted on an Azure VM."

You need to investigate the network configuration.

Check these items in order:

1. Is the VM running?
2. Is the application/web service running?
3. Is the VM's network interface connected to the correct subnet?
4. Is an NSG associated with the VM's network interface or subnet?
5. Does the NSG permit the required destination port?
6. Is the source IP permitted?
7. Are there any other network controls, such as firewalls or routes, blocking traffic?
8. Is the application listening on the correct port?

### **Important: NSG can be associated in two places**

An NSG can be associated with:

- A subnet.
- A network interface (NIC).

When both subnet and NIC NSGs apply, traffic must be allowed by both applicable security layers.

For inbound traffic, an allow in one NSG does not override a deny in the other.

This is a useful real-world troubleshooting concept.

## **8. Hands-on Lab 4 — Understand VNet Peering**

Now imagine your company has two separate Azure networks:

- VNet A: `10.10.0.0/16`
- VNet B: `10.20.0.0/16`

The company wants resources in both networks to communicate privately.

How can you connect them?

**Azure VNet Peering** connects two virtual networks so resources can communicate using private IP addresses over Microsoft's backbone network.

**VNet A**

10.10.0.0/16

Application network

**VNet B**

10.20.0.0/16

Database network

**VNet Peering**

Private network connectivity

### **Important peering rules**

- The two VNet address spaces must not overlap.
- Peering must be configured in both directions to establish a normal two-way peering relationship.
- Peering does not automatically allow every type of traffic; NSGs and other security controls still apply.
- VNet peering is not the same as VPN Gateway.
- Data transferred through peering can incur charges.

**AZ-900 exam point:** If a question asks how to connect two Azure VNets privately using Microsoft's network, think VNet Peering.

### **Optional practical: Create a second VNet and configure peering**

Do this only after successfully completing the first two labs.

**Step 1 — Create another VNet**

Use the same resource group:

| Setting       | Value          |
| ------------- | -------------- |
| Name          | `AZ900-VNet02` |
| Region        | Central India  |
| Address space | `10.20.0.0/16` |
| Subnet name   | `App-Subnet`   |
| Subnet range  | `10.20.1.0/24` |

**Step 2 — Configure peering from VNet 1**

1. Open `AZ900-VNet01`.
2. Select **Peerings**.
3. Select **Add**.
4. Peering link name: `VNet01-to-VNet02`.
5. Remote virtual network: `AZ900-VNet02`.
6. Remote peering link name: `VNet02-to-VNet01`.
7. Keep gateway transit options disabled for this basic lab.
8. Select **Add**.

The Portal can create the two peering links together.

**Step 3 — Verify**

Open the Peerings page on both VNets.

You should see:

- `AZ900-VNet01` → Connected.
- `AZ900-VNet02` → Connected.

[Membuat, Mengubah, atau Menghapus Peering Azure Virtual Network | Microsoft Learn](https://images.openai.com/static-rsc-4/mzPMx4xc60Zt3U74EUokDPypmF0qEw6IFK6WzPtnLJMQpwQ7DmhgM4QBfFnZDoJijz5iPOEhe5vg9TXlpNUSWUuvdvpcEO7BcQqSwxBVV-Z5XMuZs8reN3YEt4MGBcE43g8HhfV6sGqk3xnE8QW4ogc7O0uE8ErvqNRK6FErl3o?purpose=inline)

[learn.microsoft.com](https://learn.microsoft.com/id-id/azure/virtual-network/virtual-network-manage-peering?utm_source=chatgpt.com)

You have now configured private network connectivity between two Azure VNets.

**Can you ping between them right now?**

Not necessarily. We have not deployed any VMs, so there are no guest operating systems to respond to ping. Also, NSGs and guest firewalls can block ICMP. A connected peering status confirms the peering relationship, not that every application connection works.

For this lab, simply verify the peering status. Avoid deploying VMs just to test it.

## **9. Other Azure networking services — know what they do**

These are also part of the official AZ-900 networking objectives. We'll explore them in more detail in upcoming lessons.

[Everything you need to know about Azure Public & Private DNS - Loth.io](https://images.openai.com/static-rsc-4/JUA5bKtU8fKSzDTJtg3d2tzBQFfOzQl--QrJ2hZmDgR5lQlRhRzgnNnVrBHEYXZ0xEscoBwHbnvsU5xC8nclJhaeWAEQ_6hfCRqnqrugXo-NvOP067Wryt1yuC3Xr-7JOwRIVmlYek1g_AdbbV-7xn4fL9Et8_Tr6zJcdjMAoO8?purpose=inline)

**Azure DNS**

Hosts DNS domains and records using Azure's DNS infrastructure.

Example: Resolving `www.company.com` to an IP address.

Remember: DNS resolves names; it does not itself host your website.

[vol.172　S2S VPN接続によるオンプレ－Azure間接続（前）｜Windows Server 2016 EOSまであと351日](https://images.openai.com/static-rsc-4/SnjKwDmF8Pjpf5jAzlZmFBxB76xNNEM1Yc2HSKzHelkFL-13Ykc30XMn_YX3iyu5txrkB3DCndg4uedbvbOVez4Y2VeWBNxI8d-GkOQ5jkVqFq38ea78FQDvsrxgWYiARU3JDHtg2uNQlDFlzVqBakwPR3dD-xhutMZjuszm8NQ?purpose=inline)

**Azure VPN Gateway**

Connects networks using encrypted VPN tunnels over the internet.

Example: A company's office network securely connects to an Azure VNet.

[The What, Where, Who, How — And Why of Azure ExpressRoute](https://images.openai.com/static-rsc-4/_KWFIm65QnSts4KWwxsNmSg-FVtCtznuUkuZe1ltMrL2EoQAWsVna4vt-5JioDm48fNg8zCaKRSgZdaMbOCa8FEZu5LDhHrkmVp6_-I8fkLM8JkgucQjHtvQARee1YMiwcjsdAM58bdXONIp30u6sA2IH2XMtqoakaHQ9xtouXg?purpose=inline)

**Azure ExpressRoute**

Provides private connectivity between on-premises infrastructure and Microsoft cloud services through a connectivity provider.

Example: A large company requires private connectivity rather than relying on a public internet VPN connection.

[Azure networking services overview | Azure Docs](https://images.openai.com/static-rsc-4/cJuCQGub-GPy7nVcpsXjqC3cnpbCsOM0E3a5yEHlcZz-K99Nx0cNHt7QpFZ0HHVgTOM5eb7BivYkbMpEumVM-KMwlA7cr0L0eMgK08QW3fUqrypV0VTS8wpQThebFjjUOSMPkz7CwzmiYdow72zpzO3d5qU-CIAJDevTOaK_4EE?purpose=inline)

**Public and private endpoints**

A public endpoint is reachable through a public network address, subject to security controls. A private endpoint uses a private IP address in your VNet to access a supported Azure service through Azure Private Link.

Example: Accessing an Azure Storage account through a private endpoint.

### **Quick comparison**

| Service          | Main purpose                                |
| ---------------- | ------------------------------------------- |
| VNet             | Private Azure network                       |
| Subnet           | Divide a VNet into network segments         |
| NSG              | Filter network traffic                      |
| VNet Peering     | Connect VNets privately                     |
| Azure DNS        | Resolve domain names                        |
| VPN Gateway      | Encrypted network connectivity              |
| ExpressRoute     | Private connectivity through a provider     |
| Private Endpoint | Private access to a supported Azure service |

## **10. Real-world troubleshooting exercise**

Imagine you are working in a cloud support team.

A developer reports:

> "Our application VM has a private IP address of `10.10.1.10`. The database VM is `10.20.1.10`. Both VNet peerings show Connected, but the application cannot connect to the database on TCP port 1433."

How would you investigate?

**Troubleshooting workflow**

1. **Check peering**

   Confirm peering status is Connected in both VNets.
2. **Check address ranges**

   Confirm VNet address spaces do not overlap.
3. **Check routing**

   Check effective routes and any custom route tables.
4. **Check NSGs**

   Confirm the database subnet/NIC permits inbound TCP 1433 from the application VM or its subnet.
5. **Check database server**

   Confirm the database service is running and listening on TCP 1433.
6. **Check host firewall**

   Confirm the database server's operating-system firewall permits the connection.
7. **Test connectivity**

   From the application VM, run:
   ```
   Test-NetConnection 10.20.1.10 -Port 1433
   ```
   Check `TcpTestSucceeded`.

A successful peering configuration does not guarantee a successful application connection. This is exactly why cloud support engineers check multiple layers.

**Important:** Do not open database port 1433 to the entire internet. Allow only the required private source network or application hosts.

## **11. Practical Azure CLI — optional command-line experience**

Once the Portal lab is complete, try creating a separate practice network using Azure Cloud Shell's Bash environment.

This gives you experience with the same kind of infrastructure configuration used by cloud engineers.

First, check that you are signed in and identify your subscription:

```
az account show --output table
```

Create a separate resource group:

```
az group create \
  --name AZ900-Day10-CLI-RG \
  --location centralindia
```

Create a VNet with one subnet:

```
az network vnet create \
  --resource-group AZ900-Day10-CLI-RG \
  --name CLI-VNet01 \
  --address-prefixes 10.30.0.0/16 \
  --subnet-name Web-Subnet \
  --subnet-prefixes 10.30.1.0/24
```

Create a second subnet:

```
az network vnet subnet create \
  --resource-group AZ900-Day10-CLI-RG \
  --vnet-name CLI-VNet01 \
  --name DB-Subnet \
  --address-prefixes 10.30.2.0/24
```

Check the VNet:

```
az network vnet show \
  --resource-group AZ900-Day10-CLI-RG \
  --name CLI-VNet01 \
  --output table
```

List its subnets:

```
az network vnet subnet list \
  --resource-group AZ900-Day10-CLI-RG \
  --vnet-name CLI-VNet01 \
  --output table
```

These commands create actual Azure networking resources, so only run them if you have permission to use the subscription.

**Important:** Cloud Shell may require a storage account for persistent files. That storage can incur charges. Review any storage setup prompt before accepting it. You can skip the CLI lab if you want to keep today's work to the Portal lab.

## **12. Cleanup — important real-world administrator habit**

After completing your lab and capturing screenshots, delete the practice resources if you no longer need them.

### **Portal cleanup**

1. Open **Resource groups**.
2. Select `AZ900-Day10-RG`.
3. Verify it contains only today's practice resources.
4. Select **Delete resource group**.
5. Type the resource group name to confirm.
6. Delete it.

If you also completed the CLI lab, delete `AZ900-Day10-CLI-RG` separately.

Before deleting

Never delete a real company resource group without authorization. In this exercise, delete only the resource groups you created specifically for this lab.

This is an important cloud cost-management habit: create, inspect, document and clean up.

## **13. Day 10 exam practice**

Try these questions before looking at the explanations.

**AZ-900 Day 10 quiz**

8 questions

**1. What is an Azure VNet?**

A physical router

A logically isolated private network in Azure

A DNS record



**2. Which address range belongs inside 10.10.0.0/16?**

10.10.1.0/24

10.20.1.0/24

192.168.1.0/24



**3. What is the main purpose of an NSG?**

Create virtual machines

Filter inbound and outbound network traffic

Register domain names



**4. Which service connects two Azure VNets privately?**

Azure DNS

VNet Peering

Azure Functions



**5. Which is true about VNet peering?**

Overlapping VNet address spaces are recommended

Peering automatically allows all traffic

VNet address spaces must not overlap



**6. What does a lower NSG priority number mean?**

Higher priority

Lower priority

The rule is disabled



**7. Which service provides encrypted VPN connectivity between networks?**

Azure VPN Gateway

Azure DNS

Azure App Service



**8. Which service provides private connectivity through a connectivity provider?**

ExpressRoute

Azure Functions

Azure Files



Check my answers

## **14. GitHub documentation**

Repository: `AZ900-Certificate-Course`

Create today's folder:

**Day-10-Azure-Virtual-Networking/**

Day-10-Azure-Virtual-Networking/\
├── README.md\
├── notes/\
│   ├── 01-Azure-Virtual-Network.md\
│   ├── 02-Subnets-and-CIDR.md\
│   ├── 03-Network-Security-Groups.md\
│   ├── 04-VNet-Peering.md\
│   └── 05-Azure-Networking-Services.md\
├── practical-labs/\
│   ├── Lab-01-Create-Azure-VNet.md\
│   ├── Lab-02-Configure-NSG.md\
│   ├── Lab-03-VNet-Peering.md\
│   └── Lab-04-Azure-CLI.md\
├── screenshots/\
│   ├── Day-10-01-Azure-VNet-and-Subnets.png\
│   ├── Day-10-02-Frontend-NSG-Association.png\
│   ├── Day-10-03-NSG-Inbound-Rules.png\
│   └── Day-10-04-VNet-Peering-Connected.png\
└── quiz/\
&#x20;   └── Day-10-AZ900-Quiz.md

Copy folder structure

**README title:** `Day 10: Azure Virtual Networking — VNet, Subnets, NSG and Peering`

**LinkedIn headline:** `AZ-900 Day 10 | Hands-on Azure Networking: Created a VNet, Configured Subnets, NSG and VNet Peering`

**Short social headline:** `AZ-900 Day 10 — My First Azure Networking Lab`

**Evidence to upload:**

- Actual VNet and subnet configuration.
- NSG inbound security rules.
- NSG-to-subnet association.
- Both VNet peering statuses, if you completed the optional lab.
- Azure CLI output, if you completed that lab.
- A short troubleshooting explanation in your README.

Do not upload screenshots of resources you did not actually create.

## **15. Day 10 completion tracker**

**Practical progress**

# **0/11**

Understand Azure VNet and its purpose

Understand CIDR and subnet address ranges

Create AZ900-VNet01

Create Frontend-Subnet and Backend-Subnet

Create Frontend-NSG

Configure the HTTP inbound rule

Associate NSG with frontend subnet

Create and verify VNet peering (optional)

Practise Azure CLI (optional)

Complete quiz and document evidence

Delete practice resources

## **Official Microsoft learning resources**

- [Introduction to Azure Virtual Networks](https://learn.microsoft.com/en-us/training/modules/introduction-to-azure-virtual-networks/?utm_source=chatgpt.com)
- [Network Security Groups overview](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview?utm_source=chatgpt.com)
- [Virtual Network Peering overview](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview?utm_source=chatgpt.com)
- [Introduction to Azure Virtual Networking](https://learn.microsoft.com/en-us/training/modules/introduction-to-azure-virtual-networking/?utm_source=chatgpt.com)

## **Your practical assignment**

I want you to complete **Lab 1 and Lab 2 first**. Don't rush through peering or CLI.

When you finish, send me:

1. Screenshot of your VNet and both subnets.
2. Screenshot of your NSG inbound rules.
3. Screenshot showing the NSG association with `Frontend-Subnet`.

If you encounter an error, send that screenshot too. We'll troubleshoot it like a real Azure support ticket, step by step.

**The target is not just to pass AZ-900. It's to become comfortable looking at an Azure network and understanding how its resources communicate.**&#x20;