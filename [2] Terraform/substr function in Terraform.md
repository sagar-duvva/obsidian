

Here’s a clear, practical explanation of the **`substr` function in Terraform**, with examples you can copy-paste.

---

# ✅ **Terraform `substr` Function**

The `substr()` function extracts a substring from a given string, starting at a specified index and for a specified length.

### **Syntax**

```hcl
substr(string, offset, length)
```

### **Parameters**

|Parameter|Description|
|---|---|
|`string`|The original string|
|`offset`|Zero-based index where extraction begins (can be negative)|
|`length`|Number of characters to extract (can be negative)|

---

# 📌 **Examples**

### **1. Basic usage**

```hcl
output "example" {
  value = substr("Terraform", 0, 4)
}
```

**Output:**

```
Terr
```

---

### **2. Offset starting from the end (negative offset)**

```hcl
output "from_end" {
  value = substr("Terraform", -4, 4)
}
```

**Output:**

```
form
```

---

### **3. Extract with negative length**

A negative length returns all characters _except_ the last N chars.

```hcl
output "neg_length" {
  value = substr("Terraform", 0, -4)
}
```

**Output:**

```
Terra
```

---

### **4. Dynamic substring for IDs or tags**

```hcl
locals {
  instance_id = "i-0a1b2c3d4e"
}

output "short_id" {
  value = substr(local.instance_id, 2, 6)
}
```

**Output:**

```
0a1b2c
```

---

# ⚠️ Notes & Edge Cases

- If the offset is outside the string, Terraform throws an error.
    
- If `length` exceeds remaining characters, it returns what is available.
    
- Use `length()` when you need dynamic slicing:
    
    ```hcl
    substr(var.name, 0, length(var.name) - 2)
    ```
    

---

If you want, I can also show:  
✅ a real-world use case (naming, tagging, random suffixes)  
✅ how to combine `substr` with regex (`regex()`)  
✅ how to safely avoid errors with `try()`

Would you like examples?