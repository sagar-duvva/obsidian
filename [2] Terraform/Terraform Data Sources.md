Below is the **complete, deep-dive, end-to-end explanation** of **Terraform Data Sources**, followed by **15 Azure-specific examples**, and finally a **real-world Azure `main.tf` configuration** using multiple data sources together.

This is a **comprehensive learning package** tailored specifically for **Azure + Terraform**.

---

# 🌍 **Terraform Data Sources — Complete Explanation**

## ✅ **What Is a Data Source?**

A Terraform **data source** is a _read-only_ lookup that lets Terraform **query existing infrastructure or external information**.

A data source **does not create or modify** anything.

It only **retrieves**.

Think of it like:  
🔍 Terraform _resource_: “Create this thing.”  
🔍 Terraform _data source_: “Find me this existing thing.”

---

# 🧠 **Why Data Sources Matter**

Data sources allow you to:

### ✔ 1. Use existing Azure resources

Example: Use an existing VNet created by networking team.

### ✔ 2. Look up dynamic values

Example: Get the latest image version from Azure Marketplace.

### ✔ 3. Reference resources created OUTSIDE Terraform

Example: Use built-in Azure images, pre-existing resource groups, shared subnets.

### ✔ 4. Make modules more flexible

Modules can query the environment dynamically.

---

# ❌ When NOT to Use Data Sources

- If the value never changes → use `locals`
    
- If the value should be passed in by users → use `variable`
    
- If you need to create something → use `resource`
    

---

# 🔧 **How Terraform Evaluates Data Sources**

Evaluation happens during `terraform plan`:

1. Load variables
    
2. Evaluate data sources
    
3. Build dependency graph
    
4. Plan resources
    
5. Apply
    

---

# 📘 **Generic Terraform Data Source Syntax**

```hcl
data "<provider>_<type>" "<name>" {
  # arguments or filters
}

# Use it
data.<provider>_<type>.<name>.attribute
```

---

# 🎯 **15 Azure Data Source Examples (All Practical & Real-World)**

> All examples use provider: `azurerm`.

---

# 1️⃣ **Get an Existing Resource Group**

```hcl
data "azurerm_resource_group" "rg" {
  name = "prod-rg"
}
```

---

# 2️⃣ **Get an Existing Virtual Network**

```hcl
data "azurerm_virtual_network" "vnet" {
  name                = "prod-vnet"
  resource_group_name = "networking-rg"
}
```

---

# 3️⃣ **Get a Specific Subnet**

```hcl
data "azurerm_subnet" "app" {
  name                 = "app-subnet"
  virtual_network_name = "prod-vnet"
  resource_group_name  = "networking-rg"
}
```

---

# 4️⃣ **Get All Subnets From a VNet**

```hcl
data "azurerm_subnets" "all" {
  resource_group_name  = "networking-rg"
  virtual_network_name = "prod-vnet"
}
```

---

# 5️⃣ **Get an Existing Storage Account**

```hcl
data "azurerm_storage_account" "sa" {
  name                = "prodstorage123"
  resource_group_name = "infra-rg"
}
```

---

# 6️⃣ **Get a Storage Account Access Key**

```hcl
data "azurerm_storage_account_blob_container_sas" "sas" {
  connection_string = data.azurerm_storage_account.sa.primary_connection_string
  container_name    = "logs"
  https_only        = true

  start  = "2025-01-01T00:00:00Z"
  expiry = "2026-01-01T00:00:00Z"

  permissions {
    read   = true
    write  = false
    delete = false
    list   = true
  }
}
```

---

# 7️⃣ **Get an Existing Public IP**

```hcl
data "azurerm_public_ip" "pip" {
  name                = "webapp-ip"
  resource_group_name = "networking-rg"
}
```

---

# 8️⃣ **Lookup Azure Key Vault**

```hcl
data "azurerm_key_vault" "kv" {
  name                = "prod-kv"
  resource_group_name = "security-rg"
}
```

---

# 9️⃣ **Get a Key Vault Secret**

```hcl
data "azurerm_key_vault_secret" "db_password" {
  name         = "db-password"
  key_vault_id = data.azurerm_key_vault.kv.id
}
```

---

# 🔟 **Get Azure AD Logged-In Client Info**

```hcl
data "azuread_client_config" "current" {}
```

---

# 1️⃣1️⃣ **Get Current Azure Subscription**

```hcl
data "azurerm_subscription" "current" {}
```

---

# 1️⃣2️⃣ **Get an Azure Container Registry**

```hcl
data "azurerm_container_registry" "acr" {
  name                = "mycompanyacr"
  resource_group_name = "containers-rg"
}
```

---

# 1️⃣3️⃣ **Get an Azure Kubernetes Cluster**

```hcl
data "azurerm_kubernetes_cluster" "aks" {
  name                = "prod-aks"
  resource_group_name = "k8s-rg"
}
```

---

# 1️⃣4️⃣ **Get All Available VM Sizes in a Region**

```hcl
data "azurerm_compute_resource_skus" "vm_sizes" {
  filter {
    name   = "location"
    values = ["eastus"]
  }

  filter {
    name   = "resourceType"
    values = ["virtualMachines"]
  }
}
```

---

# 1️⃣5️⃣ **Get an Image From Azure Marketplace**

```hcl
data "azurerm_image" "custom" {
  name                = "myCustomImage"
  resource_group_name = "images-rg"
}
```

Or the Marketplace shared gallery:

```hcl
data "azurerm_shared_image" "ubuntu" {
  name                = "UbuntuServer"
  gallery_name        = "SharedGallery"
  resource_group_name = "images-rg"
}
```

---

# 🎉 **Now — Real-World Azure Example Using Data Sources**

## 🏗 Scenario

We will:

✔ Use an existing resource group  
✔ Use an existing VNet + subnet  
✔ Fetch the latest Ubuntu image from Azure  
✔ Fetch a Key Vault secret (admin password)  
✔ Deploy a Linux VM using this data

---

# 🧩 **main.tf — Complete Real-World Example**

```hcl
terraform {
  required_version = ">= 1.6"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.0"
    }
    azuread = {
      source = "hashicorp/azuread"
    }
  }
}

provider "azurerm" {
  features {}
}

# -------------------------------------------------
# 1. Existing Resource Group
# -------------------------------------------------
data "azurerm_resource_group" "rg" {
  name = "prod-rg"
}

# -------------------------------------------------
# 2. Get VNet and Subnet
# -------------------------------------------------
data "azurerm_virtual_network" "vnet" {
  name                = "prod-vnet"
  resource_group_name = data.azurerm_resource_group.rg.name
}

data "azurerm_subnet" "subnet" {
  name                 = "web-subnet"
  virtual_network_name = data.azurerm_virtual_network.vnet.name
  resource_group_name  = data.azurerm_resource_group.rg.name
}

# -------------------------------------------------
# 3. Azure Key Vault Secret (VM password)
# -------------------------------------------------
data "azurerm_key_vault" "kv" {
  name                = "prod-kv"
  resource_group_name = "security-rg"
}

data "azurerm_key_vault_secret" "admin_pw" {
  name         = "vm-admin-password"
  key_vault_id = data.azurerm_key_vault.kv.id
}

# -------------------------------------------------
# 4. Get Latest Ubuntu Image from Azure Marketplace
# -------------------------------------------------
data "azurerm_shared_image_gallery" "sig" {
  name                = "mySharedGallery"
  resource_group_name = "images-rg"
}

data "azurerm_shared_image" "ubuntu" {
  name                = "UbuntuServer"
  gallery_name        = data.azurerm_shared_image_gallery.sig.name
  resource_group_name = data.azurerm_shared_image_gallery.sig.resource_group_name
}

data "azurerm_shared_image_version" "latest" {
  name                = "latest"
  image_name          = data.azurerm_shared_image.ubuntu.name
  gallery_name        = data.azurerm_shared_image_gallery.sig.name
  resource_group_name = data.azurerm_shared_image_gallery.sig.resource_group_name
}

# -------------------------------------------------
# 5. Create Linux VM Using Only Data Source Inputs
# -------------------------------------------------
resource "azurerm_linux_virtual_machine" "vm" {
  name                = "prod-webserver"
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name
  size                = "Standard_B2s"

  admin_username = "azureuser"
  admin_password = data.azurerm_key_vault_secret.admin_pw.value

  network_interface_ids = [
    azurerm_network_interface.nic.id
  ]

  source_image_id = data.azurerm_shared_image_version.latest.id

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Premium_LRS"
  }
}

resource "azurerm_network_interface" "nic" {
  name                = "vm-nic"
  resource_group_name = data.azurerm_resource_group.rg.name
  location            = data.azurerm_resource_group.rg.location

  ip_configuration {
    name                          = "internal"
    subnet_id                     = data.azurerm_subnet.subnet.id
    private_ip_address_allocation = "Dynamic"
  }
}
```

---

# 🎯 **What This Real Example Demonstrates**

✔ Fetch existing Azure infrastructure  
✔ Securely pull secrets from Key Vault  
✔ Query latest image version dynamically  
✔ Build VM configuration fully using data sources

This is exactly how **enterprise Terraform on Azure** is done.

---

# 🚀 Want More?

I can also generate:

🔹 A full **module-based** Azure project  
🔹 A version using **Azure Load Balancers + VMs**  
🔹 A version for **AKS** using data sources  
🔹 Architecture diagrams  
🔹 A complete **variables.tf + outputs.tf** optimized structure

Just tell me!