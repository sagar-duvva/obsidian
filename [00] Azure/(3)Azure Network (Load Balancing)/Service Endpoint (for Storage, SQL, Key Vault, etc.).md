


## **2. Step-by-Step Configuration Guide (All Services)**

### **A. Service Endpoint (for Storage, SQL, Key Vault, etc.)**

1. Go to **Virtual Network → Subnets → select subnet → Service endpoints → Add**.
2. Select the **service type** (e.g., Storage, SQL).
3. Choose the **region**.
4. Click **Add** → **Save**.
5. Go to the **PaaS resource → Networking → Firewalls and virtual networks**.
6. Select **Selected networks** → Add the **subnet** with service endpoint.
7. Save the changes.

✅ Traffic stays on Azure backbone but still uses public IP.


---

Back [[Azure Networking]]

---



