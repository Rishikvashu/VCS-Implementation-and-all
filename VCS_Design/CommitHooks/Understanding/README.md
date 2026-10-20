# Commit Hooks Documentation

<p align="center">
  <img src="https://github.com/user-attachments/assets/3a02d164-249d-4d74-a73a-09eb2f5793df" alt="Commit Hooks Documentation" width="178"/>
</p>

---

# Author Table

| Author | Created On | Version | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Rani | 12-09-2026 | 1.0 | Ishaan | Anuj Jain | Piyush Upadhyay |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What are Commit Hooks](#2-what-are-commit-hooks)
3. [Why Commit Hooks](#3-why-commit-hooks)
4. [Purpose of Commit Hooks](#4-purpose-of-commit-hooks)
5. [Types of Git Hooks](#5-types-of-git-hooks)
6. [How to Use Git Hooks](#6-how-to-use-git-hooks)
7. [Advantages](#7-advantages)
8. [Disadvantages](#8-disadvantages)
9. [Best Practices](#9-best-practices)
10. [FAQ](#10-faq)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

## 1. Introduction

The purpose of this document is to explain **Git Commit Hooks** and demonstrate a simple Proof of Concept (POC).

Commit Hooks are used to automatically run checks or actions at specific points in the Git workflow. They can help teams automate validation, maintain code quality, and reduce repetitive manual work.

---

## 2. What are Commit Hooks

**Git Hooks** are scripts that Git automatically executes at specific points during the Git workflow.

They can be used for tasks such as:

- Code formatting
- Code validation
- Running tests
- Checking commit messages
- Preventing invalid commits
- Performing automated actions before or after Git operations

For example, a `pre-commit` hook runs automatically before Git creates a commit.

---

## 3. Why Commit Hooks

Commit Hooks are used to automate checks and actions during the Git workflow.

The main reasons for using Commit Hooks are:

| **Why** | **Description** |
| :--- | :--- |
| **Automation** | Automatically run repetitive checks and development tasks. |
| **Early Validation** | Identify issues before code is committed or pushed. |
| **Code Quality** | Help maintain coding standards and consistent development practices. |
| **Error Prevention** | Prevent common mistakes from entering the repository. |
| **Reduced Manual Effort** | Reduce the need to perform repetitive checks manually. |

---

## 4. Purpose of Commit Hooks

The main purposes of Commit Hooks are:

| Purpose | Description |
| :--- | :--- |
| **Automation** | Automate repetitive development tasks. |
| **Validation** | Validate code before it is committed. |
| **Code Quality** | Maintain coding standards and consistency. |
| **Error Prevention** | Prevent incorrect or incomplete code from being committed. |
| **Automated Checks** | Run tests and other checks during the Git workflow. |
| **Reduced Manual Effort** | Reduce the need for developers to perform checks manually. |

---

## 5. Types of Git Hooks

Git Hooks are mainly divided into two categories:

### 5.1 Client-Side Hooks

Client-side hooks run on the developer's local machine.

| Hook | Purpose |
| :--- | :--- |
| `pre-commit` | Runs before a commit is created. |
| `prepare-commit-msg` | Runs before the commit message editor is started. |
| `commit-msg` | Validates the commit message. |
| `post-commit` | Runs after a commit is created. |
| `pre-push` | Runs before code is pushed to a remote repository. |
| `post-checkout` | Runs after checkout. |
| `post-merge` | Runs after a successful merge. |

### 5.2 Server-Side Hooks

Server-side hooks run on the Git server or remote repository.

| Hook | Purpose |
| :--- | :--- |
| `pre-receive` | Runs before references are updated on the server. |
| `update` | Runs once for each reference being updated. |
| `post-receive` | Runs after all references have been updated. |

---

## 6. How to Use Git Hooks

Git Hooks are stored inside the repository's:

```text
.git/hooks/
```

### Step 1: Check Available Hooks

Run:

```bash
ls .git/hooks/
```

Git provides sample hook files in this directory.

### Step 2: Create a `pre-commit` Hook

Create or edit the hook:

```bash
nano .git/hooks/pre-commit
```

Add the following script:

```bash

#!/bin/bash

echo "Running pre-commit hook..."

# Check for unstaged changes
if ! git diff --cached --quiet; then
    echo "Validation passed: staged changes detected."
else
    echo "Error: No staged changes found."
    exit 1
fi

echo "Commit validation completed."
```

### Step 3: Make the Hook Executable

Run:

```bash
chmod +x .git/hooks/pre-commit
```

### Step 4: Test the Hook

Make a change, stage it, and create a commit:

```bash
git add .
git commit -m "Test commit hook"
```

The `pre-commit` hook runs automatically before the commit is created.

---

## 7. Advantages

| **Advantage** | **Description** |
| ------------------------- | ------------------------------------------------------------------------------- |
| **Early Error Detection** | Helps identify coding or validation issues before the code is committed. |
| **Automation** | Automatically performs repetitive checks without requiring manual execution. |
| **Improved Code Quality** | Helps maintain consistent coding standards and quality across development work. |

---

## 8. Disadvantages

| **Disadvantage** | **Description** |
| --------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Local Configuration** | Hooks stored in `.git/hooks/` are local and are not normally tracked with the repository. |
| **Slower Workflow** | Running heavy tests or checks can increase commit or push time. |
| **Limited Enforcement** | Local hooks can be bypassed or disabled, so important checks should also be performed through CI/CD. |

---

## 9. Best Practices

| Best Practice | Description |
| :--- | :--- |
| **Keep Hooks Fast** | Avoid unnecessarily long-running checks in local hooks. |
| **Use Clear Messages** | Display useful output when a check fails. |
| **Fail Safely** | Return a non-zero exit code when a required validation fails. |
| **Share Hook Configuration** | Use an appropriate shared-hook approach when hooks need to be consistent across a team. |
| **Use CI/CD Validation** | Keep important validation in CI/CD because local hooks are not a complete enforcement mechanism. |
| **Document Hooks** | Clearly document what each hook checks and how developers can configure it. |

> **Note:** Hooks stored directly in `.git/hooks/` are local to the repository and are not normally tracked as regular repository files.

---

## 10. FAQ

### 1. What is a Commit Hook?

A Commit Hook is a script that automatically executes at a specific point in the Git workflow.

### 2. Why are Commit Hooks used?

They are used to automate checks and actions, maintain code quality, and reduce manual effort.

### 3. What is the difference between `pre-commit` and `post-commit`?

`pre-commit` runs before the commit is created, while `post-commit` runs after the commit has been successfully created.

---

## 11. Conclusion

Git Hooks provide a simple way to automate actions at important points in the Git workflow.

A `pre-commit` hook can perform validation before a commit is created, helping developers identify problems early and maintain consistent code quality.

For team-wide enforcement, local hooks should be complemented by CI/CD and appropriate server-side validation.

---

## 12. Contact Information

| Name | Email |
| :--- | :--- |
| Rani Kumari | rani.kumari.snaatak@mygurukulam.co |

---

## 13. References

| **ResourceLink**                |                                                                                             |
| ------------------------------- | ------------------------------------------------------------------------------------------- |
| Git Hooks Documentation         | [Git Hooks Documentation](https://git-scm.com/docs/githooks)                                |
| Git Customizing Git - Git Hooks | [Git Customizing Git - Git Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) |                                           |

---
