<div align="center">

# VCS | Commit Hooks | Recommendation & Workflow Documentation

<img width="100%" height="200" style="object-fit: cover; border-radius: 6px;" alt="Commit Hooks Banner" src="https://images.unsplash.com/photo-1618401471353-b98afee0b2eb?q=80&w=1200&auto=format&fit=crop" />

</div>

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Anurag Maurya | 25-08-2026 | v1 | Anurag Maurya | 11-09-2026 | Divya M/Vishal Tyagi| Aayush Verma | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What are Git Commit Hooks](#2-what-are-git-commit-hooks)
3. [Why Commit Hooks are Required](#3-why-commit-hooks-are-required)
4. [Git Hook Types and Responsibilities](#4-git-hook-types-and-responsibilities)
5. [Jira + VCS Traceability](#5-jira--vcs-traceability)
6. [GitLab Server-Side Enforcement](#6-gitlab-server-side-enforcement)
7. [Commit Hook Tool Comparison](#7-commit-hook-tool-comparison)
8. [Recommended Architecture Options](#8-recommended-architecture-options)
9. [Weighted Decision Matrix](#9-weighted-decision-matrix)
10. [Recommended Architecture](#10-recommended-architecture)
11. [Recommended Commit Message Convention](#11-recommended-commit-message-convention)
12. [Recommended Jira + VCS Workflow](#12-recommended-jira--vcs-workflow)
13. [Security and Bypass Considerations](#13-security-and-bypass-considerations)
14. [Best Practices](#14-best-practices)
15. [Recommendation / Conclusion](#15-recommendation--conclusion)
16. [Contact Information](#16-contact-information)
17. [References](#17-references)

---

# 1. Introduction

<div align="justify">

This document explains how to validate Git commit messages and link them to Jira tickets for teams using Git and GitLab.

**Git** tracks code changes on a developer's machine, **GitLab** hosts the repository and runs CI/CD pipelines, and **Jira** tracks tasks. To connect a commit to its Jira ticket reliably, we need quick checks on the developer's machine and a strict check on the server.

The document compares four hook tools (Native Shell, pre-commit, Husky, and Lefthook), defines a commit message rule, and describes a workflow that links a ticket such as SCRUM-105 to its commits and GitLab Merge Requests.

</div>

> [!NOTE]
> The local hook and message rules here are recommended practices. Automatic linking, the Jira Development Panel, and Smart Commits depend on an active GitLab ↔ Jira integration.

---

# 2. What are Git Commit Hooks

<div align="justify">

**Git hooks** are scripts that Git runs automatically at key moments, such as committing or pushing. If a hook exits with an error, Git stops the action.

There are two kinds:

1. **Client-side hooks** run on the developer's computer (for example, pre-commit and commit-msg) and can stop a commit before it is created.
2. **Server-side hooks** run on the remote server (for example, GitLab Push Rules) during a push, and accept or reject the incoming commits.

**Main hook for this document: commit-msg.** It runs right after the developer writes the commit message, reads the message, and checks that it contains a valid Jira key. This is the best place to give instant feedback on the message format.

Other client-side hooks:

- **pre-commit:** runs before the commit is created. Used for formatting, linting, and secret scanning.
- **prepare-commit-msg:** runs before the editor opens. Can prefill the Jira key from the branch name, such as feature/SCRUM-105-login.
- **pre-push:** runs before a push. Used to run local tests.

</div>

---

# 3. Why Commit Hooks are Required

<div align="justify">

Without validation, it is easy to lose track of which change belongs to which ticket, and quality slips.

</div>

| Problem | Without hooks | With the recommended hooks | Benefit |
| ------- | ------------- | -------------------------- | ------- |
| **Missing Jira key** | Commits like "fixed bug" with no ticket | commit-msg rejects messages without a valid key | Every commit traces back to a ticket |
| **Inconsistent format** | Each developer writes differently | One format: &lt;JIRA-KEY&gt;: &lt;Short Summary&gt; | Clean, readable history |
| **Secrets in commits** | API keys or passwords get committed | pre-commit scans for secrets | Helps stop credential leaks |
| **Skipping local checks** | Developer uses --no-verify | GitLab Push Rules reject the push | Rules enforced on the server |
| **Late feedback** | Format errors found only in CI | commit-msg fails instantly on the developer's machine | Quick fix, no waiting |

---

# 4. Git Hook Types and Responsibilities

| Hook | Where it runs | Main use | Role in the Jira workflow |
| ---- | ------------- | -------- | ------------------------- |
| **pre-commit** | Local machine | Linting, formatting, secret scanning | Stops secrets and bad code from being committed |
| **prepare-commit-msg** | Local machine | Prefill the commit message template | Optional: adds the Jira key from the branch name |
| **commit-msg** | Local machine | Validate the commit message format | **Primary:** rejects messages without a valid Jira key |
| **pre-push** | Local machine | Run tests or check branch names | Optional: checks branch naming |
| **Push Rules** | GitLab server | Reject non-compliant commits on push | **Authoritative:** server-side enforcement |

---

# 5. Jira + VCS Traceability

<div align="justify">

Linking commits to Jira gives clear visibility from sprint ticket to branch, commit, Merge Request, and pipeline build. The figure below shows this chain for SCRUM-105.

</div>

<details>
<summary><b>Figure 1: Jira Ticket Traceability Chain</b></summary>

<br>

![Jira Ticket Traceability](./assets/Tracabilitychain.png)

*Figure 1: Traceability chain from Jira ticket to branch, commit, Merge Request, and pipeline.*

</details>

| Item | Jira key needed | Needs integration? | Result |
| ---- | --------------- | ------------------ | ------ |
| **Feature branch** | Branch name contains SCRUM-105 | Yes (GitLab Jira integration) | Branch appears in the Jira Development Panel |
| **Commit message** | Message starts with SCRUM-105: | No (hook, Push Rule, or CI) | Format is validated and the key becomes a Jira link |
| **Merge Request** | Title or description contains SCRUM-105 | Yes (GitLab Jira integration) | MR appears in the Jira Development Panel |
| **Smart Commits** | Commit contains commands such as #comment or #time | Yes (Smart Commits enabled) | Adds comments, logs time, or moves the issue |

---

# 6. GitLab Server-Side Enforcement

<div align="justify">

Local hooks give fast feedback, but anyone can skip them with the --no-verify flag. So the final check must happen on the GitLab server.

**6.1 GitLab Push Rules.** Push Rules run on the server when code is pushed, before any branch is updated.

- **Rule:** Require expression in commit messages.
- **Expression:** the regular expression shown below.
- **Bypass:** cannot be skipped from the client, so non-compliant pushes are rejected.
- **Availability:** needs GitLab Premium or Ultimate.

```regex
^(SCRUM|JIRA)-\d+:\s.+
```

**6.2 CI/CD Pipeline Validation (fallback).** On the Free tier, or as an extra safety layer, a CI job can check every commit in a Merge Request:

</div>

```yaml
commit_message_check:
  stage: test
  script:
    - |
      python3 -c "
      import os, sys, re, subprocess

      target = os.environ.get('CI_MERGE_REQUEST_TARGET_BRANCH_NAME', 'main')
      rev_range = f'origin/{target}..HEAD'
      raw_output = subprocess.check_output(['git', 'log', rev_range, '--pretty=format:%H %s']).decode('utf-8')

      failed = False
      pattern = re.compile(r'^(SCRUM|JIRA)-\d+:\s.+')

      for line in raw_output.splitlines():
          if not line.strip():
              continue
          parts = line.split(' ', 1)
          commit_sha = parts[0][:8]
          subject = parts[1] if len(parts) > 1 else ''
          if not pattern.match(subject):
              print(f'[ERROR] Invalid commit message on {commit_sha}: \"{subject}\"')
              failed = True

      if failed:
          sys.exit(1)
      print('[OK] All commits in Merge Request range match valid Jira issue key format.')
      "
  only:
    - merge_requests
```

<div align="justify">

Together with the Merge Request setting "Pipelines must succeed", commits with bad messages cannot reach the protected main branch.

</div>

---

# 7. Commit Hook Tool Comparison

| Criteria | Native Shell (core.hooksPath) | pre-commit (Python) | Husky (Node.js) | Lefthook (Go) |
| -------- | ----------------------------- | ------------------- | --------------- | ------------- |
| **Needs** | Nothing (POSIX shell) | Python and pip | Node.js and npm | A Go binary |
| **Setup** | Very easy: one git config command | Moderate | Moderate | Moderate |
| **Portability** | High (Linux, WSL, macOS, Git Bash) | Medium (tied to Python) | Medium (tied to Node.js) | High (single binary) |
| **Fit for a DevOps repo** | High | Moderate | Low (needs Node) | High |
| **Speed** | Instant (under 10 ms) | Fast | Fast | Fast |
| **Maintenance** | Very low (one script) | Moderate | Moderate | Low |

---

# 8. Recommended Architecture Options

<div align="justify">

Six options were evaluated for developer feedback, enforcement strength, and maintenance effort:

- **Option A:** Native local hook (core.hooksPath) + GitLab Push Rule / CI fallback.
- **Option B:** pre-commit framework + Push Rule / CI.
- **Option C:** Husky + Push Rule / CI.
- **Option D:** Lefthook + Push Rule / CI.
- **Option E:** Local hook only. It fails the bypass resistance requirement.
- **Option F:** Server Push Rule and CI only. It fails the fast local feedback requirement.

</div>

---

# 9. Weighted Decision Matrix

<div align="justify">

Each option is scored from 1.0 to 5.0 on seven weighted criteria. Higher is better.

</div>

| Criteria | Weight | A: Native + Server | B: pre-commit + Server | C: Husky + Server | D: Lefthook + Server | E: Local only | F: Server + CI only |
| -------- | -----: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Simplicity** | 20% | 5.0 | 3.5 | 3.0 | 4.0 | 5.0 | 4.0 |
| **Enforcement strength** | 20% | 5.0 | 5.0 | 5.0 | 5.0 | 1.0 | 5.0 |
| **Developer feedback** | 15% | 4.5 | 4.5 | 4.5 | 4.5 | 4.5 | 2.0 |
| **GitLab compatibility** | 15% | 5.0 | 4.5 | 4.0 | 4.5 | 1.0 | 5.0 |
| **Jira traceability** | 10% | 5.0 | 5.0 | 5.0 | 5.0 | 3.0 | 5.0 |
| **Maintenance** | 10% | 5.0 | 4.0 | 3.5 | 4.5 | 5.0 | 5.0 |
| **Portability** | 10% | 5.0 | 3.5 | 3.0 | 4.5 | 5.0 | 5.0 |
| **Weighted total** | **100%** | **4.93** | **4.30** | **4.03** | **4.55** | **3.33** | **4.35** |

---

# 10. Recommended Architecture

<div align="justify">

**Option A** is recommended: a native commit-msg hook distributed through core.hooksPath, plus GitLab server-side Push Rules (or a CI check as a fallback). It scores highest because it needs no extra tools, gives instant feedback, and cannot be bypassed on the server.

1. **Local feedback layer:** a POSIX shell script at .githooks/commit-msg, enabled with git config core.hooksPath .githooks.
2. **Server enforcement layer:** a GitLab Push Rule using the expression from section 6 (needs Premium or Ultimate).
3. **CI merge gate:** a lint job in .gitlab-ci.yml that checks commit messages before a Merge Request can merge.
4. **Jira integration layer:** the GitLab ↔ Jira integration, for automatic links, Development Panel updates, and optional Smart Commits.

</div>

---

# 11. Recommended Commit Message Convention

<div align="justify">

**Format:**

</div>

```text
<JIRA-KEY>: <Short summary in imperative mood> [#<smart-commit-command>]
```

<div align="justify">

**Validation expression:**

</div>

```regex
^(SCRUM|JIRA)-\d+:\s.+
```

<div align="justify">

**What each part means**

- **Jira key:** required, uppercase, for example SCRUM-105.
- **Colon and space:** required right after the key.
- **Short summary:** a clear, human-readable description of the change.
- **Smart Commit command:** optional, such as #comment or #time (needs integration).

</div>

| Type | Example commit message | Result |
| ---- | ---------------------- | ------ |
| **Valid** | SCRUM-105: Add commit-msg hook script for Jira format validation | Pass |
| **Valid** | SCRUM-105: Update pipeline linting job #comment Validated in WSL | Pass |
| **Invalid** | Add commit-msg hook script for Jira format validation | Fail: missing Jira key |
| **Invalid** | SCRUM-105 Add commit-msg hook script for Jira format validation | Fail: missing colon |
| **Invalid** | scrum-105: Add commit-msg hook script for Jira format validation | Fail: key must be uppercase |

---

# 12. Recommended Jira + VCS Workflow

<div align="justify">

The figure below shows how local Git checks, GitLab controls, CI/CD validation, and Jira tracking work together, from ticket creation to merge into main.

</div>

<details>
<summary><b>Figure 2: Commit Validation Workflow</b></summary>

<br>

![Jira Ticket Traceability & Commit Validation Architecture](./assets/commithook.png)

*Figure 2: Commit validation flow from Jira ticket creation through local hook validation and GitLab checks, to merge into protected main.*

</details>

<div align="justify">

**If something fails**

- **Invalid message locally:** the commit-msg hook stops the commit instantly.
- **Hook skipped with --no-verify:** the Push Rule rejects the push, or the CI job blocks the Merge Request.
- **CI pipeline fails:** the protected branch rules block the merge into main.
- **Jira integration not active:** Git and GitLab work normally, and linking can be enabled later.

</div>

---

# 13. Security and Bypass Considerations

<div align="justify">

1. **Client-side hooks are advisory.** They give fast feedback but are not a security boundary, because developers can skip them with --no-verify.
2. **Server-side rules are authoritative.** Push Rules and CI gates cannot be overridden from the client, so they must be the final enforcement.
3. **Hooks are not signatures.** Commit signing (git commit -S) verifies who made a commit, while message hooks only enforce ticket traceability.

</div>

---

# 14. Best Practices

<div align="justify">

- **Track hooks in the repository:** keep scripts in .githooks/ instead of the untracked .git/hooks/.
- **Use core.hooksPath:** set it with git config core.hooksPath .githooks so setup is one command.
- **Keep hooks fast:** commit-msg should finish in well under 50 ms.
- **Enforce on the server:** use Push Rules or CI so bad commits never reach main.
- **Standardize the key format:** uppercase key followed by a colon, such as SCRUM-105: summary.
- **Give clear errors:** when a message is rejected, show what was wrong and a valid example.

</div>

---

# 15. Recommendation / Conclusion

<div align="justify">

> **Recommended decision:** use a native Git commit-msg hook distributed through core.hooksPath, together with GitLab server-side Push Rules where available, and CI validation as a fallback or extra control.

**Summary**

- **Local validation:** fast feedback from the .githooks/commit-msg script.
- **Server enforcement:** authoritative Push Rules (needs Premium or Ultimate).
- **CI gate:** a pipeline job that stops unchecked Merge Requests.
- **Jira traceability:** automatic links and Development Panel updates once the integration is active.

**When to revisit this decision**

- **Husky:** if the repository becomes a Node.js project with a package.json.
- **pre-commit:** if the team standardizes on Python tooling everywhere.
- **Lefthook:** if hooks for many languages need one centrally distributed binary.
- **Push Rules:** when the GitLab tier changes between Free and Premium or Ultimate.

</div>

---

# 16. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Anurag Maurya | [anurag.maurya.snaatak@mygurukulam.co](mailto:anurag.maurya.snaatak@mygurukulam.co) |

---

# 17. References

| **Resource** | **Description / Link** |
| ------------ | ---------------------- |
| Official Git Documentation | [Git Hooks Reference](https://git-scm.com/docs/githooks) |
| Official Git Documentation | [Git Config core.hooksPath](https://git-scm.com/docs/git-config#Documentation/git-config.txt-corehooksPath) |
| Official Git Documentation | [git-commit Options & --no-verify](https://git-scm.com/docs/git-commit) |
| GitLab Official Documentation | [GitLab Push Rules](https://docs.gitlab.com/ee/user/project/repository/push_rules.html) |
| GitLab Official Documentation | [GitLab Jira Integration](https://docs.gitlab.com/ee/user/project/integrations/jira.html) |
| GitLab Official Documentation | [GitLab Jira Smart Commits](https://docs.gitlab.com/ee/user/project/integrations/jira.html#smart-commits) |
| pre-commit Framework | [pre-commit Framework Documentation](https://pre-commit.com/) |
| Husky Documentation | [Husky Git Hooks](https://typicode.github.io/husky/) |
| Lefthook Documentation | [Lefthook Fast Git Hooks Manager](https://github.com/evilmartians/lefthook) |
