

Below is a **beginner-friendly, end-to-end guide** to understanding, planning, and deploying **Azure Landing Zones (ALZ)**.  
This guide is designed so **you can implement everything directly in Azure** without needing external documentation.

---

# ⭐ **What You Will Learn**

1. **What Azure Landing Zones are (simple explanation)**
2. **Core concepts & building blocks**
3. **Design principles & architecture**
4. **Step-by-step setup in the Azure Portal**
5. **Real examples for each component**
6. **How to deploy an enterprise-ready Azure Landing Zone using Azure Portal or Bicep**

---

# 🟦 **1. What Is an Azure Landing Zone? (Beginner-friendly)**

Think of an **Azure Landing Zone** as a **blueprint for your cloud environment**—a pre-designed structure that ensures:

✔ Security  
✔ Governance  
✔ Networking  
✔ Identity  
✔ Monitoring  
✔ Separation of workloads  
✔ Scalability and cost control

It’s like building the **foundation of a house** before adding rooms inside (your workloads).

---

# 🟧 **2. Why You Need a Landing Zone**

Azure Landing Zones help you avoid:

❌ Uncontrolled sprawl  
❌ Security misconfigurations  
❌ Inconsistent resource structures  
❌ Difficult migrations  
❌ Lack of visibility into costs and logs

Instead, they give you a **repeatable, secure, organized environment** to run apps.

---

# 🟩 **3. Key Components of an Azure Landing Zone**

Below are the core building blocks with simple explanations + examples.

---

## **3.1 Management Groups**

A folder-like structure to group subscriptions.

**Typical structure:**

```
Tenant Root
│
├── Management
├── Platform
│   ├── Connectivity
│   └── Identity
└── LandingZones
    ├── Corp
    └── Online
```

👍 Example:

- You put all production apps under **LandingZones/Corp**
- Your hub network under **Platform/Connectivity**

---

## **3.2 Subscriptions**

Logical containers for resources.

**Common subscription layout:**

- **Identity Subscription** (Azure AD Connect, domain services)
- **Connectivity Subscription** (Hub Virtual Network, Firewalls)
- **Management Subscription** (Log Analytics, Monitor)
- **Landing Zone Subscriptions** (Workloads)

👍 Example:  
Your web app goes into a dedicated **Workload Subscription**.

---

## **3.3 Azure Policies**

Azure Policies enforce rules such as:

- Enforce tagging
- Restrict VM sizes
- Require encryption
- Prevent public IPs

👍 Example:  
A policy prevents anyone from creating a VM without a **costCenter** tag.

---

## **3.4 Role-Based Access Control (RBAC)**

Controls who can do what.

👍 Example:

- Network Admin → “Network Contributor” on Connectivity Subscription
- Security Team → “Security Reader” at the Management Group root
- Dev Team → “Contributor” on workload subscription only

---

## **3.5 Networking (Hub & Spoke)**

Most Landing Zones use:

**Hub = Shared services (Firewall, VPN, Bastion)**  
**Spokes = Workload networks**

```
      Hub
   /   |   \
Web  Data  Apps
```

👍 Example:  
Your App Service and SQL Database sit in **App Spoke**, connected back to the Hub Firewall.

---

## **3.6 Identity (Azure AD / Entra ID)**

Identity is centralized and includes:

- Conditional Access
- MFA
- Privileged Identity Management (PIM)
- Managed Identities

👍 Example:  
A VM uses a **system-assigned managed identity** to access Key Vault.

---

## **3.7 Management & Monitoring**

Includes:

- Log Analytics Workspace
- Azure Monitor
- Alert Rules
- Automation Accounts

👍 Example:  
All activity logs are forwarded to a **Central Log Analytics Workspace** in the Management subscription.

---

# 🟦 **4. Azure Landing Zone Architecture Explained Simply**

Here's the simple architecture you want to build:

```
Tenant Root
│
├── Management Group Structure
│
├── Identity Subscription
│── Azure AD DS, PIM
│
├── Connectivity Subscription
│── Hub VNet, VPN/ExpressRoute, Firewall
│
├── Management Subscription
│── Log Analytics, Automation Accounts, Monitor
│
└── Landing Zone Subscriptions (Workloads)
    ├── Corp Apps
    ├── Online Apps
    └── Shared Services
```

---

# 🟧 **5. Step-by-Step: Deploy an Azure Landing Zone (Portal Only)**

This is the **simplest** method for beginners—no coding.

---

# ⭐ **Step 1 — Deploy Azure Landing Zone Accelerator**

1. Go to Azure Portal
2. Search: **"Azure Landing Zone"**
3. Select: **"Azure Landing Zone Accelerator"**
4. Click **Create**
5. Choose:
    - Management Group: **Tenant Root**
    - Deployment type: **Custom**
6. Create supporting resources:  
    ✔ Management groups  
    ✔ Policies  
    ✔ RBAC roles  
    ✔ Blueprints

Click **Review + Create → Create**

---

# ⭐ **Step 2 — Create the Management Group Structure**

Azure will create this automatically, but here’s how to verify:

1. Go to **Management Groups** in Azure Portal
2. Ensure the following exist:
    - **Management**
    - **Platform**
        - Connectivity
        - Identity
    - **LandingZones**
        - Corp
        - Online

If not, create them manually:

**Steps:**

- Click **Create**
- Name it (e.g. Platform)
- Select parent group

---

# ⭐ **Step 3 — Create the Core Subscriptions**

You need **at least three**:

### ✔ Identity Subscription

### ✔ Connectivity Subscription

### ✔ Management Subscription

**Steps:**

1. Go to **Subscriptions**
2. Click **Create a subscription**
3. Assign each to the correct **Management Group**:
    - Identity → Platform/Identity
    - Connectivity → Platform/Connectivity
    - Management → Management

---

# ⭐ **Step 4 — Build the Hub Virtual Network (Connectivity Subscription)**

1. Go to **Subscriptions → Connectivity**
2. Create a **Virtual Network**
    - Name: `hub-vnet`
    - Address space: `10.0.0.0/16`
3. Add subnets:
    - `AzureFirewallSubnet`
    - `AzureBastionSubnet`
    - `GatewaySubnet`
4. Deploy:
    - Azure Firewall (optional for now)
    - Bastion Host (optional for now)
    - VPN Gateway (if connecting to on-prem)

---

# ⭐ **Step 5 — Build the Shared Monitoring Resources (Management Subscription)**

1. Switch to the **Management Subscription**
2. Create a **Log Analytics Workspace**
3. Create an **Automation Account**
4. Open Azure Policy
5. Assign these policies at the **Tenant Root** or **Management Group Root**:
    - **Audit VMs without monitoring agent**
    - **Enforce tagging rules**
    - **Require encrypted disks**

---

# ⭐ **Step 6 — Create a New Landing Zone (Workload Subscription)**

1. Create a new subscription, e.g. `CorpApp01`
2. Move it under **LandingZones/Corp** MG
3. Deploy a spoke VNet:
    - Name: `corp-spoke-vnet`
    - Address: `10.10.0.0/16`
4. Create a **VNet Peering** to the Hub:
    - Hub → Spoke: Allow forwarded traffic ✔
    - Spoke → Hub: Use remote gateway ✔

---

# ⭐ **Step 7 — Deploy a Simple App in Your Landing Zone**

Example: Azure App Service + Azure SQL Database

### **1. Deploy App Service**

- Go to Your _Corp App_ Subscription
- Create → App Service
- Choose:
    - Runtime: .NET, Node.js, Python (your choice)
    - Plan: Basic or Standard

### **2. Deploy Azure SQL Database**

- Go to Create → SQL Database
- Configure:
    - SQL Server (new)
    - Firewall rules: Deny public access
    - Enable **Private Endpoint** into the spoke VNet

### **3. Connect App to SQL**

Use a **Managed Identity**:

In the App Service → _Identity_ → Enable system-assigned identity.

In SQL → Set identity as **db_datareader/db_datawriter**.

---

# ⭐ **Step 8 — Enable Logging and Monitoring**

1. Go to **Azure Monitor**
2. Configure:
    - Activity Logs → Log Analytics Workspace
    - Metrics → Workspace
3. Turn on:
    - VNet flow logs
    - Firewall logs
    - NSG logs
    - App Service logs

---

# ⭐ **Step 9 — Apply Governance Controls**

At the **Management Group root**, assign Azure Policies:

### Recommended:

- Deny public IP creation
- Enforce private endpoints
- Enforce resource naming convention
- Enforce log collection
- Limit VM SKUs
- Require tags

---

# 🟪 **6. Real-World Examples**

### **Example: Least-Privilege Access**

- A developer gets **Contributor** only on their App Subscription.
- Cannot modify network, identity, or logging.

### **Example: Secure Networking**

- Web apps in a spoke use:
    - Private Endpoints
    - UDRs routing traffic to the Hub Firewall

### **Example: Multi-Region Architecture**

Add another region by:

1. Creating additional spokes
2. Peering to the hub
3. Deploying regional monitoring

---

# 🟫 **7. Azure Landing Zones Using Bicep (Beginner Example)**

Here is a **simple Bicep snippet** that deploys a hub VNet:

```bicep
param location string = resourceGroup().location

resource hubVnet 'Microsoft.Network/virtualNetworks@2023-04-01' = {
  name: 'hub-vnet'
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
    subnets: [
      {
        name: 'AzureFirewallSubnet'
        properties: {
          addressPrefix: '10.0.1.0/24'
        }
      }
      {
        name: 'GatewaySubnet'
        properties: {
          addressPrefix: '10.0.2.0/24'
        }
      }
    ]
  }
}
```

Deploy using:

```
az deployment group create -g MyRG -f main.bicep
```

---

# 🟦 **8. What To Do Next**

After completing this guide, you can:

✔ Add more spokes  
✔ Deploy zero-trust networking policies  
✔ Add CI/CD pipelines for workloads  
✔ Integrate with Defender for Cloud  
✔ Automate policies using Azure DevOps or GitHub

---

# 🎉 **You Now Have a Complete Azure Landing Zone**

This guide gives you everything needed to:

- Understand Azure Landing Zones
- Build the full architecture
- Deploy the environment using Azure Portal
- Add workloads securely
- Apply governance and monitoring

If you'd like, I can also generate:

📌 Architecture diagrams  
📌 Bicep modules for the entire ALZ  
📌 Naming convention templates  
📌 A 30-day learning plan

Just tell me!