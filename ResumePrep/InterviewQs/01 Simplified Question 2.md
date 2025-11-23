

---
---
---



# 📘 **1. STUDY GUIDE – Senior Azure DevOps Engineer**

This study guide is organized by core competency areas.

---

## **A. Azure Kubernetes Service (AKS)**

### **1. Kubernetes Architecture**

- Control plane (API server, scheduler, controller manager, etcd) is fully managed by Azure.
    
- Worker nodes run in your subscription with VM Scale Sets.
    
- Key components: kubelet, kube-proxy, CNI plugin (Azure CNI or Cilium).
    

### **2. Deployments vs StatefulSets**

- Deployments for stateless apps; rolling updates, scale easily.
    
- StatefulSets for apps needing stable identities (databases, queues).
    
- Uses PersistentVolumeClaims with Azure Disks or Azure Files.
    

### **3. HPA (Horizontal Pod Autoscaler)**

- Triggers: CPU, memory, custom metrics, KEDA event-driven triggers (Queues, HTTP, Kafka).
    
- Scales pods, not nodes.
    

### **4. AKS Security**

- Azure AD/Entra ID authentication
    
- RBAC (cluster + namespace roles)
    
- Network policies
    
- Workload Identity (replacing pod-managed identities)
    
- Private API server and egress restrictions
    
- Image scanning (ACR Tasks, Defender for Cloud)
    

### **5. AKS High Availability**

- Node pools in availability zones
    
- Multiple replicas
    
- Cluster autoscaler
    
- PDBs to maintain minimum availability
    
- Multi-region DR
    

### **6. AKS Cost Optimization**

- Spot node pools
    
- Right-size VM SKUs
    
- Autoscaling aggressively
    
- Disable unused add-ons
    
- Optimize container requests/limits
    

---

## **B. CI/CD (Azure Pipelines)**

### **7. Pipeline Structure**

- YAML stored in repo root
    
- Stages → Jobs → Steps
    
- Build → test → security → package → deploy
    

### **8. Scheduled Triggers**

- `schedules:` block in YAML
    
- Useful for nightly or weekly deployments
    

### **9. CD Deployment to AKS**

- Build Docker image → Push to ACR → Deploy via Kubernetes manifest or Helm
    
- Use deployment strategies (rolling, blue-green, canary)
    
- Promote artifacts between environments using pipeline approvals
    

### **10. Integration with Key Vault / SonarQube / Snyk**

- Key Vault task to fetch secrets securely
    
- Sonar tasks: Prepare → Analyze → Publish
    
- Snyk tasks for SAST + SCA
    
- Fail PR if gates fail
    

---

## **C. IaC (Terraform / Bicep / ARM)**

### **11. Terraform Basics**

- `init`, `plan`, `apply`
    
- State stored in Azure Storage backend
    
- Use `for_each` for stable resource addressing
    
- Avoid provisioners when possible
    

### **12. Bicep**

- Azure-native templating
    
- Modules for reusable components
    
- Fully declarative and idempotent
    

---

## **D. Containers & Docker**

### **13. Docker Basics**

- Images vs containers
    
- Bind mounts vs volumes
    
- Multi-stage builds
    
- Optimizing image size
    
- CMD vs ENTRYPOINT
    

### **14. Docker Networking**

- Bridge (default), host, overlay, macvlan
    
- Bridge provides isolated, NAT-based networking
    

---

## **E. Git & SCM**

### **15. Branching Strategies**

- GitFlow
    
- Trunk-based development
    
- PR-based validation
    
- Release branches for hotfixes
    

### **16. Rebase vs Merge**

- Rebase rewrites history
    
- Merge keeps commit lineage
    

### **17. Handling Merge Conflicts**

- Strategies: CLI, IDE, rebase, merge resolution
    
- Always resolve on feature branch, never on main
    

---

## **F. Observability (Prometheus, Grafana)**

### **18. Prometheus + Grafana**

- Prometheus collects metrics via kube-state-metrics and node exporters
    
- Grafana visualizes with dashboards
    
- Use alerts for CPU, memory, pod restarts, HPA triggers
    

---

## **G. Troubleshooting & SRE Principles**

### **19. Pods Pending**

- Not enough nodes
    
- PVC not bound
    
- Taints/tolerations mismatch
    
- Network policy denial
    

### **20. CrashLoopBackOff**

- Bad configuration, secrets, image pull, health probe failures
    

### **21. Long-Running Outages**

- Provide clear updates
    
- RCA
    
- Preventive automation
    
- Alerting improvements
    

---

# 🎴 **2. FLASHCARDS (Q & A)**

**Flashcard formatting:**  
**Q:** question  
**A:** answer

---

### Flashcard 1

**Q:** What manages the control plane in AKS?  
**A:** Azure manages the control plane; you only manage worker nodes.

### Flashcard 2

**Q:** When do you use StatefulSets?  
**A:** For workloads needing stable identity, persistent data, and ordered startup.

### Flashcard 3

**Q:** What triggers an AKS HPA?  
**A:** CPU, memory, custom metrics, or KEDA event-driven sources.

### Flashcard 4

**Q:** How do you secure an AKS cluster?  
**A:** Use Azure AD, RBAC, Workload Identity, network policies, private cluster.

### Flashcard 5

**Q:** What tool visualizes metrics from Prometheus?  
**A:** Grafana.

### Flashcard 6

**Q:** Difference between CMD and ENTRYPOINT?  
**A:** ENTRYPOINT is the main command; CMD provides default args.

### Flashcard 7

**Q:** Why store Terraform state in Azure Storage?  
**A:** For locking, consistency, and collaboration.

### Flashcard 8

**Q:** What is the default Docker network?  
**A:** Bridge network.

### Flashcard 9

**Q:** What is a Git rebase used for?  
**A:** Clean, linear history.

### Flashcard 10

**Q:** What’s inside an AKS deployment YAML?  
**A:** Replica count, selectors, containers, env vars, volumes, probes.

---

# 🎤 **3. MOCK INTERVIEW (Realistic Senior Azure DevOps)**

## I will play the interviewer.  
You answer as a candidate.

### **Question 1**

Explain the architecture of AKS and what parts are your responsibility as a DevOps engineer.

### **Question 2**

Your AKS pods are stuck in a pending state. Walk me through how you troubleshoot this.

### **Question 3**

How do you secure secrets across CI/CD, AKS, and Azure services?

### **Question 4**

Describe how your organization handles deployments from dev to prod in Azure DevOps.

### **Question 5**

Explain how you would design a cost-optimized AKS environment serving production traffic.

### **Question 6**

How do you integrate SonarQube and Snyk into an Azure Pipeline and enforce quality gates?

### **Question 7**

You updated a Helm chart, deployed it, and the application crashes immediately. Walk me through your approach.

### **Question 8**

Describe the pros and cons of using Terraform vs Bicep for Azure.

### **Question 9**

Give me a scenario where rebase is preferable to merge.

### **Question 10**

What preventive steps do you put in place after a major production outage?

---

If you want, I can **conduct the mock interview live**, asking questions one by one and evaluating answers.

---

# 🧠 **4. DEEP, LONG ANSWERS FOR ALL 52 QUESTIONS**

Below is the full detailed expansion—**long-form, senior-level answers**.

---

## **1. Kubernetes Architecture (Deep Answer)**

AKS provides a fully managed control plane containing the API server, controller manager, scheduler, and etcd. This is abstracted from the customer and maintained by Azure, including patching, scaling, and HA of the control plane. Workers are Kubernetes nodes running containerized workloads; these are deployed as VM Scale Sets in your Azure subscription.

Your responsibility:

- Node pool design
    
- Networking plugin selection (Azure CNI/Cilium)
    
- Ingress controllers
    
- RBAC
    
- Upgrades
    
- Add-ons (OIDC issuer, CSI drivers, KEDA, Dapr)
    
- Monitoring & scaling
    

---

## **2. Deployment vs StatefulSet**

Deployments ensure stateless pods with rolling updates and ephemeral instances. They rely on ReplicaSets and can scale horizontally without identity persistence.  
StatefulSets maintain stable pod identities and persistent storage. They are used for workloads like databases and messaging systems where you require stable network IDs, orderly startup/shutdown, and deterministic scaling.

---

## **3. Docker Networking**

Docker supports:

- **Bridge (default):** NAT-based, isolated network
    
- **Host:** Shares host network stack
    
- **Overlay:** Multi-host networking
    
- **Macvlan:** Assigns MAC addresses, direct L2 access
    

Bridge is default, suitable for local development.

---

## **4. Terraform Provisioners**

Provisioners run scripts but break idempotency. Best practice: avoid them unless no alternative exists (e.g., initial bootstrapping). They should be used sparingly to avoid unpredictable resource lifecycle behavior.

---

## **5. Terraform State File**

Terraform state stores resource mappings and attributes. In Azure DevOps, the backend uses Azure Storage with state locking via blob leases, preventing concurrent operations. Losing the state can cause drift and is recoverable only by importing resources.

---

## **6. Docker Image vs Container**

Images are read-only templates; containers are runtime instances. Immutable images enforce consistency across environments.

---

## **7. Docker Bind Mount vs Volume**

Bind mounts use host paths and are less portable.  
Volumes are managed by Docker, portable, and safer for containerized apps.

---

## **8. Installing Docker Inside a Container**

Possible but discouraged due to nested cgroups, security, and complexity. Use Docker socket mounting for CI workloads (DinD vs DooD).

---

## **9. Terraform List Change (Remove Index)**

Terraform index-based lists cause resource recreation. Use `for_each` with names as keys to avoid destructive operations when reordering lists.

---

## **10. Jenkins: Stage 5 Syntax Error**

The pipeline fails during compilation before any stage executes. Declarative pipelines validate before runtime.

---

## **11. Scripted vs Declarative**

Scripted pipelines allow custom Groovy scripting; declarative pipelines enforce structure, reduce errors, and support better governance.

---

## **12. Code Quality vs Coverage**

Quality focuses on maintainability, bugs, vulnerabilities.  
Coverage measures tested lines of code.  
Both are essential for secure, maintainable pipelines.

---

## **13. Default Sonar Quality Gate**

New code must meet thresholds:

- ≥ 80% coverage
    
- No blocker/critical issues
    
- Zero vulnerabilities
    
- Maintainability index check
    

---

## **14. Run YAML Manifest Without File**

Use standard input:  
`kubectl apply -f - <<EOF ...EOF`

---

## **15. Git Branching Strategy**

Trunk-based development with short-lived branches, PR validation, and deployment rings is modern standard. GitFlow is used for stable enterprise products.

---

## **16. PAT**

Personal Access Token is a scoped token used by Azure DevOps to authenticate with repos, pipelines, artifacts, and REST APIs.

---

## **17. Configure SonarQube**

Install extension → Prepare analysis configuration → Run analysis → Publish results.  
Use service connections with tokens.

---

## **18. Azure Key Vault in CI/CD**

Pipeline task retrieves secrets into variables.  
Optional: use Azure AD Workload Identity for accessing Key Vault from AKS workloads.

---

## **19. Merge Conflict Handling**

Three approaches:

- Resolve manually in IDE
    
- Rebase to reduce diff noise
    
- Use merge tool (VS Code, Azure DevOps)  
    Always pull latest main before starting work.
    

---

## **20. SonarQube Outputs**

Outputs include bugs, vulnerabilities, smells, duplications.  
Fix based on priority → update code → rerun pipeline → ensure gate passes.

---

## **21. Where Pipeline YAML Lives**

Stored in root directory of repo as `azure-pipelines.yml`.

---

## **22. Dockerfile Contents**

Includes base image, environment setup, build steps, exposed ports, ENTRYPOINT, and scripts.  
Use multi-stage builds to optimize size and security.

---

## **23. Schedule Pipeline**

Use YAML schedules or GUI triggers.  
Schedules do not run on PR branches unless configured.

---

## **24–26. Key Vault Secrets in AKS**

Use CSI secret store driver for mounted secrets.  
Secrets rotate automatically.  
Use Workload Identity instead of pod-managed identities.

---

## **27. Securing AKS**

Network policies, RBAC, AAD integration, private cluster, restricted egress, image scanning, and policy-as-code (OPA/Gatekeeper).

---

## **28. High Availability**

Multi-zone node pools, multiple replicas, automatic rollouts, and separate system/user node pools.

---

## **29. Cost Optimization**

Spot nodes, autoscaling, right-sized requests/limits, avoiding idle pods, scaling down dev clusters.

---

## **30. HPA Triggers**

CPU, memory, custom Prometheus metrics, KEDA event triggers like Azure Queue, Kafka, RabbitMQ.

---

## **31. Stateful vs Stateless**

Stateless apps use ephemeral compute and external storage. Stateful apps maintain session data and depend on persistent volumes.

---

## **32. Manage Data of Stateless Apps**

Use Azure SQL, CosmosDB, Blob storage, Redis Cache, or external persistent storage.

---

## **33. Entra ID Integration**

Use OIDC issuer + Azure AD integration with Kubernetes RBAC to authenticate and authorize users/groups.

---

## **34. Deploying Same Artifact to On-Prem & AKS**

Package artifact once → multi-environment promotion → two release pipelines with different deploy tasks.

---

## **35. Problem with Large Docker Images**

Slow build/pull times, security risk surface increases.  
Mitigate with multi-stage builds and distroless images.

---

## **36. CD Deployment Flow**

Build → push to ACR → update manifest/Helm → apply via pipeline → rollout monitored with liveness/readiness probes.

---

## **37. Pod Fails After Vault Secret Rotation**

Check CSI logs, reattach mount, restart pod if necessary.  
Use `kubectl describe pod` to verify volume errors.

---

## **38. App Down Troubleshooting**

Check: logs, events, probes, configmaps/secrets, DNS, networking, image, node status, quota, scaling.

---

## **39. Customer Update After 2-Week Outage**

Explain mitigation steps, root cause progress, ETA, and interim solutions; emphasize transparency.

---

## **40. Preventing Future Issues**

Add alerts, dashboards, auto-healing, chaos testing, improved CI/CD gates, runbooks, readiness/liveness probes.

---

## **41. Deployment YAML/Helm Contents**

Metadata, selectors, containers, ports, probes, resources, volumes, autoscaling, service, ingress.

---

## **42. Kubernetes Architecture Re-Explained**

Add detail about etcd, controller loops, node components, cloud-controller-manager interactions in AKS.

---

## **43. Troubleshoot Pending Pod**

Resource availability, PVC binding, taints, network restrictions, CNI issues.

---

## **44. Crash and Cannot Exec**

Analyze logs, events, health probes, init container failures, or missing secrets.

---

## **45. Blue-Green Deployment**

Two identical environments; switch traffic with ingress or service routing.

---

## **46. Grafana + Prometheus**

Prometheus scrapes Kubernetes; Grafana visualizes.  
Use Kube Prometheus Stack helm chart.

---

## **47. Pod Disruption Budget**

Limits voluntary disruptions; ensures availability during maintenance.

---

## **48. Rebase vs Merge**

Rebase creates cleaner history; merge preserves lineage.  
Rebase used for feature branches.

---

## **49. CMD vs ENTRYPOINT**

ENTRYPOINT defines app executable; CMD defines arguments. ENTRYPOINT is preferred for containerized apps.

---

## **50. Day-to-Day DevOps Activities**

CI/CD maintenance, AKS management, monitoring, IaC updates, troubleshooting, security enforcement.

---

## **51. Production Deployment Experience**

Fully automated pipelines, manual approvals, monitoring rollouts, feature flags.

---

## **52. Deployment Frequency**

Depends on org—modern teams aim for daily deployments with automated gates.

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


