# Pull Request Documentation

<p align="center">
<img width="290" height="290" alt="6039189" src="https://github.com/user-attachments/assets/4ca657a5-3e6f-42e2-95e3-098f57cf072b" />
</p>

##  **Author Information**
| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Mansoor Alam | 08-09-2026 | 1.0 | Mansoor Alam | 08-09-2026 | Rajnish Sharma | Pritam/Komal | Abhishek/Manish Nautiyal |


---


## Table of Contents

1. [Introduction](#introduction)
2. [What is a Pull Request?](#what-is-a-pull-request)
3. [Why Do We Use Pull Requests](#why-do-we-use-pull-requests)
4. [Features of Pull Requests](#features-of-pull-requests)
5. [Pull Request Workflow](#pull-request-workflow)
   * [Advantages of Pull Requests](#advantages-of-pull-requests)
   * [Disadvantages of Pull Requests](#disadvantages-of-pull-requests)
6. [Jira and Pull Request Integration](#jira-and-pull-request-integration)
7. [Best Practices for Pull Requests](#best-practices-for-pull-requests)
8. [Conclusion](#conclusion)
9. [Contact Information](#contact-information)
10. [References](#references)

---



## Introduction

This document provides an overview of Pull Requests (PRs), which are widely used in open-source and enterprise environments. Pull Requests help teams review, discuss, and improve code changes before merging them into the main codebase. They also improve code quality, collaboration, and change tracking.



## What is a Pull Request?

A **Pull Request (PR)** is a request to merge code changes from one branch into another branch. It allows developers to review, discuss, and approve the changes before they are merged into the main codebase.

---


## Why Do We Use Pull Requests

| Reason | Description |
|---|---|
| **Collaboration** | Helps developers work together and review each other's changes. |
| **Code Quality** | Ensures that code is reviewed before it is merged. |
| **Transparency** | Keeps a record of code changes and discussions. |
| **Feedback** | Allows team members to suggest improvements before merging. |
| **Change Control** | Ensures that changes are reviewed and approved before merging into the main branch. |

---

## Features of Pull Requests

| Feature | Description |
|---|---|
| **Code Review** | Allows team members to review code changes before they are merged. |
| **Discussion and Feedback** | Allows developers to discuss changes and suggest improvements. |
| **Continuous Integration** | Runs automated tests and checks to identify issues before merging. |
| **Documentation** | Keeps a record of the changes, discussions, and approvals. |
| **Merge Control** | Ensures that changes are reviewed and approved before merging into the main branch. |

---




## Pull Request Workflow

<img width="1099" height="453" alt="PR architecture" src="https://github.com/user-attachments/assets/45f8bef4-7fb0-4e01-94c4-61a1bb3cd88a" />



The Pull Request workflow follows these steps:

1. **Create Branch** – Create a separate feature or bug-fix branch from the `main` or `develop` branch.
2. **Develop & Commit** – Make the required code changes and commit them to the feature branch.
3. **Open Pull Request** – Create a Pull Request from the feature branch to the target branch.
4. **CI/CD Checks** – Automated build, testing, and code quality checks are performed.
5. **Code Review** – Team members review the changes, provide feedback, and request updates if required.
6. **Approval** – Once the changes and checks are satisfactory, the reviewer approves the Pull Request.
7. **Merge** – The approved Pull Request is merged into the target branch.
8. **Main Branch** – The approved changes become part of the main codebase.

This process helps ensure that code changes are reviewed, tested, and approved before being merged into the main branch.

---



## Jira and Pull Request Integration

Jira and GitHub can be used together to track development work from an issue to a Pull Request.
The Jira issue provides the work item, while the GitHub Pull Request is used to review and merge the related code changes.

<img width="1425" height="393" alt="JIRA with PR workflow" src="https://github.com/user-attachments/assets/c7cb58d5-f9fe-4333-a5d8-8f7497ee1567" />




### Jira and Pull Request Workflow

1. **Create Jira Ticket** – Create a Jira issue for the required task or change.

2. **Create Branch** – Create a feature or bug-fix branch using the Jira issue key.

3. **Develop & Commit** – Make the required changes and use the Jira issue key in commit messages when required.

4. **Create Pull Request** – Create a Pull Request from the feature branch to the target branch.

5. **Link with Jira** – The Jira issue can show the related branch, commits, and Pull Request for tracking.

6. **Review & Merge** – Review the Pull Request, complete the required checks, and merge the changes after approval.


---
### Advantages of Pull Requests

| Benefit                             | Description |
|-------------------------------------|-----------------------------------------------|
| **Promotes Team Collaboration**     | Encourages multiple team members to review, suggest changes, and collaborate effectively.|
| **Improves Code Quality**           | By allowing multiple people to review the changes, potential bugs or improvements can be identified. |
| **Tracks Changes**                  | Helps maintain a record of what changes were made and why.  |
| **Enforces Best Practices**         | Pull requests can be set up to enforce guidelines and best practices before merging code.  |

### Disadvantages of Pull Requests

| Disadvantage | Description |
|:-------------|:------------|
| **Slower Development Cycle** | Waiting for reviews and approvals can delay merging and releasing new features. |
| **Review Bottlenecks** | If reviewers are busy or unavailable, PRs can pile up, creating a backlog. |
| **Overhead for Small Changes** | Even simple fixes (like typo corrections) require formal review, adding unnecessary process overhead. |
| **Merge Conflicts** | Long-living PRs can easily get outdated, leading to complex merge conflicts that are harder to resolve. |

---




## Best Practices for Pull Requests

| Best Practice | Description |
|:--------------|:------------|
| **Write Clear Titles and Descriptions** | Summarize *what* the change does and *why* it’s needed to help reviewers quickly grasp the purpose. |
| **Request Specific Reviewers** | Assign reviewers who have the right context or ownership of the code being modified. |
| **Link with Tickets** | Connect the PR to related issues (e.g., Jira, GitHub Issues) for full traceability. |
| **Address Review Comments Thoughtfully** | Engage respectfully with reviewer feedback; explain if you disagree instead of ignoring. |


---



### Conclusion

Through this documentation, we explored the complete Pull Request workflow, from creating a branch and Pull Request to code review, approval, and merge.

We also explored Jira and GitHub integration for tracking development work and code changes. Along with this, we covered the advantages, disadvantages, and best practices of Pull Requests to understand how they support effective code review and controlled code integration.

## Contact Information

| Name         | Email Address                                 |
|--------------|-----------------------------------------------|
| Mansoor Alam | mansoor.alam.snaatak@mygurukulam.co           |



### References
| Links                                             | Descriptions                                                    |
|---------------------------------------------------|-----------------------------------------------------------------|
|[Link](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git) |Pull Request |
|[Link](https://axolo.co/blog/p/part-1-what-are-github-pull-requests)| Github doc for pull request|
|[Link](https://opensource.com/article/19/7/create-pull-request-github)|Opensource.com|

