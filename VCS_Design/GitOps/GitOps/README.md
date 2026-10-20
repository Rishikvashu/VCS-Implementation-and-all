# VCS Design + POC | GitOps | Documentation



## Author Table

| Author | Created on | Version | Last updated by | Last edited on | PRE Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| ------ | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ----------- |
| Vineet | 12-09-2026 | v1.0 | Vineet | 12-09-2026 | Team | Aryan Mishra | Aditya Raj Singh  |  |

---

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [What is GitOps?](#what-is-gitops)
4. [Why GitOps?](#why-gitops)
5. [Types of GitOps Workflows](#types-of-gitops-workflows)
6. [Comparison: Push-based vs. Pull-based](#comparison-push-based-vs-pull-based)
7. [GitOps Tools Workflow](#gitops-tools-workflow)
8. [Comparison of GitOps Tools](#comparison-of-gitops-tools)
9. [Conclusion](#conclusion)
10. [Contact Information](#contact-information)
11. [References](#references)

---

# Introduction

As software delivery pipelines mature, managing infrastructure and application configurations manually becomes a bottleneck. **GitOps** is an operational framework that takes DevOps best practices used for application development (such as version control, collaboration, compliance, and CI/CD) and applies them to infrastructure automation. By implementing GitOps, teams can manage their infrastructure and application deployments using Git as the single source of truth.

---

# Prerequisites

Before deep-diving into GitOps, the reader should have a foundational understanding of the following concepts:
* **Version Control Systems (Git):** Proficiency in Git operations (commit, push, merge, branching strategies).
* **Infrastructure as Code (IaC):** Basic knowledge of writing infrastructure configurations declaratively (e.g., using Terraform, Ansible, or Kubernetes YAMLs).
* **CI/CD Pipelines:** Understanding of how Continuous Integration and Continuous Deployment workflows function.
* **Container Orchestration:** Familiarity with Kubernetes architecture, as modern Pull-based GitOps heavily relies on it.

---

# What is GitOps?

GitOps is a set of practices that uses Git repositories to manage infrastructure and application configurations in a declarative manner. 

The core principles of GitOps include:
* **Declarative System:** The entire system is described declaratively (e.g., using YAML or JSON).
* **Single Source of Truth:** The declarative descriptions are stored and versioned in Git.
* **Automatic Pulling:** Approved changes in Git are automatically applied to the system.
* **Continuous Reconciliation:** Software agents continuously verify that the live state of the system matches the desired state defined in Git. If there is a drift, it automatically corrects it or alerts the team.

---

# Why GitOps?

Implementing GitOps provides several critical benefits to the development and operations workflow:
* **Enhanced Security:** Developers do not need direct access (like `kubectl` or SSH) to production environments. They only need access to the Git repository.
* **Faster Deployments:** Automated synchronization reduces manual deployment steps, accelerating release cycles.
* **Easy Rollbacks:** Since every change is a Git commit, reverting to a previous stable state is as simple as executing a `git revert` command.
* **Auditability & Compliance:** Every change to the infrastructure is logged in Git history, showing exactly who made the change, when, and why.

---

# Types of GitOps Workflows

There are two primary strategies for implementing GitOps, depending on how the changes are applied to the target environment:

### 1. Push-Based Workflow (Agentless)
In a Push-based workflow, the CI/CD pipeline (e.g., GitLab CI, Jenkins, GitHub Actions) is responsible for pushing changes directly to the target environment.
* **How it works:** A developer merges code into the `main` branch. The CI pipeline triggers, builds the artifact, updates the configuration, and executes commands (like `kubectl apply` or `terraform apply`) against the destination server or cluster.
* **Primary Use Case:** Traditional infrastructure, serverless deployments, or teams relying entirely on existing CI pipelines.

### 2. Pull-Based Workflow (Agent-Based)
In a Pull-based workflow, a specialized software agent (operator) runs *inside* the target environment (e.g., inside the Kubernetes cluster). 
* **How it works:** The agent continuously monitors the Git repository. When it detects a change in the `main` branch, it pulls the new configuration and applies it to the cluster to reconcile the state. 
* **Primary Use Case:** Kubernetes-native environments using tools like **ArgoCD** or **FluxCD**.

---

# Comparison: Push-based vs. Pull-based

| Feature | Push-Based GitOps | Pull-Based GitOps |
| :--- | :--- | :--- |
| **Trigger Mechanism** | CI/CD Pipeline pushes to the target. | Internal Agent pulls from Git. |
| **Security** | CI tool needs high-level credentials to access the production cluster. | More secure. The cluster pulls data; no external inbound access is needed. |
| **State Monitoring** | Does not detect manual changes/drift in the cluster after deployment. | Continuously reconciles. Automatically detects and reverts manual drift. |
| **Complexity** | Easier to set up with existing CI/CD tools. | Requires installing and managing additional agents (e.g., ArgoCD). |
| **Best For** | Legacy systems, Terraform, traditional VMs. | Cloud-native, Kubernetes clusters. |

---

# GitOps Tools Workflow

The general workflow of GitOps tools can be represented as:


<img width="364" height="862" alt="image" src="https://github.com/user-attachments/assets/4287bc78-854f-41bc-b8c3-818b6c56c2b5" />




---

# Comparison of GitOps Tools

Different GitOps tools provide different capabilities for application deployment, continuous reconciliation, CI/CD integration, and Kubernetes management.

| Feature | Argo CD | Flux CD | Jenkins X | Rancher Fleet |
|---|---|---|---|---|
| **Primary Focus** | Kubernetes application delivery | Kubernetes GitOps and reconciliation | CI/CD + GitOps | Multi-cluster Kubernetes management |
| **GitOps Model** | Pull-based | Pull-based | GitOps-based | Pull-based |
| **Kubernetes Support** | Strong | Strong | Strong | Strong |
| **Continuous Reconciliation** | Yes | Yes | Supported | Yes |
| **Automated Deployment** | Yes | Yes | Yes | Yes |
| **Drift Detection** | Yes | Yes | Supported through GitOps workflow | Yes |

---

# Conclusion

GitOps uses Git as the single source of truth to manage application deployments and infrastructure through automated and declarative practices. Push-based and Pull-based workflows provide different approaches for applying changes to target environments. In this workflow, Argo CD continuously monitors the desired state in Git and reconciles it with the actual state of the Kubernetes environment, helping maintain consistent and automated deployments.

---

# Contact Information

| **Name** | **Email Address** |
| -------- | ----------------- |
| Vineet   | [vineet.shrivastava.snaatak@mygurukulam.co](mailto:vineet.shrivastava.snaatak@mygurukulam.co) |

---

# References

| **Topic** | **Reference Link** |
| --------- | ------------------ |
| Weaveworks (Creators of GitOps) | [Guide to GitOps](https://www.weave.works/technologies/gitops/) |
| ArgoCD | [ArgoCD Documentation](https://argo-cd.readthedocs.io/en/stable/) |
