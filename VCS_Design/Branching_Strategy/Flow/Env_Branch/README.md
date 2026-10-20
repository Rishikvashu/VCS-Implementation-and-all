#  VCS Design | Branching Strategy | Environment Branch Flow 

<p align="center">
  <img width="90" height="auto" alt="git-branch-icon" src="https://img.icons8.com/fluency/96/git.png" />
</p>

---

# Author Table

| **Author**    | **Created On** | **Version** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ------------- | --------------  | -------------------- | ------------------- | ---------------- | ---------------- | ---------------- |
| Anshul Kotiya | 09-09-2026 | 1.0      | 10-09-2026     |     Shubham Rathi              |   Shreya / Nikita       |    Piyush Upadhyay               |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is an Environment Branch](#2-what-is-an-environment-branch)
3. [Why Environment Branches Are Used](#3-why-environment-branches-are-used)
4. [Types of Environment Branches](#4-types-of-environment-branches)
5. [How Environment Branch Flow Works](#5-how-environment-branch-flow-works)
   - [5.1 Workflow Diagram](#51-workflow-diagram)
6. [Advantages and Disadvantages](#6-advantages-and-disadvantages)
7. [Best Practices](#7-best-practices)
8. [Recommendation / Conclusion](#8-recommendation--conclusion)
9. [Contact Information](#9-contact-information)
10. [References](#10-references)

---

# 1. Introduction

This document explains the Environment Branch strategy, where branches like dev, staging, and prod map directly to deployment environments. It covers what this strategy is, why it's used, its workflow, and its pros, cons, and best practices.

---

# 2. What is an Environment Branch

An **Environment Branch** is a long-lived branch that represents a specific deployment environment rather than a single feature or release.

Instead of asking "which feature does this branch belong to," an environment branch answers "which environment does this branch deploy to." For example:

- `dev` branch → deploys to the Development environment
- `staging` branch → deploys to the Staging/QA environment
- `prod` branch → deploys to the Production environment

The code present on a given environment branch at any time reflects exactly what is running in that environment.

### In Simple Terms

One branch represents one environment. To know what's live in production, you only need to check the `prod` branch.

---

# 3. Why Environment Branches Are Used

Without a defined process for moving code between environments, teams often run into a few common issues:

- No reliable way to confirm what code is currently running in production
- Untested or unreviewed changes reaching production unintentionally
- Environment-specific settings (credentials, endpoints, feature flags) becoming difficult to manage
- Deployments being handled inconsistently across environments

Environment Branch Flow addresses these issues by assigning each environment its own dedicated branch, and by allowing code to move only in a controlled, forward direction — from `dev` to `staging` to `prod`.

### Key Benefits

| **Challenge**                     | **How Environment Branches Help**                                       |
| ------------------------------------ | -------------------------------------------------------------------------- |
| **Uncertainty about production**    | The `prod` branch always reflects exactly what's live.                   |
| **Untested code reaching users**    | Every change passes through `dev` and `staging` before reaching `prod`.  |
| **Managing environment settings**   | Each branch can maintain its own environment-specific configuration.     |
| **Inconsistent deployments**        | Deployments can be automated to trigger directly from the branch.        |

---

# 4. Types of Environment Branches

| **Branch**        | **Environment**           | **Purpose**                                                                  |
| -------------------- | ----------------------------- | ----------------------------------------------------------------------------------- |
| `dev`               | Development                    | Integration branch for newly developed features; used for early testing.           |
| `qa` / `staging`    | QA / Staging                    | Stabilized code promoted from `dev`; used for QA validation before production.      |
| `prod` / `main`     | Production                      | Contains only tested and approved code; this is what end users interact with.       |

---

# 5. How Environment Branch Flow Works

The typical flow of a change through the system is as follows:

1. A developer begins work on a feature, starting from the `dev` branch.
2. Once the feature is complete and reviewed, it is merged into `dev`; the Development environment updates automatically.
3. Once `dev` is stable, the changes are promoted into `staging`, updating the Staging environment.
4. QA validates the change on staging. Once approved, the same changes are promoted into `prod`.
5. Production is updated only from the `prod` branch — never directly from `dev` or a feature branch.
6. If an issue arises in production, a hotfix is created directly from `prod`, merged back into `prod`, and then back-merged into `staging` and `dev` to keep all branches in sync.

### 5.1 Workflow Diagram

<img width="1492" height="201" alt="Screenshot 2026-09-10 143047" src="https://github.com/user-attachments/assets/bc3a1b94-8ae2-41a2-b870-9479df347117" />



- `dev` deploys continuously to the Development environment as changes are merged in.
- `staging` deploys to the Staging/QA environment once changes are promoted from `dev`.
- `prod` deploys to Production only after staging validation; any hotfix applied to `prod` is subsequently merged back into `staging` and `dev`.

---

# 6. Advantages and Disadvantages

| **Advantages**                                                       | **Disadvantages**                                                          |
| --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Provides a clear, direct mapping between a branch and its environment.     | Long-lived branches can drift apart if not synced regularly.                      |
| Makes it easy to confirm what's currently in production.                   | Requires discipline to consistently back-merge hotfixes.                          |
| Allows environment-specific configuration to live alongside each branch.   | Adds process overhead compared to a single-branch, trunk-based model.             |
| Ensures every change is validated before reaching production.              | Does not scale well when multiple parallel release lines need to be supported.    |

---

# 7. Best Practices

| **Best Practice**                   | **Description**                                                                        |
| --------------------------------------- | ------------------------------------------------------------------------------------------ |
| **Promote code forward only**          | Move changes strictly `dev → staging → prod`; never deploy to `prod` from an untested source. |
| **Protect key branches**               | Apply branch protection to `prod` (and ideally `staging`) requiring review and passing checks. |
| **Keep branches synchronized**         | Avoid letting `dev` and `prod` diverge for extended periods.                              |
| **Externalize environment configuration** | Store environment-specific settings separately rather than hardcoding them.             |

---

# 8. Recommendation / Conclusion

Environment Branch Flow provides clear visibility and a controlled promotion process across environments. It ensures changes are tested before production while requiring proper branch synchronization and management.

---

# 9. Contact Information

| **Name**       | **Email**         |
| ---------------- | ------------------- |
| Anshul Kotiya   | anshul.kotiya.snaatak@mygurukulam.co    |

---

# 10. References

| **Reference**                                                                                     | **Description**                                                       |
| ------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------- |
| [Atlassian — Comparing Git branching workflows](https://www.atlassian.com/git/tutorials/comparing-workflows) | A comparison of common Git branching strategies.                    |
|[Git evn branch ](https://www.compilenrun.com/docs/devops/git/git-workflow-models/git-environment-branches)| git environment branch flow|

---
