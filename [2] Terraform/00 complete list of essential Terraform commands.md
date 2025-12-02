

Here’s a **complete list of essential Terraform commands** with **summaries** and **one-liner examples** for each — perfect for cheat sheets or quick references:

---

### ⚙️ **Core Terraform Commands**

|Command|Summary|Example|
|---|---|---|
|`terraform init`|Initializes a working directory with Terraform configuration files and downloads provider plugins.|`terraform init`|
|`terraform validate`|Validates the syntax and structure of your Terraform configuration files.|`terraform validate`|
|`terraform plan`|Creates an execution plan showing what Terraform will do (create, update, destroy).|`terraform plan -out=tfplan`|
|`terraform apply`|Applies the changes required to reach the desired state.|`terraform apply tfplan`|
|`terraform destroy`|Destroys all infrastructure managed by Terraform.|`terraform destroy -auto-approve`|
|`terraform refresh`|Updates the Terraform state file with real-world resource data.|`terraform refresh`|
|`terraform output`|Displays output values defined in the configuration.|`terraform output vm_ip`|
|`terraform fmt`|Formats Terraform files to canonical style.|`terraform fmt`|
|`terraform taint`|Marks a resource for recreation during the next apply.|`terraform taint azurerm_virtual_machine.vm1`|
|`terraform untaint`|Removes the taint (marked for recreation) from a resource.|`terraform untaint azurerm_virtual_machine.vm1`|
|`terraform show`|Shows the current state or a saved plan file.|`terraform show`|
|`terraform state`|Advanced command to inspect and modify Terraform state.|`terraform state list`|

---

### 📦 **Modules & Providers**

|Command|Summary|Example|
|---|---|---|
|`terraform get`|Downloads and installs modules needed for the configuration.|`terraform get`|
|`terraform providers`|Lists all providers required by the configuration.|`terraform providers`|
|`terraform providers lock`|Locks provider versions in `.terraform.lock.hcl` for consistent installs.|`terraform providers lock`|

---

### ☁️ **Terraform Cloud & Workspaces**

|Command|Summary|Example|
|---|---|---|
|`terraform login`|Authenticates Terraform CLI with Terraform Cloud/Enterprise.|`terraform login`|
|`terraform logout`|Removes saved credentials for Terraform Cloud.|`terraform logout`|
|`terraform workspace list`|Lists all existing workspaces in the current configuration.|`terraform workspace list`|
|`terraform workspace new`|Creates a new workspace for managing a separate state.|`terraform workspace new dev`|
|`terraform workspace select`|Switches to a specific workspace.|`terraform workspace select prod`|
|`terraform workspace delete`|Deletes a given workspace.|`terraform workspace delete test`|

---

### 📄 **State File Management**

|Command|Summary|Example|
|---|---|---|
|`terraform state list`|Lists all resources in the current state.|`terraform state list`|
|`terraform state show`|Shows attributes of a specific resource in the state.|`terraform state show azurerm_vm.example`|
|`terraform state rm`|Removes a resource from the state file.|`terraform state rm azurerm_storage_account.example`|
|`terraform state mv`|Moves a resource in state from one address to another.|`terraform state mv old.name new.name`|

---

### 📂 **Plan & Apply Enhancements**

|Command|Summary|Example|
|---|---|---|
|`terraform apply -target`|Applies only the specified resource(s).|`terraform apply -target=azurerm_network_interface.nic1`|
|`terraform plan -var`|Passes a variable on the command line during plan.|`terraform plan -var="env=dev"`|
|`terraform apply -var-file`|Applies using variables from a file.|`terraform apply -var-file="prod.tfvars"`|

---

### 📡 **Remote Backend Commands (Optional)**

|Command|Summary|Example|
|---|---|---|
|`terraform init -backend-config`|Initializes Terraform with backend config for remote state.|`terraform init -backend-config="key=prod.terraform.tfstate"`|

---

### ✅ **Example Workflow**

```bash
terraform init
terraform validate
terraform plan -out=tfplan
terraform apply tfplan
terraform output
```

---

Would you like this as a downloadable `.txt`, `.md`, Notion-compatible text, or even as an Excel/CSV cheat sheet?