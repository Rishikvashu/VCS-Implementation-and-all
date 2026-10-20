# Naming Conventions Documentation

---

# Author Table

##  Author Table

| Author | Created on | Version | Last updated by | Last edited on | PRE Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| ------ | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ----------- |
| Arjun | 11-09-2026 | v1.0    | Arjun           | 11-09-2026     |       Team    |      Rajnish sharma      | Pritam/Komal           |     Abhsihek/Manish Nautiyal       |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Naming Conventions](#2-what-is-naming-conventions)
3. [Why Naming Conventions are Required](#3-why-naming-conventions-are-required)
4. [VCS Naming Conventions Workflow](#4-vcs-naming-conventions-workflow)
5. [Naming Convention Templates](#5-naming-convention-templates)
6. [Template Comparison](#6-template-comparison)
7. [Advantages](#7-advantages)
8. [Best Practices](#8-best-practices)
9. [Conclusion](#9-conclusion)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

# 1. Introduction

This document defines the naming conventions to be followed for branches, tags, and commit messages across all repositories in the project's Version Control System (VCS). 

---

# 2. What is Naming Conventions

Naming conventions are a standardized set of rules for naming branches, tags, and commit messages in a Git repository, ensuring consistency and clarity across the codebase and history.

---

# 3. Why Naming Conventions are Required
- **Improves Readability and Traceability:** Makes branches, commits, and tags easier to understand and helps track changes throughout the development lifecycle.

- **Simplifies Code Reviews and Audits:** Clear and standardized names help reviewers quickly understand the purpose of changes and make audit activities easier.

- **Reduces Confusion:** Consistent naming across multiple teams and repositories makes it easier to identify ownership, purpose, and status of development work.

- **Supports Enforcement:** Naming rules can be automatically validated using Git hooks, CI pipelines, and repository-level checks to prevent non-compliant changes.

---

# 4. VCS Naming Conventions Workflow


<img width="1672" height="940" alt="image" src="https://github.com/user-attachments/assets/d1b6a3ec-72b2-4c50-a61c-9490f0d8a05e" />


---

# 5. Naming Convention Templates

| **Type** | **Template** | **Example** |
| -------- | ------------ | ----------- |
| Branch Name | `<type>/<ticket-id>-<short-description>` | `feature/PROJ-123-add-login-page` |
| Tag Name | `v<major>.<minor>.<patch>` | `v1.4.2` |
| Commit Message | `<type>(<scope>): <ticket-id> <short summary>` | `fix(auth): PROJ-123 resolve token expiry bug` |
| Hotfix Branch | `hotfix/<ticket-id>-<short-description>` | `hotfix/PROJ-456-fix-crash` |
| Release Branch | `release/<version>` | `release/1.4.0` |

**Common branch types:** `feature`, `bugfix`, `hotfix`, `release`, `chore`, `docs`

**Common commit types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

---

# 6. Template Comparison

| **Element** | **Format** | **Enforced By** | **Purpose** |
| ----------- | ---------- | --------------- | ----------- |
| Branch | `type/ticket-desc` | Git hook / CI | Identify work type & ticket |
| Tag | `vX.Y.Z` (SemVer) | CI on release | Version tracking |
| Commit | `type(scope): ticket summary` | Commit-lint / CI | Change history clarity |

---

# 7. Advantages

- Consistent and predictable history
- Enables automated tooling such as changelogs and triggers
- Easier ticket traceability
- Simplifies release management

---

# 8. Best Practices

- Use lowercase, hyphen-separated names
- Always include ticket/issue ID
- Follow Conventional Commits
- Enforce via CI/pre-commit hooks
- Delete branches after merge
- Use SemVer for tags

---

# 9. Conclusion

Adopt Conventional Commits for commit messages and a `type/ticket-description` branch pattern, enforced through CI naming checks and commit-lint hooks. This ensures consistency, automation compatibility, and easier audits across all repositories.

---

# 10. Contact Information

| Name           | Email ID |
|----------------|----------|
| Arjun Kumar  | arjun.kumar.snaatak@mygurukulam.co |

---

# 11. References

| **Topic** | **Description** |
| --------- | --------------- |
| [Conventional Commits](https://www.conventionalcommits.org/) | Official spec for commit message format. |
| [Semantic Versioning](https://semver.org/) | Official SemVer specification for tags. |
| [Git Branching Model](https://nvie.com/posts/a-successful-git-branching-model/) | Reference branching strategy (Git Flow). |
