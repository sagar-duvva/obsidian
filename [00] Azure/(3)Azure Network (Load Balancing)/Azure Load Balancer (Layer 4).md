## **2. Step-by-Step Configuration Guides**

### **A. Azure Load Balancer (Layer 4)**

**Goal:** Distribute TCP/UDP traffic to multiple VMs.

**Steps:**

1. **Go to Azure Portal → Create Resource → Load Balancer**
    
2. **Choose SKU**: Standard (recommended for production) or Basic
    
3. **Type:** Public (internet-facing) or Internal (private)
    
4. **Frontend IP Configuration:** Assign a public IP or private IP
    
5. **Backend Pool:** Add VMs or VM Scale Sets
    
6. **Health Probe:** Configure protocol (TCP/HTTP) and port to check VM health
    
7. **Load Balancing Rule:** Define traffic distribution (port mapping, backend pool)
    
8. **Review + Create** → Wait for deployment
    
9. **Test:** Access the frontend IP and verify traffic reaches all VMs
    

Back [[(3)Azure Network (Load Balancing)]]