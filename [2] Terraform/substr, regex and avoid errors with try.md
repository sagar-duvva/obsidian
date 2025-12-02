
how to combine substr with regex (regex()), how to safely avoid errors with try() and a real-world use case (naming, tagging, random suffixes)


Below are **three complete examples** showing:

1. ✅ How to **combine `substr` with `regex()`**
    
2. ✅ How to **safely avoid errors with `try()`**
    
3. ✅ A **real-world use case** (naming, tagging, random suffixes)
    

Each example is “copy-paste ready.”

---

# 1️⃣ Combine `substr` with `regex()`

Often you want to extract part of a string using regex, and then take a substring from the match.

### **Example: Extract the environment code (e.g., "prod123")**

```hcl
locals {
  resource_name = "app-prod123-westus"
}

# Extract the "prod123" using regex
locals {
  env_code_full = regex("prod[0-9]+", local.resource_name)
}

# Take only the "prod" prefix from the matched code
locals {
  env_code_prefix = substr(local.env_code_full, 0, 4)
}

output "env_code_full"   { value = local.env_code_full }
output "env_code_prefix" { value = local.env_code_prefix }
```

**Result:**

- `env_code_full` → `prod123`
    
- `env_code_prefix` → `prod`
    

---

# 2️⃣ Avoid substring errors with `try()`

Terraform throws an error if `substr()` goes out of bounds, if regex has no match, or if the value is null.  
Use `try()` to safely catch errors.

### **Safe version of the above example**

```hcl
locals {
  resource_name = "app-westus" # no prod123 here
}

locals {
  env_code_full = try(regex("prod[0-9]+", local.resource_name), null)
}

locals {
  env_code_prefix = try(substr(local.env_code_full, 0, 4), "unknown")
}

output "env_code_full"   { value = local.env_code_full }
output "env_code_prefix" { value = local.env_code_prefix }
```

**Result:**

- `env_code_full` → `null`
    
- `env_code_prefix` → `"unknown"` (fallback instead of error)
    

---

# 3️⃣ Real-World Use Case — Naming, Tagging & Random Suffixes

This combines `regex()`, `substr()`, and `try()` in a robust pattern used in production.

## 🔵 Scenario:

You have service names like:

```
api-prod-eu-west-01
```

You want to extract:

- environment: `prod`
    
- region: the 2-letter region code (first 2 letters of region)
    
- plus a random suffix
    

### **Solution**

```hcl
locals {
  service_name = "api-prod-eu-west-01"
}

# Extract environment (prod, dev, stg, qa, etc.)
locals {
  environment = try(regex("(prod|dev|stg|qa)", local.service_name), "unknown")
}

# Extract region such as "eu" from "eu-west-01"
locals {
  region_full = try(regex("[a-z]{2}-[a-z]+-[0-9]+", local.service_name), null)
  region_code = try(substr(local.region_full, 0, 2), "xx")
}

# Random suffix
resource "random_string" "suffix" {
  length  = 4
  special = false
  upper   = false
}

locals {
  final_name = "${local.environment}-${local.region_code}-${random_string.suffix.result}"
}

output "environment" { value = local.environment }
output "region_code" { value = local.region_code }
output "final_name"  { value = local.final_name }
```

### **Example Output**

```
environment = "prod"
region_code = "eu"
final_name  = "prod-eu-a3f9"
```

---

# ✅ Summary Cheatsheet

### **Combine substr + regex**

Use regex to extract, substr to trim.

```hcl
substr(regex("pattern", var.text), 0, 4)
```

### **Safe version**

```hcl
try(substr(regex("pattern", var.text), 0, 4), "fallback")
```

### **Common production pattern**

- `regex()` → extract
    
- `substr()` → shorten
    
- `try()` → avoid errors
    
- Random suffix → uniqueness
    
- Combine for robust naming/tags
    

---

If you'd like, I can also provide:

📌 A module-ready naming convention example (Azure, AWS, GCP)  
📌 A version that auto-normalizes names (lowercase, no symbols)  
📌 A `regexall()` example for multiple captures

Would you like any of those?