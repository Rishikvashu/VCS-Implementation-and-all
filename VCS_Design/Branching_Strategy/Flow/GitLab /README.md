#  GitLab Flow Document

---

## Author Table

| Author | Created on | Version | Last updated by | Last edited on | PRE Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| ------ | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ----------- |
| Vineet | 12-09-2026 | v1.0 | Vineet | 12-09-2026 | Team | Aryan Mishra | | |
| Vineet | 12-09-2026 | v1.0 | Vineet | 12-09-2026 | Team | Aryan Mishra |Aditya Raj Singh | |

---

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Why GitLab Flow](#why-gitlab-flow)
4. [GitLab Flow Workflow Diagram](#gitlab-flow-workflow-diagram)
5. [GitLab Flow](#gitlab-flow)
   - [Step 0: Verify Git and GitLab Access](#step-0-verify-git-and-gitlab-access)
   - [Step 1: Create Repository](#step-1-create-repository)
   - [Step 2: Create Feature Branch](#step-2-create-feature-branch)
   - [Step 3: Make Changes](#step-3-make-changes)
   - [Step 4: Commit Changes](#step-4-commit-changes)
   - [Step 5: Push Feature Branch](#step-5-push-feature-branch)
   - [Step 6: Create Merge Request](#step-6-create-merge-request)
   - [Step 7: Review and Approve](#step-7-review-and-approve)
   - [Step 8: Merge Changes](#step-8-merge-changes)
6. [Advantages and Disadvantages](#advantages-and-disadvantages)
7. [Best Practices](#best-practices)
8. [Conclusion](#conclusion)
9. [Contact Information](#contact-information)
10. [References](#references)
---
# Introduction

**GitLab Flow** combines Git version control with GitLab features like Merge Requests and CI/CD pipelines. It provides a simple, structured workflow for managing code changes from development to production by tying branches to specific environments.

---

# Prerequisites

* **GitLab Account:** Active account with repository creation permissions.
* **Git Installed:** Verify using `git --version`.

---

# Why GitLab Flow

* Keeps the `main` branch clean, stable, and always deployable.
* Prevents direct, untested changes to production code.
* Encourages peer code reviews via Merge Requests (MRs).
* Integrates seamlessly with automated CI/CD pipelines.
* Simpler to manage than GitFlow, but more structured than GitHub Flow.

---
# GitLab Flow Workflow Diagram

<div align="center">
  <img width="153" height="862" alt="GitLab Flow Workflow Diagram" src="https://github.com/user-attachments/assets/7fa497ed-8102-4b97-9052-99ebf3109f65" />
</div>

1. Main Branch – The workflow starts from the `main` branch, which contains the stable code.

2. Create Feature Branch – A separate feature branch is created from `main` to work on a new feature or bug fix.

3. Feature Branch – Developers work on the required changes inside the feature branch without directly modifying `main`.

4. Make Changes & Commit – The required code changes are made and committed to the feature branch.

5. Push Branch – The feature branch is pushed to the GitLab remote repository.

6. Merge Request – A Merge Request (MR) is created to propose merging the feature branch into `main`.

7. Code Review – Team members review the code and provide feedback or request changes if required.

8. Merge to Main – After the review is completed and the required checks are passed, the feature branch is merged into `main`.

9. Main Branch Updated – The `main` branch now contains the newly merged changes.

10. CI/CD Pipeline – The CI/CD pipeline is triggered to automatically build, test, and prepare the application for deployment.

11. Build – The application is compiled or packaged to create a deployable build.

12. Test – Automated tests are executed to verify that the application works correctly.

13. Staging – The application is deployed to the staging environment for final validation.

14. Production – After successful validation, the application is deployed to the production environment.
# GitLab Flow

This Proof of Concept (POC) demonstrates the complete GitLab Flow workflow, starting from repository creation and ending with deployment/promotion.

---

## Step 0: Verify Git and GitLab Access

Before starting the GitLab Flow POC, verify that Git is installed and GitLab access is available.
<img width="1920" height="251" alt="Screenshot (482)" src="https://github.com/user-attachments/assets/f565c712-7926-498d-b65c-18635841fcf4" />

## Step 1: Create Repository

Log in to GitLab and create a new project. Then, clone it to your local machine.

### Clone and Verify
<img width="1920" height="325" alt="Screenshot (483)" src="https://github.com/user-attachments/assets/3a6b2769-3c6d-4e8a-9259-64ecd801c886" />

## Step 2: Create Feature Branch

The main branch should remain stable. Developers should create a separate feature branch for making changes.
<img width="1920" height="165" alt="Screenshot (484)" src="https://github.com/user-attachments/assets/da1f4488-549e-449b-b9d2-239b22a85e7a" />

## Step 3: Make Changes

After creating the feature branch, make the required changes in the working directory.
<img width="1920" height="303" alt="Screenshot (485)" src="https://github.com/user-attachments/assets/1c3067c1-6d78-4dcf-a9ab-0eb4ea6de11f" />

## Step 4: Commit Changes

Add the modified file to the staging area and commit it with a descriptive message.
<img width="1920" height="217" alt="Screenshot (486)" src="https://github.com/user-attachments/assets/c3f00ea5-fc15-4c18-88a3-e3402982083d" />

## Step 5: Push Feature Branch

Push the local feature branch to the GitLab remote repository so you can create a Merge Request.
<img width="1920" height="278" alt="Screenshot (487)" src="https://github.com/user-attachments/assets/813cff00-c3f7-4d53-a37b-9a6e372ff1a1" />

## Step 6: Create Merge Request

After pushing the feature branch, go to the GitLab UI to create a Merge Request (MR) to merge your changes into the main branch.

1. Open your repository in GitLab.
2. Go to **Merge requests** → **New merge request**.
3. Select **Source Branch:** `feature/gitlab-flow-demo` and **Target Branch:** `main`.
4. Fill in the Title and Description.
5. Assign a Reviewer and click **Create merge request**.

---

## Step 7: Review and Approve

The assigned reviewer checks the code changes before they are merged into the main branch to ensure code quality and stability.

1. The reviewer opens the Merge Request in GitLab.
2. Reviews the **Changes** tab for code correctness.
3. Checks the **Pipelines** tab to ensure all CI/CD checks have passed.
4. If satisfied, the reviewer clicks the **Approve** button.


---

## Step 8: Merge Changes

After the Merge Request is approved and all required CI/CD checks pass, the changes can be merged into the main branch.

1. In the GitLab MR UI, ensure **Delete source branch** is checked (this keeps the repository clean).
2. Click the **Merge** button.
3. Go back to your local terminal, switch to the main branch, and pull the latest merged changes:
<img width="1920" height="980" alt="Screenshot (496)" src="https://github.com/user-attachments/assets/5265438c-9b96-4fb0-a739-a053985a8c0a" />

```
```
# Advantages and Disadvantages

| Advantages | Disadvantages |
|---|---|
| 1. Single Source of Truth: The `main` branch always reflects the deployable state. | 1. Overkill for Small Projects: Might be too complex for solo developers or very small projects. |
| 2. Built-in Code Review: Merge Requests (MRs) enforce peer reviews before code integration. | 2. Requires Discipline: Developers must strictly avoid pushing directly to the `main` branch. |
| 3. Simplified Deployment: Seamlessly integrates with CI/CD for automated testing and deployment. | 3. Merge Request Dependency: Changes may take longer if review or approval is delayed. |
| 4. Traceability: Easy to track which features are deployed to which environment. | 4. Branch Management: Managing multiple feature branches can become complex in large teams. |
---

# Best Practices

*  Always branch from `main`: Keep your feature branches updated with the latest changes from the main branch.
*  Use Descriptive Branch Names: Use clear names like `feature/login-page` or `bugfix/header-issue`.
*  Keep Merge Requests Small: Smaller MRs are easier to review and less likely to cause conflicts.
*  Protect the `main` Branch: Configure GitLab repository settings to require approvals and passing pipelines before merging.
*  Delete Feature Branches: Always check "Delete source branch" when merging to keep the repository clean.

---

# Conclusion

This Proof of Concept (POC) effectively demonstrates how GitLab Flow provides a structured, secure, and efficient way to manage code changes. By leveraging feature branches, Merge Requests, and automated CI/CD pipelines, teams can deliver high-quality software with a reliable and predictable deployment process.

---

# Contact Information

| **Name** | **Email Address** |
| -------- | ----------------- |
| Vineet   | [vineet.shrivastava.snaatak@mygurukulam.co](mailto:vineet.shrivastava.snaatak@mygurukulam.co) |

---

# References

| **Topic** | **Reference Link** |
| --------- | ------------------ |
| GitLab Flow Documentation | [GitLab Docs - GitLab Flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html) |
| Git Branching Basics | [Git SCM - Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging) |
