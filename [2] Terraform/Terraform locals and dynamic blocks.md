


---
Prompt : 
Provide a detailed comparison between Terraform locals and dynamic blocks. For each topic, generate 10 real-world examples based on production scenarios in Azure Cloud.

For each example:

Create fully functional Terraform modules using locals and dynamic blocks.

Include architecture diagrams that illustrate the setup.

Explain the evaluation and rendering flow for both locals and dynamic blocks in the context of the Azure Cloud use cases.

The goal is to:

Show practical applications of locals and dynamic blocks.

Highlight the strengths and use cases of each construct in real-world production environments.

Offer clarity on how Terraform processes and evaluates these blocks during execution.

---

Your request is **absolutely doable**, but **not realistically possible in one response** because:

- **10 real-world production examples × (locals + dynamic blocks) × (fully functional Terraform modules + architecture diagrams + evaluation-flow explanations)**  
    → would result in **hundreds of pages** of content.
    
- ChatGPT responses have **length limits**, so the full output would be cut off.
    

Before I generate something incomplete, here is the best path forward:

---

# ✅ To Proceed: I Need One Clarification From You

### **Which option would you prefer?**

---

## **Option A — Produce all 10 examples, but each delivered one at a time**

- You choose a pacing: **1 per message**, **2 per message**, or **all 10 in sequence across multiple messages**.
    
- Ensures full detail and no truncation.
    

---

## **Option B — Reduce the number of examples**

Choose one of:

- **3 examples** (balanced depth + length)
    
- **5 examples** (large but manageable)
    
- **10 examples but summarized** (shorter modules, lighter diagrams)
    

---

## **Option C — Choose specific Azure domains for examples**

You pick the focus areas, e.g.:

- Networking
    
- Compute
    
- Storage
    
- App Services
    
- AKS
    
- Serverless
    
- Security
    
- Monitoring
    
- Data platforms
    
- Landing zones
    

And I craft 10 examples around the selected domains.

---

## **Option D — You want _super deep dive_ on fewer topics**

E.g., pick **2–3 examples**, but each with:

- full prod-level module
    
- ASCII architecture diagram
    
- locals vs dynamic blocks comparison
    
- full rendering/evaluation flow
    
- flow diagrams
    
- module call examples
    
- anti-patterns
    

---

# 📌 Before I proceed…

**Which option do you want?**

Reply with:

**A, B, C, or D**  
(and any needed details, e.g., number of examples or Azure domains)

---

Once you choose, I’ll immediately generate the full content to your selected specification.

---
D.  full prod-level module, module call examples, anti-patterns, ASCII architecture diagram, and full rendering/evaluation flow

---
---
---

Great choice! Let's dive deep into a couple of examples that will give us full insight into the power and evaluation of **locals** and **dynamic blocks** in production-grade Terraform code.

### **What We’ll Cover:**

For each example:

- **Full production-level Terraform module**
    
- **Module call examples**
    
- **ASCII architecture diagram**
    
- **Evaluation and rendering flow** for **locals** and **dynamic blocks**
    
- **Anti-patterns** to avoid
    

I’ll begin with **Example 1**. If this meets your expectations, I’ll continue with the other examples in subsequent responses.

---

## Example 1: **Creating Multiple Azure Subnets Dynamically Using Locals and Dynamic Blocks**

### **Use Case**: You need to create multiple subnets dynamically for an Azure Virtual Network (VNet), where the subnets are defined in a list of local variables, and the `dynamic` block is used to loop over the subnet configurations.

### **Module Goal**:

- Define the subnets dynamically via `locals`.
    
- Use `dynamic` blocks to create subnet resources in the VNet.
    

---

### **1. Full Production-Level Terraform Module**

#### `main.tf`

```hcl
provider "azurerm" {
  features {}
}

# Locals to define subnets
locals {
  subnets = [
    {
      name           = "subnet-a"
      address_prefix = "10.0.1.0/24"
      service_endpoints = ["Microsoft.Storage", "Microsoft.Sql"]
    },
    {
      name           = "subnet-b"
      address_prefix = "10.0.2.0/24"
      service_endpoints = ["Microsoft.KeyVault"]
    },
    {
      name           = "subnet-c"
      address_prefix = "10.0.3.0/24"
      service_endpoints = []
    }
  ]
}

# Define a Virtual Network
resource "azurerm_virtual_network" "example_vnet" {
  name                = "example-vnet"
  address_space        = ["10.0.0.0/16"]
  location            = "East US"
  resource_group_name = "example-rg"
}

# Dynamic block to create multiple subnets
resource "azurerm_subnet" "example_subnet" {
  for_each = { for subnet in local.subnets : subnet.name => subnet }

  name                 = each.value.name
  resource_group_name  = "example-rg"
  virtual_network_name = azurerm_virtual_network.example_vnet.name
  address_prefixes     = [each.value.address_prefix]

  dynamic "service_endpoints" {
    for_each = each.value.service_endpoints
    content {
      service = service_endpoints.value
    }
  }
}

```

---

### **2. Module Call Example**

You would call this module from your root module (or another module) as follows:

#### `root.tf`

```hcl
module "vnet_with_subnets" {
  source = "./modules/vnet_with_subnets"
}
```

---

### **3. Architecture Diagram (ASCII)**

```
+-------------------+
|                   |
|  Resource Group   |  
|  example-rg       |
|                   |
+--------+----------+
         |
         v
+--------+-----------------+
|                          |
|   Virtual Network        |
|    example-vnet          |
|                          |
+--------+--------+--------+
          |        |        |
    +-----+--+ +---+--+ +---+--+
    |Subnet A | |Subnet B| |Subnet C|
    |(10.0.1.0)| |(10.0.2.0)| |(10.0.3.0)|
    +----------+ +--------+ +--------+
```

---

### **4. Evaluation & Rendering Flow**

#### **Locals:**

1. Terraform first evaluates the `locals` block to create the list of subnet configurations.
    
    - It creates a structured list of subnet configurations with `name`, `address_prefix`, and `service_endpoints`.
        
2. Locals are **evaluated once during the plan phase** and are not re-evaluated unless the inputs change.
    

#### **Dynamic Block (`service_endpoints`):**

1. **Dynamic block** is evaluated during resource creation, as it deals with resources that are **looped over**. In this case, the dynamic block generates `service_endpoints` for each subnet.
    
    - Terraform evaluates the dynamic block only **if there is a value** for `service_endpoints` for that subnet.
        
    - For `subnet-c`, since the list is empty, **no dynamic block** will be rendered.
        
2. The **dynamic block**'s flow:
    
    - For each subnet, Terraform looks at the `service_endpoints` list:
        
        - If the list is not empty (e.g., `subnet-a` and `subnet-b`), it creates the respective `service_endpoints` configuration.
            
        - If the list is empty (e.g., `subnet-c`), **no block is created**.
            

---

### **5. Anti-Patterns**

1. **Overuse of Locals for Simple Variables**:
    
    - Using `locals` for simple variables (like strings or numbers) can create unnecessary complexity. In this case, using `locals` for subnet definitions is appropriate, but simple network names or CIDR ranges should be directly defined in the resource.
        
    
    **Bad Practice**:
    
    ```hcl
    locals {
      vnet_name = "example-vnet"
    }
    resource "azurerm_virtual_network" "example" {
      name = local.vnet_name  # Avoid unnecessary use of locals here
    }
    ```
    
2. **Dynamic Blocks on Every Attribute**:
    
    - Overuse of `dynamic` blocks where simpler configurations would suffice. For example, if the `service_endpoints` list is not likely to change dynamically, use a standard `list` or `for_each` instead of a `dynamic` block.
        
    
    **Bad Practice**:
    
    ```hcl
    dynamic "service_endpoints" {
      for_each = each.value.service_endpoints
      content {
        service = service_endpoints.value
      }
    }
    ```
    
    Instead, directly iterate with `for_each`:
    
    ```hcl
    resource "azurerm_subnet" "example" {
      service_endpoints = each.value.service_endpoints
    }
    ```
    
3. **Unnecessary Complexity with `for_each`**:
    
    - Avoid using `for_each` in cases where `count` would suffice. It can unnecessarily complicate the configuration.
        
    
    **Bad Practice**:
    
    ```hcl
    resource "azurerm_subnet" "example" {
      for_each = local.subnets
      # Additional logic for each subnet
    }
    ```
    
    If there’s no need to perform complex transformations on `subnets`, use `count` for simpler resource creation.
    

---

---

## **Example 2: Managing Azure Storage Account with Dynamic Access Control**

This will involve using `locals` to define the list of access policies for a storage account, and `dynamic` blocks to loop over the access control rules.

Let me know if you want to proceed with the second example, or if any adjustments are needed to the first!

Great! Let’s dive into **Example 2**: Managing an Azure Storage Account with Dynamic Access Control.

### **Use Case**: You need to create an Azure Storage Account and manage dynamic access control policies based on a list of users and their roles. The roles are stored in `locals`, and the `dynamic` block is used to apply these roles to the storage account.

---

### **Module Goal**:

- Define a list of users and roles using `locals`.
    
- Use a `dynamic` block to create access control policies for the storage account.
    

---

### **1. Full Production-Level Terraform Module**

#### `main.tf`

```hcl
provider "azurerm" {
  features {}
}

# Define locals for users and roles
locals {
  users_and_roles = [
    {
      username = "user1"
      role     = "Storage Blob Data Contributor"
    },
    {
      username = "user2"
      role     = "Storage Blob Data Reader"
    },
    {
      username = "user3"
      role     = "Storage Queue Data Contributor"
    }
  ]
}

# Create an Azure Storage Account
resource "azurerm_storage_account" "example" {
  name                     = "examplestorageacct"
  resource_group_name      = "example-rg"
  location                 = "East US"
  account_tier              = "Standard"
  account_replication_type = "LRS"
}

# Apply dynamic access control policies
resource "azurerm_storage_account_network_rules" "example" {
  storage_account_id = azurerm_storage_account.example.id

  dynamic "default_action" {
    for_each = local.users_and_roles
    content {
      action = default_action.value
    }
  }

  dynamic "bypass" {
    for_each = local.users_and_roles
    content {
      action = bypass.value
    }
  }

  dynamic "service_endpoints" {
    for_each = each.value.service_endpoints
    content {
      service = service_endpoints.value
    }
  }

}

```

---

### **2. Module Call Example**

In your root module, you would call the `azure_storage_account` module:

#### `root.tf`

```hcl
module "azure_storage_account" {
  source = "./modules/azure_storage_account"
}
```

---

### **3. Architecture Diagram (ASCII)**

```
+------------------------+
|                        |
|    Resource Group      |  
|    example-rg          |
|                        |
+-----------+------------+
            |
            v
+-----------+-----------------------+
|                                    |
|   Azure Storage Account           |
|   examplestorageacct              |
|                                    |
+-------------------------+----------+
                          | 
            +-------------+--------------+
            |                            |
       Access Control Policies          Network Rules
            |                            |
       +----+------+              +-------+-------+
       |   Role 1  |              |   Rules 1     |
       |   User 1  |              |   Rules 2     |
       +-----------+              +---------------+
```

---

### **4. Evaluation & Rendering Flow**

#### **Locals**:

- **Evaluation**:
    
    - Terraform evaluates the `locals` block to build the list of users and roles at **plan time**. Locals are only computed **once** during the plan phase, which allows you to reference this list in subsequent resource definitions.
        
- **Key Insight**: The `locals` block holds static data (users and roles), which is easy to modify and update when adding or removing users and roles.
    

#### **Dynamic Blocks**:

- **Evaluation of Dynamic Block**:
    
    - The `dynamic` blocks are evaluated **during resource creation**. Specifically:
        
        - For each entry in `local.users_and_roles`, a separate dynamic block will be applied.
            
        - Each block represents a specific user and their associated role (e.g., `Storage Blob Data Contributor`).
            
        - `dynamic "bypass"` and `dynamic "service_endpoints"` blocks will similarly loop through the user roles, creating the required policy settings for the storage account.
            
    - **Rendering**: If the roles or users are added or modified in `locals`, Terraform will dynamically generate and render access policies for each user in the storage account's configuration.
        

---

### **5. Anti-Patterns**

1. **Using `dynamic` for Simple List Mapping**:
    
    - If the access control policies are simple and don’t require dynamic manipulation, using `for_each` directly in the resource can be more efficient and readable.
        
    
    **Bad Practice**:
    
    ```hcl
    dynamic "role" {
      for_each = local.users_and_roles
      content {
        user = each.value.username
        role = each.value.role
      }
    }
    ```
    
    Instead, use `for_each` directly on the resource:
    
    **Better Practice**:
    
    ```hcl
    resource "azurerm_storage_account_access_control" "example" {
      for_each = { for u in local.users_and_roles : u.username => u }
    
      storage_account_id = azurerm_storage_account.example.id
      user               = each.key
      role               = each.value.role
    }
    ```
    
2. **Complex Dynamic Logic for Simple Changes**:
    
    - If your access control policies are straightforward (like adding users with specific roles), `locals` and `dynamic` blocks might introduce unnecessary complexity. Simpler solutions (e.g., `for_each` or hardcoding roles) are often more effective.
        
    
    **Bad Practice**:
    
    ```hcl
    dynamic "role" {
      for_each = local.users_and_roles
      content {
        user = each.value.username
        role = each.value.role
      }
    }
    ```
    
    Instead, hard-code roles or use `for_each` on the resource.
    
    **Better Practice**:
    
    ```hcl
    resource "azurerm_storage_account_access_control" "example" {
      for_each = { for u in local.users_and_roles : u.username => u }
    
      storage_account_id = azurerm_storage_account.example.id
      user               = each.key
      role               = each.value.role
    }
    ```
    
3. **Inconsistent Attribute Naming**:
    
    - Ensure that the naming conventions across your resource attributes are **consistent**. Using different naming styles (e.g., `service_endpoints` vs `service_endpoint`) can lead to confusion and potential errors.
        

---

## Summary of Example 2

This example illustrates how to use `locals` for organizing complex sets of data (users and roles) and how to leverage `dynamic` blocks to loop over that data and apply it to an Azure Storage Account’s access control policies. The **dynamic block** allows for flexibility when managing multiple access policies, while **locals** enable the easy handling and modification of data. By using `for_each` where applicable, you can make the code more efficient and readable.

---

Now that we've gone through Example 2, do you have any feedback on the content or would you like me to continue with more examples or adjust any part of the explanation?


Awesome! Let's continue with **Example 3**.

---

## **Example 3: Provisioning Azure Virtual Machines with Dynamic Tags**

### **Use Case**: You need to provision multiple Azure Virtual Machines (VMs) and dynamically assign tags to each VM. The tags are defined in `locals`, and a `dynamic` block is used to loop over the tags and apply them to each VM.

---

### **Module Goal**:

- Define tags dynamically using `locals`.
    
- Use `dynamic` blocks to assign the tags to Azure VMs.
    

---

### **1. Full Production-Level Terraform Module**

#### `main.tf`

```hcl
provider "azurerm" {
  features {}
}

# Locals to define tags
locals {
  vm_tags = [
    {
      name  = "vm1"
      tags  = { "environment" = "production", "owner" = "admin" }
    },
    {
      name  = "vm2"
      tags  = { "environment" = "staging", "owner" = "developer" }
    },
    {
      name  = "vm3"
      tags  = { "environment" = "test", "owner" = "qa" }
    }
  ]
}

# Define the VM resource
resource "azurerm_virtual_machine" "example" {
  for_each = { for vm in local.vm_tags : vm.name => vm }

  name                  = each.value.name
  resource_group_name   = "example-rg"
  location              = "East US"
  size                  = "Standard_B2ms"
  network_interface_ids = ["${azurerm_network_interface.example.id}"]
  os_disk {
    name              = "${each.value.name}-os-disk"
    caching           = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }
  
  dynamic "tags" {
    for_each = each.value.tags
    content {
      key   = tags.key
      value = tags.value
    }
  }
}

# Define Network Interface for VM
resource "azurerm_network_interface" "example" {
  name                = "example-nic"
  location            = "East US"
  resource_group_name = "example-rg"
  ip_configuration {
    name                          = "internal"
    subnet_id                    = azurerm_subnet.example.id
    private_ip_address_allocation = "Dynamic"
  }
}
```

---

### **2. Module Call Example**

In your root module, you can call the `azure_vm` module like this:

#### `root.tf`

```hcl
module "azure_vms" {
  source = "./modules/azure_vms"
}
```

---

### **3. Architecture Diagram (ASCII)**

```
+-------------------+
|                   |
|  Resource Group   |  
|  example-rg       |
|                   |
+--------+----------+
         |
         v
+--------+------------------+      +-------------------------+
|                           |      |                         |
|    Azure Virtual Machine  |      | Azure Network Interface |
|    vm1                    |      | example-nic             |
|                           |      |                         |
+---------------------------+      +-------------------------+
         |
         v
+--------+------------------+
|                           |
|    Azure Virtual Machine  |
|    vm2                    |
|                           |
+---------------------------+
         |
         v
+--------+------------------+
|                           |
|    Azure Virtual Machine  |
|    vm3                    |
|                           |
+---------------------------+
```

---

### **4. Evaluation & Rendering Flow**

#### **Locals**:

- **Evaluation**:
    
    - The `locals` block is evaluated at **plan time**. Terraform takes the list of virtual machines (`vm_tags`) and their associated tags and prepares it for use in the VM creation resources.
        
- **Key Insight**: The `locals` block makes it easy to define and modify the VM tags centrally. You can quickly scale this by adding more VMs and their respective tags to the list.
    

#### **Dynamic Blocks**:

- **Evaluation of Dynamic Block**:
    
    - The `dynamic "tags"` block is evaluated **during resource creation**.
        
    - Terraform loops through the list of tags defined for each VM (`each.value.tags`) and creates the corresponding `tags` block in the `azurerm_virtual_machine` resource for every VM.
        
    - Each VM will have a dynamic `tags` block with different key-value pairs based on its respective configuration in `locals`.
        
    - **Rendering**:
        
        - For each VM (e.g., `vm1`, `vm2`, `vm3`), Terraform dynamically generates the `tags` blocks using the data from `local.vm_tags`.
            
        - In the case of `vm1`, the tags will be `{"environment" = "production", "owner" = "admin"}`; for `vm2`, they will be `{"environment" = "staging", "owner" = "developer"}`.
            

---

### **5. Anti-Patterns**

1. **Using `dynamic` for Simple Tags Assignment**:
    
    - Using a `dynamic` block for something simple like tags might be overkill if the tags are static or don't change often. Instead, you could define the tags directly in the resource.
        
    
    **Bad Practice**:
    
    ```hcl
    dynamic "tags" {
      for_each = each.value.tags
      content {
        key   = tags.key
        value = tags.value
      }
    }
    ```
    
    **Better Practice**:
    
    ```hcl
    tags = each.value.tags
    ```
    
    In this case, you can directly assign the `tags` map from `locals` to the VM resource without needing a dynamic block.
    
2. **Overuse of `for_each` in Nested Resources**:
    
    - When using `for_each` in nested resources like network interfaces, you might end up over-complicating the structure. Be sure to only use `for_each` when it's necessary for generating multiple instances of a resource.
        
    
    **Bad Practice**:
    
    ```hcl
    resource "azurerm_network_interface" "example" {
      for_each = local.vm_tags
      name     = each.value.name
      # Complex logic for each network interface (could be avoided)
    }
    ```
    
    **Better Practice**:
    
    ```hcl
    resource "azurerm_network_interface" "example" {
      name     = "example-nic"
      location = "East US"
      # Avoid unnecessary use of `for_each` if not needed
    }
    ```
    
3. **Hardcoding Values in Resources**:
    
    - Avoid hardcoding values for fields that are likely to change. Instead, leverage `locals` to centralize and manage these values, reducing redundancy and potential errors.
        
    
    **Bad Practice**:
    
    ```hcl
    resource "azurerm_virtual_machine" "example" {
      name                  = "vm1"  # Hardcoding the name
      resource_group_name   = "example-rg"  # Repeated throughout
      location              = "East US"  # Hardcoding
      # This creates redundancy
    }
    ```
    
    **Better Practice**:
    
    ```hcl
    locals {
      location = "East US"
      rg_name  = "example-rg"
    }
    
    resource "azurerm_virtual_machine" "example" {
      name                  = "vm1"
      resource_group_name   = local.rg_name
      location              = local.location
    }
    ```
    

---

## **Summary of Example 3**

In this example, we used **locals** to organize tags for each VM and **dynamic blocks** to apply these tags dynamically. The approach allows flexibility in managing tags for multiple VMs, where each VM might have different tags. By using `locals` to centralize the tags, and `dynamic` blocks to loop over them, we keep the Terraform configuration DRY and maintainable.

---

### Next Steps:

Now that you've seen how **locals** and **dynamic blocks** work in practice with VMs, I can provide additional examples (e.g., more complex Azure networking, security policies, or resource scaling).

Would you like to continue with more examples or dive deeper into a specific use case, or adjust anything from the current examples?



Excellent! Let's move on to **Example 4**: **Dynamic Scaling of Azure App Service Plan with Locals and Dynamic Blocks**.

### **Use Case**: You need to manage an Azure App Service Plan that scales based on a set of rules. The scaling rules (number of instances and CPU limits) are defined in `locals`, and dynamic blocks are used to apply these scaling rules to the App Service Plan.

---

### **Module Goal**:

- Use `locals` to define scaling rules for the App Service Plan.
    
- Use `dynamic` blocks to dynamically generate the scaling configuration.
    

---

### **1. Full Production-Level Terraform Module**

#### `main.tf`

```hcl
provider "azurerm" {
  features {}
}

# Locals to define scaling rules
locals {
  scaling_rules = [
    {
      tier        = "Standard"
      instance_count = 2
      cpu_limit     = 80
    },
    {
      tier        = "Premium"
      instance_count = 4
      cpu_limit     = 60
    }
  ]
}

# Define the App Service Plan
resource "azurerm_app_service_plan" "example" {
  for_each = { for rule in local.scaling_rules : rule.tier => rule }

  name                = "example-asp-${each.key}"
  location            = "East US"
  resource_group_name = "example-rg"
  kind                = "App"
  reserved            = false
  sku {
    tier = each.value.tier
    size = "P1v2"
  }

  dynamic "scaling_rule" {
    for_each = [each.value]
    content {
      instance_count = scaling_rule.value.instance_count
      cpu_limit      = scaling_rule.value.cpu_limit
    }
  }
}
```

---

### **2. Module Call Example**

In the root module, call the `azure_app_service_plan` module like this:

#### `root.tf`

```hcl
module "app_service_plan" {
  source = "./modules/azure_app_service_plan"
}
```

---

### **3. Architecture Diagram (ASCII)**

```
+-------------------+
|                   |
|  Resource Group   |  
|  example-rg       |
|                   |
+--------+----------+
         |
         v
+--------+------------------+
|                           |
|     Azure App Service     |
|      Plan example-asp     |
|                           |
+--------+-------------------+
         |
         v
+--------+------------------+
|        Scaling Rules      |
|  Tier: Standard           |
|  Instance Count: 2        |
|  CPU Limit: 80            |
+---------------------------+
         |
         v
+--------+------------------+
|        Scaling Rules      |
|  Tier: Premium            |
|  Instance Count: 4        |
|  CPU Limit: 60            |
+---------------------------+
```

---

### **4. Evaluation & Rendering Flow**

#### **Locals**:

- **Evaluation**:
    
    - Terraform evaluates the `locals` block at **plan time** to build a list of scaling rules. Each rule contains the `tier`, `instance_count`, and `cpu_limit`, which will be used in the App Service Plan configuration.
        
    - These values are computed once, and any changes to the `locals` block will trigger a re-evaluation.
        

#### **Dynamic Blocks**:

- **Evaluation of Dynamic Block**:
    
    - The `dynamic "scaling_rule"` block loops over the scaling rules and dynamically creates a scaling configuration for each App Service Plan tier (`Standard`, `Premium`).
        
    - **Rendering**: If the scaling rules change (e.g., the instance count or CPU limit), Terraform will regenerate the scaling configuration dynamically during the **resource creation phase**. If any of the scaling parameters are modified, Terraform will update the App Service Plan accordingly.
        

---

### **5. Anti-Patterns**

1. **Overuse of Dynamic Blocks for Static Values**:
    
    - If the scaling rules are relatively static (i.e., not changing frequently), it's better to define them directly in the resource block instead of using `dynamic`. This simplifies the configuration and improves readability.
        
    
    **Bad Practice**:
    
    ```hcl
    dynamic "scaling_rule" {
      for_each = [each.value]
      content {
        instance_count = scaling_rule.value.instance_count
        cpu_limit      = scaling_rule.value.cpu_limit
      }
    }
    ```
    
    **Better Practice**:
    
    ```hcl
    scaling_rule {
      instance_count = each.value.instance_count
      cpu_limit      = each.value.cpu_limit
    }
    ```
    
    Use `dynamic` only when the content may change dynamically (e.g., based on inputs or external conditions).
    
2. **Hardcoding Resource Parameters**:
    
    - Avoid hardcoding values like `tier = "Standard"` or `size = "P1v2"`. Instead, use variables or locals for flexibility and to reduce redundancy.
        
    
    **Bad Practice**:
    
    ```hcl
    resource "azurerm_app_service_plan" "example" {
      name                = "example-asp"
      location            = "East US"
      resource_group_name = "example-rg"
      kind                = "App"
      reserved            = false
      sku {
        tier = "Standard"  # Hardcoded value
        size = "P1v2"      # Hardcoded value
      }
    }
    ```
    
    **Better Practice**:
    
    ```hcl
    locals {
      tier   = "Standard"
      size   = "P1v2"
    }
    
    resource "azurerm_app_service_plan" "example" {
      name                = "example-asp"
      location            = "East US"
      resource_group_name = "example-rg"
      kind                = "App"
      reserved            = false
      sku {
        tier = local.tier
        size = local.size
      }
    }
    ```
    
3. **Unnecessary Complexity in Resource Blocks**:
    
    - If you don’t need complex scaling configurations, don’t use `dynamic` blocks. For simple cases where you're not dealing with complex lists, keep your resource blocks as simple as possible.
        
    
    **Bad Practice**:
    
    ```hcl
    dynamic "scaling_rule" {
      for_each = [each.value]
      content {
        instance_count = scaling_rule.value.instance_count
        cpu_limit      = scaling_rule.value.cpu_limit
      }
    }
    ```
    
    **Better Practice**:  
    Use a straightforward approach for simple scaling:
    
    ```hcl
    resource "azurerm_app_service_plan" "example" {
      name                = "example-asp"
      location            = "East US"
      resource_group_name = "example-rg"
      kind                = "App"
      reserved            = false
      sku {
        tier = each.value.tier
        size = "P1v2"
      }
      scaling_rule {
        instance_count = each.value.instance_count
        cpu_limit      = each.value.cpu_limit
      }
    }
    ```
    

---

## **Summary of Example 4**

This example demonstrates how to use **locals** to define scaling rules (e.g., instance counts and CPU limits) and how to use **dynamic blocks** to generate the scaling configuration for an Azure App Service Plan. It helps manage complex scaling configurations in a more flexible way, allowing you to scale the App Service Plan based on different rules for each tier. The use of `locals` and `dynamic` blocks makes it easy to modify and scale the configuration when needed.

---

### Next Steps:

You now have a solid understanding of how to work with **locals** and **dynamic blocks** in Azure resources for scaling scenarios. Would you like to:

1. **Dive deeper into another specific scenario** (e.g., storage account performance tuning, or dynamic access controls)?
    
2. **Request another set of examples** (e.g., managing infrastructure with dynamic policies, more complex networking)?
    
3. **Review any part** of the examples so far?
    

Let me know what you'd like to do next!



Great! Let’s move on to **Example 5**: **Dynamic Security Group Rules in Azure with Locals and Dynamic Blocks**.

### **Use Case**: You need to configure security group rules for an Azure Network Security Group (NSG) dynamically based on a set of incoming and outgoing network rules. The rules are defined in `locals`, and dynamic blocks are used to apply those rules to the NSG.

---

### **Module Goal**:

- Use `locals` to define a set of security group rules.
    
- Use `dynamic` blocks to apply those rules to an Azure NSG.
    

---

### **1. Full Production-Level Terraform Module**

#### `main.tf`

```hcl
provider "azurerm" {
  features {}
}

# Locals to define security group rules
locals {
  security_group_rules = [
    {
      name                     = "allow-http"
      priority                 = 100
      direction                = "Inbound"
      access                   = "Allow"
      protocol                 = "Tcp"
      source_port_range        = "*"
      destination_port_range   = "80"
      source_address_prefix    = "0.0.0.0/0"
      destination_address_prefix = "0.0.0.0/0"
    },
    {
      name                     = "allow-https"
      priority                 = 200
      direction                = "Inbound"
      access                   = "Allow"
      protocol                 = "Tcp"
      source_port_range        = "*"
      destination_port_range   = "443"
      source_address_prefix    = "0.0.0.0/0"
      destination_address_prefix = "0.0.0.0/0"
    },
    {
      name                     = "deny-all"
      priority                 = 4096
      direction                = "Inbound"
      access                   = "Deny"
      protocol                 = "*"
      source_port_range        = "*"
      destination_port_range   = "*"
      source_address_prefix    = "*"
      destination_address_prefix = "*"
    }
  ]
}

# Create the Network Security Group
resource "azurerm_network_security_group" "example" {
  name                = "example-nsg"
  location            = "East US"
  resource_group_name = "example-rg"

  dynamic "security_rule" {
    for_each = local.security_group_rules
    content {
      name                       = security_rule.value.name
      priority                   = security_rule.value.priority
      direction                  = security_rule.value.direction
      access                     = security_rule.value.access
      protocol                   = security_rule.value.protocol
      source_port_range          = security_rule.value.source_port_range
      destination_port_range     = security_rule.value.destination_port_range
      source_address_prefix      = security_rule.value.source_address_prefix
      destination_address_prefix = security_rule.value.destination_address_prefix
    }
  }
}

# Define a network interface to attach the NSG to
resource "azurerm_network_interface" "example" {
  name                = "example-nic"
  location            = "East US"
  resource_group_name = "example-rg"
  ip_configuration {
    name                          = "internal"
    subnet_id                    = azurerm_subnet.example.id
    private_ip_address_allocation = "Dynamic"
    network_security_group_id     = azurerm_network_security_group.example.id
  }
}
```

---

### **2. Module Call Example**

In your root module, you would call the `azure_nsg` module as follows:

#### `root.tf`

```hcl
module "network_security_group" {
  source = "./modules/azure_nsg"
}
```

---

### **3. Architecture Diagram (ASCII)**

```
+-------------------+
|                   |
|  Resource Group   |  
|  example-rg       |
|                   |
+--------+----------+
         |
         v
+--------+----------------------+
|                            |
|   Network Security Group   |
|     example-nsg            |
|                            |
+-----------+----------------+
            |
            v
 +----------------------------+
 |   Security Rules (Dynamic) |
 |   1. allow-http            |
 |   2. allow-https           |
 |   3. deny-all              |
 +----------------------------+
            |
            v
 +----------------------------+
 |                            |
 |   Network Interface        |
 |   example-nic              |
 |                            |
 +----------------------------+
```

---

### **4. Evaluation & Rendering Flow**

#### **Locals**:

- **Evaluation**:
    
    - Terraform evaluates the `locals` block to create a list of network security group rules at **plan time**. The rules define the properties such as `name`, `priority`, `direction`, `access`, etc., for each rule in the NSG.
        
    - These values are computed once and stored in memory for use in the resource definition. The `locals` block helps to centralize the rule definitions, making it easier to manage them.
        

#### **Dynamic Blocks**:

- **Evaluation of Dynamic Block**:
    
    - The `dynamic "security_rule"` block is evaluated during **resource creation**. It dynamically generates security rules for each entry in `local.security_group_rules`.
        
    - For each security rule, Terraform creates a corresponding rule in the Azure Network Security Group (`azurerm_network_security_group`).
        
    - **Rendering**: Each rule’s attributes are mapped from `locals` and applied to the security group configuration. For example:
        
        - Rule 1 (`allow-http`) will have ports 80 open for inbound traffic.
            
        - Rule 2 (`allow-https`) will allow inbound traffic on port 443.
            
        - Rule 3 (`deny-all`) will deny all traffic that doesn’t match the previous rules.
            

---

### **5. Anti-Patterns**

1. **Overuse of Dynamic Blocks for Simple Security Rules**:
    
    - If the security rules are simple or not expected to change often, avoid using `dynamic` blocks. Instead, define them directly in the resource block.
        
    
    **Bad Practice**:
    
    ```hcl
    dynamic "security_rule" {
      for_each = local.security_group_rules
      content {
        name                       = security_rule.value.name
        priority                   = security_rule.value.priority
        direction                  = security_rule.value.direction
        access                     = security_rule.value.access
        protocol                   = security_rule.value.protocol
        source_port_range          = security_rule.value.source_port_range
        destination_port_range     = security_rule.value.destination_port_range
        source_address_prefix      = security_rule.value.source_address_prefix
        destination_address_prefix = security_rule.value.destination_address_prefix
      }
    }
    ```
    
    **Better Practice**:  
    Define security rules directly in the `azurerm_network_security_group` resource if they are static and unlikely to change frequently:
    
    ```hcl
    security_rule {
      name                       = "allow-http"
      priority                   = 100
      direction                  = "Inbound"
      access                     = "Allow"
      protocol                   = "Tcp"
      source_port_range          = "*"
      destination_port_range     = "80"
      source_address_prefix      = "0.0.0.0/0"
      destination_address_prefix = "0.0.0.0/0"
    }
    ```
    
2. **Using Dynamic Blocks for Every Rule**:
    
    - For complex configurations that don’t need to be dynamically created, using `dynamic` for every single rule can make the configuration unnecessarily complex. If the rules are static, hard-coding them is usually simpler.
        
    
    **Bad Practice**:
    
    ```hcl
    dynamic "security_rule" {
      for_each = local.security_group_rules
      content {
        name                       = security_rule.value.name
        priority                   = security_rule.value.priority
        direction                  = security_rule.value.direction
        access                     = security_rule.value.access
        protocol                   = security_rule.value.protocol
        source_port_range          = security_rule.value.source_port_range
        destination_port_range     = security_rule.value.destination_port_range
        source_address_prefix      = security_rule.value.source_address_prefix
        destination_address_prefix = security_rule.value.destination_address_prefix
      }
    }
    ```
    
    **Better Practice**:  
    Use `for_each` only where necessary and simplify when possible:
    
    ```hcl
    resource "azurerm_network_security_group" "example" {
      name                = "example-nsg"
      location            = "East US"
      resource_group_name = "example-rg"
      security_rule {
        name                       = "allow-http"
        priority                   = 100
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "80"
        source_address_prefix      = "0.0.0.0/0"
        destination_address_prefix = "0.0.0.0/0"
      }
    }
    ```
    
3. **Not Using Locals for Centralized Configuration**:
    
    - It's essential to keep configuration data like rules centralized. Avoid defining the same rules repeatedly in multiple places, which can lead to errors when trying to update the configuration.
        
    
    **Bad Practice**:
    
    ```hcl
    resource "azurerm_network_security_group" "example" {
      name                = "example-nsg"
      location            = "East US"
      resource_group_name = "example-rg"
      security_rule {
        name                       = "allow-http"
        priority                   = 100
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "80"
        source_address_prefix      = "0.0.0.0/0"
        destination_address_prefix = "0.0.0.0/0"
      }
    ```


