<div align="center">

# VCS | Branching Strategy | Evaluation & Workflow Documentation

<img width="100%" height="200" style="object-fit: cover; border-radius: 6px;" alt="Branching Strategy Banner" src="https://images.unsplash.com/photo-1618401471353-b98afee0b2eb?q=80&w=1200&auto=format&fit=crop" />

</div>

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Anurag Maurya | 11-09-2026 | v1 | Anurag Maurya | 11-09-2026 | Divya M/Vishal Tyagi| Aayush Verma | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is a Branching Strategy](#2-what-is-a-branching-strategy)
3. [Why a Branching Strategy is Required](#3-why-a-branching-strategy-is-required)
4. [Overview of Key Branching Strategies](#4-overview-of-key-branching-strategies)
5. [How to Choose a Branching Strategy](#5-how-to-choose-a-branching-strategy)
6. [Comprehensive Strategy Comparison Table](#6-comprehensive-strategy-comparison-table)
7. [Advantages and Disadvantages](#7-advantages-and-disadvantages)
8. [Best Practices](#8-best-practices)
9. [Recommendation](#9-recommendation)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

# 1. Introduction

<div align="justify">

This document evaluates branching strategies for teams using Git and GitLab.

As a team grows, many people work on the code at the same time. Without clear rules, teams run into broken main branches, slow integrations, uncoordinated hotfixes, and commits that cannot be traced to a task. A defined branching model prevents these problems.

We compare five common models (**Git Flow**, **GitHub Flow**, **GitLab Flow**, **Trunk-Based Development**, and **Feature Branch Workflow**) against practical criteria, and record the one selected for our team's current workflow.

</div>

---

# 2. What is a Branching Strategy

<div align="justify">

A **branching strategy** is a set of agreed rules for how developers create, name, and merge Git branches.

A **branch** is a lightweight, independent line of work in Git. It lets a developer build a feature, fix a bug, or experiment without touching the stable code.

- **Long-lived branches** stay for the whole life of the repository, such as main, develop, or staging. They are the baseline for integration or deployment.
- **Short-lived branches** are temporary and made for one task, such as feature/SCRUM-93-branching or fix/SCRUM-99-hotfix. They are created from a long-lived branch, reviewed, merged, and then deleted.

</div>

<details>
<summary><b>Figure 1: Basic Single-Baseline Branch Hierarchy</b></summary>

<br>

![Basic Single-Baseline Branch Hierarchy](./assets/Branch%20Hierarchy.png)

*Figure 1: Basic single-baseline branch hierarchy.*

</details>

---

# 3. Why a Branching Strategy is Required

<div align="justify">

Without a defined strategy, teams face everyday problems that a clear model solves:

</div>

| Problem | Without a strategy | With a defined strategy | Benefit |
| ------- | ------------------ | ----------------------- | ------- |
| **Main branch stability** | Direct commits can break main | main is protected; code enters through Merge Requests | A stable, protected baseline |
| **Parallel work** | Edits interfere with each other | Separate feature/&lt;ticket-id&gt;-&lt;description&gt; branches | Developers work in isolation |
| **Code review and QA** | Code is pushed without review | Peer review and CI checks before merge | Better quality and governance |
| **Hotfixes** | Fixes applied without coordination | One standard hotfix path back to main | Controlled, traceable fixes |
| **Jira traceability** | Unclear who changed what and why | Branch name, ticket, and MR are linked | Easy to trace tickets to code |

---

# 4. Overview of Key Branching Strategies

<div align="justify">

**4.1 Git Flow.** A structured model built around scheduled releases. It has two long-lived branches (main and develop) plus feature, release, and hotfix branches. Features merge into develop. Release branches are cut from develop, stabilized, and merged into both main and develop. Hotfixes branch from main and are merged into both. *Best for:* traditional versioned software or mobile apps with planned releases.

**4.2 GitHub Flow.** A light, continuous model with one long-lived branch (main). Short-lived topic branches are opened as Pull Requests, reviewed, validated, and merged into main. *Best for:* web applications that deploy often from a single baseline.

**4.3 GitLab Flow.** Feature branching plus environment awareness (main → staging → production) or stable release branches. Changes always go into main first and then flow downstream. *Best for:* teams that need clear promotion gates between environments.

**4.4 Trunk-Based Development.** Everyone merges into a single trunk very often, using branches that live for hours or at most a day or two. It relies on feature flags, fast CI, and strong discipline. *Best for:* teams with fast CI pipelines (under about 10 minutes) and automated deployment.

**4.5 Feature Branch Workflow.** Every feature, fix, or task gets its own branch linked to a ticket. The branch is created from main, reviewed through a Merge Request with CI checks, and merged back into main. *Best for:* Agile Scrum teams that need ticket traceability, a protected main, and structured reviews.

</div>

---

# 5. How to Choose a Branching Strategy

<div align="justify">

The right choice depends on the team and its constraints. These questions guide the decision:

</div>

| Factor | Question to ask | Effect on the choice |
| ------ | --------------- | -------------------- |
| **Team size** | How many developers commit at once? | Larger teams benefit from feature branches and a protected main. |
| **Deployment pace** | Do we deploy continuously, daily, or per sprint? | Continuous deployment suits GitHub Flow or Trunk-Based; sprint releases suit Feature Branch or GitLab Flow. |
| **Environments** | Do we have formal staging and production gates? | Multiple environments suit GitLab Flow; a single baseline suits Feature Branch with a protected main. |
| **Feature flags** | Can we hide unfinished code in production? | Without flags and strong test automation, Trunk-Based needs more tooling first. |
| **Version support** | Must we maintain several old versions? | Needs stable release branches (Git Flow or GitLab Flow release model). |
| **Code review** | Is peer review required? | Needs protected branches with Merge Request approval. |

---

# 6. Comprehensive Strategy Comparison Table

<div align="justify">

A neutral comparison of the five models:

</div>

| Criteria | Git Flow | GitHub Flow | GitLab Flow | Trunk-Based | Feature Branch |
| -------- | -------- | ----------- | ----------- | ----------- | -------------- |
| **Complexity** | High | Low | Moderate | Low | Low |
| **Long-lived branches** | main, develop | main | main + environment or release | main only | main only |
| **Branch lifespan** | Days to weeks | Hours to days | Hours to days | Hours to 1-2 days | Short (within a sprint) |
| **CI/CD friendliness** | Moderate | High | High | High | High |
| **Releases** | Dual-merge releases | Continuous | Environment or stable branches | Tagged from trunk | Tagged from main |
| **Hotfixes** | Merge into main and develop | Branch from main | main first, then downstream | Fix on trunk, cherry-pick | Branch from main |
| **Merge conflict risk** | Higher | Low | Low to moderate | Low | Low |
| **Production safety** | High | Moderate | High | High (flags + CI) | High (MR gates + CI) |
| **Discipline needed** | Process-heavy | Moderate | Moderate | High | Moderate |
| **Best fit** | Versioned software | Web apps | DevOps, multi-environment | High-velocity CI/CD | Agile Scrum teams |

---

# 7. Advantages and Disadvantages

| Model | Advantages | Disadvantages |
| ----- | ---------- | ------------- |
| **Git Flow** | Clear release staging; bugs can be fixed on a release. | More merge complexity and double-merge effort. |
| **GitHub Flow** | Simple, fast feedback, easy to learn. | No built-in guidance for multiple environments or versions. |
| **GitLab Flow** | Upstream-first rule avoids lost hotfixes; clear environment promotion. | More branches to maintain; needs careful cherry-picking. |
| **Trunk-Based** | Less branch drift; fast integration. | Needs feature flags, fast CI, and strong tests. |
| **Feature Branch + Protected Main** | Good ticket traceability; stable main; one simple long-lived branch. | Long-running branches need regular rebasing. |

---

# 8. Best Practices

<div align="justify">

- **Protect the main branch:** set "Allowed to push" to No one, so every change comes through an approved Merge Request.
- **Name branches by ticket:** use feature/&lt;ticket-id&gt;-&lt;description&gt; or fix/&lt;ticket-id&gt;-&lt;description&gt;.
- **Keep branches short-lived:** merge as soon as the work is reviewed and validated, ideally within the sprint.
- **Require Merge Request review:** get peer approval and resolve comments before merging.
- **Run CI before merging:** the test and build stages in .gitlab-ci.yml must pass.
- **Sign commits where required:** use GPG or SSH signing (git commit -S) for cryptographic verification.
- **Delete merged branches:** this keeps the repository clean.
- **Fix production issues upstream first:** use a fix/ branch that targets main, then tag a release.

</div>

---

# 9. Recommendation

### Decision Statement

<div align="justify">

> **Based on our current workflow, operational needs, and decision criteria, the team will follow a Feature Branch Workflow with a protected main branch.**

</div>

### Justification

<div align="justify">

1. **Matches our workflow:** protected main, ticket-based feature branches, signed commits where required, and GitLab Merge Request reviews.
2. **Simple:** one long-lived branch, with no develop or environment branches to double-merge.
3. **Safe:** main stays stable because every Merge Request needs passing CI and peer approval.
4. **Traceable:** every change links back to a ticket without extra tooling.

</div>

### Target Feature Branch Workflow Mechanics

<details>
<summary><b>Figure 2: Feature Branch Workflow</b></summary>

<br>

![Feature Branch Workflow](./assets/Branching%20Stategies.png)

*Figure 2: Feature branch workflow showing a branch created from protected main, Merge Request review, CI validation, and merge.*

</details>

### Future Revisit Triggers

<div align="justify">

We will stay with the Feature Branch Workflow and protected main unless something changes:

- **GitLab Flow (environment model):** if compliance requires separate long-lived staging and production branches with promotion gates.
- **GitLab Flow (release model):** if we must support several major versions at once, such as v1.0 LTS and v2.0.
- **Trunk-Based Development:** if we adopt a full feature flag system, fast CI, and frequent automated deployments.

</div>

---

# 10. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Anurag Maurya | [anurag.maurya.snaatak@mygurukulam.co](mailto:anurag.maurya.snaatak@mygurukulam.co) |

---

# 11. References

| **Resource** | **Description / Link** |
| ------------ | ---------------------- |
| Official Git Documentation | [Git Branching Workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows) |
| GitLab Official Documentation | [GitLab Flow Documentation](https://docs.gitlab.com/ee/topics/gitlab_flow.html) |
| GitHub Official Documentation | [GitHub Flow Guide](https://docs.github.com/en/get-started/using-github/github-flow) |
| Trunk-Based Development | [Trunk-Based Development Reference](https://trunkbaseddevelopment.com/) |
| Atlassian Git Tutorials | [Comparing Git Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows) |
