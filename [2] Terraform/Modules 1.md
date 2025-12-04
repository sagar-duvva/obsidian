
Below is a clean, ready-to-use **Terraform module for an Azure Virtual Network** (`vnet/`) and an example of how to **call it from `main.tf`**.

---

# ✅ **Module Structure**

```
/modules
  /vnet
    vnet.tf
    variables.tf
    outputs.tf
main.tf
```

---

# 📌 **modules/vnet/vnet.tf**

```hcl
terraform {
  required_version = ">= 1.3.0"
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = ">= 3.0"
    }
  }
}

resource "azurerm_virtual_network" "this" {
  name                = var.vnet_name
  location            = var.location
  resource_group_name = var.resource_group_name
  address_space       = var.address_space
  tags                = var.tags
}

resource "azurerm_subnet" "subnets" {
  for_each = var.subnets

  name                 = each.key
  resource_group_name  = var.resource_group_name
  virtual_network_name = azurerm_virtual_network.this.name
  address_prefixes     = each.value
}
```

---

# 📌 **modules/vnet/variables.tf**

```hcl
variable "vnet_name" {
  type        = string
  description = "Name of the VNet"
}

variable "location" {
  type        = string
  description = "Azure region"
}

variable "resource_group_name" {
  type        = string
  description = "Resource group name"
}

variable "address_space" {
  type        = list(string)
  description = "VNet address space"
}

variable "subnets" {
  type        = map(list(string))
  description = "Map of subnet names to address prefixes"
}

variable "tags" {
  type        = map(string)
  default     = {}
}
```

---

# 📌 **modules/vnet/outputs.tf**

```hcl
output "vnet_id" {
  value = azurerm_virtual_network.this.id
}

output "subnet_ids" {
  value = { for k, s in azurerm_subnet.subnets : k => s.id }
}
```

---

# 📌 **main.tf (Calling the Module)**

```hcl
provider "azurerm" {
  features {}
}

module "vnet" {
  source              = "./modules/vnet"
  vnet_name           = "my-vnet"
  location            = "eastus"
  resource_group_name = "rg-network"
  address_space       = ["10.0.0.0/16"]

  subnets = {
    "subnet-app" = ["10.0.1.0/24"]
    "subnet-db"  = ["10.0.2.0/24"]
  }

  tags = {
    environment = "dev"
    project     = "example"
  }
}
```

---

# Want extras?

I can also generate:

✅ A complete resource-group module  
✅ NSG + subnet associations  
✅ Route tables  
✅ Private DNS zones  
✅ Hub-spoke network topology

Just tell me what you'd like added!



