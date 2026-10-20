
#  Documentation: Branch Access Policies

<div align="center">
<img width="474" height="300" alt="logo" src="https://github.com/user-attachments/assets/58e4f6f7-b424-4d01-8d8e-5f8cde99cc26" />

</div>

---

| Author       | Created on | Version | Last updated by | Last edited on | Pre Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer  |
| ------------ | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ------------ |
| Mansoor Alam| 08/09/2026 | 1.0     | Mansoor Alam   | 08/09/2026     | Team         | Rajnish/Asma | Pritam/Komal | Abhishek/Manish Nautiyal |
| Mansoor Alam| 08/09/2026 | 1.1     | Mansoor Alam   | 22/09/2026     | Team         | Rajnish/Asma | Pritam/Komal | Abhishek/Manish Nautiyal |


---

## Table of Contents

- [Introduction](#introduction)
- [What is Branch Access Policy?](#what-is-branch-access-policy)
- [Why Branch Protection is Required?](#why-branch-protection-is-required)
- [Branch Access and Protection](#branch-access-and-protection)
  - [Branch Protection Rules](#branch-protection-rules)
  - [Repository Roles](#repository-roles)
  - [Recommended Access Model](#recommended-access-model)
- [Validation and Verification](#validation-and-verification)
  - [Verify Branch Ruleset](#1-verify-branch-ruleset)
  - [Verify Branch Protection Rules](#2-verify-branch-protection-rules)
  - [Verify Direct Push Restriction](#3-verify-direct-push-restriction)
  - [Verify Pull Request and Approval](#4-verify-pull-request-and-approval)
  - [Verify Approval and Merge](#5-verify-approval-and-merge)
  - [Verify Successful Merge](#6-verify-successful-merge)
- [Best Practices](#best-practices)
- [Conclusion](#conclusion)
- [Contacts](#contacts)
- [References](#references)

---

## Introduction
This document provides guidelines for managing branch access and protection in project repositories.
It explains how branch protection rules and repository access permissions can be used to control changes, require proper reviews, and prevent unauthorized or unvalidated changes from being merged.


---

## What is Branch Access policy?
A Branch Access Policy is a Set of rules that controls who can modify a branch and what conditions must be satisfied before changes can be merged.

---

## Why Branch Protection is Required?

Branch protection helps keep important branches secure and ensures that only approved and validated changes are merged.

Key benefits include:

- Preventing unauthorized or accidental changes.
- Requiring Pull Requests and code reviews.
- Ensuring required CI checks pass before merging.
- Preventing force pushes that can change the branch history.
- Protecting important branches from accidental deletion.

---

## Branch Access and Protection

Branch access defines who can modify a branch, while branch protection defines the rules that must be satisfied before changes can be merged.

Important branches such as `main` should be protected to ensure that only authorized and reviewed changes are merged.


<img width="1400" height="175" alt="workflow" src="https://github.com/user-attachments/assets/82f2ce6e-8971-4e09-a2a4-dc9a5e91e9e5" />



---

### Branch Protection Rules

The following protection rules can be configured for important or protected branches such as `main`:

| Rule | Purpose |
|------|---------|
| **Require Pull Request** | Ensures that changes are submitted through a Pull Request instead of direct changes. |
| **Require Approving Reviews** | Ensures that changes are reviewed and approved before merging. |
| **Require Status Checks** | Ensures that required CI or status checks pass before merging. |
| **Require Conversation Resolution** | Ensures that required review conversations are resolved before merging. |
| **Restrict Push Access** | Allows only authorized users or teams to push to the protected branch. |
| **Prevent Force Push** | Prevents users from rewriting the protected branch history using force push. |
| **Prevent Branch Deletion** | Prevents accidental deletion of the protected branch. |
| **Require Linear History** | Keeps the protected branch history linear by preventing merge commits. |

---

### Repository Roles

Repository roles define the level of access a user or team has within a repository.

The appropriate role should be assigned based on the responsibilities of the user or team.

| Role | Purpose |
|------|---------|
| **Read** | Allows users to view and discuss the repository. |
| **Triage** | Allows users to manage issues and Pull Requests without making code changes. |
| **Write** | Allows users to contribute code and work with Pull Requests. |
| **Maintain** | Allows users to manage the repository and its day-to-day activities without full administrative access. |
| **Admin** | Provides full administrative control over the repository. |

---

### Recommended Access Model

Repository access should be assigned based on the responsibilities of users and teams. Users should receive only the permissions required to perform their work.

| User / Team | Recommended Role | Purpose |
|-------------|------------------|---------|
| Developers | **Write** | Develop and contribute code through Pull Requests. |
| Reviewers | **Write** | Review and approve code changes. |
| Project Maintainers | **Maintain** | Manage repository settings and day-to-day activities. |
| Repository Administrators | **Admin** | Manage repository configuration, permissions, and protection rules. |
| Read-only Users | **Read** | View repository content without making code changes. |

---


## **Validation and Verification**

### **1. Verify Branch Ruleset**

Go to **Repository → Settings → Rules → Rulesets** and verify that the `main-branch-protection` ruleset is **Active** and applied to the default branch.

<img width="1366" height="768" alt="branch-ruleset" src="https://github.com/user-attachments/assets/52515de5-28a9-41d2-8929-3068d350bc1b" />


---

### **2. Verify Branch Protection Rules**

Open the `main-branch-protection` ruleset and verify that the required protection rules are enabled.

The configured rules include:

- Pull Request required before merging
- At least 1 approving review required
- Conversation resolution required before merging
- Status checks required
- Force pushes blocked


<img width="1354" height="605" alt="required approval before merging" src="https://github.com/user-attachments/assets/bfff6782-a734-434c-97e6-abba9d334fb9" />

---

### **3. Verify Direct Push Restriction**

Attempting to commit directly to the `main` branch is blocked because the branch is protected. Changes must be submitted through a Pull Request.

<img width="1355" height="613" alt="can&#39;t commit" src="https://github.com/user-attachments/assets/00ca7cd4-3150-44a3-893f-2ca555755b8c" />

---

### **4. Verify Pull Request and Approval**

Create a Pull Request from a separate branch to the protected `main` branch. GitHub requires at least one approving review before the Pull Request can be merged.

<img width="1349" height="621" alt="pull request created" src="https://github.com/user-attachments/assets/0b5e1b28-1453-4d94-941b-9c91e9e5bb27" />


---

### **5. Verify Approval and Merge**

After the required approval is provided, the Pull Request becomes eligible for merging.

<img width="1357" height="689" alt="PR approved submitted" src="https://github.com/user-attachments/assets/689de10b-b8c3-4e82-84ed-1ec9b8e8b2d5" />


---

### **6. Verify Successful Merge**

After approval, the Pull Request was successfully merged into the `main` branch.

<img width="1335" height="635" alt="PR merged" src="https://github.com/user-attachments/assets/2a7ac8de-7a37-403c-a387-bd4e3df86928" />


---

## Best Practices

- Protect important branches such as `main` or `production`.
- Require Pull Requests and code reviews before merging.
- Require CI/status checks to pass before merging.
- Follow the principle of least privilege when assigning repository access.
- Review branch protection rules regularly.

---

## Conclusion

Through this documentation, we explored branch access policies, repository roles, branch protection rules, and their practical implementation using GitHub Rulesets. We also verified the complete Pull Request, approval, and merge workflow for the protected `main` branch.

---

## Contacts

| **Name**         | **Email Address**                     |
|------------------|----------------------------------------|
| Mansoor Alam     | mansoor.alam.snaatak@mygurukulam.co              

## References

| Reference | Link |
|-----------|------|
| GitHub - Managing Protected Branches | https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches |
| GitHub - Managing Rulesets | https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets |
| GitHub - Repository Roles for an Organization | https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization |

---
