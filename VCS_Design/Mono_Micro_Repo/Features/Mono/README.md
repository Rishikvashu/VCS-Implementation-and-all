# Mono repo features
---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | -------------------- | ------------------- | --------------- | --------------- | --------------- |
| Priyanshu | 08-09-2026 | 1.0 | Priyanshu | 09-09-2026 | Aryan Mishra |  |  |
| Priyanshu | 01-10-2026 | 1.1 | Priyanshu | 01-10-2026 | Aryan Mishra | Aditya |  |


---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is a Monorepo](#2-what-is-a-monorepo)
3. [Why Monorepo is Used](#3-why-monorepo-is-used)
4. [Monorepo Workflow](#4-monorepo-workflow)
5. [Key Features of Monorepo Management](#5-key-features-of-monorepo-management)
6. [Different Tools for Monorepo Management](#6-different-tools-for-monorepo-management)
7. [Tool Comparison](#7-tool-comparison)
8. [Advantages and Challenges/Disadvantages](#8-advantages-and-challengesdisadvantages)
9. [Best Practices](#9-best-practices)
10. [Conclusion](#10-conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

---

# 1. Introduction

This document explores Monorepo as an alternative repository strategy to the multi-repo (poly-repo) model — explaining why monorepo management is worth considering, how the CI/CD workflow operates within a monorepo setup using affected-graph detection, and what key features make it scalable for growing teams. It compares the most widely used monorepo management tools — Nx, Turborepo, Bazel, Lerna, and others — side by side.

---

# 2. What is a Monorepo

A **monorepo** ("mono" = single, "repo" = repository) is a single version-controlled repository that holds the source code for multiple applications, services, and/or shared libraries, each typically organized into its own top-level folder or package, but sharing one Git history, one set of branch/PR policies, and (usually) one build-orchestration system. Example: An e-commerce company can use a single monorepo to manage its payment service, order service, authentication service, and shared libraries. All these components are maintained within one Git repository, making it easier to manage shared code, coordinate changes, and automate CI/CD across the entire application.

---

# 3. Why Monorepo is Used

1. **Centralized codebase:** A monorepo keeps multiple applications, services, and shared libraries in a single repository, making the entire system available from one place.
2. **Easier cross-service changes:** A change to a shared data model or API can be made across multiple services in a single commit and pull request instead of coordinating changes across separate repositories.
3. **Consistent dependencies and standards:** A monorepo makes it easier to maintain consistent versions, coding standards, configurations, and development practices across projects.
4. **Better code visibility:** Developers can easily explore related applications and services without switching between multiple repositories, improving understanding of the overall system.
5. **Atomic changes:** Changes involving multiple applications or services can be committed together, ensuring that related components remain compatible.
6. **Unified CI/CD:** A single repository can contain CI/CD workflows that detect changed components and build, test, or deploy only the affected applications or services.

---

# 4. Monorepo Workflow

<details>
<summary>Click to Expand Monorepo CI/CD Workflow Diagram</summary>

<img width="1042" height="874" alt="image" src="https://github.com/user-attachments/assets/16a165b6-8062-4e80-a8a7-3eda7e9f5d6a" />


</details>

1. A developer commits changes to one or more packages (e.g. `employee-svc`, `frontend`) inside the single monorepo.
2. The CI pipeline is triggered on push or pull request, as with any repository.
3. An affected-package detection step runs first. It computes a dependency graph across all packages and analyzes changes in the modified files against the last known-good build to determine exactly which packages are impacted, directly or transitively.
4. Only the affected packages are built, linted, unit-tested; unaffected packages are skipped entirely.
5. Affected services are deployed; unaffected services are left untouched, preserving the same "deploy only what changed" behaviour that a poly-repo setup gets naturally, but now within one repository.


---

# 5. Key Features of Monorepo Management

| **Feature** | **Description** |
| ----------- | --------------- |
| **Affected-Graph Build Detection** | Compute which packages are impacted by a given change so CI only builds, tests, and deploys necessary components instead of the whole repository. |
| **Remote/Distributed Caching** | Fetch build and test outputs for unchanged packages from a shared local or remote cache rather than recomputing them to accelerate CI execution. |
| **Workspace/Package Management** | Provide native support for declaring multiple packages within one repository, allowing each package to maintain its own manifest and dependencies. |
| **Task Orchestration & Pipelines** | Define execution dependencies across tasks to ensure shared libraries are built before consuming services. |
| **Unified Dependency Management** | Maintain dependency consistency across all packages using a single lockfile to prevent version drift. |
   
---

# 6. Different Tools for Monorepo Management

| **Tool** | **Description** |
| -------- | --------------- |
| **Nx** | Extensible build system with first-class monorepo support (affected-graph detection, caching, task orchestration); popular across JS/TS, and via community plugins for Go, Java, and Python. |
| **Turborepo** | High-performance build system for JavaScript/TypeScript monorepos, focused on fast incremental builds and remote caching; maintained by Vercel. |
| **Bazel** | Google's polyglot build system built specifically for very large, multi-language monorepos; steep learning curve but extremely scalable and hermetic. |
| **Lerna** | One of the original JavaScript monorepo tools, focused on versioning and publishing multiple npm packages from a single repository. |
| **Rush** | Microsoft's scalable monorepo manager for large JavaScript/TypeScript codebases, with built-in incremental builds and change tracking. |

---

# 7. Tool Comparison

| **Tool** | **Main Purpose** | **Speed** | **Ease of Use** | **Best Use Case** |
| -------- | ---------------- | --------- | --------------- | ------------------ |
| Nx | Affected-graph builds + caching, polyglot | High | Moderate | Mixed-language microservices monorepo |
| Turborepo | Fast incremental builds + remote cache | Very High | Easy | JS/TS-heavy monorepos (would suit the `frontend` package well) |
| Bazel | Hermetic, polyglot builds at massive scale | Very High (at scale) | Complex | Very large, multi-language enterprise monorepos |
| Lerna | Versioning/publishing JS packages | Moderate | Easy | Publishing multiple npm packages from one repo |
| Rush | Incremental builds for large JS/TS repos | High | Moderate | Enterprise-scale JavaScript/TypeScript monorepos |


---

# 8. Advantages and Challenges/Disadvantages

| **Advantages** | **Challenges/Disadvantages** |
| --------------- | ------------------------------ |
| Atomic cross-service commits — a single PR can update various services together, keeping them in sync | Without affected-graph tooling, CI time grows with the size of the whole repo rather than just the changed service |
| Shared code (health-check patterns, config parsing, clients) is reused directly | Coarser default access control — anyone with repo access can see/change every service unless path-based rules are configured |
| Single source of truth improves onboarding — one clone shows the entire repo | Git history and PR volume grow much faster, which can make history harder to navigate without good tooling/labels |
| Simplifies dependency and version alignment across services | Migrating an existing poly-repo (like OT-Microservices' repos) requires careful history preservation and tooling investment |
| Refactors spanning multiple services are safer and verifiable in one commit | Team/ownership boundaries need to be actively maintained via `CODEOWNERS` and folder conventions, since the repo boundary no longer enforces them |

---

# 9. Best Practices

1. Never let CI rebuild every package on every commit; adopt a tool like Nx, Turborepo, or Bazel from day one to keep pipeline times manageable as services are added.
2. Use `CODEOWNERS` and branch protection rules scoped per top-level package folder to preserve the team boundaries.
3. Maintain clear documentation at the monorepo root defining where each service, shared library, and infrastructure configuration lives.
4. Track build and test duration trends over time to prevent CI performance degradation as new packages are added.
5. Keep shared dependencies centralized, so services use consistent versions and avoid unnecessary dependency duplication.

---

# 10. Conclusion

A monorepo with affected-graph tooling (such as Nx or Turborepo ) would reduce the coordination overhead of multi-repo PRs and eliminate the duplication of common patterns across different services. For any modern engineering team managing a distributed application or a microservices platform, a monorepo is highly recommended, but with one critical condition: it must be paired with an advanced build orchestrator (like Nx or Turborepo).

---

# 11. Contact Information

| **Name** | **Team** | **Email** |
| -------- | --------- | -------- |
| Priyanshu | Kubeclan | [priyanshu.banwala.snaatak@mygurukulam.co](mailto:<email>) |

---

# 12. References

| **Topic** | **Description** |
| --------- | ---------------- |
| [Nx Documentation](https://nx.dev/getting-started/intro) | Official documentation for the Nx monorepo build system and affected-graph tooling. |
| [Turborepo Documentation](https://turbo.build/repo/docs) | Official documentation for Turborepo's incremental build and remote caching model. |
| [Bazel Documentation](https://bazel.build/about/intro) | Official documentation for Google's Bazel build system, commonly used for very large polyglot monorepos. |
