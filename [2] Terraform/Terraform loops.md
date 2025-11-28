

---
---
---


teach me terraform loops like for, etc with 10 real-world examples for each topic

---

Sure! Let's dive into **Terraform loops**, specifically **`for` loops**, **`for_each`**, and **`count`**. I'll walk you through the key Terraform looping constructs and provide 10 real-world examples for each of them.

---

### 1. **`for` Loop (for expressions)**

The **`for`** loop in Terraform allows you to iterate over a list or map, transforming the elements in the process.

#### Syntax:

```hcl
[for element in collection : expression]
```

Here’s how it works: you iterate over a collection (like a list or map) and apply a transformation to each element, which can be filtered or modified.

---

#### Real-World Examples of `for` Loops:

1. **Create a List of EC2 Instances with Dynamic Names:**  
    Use a `for` loop to dynamically generate names for instances based on a list.
    
    ```hcl
    locals {
      instance_names = [for i in range(5) : "instance-${i}"]
    }
    output "instance_names" {
      value = local.instance_names
    }
    ```
    
2. **Generate a List of Tags for Resources:**  
    If you have a list of keys and want to create tags dynamically:
    
    ```hcl
    locals {
      tags = { for key in ["Name", "Environment", "Team"] : key => "example-${key}" }
    }
    resource "aws_instance" "example" {
      tags = local.tags
    }
    ```
    
3. **Filter a List Based on a Condition:**  
    Use a `for` loop with filtering to only include certain values.
    
    ```hcl
    locals {
      numbers = [1, 2, 3, 4, 5, 6]
      even_numbers = [for num in local.numbers : num if num % 2 == 0]
    }
    output "even_numbers" {
      value = local.even_numbers
    }
    ```
    
4. **Create Multiple Security Groups with Different Names:**  
    Create multiple security groups dynamically using `for` loops.
    
    ```hcl
    locals {
      security_group_names = ["sg-web", "sg-db", "sg-app"]
    }
    
    resource "aws_security_group" "example" {
      for_each = toset(local.security_group_names)
      name     = each.key
    }
    ```
    
5. **Create Multiple Key-Value Pairs in a Map:**  
    If you need a list of key-value pairs from a list:
    
    ```hcl
    locals {
      keys = ["apple", "banana", "cherry"]
      fruit_map = { for key in local.keys : key => "${key}-fruit" }
    }
    output "fruit_map" {
      value = local.fruit_map
    }
    ```
    
6. **Constructing a Map of Instances and Their IPs:**  
    Suppose you have a list of instance names and IPs:
    
    ```hcl
    locals {
      instance_data = [
        { name = "web-1", ip = "192.168.1.1" },
        { name = "db-1", ip = "192.168.1.2" }
      ]
      instance_ips = { for data in local.instance_data : data.name => data.ip }
    }
    output "instance_ips" {
      value = local.instance_ips
    }
    ```
    
7. **Generate a List of Resource IDs from a Set of Resources:**
    
    ```hcl
    resource "aws_instance" "example" {
      count = 5
    }
    
    output "instance_ids" {
      value = [for instance in aws_instance.example : instance.id]
    }
    ```
    
8. **Create Multiple S3 Buckets Based on a List:**
    
    ```hcl
    locals {
      bucket_names = ["bucket-a", "bucket-b", "bucket-c"]
    }
    
    resource "aws_s3_bucket" "example" {
      for_each = toset(local.bucket_names)
      bucket   = each.key
    }
    ```
    
9. **Generate EC2 Instance Tags Dynamically:**
    
    ```hcl
    locals {
      instance_ids = ["i-1234567890", "i-0987654321"]
      tags = { for id in local.instance_ids : id => "Tag-${id}" }
    }
    
    resource "aws_instance" "example" {
      count = length(local.instance_ids)
      tags  = local.tags
    }
    ```
    
10. **Create a List of Security Group Rules Dynamically:**  
    Generate a list of inbound rules based on allowed IPs.
    

```hcl
locals {
  allowed_ips = ["10.0.0.1", "10.0.0.2"]
}

resource "aws_security_group" "example" {
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = [for ip in local.allowed_ips : "${ip}/32"]
  }
}
```

---

### 2. **`for_each` Loop**

The **`for_each`** loop allows you to iterate over a map or set, and it will create one resource for each element in the collection.

#### Syntax:

```hcl
resource "resource_type" "name" {
  for_each = collection
  ...
}
```

`for_each` is very powerful because it gives you more flexibility with keys in the map.

---

#### Real-World Examples of `for_each` Loops:

1. **Create Multiple Security Groups from a Map:**
    
    ```hcl
    locals {
      security_groups = {
        sg_web = "Web Security Group"
        sg_db  = "Database Security Group"
      }
    }
    
    resource "aws_security_group" "example" {
      for_each = local.security_groups
      name     = each.key
      description = each.value
    }
    ```
    
2. **Create Multiple Instances with Different Sizes:**
    
    ```hcl
    locals {
      instance_sizes = {
        web = "t2.micro"
        db  = "t2.medium"
      }
    }
    
    resource "aws_instance" "example" {
      for_each = local.instance_sizes
      instance_type = each.value
      ami            = "ami-12345678"
      tags = {
        Name = each.key
      }
    }
    ```
    
3. **Create Multiple Subnets Based on Region:**
    
    ```hcl
    locals {
      regions = ["us-east-1", "us-west-1", "eu-west-1"]
    }
    
    resource "aws_subnet" "example" {
      for_each = toset(local.regions)
      cidr_block = "10.0.1.0/24"
      availability_zone = each.key
    }
    ```
    
4. **Deploy Different EC2 Instances in Different Availability Zones:**
    
    ```hcl
    locals {
      azs = ["us-east-1a", "us-east-1b"]
    }
    
    resource "aws_instance" "example" {
      for_each = toset(local.azs)
      ami            = "ami-12345678"
      instance_type  = "t2.micro"
      availability_zone = each.key
    }
    ```
    
5. **Create Multiple CloudWatch Alarms for Different Metrics:**
    
    ```hcl
    locals {
      alarms = {
        cpu = "AWS/EC2 CPUUtilization"
        disk = "AWS/EC2 DiskReadOps"
      }
    }
    
    resource "aws_cloudwatch_metric_alarm" "example" {
      for_each = local.alarms
      metric_name = each.value
      namespace   = "AWS/EC2"
      comparison_operator = "GreaterThanOrEqualToThreshold"
      threshold = 80
      statistic = "Average"
    }
    ```
    
6. **Deploy Multiple EC2 Instances with Different Tags:**
    
    ```hcl
    locals {
      instances = {
        web  = "t2.micro"
        db   = "t2.large"
      }
    }
    
    resource "aws_instance" "example" {
      for_each = local.instances
      instance_type = each.value
      ami            = "ami-12345678"
      tags = {
        Name = each.key
      }
    }
    ```
    
7. **Create Multiple Load Balancers:**
    
    ```hcl
    locals {
      lb_names = ["web-lb", "app-lb", "db-lb"]
    }
    
    resource "aws_lb" "example" {
      for_each = toset(local.lb_names)
      name     = each.key
      internal = false
      load_balancer_type = "application"
      security_groups = ["sg-12345678"]
      subnets = ["subnet-12345678", "subnet-23456789"]
    }
    ```
    
8. **Provision Multiple RDS Databases:**
    
    ```hcl
    locals {
      dbs = {
        db1 = "mysql"
        db2 = "postgres"
      }
    }
    
    resource "aws_db_instance" "example" {
      for_each = local.dbs
      engine   = each.value
      instance_class = "db.t2.micro"
      allocated_storage = 20
    }
    ```
    
9. **Assign IAM Roles to Multiple Users:**
    
    ```hcl
    locals {
      users = ["user1", "user2", "user3"]
    }
    
    resource "aws_iam_user" "example" {
      for_each = toset(local.users)
      name = each.key
    }
    ```
    
1. **Create Multiple VPC Peering


---
---


