

---


## **1. Differences Between Service Endpoint, Private Endpoint, and Private Link**

|Feature|**Service Endpoint**|**Private Endpoint**|**Private Link**|
|---|---|---|---|
|**Definition**|Extends your VNet to access Azure PaaS services via Azure backbone while still using public IP.|A network interface in your VNet that connects privately to an Azure service using a private IP.|The underlying technology that allows Azure services to be accessible via private endpoints.|
|**IP Address Used**|Service’s public IP.|Private IP from your VNet/subnet.|N/A (enables private endpoint).|
|**Traffic Path**|Stays on Azure backbone, goes to public endpoint.|Goes directly to service over private IP, not exposed to public internet.|Mechanism enabling private endpoint; traffic flows privately via private IP.|
|**Security**|Restricts access to selected VNets/subnets.|Completely isolates the service from the public internet.|Allows creation of private endpoints to access services securely.|
|**DNS**|Optional; can still resolve public endpoint.|DNS must resolve to private IP to ensure private traffic.|Requires DNS setup when exposing service privately.|
|**Cost**|Free.|Data transfer costs may apply.|Included with private endpoint; charges may apply for cross-region traffic.|
|**Use Case**|Quick way to secure access from VNets without changing service.|Needed when you want complete private connectivity.|Required if exposing your own service to other VNets or tenants.|

**Summary:**

- **Service Endpoint** → VNet-aware public access.
- **Private Endpoint** → Private access using VNet IP.
- **Private Link** → The underlying tech that allows private endpoints to exist.



---

Back [[Azure Networking]]

---

