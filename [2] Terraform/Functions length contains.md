

```hcl
variable "vm_size" {
  type = string

  validation {
    condition = (
      length(var.vm_size) >= 2 &&
      length(var.vm_size) <= 20 &&
      contains(lower(var.vm_size), "standard") &&
      contains(["Standard_B2s", "Standard_D2as_v6"], var.vm_size)
    )

    error_message = "VM size must be either \"Standard_B2s\" or \"Standard_D2as_v6\", contain \"standard\", and be 2–20 characters long."
  }
}
```



The error occurs because **`contains()` requires the _first_ argument to be a list**, but in this expression:

```hcl
contains(lower(var.vm_size), "standard")
```

`lower(var.vm_size)` is a **string**, not a list — so Terraform throws:

> Call to function "contains" failed: argument must be list, tuple, or set.

### ✅ How to fix it

If you only need to check that `"standard"` exists in the string, use **`can()`** with **`regex()`**, because Terraform has no native "string contains" function.

### Corrected validation example:

```hcl
variable "vm_size" {
  type = string

  validation {
    condition = (
      length(var.vm_size) >= 2 &&
      length(var.vm_size) <= 20 &&
      can(regex("standard", lower(var.vm_size))) &&
      contains(["Standard_B2s", "Standard_D2as_v6"], var.vm_size)
    )

    error_message = "VM size must be either \"Standard_B2s\" or \"Standard_D2as_v6\", contain \"standard\", and be 2–20 characters long."
  }
}
```

### 🔍 Why this works

- **`can(regex(...))`** → safely checks if `"standard"` appears anywhere in the string.
    
- **`contains([list], var.vm_size)`** → valid, since the first argument is a list.
    
- Length checks remain unchanged.
    

If you want, I can produce the full corrected `main.tf` variable block again.