
---


**Short answer:** _Terraform supports three main looping patterns — `for` expressions (list/map comprehensions), the `for_each` meta-argument, and the `count` meta-argument — plus `dynamic` blocks for nested repeated blocks; use `for` to transform collections, `for_each`/`count` to create multiple resource/module instances, and `dynamic` to generate repeated nested blocks in a resource or module_.

### Overview of loop types

**`count`** creates N instances of a resource or module; **`for_each`** iterates a set or map to create one instance per item; **`for` expressions** transform/filter lists and maps inside expressions; **`dynamic`** builds repeated nested blocks inside a resource. These behaviors are documented in the Terraform language reference [HashiCorp Developer](https://developer.hashicorp.com/terraform/language/meta-arguments/count) [HashiCorp Developer](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each) [HashiCorp Developer](https://developer.hashicorp.com/terraform/language/expressions).

---

### For expressions — 10 real-world examples

- **Prefix ports**: `join("-", [for p in split(",", var.ports) : "port-${trimspace(p)}"])`
- **Lowercase tags**: `[for t in var.tags : lower(t)]`
- **Filter healthy servers**: `[for s in local.servers : s if s.status == "healthy"]`
- **Map to objects**: `{ for i, v in var.names : v => { index = i } }`
- **Extract IDs**: `[for r in aws_instance.app : r.id]`
- **Unique list**: `distinct([for u in var.users : u.email])`
- **Convert strings to numbers**: `[for s in split(",", var.nums) : tonumber(trimspace(s))]`
- **Flatten pairs**: `flatten([for m in var.maps : values(m)])`
- **Conditional transform**: `[for p in var.ports : p == "80" ? "http" : "other"]`
- **Build resource names**: `[for i in range(var.count) : "${var.prefix}-${i}"]`

---

### for_each meta-argument — 10 real-world examples

- **Create DNS records**: `for_each = var.dns_records` (map name→value)
- **Per-AZ subnets**: `for_each = toset(var.azs)` create one subnet per AZ
- **IAM users from map**: `for_each = var.users` (map username→attrs)
- **Security group rules**: `for_each = { for r in var.rules : r.name => r }`
- **Per-environment modules**: `for_each = var.environments`
- **S3 buckets per team**: `for_each = var.teams`
- **Per-key vault secrets**: `for_each = var.secrets_map`
- **Create tags per resource**: `for_each = local.tag_map`
- **Per-region resources**: `for_each = toset(var.regions)`
- **Conditional creation**: `for_each = var.enabled ? var.items : {}`

---

### count meta-argument — 10 real-world examples

- **Create N identical VMs**: `count = var.instance_count`
- **Optional resource**: `count = var.create ? 1 : 0`
- **Multiple EBS volumes**: `count = var.volume_count`
- **Create test users**: `count = var.test_user_count`
- **Scale replicas**: `count = var.replicas`
- **Create multiple NICs**: `count = length(var.nics)`
- **Index-based names**: `name = "${var.name}-${count.index}"`
- **Conditional subresource**: `count = var.enable_feature ? 1 : 0`
- **Create placeholder resources for plan-only**: `count = var.dry_run ? 0 : 1`
- **Repeat simple resources**: `count = 3` for three identical items

---

### Dynamic blocks & nested loops — 10 real-world examples

- **Multiple ingress rules**: `dynamic "ingress" { for_each = var.ingress_rules ... }`
- **Multiple lifecycle rules in S3**: `dynamic "rule" { for_each = var.lifecycle }`
- **Nested blocks per AZ/subnet**: precompute pairs and `for_each` in `dynamic`
- **Multiple listener rules**: `dynamic "default_action" { for_each = var.actions }`
- **Per-user policy statements**: `dynamic "statement" { for_each = var.statements }`
- **Complex nested maps → blocks**: flatten then `dynamic` over list
- **Multiple backend blocks**: `dynamic "backend" { for_each = var.backends }`
- **Multiple mount points**: `dynamic "mount" { for_each = var.mounts }`
- **Multiple health checks**: `dynamic "health_check" { for_each = var.hc }`
- **Nested security rules per interface**: compute map of interfaces→rules, then `dynamic`

---

### Quick tips

- **Use `for` expressions for transformation/filtering**, `for_each` when instances need stable keys, and `count` for simple numeric repetition. **Prefer `for_each` with maps/sets for predictable instance addresses**. For nested complexity, precompute locals and use `dynamic` blocks.

**Sources:** Terraform docs on `count`, `for_each`, and expressions [HashiCorp Developer](https://developer.hashicorp.com/terraform/language/meta-arguments/count) [HashiCorp Developer](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each) [HashiCorp Developer](https://developer.hashicorp.com/terraform/language/expressions).