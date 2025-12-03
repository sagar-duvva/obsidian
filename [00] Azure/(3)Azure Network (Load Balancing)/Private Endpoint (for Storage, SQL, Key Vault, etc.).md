

## **2. Step-by-Step Configuration Guide (All Services)**

### **B. Private Endpoint (for Storage, SQL, Key Vault, etc.)**

1. Go to the **PaaS resource → Networking → Private endpoint connections → + Private endpoint**.
2. Fill in **Subscription, Resource group, Name, Region**.
3. Select **VNet and subnet** for private IP allocation.
4. Choose **Target sub-resource** (e.g., blob, sqlServer, vault).
5. Optionally integrate with **Private DNS zone** for automatic DNS mapping.
6. Click **Create**.
7. Verify that the **private IP** is allocated and DNS resolves to it.

✅ All traffic to the service goes privately through VNet.

---

Back [[Azure Networking]]

---
