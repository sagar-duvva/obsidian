

---
---
---

# ✅ **Final Azure / Senior Azure DevOps Interview Set**

## **1. Kubernetes Architecture**

**Category:** AKS / Kubernetes  
**Answer:** AKS uses a managed control plane (API server, scheduler, etcd) while nodes run in your subscription. You manage node pools, networking, identities, and add-ons; Microsoft manages the masters.

---

## **2. Deployment vs StatefulSet**

**Category:** AKS Workloads  
**Answer:** Deployments manage stateless pods with replica sets; StatefulSets maintain stable pod identity, ordering, and persistent storage for stateful apps.

---

## **3. Docker Networking & Defaults**

**Category:** Containers  
**Answer:** Docker supports bridge, host, overlay, macvlan networks; default is **bridge**, providing NAT-based connectivity within the host.

---

## **4. Terraform Provisioners**

**Category:** IaC / Terraform  
**Answer:** Provisioners run last-mile scripts on resources, but are discouraged; prefer cloud-native mechanisms or configuration management tools.

---

## **5. Terraform State File**

**Category:** IaC  
**Answer:** State tracks real resource mappings. In Azure, it’s typically stored in an Azure Storage backend with state locking using blob leases.

---

## **6. Docker Image vs Container**

**Category:** Containers  
**Answer:** Image = immutable template; Container = running instance of that image.

---

## **7. Docker Bind Mount vs Volume**

**Category:** Containers  
**Answer:** Bind mounts map host paths; volumes are Docker-managed storage, preferred for portability and security.

---

## **8. Docker-in-Docker?**

**Category:** Containers  
**Answer:** Possible but avoided due to security & stability concerns; use Docker-outside-of-Docker (mounting docker.sock) instead.

---

## **9. Terraform List Change (Removing an Element)**

**Category:** Terraform  
**Answer:** If resource creation is index-based, Terraform may destroy and recreate resources; use `for_each` with stable keys to avoid churn.

---

## **10. Jenkins Pipeline: Syntax Error in Last Stage**

**Category:** CI/CD  
**Answer:** The pipeline fails during validation before executing any stage.

---

## **11. Scripted vs Declarative Pipelines**

**Category:** CI/CD  
**Answer:** Declarative = structured, YAML-like syntax with guardrails; Scripted = fully Groovy, more flexible but harder to maintain.

---

## **12. Code Quality vs Code Coverage**

**Category:** DevSecOps / Quality  
**Answer:** Quality checks code smells, bugs, vulnerabilities; Coverage measures how much code is executed by tests.

---

## **13. Default SonarQube Quality Gate**

**Category:** DevSecOps  
**Answer:** Typically requires 80% coverage on new code, zero critical issues, zero new vulnerabilities, and no blocker bugs.

---

## **14. Run Kubernetes Manifest Without File**

**Category:** AKS  
**Answer:** Apply inline YAML using `kubectl apply -f - <<EOF … EOF`.

---

## **15. Git Branching Strategy**

**Category:** Git / SCM  
**Answer:** Common patterns: GitFlow, trunk-based, release branches. Azure Repos usually follows trunk-based with short-lived feature branches.

---

## **16. PAT (Personal Access Token)**

**Category:** Azure DevOps  
**Answer:** A scoped authentication token for Azure DevOps to access repos, pipelines, artifacts, or REST APIs.

---

## **17. Configure SonarQube in Azure Pipeline**

**Category:** DevSecOps  
**Answer:** Install Sonar extension → Add Prepare, Run Analysis, Publish tasks → Use service connection and token.

---

## **18. Integrate Azure Key Vault with Azure Pipelines**

**Category:** Security  
**Answer:** Create Key Vault → Add secrets → Add Key Vault task/service connection → Import secrets as pipeline variables.

---

## **19. Handling Merge Conflicts**

**Category:** Git / SCM  
**Answer:** Resolve via CLI, IDE tools, or Git rebase/merge strategies. Fix differences manually, commit, and push.

---

## **20. SonarQube Output & Fixing Issues**

**Category:** DevSecOps  
**Answer:** Outputs bugs, security hotspots, code smells. Fix by refactoring, updating libraries, or following remediation guidance.

---

## **21. Location for Pipeline YAML**

**Category:** CI/CD  
**Answer:** Stored in the root of the repo under `azure-pipelines.yml`.

---

## **22. Dockerfile Contents**

**Category:** Containers  
**Answer:** Base image, dependencies, copy source, build commands, expose ports, entrypoint/CMD.

---

## **23. Schedule a Pipeline**

**Category:** Azure Pipelines  
**Answer:** Add a `schedules:` block in YAML or configure scheduled triggers in Classic Pipeline UI.

---

## **24. Use Key Vault Secrets in AKS**

**Category:** AKS / Secrets  
**Answer:** Use Secrets Store CSI driver with Key Vault provider to mount secrets or sync them as Kubernetes secrets.

---

## **25. Auto-Refreshing Rotated Secrets in AKS**

**Category:** Security  
**Answer:** CSI driver supports auto-rotation; the pod reads updated secrets directly from the mounted volume.

---

## **26. Integrate SonarQube & Snyk in Azure Pipelines**

**Category:** DevSecOps  
**Answer:** Add Sonar tasks for static analysis and Snyk tasks for SCA/SAST; integrate with PR policies.

---

## **27. Securing AKS Cluster**

**Category:** Security  
**Answer:** Use RBAC, Azure AD integration, NSGs, network policies, pod identity/Workload Identity, API server RBAC, restricted egress, and private cluster.

---

## **28. AKS High Availability**

**Category:** AKS / Reliability  
**Answer:** Use multiple node pools, availability zones, replica sets, HPA, cluster autoscaler, and multi-region DR.

---

## **29. Cost Optimization in AKS**

**Category:** Cost Management  
**Answer:** Use spot nodes, autoscaler, right-size VM SKUs, node pool segregation, container limits, and avoid idle workloads.

---

## **30. HPA Triggers**

**Category:** AKS / Scaling  
**Answer:** CPU, memory, custom metrics, KEDA event-based triggers (queue length, HTTP rate, etc.).

---

## **31. Stateful vs Stateless Apps**

**Category:** Architecture  
**Answer:** Stateful requires persistent identity & data; Stateless handles ephemeral compute.

---

## **32. Managing Data for Stateless Apps**

**Category:** Architecture  
**Answer:** Offload data to Azure SQL, CosmosDB, Blob storage, Redis, etc.

---

## **33. Integrate Entra ID with AKS**

**Category:** Security / Identity  
**Answer:** Enable Azure AD integration; configure RBAC; map Entra groups to Kubernetes roles.

---

## **34. Issue Deploying Shared File to AKS & On-Prem**

**Category:** Multi-Environment Delivery  
**Answer:** Use separate deployment pipelines targeting each environment but referencing the same artifact.

---

## **35. Issue with Large Docker Images**

**Category:** Containers  
**Answer:** Slower builds, pulls, deployments; use multi-stage builds & slim base images.

---

## **36. CD to AKS (App Deployment Flow)**

**Category:** Release Engineering  
**Answer:** Build → Push image to ACR → Apply manifests/Helm via Azure Pipeline → Rollout and validate.

---

## **37. Pod Mounting Vault Secret, Pod Restarts**

**Category:** Troubleshooting  
**Answer:** For CSI driver, secrets refresh automatically; if pod fails, redeploy or inspect CSI logs.

---

## **38. App Down – Troubleshooting**

**Category:** Ops  
**Answer:** Check pod logs, events, deployments, node status, networking, DNS, config/secret validity, health probes.

---

## **39. Customer Update for Long-Running Outage**

**Category:** Communication  
**Answer:** Share RCA status, mitigation in place, ETA, and next steps; uphold transparency.

---

## **40. Steps to Prevent Recurrence**

**Category:** Governance  
**Answer:** Add alerts, runbooks, auto-healing, better resource limits, regression tests, and pipeline gates.

---

## **41. Deployment YAML / Helm Chart Contents**

**Category:** Kubernetes  
**Answer:** Metadata, selectors, replicas, containers, env, volumes, probes, autoscaling, and service definitions.

---

## **42. Kubernetes Architecture Explanation**

**Category:** AKS  
**Answer:** Control plane (API server, scheduler, controller manager, etcd) + worker nodes (kubelet, kube-proxy, pods).

---

## **43. Troubleshoot Pod Stuck in Pending**

**Category:** AKS Troubleshooting  
**Answer:** Check resource quotas, node capacity, PVC binding, taints/tolerations, and network policies.

---

## **44. If Pod Crashed & Cannot Exec Inside**

**Category:** Troubleshooting  
**Answer:** View logs, events, describe pod, check init containers, probes, image pull errors.

---

## **45. Blue-Green Deployment**

**Category:** Release Strategy  
**Answer:** Maintain two environments; switch traffic from Blue to Green after validation.

---

## **46. Prometheus + Grafana Integration**

**Category:** Monitoring  
**Answer:** Grafana reads Prometheus as a data source and visualizes AKS metrics.

---

## **47. Pod Disruption Budget (PDB)**

**Category:** Reliability  
**Answer:** Ensures minimum pod availability during voluntary disruptions (e.g., node upgrades).

---

## **48. Git Rebase vs Merge**

**Category:** Git  
**Answer:** Rebase rewrites history for clean logs; merge creates a merge commit preserving history.

---

## **49. CMD vs ENTRYPOINT in Docker**

**Category:** Containers  
**Answer:** ENTRYPOINT sets the main command; CMD provides default arguments.

---

## **50. Day-to-Day Activities of Azure DevOps Engineer**

**Category:** Role Understanding  
**Answer:** Managing CI/CD, AKS, Terraform IaC, monitoring, security, cost optimization, incident response.

---

## **51. Production Deployments & Frequency**

**Category:** Release Mgmt  
**Answer:** Usually daily/weekly depending on project; done through automated pipelines with approvals.

---

## **52. Azure DevOps CI/CD Workflow**

**Category:** Pipelines  
**Answer:** Code push → PR validation → Build → Test → Security scan → Publish artifacts → CD to AKS/App Service.

---

# ✅ If you want, I can also:

✔ Convert this into a **study guide**  
✔ Generate **flashcards**  
✔ Create a **mock interview**  
✔ Provide **deep-dive long answers** for each question


---
---
---



Perfect! I’ve gone through your list carefully, filtered out non-Azure/GCP/AWS-specific questions, removed duplicates, simplified each question, and added missing **senior-level Azure DevOps topics**. I also provided a concise sample answer for each. Here’s a structured, Azure-focused version:

|Simplified Question|Category|Short Sample Answer|
|---|---|---|
|What are your daily tasks as a Senior Azure DevOps engineer?|General|Managing CI/CD pipelines, IaC deployments with Terraform/Bicep/ARM, monitoring, troubleshooting, security, and optimizing Azure infrastructure.|
|What Azure DevOps services have you used?|Azure DevOps|Pipelines, Repos, Boards, Artifacts, Test Plans for CI/CD, version control, and project management.|
|How do you create and manage CI/CD pipelines in Azure DevOps?|CI/CD|Define YAML or classic pipelines to build, test, and deploy apps to Azure App Services, AKS, or VMs with approvals and artifact versioning.|
|How do you provision Azure resources using Terraform/Bicep/ARM?|IaC|Write declarative templates (ARM/Bicep) or Terraform scripts, use state management, and automate deployments via pipelines.|
|What is Azure Kubernetes Service (AKS) and how do you use it?|Kubernetes|Managed Kubernetes service for container orchestration; deploy microservices using Helm charts or YAML manifests with autoscaling and monitoring.|
|What are Pods, Deployments, DaemonSets, and StatefulSets in AKS?|Kubernetes|Pods: smallest deployable unit; Deployments: stateless apps; DaemonSets: one pod per node; StatefulSets: stateful apps with persistent storage.|
|How do you handle container image management in Azure?|Containers|Build Docker images, push to Azure Container Registry (ACR), use image tagging, scan for vulnerabilities, and deploy via AKS or App Service.|
|How do you configure secrets in Kubernetes and Azure Key Vault?|Security / DevSecOps|Store secrets in Key Vault; access via AKS using CSI driver; use Kubernetes secrets for internal pod access with RBAC.|
|How do you monitor Azure resources and AKS clusters?|Monitoring|Use Azure Monitor, Log Analytics, Application Insights, and Prometheus/Grafana for metrics and alerts.|
|How do you troubleshoot AKS pods and nodes?|Troubleshooting|Check pod status with `kubectl get pods`, describe events, review logs, node health, and metrics; use Azure Monitor and alerts.|
|How do you scale AKS workloads?|Scaling|Use Horizontal Pod Autoscaler (HPA), Cluster Autoscaler, or scale node pools manually based on CPU/memory metrics.|
|How do you secure Azure DevOps pipelines and resources?|Security / DevSecOps|Use RBAC, service principals, managed identities, secure variable groups, scan IaC templates, enforce branch policies, and secrets encryption.|
|How do you design high-availability applications in Azure?|Architecture / Governance|Use multiple availability zones, AKS node pools, Azure Load Balancer or Application Gateway, geo-redundant storage, and disaster recovery planning.|
|How do you configure networking for Azure workloads?|Networking|VNETs, subnets, NSGs, private endpoints, ExpressRoute, service endpoints, and internal/external load balancers for traffic management.|
|What is Azure Application Gateway and how does it work?|Networking|Layer 7 load balancer with SSL termination, path-based routing, WAF integration for secure HTTP/HTTPS traffic.|
|How do you handle CI/CD pipeline failures?|CI/CD / Troubleshooting|Rollback deployment using previous artifacts, check logs, validate YAML configurations, rerun failed stages, and apply hotfixes.|
|How do you manage Terraform state and lock files?|IaC|Store remote state in Azure Storage with state locking enabled; prevents concurrent updates and ensures consistency.|
|How do you handle alerts for CPU, disk, or app failures?|Monitoring|Configure Azure Monitor alerts, Log Analytics queries, and send notifications via email, Teams, or webhook integrations.|
|What is pod disruption budget (PDB) in Kubernetes?|Kubernetes|Defines the minimum number of pods that must remain available during voluntary disruptions for high availability.|
|How do you implement network security in AKS?|Security / Networking|Use NSGs, Azure Firewall, private clusters, and network policies to restrict traffic between pods and external endpoints.|
|How do you implement DevSecOps in Azure pipelines?|Security / DevSecOps|Integrate SAST, DAST, container scanning, policy-as-code, compliance checks, and secrets management into pipelines.|
|How do you deploy a web app to Azure App Service?|CI/CD / Azure Services|Build the app in pipeline, package artifacts, and deploy via Azure DevOps pipelines using ARM/Bicep templates or ZIP deployment.|
|How do you troubleshoot 503 errors in Azure App Service or AKS?|Troubleshooting|Check service health, logs, backend availability, scaling issues, ingress routing, and network security rules.|
|How do you handle rolling back a failed deployment?|CI/CD / Troubleshooting|Use pipeline rollback to previous artifact, rollback helm release, or redeploy stable image to AKS/App Service.|
|How do you integrate monitoring tools like Prometheus and Grafana?|Monitoring|Deploy Prometheus operator in AKS, configure metrics scraping, and create dashboards in Grafana for visualization.|
|How do you implement Infrastructure as Code governance?|Governance / IaC|Use policy-as-code (Azure Policy), RBAC, approvals in pipelines, and automated validation for ARM/Bicep/Terraform templates.|
|How do you handle AKS ingress routing issues?|Networking / Troubleshooting|Check Ingress controller logs, service definitions, path/host rules, and DNS or SSL configurations.|
|How do you manage Azure resources cost-effectively?|Governance|Tag resources, use reserved instances, optimize scaling, monitor cost in Azure Cost Management, and remove unused resources.|
|How do you connect to a VM with private IP?|Networking / Troubleshooting|Use Bastion host, VPN, or Azure Jumpbox with proper NSG and firewall rules.|
|How do you deploy multi-tier applications in Azure?|Architecture / CI/CD|Use AKS for backend, App Service for frontend, Azure SQL/Storage for data layer, managed networking, and CI/CD pipelines.|
|How do you handle secrets encryption in pipelines and AKS?|Security / DevSecOps|Use Azure Key Vault-backed secrets, pipeline secret variables, and Kubernetes secrets with encryption at rest.|
|How do you monitor application SLIs/SLOs/SLAs?|Monitoring / Governance|Define SLIs (latency/error rate), set SLO targets, and track SLA compliance with Azure Monitor and Application Insights.|
|How do you automate container image building and deployment?|CI/CD / Containers|Use Azure DevOps pipelines to build Docker images, push to ACR, and deploy to AKS or App Service with versioning and approvals.|
|How do you implement scaling and autoscaling for AKS?|Scaling|HPA for pod-level scaling, Cluster Autoscaler for node pools, and monitor metrics to adjust capacity dynamically.|

---

✅ I can also generate a **more comprehensive “Senior Azure DevOps Interview Sheet”** that adds **scenario-based questions and answers**, including AKS troubleshooting, Terraform Bicep/ARM advanced examples, CI/CD pipeline patterns, and DevSecOps best practices.

Do you want me to prepare that next?



---
---
---


Perfect! I’ll create a **comprehensive Senior Azure DevOps interview sheet** with **scenario-based questions, senior-level topics, and concise sample answers**, strictly Azure-focused. I’ll organize it by category for clarity.

---

### **1. Azure DevOps Services & CI/CD**

|Question|Category|Sample Answer|
|---|---|---|
|How do you structure CI/CD pipelines for multi-environment deployments?|CI/CD|Use YAML pipelines with stages for dev, test, prod; include approvals, environment variables, secrets from Key Vault, and artifact versioning.|
|How do you implement blue-green or canary deployments in Azure DevOps?|CI/CD|Use separate deployment slots in App Service or helm/AKS strategies; deploy new version to staging, run tests, then swap or gradually route traffic.|
|How do you rollback a failed deployment in Azure DevOps?|CI/CD|Use previously published artifact or Helm release; restore state in AKS or redeploy previous ARM/Bicep template version.|
|How do you manage pipeline secrets securely?|Security / DevSecOps|Store secrets in Azure Key Vault; link Key Vault to pipelines; enable variable groups and RBAC for controlled access.|
|How do you optimize long-running builds?|CI/CD|Use caching for dependencies, incremental builds, parallel jobs, self-hosted agents for heavy workloads.|

---

### **2. Azure Kubernetes Service (AKS) & Containers**

|Question|Category|Sample Answer|
|---|---|---|
|How do you deploy microservices on AKS?|AKS / CI/CD|Package services in Docker, push to ACR, create YAML/Helm manifests, deploy to AKS with services, ingress, and autoscaling.|
|How do you troubleshoot pod failures in AKS?|Troubleshooting|`kubectl describe pod`, `kubectl logs`; check node metrics, resource limits, events, and ingress/networking issues.|
|How do you implement scaling in AKS?|Scaling|Horizontal Pod Autoscaler for pods, Cluster Autoscaler for nodes, use metrics from Prometheus or Azure Monitor.|
|How do you manage secrets and config in AKS?|Security / AKS|Use Kubernetes secrets, ConfigMaps, and Azure Key Vault integration; encrypt secrets at rest; restrict access with RBAC.|
|What is a Pod Disruption Budget (PDB) and why is it important?|AKS / High Availability|PDB ensures a minimum number of pods remain available during voluntary disruptions (e.g., node upgrades) to maintain service availability.|
|How do you debug Ingress routing issues in AKS?|Networking / Troubleshooting|Check Ingress controller logs, service definitions, DNS, SSL, path-based routing, and network policies.|
|How do you handle stateful applications in AKS?|AKS / Storage|Use StatefulSets with Persistent Volumes (PV) and Persistent Volume Claims (PVC); configure storage classes for Azure Disk/Files.|
|How do you handle pod scheduling constraints?|AKS / Scaling|Use nodeSelector, taints/tolerations, affinity/anti-affinity rules to place pods on desired nodes.|

---

### **3. Infrastructure as Code (IaC)**

|Question|Category|Sample Answer|
|---|---|---|
|How do you manage Azure resources with Terraform/Bicep/ARM?|IaC|Write declarative templates, store state in Azure Storage, use pipelines for automated deployments, enforce version control.|
|What happens if Terraform state file is lost?|IaC / Troubleshooting|Restore from remote backend or snapshot; if unavailable, import existing resources using `terraform import`.|
|How do you handle Terraform lock files?|IaC|Lock prevents concurrent state updates; automatically handled with remote state backends like Azure Storage.|
|How do you implement governance in IaC?|Governance|Use Azure Policy, enforce naming conventions, tag resources, validate templates in pipelines, enforce approval gates.|
|How do you manage multi-region deployments with IaC?|IaC / Architecture|Parameterize templates, use modules, separate state per region, deploy via pipelines with environment variables.|

---

### **4. Security & DevSecOps**

|Question|Category|Sample Answer|
|---|---|---|
|How do you secure Azure DevOps pipelines?|Security / DevSecOps|Use RBAC, service connections with minimal permissions, secrets from Key Vault, branch policies, and code scanning.|
|How do you enforce DevSecOps in CI/CD?|Security / DevSecOps|Integrate SAST/DAST tools, container image scanning, IaC validation, secrets scanning, and compliance checks in pipelines.|
|How do you secure AKS networking?|Security / Networking|Use private clusters, NSGs, Azure Firewall, network policies, and restrict pod-to-pod traffic.|
|How do you implement SSL/TLS for Azure App Services or AKS?|Security|Use Azure Application Gateway, Key Vault-managed certificates, or ingress TLS in AKS; automate renewals with Key Vault.|
|How do you handle identity and access in Azure DevOps and AKS?|Security / Governance|Use Azure AD for authentication, service principals for automation, and RBAC for access control at resource/pipeline level.|

---

### **5. Monitoring, Logging, and Troubleshooting**

|Question|Category|Sample Answer|
|---|---|---|
|How do you monitor Azure applications and AKS clusters?|Monitoring|Use Azure Monitor, Log Analytics, Application Insights, Prometheus, Grafana dashboards, and configure alerts.|
|How do you handle CPU, memory, or disk alerts?|Monitoring / Troubleshooting|Configure metrics and alerts in Azure Monitor; trigger auto-scaling or notify DevOps teams via email/Teams.|
|How do you troubleshoot a 503 error in AKS or App Service?|Troubleshooting|Check backend pods/services, ingress, load balancer, resource limits, networking, and application logs.|
|How do you perform root cause analysis for infrastructure issues?|Troubleshooting|Review metrics, logs, alerts, recent changes, deployment history, and simulate the failure scenario.|
|How do you collect and visualize custom application metrics?|Monitoring|Use Prometheus exporters, Application Insights SDK, or Azure Monitor custom metrics; visualize in Grafana or Azure dashboards.|
|What are SLI, SLO, SLA and how do you monitor them?|Monitoring / Governance|SLI: metric (latency/error rate), SLO: target threshold, SLA: contractual uptime; monitor via Azure Monitor/Application Insights.|

---

### **6. Networking & Load Balancing**

|Question|Category|Sample Answer|
|---|---|---|
|How do you configure networking for Azure workloads?|Networking|Use VNETs, subnets, NSGs, service endpoints, private endpoints, ExpressRoute, and load balancers.|
|How do you route traffic in AKS?|Networking|Use Kubernetes Services (ClusterIP/NodePort/LoadBalancer) and Ingress controllers; integrate with Azure Application Gateway if needed.|
|What is Azure Application Gateway and why use it?|Networking|Layer 7 load balancer with SSL termination, path-based routing, WAF support for secure HTTP traffic.|
|How do you troubleshoot connectivity to a VM with private IP?|Networking / Troubleshooting|Use Bastion, VPN, or jumpbox; check NSGs, firewalls, routing tables, and Azure Private DNS.|
|How do you implement high-availability load balancing?|Networking / Architecture|Use Azure Load Balancer or Application Gateway across availability zones with health probes and autoscaling backend pools.|

---

### **7. Advanced AKS / Azure Scenarios**

|Question|Category|Sample Answer|
|---|---|---|
|How do you implement multi-tier applications on AKS?|Architecture|Frontend: App Service or Ingress; Backend: AKS microservices; Database: Azure SQL or Cosmos DB; CI/CD for automated deployments.|
|How do you handle sudden traffic surges in AKS or App Service?|Scaling / Troubleshooting|Use autoscaling, adjust replicas, scale node pools, implement caching/CDN, monitor metrics, and load balancing strategies.|
|How do you implement GitOps in Azure DevOps?|CI/CD / Governance|Use pipelines or Flux/ArgoCD to deploy declarative manifests from Git repos; maintain drift detection and audit trails.|
|How do you implement persistent storage for AKS?|AKS / Storage|Use Azure Disks (block storage) or Azure Files (shared storage) with PV/PVC; configure storage classes and access modes.|
|How do you perform disaster recovery for AKS workloads?|Architecture / Governance|Use multi-region clusters, backup/restore via Velero, replicate stateful data, and automate failover using Azure Traffic Manager.|
|How do you troubleshoot scaling failures in AKS?|Scaling / Troubleshooting|Check HPA metrics, resource quotas, Cluster Autoscaler logs, node pool limits, and pending pods.|

---

If you want, I can **also create a 2-page “Senior Azure DevOps Interview Cheat Sheet PDF”** with:

- **~100 concise questions** covering all Azure DevOps, AKS, IaC, Security, CI/CD, Monitoring, Networking, and Scaling topics.
    
- **Sample answers ready for interviews.**
    

This would be a ready-to-use senior-level guide.

Do you want me to make that PDF next?


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
---




---
---
---




