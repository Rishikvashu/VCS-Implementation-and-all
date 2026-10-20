# AuthN & AuthZ Strategy: Notification Service

<div align="center">
  <img src="https://cdn-icons-png.flaticon.com/512/3602/3602145.png" alt="Notification Service Security" width="80"/>
</div>

---

## Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Sarthak Bhatnagar | 25-09-2026 | v1.0 | Sarthak Bhatnagar | 25-09-2026 | Ishaan | Anuj Jain | Piyush Upadhyay |

---
## Table of Contents

1. [Introduction](#1-introduction)
2. [Pre-requisites](#2-pre-requisites)
5. [Proof of Concept (POC)](#5-proof-of-concept-poc)
6. [Best Practices](#6-best-practices)
7. [Conclusion](#7-conclusion)
8. [Contact Information](#8-contact-information)
9. [References](#9-references)

---

## 1. Introduction

This Document provides the strategy for the VCS Authentication (Authn), Authorization (Authz) & Notification Strategy

---

## 2. Pre-requisites

| **Requirement** | **Description** |
|---|---|
| Identity Provider (IdP) | Admin access to Okta, Microsoft Entra ID, or Google Workspace |
| VCS Access | Admin access to GitHub Enterprise or GitLab |
| Notification Platform | Slack or Microsoft Teams webhook access |
| Local Tools | Git, web browser, and terminal |

---

## 3. Proof of Concept (POC) 

Use this section to attach proof screenshots demonstrating that each requirement has been implemented and tested.

### Proof 1: Authentication (2FA Enforcement)

<img width="1440" height="900" alt="Screenshot 2026-10-19 at 12 37 46 AM" src="https://github.com/user-attachments/assets/786edef8-4a41-4266-bbf0-d9773bb8d64d" />

<img width="1440" height="900" alt="Screenshot 2026-10-18 at 11 18 06 PM" src="https://github.com/user-attachments/assets/1a227c2b-afb9-44a2-a4b7-29be2cb08db8" />


### Proof 2: Authorization (Branch Protection Enforcement)

<img width="1440" height="900" alt="Screenshot 2026-10-19 at 12 28 18 AM" src="https://github.com/user-attachments/assets/1c9307df-c32f-4c07-96cb-ea97282c4657" />

### Proof 3: Webhook Delivery (200 OK)

<img width="1440" height="900" alt="Screenshot 2026-10-18 at 11 27 52 PM" src="https://github.com/user-attachments/assets/ca8050d3-f750-495d-9dc1-4d6df76730d0" />

<img width="1440" height="900" alt="Screenshot 2026-10-18 at 11 29 29 PM" src="https://github.com/user-attachments/assets/3c57b718-bee7-4475-b4f7-66b764c879ae" />

<img width="1440" height="900" alt="github_webhook_configured" src="https://github.com/user-attachments/assets/524cea50-4eba-412e-8d50-d9bfe0832dff" />

---

## 4. Best Practices

| **Best Practice** | **Description** |
|---|---|
| Least Privilege | Give users only the permissions they need. |
| Token Management | Set token expiry dates and renew tokens regularly. |
| Commit Signing | Use GPG or SSH signatures to verify commits. |
| Webhook Security | Use a secret key to verify webhook requests. |
| Audit Logs | Review logs regularly to identify suspicious activities. |

---

## 5. Conclusion

This Documentation provides the complete strategy for the VCS Authentication (Authn), Authorization (Authz) & Notification Strategy which helps to secure code access using SSO and MFA, prevents direct pushes to main branches, ensures code reviews and CI checks, and keeps developers updated through automated notifications.

---

## 6. Contact Information

| **Name** | **Email** |
| :--- | :--- |
| Sarthak Bhatnagar | <a href="mailto:sarthak.bhatnagar.snaatak@mygurukulam.co">sarthak.bhatnagar.snaatak@mygurukulam.co</a> |


---

## 7. References

| **Resource** | **Link** |
|---|---|
| GitHub Enterprise SAML SSO Guide | [View Documentation](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/using-saml-for-enterprise-iam) |
| GitHub Protected Branches | [View Documentation](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-a-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule) |
| Slack Incoming Webhooks API | [View Documentation](https://api.slack.com/messaging/webhooks) |
| Microsoft Teams Incoming Webhooks | [View Documentation](https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook) |
