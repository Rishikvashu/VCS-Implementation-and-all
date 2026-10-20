#  PR Creation & Rules

## Author Table

| Author | Created on | Version | Last updated by | Last edited on | PRE Reviewer | L0 Reviewer    | L1 Reviewer  | L2 Reviewer              |
| ------ | ---------- | ------- | --------------- | -------------- | ------------ | -------------- | ------------ | ------------------------ |
| Arjun  | 11-09-2026 | v1.0    | Arjun           | 11-09-2026     | Team         | Rajnish Sharma | Pritam/Komal | Abhishek/Manish Nautiyal |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [Project Setup](#4-project-setup)
5. [Implementation / Execution](#5-implementation--execution)
6. [Validation](#6-validation)
7. [Observations](#7-observations)
8. [Conclusion](#8-conclusion)
9. [Contact Information](#9-contact-information)
10. [References](#10-references)

---

# 1. Introduction

This Proof of Concept demonstrates **Pull Request (PR) creation and PR rules** using the GitHub UI.

The POC validates that required reviewers, branch protection, and merge restrictions are enforced before changes are merged.

---

# 2. Objective

The objective of this POC is to:

* Demonstrate the standard PR creation process.
* Configure mandatory PR rules.
* Require a minimum of 1 approvals before merge.
* Prevent direct pushes to the protected branch.
* Verify that non-compliant PRs are blocked.

---

# 3. Prerequisites

| Prerequisite | Details                                  |
| ------------ | ---------------------------------------- |
| Repository   | GitHub repository                        |
| Access       | Repository write access                  |
| Permissions  | Branch and PR creation                   |
| Reviewers    | Minimum 1 reviewers                      |

---

# 4. Project Setup

## 4.1 Create Feature Branch

1. Open the GitHub repository.
2. Select the branch selector.
3. Click **New branch**.
4. Enter the branch name:

```text
feature/PROJ-123-add-login-page
```

5. Select `main` as the source branch.
6. Click **Create branch**.

<img width="1288" height="472" alt="image" src="https://github.com/user-attachments/assets/0fac4519-8042-46d7-86ce-af297b66842b" />



---

## 4.2 Configure PR Rules

Navigate to:

**Repository → Settings → Rules / Branch Protection**

Configure:

| PR Rule                 | Configuration |
| ----------------------- | ------------- |
| Pull Request required   | Enabled       |
| Required approvals      | Minimum 1     |
| Branch protection       | Enabled       |
| Conversation resolution | Required      |

<img width="998" height="628" alt="Screenshot from 2026-09-13 19-20-57" src="https://github.com/user-attachments/assets/3d13d8c8-c717-479c-ab2f-80c809a14b9c" />
<img width="998" height="628" alt="Screenshot from 2026-09-13 19-21-06" src="https://github.com/user-attachments/assets/c0ab2c9f-665e-4eba-b89a-bbaabc621df9" />


---

# 5. Implementation / Execution

## 5.1 Make Changes

1. Switch to the feature branch.
2. Open or create the required file.
3. Make the required changes.
4. Select **Commit changes**.
5. Commit the changes to the feature branch.
<img width="1294" height="348" alt="image" src="https://github.com/user-attachments/assets/58d2fdc7-9786-4aa6-b6df-3721186bad46" />

---

## 5.2 Create Pull Request

1. Open **Pull Requests**.
2. Click **New Pull Request**.
3. Select the feature branch as the source.
4. Select `main` as the target branch.
5. Enter the PR title and description.
6. Assign the required reviewers.
7. Create the Pull Request.

Verify:

* PR title
* PR description
* Source branch
* Target branch
* Reviewers
<img width="911" height="566" alt="image" src="https://github.com/user-attachments/assets/51ac2702-aebe-4ac1-a8e0-c85d66b6bdbd" />

---

# 6. Validation

## 6.1 Validate Approval Rule

Before receiving the required 1 approvals, attempt to merge the PR.

**Expected Result:** Merge should be blocked.

After receiving 1 approvals, verify that the approval requirement is satisfied.
<img width="1294" height="348" alt="Screenshot from 2026-09-13 19-43-25" src="https://github.com/user-attachments/assets/09423615-8b0a-4016-943b-65a27f4cfb66" />


---

## 6.2 Validate Direct Push Restriction

Attempt to make a direct change to the protected `main` branch.

**Expected Result:** Direct changes to the protected branch should be restricted according to the configured rule.

---

## 6.3 Validate Successful Merge

After all required conditions are satisfied:

* 1 approvals received
* Conversations resolved
* No merge conflicts

Click **Merge Pull Request**.

**Expected Result:** The PR is successfully merged.

<img width="911" height="126" alt="image" src="https://github.com/user-attachments/assets/4f646095-d419-4551-8e30-33cb3cd020a2" />

---

# 7. Observations

| Component               | Status |
| ----------------------- | ------ |
| Feature branch creation | PASS   |
| PR creation             | PASS   |
| Required approvals      | PASS   |
| Branch protection       | PASS   |
| Merge restriction       | PASS   |

### Key Findings

* PRs without the required approvals cannot be merged.
* Direct changes to the protected branch are restricted.
* PRs satisfying all configured rules can be merged.

---

# 8. Conclusion

This POC successfully demonstrates **Pull Request creation and PR rule enforcement using the GitHub UI**.

The validation confirms that required approvals, branch protection, and merge restrictions can be used to control changes before they are merged.

---

# 9. Contact Information

| Name        | Email ID                                                                        |
| ----------- | ------------------------------------------------------------------------------- |
| Arjun Kumar | [arjun.kumar.snaatak@mygurukulam.co](mailto:arjun.kumar.snaatak@mygurukulam.co) |

---

# 10. References

| Resource                 | Link                                                                                                                   |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| GitHub Pull Requests     | https://docs.github.com/en/pull-requests                                                                               |
| GitHub Branch Protection | https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches |
