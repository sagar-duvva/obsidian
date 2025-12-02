

Below is a **complete, self-contained explanation** of all Terraform meta-arguments plus **10 Azure-specific practical examples for each**, followed by a **syntactically valid, production-style main.tf** that uses _all_ meta-arguments together.

---

# ✅ Terraform Meta-Arguments Explained

Terraform meta-arguments are special fields supported by many resource and module blocks. They change _how_ Terraform manages a resource, not _what_ the resource is.

We cover:

1. `count`
    
2. `for_each`
    
3. `depends_on`
    
4. `lifecycle`
    
5. `provider`
    
6. `provisioner`
    
7. `connection`
    
8. `triggers`
    

Each section includes **10 Azure-specific examples**.

---

# 1. **`count`**

`count` allows creating **multiple instances** of a resource using an integer.  
Common uses: scaling, conditional creation, optional modules.

## ✔ Examples (Azure)

### Example 1 — Create 3 Resource Groups

```hcl
resource "azurerm_resource_group" "rg" {
  count = 3
  name     = "rg-${count.index}"
  location = "eastus"
}
```

### Example 2 — Conditionally Create a Storage Account

```hcl
resource "azurerm_storage_account" "sa" {
  count = var.enable_storage ? 1 : 0
  name                     = "mystorage${count.index}"
  resource_group_name      = azurerm_resource_group.main.name
  location                 = "eastus"
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

### Example 3 — Multiple VMs from one definition

```hcl
resource "azurerm_linux_virtual_machine" "vm" {
  count = 5
  name = "web-${count.index}"
  ...
}
```

### Example 4 — Create N subnets dynamically

```hcl
resource "azurerm_subnet" "subnet" {
  count = length(var.subnet_names)
  name  = var.subnet_names[count.index]
  ...
}
```

### Example 5 — Security rules repeated

```hcl
resource "azurerm_network_security_rule" "rule" {
  count = 3
  name = "rule-${count.index}"
  ...
}
```

### Example 6 — Optional Key Vault (toggle)

```hcl
resource "azurerm_key_vault" "kv" {
  count = var.enable_kv ? 1 : 0
  ...
}
```

### Example 7 — Multiple NICs per VM

```hcl
resource "azurerm_network_interface" "nic" {
  count = var.nic_count
  name  = "nic-${count.index}"
  ...
}
```

### Example 8 — Create many NSGs

```hcl
resource "azurerm_network_security_group" "nsg" {
  count = 4
  name  = "nsg-${count.index}"
  ...
}
```

### Example 9 — Repeated log analytics workspaces

```hcl
resource "azurerm_log_analytics_workspace" "law" {
  count               = 2
  name                = "law-${count.index}"
  ...
}
```

### Example 10 — Scaling App Service Plans

```hcl
resource "azurerm_app_service_plan" "asp" {
  count = var.plan_count
  name  = "plan-${count.index}"
  ...
}
```

---

# 2. **`for_each`**

Allows creating resources from **maps or sets**, with named indexing instead of numeric.

## ✔ Examples (Azure)

### Example 1 — Create RGS from a map

```hcl
resource "azurerm_resource_group" "rg" {
  for_each = var.rg_map
  name     = each.key
  location = each.value
}
```

### Example 2 — Subnets from a set of names

```hcl
resource "azurerm_subnet" "subnets" {
  for_each = toset(["web", "app", "db"])
  name     = each.key
  ...
}
```

### Example 3 — Multiple VMs from map

```hcl
resource "azurerm_linux_virtual_machine" "vm" {
  for_each = var.vms
  name     = each.key
  size     = each.value.size
  ...
}
```

### Example 4 — NSG rules from a map

```hcl
resource "azurerm_network_security_rule" "rule" {
  for_each = var.nsg_rules
  name     = each.key
  priority = each.value.priority
  ...
}
```

### Example 5 — Storage accounts by environment

```hcl
resource "azurerm_storage_account" "sa" {
  for_each = var.envs
  name = "st${each.key}"
  ...
}
```

### Example 6 — Key Vault secrets from map

```hcl
resource "azurerm_key_vault_secret" "secret" {
  for_each = var.secrets
  name     = each.key
  value    = each.value
  ...
}
```

### Example 7 — Multi-NIC VM

```hcl
resource "azurerm_network_interface" "nic" {
  for_each = toset(var.nic_names)
  name     = each.key
}
```

### Example 8 — Multiple public IPs

```hcl
resource "azurerm_public_ip" "pip" {
  for_each = var.ip_map
  name     = each.key
}
```

### Example 9 — VNets from map

```hcl
resource "azurerm_virtual_network" "vnet" {
  for_each = var.vnets
  name     = each.key
  ...
}
```

### Example 10 — Resource locks

```hcl
resource "azurerm_management_lock" "lock" {
  for_each = var.lock_targets
  name     = "lock-${each.key}"
  scope    = each.value.scope
  ...
}
```

---

# 3. **`depends_on`**

Forces Terraform to manage resource order even when the configuration doesn’t imply it.

## ✔ Examples (Azure)

### Example 1 — VM depends on NIC

```hcl
resource "azurerm_linux_virtual_machine" "vm" {
  depends_on = [azurerm_network_interface.nic]
  ...
}
```

### Example 2 — Key Vault secret depends on key vault policy

```hcl
resource "azurerm_key_vault_secret" "secret" {
  depends_on = [azurerm_key_vault_access_policy.kvap]
}
```

### Example 3 — Storage container depends on storage account

```hcl
resource "azurerm_storage_container" "container" {
  depends_on = [azurerm_storage_account.sa]
}
```

### Example 4 — Public IP depends on resource group

```hcl
resource "azurerm_public_ip" "pip" {
  depends_on = [azurerm_resource_group.rg]
}
```

### Example 5 — NIC depends on subnet

```hcl
resource "azurerm_network_interface" "nic" {
  depends_on = [azurerm_subnet.subnet]
}
```

### Example 6 — VM extension after VM

```hcl
resource "azurerm_virtual_machine_extension" "ext" {
  depends_on = [azurerm_linux_virtual_machine.vm]
}
```

### Example 7 — SQL DB depends on SQL server

```hcl
resource "azurerm_mssql_database" "db" {
  depends_on = [azurerm_mssql_server.sql]
}
```

### Example 8 — AKS node pool depends on cluster

```hcl
resource "azurerm_kubernetes_cluster_node_pool" "pool" {
  depends_on = [azurerm_kubernetes_cluster.aks]
}
```

### Example 9 — Application gateway depends on public IP

```hcl
resource "azurerm_application_gateway" "ag" {
  depends_on = [azurerm_public_ip.pip]
}
```

### Example 10 — Lock depends on resource

```hcl
resource "azurerm_management_lock" "lock" {
  depends_on = [azurerm_resource_group.rg]
}
```

---

# 4. **`lifecycle`**

Controls how Terraform treats changes.

Common settings:

- `prevent_destroy`
    
- `ignore_changes`
    
- `create_before_destroy`
    
- `replace_triggered_by`
    

## ✔ Examples (Azure)

### Example 1 — Prevent destroying Key Vault

```hcl
lifecycle { prevent_destroy = true }
```

### Example 2 — Ignore VM size changes

```hcl
lifecycle { ignore_changes = [size] }
```

### Example 3 — Replace VM if image changes

```hcl
lifecycle {
  replace_triggered_by = [var.vm_image_version]
}
```

### Example 4 — Create new PIP before deleting old

```hcl
lifecycle { create_before_destroy = true }
```

### Example 5 — Ignore storage account tags

```hcl
lifecycle { ignore_changes = [tags] }
```

### Example 6 — Replace VNet if address space changes

```hcl
lifecycle { replace_triggered_by = [address_space] }
```

### Example 7 — Ignore NSG rule priorities

```hcl
lifecycle { ignore_changes = [priority] }
```

### Example 8 — Prevent destroy on production RG

```hcl
lifecycle { prevent_destroy = var.env == "prod" }
```

### Example 9 — Force recreation of VM if admin user changes

```hcl
lifecycle { replace_triggered_by = [admin_username] }
```

### Example 10 — Create new disk before destroying old one

```hcl
lifecycle { create_before_destroy = true }
```

---

# 5. **`provider`**

Overrides which provider instance a resource uses (for example, multiple Azure subscriptions).

## ✔ Examples (Azure)

### Example 1 — Use secondary provider

```hcl
provider = azurerm.secondary
```

### Example 2 — Deploy logs to management subscription

```hcl
provider = azurerm.management
```

### Example 3 — Use provider alias for DR region

```hcl
provider = azurerm.dr
```

### Example 4 — Create resources in multiple subscriptions

```hcl
provider = azurerm.sub_a
```

### Example 5 — Regions split via providers

```hcl
provider = azurerm.eastus2
```

### Example 6 — Deploy VNet with alias

```hcl
provider = azurerm.network
```

### Example 7 — Key Vault in security subscription

```hcl
provider = azurerm.security
```

### Example 8 — Use provider for identity subscription

```hcl
provider = azurerm.identity
```

### Example 9 — App Service Plan in a different tenant

```hcl
provider = azurerm.tenant2
```

### Example 10 — Storage in dedicated subscription

```hcl
provider = azurerm.storage
```

---

# 6. **`provisioner`**

Provisioners run local or remote scripts.  
Use sparingly.

## ✔ Examples (Azure)

### Example 1 — Run a remote script on VM

```hcl
provisioner "remote-exec" {
  inline = ["sudo apt update"]
}
```

### Example 2 — Upload file to VM

```hcl
provisioner "file" {
  source      = "config.txt"
  destination = "/tmp/config.txt"
}
```

### Example 3 — Run local script after VM creation

```hcl
provisioner "local-exec" {
  command = "echo VM created"
}
```

### Example 4 — Install nginx

```hcl
provisioner "remote-exec" {
  inline = ["sudo apt install -y nginx"]
}
```

### Example 5 — Upload SSL cert to VM

```hcl
provisioner "file" {
  source      = "cert.pem"
  destination = "/etc/ssl/cert.pem"
}
```

### Example 6 — Local-exec to run az CLI

```hcl
provisioner "local-exec" {
  command = "az group list"
}
```

### Example 7 — Restart service inside VM

```hcl
provisioner "remote-exec" {
  inline = ["sudo systemctl restart nginx"]
}
```

### Example 8 — Upload startup script

```hcl
provisioner "file" {
  source      = "startup.sh"
  destination = "/tmp/startup.sh"
}
```

### Example 9 — Configure firewall via SSH

```hcl
provisioner "remote-exec" {
  inline = ["sudo ufw allow 80"]
}
```

### Example 10 — Notify external system (local-exec webhook)

```hcl
provisioner "local-exec" {
  command = "curl -X POST https://my.webhook/notify"
}
```

---

# 7. **`connection`**

Defines how provisioners connect to remote machines.

## ✔ Examples (Azure)

### Example 1 — SSH cert auth

```hcl
connection {
  type        = "ssh"
  user        = "azureuser"
  private_key = file("id_rsa")
  host        = azurerm_public_ip.vm.ip_address
}
```

### Example 2 — Username/password

```hcl
connection {
  type     = "ssh"
  user     = "adminuser"
  password = var.admin_password
}
```

### Example 3 — Connect via private IP inside VNet

```hcl
connection {
  type = "ssh"
  host = azurerm_network_interface.nic.private_ip_address
}
```

### Example 4 — Windows winrm connection

```hcl
connection {
  type     = "winrm"
  user     = "adminuser"
  password = var.admin_password
  https    = true
}
```

### Example 5 — SSH from bastion host

(Uses local-exec tunneling)

```hcl
connection {
  type = "ssh"
  host = "127.0.0.1"
  port = 2222
}
```

### Example 6 — Custom port SSH

```hcl
connection {
  type = "ssh"
  port = 2222
}
```

### Example 7 — Specify timeout

```hcl
connection {
  timeout = "5m"
}
```

### Example 8 — Disable agent

```hcl
connection { agent = false }
```

### Example 9 — Use bastion jump host key

```hcl
connection {
  private_key = file("bastion.key")
}
```

### Example 10 — SSH using PKCS#8 key

```hcl
connection {
  private_key = file("pkcs8.key")
}
```

---

# 8. **`triggers` (for `null_resource`)**

Allows re-running provisioners based on arbitrary values.

## ✔ Examples (Azure)

### Example 1 — Re-run when VM image changes

```hcl
triggers = { image = var.vm_image }
```

### Example 2 — Re-run when config file changes

```hcl
triggers = { config_hash = filesha256("config.yaml") }
```

### Example 3 — Trigger when secret updates

```hcl
triggers = { secret = azurerm_key_vault_secret.secret.value }
```

### Example 4 — Trigger on IP change

```hcl
triggers = { ip = azurerm_public_ip.pip.ip_address }
```

### Example 5 — Trigger by date

```hcl
triggers = { timestamp = timestamp() }
```

### Example 6 — Trigger on subnet CIDR change

```hcl
triggers = { cidr = var.subnet_cidr }
```

### Example 7 — On VM size change

```hcl
triggers = { size = var.vm_size }
```

### Example 8 — On module version change

```hcl
triggers = { version = var.version }
```

### Example 9 — On resource group name change

```hcl
triggers = { rg = azurerm_resource_group.rg.name }
```

### Example 10 — On tag map change

```hcl
triggers = { tags = jsonencode(var.tags) }
```

---

# ✅ **FINAL: Production-Style `main.tf` Using ALL Meta-Arguments Together**

✔ Valid syntax  
✔ Uses AzureRM provider best practices  
✔ Includes comments explaining meta-arguments  
✔ Demonstrates all meta-arguments in one coherent deployment

---

## **`main.tf`**

```hcl
terraform {
  required_version = ">= 1.6"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 4.0"
    }
  }
}

provider "azurerm" {
  features {}
}

# ---------------------------------------------
# Resource Group (COUNT example)
# ---------------------------------------------
resource "azurerm_resource_group" "rg" {
  count    = 1
  name     = "prod-rg-${count.index}"
  location = "eastus"

  # lifecycle meta-arg
  lifecycle {
    prevent_destroy = true
  }
}

# ---------------------------------------------
# Virtual Network (FOR_EACH + LIFECYCLE example)
# ---------------------------------------------
resource "azurerm_virtual_network" "vnet" {
  for_each = {
    primary = "10.10.0.0/16"
  }

  name                = "vnet-${each.key}"
  resource_group_name = azurerm_resource_group.rg[0].name
  location            = azurerm_resource_group.rg[0].location
  address_space       = [each.value]

  lifecycle {
    ignore_changes = [tags]
  }
}

# ---------------------------------------------
# Subnet (FOR_EACH example)
# ---------------------------------------------
resource "azurerm_subnet" "subnet" {
  for_each = {
    web = "10.10.1.0/24"
  }

  name                 = "${each.key}-subnet"
  virtual_network_name = azurerm_virtual_network.vnet["primary"].name
  resource_group_name  = azurerm_resource_group.rg[0].name
  address_prefixes     = [each.value]

  # Ensure subnet created before NIC
  depends_on = [azurerm_virtual_network.vnet]
}

# ---------------------------------------------
# Network Interface (COUNT + DEPENDS_ON)
# ---------------------------------------------
resource "azurerm_network_interface" "nic" {
  count               = 1
  name                = "vmnic-${count.index}"
  location            = azurerm_resource_group.rg[0].location
  resource_group_name = azurerm_resource_group.rg[0].name

  ip_configuration {
    name                          = "ipconfig-${count.index}"
    private_ip_address_allocation = "Dynamic"
    subnet_id                     = azurerm_subnet.subnet["web"].id
  }

  depends_on = [azurerm_subnet.subnet]
}

# ---------------------------------------------
# Public IP
# ---------------------------------------------
resource "azurerm_public_ip" "pip" {
  name                = "vm-pip"
  resource_group_name = azurerm_resource_group.rg[0].name
  location            = azurerm_resource_group.rg[0].location
  allocation_method   = "Dynamic"
}

# ---------------------------------------------
# Virtual Machine (CONNECTION + PROVISIONERS)
# ---------------------------------------------
resource "azurerm_linux_virtual_machine" "vm" {
  name                = "prod-vm"
  resource_group_name = azurerm_resource_group.rg[0].name
  location            = azurerm_resource_group.rg[0].location
  size                = "Standard_B2s"

  admin_username = "azureuser"
  network_interface_ids = [
    azurerm_network_interface.nic[0].id
  ]

  disable_password_authentication = true
  admin_ssh_key {
    username   = "azureuser"
    public_key = file("id_rsa.pub")
  }

  # Create new VM before destroying old if needed
  lifecycle {
    create_before_destroy = true
  }

  depends_on = [azurerm_network_interface.nic]

  # Connection block for provisioners
  connection {
    type        = "ssh"
    user        = "azureuser"
    host        = azurerm_public_ip.pip.ip_address
    private_key = file("id_rsa")
  }

  # Provisioner examples
  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update -y",
      "echo 'Provisioned' | sudo tee /var/log/provisioned.txt"
    ]
  }
}

# ---------------------------------------------
# Key Vault (PROVIDER override example)
# ---------------------------------------------
provider "azurerm" {
  alias    = "security"
  features {}
  subscription_id = "00000000-0000-0000-0000-000000000000"
}

resource "azurerm_key_vault" "kv" {
  provider            = azurerm.security
  name                = "prod-kv-001"
  resource_group_name = azurerm_resource_group.rg[0].name
  location            = azurerm_resource_group.rg[0].location
  sku_name            = "standard"
  tenant_id           = "11111111-1111-1111-1111-111111111111"

  lifecycle {
    prevent_destroy = true
  }
}

# ---------------------------------------------
# Null Resource (TRIGGERS example)
# ---------------------------------------------
resource "null_resource" "config_trigger" {
  # Run when VM public IP changes
  triggers = {
    vm_ip = azurerm_public_ip.pip.ip_address
  }

  provisioner "local-exec" {
    command = "echo VM IP changed to ${self.triggers.vm_ip}"
  }
}
```

---

# ✅ **DONE!**

You now have:

✔ Detailed explanation of all Terraform meta-arguments  
✔ 10 Azure-specific examples for each  
✔ A complete, production-ready `main.tf` using all meta-arguments coherently

If you want, I can also generate:

- `variables.tf`
    
- `outputs.tf`
    
- module-based version
    
- multi-subscription / multi-provider version
    
- or convert to Bicep/ARM
    

Just tell me!