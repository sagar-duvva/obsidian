

---

## **2. Step-by-Step Configuration Guides**

### **B. Azure Application Gateway (Layer 7)**

**Goal:** Route HTTP/HTTPS traffic with SSL termination and optional WAF.

**Steps:**

1. **Go to Azure Portal → Create Resource → Application Gateway**
2. **Basics:** Give name, region, SKU (Standard_v2 or WAF_v2)
3. **Frontend IP:** Assign Public or Private IP
4. **Backend Pool:** Add VM/VMSS, App Service, or IP addresses
5. **Health Probes:** Configure HTTP(S) probe to monitor backend health
6. **Routing Rules:**
    - **Path-based routing**: `/app1/* → backend1`, `/app2/* → backend2`
    - **Host-based routing**: `app1.domain.com → backend1`
7. **SSL Termination (Optional):** Upload certificate for HTTPS
8. **Enable WAF (Optional):** Protect against SQLi, XSS, etc.
9. **Review + Create** → Deploy
10. **Test:** Access application using the frontend IP/FQDN



---

Back [[Azure Networking]]

---

