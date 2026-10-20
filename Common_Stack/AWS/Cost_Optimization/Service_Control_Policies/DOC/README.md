<div align="center">

# AWS | Cost Optimization | Service Control Policies Documentation

<img width="100%" height="200" style="object-fit: cover; border-radius: 6px;" alt="AWS Service Control Policies & Cost Optimization Governance Banner" src="https://valiancesolutions.com/wp-content/uploads/2024/09/AWS-banner.png" />

</div>

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Anurag Maurya | 25-08-2026 | v1 | Anurag Maurya | 11-09-2026 | Divya M/Vishal Tyagi | Aayush Verma | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What are AWS Service Control Policies (SCPs)](#2-what-are-aws-service-control-policies-scps)
3. [Why SCPs and Cost Governance are Required](#3-why-scps-and-cost-governance-are-required)
4. [SCP Evaluation Model and Organization Hierarchy](#4-scp-evaluation-model-and-organization-hierarchy)
5. [Preventive SCP Cost Guardrails](#5-preventive-scp-cost-guardrails)
6. [What SCPs Cannot Do](#6-what-scps-cannot-do)
7. [AWS Cost Monitoring and Optimization Measures](#7-aws-cost-monitoring-and-optimization-measures)
8. [SCP vs Cost Optimization Tools](#8-scp-vs-cost-optimization-tools)
9. [AWS Cost Governance Workflow](#9-aws-cost-governance-workflow)
10. [Advantages and Disadvantages](#10-advantages-and-disadvantages)
11. [Best Practices](#11-best-practices)
12. [Recommended Governance Model](#12-recommended-governance-model)
13. [Implementation Considerations and Configuration Dependencies](#13-implementation-considerations-and-configuration-dependencies)
14. [Conclusion](#14-conclusion)
15. [Contact Information](#15-contact-information)
16. [References](#16-references)

---
# 1. Introduction

<div align="justify">

Controlling AWS costs across many accounts needs two things: controls that stop unwanted spending before it happens, and tools that watch and reduce the spending that does happen. **AWS Organizations** lets teams manage multiple accounts together, and its **Service Control Policies (SCPs)** set the maximum permissions those accounts can use.

This document (Jira ticket **SCRUM-117, Cost Optimization Designing**) explains how SCPs can act as cost guardrails. It also separates two ideas:

1. **Preventive controls (SCPs):** rules that block unapproved or risky resource creation.
2. **Cost management tools:** AWS services such as Cost Explorer, Budgets, Cost Anomaly Detection, Compute Optimizer, Savings Plans, and S3 Lifecycle policies, which track, alert on, and reduce actual spend.

SCPs do not calculate costs, show bills, or rightsize resources. Good cost control needs SCPs and cost tools working together.

</div>

---
# 2. What are AWS Service Control Policies (SCPs)

<div align="justify">

An **SCP** is a policy type in AWS Organizations that sets the maximum permissions for the accounts under it. SCPs never grant permissions. They act like a filter: an action works only if both the SCP and the account's own IAM policy allow it.

<details>
<summary><b>Figure 1: AWS Organizations Hierarchy</b></summary>

<br>

![AWS Organizations Hierarchy](./assets/scp_organization_hierarchy.png)

*Figure 1: AWS Organizations hierarchy showing where Service Control Policies can be attached.*

</details>

**Key points**

- **Where they attach:** the Organization Root, an Organizational Unit (OU), or a single member account.
- **Inheritance:** an SCP on a parent applies to every OU and account below it.
- **Who is affected:** all IAM users, IAM roles, and the root user of member accounts.
- **Who is exempt:** the Management Account, and service-linked roles that AWS services use in the background.
- **Default policy:** AWS attaches the managed policy FullAWSAccess to every node. When using Deny guardrails, it stays attached and only the unwanted actions are denied.

| Limit | Value | What it means |
| :--- | :---: | :--- |
| **SCP document size** | 10,240 characters | Keep policies short, or split them. |
| **SCPs attached directly** | 10 | Maximum per Root, OU, or account. |
| **OU nesting depth** | 5 levels | Maximum depth under the Root. |
| **Organizations mode** | All Features | SCPs do not work in "Consolidated Billing Only" mode. |

</div>

---
# 3. Why SCPs and Cost Governance are Required

<div align="justify">

Without central guardrails, anyone with access can launch expensive resources in unapproved regions or create resources that nobody can trace to a team. This often leads to surprise bills and forgotten resources.

| Challenge | Preventive SCP control | Outcome |
| :--- | :--- | :--- |
| **Unapproved regions** | Allow only approved regions using aws:RequestedRegion. | Helps avoid resources in costly or unapproved regions. |
| **Costly services** | Block expensive analytics services in non-production OUs. | Reduces the risk of costly clusters in sandboxes. |
| **Oversized resources** | Restrict high-cost EC2 families and EBS types. | Keeps resources within approved limits. |
| **Untracked spend** | Deny creation when mandatory tags are missing. | Every resource can be linked to a team or cost center. |
| **Unrestricted non-prod** | Apply stricter SCPs to non-production OUs. | Limits financial risk in development. |

</div>

> [!NOTE]
> Guardrails reduce the chance of unapproved spending. They do not remove waste from idle or inefficient resources that are within the allowed limits.

---
# 4. SCP Evaluation Model and Organization Hierarchy

<div align="justify">

When an IAM identity makes a request, AWS checks both the SCPs along the path (Root, parent OUs, member account) and the account's IAM policies.

1. **Explicit Deny wins:** if any SCP or IAM policy denies the action, the request is denied.
2. **SCP check:** with the usual FullAWSAccess plus Deny approach, the request passes as long as nothing denies it. With an allow-list approach, an Allow must exist at every level from Root to account.
3. **IAM check:** if the SCPs permit it, the IAM policy must also explicitly allow the action.

<details>
<summary><b>Figure 2: AWS Effective Permission Evaluation Flow</b></summary>

<br>

![AWS Effective Permission Evaluation Flow](./assets/scp_evaluation_flow.png)

*Figure 2: AWS effective permission evaluation flow combining SCP guardrails and IAM policies.*

</details>

</div>

---
# 5. Preventive SCP Cost Guardrails

> [!IMPORTANT]
> The policies below are reference examples. Test every SCP in a non-production account before attaching it across the organization.

<div align="justify">

**5.1 Approved Region Restrictions.** Allows work only in approved regions, so resources do not spread into unapproved or higher-priced ones. Admin roles are exempt to avoid blocking governance tasks.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyUnapprovedRegions", "Effect": "Deny", "Action": "*", "Resource": "*",
    "Condition": {
      "StringNotEquals": { "aws:RequestedRegion": ["us-east-1", "us-west-2", "eu-central-1"] },
      "ArnNotLike": { "aws:PrincipalARN": [
        "arn:aws:iam::*:role/OrganizationAccountAccessRole",
        "arn:aws:iam::*:role/AWSControlTowerExecution" ] }
    }
  }]
}
```

**5.2 High-Cost Service Restrictions.** Stops non-production accounts from using costly managed services such as Redshift without central approval.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyHighCostServicesInNonProd", "Effect": "Deny", "Resource": "*",
    "Action": ["redshift:*", "quicksight:*", "kinesisanalytics:*"]
  }]
}
```

**5.3 Mandatory Tag-on-Create Controls.** Denies creation of resources that lack the required cost tag. It works only for services whose API supports tagging at creation, so it depends on configuration.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyEC2CreationWithoutCostCenterTag", "Effect": "Deny",
    "Action": ["ec2:RunInstances", "ec2:CreateVolume"],
    "Resource": ["arn:aws:ec2:*:*:instance/*", "arn:aws:ec2:*:*:volume/*"],
    "Condition": { "Null": { "aws:RequestTag/CostCenter": "true" } }
  }]
}
```

**5.4 High-Cost Compute Instance Controls (illustrative).** Blocks specialized, expensive EC2 families such as GPU (p4, p5) and high-memory (u-) instances. Confirm the service supports the condition key before using it in production.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyHighCostInstanceTypes", "Effect": "Deny",
    "Action": "ec2:RunInstances", "Resource": "arn:aws:ec2:*:*:instance/*",
    "Condition": { "StringLike": { "ec2:InstanceType": ["p4*.*", "p5*.*", "x2gd*.*", "u-*.*"] } }
  }]
}
```

**5.5 EBS Volume Type Controls (illustrative).** Prevents expensive provisioned-IOPS volumes (io1, io2) in non-production environments.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "DenyExpensiveEBSVolumeTypes", "Effect": "Deny",
    "Action": "ec2:CreateVolume", "Resource": "arn:aws:ec2:*:*:volume/*",
    "Condition": { "StringEquals": { "ec2:VolumeType": ["io1", "io2"] } }
  }]
}
```

</div>

---
# 6. What SCPs Cannot Do

<div align="justify">

SCPs only allow or block actions. They cannot:

- See billing data, invoices, or past costs.
- Enforce a spending limit (for example, "deny if monthly spend exceeds $10,000").
- Detect unusual spending or send alerts.
- Rightsize resources based on CPU or memory use.
- Find or delete unused resources such as unattached EBS volumes, idle Elastic IPs, or old S3 objects.
- Start or stop instances on a schedule.
- Buy Savings Plans or Reserved Instances.

</div>

---
# 7. AWS Cost Monitoring and Optimization Measures

<div align="justify">

SCPs prevent unapproved spending. Tracking, alerting, and reducing cost are handled by AWS cost management tools.

- **Visibility:** Cost Explorer shows trends and forecasts. Cost Allocation Tags (such as CostCenter) and Cost Categories assign costs to teams. Cost and Usage Reports (CUR 2.0) deliver detailed billing data to S3 for Athena queries.
- **Alerts:** AWS Budgets sends email or SNS alerts when actual or forecasted spend crosses limits such as 80% or 100%. Cost Anomaly Detection uses machine learning to spot unexpected spikes and show the likely cause.
- **Rightsizing and scheduling:** Compute Optimizer analyzes EC2, EBS, Lambda, and ECS usage and recommends smaller sizes. Instance Scheduler automatically starts and stops EC2 and RDS instances in non-production accounts.
- **Storage:** S3 Lifecycle policies move objects to cheaper classes or delete them after a set age. EBS Lifecycle Manager automates snapshots and removes old backups. Unattached volumes, unused Elastic IPs, and obsolete load balancers should be cleaned up.
- **Network and logs:** VPC Gateway Endpoints send S3 and DynamoDB traffic around NAT Gateway charges. CloudWatch log groups should have fixed retention (for example, 14 days for Dev and 90 days for Prod) instead of "Never Expire".

**Commitment discounts** give large savings over On-Demand pricing in exchange for a 1-year or 3-year commitment.

| Option | Maximum discount | Scope and risk |
| :--- | :---: | :--- |
| **Compute Savings Plans** | Up to 66% | Flexible across EC2, Fargate, and Lambda in any region. Lowest risk. |
| **EC2 Instance Savings Plans** | Up to 72% | Fixed to one instance family in one region. Moderate risk. |
| **Standard Reserved Instances** | Up to 72% | Reservations for EC2, RDS, or ElastiCache. Higher risk if your architecture changes. |

</div>

> [!CAUTION]
> You pay for committed capacity whether or not you use it. Check workload stability with Cost Explorer recommendations before buying.

---
# 8. SCP vs Cost Optimization Tools

<div align="justify">

| Tool | Purpose | Type | Role in cost control |
| :--- | :--- | :--- | :--- |
| **SCPs** | Permission boundaries | Preventive | Block unapproved regions, services, instance types, and untagged resources. |
| **IAM Policies** | Access authorization | Preventive | Give least-privilege access to users and roles. |
| **Cost Explorer** | Cost visibility | Detective | Shows past trends and forecasts spend. |
| **AWS Budgets** | Budget tracking | Detective | Alerts when spend crosses thresholds. |
| **Cost Anomaly Detection** | Spike detection | Detective | Finds unusual spend and its cause. |
| **Cost Categories and Tags** | Allocation | Accountability | Assign costs to teams and cost centers. |
| **Compute Optimizer** | Rightsizing | Optimizing | Recommends resizing over-provisioned resources. |
| **Savings Plans / RIs** | Discounts | Optimizing | Lower rates in return for commitment. |
| **S3 Lifecycle Policies** | Storage tiering | Optimizing | Move or delete idle data. |
| **Instance Scheduler** | Scheduling | Corrective | Stop non-production instances after hours. |

</div>

---
# 9. AWS Cost Governance Workflow

<div align="justify">

The diagram below shows how preventive SCP guardrails combine with AWS tools for visibility, detection, optimization, and remediation.

<details>
<summary><b>Figure 3: AWS Cost Governance Workflow</b></summary>

<br>

![Reference AWS FinOps Cost Governance Architecture](./assets/aws_cost_governance_workflow.png)

*Figure 3: Cost governance flow combining preventive SCP controls with AWS cost monitoring, alerting, optimization, and remediation tools.*

</details>

</div>

---
# 10. Advantages and Disadvantages

<div align="justify">

| Layer | Advantages | Disadvantages |
| :--- | :--- | :--- |
| **SCPs** | • Central guardrails that member accounts cannot bypass.<br>• Block costly provisioning before it happens.<br>• Apply to all member accounts, including root users.<br>• Scale well across nested OUs. | • No view of billing or dollar amounts.<br>• Cannot find idle resources or rightsize.<br>• Overly strict Deny rules can break valid workloads.<br>• Limited to 10,240 characters and 10 direct attachments. |
| **Combined FinOps model** | • Covers prevention, detection, optimization, and remediation.<br>• Continuous visibility with automatic anomaly detection.<br>• Maximum savings through rightsizing, lifecycle rules, and discounts.<br>• Clear accountability through tagging. | • Needs more maturity and teamwork.<br>• Budgets and rightsizing need regular attention.<br>• Commitments carry financial risk. |

</div>

---
# 11. Best Practices

<div align="justify">

**SCP guardrails**

- Test every new SCP in a sandbox OU before using it in production.
- Use targeted conditions such as aws:RequestedRegion instead of broad service-wide Denies.
- Exempt break-glass and admin roles so you do not lock yourself out.
- Confirm that a service supports tag-on-create conditions before enforcing them.
- Keep approved-region rules in place.

**Cost management**

- Activate cost allocation tags (CostCenter, Environment, Owner) in the Billing Console.
- Set budget alerts at 80% and 100%.
- Turn on Cost Anomaly Detection.
- Review Compute Optimizer recommendations every month.
- Check usage history before renewing Savings Plans or Reserved Instances.
- Use Instance Scheduler for non-production compute.
- Use S3 Lifecycle rules and EBS lifecycle policies to remove old data and backups.
- Hold a monthly FinOps review of cost trends, tag coverage, and unused assets.

</div>

---
# 12. Recommended Governance Model

<div align="justify">

The recommended approach is to combine preventive SCP guardrails with AWS cost visibility, detection, and continuous optimization.

| Step | Action |
| :--- | :--- |
| **1. Prevent** | Use SCPs to control regions, services, instance types, and tagging. |
| **2. Detect** | Track spend with Cost Explorer, Budgets, and Anomaly Detection. |
| **3. Optimize** | Rightsize with Compute Optimizer and use Savings Plans or RIs. |
| **4. Remediate** | Schedule non-production instances and clean up S3 and EBS storage automatically. |

**Why this model.** Using only SCPs leaves running spend unmanaged. Using only billing tools lets unapproved, costly resources get created in the first place. Combining both prevents unapproved spending while also lowering day-to-day costs.

</div>

---
# 13. Implementation Considerations and Configuration Dependencies

<div align="justify">

- **Organization structure:** OUs such as Workloads-Prod, Workloads-NonProd, and Sandbox are examples. Attach policies according to your own structure.
- **Organizations mode:** it must be set to All Features to use SCPs.
- **Billing permissions:** IAM identities need permission for the billing and Cost Explorer (ce) actions to view Cost Explorer, Budgets, and Savings Plans.
- **Tag activation:** custom tags must be activated as Cost Allocation Tags before they appear in reports.
- **Compute Optimizer data:** for EC2 and Auto Scaling groups it needs at least 30 hours of CloudWatch metrics from the last 14 days, and recommendations can take up to 24 hours to appear.
- **API validation:** check each service's API before enforcing tag-on-create SCPs.

</div>

---
# 14. Conclusion

<div align="justify">

SCPs work best as preventive guardrails in AWS Organizations. They cap what member accounts can do, which helps stop unapproved regions, costly services, and untagged resources.

However, **SCPs are not cost tools.** They cannot track spend, send alerts, rightsize instances, or manage Savings Plans. For complete cost control, combine SCPs with visibility tools (Cost Explorer, cost allocation tags), alerts (Budgets, Cost Anomaly Detection), optimization tools (Compute Optimizer, Savings Plans), and cleanup tools (S3 Lifecycle policies, Instance Scheduler).

</div>

---

# 15. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Anurag Maurya | [anurag.maurya.snaatak@mygurukulam.co](mailto:anurag.maurya.snaatak@mygurukulam.co) |

---

# 16. References

| **Topic / Source** | **Description** |
| :--- | :--- |
| [AWS Organizations SCP User Guide](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html) | Configuring and managing Service Control Policies. |
| [AWS Organizations Quotas & Limits](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_reference_limits.html) | SCP size limit (10,240 characters) and attachment quotas. |
| [AWS IAM Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) | Reference for aws:RequestedRegion, aws:RequestTag/*, and other global condition keys. |
| [AWS Cost Management Documentation](https://docs.aws.amazon.com/cost-management/) | Cost Explorer, Cost Allocation Tags, and Cost Categories. |
| [AWS Budgets User Guide](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html) | Creating monetary, usage, and reservation budgets. |
| [AWS Cost Anomaly Detection User Guide](https://docs.aws.amazon.com/cost-management/latest/userguide/CostAnomalyDetection.html) | Setting up cost anomaly monitors and alert subscriptions. |
| [AWS Compute Optimizer User Guide](https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is-compute-optimizer.html) | EC2, EBS, Lambda, and ECS rightsizing recommendations. |
| [AWS Savings Plans User Guide](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html) | Compute and EC2 Instance Savings Plans. |
| [Amazon S3 Lifecycle Management Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html) | Automated S3 storage class transitions and expiration. |
| [AWS Instance Scheduler Solution](https://aws.amazon.com/solutions/implementations/instance-scheduler-on-aws/) | Automated start and stop scheduling for EC2 and RDS. |
