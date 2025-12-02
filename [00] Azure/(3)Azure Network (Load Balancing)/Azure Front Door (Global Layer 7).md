

---

## **2. Step-by-Step Configuration Guides**

### **C. Azure Front Door (Global Layer 7)**

**Goal:** Distribute HTTP/HTTPS traffic globally with caching, acceleration, and failover.

**Steps:**

1. **Go to Azure Portal → Create Resource → Front Door**
    
2. **Basics:** Name, Subscription, Resource Group
    
3. **Frontend/domains:** Add custom domain or use default Front Door domain
    
4. **Backend Pools:**
    
    - Add regions (App Services, VMs, endpoints)
        
    - Configure **load balancing and priority** for failover
        
5. **Health Probes:** HTTP(S) probe to monitor backend health
    
6. **Routing Rules:** Define path-based or host-based routing to backend pool
    
7. **Caching & Compression (Optional):** Enable static content caching or compression
    
8. **SSL Termination:** Enable HTTPS with your certificate
    
9. **Enable WAF (Optional):** Protect against web attacks
    
10. **Review + Create** → Deploy
    
11. **Test:** Access via Front Door domain → traffic routed to nearest healthy backend
    


Back [[(3)Azure Network (Load Balancing)]]