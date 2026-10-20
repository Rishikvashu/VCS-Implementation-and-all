<div align="center">

# VCS Design + POC | Features of VCS | GitLab features

<img width="100%" height="200" style="object-fit: cover; border-radius: 6px;" alt="GitLab Banner" src="https://images.unsplash.com/photo-1618401471353-b98afee0b2eb?q=80&w=1200&auto=format&fit=crop" />

</div>

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Anurag Maurya | 25-08-2026 | v1 | Anurag Maurya | 11-09-2026 | Divya M/Vishal Tyagi | Aayush Verma | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Tool Overview](#2-tool-overview)
3. [Functional Capabilities](#3-functional-capabilities)
4. [Architecture & Deployment Model](#4-architecture--deployment-model)
5. [Security](#5-security)
6. [Tool Comparison](#6-tool-comparison)
7. [Advantages](#7-advantages)
8. [Limitations](#8-limitations)
9. [Cost Considerations](#9-cost-considerations)
10. [Assessment Summary](#10-assessment-summary)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

---

# 1. Introduction

<div align="justify">

GitLab is a Git-based DevOps platform. It brings source code management, Merge Requests, CI/CD pipelines, security scanning, container and package registries, and project governance together in one place.

This document reviews GitLab's features, architecture, security controls, and deployment options to decide whether it suits the team's version control (VCS) and DevOps workflow.

</div>

---

# 2. Tool Overview

| Attribute | Details |
|---|---|
| **Tool Name** | GitLab |
| **Vendor** | GitLab Inc. |
| **Category** | VCS / DevOps / CI/CD Platform |
| **Version** | Varies by deployment and release |
| **Primary Use Case** | Source control, collaboration, CI/CD, and DevOps workflow management |

---

# 3. Functional Capabilities

## 3.1 Core Features

<div align="justify">

GitLab organizes its version control and DevOps features under groups, subgroups, and projects.

</div>

| Feature | What it does | Availability |
|---|---|---|
| **Repository Management** | Manages repositories, branches, tags, and commits over SSH and HTTPS. Supports Git LFS for large binary files. | All tiers |
| **Merge Requests & Code Review** | Proposes changes, shows diffs, supports inline comments, and can require discussions to be resolved. | All tiers |
| **Protected Branches** | Limits who can push or merge into key branches such as main. | Basic in Free; user and group rules need a higher tier |
| **CODEOWNERS** | Sends Merge Requests to code owners based on file paths in .gitlab/CODEOWNERS. | Syntax in Free; mandatory approvals need a higher tier |
| **GitLab CI/CD** | Runs build, test, and deploy jobs defined in .gitlab-ci.yml. | All tiers (compute limits vary) |
| **GitLab Runner** | The agent that picks up and runs pipeline jobs using Docker, Kubernetes, or shell. | All tiers |
| **Container and Package Registries** | Stores container images and packages (npm, PyPI, Maven, Helm, NuGet, Conan) per project. | All tiers |
| **Security Scanning** | SAST and Secret Detection. DAST and Dependency Scanning need higher tiers. | Basic in Free; full suite depends on tier |
| **Permissions & RBAC** | Five standard roles: Guest, Reporter, Developer, Maintainer, and Owner. | Standard roles in Free; custom roles depend on tier |
| **Audit & Compliance** | Logs activity and permission changes. Advanced compliance features need higher tiers. | Basic in Free; advanced depends on tier |

---

## 3.2 Workflow Support

<div align="justify">

GitLab supports the Feature Branch workflow with a protected main branch.

</div>

<details>
<summary><b>Figure 1: GitLab Software Delivery Workflow</b></summary>

<br>

![GitLab Software Delivery Workflow](./assets/Gitlab_Workflow.png)

*Figure 1: GitLab software delivery workflow.*

</details>

<div align="justify">

1. **Create a feature branch** from main to do the work.
2. **Commit locally.** Commit signing (GPG or SSH) can be enforced.
3. **Push** the branch to GitLab over SSH or HTTPS.
4. **Open a Merge Request (MR)** to propose merging into main. This starts automated CI/CD checks.
5. **Review.** Reviewers read the diff and comment, and all discussions must be resolved before merging.
6. **Run CI/CD checks.** GitLab Runner executes linting, unit tests, and security scans from .gitlab-ci.yml.
7. **Pass the protected branch gate.** Main blocks direct pushes, and merging needs passing CI and Maintainer approval.
8. **Merge and deploy.** After approval, the MR merges into main, and the main pipeline packages and deploys the application.

</div>

---

## 3.3 Automation & Integration

<div align="justify">

GitLab has built-in pipeline automation and connects with external tools.

- **CI/CD pipelines:** workflows defined in .gitlab-ci.yml, run in stages or as dependency graphs.
- **Runners:** agents that poll GitLab for jobs and run them in Docker containers, Kubernetes pods, or shell environments.
- **Merge Request automation:** pipelines start on push, MRs can auto-merge when the pipeline passes, and discussion resolution can be required.
- **Registries:** built-in container and package registries with automatic authentication using CI_JOB_TOKEN.
- **Webhooks and APIs:** webhooks fire on events such as push, MR, and pipeline finish, and REST and GraphQL APIs support custom tooling.
- **Jira integration (needs configuration):** links MRs to Jira issue keys and updates issue status on merge. It requires webhook and API setup.
- **Security scanning (tier-dependent):** basic SAST and Secret Detection run in standard pipelines, while DAST, Container Scanning, and Dependency Scanning depend on tier.
- **Push Rules (tier-dependent):** server-side checks such as file size limits, commit message patterns, and secret blocking.

</div>

---

# 4. Architecture & Deployment Model

<div align="justify">

GitLab can be used as a cloud service or installed on your own infrastructure.

</div>

<details>
<summary><b>Figure 2: GitLab Architecture</b></summary>

<br>

![GitLab Architecture](./assets/Gitlab_Architecture.png)

*Figure 2: GitLab architecture.*

</details>

## 4.1 Cloud Deployment

<div align="justify">

**GitLab.com (SaaS)**

- **Model:** a multi-tenant service hosted and managed by GitLab Inc.
- **GitLab manages:** hardware, operating systems, databases, storage, backups, security patches, and high availability.
- **You manage:** projects, user access, code, pipeline definitions, and any self-hosted runners you add.
- **Scaling:** automatic. Shared runners come with monthly compute minute quotas based on tier.

</div>

---

## 4.2 On-Premises / Self-Hosted Deployment

<div align="justify">

**GitLab Self-Managed**

- **Model:** your own GitLab instance, installed with Linux packages (Omnibus) or Helm charts on Kubernetes.
- **You manage:** servers, storage, database clusters, networking, backups, and upgrades.
- **Control:** full control over data location, network policies, and LDAP or Active Directory integration.
- **Best for:** organizations with strict data control, compliance rules, or air-gapped networks.

</div>

---

# 5. Security

<div align="justify">

GitLab includes controls for repository access, code review, pipeline execution, and compliance logging.

</div>

| Control | Description | Availability |
|---|---|---|
| **Authentication and SSO** | Passwords, SSH and GPG keys, Personal Access Tokens, OAuth2, SAML SSO, and LDAP. | Basic in Free; SAML SSO depends on tier |
| **Role-Based Access Control** | Five standard roles: Guest, Reporter, Developer, Maintainer, and Owner. | Standard roles in Free; custom roles depend on tier |
| **Protected Branches** | Limits who can push or merge into key branches such as main. | Basic in Free; advanced rules depend on tier |
| **Signed Commits, Branch Protection, and CI** | Signed commits verify who wrote a commit. Protected branches control who can push or merge. CI checks confirm the code builds and passes tests before merge. | Signature tracking in all tiers; push rules depend on tier |
| **Merge Request Approvals** | Requires approvers to review code before it merges. | Standard in Free; mandatory CODEOWNERS approvals depend on tier |
| **Security Scanning** | Finds vulnerabilities (SAST) and hardcoded credentials (Secret Detection) in pipelines. | Basic in Free; full suite depends on tier |
| **Audit Events and Logging** | Records admin actions, access changes, and key project events. | Basic in Free; advanced depends on tier |
| **Transport and Storage Security** | Encrypts data in transit with HTTPS/TLS and SSH, and supports object storage encryption for artifacts and images. | All deployment options |

---

# 6. Tool Comparison

<div align="justify">

A neutral comparison of GitLab with other major platforms:

</div>

| Criteria | GitLab | GitHub | Bitbucket | Azure DevOps |
|---|---|---|---|---|
| **Category** | One unified DevOps and VCS platform | Repository hosting and DevOps platform | Git code management tool | Enterprise lifecycle management suite |
| **Deployment** | SaaS and Self-Managed | SaaS, Dedicated SaaS, Enterprise Server | Cloud and Data Center | Services and Server |
| **CI/CD** | Built-in GitLab CI/CD | Built-in GitHub Actions | Built-in Bitbucket Pipelines | Built-in Azure Pipelines |
| **Registries** | Built-in container and package registries | Built-in GitHub Packages | Needs an external registry | Built-in Azure Artifacts |
| **Code review** | Merge Requests with CODEOWNERS (approvals depend on tier) | Pull Requests with CODEOWNERS (approvals depend on tier) | Pull Requests with branch permissions | Pull Requests with branch policies |
| **Security scanning** | SAST, Secret Detection, DAST (advanced depends on tier) | Dependabot, CodeQL, Secret Scanning | Via Atlassian Marketplace | Microsoft Defender for DevOps |
| **Integrations** | Webhooks, APIs, native Jira support | Large Marketplace ecosystem | Deep Jira and Atlassian integration | Azure and Entra ID integration |
| **Main strength** | Single platform for source control, CI/CD, and registries | Large ecosystem and action marketplace | Direct Jira integration | Microsoft enterprise ecosystem |
| **Key consideration** | Advanced features depend on tier | Complex deployments rely on third-party actions | Smaller ecosystem outside Atlassian | Complex permission and project hierarchy |

---

# 7. Advantages

<div align="justify">

- Source control, Merge Requests, and CI/CD pipelines in one platform.
- Strong collaborative code review with thread resolution.
- Built-in CI/CD reduces the need for separate pipeline servers.
- Protected branches control changes to critical branches.
- Works as cloud SaaS or self-managed.
- Built-in container and package registries simplify artifact management.
- Integrates with Jira and custom webhooks.

</div>

---

# 8. Limitations

<div align="justify">

- Some advanced security and governance features depend on the subscription tier.
- Self-managed setups need ongoing infrastructure administration.
- It can be more than needed if the team only wants basic source control.
- Runners and pipelines need configuration and compute management.
- Features can differ between GitLab.com and self-managed installations.

</div>

---

# 9. Cost Considerations

<div align="justify">

- **Subscription:** per-user pricing across Free, Premium, and Ultimate plans, for both SaaS and Self-Managed.
- **Free vs paid:** Free covers repository hosting, standard Merge Request reviews, registries, and basic CI/CD. Premium and Ultimate add governance, code ownership rules, security scanners, and compliance tools.
- **Infrastructure:** self-managed setups need budget for servers, storage, backups, and database high availability.
- **Runners and compute:** self-managed runners mean hosting your own build servers, and SaaS shared runner usage beyond the free minutes must be purchased.
- **Storage:** large repositories, LFS files, container images, and artifacts need regular storage monitoring.
- **Maintenance:** self-managed instances need DevOps effort for updates and security patches.

</div>

> [!NOTE]
> Pricing and feature availability depend on the selected GitLab plan.

---

# 10. Assessment Summary

| Area | Assessment |
|---|---|
| **VCS capabilities** | Strong |
| **Code review** | Strong |
| **CI/CD** | Strong |
| **Security** | Strong, tier-dependent |
| **Integration** | Strong |
| **Deployment flexibility** | Strong |
| **Maintenance** | Low for SaaS, higher for self-managed |
| **Overall suitability** | Suitable for the team's VCS workflow |

### Recommendation

<div align="justify">

GitLab is recommended and suits the documented workflow: Feature Branch Workflow, a protected main branch, Merge Requests, and CI validation. Its single-platform model combines source control, code review, pipelines, and registries. The team should check whether the Free tier meets its needs, or whether Premium or Ultimate is required for compliance and governance.

</div>

---

# 11. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Anurag Maurya | [anurag.maurya.snaatak@mygurukulam.co](mailto:anurag.maurya.snaatak@mygurukulam.co) |

---

# 12. References

- [GitLab Official Documentation](https://docs.gitlab.com/)
- [GitLab Pricing and Plans Documentation](https://about.gitlab.com/pricing/)
- [GitLab Branch Rules & Protected Branches Documentation](https://docs.gitlab.com/ee/user/project/repository/branches/protected.html)
- [GitLab CODEOWNERS Syntax Documentation](https://docs.gitlab.com/ee/user/project/code_owners.html)
- [GitLab CI/CD Documentation](https://docs.gitlab.com/ee/ci/)
- [GitLab Runner Architecture Documentation](https://docs.gitlab.com/runner/architecture/)
- [GitLab Application Security Documentation](https://docs.gitlab.com/ee/user/application_security/)
- [GitLab Jira Integration Documentation](https://docs.gitlab.com/ee/user/project/integrations/jira/)
- [Git Documentation](https://git-scm.com/doc)
