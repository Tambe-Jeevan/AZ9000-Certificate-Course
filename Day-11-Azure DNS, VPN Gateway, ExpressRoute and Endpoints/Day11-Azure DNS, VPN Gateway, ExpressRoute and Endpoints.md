# **AZ-900 Certification — Day 11**

Azure Networking Services • DNS, VPN Gateway, ExpressRoute and Endpoints

Today we continue from Day 10, where we created an Azure VNet, subnets and an NSG, and learned VNet peering.

**Today's goal:** Understand how Azure networks connect to other networks, how resources resolve names, and how to access Azure services securely.

We'll use real-world support scenarios and practical exercises rather than only memorizing definitions.

Today's topics align with the official AZ-900 networking objectives.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn



## **Today's real-world scenario**

Imagine your company has:

- An office in Pune.
- An on-premises Active Directory server.
- An application running in Azure.
- A database hosted in Azure.
- Employees who need to access the application securely.

Your manager asks:

> "How will our office connect to Azure? How will users access the application by name? And how can we access Azure services without exposing them publicly?"

Today we'll answer these questions.

## **Part 1 — Azure DNS**

### **1. Why do we need DNS?**

You already know that computers communicate using IP addresses.

For example:

```
Server IP: 10.10.1.10
```

But users prefer names:

```
app.company.com
```

DNS (Domain Name System) translates domain names into IP addresses.

**User's laptop**

app.company.com

**DNS server**

Finds the IP address for the name

**Application server**

10.10.1.10

DNS does not transfer the website itself. It helps the client discover the IP address to connect to.

### **2. What is Azure DNS?**

Azure DNS is a service that hosts DNS domains and records using Azure's DNS infrastructure.

It allows you to manage DNS records such as:

| Record | Purpose                                              |
| ------ | ---------------------------------------------------- |
| A      | Maps a name to an IPv4 address                       |
| AAAA   | Maps a name to an IPv6 address                       |
| CNAME  | Maps one name to another name                        |
| MX     | Identifies mail servers                              |
| TXT    | Stores text information, often used for verification |
| NS     | Identifies authoritative name servers                |

Example:

```
www.company.com  →  203.0.113.10
```

Here, the A record maps the hostname to an IPv4 address.

**Important:** Azure DNS is a DNS hosting service. It is not the same thing as an Azure VM running Windows DNS Server.

### **3. Public DNS vs Private DNS**

[How DNS Resolution Works - DEV Community](https://images.openai.com/static-rsc-4/mM_yn-gLyLpU6FHZXFdSdlLN8rRx3bcMh6invC27KCeQ81CkP5TAB3j0RrDzx804O-tkIZvF6683BX5d9493vaEIuMcINNHJSNNCCxiSL_T5MNdui_31w3quBGm-kJlKs30az1RLZnPhPzPM3zlnNP7_RaTpZMQa_sNvw-XJmgs?purpose=inline)

**Public DNS**

Resolves publicly available domain names.

[Difference between Azure Private DNS zones vs. DNS Zones ? - Microsoft Q&A](https://images.openai.com/static-rsc-4/gdSc2-baF8VVE60DkSq6q1L9ZAa1AkQJM989Irh6XSfczh1xO3nic5dNnAeLi4G0EzrYEhQwDAbm7zEmzuUmtJ_kZVsO-Sq5myze0jQ1H5UvPw3Ams_lH9hqMHTzAyOhaB3RzJ-6sYop17z8KG2d6W724Xz2d-wVdtaQSl1oa50?purpose=inline)

**Private DNS**

Resolves names used within private networks.

Example:

Public name:

```
www.company.com
```

Private name:

```
db01.internal.company.com
```

The private name may resolve to:

```
10.10.2.10
```

That private IP is not directly reachable from the public internet.

**Exam tip:** Azure Private DNS zones provide name resolution within linked virtual networks, subject to their configuration.

## **Part 2 — Practical Lab 1: DNS troubleshooting on your laptop**

This lab uses Windows commands. It does not require deploying Azure resources.

### **Step 1 — Check your DNS configuration**

Open Command Prompt:

```
ipconfig /all
```

Find your active network adapter.

Look for:

```
DNS Servers
```

This tells you which DNS server your computer is configured to use.

Your DNS server may be your home router, your organization's DNS server, or another configured resolver.

### **Step 2 — Resolve a domain name**

```
nslookup microsoft.com
```

You should see the DNS server used and one or more IP addresses returned for the name.

Try:

```
nslookup learn.microsoft.com
```

The returned addresses can vary because DNS responses depend on location, caching and service configuration.

### **Step 3 — Query a specific DNS server**

```
nslookup microsoft.com 8.8.8.8
```

This asks the DNS resolver at `8.8.8.8` for the name.

If the query fails, it does not automatically mean DNS is broken. Your network or firewall may block traffic to that resolver.

### **Step 4 — Test DNS and network connectivity separately**

```
Resolve-DnsName microsoft.com
```

Then:

```
Test-NetConnection microsoft.com -Port 443
```

Understand the difference:

- `Resolve-DnsName` tests name resolution.
- `Test-NetConnection` tests connectivity to a destination and port.

A website can resolve correctly but still fail to connect because of routing, firewall, service or application problems.

**Real-world support note:** Always identify whether the issue is DNS resolution or network connectivity before changing configuration.

## **Part 3 — Azure VPN Gateway**

### **1. The business problem**

Your company has an office network:

```
Office network: 192.168.10.0/24
```

Your Azure VNet is:

```
Azure VNet: 10.10.0.0/16
```

Employees in the office need to access Azure resources using private IP addresses.

How do you connect the office network to Azure?

One option is **Azure VPN Gateway**.

[gautampambhar.com](https://gautampambhar.com/AllCourses/networkingBasics/networkingBasics.html?utm_source=chatgpt.com)

A VPN Gateway creates VPN connectivity between networks using encrypted tunnels.

For a typical site-to-site VPN:

- The office has a VPN device or gateway.
- Azure has a VPN Gateway.
- Both sides are configured with matching connection details.
- Traffic travels through an encrypted tunnel over the internet.

### **2. Site-to-site VPN**

A Site-to-Site (S2S) VPN connects an entire office network to an Azure VNet.

Example:

```
Office laptop
192.168.10.25
      |
Office router / VPN device
      |
Encrypted VPN tunnel
      |
Azure VPN Gateway
      |
Azure VNet
10.10.0.0/16
      |
Azure application VM
10.10.1.10
```

The office laptop can potentially access the Azure application using its private IP address, provided routing and security rules allow it.

### **3. Point-to-site VPN**

A Point-to-Site (P2S) VPN connects an individual client device to an Azure VNet.

Example:

You are working from home with your laptop and need secure access to your company's Azure network.

You connect using a supported VPN client and configuration.

[Cloud Safety: How to Establish Azure Point-to-Site VPN Connections - DEV Community](https://images.openai.com/static-rsc-4/cVKpFEW_rWuIlt9dd8bBX9fPOF9p13dTYEmIbyqLAC8BBWBcyCxDEcZHZwbwOeqe3n_IfrAtIMIushvRmbFLzNYWcFVtiR43dzYgEIgZ9HesbgkEmL8SOzECdUj2wNVw6qgo9MPy9UzcybGwqqXq3RNPXJzJIA8EgdFO_xYa5Kk?purpose=inline)

[dev.to](https://dev.to/gabbyti/cloud-safety-how-to-establish-azure-point-to-site-vpn-connections-3e?utm_source=chatgpt.com)

### **S2S vs P2S**

| Feature             | Site-to-Site                               | Point-to-Site                                      |
| ------------------- | ------------------------------------------ | -------------------------------------------------- |
| Connects            | Network to network                         | Individual device to network                       |
| Typical use         | Office to Azure                            | Remote employee to Azure                           |
| Client VPN software | Usually not required on each office laptop | Required on the client, depending on configuration |
| VPN Gateway         | Yes                                        | Yes                                                |

**Exam memory trick:**

- S2S = Site network ↔ Azure network.
- P2S = Personal laptop ↔ Azure network.

## **Part 4 — Azure ExpressRoute**

Now imagine a large company has strict connectivity requirements.

The company does not want its private corporate network connectivity to rely on a normal internet VPN connection.

It needs private connectivity to Microsoft cloud services through a connectivity provider.

This is where **Azure ExpressRoute** comes in.

[Microsoft Azure Express Route - fast private connection from Vega Telecom provider](https://images.openai.com/static-rsc-4/0QF68GtZejZeKSnlcUSPxZE5BJQnPfcyZXaNB00ek-JIxwpIwId9Ht_xbXgsVEuHZvop3s1nFbkrRXgUDy8Pcxx4uyTNdmmBZV5BKgkE_s5Zxek_PUft2qmpa8sosR-svnG9KXohxTSqyvxKvq1-uAmjzgusQ9FmT4s1RRj4Txw?purpose=inline)

[vega.ua](https://vega.ua/eng/for_office/azure_express_route?utm_source=chatgpt.com)

ExpressRoute provides private connectivity between an organization's on-premises infrastructure and Microsoft cloud services through a connectivity provider.

### **VPN Gateway vs ExpressRoute**

| Feature             | VPN Gateway                                          | ExpressRoute                                                |
| ------------------- | ---------------------------------------------------- | ----------------------------------------------------------- |
| Connection          | VPN tunnel over the internet, commonly encrypted     | Private connectivity through a provider                     |
| Internet dependency | Uses internet connectivity for typical VPN scenarios | Does not use public internet as the private connection path |
| Setup               | Often simpler and faster to establish                | Requires provider connectivity and planning                 |
| Typical use         | Remote access or office-to-Azure VPN                 | Enterprise private connectivity requirements                |

**AZ-900 exam tip:** If the question says "private connectivity through a connectivity provider," think ExpressRoute.

If it says "encrypted VPN tunnel over the internet," think VPN Gateway.

Note that ExpressRoute does not automatically encrypt all traffic. If encryption is required, additional design considerations may apply.

## **Part 5 — Public and private endpoints**

This is an important concept for cloud support and security.

Imagine your company stores documents in an Azure Storage account.

The company wants its Azure VM to access the storage account without exposing the storage access path through a public endpoint.

### **1. Public endpoint**

A public endpoint allows access through a service's public network address, subject to the service's network and authentication controls.

Example:

```
Azure VM
   |
   | Public network endpoint
   |
Azure Storage
```

A public endpoint does not mean everyone can automatically access your data. Authentication, authorization and network restrictions still apply.

### **2. Private endpoint**

A private endpoint uses a private IP address from your VNet to access a supported Azure service through Azure Private Link.

[Deploy Application on Azure AKS using HELM | by Arun Singh | KPMG UK Engineering | Medium](https://images.openai.com/static-rsc-4/JL7pf0iJTRzz3_QmS4Rcl3Jdo8v5ww-UBC8u9UJb6i1TK7xKiJNuqtqrrdkycO6Nu-9B4wPC_oGdvPFv8nwim5HJaJaFw9AjwFDS_tc0dghRrJexjNvZ_cpjFFr1obSKZ4XW1SZNDQ81sZJVUAG3CNg8AH7cdhGACkM7e-IoOHs?purpose=inline)

[medium.com](https://medium.com/kpmg-uk-engineering/deploy-application-on-azure-aks-using-helm-8c3b88f29abc?utm_source=chatgpt.com)

Example:

```
Azure VM
10.10.1.10
     |
     | Private network
     |
Private endpoint
10.10.2.10
     |
     | Azure Private Link
     |
Azure Storage account
```

The private endpoint is a network interface with a private IP address in your VNet.

This helps you access supported Azure services privately.

### **Important difference**

| Public endpoint                                    | Private endpoint                                                       |
| -------------------------------------------------- | ---------------------------------------------------------------------- |
| Uses a public service endpoint                     | Uses a private IP in your VNet                                         |
| Access may be restricted by service network rules  | Provides private connectivity through Private Link                     |
| Public network access can be enabled or restricted | Can be used to avoid public network access for the private access path |

**Remember:** A private endpoint does not automatically make the entire service secure. You still need proper identity, permissions, DNS and network configuration.

## **Part 6 — Practical Lab 2: Create a DNS record in your Windows Server lab**

This is a valuable SysAdmin exercise because Active Directory depends heavily on DNS.

You already have a Windows Server lab with the domain `lab.local`. We'll use that environment if your DNS Server role is installed and working.

Local lab — no Azure charges

### **Lab objective**

Create a DNS record:

```
app01.lab.local → 192.168.10.50
```

This is a practice record. It does not mean a real server exists at that IP address.

### **Step 1 — Check DNS Server**

On your Windows Server VM:

1. Open **Server Manager**.
2. Select **Tools**.
3. Open **DNS**.
4. Expand your server name.
5. Expand **Forward Lookup Zones**.
6. Look for `lab.local`.

If the DNS console or `lab.local` zone is missing, stop and check whether the DNS Server role is installed and whether your AD DNS configuration is correct. Don't change the production DNS configuration at work.

### **Step 2 — Create a host (A) record**

1. Right-click the `lab.local` zone.
2. Select **New Host (A or AAAA)**.
3. Name: `app01`
4. IP address: `192.168.10.50`
5. Select **Add Host**.

You have now created a DNS record.

[WEC/WEF - TeskaLabs LogMan.io](https://images.openai.com/static-rsc-4/F9Y96qPzt1WDpgpJmeSzNE2vT4u0qmHN4eSPo5eZlz7QoN9fOQ_qcwZgKQwaUDihTjzvqZZJJJRnPhdcn-W1AXaRU44lt15fPFkgxmdcoawAUn28tViMBRUgyjV4OVNvcWCoyFMdkbl-PpeyPsygXrUG0JzXX5Y643I5lbVdlQ8?purpose=inline)

[ndocs.teskalabs.com](https://ndocs.teskalabs.com/logman.io/collector/log-sources/microsoft/windows-events/wec/?utm_source=chatgpt.com)

### **Step 3 — Test the record**

On a client machine that can reach your lab DNS server, run:

```
nslookup app01.lab.local
```

Expected result:

```
Name:    app01.lab.local
Address: 192.168.10.50
```

If the result is different, investigate:

- Is the client using the correct DNS server?
- Does the DNS record exist?
- Is the DNS zone loaded?
- Is the client using a cached DNS response?

You can clear the Windows DNS client cache with:

```
ipconfig /flushdns
```

Then query again:

```
nslookup app01.lab.local
```

### **Step 4 — Understand what you just did**

You created a name-to-IP mapping.

You did **not** create a server, open a network port, or establish connectivity to that IP.

DNS resolution and network connectivity are separate tasks.

This distinction is frequently tested in cloud support interviews.

## **Part 7 — Practical Lab 3: Explore Azure networking in the Portal**

Today we will inspect the services rather than deploy expensive gateways.

Open the [Azure Portal](https://portal.azure.com/?utm_source=chatgpt.com).

### **Explore VPN Gateway**

1. Search for **Virtual network gateways**.
2. Open the service page.
3. Select **Create** only if you want to inspect the wizard.
4. Observe the gateway type and VPN type options.
5. Stop before deploying.

A VPN Gateway deployment can take time and may incur significant charges. Do not create one just for this lesson.

### **Explore ExpressRoute**

1. Search for **ExpressRoute circuits**.
2. Open the service page.
3. Review the configuration options.
4. Do not create a circuit.

ExpressRoute requires provider connectivity and is not a simple free lab resource.

### **Explore Azure DNS**

1. Search for **DNS zones**.
2. Open the service page.
3. Review the creation options.
4. Observe the domain name and record-set concepts.

Creating a DNS zone may incur charges. For today's practical experience, use your local Windows DNS lab instead.

### **Explore Private Endpoint**

1. Search for **Private endpoints**.
2. Open the service page.
3. Review the resource type and network configuration options.
4. Stop before creating a private endpoint.

A private endpoint normally requires a supported Azure service resource and a VNet/subnet. Some associated resources may incur charges.

**Practical evidence:** Capture screenshots of the service pages or configuration options you actually inspected. Do not claim that a gateway or private endpoint was deployed if you only viewed its wizard.

## **Part 8 — Real-world troubleshooting: DNS works, but application doesn't**

Imagine a user reports:

> "I can resolve `app01.lab.local` to `192.168.10.50`, but I cannot open the application."

What do you check?

## **Troubleshooting flow**

1. **Check DNS resolution**
   ```
   nslookup app01.lab.local
   ```
   Confirm the name resolves to the expected IP.
2. **Check IP connectivity**
   ```
   ping 192.168.10.50
   ```
   Ping may be blocked, so a failed ping alone doesn't prove the server is down.
3. **Check the application port**
   ```
   Test-NetConnection 192.168.10.50 -Port 443
   ```
   Replace 443 with the actual application port.
4. **Check routing and firewall**\
   Confirm the client has a route to the destination and that firewalls permit the traffic.
5. **Check the application**\
   Confirm the application service is running and listening on the expected port.

### **Interview-ready answer**

If asked, "What is your troubleshooting approach when DNS resolves but an application is unreachable?"

You can answer:

> First, I verify DNS resolution and confirm the returned IP address. Then I test network connectivity and the required application port. If the port is unreachable, I check routing, NSG rules, firewall settings and whether the application is listening. I isolate the issue layer by layer rather than immediately changing DNS.

This is a strong support-engineer troubleshooting approach.

## **Part 9 — AZ-900 exam revision**

| Exam question wording                                              | Correct concept           |
| ------------------------------------------------------------------ | ------------------------- |
| Host DNS domains and records in Azure                              | Azure DNS                 |
| Connect an office network to Azure through an encrypted VPN tunnel | Site-to-Site VPN Gateway  |
| Connect an individual laptop to Azure through VPN                  | Point-to-Site VPN Gateway |
| Private connectivity through a connectivity provider               | ExpressRoute              |
| Connect two VNets privately                                        | VNet Peering              |
| Access a supported Azure service through a private IP in a VNet    | Private Endpoint          |
| Resolve names within linked VNets                                  | Azure Private DNS         |

### **Quick memory map**

**DNS**

Name → IP

**VPN Gateway**

Encrypted VPN

**ExpressRoute**

Private provider connectivity

**Private Endpoint**

Private service access

## **Part 10 — Day 11 quiz**

**AZ-900 Day 11 quiz**

8 questions

**1. What does DNS do?**

Encrypts network traffic

Resolves domain names to IP addresses

Creates virtual machines



**2. Which service hosts DNS domains and records in Azure?**

Azure DNS

Azure VPN Gateway

Azure ExpressRoute



**3. Which VPN connection is typically used to connect an entire office network to Azure?**

Point-to-Site

Site-to-Site

VNet Peering



**4. Which VPN type connects an individual laptop to an Azure VNet?**

Point-to-Site

Site-to-Site

ExpressRoute



**5. Which service provides private connectivity through a connectivity provider?**

Azure DNS

ExpressRoute

NSG



**6. What does a private endpoint provide?**

A public IP for every VM

A private IP in a VNet to access a supported Azure service

A DNS server for the internet



**7. DNS resolves a name successfully, but TCP 443 fails. What should you check?**

Only change the DNS record

Routing, firewalls, NSGs and application listener

Delete the VNet



**8. Which command tests TCP connectivity to a specific port in Windows PowerShell?**

ipconfig /all

nslookup

Test-NetConnection



Check my answers

## **Part 11 — GitHub documentation**

Repository: `AZ900-Certificate-Course`

Create today's folder:

**Day-11-Azure-DNS-VPN-and-Endpoints/**

Day-11-Azure-DNS-VPN-and-Endpoints/\
├── README.md\
├── notes/\
│   ├── 01-Azure-DNS.md\
│   ├── 02-Public-vs-Private-DNS.md\
│   ├── 03-VPN-Gateway-S2S-vs-P2S.md\
│   ├── 04-ExpressRoute.md\
│   └── 05-Public-vs-Private-Endpoints.md\
├── practical-labs/\
│   ├── Lab-01-DNS-Troubleshooting-Windows.md\
│   ├── Lab-02-Create-DNS-Record-LAB-DC01.md\
│   └── Lab-03-Explore-Azure-Networking-Services.md\
├── screenshots/\
│   ├── Day-11-01-DNS-Resolution.png\
│   ├── Day-11-02-LAB-DC01-DNS-Record.png\
│   └── Day-11-03-Azure-Networking-Services.png\
└── quiz/\
&#x20;   └── Day-11-AZ900-Quiz.md

Copy folder structure

**README title:** `Day 11: Azure DNS, VPN Gateway, ExpressRoute and Endpoints`

**LinkedIn headline:** `AZ-900 Day 11 | Practised DNS Troubleshooting and Studied Azure VPN Gateway, ExpressRoute and Private Endpoints`

**Short social headline:** `AZ-900 Day 11 — Azure Networking Beyond the VNet`

**Evidence to upload:**

- `nslookup` output.
- `Resolve-DnsName` output.
- DNS record created in your own Windows Server lab, if completed.
- Azure Portal screenshots showing services you explored.
- Your written comparison of VPN Gateway and ExpressRoute.
- Quiz score.

## **Day 11 completion checklist**

**Learning progress**

# **0/9**

Understand Azure DNS and DNS records

Practise nslookup and Resolve-DnsName

Understand public DNS vs private DNS

Create and test a local DNS A record (if lab DNS is available)

Understand S2S vs P2S VPN

Understand ExpressRoute

Understand public vs private endpoints

Explore Azure Portal networking services without deploying costly resources

Complete quiz and document today's work

**Your main practical task today:** Create and test a DNS A record in your Windows Server lab, then troubleshoot name resolution from your client. This is useful experience for both AZ-900 and real-world Active Directory / IT support.

Next: **Day 12 — Azure Network Security and Connectivity Troubleshooting**, where we'll practise reading network rules, identifying blocked traffic and building a systematic troubleshooting workflow.