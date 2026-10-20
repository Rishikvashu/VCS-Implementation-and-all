# VCS Design: Monorepo vs Microrepo

<p align="center">
  <img src="https://cdn-icons-png.flaticon.com/512/4494/4494740.png" alt="Version Control System Architecture" width="80"/>
</p>

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Sarthak Bhatnagar | 10-09-2026 | v1.1 | Sarthak Bhatnagar | 10-09-2026 | Ishaan | Anuj Jain | Piyush Upadhyay |

---

# Table of Contents

 - [Introduction](#1-introduction)
 - [Purpose](#2-purpose)
 - [Monorepo vs Microrepo](#3-monorepo-vs-microrepo)
 - [Key Features](#4-key-features)
 - [Monorepo vs Microrepo Comparison](#5-monorepo-vs-microrepo-comparison)
 - [Best Practices](#6-best-practices)
 - [Conclusion](#7-conclusion)
 - [Contact Information](#8-contact-information)
 - [References](#9-references)

---

# 1. Introduction

This document provides an overview of **Monorepo** and **Microrepo** approaches used in **Version Control Systems (VCS)**. It explains their purpose, key features, differences, and best practices.

---

# 2. Purpose

| **Objective** | **Description**|
| :--- | :--- |
| **Architectural Clarity** | Understand the structural differences between Monorepo and Microrepo. |
| **Decision Support** | Help teams select the appropriate repository model based on project requirements and scale. |
| **Team Collaboration** | Understand how Monorepo and Microrepo approaches support collaboration and team ownership. |

---

# 3. Monorepo vs Microrepo

### 3.1. Monorepo

Multiple projects and shared libraries stored in **one single Git repository**.

* **Code Sharing:** Direct relative imports without publishing packages.
* **Commit Flow:** Single atomic PR can update APIs and consumers together.

```
my-monorepo/
├── services/ (attendance-api, employee-api, salary-api)
├── shared/   (common-logger, auth-lib)
└── package.json
```

---

### 3.2. Microrepo (Polyrepo)

Each microservice or library stored in its **own isolated Git repository**.

* **Code Sharing:** Libraries are published as versioned packages to artifact registries.
* **Commit Flow:** Teams work independently; cross-service updates require multiple PRs.

```
Organization
├── attendance-api.git
├── employee-api.git
├── salary-api.git
└── shared-logger.git (published package)
```

---

# 4. Key Features

### 4.1 Monorepo

| **Key Feature** | **Description** |
| :--- | :--- |
| **Single Repository** | Multiple projects or services are managed in one repository. |
| **Shared Code** | Common libraries and components can be easily shared. |
| **Centralized Management** | Code, branches, and changes are managed from one repository. |
| **Unified Versioning** | All projects maintain a common version history. |
| **Cross-Service Changes** | Changes across multiple services can be handled in a single PR. |

### 4.2 Microrepo

| **Key Feature** | **Description** |
| :--- | :--- |
| **Separate Repository** | Each service or project has its own repository. |
| **Independent Development** | Teams can develop and manage services independently. |
| **Independent CI/CD** | Each repository can have its own CI/CD pipeline. |
| **Separate Access Control** | Repository permissions can be managed independently. |
| **Independent Deployment** | Services can be built and deployed separately. |

---

# 5. Monorepo vs Microrepo Comparison



| **Metric**                | **Monorepo**                                       | **Microrepo (Polyrepo)**                           |
| ------------------------- | -------------------------------------------------- | -------------------------------------------------- |
| **Repository Structure**  | One central repository for multiple services       | Separate repository for each service               |
| **Code Sharing**          | Easy to share code directly                        | Code is shared through packages                    |
| **Atomic Commits**        | Multiple service changes can be committed together | Changes usually require separate commits           |
| **CI/CD**                 | Requires path-based builds and caching             | Each repository has its own simple pipeline        |
| **Team Autonomy**         | Teams work in a shared repository                  | Teams have more independence                       |
| **Access Control**        | Managed using folders and `CODEOWNERS`             | Managed separately for each repository             |
| **Dependency Management** | Dependencies can be managed centrally              | Each service manages its own dependencies          |
| **Versioning**            | Services can share common versions                 | Each service can have its own version              |
| **Scalability**           | Repository can become large as projects grow       | New services can be added as separate repositories |
| **Release Management**    | Suitable for coordinated releases                  | Suitable for independent releases                  |

---

# 6. Best Practices

| **Best Practice**                | **Description**                                                                  |
| :------------------------------- | :------------------------------------------------------------------------------- |
| **Keep Repositories Clean**      | Remove unused files, old configurations, and unnecessary code regularly.         |
| **Maintain Documentation**       | Keep README files and important project information updated.                     |
| **Use Code Reviews**             | Review changes before merging them into the main codebase.                       |
| **Keep Commits Meaningful**      | Use clear and meaningful commit messages for better tracking.                    |
| **Avoid Duplicate Code**         | Reuse common components instead of maintaining the same code in multiple places. |               |
| **Use Automated Testing**        | Run automated tests to identify issues before changes are merged or released.    |



---

# 7. Conclusion

Monorepo and Microrepo are two different approaches to organizing source code in a Version Control System.
**Monorepo** keeps multiple services or projects in a single repository.
**Microrepo** keeps each service or project in a separate repository.

For the OT Microservices (OTMS) project, Microrepo is more suitable because each service can be managed independently. Different teams can work on separate services without disturbing other services. It reduces code management conflicts, allows independent development and deployment, and makes access control easier for each service.

---

# 8. Contact Information

| **Name** | **Email** |
| :--- | :--- |
| Sarthak Bhatnagar | [sarthak.bhatnagar.snaatak@mygurukulam.co](mailto:sarthak.bhatnagar.snaatak@mygurukulam.co) |

---

# 9. References
| **Topic**                      | **Link**                                                                   |
| ------------------------------ | -------------------------------------------------------------------------- |
| **Git Official Documentation** | [git-scm.com/doc](https://git-scm.com/doc)                                 |
| **Monorepo Tools Guide**       | [monorepo.tools](https://monorepo.tools)                                   |
| **GitHub Repositories Guide**  | [docs.github.com/en/repositories](https://docs.github.com/en/repositories) |
