# Version Control System (VCS) Setup and Features DOC

---

## Author Table

| Author | Created On | Version | Last Updated By | Last Edited On | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Palak Upadhyay | 08-09-2026 | 1.0 | Palak Upadhyay | 18-09-2026 | Aryan Mishra | - | - |
| Palak Upadhyay | 08-09-2026 | 1.1 | Palak Upadhyay | 20-09-2026 | Aryan Mishra | Aditya raj singh | - |
| Palak Upadhyay | 08-09-2026 | 1.2 | Palak Upadhyay | 06-10-2026 | Aryan Mishra | Aditya raj singh | - |

---

## Table of Contents

* [1. Introduction](#1-introduction)
* [2. What Is VCS?](#2-what-is-vcs)
* [3. VCS Setup & Implementation](#3-vcs-setup--implementation)
  * [Step 1: Configure Git Identity](#step-1-configure-git-identity)
  * [Step 2: Initialize Local Repository](#step-2-initialize-local-repository)
  * [Step 3: Track and Commit Changes](#step-3-track-and-commit-changes)
  * [Step 4: Branch Management](#step-4-branch-management)
  * [Step 5: Connect and Push to Remote (GitHub)](#step-5-connect-and-push-to-remote-github)
* [4. Important VCS Features](#4-important-vcs-features)
* [5. VCS Workflow Architecture](#5-vcs-workflow-architecture)
* [6. Best Practices](#6-best-practices)
* [7. Conclusion](#7-conclusion)
* [8. References & Documentation](#8-references--documentation)
* [9. Document Ownership & Contact Info](#9-document-ownership--contact-info)

---

## 1. Introduction

The purpose of this document is to establish a standardized, repeatable procedure for setting up a Version Control System (VCS), detailing its core architectural features, and outlining the end-to-end workflow used to track and manage code changes across microservices.

---

## 2. What Is VCS?

A **Version Control System (VCS)** is an engineering software category that records and manages changes to source code and configuration files over time. It allows development teams to recall specific versions, compare changes, audit history, and work concurrently without overwriting code.

* **Primary Tool:** Git (Distributed Version Control System)
* **Remote Hosting Platform:** GitHub / GitLab

---

## 3. VCS Setup & Implementation

Click on each step below to expand the commands, technical descriptions, and engineering rationale.

<details>
<summary><strong>Step 1: Configure Git Identity</strong></summary>

<br>

### Commands:
```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

### Explanation:
* `user.name`: Sets the author's full name to be recorded with every commit.
* `user.email`: Associates commits with the user's GitHub account for verified contributor attribution.
* `--global`: Applies these configuration settings globally across all repositories on the local system (persisted in `~/.gitconfig`).

### Why:
* Establishes a verifiable audit trail so team members can trace who authored and committed specific changes.

</details>

<details>
<summary><strong>Step 2: Initialize Local Repository</strong></summary>

<br>

### Commands:
```bash
mkdir vcs-poc
cd vcs-poc
git init
```

### Explanation:
* `mkdir vcs-poc`: Creates a dedicated project directory.
* `cd vcs-poc`: Navigates into the newly created folder.
* `git init`: Initializes an internal hidden `.git` directory containing object storage, staging index, and configuration files.

### Why:
* Transforms an ordinary local folder into a Git-managed repository capable of tracking version history.

</details>

<details>
<summary><strong>Step 3: Track and Commit Changes</strong></summary>

<br>

### Commands:
```bash
# Create baseline file
echo "# VCS POC Project" > README.md

# Stage files
git add .

# Create snapshot
git commit -m "feat: initial project structure with README"

# Inspect status
git status
```

### Explanation:
* `git add .`: Moves modified and newly created files from the working directory into the **Staging Area (Index)**.
* `git commit -m "<message>"`: Captures a permanent, cryptographic snapshot (SHA-1 hash) of staged changes in the local repository history.
* `git status`: Displays the status of untracked, modified, and staged files relative to the working tree.

### Why:
* The multi-stage commit process allows developers to craft atomic, logical commits and prevents unfinished or accidental files from entering version history.

</details>

<details>
<summary><strong>Step 4: Branch Management</strong></summary>

<br>

### Commands:
```bash
# Create and switch to a feature branch
git switch -c feature/vcs-setup

# Verify current branches
git branch
```

### Explanation:
* `git switch -c <branch-name>`: Simultaneously creates a new feature branch and points `HEAD` to it.
* `git branch`: Lists all local branches, with an asterisk (`*`) designating the currently active branch.

### Why:
* Keeps the production line (`main`) pristine and gives developers an isolated environment to build and test features without impacting mainline stability.

</details>

<details>
<summary><strong>Step 5: Connect and Push to Remote (GitHub)</strong></summary>

<br>

### Commands:
```bash
# Enforce modern branch naming standard
git branch -M main

# Link remote repository
git remote add origin https://github.com/Team-KubeClan/vcs-poc.git

# Push changes and set upstream tracking
git push -u origin main
```

### Explanation:
* `git branch -M main`: Renames the default local branch to `main`, aligning with modern industry standards.
* `git remote add origin <URL>`: Associates the local repository with the remote GitHub repository URL under the alias `origin`.
* `git push -u origin main`: Uploads local commits to the remote server and sets up upstream branch tracking (allowing future shorthand `git push` commands).

### Why:
* Centralizes code on a secure remote platform, protects against local data loss, and enables team collaboration and automated CI/CD workflows.

</details>

---

## 4. Important VCS Features

| Feature | Technical Purpose | Project Impact |
| :--- | :--- | :--- |
| **Version Tracking** | Records commit timestamp, author identity, and SHA hashes for every change. | Full accountability and verifiable audit logs. |
| **Branching & Merging** | Enables independent, parallel development streams. | Rapid, isolated feature delivery without mainline breakage. |
| **Change Comparison (`diff`)** | Inspects line-by-line differences between files, commits, and branches. | High-quality pre-commit validation and code reviews. |
| **Rollback & Recovery** | Restores previous working states via `git revert` or `git reset`. | Rapid disaster recovery during deployment failures. |
| **Access Control & Governance** | Restricts branch pushes via automated review and CI status checks. | Guardrails that protect production stability. |

---

## 5. VCS Workflow Architecture


  <img width="400" height="800" alt="vcs_workflow_diagram" src="https://github.com/user-attachments/assets/686e8e07-7f28-40a5-97fe-08a6de261b16" />

---

## 6. Best Practices

* Write clear and meaningful commit messages following standard conventions (`feat:`, `fix:`, `docs:`).
* Always create separate feature branches for new tasks and avoid committing directly to `main` or `develop`.
* Review your changes carefully using `git status` and `git diff` before staging and committing.
* Never commit sensitive data, passwords, API keys, or `.env` files into the repository.
* Keep your local branches regularly synchronized with the remote repository to avoid merge conflicts.
* Configure branch protection rules to ensure code is reviewed and CI tests pass before merging pull requests.

---

## 7. Conclusion

A Version Control System is the foundational pillar of modern DevOps and microservices engineering. By adopting Git with standardized branch naming, automated branch protection rules, and structured commit practices, teams ensure continuous code integrity, seamless collaboration, and rapid rollback capabilities.

---

## 8. Contact Information

| Name | Email Address |
| :--- | :--- |
| Palak Upadhyay | [palak.upadhyay.snaatak@mygurukulam.co](mailto:palak.upadhyay.snaatak@mygurukulam.co) |

---

## 9. References

| Description | Reference Link |
| :--- | :--- |
| Official Git documentation and core concepts | [Official Git Documentation](https://git-scm.com/doc) |
| Comprehensive Git architecture and internals guide | [Pro Git Book](https://git-scm.com/book/en/v2) |
| GitHub Flow branching and collaboration workflow | [GitHub Flow Guide](https://docs.github.com/en/get-started/using-github/github-flow) |
| Standardized commit conventions and best practices | [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/) |
