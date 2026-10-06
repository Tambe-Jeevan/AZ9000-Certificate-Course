# AZ-900 — Day 06

## Azure Core Architecture: Resources, Resource Groups, Subscriptions & Management Groups

Day 5 taught you **where Azure infrastructure exists** — Regions, Availability Zones, and Region Pairs.

Today we answer a different question:

> **How does Azure organize and manage everything we create?**

This is one of the most important AZ-900 foundations because almost every Azure service you use eventually becomes an **Azure resource**.

---

# 🎯 Day 6 Goal

By the end of today, you should clearly understand:

* Azure Resource
* Resource Group
* Azure Subscription
* Management Group
* Azure Resource Manager (ARM)
* Azure hierarchy
* Resource Group vs Subscription
* Subscription vs Management Group
* Why resources belong to resource groups
* Why companies use multiple subscriptions
* How permissions and policies fit into this hierarchy
* How to explore this structure in Azure Portal
* Basic Azure CLI commands for viewing resources

Our mental model today:

```text
Management Group
       ↓
   Subscription
       ↓
 Resource Group
       ↓
    Resource
```

---

# Part 1 — Start From Zero

Imagine you join an IT company.

The company decides to move its infrastructure to Azure.

They need:

```text
2 Virtual Machines
1 Database
1 Storage Account
1 Virtual Network
1 Public IP
1 Network Security Group
```

These are all different Azure services/resources.

If Azure simply gave you a huge list containing thousands of resources, management would become difficult.

So Azure provides a hierarchy.

---

# Part 2 — What Is an Azure Resource?

A **resource** is an individual item that you create/manage in Azure.

Examples:

```text
Virtual Machine
Storage Account
Virtual Network
Database
Public IP Address
Network Interface
Load Balancer
Key Vault
```

Think of it like physical IT infrastructure.

Traditional environment:

```text
Physical Server
     │
     ├── IP Address
     ├── Storage
     └── Network
```

Azure:

```text
Azure
 │
 ├── Virtual Machine
 ├── Storage Account
 ├── VNet
 ├── Public IP
 └── Database
```

Each item is an Azure **resource**.

---

# Part 3 — Real-World Example

Suppose your company has an application called:

**Employee Portal**

The application needs:

```text
Employee Portal
       │
       ├── Web VM
       ├── Database
       ├── Storage
       ├── Virtual Network
       └── Public IP
```

In Azure, these become resources.

So:

```text
Web VM              → Resource
Database            → Resource
Storage Account     → Resource
Virtual Network     → Resource
Public IP           → Resource
```

---

# Part 4 — Why Do We Need Resource Groups?

Imagine you have 500 Azure resources.

If everything is kept in one giant list:

```text
VM-001
VM-002
Storage-001
DB-001
VNet-001
...
500 resources
```

Management becomes difficult.

Azure provides:

## Resource Group

A **Resource Group** is a logical container used to organize and manage related Azure resources.

Think of it like a folder.

```text
Resource Group
      │
      ├── VM
      ├── Database
      ├── Storage
      ├── VNet
      └── Public IP
```

---

# Part 5 — Very Important: Resource Group Is Not a Folder on Disk

Don't think:

```text
C:\ResourceGroup
```

❌ Wrong.

A Resource Group is an **Azure management container**.

It helps with:

* Organization
* Access control
* Policies
* Monitoring
* Resource lifecycle management
* Cost organization

---

# Part 6 — Real Company Example

Suppose your company has:

### Production application

```text
RG-Production
    │
    ├── Web VM
    ├── App VM
    ├── Database
    ├── Storage
    └── Network
```

### Development application

```text
RG-Development
    │
    ├── Dev VM
    ├── Dev Database
    └── Dev Storage
```

Now you can easily distinguish:

```text
Production
       ≠
Development
```

This is much easier to manage.

---

# Part 7 — Resource Group Naming

Companies normally use meaningful names.

For example:

```text
rg-prod-employeeportal
rg-dev-employeeportal
rg-test-employeeportal
```

Instead of:

```text
group123
abcgroup
testxyz
```

Good naming becomes extremely important when you have hundreds or thousands of resources.

---

# Part 8 — Important Rule

A resource belongs to **one resource group at a time**.

But a resource group can contain **many resources**.

Think:

```text
1 Resource Group
       │
       ├── Resource
       ├── Resource
       ├── Resource
       └── Resource
```

---

# Part 9 — What Is an Azure Subscription?

Now we move one level upward.

A **subscription** is an Azure management and billing boundary.

It is associated with:

* Billing
* Resource access
* Quotas/limits
* Resource organization
* Access control

Think of it as a major administrative boundary for Azure resources.

---

# Part 10 — Simple Real-World Analogy

Imagine a company.

```text
Company
   │
   ├── Production Department
   ├── Development Department
   └── Testing Department
```

Now imagine Azure:

```text
Subscription
   │
   ├── Production Resource Group
   ├── Development Resource Group
   └── Testing Resource Group
```

Each Resource Group then contains resources.

---

# Part 11 — Subscription vs Resource Group

This is one of today's most important comparisons.

| Resource Group                  | Subscription                             |
| ------------------------------- | ---------------------------------------- |
| Logical container for resources | Larger management/billing boundary       |
| Contains resources              | Contains resource groups                 |
| Helps organize workloads        | Helps separate/manage Azure environments |
| Resources are deployed into it  | Resource groups exist inside it          |
| More granular organization      | Higher-level boundary                    |

Mental model:

```text
Subscription
      │
      ├── Resource Group A
      │       ├── VM
      │       └── Storage
      │
      └── Resource Group B
              ├── Database
              └── VNet
```

---

# Part 12 — What Is a Management Group?

Now we go one level higher.

Imagine a very large organization.

It has:

```text
Company
 │
 ├── Production Subscription
 ├── Development Subscription
 ├── Testing Subscription
 ├── Finance Subscription
 └── HR Subscription
```

Managing policies separately for every subscription can become difficult.

Azure provides:

## Management Groups

Management Groups allow organizations to organize multiple Azure subscriptions into a hierarchy.

Conceptually:

```text
Management Group
       │
       ├── Subscription A
       ├── Subscription B
       └── Subscription C
```

---

# Part 13 — Complete Azure Hierarchy

Now combine everything you've learned.

```text
                Management Group
                       │
              ┌────────┴────────┐
              ▼                 ▼
        Subscription A    Subscription B
              │
       ┌──────┴──────┐
       ▼             ▼
 Resource Group  Resource Group
       │             │
    ┌──┼──┐        ┌─┼──┐
    ▼  ▼  ▼        ▼ ▼  ▼
   VM DB VNet      VM DB Storage
```

### Memorize:

> **Management Group → Subscription → Resource Group → Resource**

This hierarchy is extremely important for AZ-900.

---

# Part 14 — Why Does Microsoft Need This Hierarchy?

Imagine a multinational company.

It has:

```text
10 Azure subscriptions
500 resource groups
10,000 resources
```

Administrators need to answer questions like:

* Who can access these resources?
* Which resources belong to production?
* Which subscription is for development?
* What security policy should apply?
* How do we manage costs?
* Which resources should be monitored?
* What compliance rules should apply?

The hierarchy helps organize this environment.

---

# Part 15 — Azure Resource Manager (ARM)

Now another important term.

## Azure Resource Manager

Usually called:

**ARM**

Azure Resource Manager is Azure's management layer for deploying and managing resources.

You interact with Azure resources through things such as:

```text
Azure Portal
Azure CLI
PowerShell
ARM templates
Bicep
APIs
```

Conceptually:

```text
             You
              │
      ┌───────┼────────┐
      ▼       ▼        ▼
   Portal    CLI    PowerShell
      │       │        │
      └───────┼────────┘
              ▼
      Azure Resource Manager
              │
      ┌───────┼───────────┐
      ▼       ▼           ▼
      VM     VNet       Storage
```

---

# Part 16 — Why Is ARM Important?

Suppose you create a VM.

Azure needs to manage:

* VM configuration
* Network interface
* Public IP
* Disk
* Permissions
* Resource group
* Policies
* Tags
* Deployment

ARM provides the management layer through which Azure resources are deployed and managed.

For AZ-900, remember:

> **Azure Resource Manager provides the management layer for Azure resources.**

---

# Part 17 — Azure Portal Is Not Azure

This is an important conceptual distinction.

You may think:

> "Azure = Azure Portal."

Not exactly.

The Portal is simply a **management interface**.

You can manage Azure using:

```text
Azure Portal
Azure CLI
Azure PowerShell
APIs
Infrastructure-as-Code tools
```

All of these ultimately interact with Azure's management capabilities.

---

# Part 18 — Real-World SysAdmin Example

Imagine your company gives you this requirement:

> "Create a production VM and allow only the infrastructure team to manage it."

You might create:

```text
Subscription
     │
     ▼
rg-prod-infrastructure
     │
     ├── VM
     ├── VNet
     ├── NIC
     ├── Disk
     └── Public IP
```

Then permissions can be managed around the appropriate scope.

Later we'll study:

**Azure RBAC**

which controls who can perform which actions.

---

# Part 19 — Scope

Today's hierarchy becomes extremely important when we later learn permissions.

For example, permissions can be assigned at different scopes.

Conceptually:

```text
Management Group
      ↓
Subscription
      ↓
Resource Group
      ↓
Resource
```

A permission assigned at a higher scope can potentially affect lower levels, depending on the role and configuration.

Don't worry about memorizing RBAC details yet.

We'll study it properly later.

---

# Part 20 — Tags

Another useful concept.

Azure resources can have **tags**.

A tag is metadata attached to a resource.

Example:

```text
Environment = Production
Department  = IT
Owner        = Infrastructure
Project      = EmployeePortal
CostCenter   = CC1001
```

Example:

```text
VM
 │
 ├── Environment = Production
 ├── Owner = IT
 └── Project = EmployeePortal
```

Tags help organizations identify and organize resources.

We'll later connect this with **cost management and governance**.

---

# Part 21 — Practical Lab: Azure Portal

Now let's do today's important hands-on work.

## Step 1 — Open Azure Portal

Go to:

**portal.azure.com**

---

## Step 2 — Find Resource Groups

Search:

```text
Resource groups
```

Open it.

You should see your available resource groups.

If you don't have any, that's completely fine.

---

# Step 3 — Understand the Resource Group Page

Look for things such as:

* Subscription
* Region
* Resources
* Tags
* Deployments
* Access control

Don't change anything yet.

We are learning the interface first.

---

# Step 4 — Create a Resource Group

If your Azure account allows resource creation and you want to practice, create a small empty resource group.

Use:

```text
Resource group:
rg-az900-day06
```

Choose an appropriate region.

Then review and create.

### Important

A resource group itself doesn't mean you should start creating expensive resources.

Today we're primarily practicing **organization and management**.

---

# Step 5 — Open Your Resource Group

After creation:

```text
Resource groups
      ↓
rg-az900-day06
```

You should see the resource group overview.

At this point:

```text
Subscription
      │
      ▼
rg-az900-day06
      │
      └── Currently empty
```

That's okay.

---

# Step 6 — Explore Subscription

Search:

```text
Subscriptions
```

Open the subscription you are using.

Observe:

* Subscription ID/name
* Subscription state
* Resource groups
* Cost-related information
* Access control

Don't change permissions or billing settings.

---

# Step 7 — Find Management Groups

Search:

```text
Management groups
```

If your account doesn't have management groups configured or you don't have permission, don't worry.

The goal is to understand the hierarchy.

---

# Part 22 — Practical Exercise

Draw this yourself:

```text
                    Management Group
                           │
                           ▼
                    Subscription
                           │
                           ▼
                   Resource Group
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
             VM           VNet       Storage
```

Then write underneath:

```text
Management Group = organize subscriptions

Subscription = management/billing boundary

Resource Group = logical container for resources

Resource = actual Azure service/item
```

---

# Part 23 — Azure CLI Practical

If you have **Azure Cloud Shell**, you can practice without installing anything.

Open:

**Azure Portal → Cloud Shell**

Choose:

**Bash**

Then:

```bash
az account show
```

This displays information about your current Azure subscription context.

---

### List resource groups

```bash
az group list --output table
```

You should see your resource groups.

---

### Get a specific resource group

```bash
az group show --name rg-az900-day06
```

If you created the group with that name.

---

### List subscriptions

```bash
az account list --output table
```

This is useful for understanding the subscription context.

---

# Part 24 — PowerShell Alternative

If you're using Azure PowerShell:

```powershell
Get-AzContext
```

This shows your current Azure context.

List resource groups:

```powershell
Get-AzResourceGroup
```

Get one resource group:

```powershell
Get-AzResourceGroup -Name "rg-az900-day06"
```

Don't worry if you haven't installed Azure PowerShell locally.

Cloud Shell is sufficient for today's practice.

---

# Part 25 — Important Troubleshooting

### Problem 1

You run:

```bash
az group list
```

and receive an authentication/login error.

Possible solution:

```bash
az login
```

In Cloud Shell, you are normally already authenticated through the portal context, but authentication state can vary.

---

### Problem 2

You cannot create a resource group.

Possible reasons include:

* Insufficient permissions
* Subscription restrictions
* Account restrictions
* Subscription disabled/expired

Don't immediately assume the Azure service is broken.

Think:

```text
Authentication
      ↓
Authorization
      ↓
Subscription
      ↓
Resource creation
```

We'll study authentication and authorization in depth later.

---

# Part 26 — Important Real-World Scenario

Imagine:

```text
Company ABC
```

They have:

### Subscription 1

```text
Production
```

### Subscription 2

```text
Development
```

### Subscription 3

```text
Testing
```

Management group:

```text
ABC-Cloud
   │
   ├── Production Subscription
   ├── Development Subscription
   └── Testing Subscription
```

Production subscription:

```text
Production Subscription
        │
        ├── rg-prod-web
        │      ├── VM
        │      └── VNet
        │
        └── rg-prod-data
               ├── Database
               └── Storage
```

Now the environment is much easier to manage.

---

# Part 27 — A Very Important Question

### Can a Resource Group contain resources from different subscriptions?

**No.**

A Resource Group belongs to a single subscription.

Think:

```text
Subscription A
   │
   ├── RG-A
   │     ├── VM
   │     └── Storage
   │
   └── RG-B
         └── VNet
```

You cannot have:

```text
RG-A
 ├── Resource from Subscription A
 └── Resource from Subscription B
```

❌ Not how Azure Resource Groups work.

---

# Part 28 — Can a Resource Group contain resources from different regions?

This is a more interesting question.

A Resource Group is primarily a management container, not a physical datacenter.

Resources in a resource group can be associated with different regions, depending on the service.

For example, conceptually:

```text
rg-company-app
     │
     ├── VM → Central India
     ├── Storage → West India
     └── Other resource → another region
```

However, **resource-group location** itself is also stored as part of the resource group's metadata.

Don't confuse:

> Resource Group's location

with:

> The physical deployment location of every resource inside it.

That's a common beginner mistake.

---

# Part 29 — Why This Matters for You as a SysAdmin

Later, when you're working as a Cloud/System Administrator, you may receive a request:

> "Give the application team access to the production application resources, but don't give them access to the entire subscription."

Now you need to understand:

```text
Subscription
     │
     └── Resource Group
             │
             └── Application Resources
```

Then RBAC can be applied at the appropriate scope.

That's why today's hierarchy is not just an AZ-900 exam topic.

It is real cloud administration knowledge.

---

# 🧠 Day 6 Master Diagram

You should be able to draw this **without looking**:

```text
                         AZURE
                           │
                    Management Group
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
       Subscription A              Subscription B
             │
       ┌─────┴─────┐
       ▼           ▼
   Resource     Resource
    Group A      Group B
       │           │
    ┌──┼──┐      ┌─┼──┐
    ▼  ▼  ▼      ▼ ▼  ▼
   VM VNet IP    DB VM Storage
```

And remember:

```text
Management Group
       ↓
Subscription
       ↓
Resource Group
       ↓
Resource
```

---

# 🔥 AZ-900 Exam Focus

These are the points I want you to know today.

### Resource

An individual Azure service/item.

### Resource Group

A logical container for Azure resources.

### Subscription

A management and billing boundary.

### Management Group

A way to organize multiple Azure subscriptions into a hierarchy.

### Azure Resource Manager

Azure's management layer for deploying and managing resources.

### Hierarchy

```text
Management Group
       ↓
Subscription
       ↓
Resource Group
       ↓
Resource
```

---

# 🎯 Interview Questions

### Q1. What is an Azure Resource Group?

A logical container used to organize and manage related Azure resources.

### Q2. What is an Azure Subscription?

A management and billing boundary for Azure resources.

### Q3. What is a Management Group?

A hierarchy used to organize multiple Azure subscriptions.

### Q4. What is Azure Resource Manager?

The management layer used to deploy and manage Azure resources.

### Q5. Can one Resource Group belong to multiple subscriptions?

**No.**

### Q6. Can one subscription have multiple Resource Groups?

**Yes.**

### Q7. Can a Resource Group contain multiple resources?

**Yes.**

### Q8. Why use multiple subscriptions?

Organizations may use multiple subscriptions for reasons such as environment separation, organizational boundaries, governance, billing, access management, and quotas.

### Q9. Is Azure Portal the only way to manage Azure?

**No.**

You can also use Azure CLI, PowerShell, APIs, and infrastructure-as-code approaches.

### Q10. What is the Azure hierarchy?

**Management Group → Subscription → Resource Group → Resource**

---

# 🧪 Day 6 Quiz

Don't look back while answering.

### 1.

What is an Azure resource?

### 2.

Give five examples of Azure resources.

### 3.

What is a Resource Group?

### 4.

Is a Resource Group the same thing as a physical folder?

### 5.

What is an Azure subscription?

### 6.

What is a Management Group?

### 7.

Write the complete Azure hierarchy.

### 8.

What is Azure Resource Manager?

### 9.

Can one Resource Group contain resources from two different subscriptions?

### 10.

Why would a large company use multiple subscriptions?

### 11.

What is the difference between Resource Group and Subscription?

### 12.

Why are tags useful?

### 13.

Can Azure resources be managed without using the Azure Portal?

### 14.

What command lists Azure Resource Groups using Azure CLI?

### 15.

What command shows your current Azure CLI account/subscription context?

---

# ✅ Day 6 Completion Checklist

Before moving ahead, make sure you can explain these in your own words:

* [ ] Resource
* [ ] Resource Group
* [ ] Subscription
* [ ] Management Group
* [ ] Azure Resource Manager
* [ ] Azure hierarchy
* [ ] Resource Group vs Subscription
* [ ] Subscription vs Management Group
* [ ] Resource lifecycle
* [ ] Tags
* [ ] Azure Portal
* [ ] Azure CLI
* [ ] Cloud Shell
* [ ] Basic `az group` commands
* [ ] Why enterprises use multiple subscriptions

---

# 📂 Day 6 Documentation

Following your daily documentation format:

### GitHub folder

```text
Day-06-Azure-Core-Architecture/
```

### Structure

```text
AZ900-Certificate-Course/
└── Day-06-Azure-Core-Architecture/
    ├── README.md
    ├── Notes/
    ├── Practical-Labs/
    ├── Commands/
    └── Screenshots/
```

### README title

```text
# Day 06 — Azure Core Architecture: Resources, Resource Groups, Subscriptions and Management Groups
```

### LinkedIn headline

```text
AZ-900 Day 06: Azure Core Architecture — Resources, Resource Groups, Subscriptions, Management Groups & ARM
```

### Short social headline

```text
AZ-900 Day 06 🚀 | Azure Resource Hierarchy + Resource Groups + Subscriptions + ARM
```

### Practical lab

```text
Day 06 Practical Lab — Azure Resource Group, Subscription and Resource Management
```

### Screenshot/file names

```text
01-resource-groups-page.png
02-resource-group-created.png
03-resource-group-overview.png
04-subscription-overview.png
05-management-groups-page.png
06-azure-hierarchy-diagram.png
07-azure-cli-account-show.png
08-azure-cli-resource-groups.png
09-azure-cli-subscription-list.png
10-day06-resource-management-notes.png
```

### Evidence to upload

Capture:

1. Resource Groups page
2. Your `rg-az900-day06` resource group
3. Resource Group overview
4. Subscription overview
5. Management Groups page/concept
6. Your Azure hierarchy diagram
7. `az account show`
8. `az group list --output table`
9. Your Day 6 quiz answers

---

## 🔗 How Day 5 + Day 6 Connect

You now have two very important Azure dimensions:

**Day 5 — Where?**

```text
Azure
 └── Region
      └── Availability Zone
```

**Day 6 — How is it organized?**

```text
Management Group
       ↓
Subscription
       ↓
Resource Group
       ↓
Resource
```

Put them together:

```text
                 AZURE
                   │
        ┌──────────┴──────────┐
        │                     │
   GLOBAL LOCATION       MANAGEMENT
        │                     │
     Region             Management Group
        │                     │
      Zone              Subscription
                              │
                        Resource Group
                              │
                           Resource
```

**This is the foundation.** From here, we can start building actual Azure services instead of just learning isolated definitions.

And remember our rule for this journey: **we are not racing to finish 60 days.** If a topic needs more practical work, we'll spend more time on it.
