

---
---
---


I’ve gone through your list carefully, keeping only **Azure- and Senior Azure DevOps–relevant topics**, removing duplicates, simplifying the questions, and adding missing senior-level Azure DevOps topics. Each question has a concise sample answer.

Here’s the cleaned and enhanced list in your requested format:

|Simplified Question|Category|Short Sample Answer|
|---|---|---|
|Explain a CI/CD pipeline in Azure DevOps.|CI/CD|A CI/CD pipeline automates code build, test, and deployment. In Azure DevOps, pipelines can use YAML or classic pipelines to build artifacts, run tests, and deploy to environments like AKS or App Service.|
|How do you pass variables and parameterize an Azure pipeline?|CI/CD|Use pipeline variables or variable groups; parameters in YAML allow dynamic values per run. Secure secrets via Azure Key Vault integration.|
|What deployment strategies do you use in Azure?|Deployment Strategy|Strategies include rolling, blue-green, canary, and A/B deployments. Choice depends on risk tolerance, downtime, and rollback needs.|
|How do you troubleshoot a failing pod in AKS?|AKS / Troubleshooting|Check `kubectl describe pod` and `kubectl logs`; inspect events, resource limits, network policies, and container images.|
|How do you assign memory to a pod and handle memory constraints?|AKS / Resource Management|Set `resources.requests` and `resources.limits`. Use Horizontal Pod Autoscaler for scaling; investigate OOMKilled events if limits are exceeded.|
|Explain namespaces in Kubernetes.|AKS / Governance|Namespaces isolate resources within a cluster, enabling multi-team deployments and resource quota management.|
|Difference between Deployment and StatefulSet in Kubernetes.|AKS / Workloads|Deployment manages stateless apps; StatefulSet manages stateful apps with persistent storage and stable network IDs.|
|How do you perform cluster scaling and auto-scaling in AKS?|AKS / Scaling|Use Cluster Autoscaler for node scaling and Horizontal Pod Autoscaler for pods based on CPU/memory metrics.|
|Explain Role-Based Access Control (RBAC) in AKS.|AKS / Security|RBAC defines user permissions via Roles and RoleBindings, controlling access to resources at namespace or cluster level.|
|How do you create and use custom resources in Kubernetes?|AKS / Custom Resources|Use CustomResourceDefinitions (CRDs) to extend Kubernetes API and manage custom objects with controllers/operators.|
|How do you configure ingress in AKS and troubleshoot a non-loading webpage?|AKS / Networking|Deploy an Ingress controller, create ingress rules for services, verify DNS and network connectivity, check TLS certificates, and inspect controller logs.|
|How do you monitor an AKS cluster using Prometheus?|AKS / Monitoring|Deploy Prometheus with kube-state-metrics, node exporters, and set up alert rules for CPU, memory, pod health, and custom metrics.|
|How do you upgrade AKS worker nodes safely?|AKS / Maintenance|Use `az aks nodepool upgrade` with drain enabled; ensure pods are evicted safely and workloads are resilient during upgrade.|
|Explain nodeselector, node affinity, taints, and tolerations in AKS.|AKS / Scheduling|NodeSelector assigns pods to specific nodes. Node affinity is more flexible. Taints prevent scheduling unless pod tolerates it.|
|How do you handle pods stuck during node drain?|AKS / Troubleshooting|Check pod disruption budgets, force delete pods if necessary, verify controllers are healthy to reschedule pods.|
|How do you define and use storage classes in Kubernetes?|AKS / Storage|StorageClass defines dynamic provisioning of persistent volumes with different performance and redundancy options, e.g., Azure Disk or Azure Files.|
|How do you manage Terraform state in Azure?|IaC / Terraform|Store state remotely in Azure Storage with a blob container and lock via `azurerm` backend to enable team collaboration and prevent conflicts.|
|How do you manage multiple environments with Terraform?|IaC / Terraform|Use workspaces, variable files, and modules to separate dev, test, and prod environments with reusable infrastructure code.|
|How do you handle Terraform provisioning errors?|IaC / Terraform|Validate using `terraform validate` and `plan`; check logs and debug errors; use `terraform import` for existing resources if needed.|
|How do you export Azure resources to Terraform code?|IaC / Terraform|Use `terraformer` or `az cli` with `az resource show` to generate resource definitions, then import into Terraform state.|
|How do you enforce Azure Policies via Terraform at scale?|IaC / Governance|Define `azurerm_policy_definition` and `azurerm_policy_assignment` resources; use modules for repeatable enforcement across subscriptions.|
|How do you write a multistage Dockerfile for Azure deployments?|Containers / CI/CD|Separate build and runtime stages to reduce image size; remove secrets and unnecessary layers; optimize caching for CI/CD pipelines.|
|How do you fix security issues in Docker images?|Containers / Security|Scan images with tools like Trivy or Azure Defender, update base images, remove unnecessary packages, and enforce security policies in pipelines.|
|How do you monitor Azure VM memory and alert if it exceeds 80%?|Monitoring / Azure|Use Azure Monitor with metric alerts, configure Action Groups for notifications, and optionally automate scaling or remediation via runbooks.|
|How do you structure repos and pipelines in a large Azure DevOps project?|CI/CD / Governance|Use mono-repo or multi-repo strategy, separate infrastructure and app code, modularize pipelines, parameterize stages, and standardize naming conventions.|
|How do you recover a force-pushed or deleted branch in Azure DevOps Git?|Git / Version Control|Use reflog or backup branches locally, recreate the branch, push it to remote, and notify teams.|
|Git Merge vs Rebase – when to use each?|Git / Version Control|Merge preserves history; Rebase creates linear history. Use merge for shared branches, rebase for feature branches before integration.|
|How do you use Azure DevOps REST API for repo security policies?|Security / Automation|Call REST endpoints to enforce branch policies, enable required reviews, build validations, or apply policies programmatically across repos.|
|How do you structure disaster recovery for Azure applications?|DR / Governance|Use geo-redundant services, backups, automated failover, replication across regions, and DR runbooks tested periodically.|
|How do you perform database migration in Azure?|Database / DevOps|Use Azure Database Migration Service, export/import pipelines, schema validation, and rollback strategies.|
|How do you implement DevSecOps in Azure pipelines?|Security / CI/CD|Integrate static code analysis, container scanning, secrets management, policy enforcement, and compliance checks into CI/CD pipelines.|
|What is observability architecture in Azure?|Monitoring / Observability|Combines metrics, logs, and traces to provide end-to-end insights into system health. Azure Monitor, Log Analytics, and Application Insights are used.|
|Difference between monitoring and observability?|Monitoring / Observability|Monitoring tracks predefined metrics/alerts; observability allows understanding unknown issues via logs, traces, and metrics.|
|How does observability help maintain site reliability?|Monitoring / Observability|Enables proactive detection, faster root-cause analysis, and informed scaling decisions, improving SLA compliance.|
|Explain SLA, SLO, and SLI from a business perspective.|Governance / Reliability|SLA is a commitment, SLO is a target within SLA, and SLI is the measurable metric. They define service reliability expectations.|
|How do you decide SLI in an application?|Governance / Reliability|Choose metrics meaningful to end-user experience, e.g., response time, error rate, or availability.|
|How do you troubleshoot incomplete logs across AKS, ingress, apps, and infra?|Monitoring / Troubleshooting|Correlate logs from Azure Monitor, Log Analytics, and App Insights; check log retention, network policies, and sidecar configurations.|
|Recommended tools for CI/CD, artifact storage, vulnerability scanning, and container registry in Azure.|CI/CD / Security|Azure DevOps Pipelines, Artifacts, Defender for Containers, Azure Container Registry, and GitHub Security scanning for hybrid setups.|
|How do you scale a Terraform pipeline that takes 25+ minutes?|IaC / Terraform|Split infrastructure into modules, use parallelism with `-parallelism` flag, and cache providers and state efficiently.|
|What happens to Terraform state if Azure resources are deleted outside Terraform?|IaC / Terraform|State becomes inconsistent; plan will detect drift. Use `terraform import` or `terraform refresh` to reconcile.|
|How do you handle pipeline failures due to existing resources?|CI/CD / Terraform|Use RIP strategy: Remove, Import, Plan; reconcile state file, or skip unchanged resources using `-target`.|
|How do you manage secrets in Azure DevOps pipelines?|Security / CI/CD|Use Azure Key Vault, variable groups marked as secret, or pipeline secrets with restricted access.|

---

✅ This version is **Azure-focused, senior-level, and covers CI/CD, AKS, IaC, DevSecOps, monitoring, scaling, troubleshooting, governance, and networking**.

If you want, I can **also create a condensed PDF-ready table** for quick interview prep that’s optimized for senior Azure DevOps engineers. It would include **~60–70 high-value questions** with sample answers.

Do you want me to do that?




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




