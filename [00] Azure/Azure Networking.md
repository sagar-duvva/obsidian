
1
1

---
---

## 1. **Azure Load Balancer (ALB)** 

[[Azure Load Balancer (Layer 4)]]
**Type:** Layer 4 (Transport Layer) Load Balancer  
**Use Case:** Distribute traffic at the TCP/UDP level, for internal or external endpoints.

**Key Features:**

- Works at **Layer 4** (TCP/UDP traffic)
- Distributes traffic across **VMs, scale sets, or IP addresses**
- Supports **internal (private)** or **public (internet-facing)** load balancing
- Health probes to detect backend availability
- High throughput and low latency
- No SSL termination or web application firewall (WAF)

**Best for:**

- High-performance, low-level traffic distribution
- Non-HTTP workloads (e.g., database traffic, gaming servers)
- Simple network-level load balancing

---

## 2. **Azure Application Gateway**

[[Azure Application Gateway (Layer 7)]]
**Type:** Layer 7 (Application Layer) Load Balancer / Web Application Firewall (WAF)  
**Use Case:** Web traffic routing, SSL termination, and protection with WAF.

**Key Features:**

- Works at **Layer 7 (HTTP/HTTPS)**
- **URL-based routing**, host-based routing
- Supports **SSL termination** (offloading encryption/decryption)
- Can integrate **Web Application Firewall (WAF)** for security
- Session affinity (cookie-based)
- Autoscaling and zone redundancy
- Supports **rewrite rules**, redirection, and multi-site hosting

**Best for:**

- Web applications needing HTTP routing or WAF
- Scenarios where SSL termination is needed
- Applications requiring path-based or host-based routing

---

## 3. **Azure Front Door**

[[Azure Front Door (Global Layer 7)]]
**Type:** Global, Layer 7 (Application Layer) Load Balancer + CDN + WAF  
**Use Case:** Fast, global HTTP/HTTPS traffic delivery with caching, acceleration, and failover.

**Key Features:**

- Operates at **Layer 7** (HTTP/HTTPS)
- **Global Anycast**: traffic is routed to the nearest point of presence (PoP)
- Provides **web application acceleration** (caching, dynamic site acceleration)
- **SSL offload and WAF integration**
- **URL rewrite and redirection**, multiple backend pools
- Automatic **global failover** across regions
- Geolocation-based routing

**Best for:**

- Applications with **global users**
- Low-latency, high-performance web applications
- Scenarios requiring **global failover** and **multi-region redundancy**
- Combining **CDN caching** and **security** in one service


---

## ✅ Comparison Table

[[(3) Differences Azure Load Balancer vs Application Gateway vs Front Door]]]

| Feature                       | Azure Load Balancer      | Azure Application Gateway | Azure Front Door                  |
| ----------------------------- | ------------------------ | ------------------------- | --------------------------------- |
| OSI Layer                     | L4 (TCP/UDP)             | L7 (HTTP/HTTPS)           | L7 (HTTP/HTTPS)                   |
| Scope                         | Regional                 | Regional                  | Global                            |
| Traffic type                  | Any TCP/UDP              | HTTP/HTTPS                | HTTP/HTTPS                        |
| SSL Termination               | ❌                        | ✅                         | ✅                                 |
| WAF                           | ❌                        | ✅                         | ✅                                 |
| URL-based routing             | ❌                        | ✅                         | ✅                                 |
| Geo-routing / Global failover | ❌                        | ❌                         | ✅                                 |
| Caching / Acceleration        | ❌                        | ❌                         | ✅                                 |
| Internal vs Public            | Both                     | Public                    | Public only                       |
| Use case                      | High-performance TCP/UDP | Web apps, WAF, SSL        | Global web apps, CDN, low-latency |

---

### **Summary (Quick Rule of Thumb)**

- **Azure Load Balancer** → For raw TCP/UDP traffic within a region (think “network-level traffic”)
- **Azure Application Gateway** → For web applications needing WAF, SSL termination, and path-based routing within a region
- **Azure Front Door** → For global web apps needing low latency, CDN-like caching, and global failover



Visual Summary Diagram on how to combine all 3 Load Balancing Services
[[(3)Azure Network (Load Balancing)/Quick Visual Summary of Usage Azure Load Balancing]]


---
---
---


### **4. Azure Service Endpoint**

[[Service Endpoint (for Storage, SQL, Key Vault, etc.)]]
**Category:** **Virtual Network (VNet) Service Access / Network Security**

- Extends your **VNet identity** to an Azure service over the public endpoint.
- Traffic goes over the **Azure backbone**, but the service still uses a **public IP**.
- Commonly categorized under **VNet Integration Features** or **Service Access Controls**.

---

### **5. Azure Private Endpoint**

[[Private Endpoint (for Storage, SQL, Key Vault, etc.)]]
**Category:** **Private Access / Private Connectivity**

- A **network interface (NIC)** with a **private IP** from your VNet.
- Provides **private, direct access** to specific Azure PaaS resources.
- Falls under **Private Network Access** or **Private Connectivity Features**.

---

### **6. Azure Private Link**

[[Private Link (for exposing your own service)]]
**Category:** **Private Connectivity Platform**

- The broader platform that **enables** private endpoints.
- Provides **private connectivity** to Azure PaaS services and customer/partner services.
- Categorized under **Private Network Access Platform** or **Private Service Connectivity**.

---

## **Summary Table**

[[(3) Differences Between Service Endpoint, Private Endpoint, and Private Link]]

| Feature              | Category                               | Key Behavior                                            |
| -------------------- | -------------------------------------- | ------------------------------------------------------- |
| **Service Endpoint** | VNet Service Access / Network Security | Public service endpoint, secure path via Azure backbone |
| **Private Endpoint** | Private Access / Private Connectivity  | Private IP to a specific resource                       |
| **Private Link**     | Private Connectivity Platform          | Underlying platform enabling private PaaS access        |


Quick Visual Concept of Usage Azure Services
[[Quick Visual Concept of Usage Azure Services]]


---
---



---
---



---
---



---
---



---
---


