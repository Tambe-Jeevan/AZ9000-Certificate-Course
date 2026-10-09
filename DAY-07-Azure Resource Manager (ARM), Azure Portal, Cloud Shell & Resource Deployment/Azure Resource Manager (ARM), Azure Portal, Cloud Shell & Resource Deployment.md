# AZ-900 — Day 07

## Azure Resource Manager (ARM), Azure Portal, Cloud Shell & Resource Deployment

Over the last few days, you've learned the foundations of Azure:

* Day 1: Computer, server, virtualization and cloud basics

* Day 2: Networking fundamentals, IP addresses, ports and TCP/UDP

* Day 3: Subnetting and Azure Virtual Networks

* Day 4: DNS and Azure DNS

* Day 5: Regions, Availability Zones and disaster recovery

* Day 6: Resources, Resource Groups, Subscriptions and Management Groups

Today, we'll connect those concepts and start thinking like a junior Azure administrator.

Our main question is:

> When you click Create in the Azure Portal, what happens behind the scenes—and how can you manage Azure without clicking through the Portal?

Today's focus is practical. We'll explore Azure's management interface, use Cloud Shell and CLI commands, and understand how resource deployment works without rushing into expensive services.

## Today's learning targets

By the end of this lesson, you should be able to:

1. Explain what Azure Resource Manager (ARM) does.

2. Distinguish the Azure Portal, Azure CLI and Azure PowerShell.

3. Explain what a deployment is.

4. Understand the difference between the control plane and data plane.

5. Use Azure Cloud Shell to inspect your account and resources.

6. Read basic Azure CLI commands.

7. Troubleshoot common resource-deployment errors.

8. Document your lab work in your GitHub portfolio.

## Part 1 — What is Azure Resource Manager (ARM)?

Azure Resource Manager (ARM) is the management service that Azure uses to create, update, delete, and organize resources. Requests made through supported Azure tools are handled through this management layer, which checks authorization and routes requests to the relevant Azure service.

![](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn

+1

Imagine you want to create a virtual machine.

```
You
 │
 ▼
Choose settings in Azure Portal
 │
 ▼
Azure Resource Manager (ARM)
 │
 ├── Checks permissions
 ├── Processes the deployment request
 └── Routes the request to the appropriate service
             │
             ▼
       Virtual Machine
```

ARM isn't the virtual machine itself. It is the management layer used to request and coordinate the deployment.

### Example from a real IT environment

Your manager asks you to create a test server for an application.

You specify:

* Name: `vm-test-01`

* Resource Group: `rg-test`

* Region: `Central India`

* Operating system: Windows Server

* Network: a suitable Azure Virtual Network

You submit the configuration. Azure processes the deployment request and creates the resource if the configuration, permissions, quotas and service requirements allow it.

Why this matters: You can manage Azure resources through the Portal, CLI, PowerShell, APIs or deployment templates rather than being limited to one interface.

![](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn

## Part 2 — Azure Portal vs CLI vs PowerShell

These are different ways of managing Azure.

![Describe core Azure services | Microsoft Press Store](https://images.openai.com/static-rsc-4/ePVBS7Las0LPa49NVmJKf8DHvpVro9gLcnkP1ak9upAEgk-FO7an9BHhIAXPcPOWvse6zFeJH4Pqcp2OpqtRGWY9aDr0LHNkGz_pCOc5KyEupdeOBcTNL9N1Y4nvh9TYuLR1rjZE-3ct7aNK1VMPHpYIqizgNCnQSYYsM12lm7g?purpose=inline)

1. Azure Portal

A graphical interface. You click menus, buttons and forms.

Best for: beginners, visual exploration and quick administration.

![GitHub - wwce/azure-tf-virtual-wan: Creates full environment to experiment and demo the VM-Series and Azure Virtual WAN. · GitHub](https://images.openai.com/static-rsc-4/efJo8ZPWxkq6qUUV_eX7bi8xCHF7uZqjQC2m0ogGzzELlby-UNnPlJy02gmcbuqp27G2dVeTVIm9e_kpqfubpd7WI3uUJnAO5HPvflqjG1PXxDDmN9zkJaQeDBbbZkM2aQNlS9O3WFgNQMnSnnCFn-nnpekM3WXVTap_gx8hG4E?purpose=inline)

2. Azure CLI

A command-line tool that uses commands beginning with `az`.

Best for: repeatable commands, scripts and automation.

![How to delete all resource groups using PowerShell Command Script in Azure? - R Hari Krishna - Medium](https://images.openai.com/static-rsc-4/Zmk3kMS5coB0la1DBbDaW_OIY5E9QVx9LpXo-t4KkOkcOm26ajAca_9a73oCipjVdBO1kQjHJI1F38urENEEqh5E_eSDxix4vMSvsQIfWT9wvnYQyGtOr0l0ylg7EzjFcG5orbcrfkB8kkDTjK9L8_hQw1SO6irYlEN5igBWRu0?purpose=inline)

3. Azure PowerShell

Azure management commands used in PowerShell, commonly with the `Az` modules.

Best for: PowerShell-based administration and automation.

For example, to list resource groups:

Bash

```
az group list --output table
```

The equivalent PowerShell command is:

PowerShell

```
Get-AzResourceGroup
```

Both can retrieve resource-group information, but they use different command syntax.

## Part 3 — Control Plane vs Data Plane

This is one of today's most useful concepts for a future Azure or System Administrator.

### 1. Control plane

The control plane is used to manage the Azure resource itself.

Examples:

* Create a VM.

* Change a VM's configuration.

* Create a storage account.

* Assign permissions.

* Delete a resource.

### 2. Data plane

The data plane is used to work with the service or the data it provides.

Examples:

* Connect to a VM using RDP.

* Upload a file to Azure Blob Storage.

* Download a file from a storage account.

* Read data from a database.

Microsoft documents this distinction in its

learn.microsoft.com

.

![](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn

| Task                            | Plane         |
| ------------------------------- | ------------- |
| Create a virtual machine        | Control plane |
| Change a VM's configuration     | Control plane |
| Connect to the VM using RDP     | Data plane    |
| Create a storage account        | Control plane |
| Upload a file into Blob Storage | Data plane    |
| Read a file from Blob Storage   | Data plane    |

### Remember this

Control plane = manage the resource.

Data plane = use the resource.

This is a conceptual distinction; exact operations depend on the Azure service and the API being used.

## Part 4 — What Is a Deployment?

A deployment means putting a resource or a collection of resources into place using a specified configuration.

Suppose your company needs a web application with a virtual machine and a network.

```
Application deployment
        │
        ├── Virtual Network
        ├── Network Interface
        ├── Virtual Machine
        └── Supporting resources
```

Instead of treating each item as unrelated, Azure deployment tools can manage the resources and their dependencies in a coordinated way.

![](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn

### Two ways to deploy

Method A — Manual deployment

You open the Portal and fill out the forms.

Method B — Automated deployment

You use commands, scripts or templates to describe and deploy the resources.

For example, a team may need the same test environment every week. Automation can make deployments more repeatable and reduce manual mistakes.

Later in your learning journey, we'll explore infrastructure as code (IaC), including ARM templates and Bicep. Today, focus on understanding the purpose rather than memorizing template syntax.

## Part 5 — Your Hands-on Lab

## Lab 1: Inspect Azure without creating expensive resources

Estimated time: 20–30 minutes · Beginner · Read-only commands

You'll inspect your current subscription, resource groups and resources. This is safe to practice because these commands only retrieve information.

Step 1 — Open Azure Cloud Shell

1. Sign in to the

   portal.azure.com

   .

2. Select the Cloud Shell icon (`>_`) near the top.

3. Choose Bash if asked.

4. If prompted for storage to persist Cloud Shell files, review the setup and any associated costs before proceeding. You can skip this lab if you don't want to configure it.

Cloud Shell provides a browser-based command-line environment; Azure CLI is available in its Bash environment.

Step 2 — Inspect your current account context

Bash

```
az account show --output table
```

Observe the subscription name, subscription ID, tenant information and state shown by the command.

Step 3 — List your subscriptions

Bash

```
az account list --output table
```

If you only see one subscription, that's fine. Don't create additional subscriptions just for this exercise.

Step 4 — List resource groups

Bash

```
az group list --output table
```

If you have no resource groups, the command may return an empty result.

Step 5 — List resources

Bash

```
az resource list --output table
```

This lists resources visible in your selected subscription context.

Step 6 — Inspect one resource group

If you already have a resource group, copy its exact name and run:

Bash

```
az group show --name "YOUR-RESOURCE-GROUP"
```

Replace the placeholder with your actual resource-group name.

Safety rule

For today's lab, avoid commands that create, modify or delete resources. You don't need a VM or paid service to learn the management basics.

The commands above use documented Azure CLI resource-management operations.

![](https://www.google.com/s2/favicons?domain=https://learn.microsoft.com\&sz=32)

Microsoft Learn

## Part 6 — Understand the output

Let's say your command returns something like this:

```
Name              Location
----------------  -------------
rg-test           centralindia
rg-learning       westindia
```

This is an illustrative example, not the output from your account.

What does it tell you?

* `Name` is the resource-group name.

* `Location` is the resource group's recorded location.

* The command is showing resource-group metadata, not proving that every resource inside each group is physically deployed in that location.

Now suppose you see:

```
Name          ResourceGroup    Location
------------  ---------------  -------------
vm-test-01    rg-test          centralindia
```

That resource listing identifies a VM and its associated resource group and location. If you see different results, use your actual output to learn from your own environment.

## Part 7 — Troubleshooting common problems

Problem 1: `az: command not found`

Possible cause: Azure CLI isn't available in the terminal you're using.

What to do: Confirm you're in Azure Cloud Shell's Bash environment. If you're using your own Windows terminal, Azure CLI may need to be installed separately.

Problem 2: Authentication or subscription error

Possible cause: You're not signed in, the subscription context is unavailable, or your account has restrictions.

What to do: Try:

Bash

```
az account show
```

If needed, sign in with `az login` in an appropriate local environment, then check your subscription context again.

Problem 3: Resource group list is empty

Possible cause: There are no resource groups in the selected subscription, or you lack permission to view them.

What to do: Run `az account show`, verify the subscription, and check your access. An empty list doesn't automatically mean Azure is broken.

Problem 4: Resource creation is denied

Possible cause: Insufficient permissions, subscription restrictions, a policy restriction or a quota/service limitation.

What to do: Check the exact error message, selected subscription, permissions and applicable policies before retrying. Don't repeatedly run a failed deployment without understanding the error.

## Part 8 — Mini scenario: A company deployment fails

Imagine your manager asks you to deploy a test VM, but Azure returns an error.

Use this troubleshooting sequence:

```
Deployment failed
       │
       ▼
Read the exact error
       │
       ▼
Check the selected subscription
       │
       ▼
Check permissions
       │
       ▼
Check region and service availability
       │
       ▼
Check quotas and policy restrictions
       │
       ▼
Correct the cause and retry
```

Real-world lesson: Don't assume every deployment failure is a network issue. Cloud troubleshooting starts with the actual error and the layer where the request failed.

## Part 9 — Quick knowledge check

## Day 7 quiz

0 of 8 answered

1. What is Azure Resource Manager?

A physical Azure server

Azure's deployment and management service

A Windows operating system

2. Which tool uses commands beginning with `az`?

Azure CLI

Azure PowerShell

Windows Device Manager

3. Creating a virtual machine is usually which plane?

Data plane

Control plane

Presentation plane

4. Uploading a file to Blob Storage is usually which plane?

Control plane

Data plane

Management group

5. Which command lists resource groups?

az vm delete

az group list --output table

ipconfig /all

6. Does using Azure Portal mean CLI cannot manage the same resources?

Yes

No

7. What is a deployment?

Putting resources into place using a configuration

Changing a laptop wallpaper

Only restarting a server

8. What should you check first when a deployment fails?

Delete the subscription

Read the exact error message

Create several more VMs

Check answers

## Part 10 — Day 7 documentation for GitHub

Your repository remains `AZ900-Certificate-Course`. Keep each day's notes, commands, lab and evidence in its own folder.

Folder name

```
Day-07-Azure-Resource-Manager-and-Deployment/
```

Folder structure

```
Day-07-Azure-Resource-Manager-and-Deployment/
├── README.md
├── Notes/
│   └── ARM-Control-Plane-Data-Plane.md
├── Practical-Labs/
│   └── Azure-Cloud-Shell-Exploration.md
├── Commands/
│   └── Azure-CLI-Resource-Management.md
└── Screenshots/
```

README title

```
# Day 07 — Azure Resource Manager and Resource Deployment
```

LinkedIn headline

```
AZ-900 Day 07: Azure Resource Manager (ARM), Control Plane vs Data Plane, Azure CLI & Cloud Shell
```

Short social headline

```
AZ-900 Day 07 🚀 | Azure ARM + Cloud Shell + CLI | Hands-on Resource Management
```

Practical lab title

```
Day 07 Practical Lab — Azure Cloud Shell and Resource Inspection
```

Screenshot/file names

```
01-azure-portal-home.png
02-cloud-shell-open.png
03-az-account-show.png
04-az-account-list.png
05-az-group-list.png
06-az-resource-list.png
07-control-plane-vs-data-plane.png
08-arm-deployment-flow.png
```

Upload only the evidence you actually captured. Don't create fake terminal outputs or screenshots if you haven't performed the lab.

## Day 7 completion checklist

* Explain ARM in your own words.

* Explain Azure Portal vs CLI vs PowerShell.

* Distinguish control plane from data plane.

* Explain what a deployment does.

* Open Cloud Shell, if available.

* Run `az account show`.

* Run `az group list --output table`.

* Run `az resource list --output table`.

* Explain one possible deployment failure and how you'd investigate it.

* Save your notes and real lab evidence to GitHub.

## What comes next?

Day 8 — Azure Compute Fundamentals: Virtual Machines, VM sizing, images, disks and the relationship between a physical server and an Azure VM.

We'll go beyond definitions and work through how to plan a VM deployment, what each setting means, how to avoid unnecessary costs, and how to troubleshoot common VM problems.

Our rule stays the same: learn the concept, practice it, troubleshoot it, explain it, and document it.
