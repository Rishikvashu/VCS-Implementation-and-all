# DNS Documentation

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | -------------------- | ------------------- | --------------- | --------------- | --------------- |
| Priyanshu | 09-09-2026 | 1.1 | Priyanshu | 14-09-2026 | Aryan Mishra | |  |
| Priyanshu | 01-10-2026 | 1.2 | Priyanshu | 01-10-2026 | Aryan Mishra | Aditya |  |


---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is DNS](#2-what-is-dns)
3. [Why DNS is Required](#3-why-dns-is-required)
4. [DNS Workflow — How It Works](#4-dns-workflow--how-it-works)
5. [How to Get a Domain](#5-how-to-get-a-domain)
6. [Different DNS Providers](#6-different-dns-providers)
7. [Detailed Comparison](#7-detailed-comparison)
8. [Advantages and Disadvantages](#8-advantages-and-disadvantages)
9. [Best Practices](#9-best-practices)
10. [Conclusion](#10-conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

---

# 1. Introduction

This document covers DNS (Domain Name System) explaining why a properly configured domain and DNS setup is essential for any production rollout of the platform, how DNS resolution works end-to-end from a user's browser to the authoritative nameserver, and how to acquire and configure a domain from scratch. 

---

# 2. What is DNS

**DNS (Domain Name System)** is the distributed, hierarchical naming system that translates human-readable domain names (e.g. `ot-microservices.io`) into the machine addresses (typically IP addresses) that computers use to locate each other on a network. It is often described as "the phonebook of the internet."

Core building blocks:

- **Domain name**: a human-readable identifier such as `ot-microservices.io`.
- **DNS record**: an instruction stored in a zone that tells resolvers how to answer queries for a name. Common types:
  - **A record** — maps a hostname to an IPv4 address (e.g. `ot-microservices.io → 34.120.10.5`).
  - **AAAA record** — maps a hostname to an IPv6 address.
  - **CNAME record** — aliases one hostname to another (e.g. `employee.api.ot-microservices.io → employee-api-lb.example.elb.amazonaws.com`).
  - **NS record** — delegates a zone (or subdomain) to a specific set of authoritative nameservers.
- **Zone / Hosted Zone**: the container of all DNS records for a domain (and its subdomains), managed through a DNS provider such as AWS Route 53 or Cloudflare.
- **Registrar vs. DNS provider**: the **registrar** is who you purchase/register the domain name from; the **DNS provider** (sometimes the same company, sometimes not) is who hosts the authoritative nameservers that answer queries for that domain's records.

---

# 3. Why DNS is Required

- **Human-readable, stable addressing**: Users of the `frontend` should never need to remember or hard-code a Load Balancer's IP address; a domain like `ot-microservices.io` remains stable even if the underlying infrastructure IP changes.
- **Service-to-service routing at scale**: As OT-Microservices grows, each backend (employee-api, attendance-api, salary-api) can be exposed on its own subdomain (e.g. employee.api.ot-microservices.io), letting an ingress/load balancer route requests to the correct service purely by hostname.
- **TLS/HTTPS certificate issuance**: Modern TLS certificates (e.g. via Let's Encrypt) are issued and validated against a domain name, not a bare IP — so HTTPS for the `frontend` and each API depends on DNS being correctly configured.
- **High availability and failover**: DNS-based health checks and weighted/failover routing policies (available in providers like Route 53) allow traffic to be redirected automatically if a region or service instance becomes unhealthy.

---

# 4. DNS Workflow — How It Works

<details>
<summary>Click to Expand DNS Resolution Workflow Diagram</summary>

<img width="1100" height="600" alt="image" src="https://github.com/user-attachments/assets/1e9dfe79-0317-4d75-9aa3-0086debcfd15" />

</details>

Using a request to the `http://example.com` as the example, DNS resolution proceeds as follows:

1. The user's **browser** checks its local cache; if no cached answer exists, it asks a configured **recursive DNS resolver** (e.g. the ISP's resolver, or a public resolver like 8.8.8.8 or 1.1.1.1).
2. If the resolver has no cached answer either, it queries a **root nameserver**, which doesn't know the answer directly but points the resolver to the nameservers responsible for the **`.com`** top-level domain (TLD).
3. The **TLD nameserver** in turn points the resolver to the **authoritative nameservers** — these are the nameservers assigned when the domain's hosted zone was created with a DNS provider (e.g. AWS Route 53 or Cloudflare).
4. The **authoritative nameserver** returns the actual record — an A or CNAME record.
5. The recursive resolver **caches** this answer for the record's TTL and returns it to the browser.
6. The browser connects to the returned IP, reaching the ingress/load balancer, which — based on the hostname in the request — routes traffic to the correct backend.
---

# 5. How to Get a Domain

Acquiring a domain for a project like OT-Microservices generally follows these steps:

1. **Choose a registrar:** Register the domain through an ICANN-accredited registrar (e.g. Amazon Route 53, Cloudflare Registrar, Google Domains successor/Squarespace Domains, Namecheap, GoDaddy). Registration is typically billed annually.
2. **Choose a domain name and TLD:** Decide on the name (e.g. `ot-microservices`) and a suitable top-level domain — `.com` for general commercial use, `.io` (popular for tech products), `.dev`, `.co`, or a country-code TLD, depending on availability and branding preference.
3. **Check availability:** Use a registrar's WHOIS/availability search to confirm the name isn't already registered.
4. **Complete registration and verification:** Provide contact/organization details (WHOIS information — many registrars now offer free WHOIS privacy) and pay the registration fee. Domain ownership must usually be verified by email.
5. **Point the domain to a DNS provider:** If the registrar and the intended DNS host differ, update the domain's **NS (nameserver) records** at the registrar to point to the chosen DNS provider's nameservers (e.g. Route 53's four assigned nameservers for the hosted zone).
6. **Create the hosted zone and records:** In the DNS provider, create a hosted zone for the domain and add the required records — A/CNAME records for `frontend` and each API subdomain, MX/SPF/DKIM/DMARC records mail.
7. **Validate propagation:** DNS changes can take from minutes up to 48 hours to propagate globally due to caching and TTLs; use tools like `dig`, `nslookup`, or online propagation checkers to confirm the records resolve as expected before relying on them in production.
8. **Renew on schedule:** Domains must be renewed periodically (commonly annually); enabling auto-renewal avoids accidental expiry and loss of the domain.

---

# 6. Different DNS Providers

| **Tool** | **Description** |
| -------- | --------------- |
| **AWS Route 53** | Amazon's scalable, highly available authoritative DNS service; integrates tightly with other AWS resources (ELB/ALB alias records, health checks, ACM certificate validation) and would pair naturally with an AWS-hosted service. |
| **Cloudflare DNS** | Free, extremely fast anycast DNS with a built-in CDN, DDoS protection, and free TLS; a common choice for public-facing frontends services. |
| **Google Cloud DNS** | Google Cloud's managed authoritative DNS service, well suited for workloads already running on GKE/GCP. |
| **Azure DNS** | Microsoft Azure's managed DNS hosting service, integrated with Azure Traffic Manager and other Azure networking primitives. |
| **GoDaddy** | Popular registrar-plus-DNS providers geared toward smaller teams and individual projects, typically simpler UIs at the cost of fewer advanced routing/health-check features. |
| **Hostinger** | Domain registrar and DNS hosting provider with an easy-to-use control panel, suitable for websites and smaller projects; provides standard DNS record management and integrates naturally with Hostinger hosting services. |

---

# 7. Detailed Comparison

| **Tool** | **Main Purpose** | **Speed** | **Ease of Use** | **Best Use Case** |
| -------- | ---------------- | --------- | --------------- | ------------------ |
| AWS Route 53 | Authoritative DNS + traffic routing policies | Very High | Moderate | Cloud-native deployments on AWS needing health checks, failover, and alias records to ELB/CloudFront |
| Cloudflare DNS | Fast anycast DNS + CDN + security | Very High | Easy | Public-facing services wanting free DNS, CDN caching, and DDoS protection together |
| Google Cloud DNS | Authoritative DNS for GCP workloads | High | Moderate | Teams already standardized on Google Cloud infrastructure |
| Azure DNS | Authoritative DNS for Azure workloads | High | Moderate | Teams already standardized on Microsoft Azure infrastructure |
| GoDaddy | Domain registration + basic DNS hosting | Moderate–High | Very Easy | Small projects or early-stage setups without advanced routing needs |
| Hostinger | Domain registration + DNS hosting | Moderate–High | Very Easy | Personal websites, small projects, portfolios, and applications needing simple DNS management and easy hosting integration |

---

# 8. Advantages and Disadvantages

| **Advantages** | **Disadvantages** |
| --------------- | ------------------- |
| Human-readable, memorable addressing instead of raw IPs for `frontend` and each API | Misconfigured records (wrong A/CNAME, expired domain) can take an entire stack offline even if the applications themselves are healthy |
| Enables independent subdomains per service, simplifying ingress routing as OT-Microservices grows | DNS propagation delays (driven by TTL/caching) mean changes are not instantaneous, which complicates fast rollbacks |
| Underpins TLS certificate issuance, so services can serve HTTPS securely | DNS is a common attack surface — DNS hijacking, cache poisoning, and typosquatting are real risks if records/registrar accounts aren't secured |
| Supports advanced traffic management (health checks, weighted/failover routing, geo-routing) | Managed DNS with advanced features (Route 53 health checks, Cloudflare enterprise features) can add ongoing cost as usage scales |
| Required for legitimate email delivery via SPF/DKIM/DMARC, directly relevant to `notification-worker` | Requires ongoing maintenance — renewals, record hygiene, and monitoring — that is easy to neglect once initially configured |

---

# 9. Best Practices

1. Enable domain auto-renewal and Registrar lock to prevent accidental expiry and unauthorized domain transfers that could take the entire system stack offline.
2. Configure SPF, DKIM, and DMARC Together configure all three email authentication records consistently to ensure outbound email deliverability and trust.
3. Use subdomains to isolate services expose microservices on distinct subdomains to keep routing, TLS certificates, and access logs cleanly separated per service.
4. Monitor DNS health and expiry proactively to track domain expiry dates and set up alerts for unexpected record changes or resolution failures to prevent unnoticed outages.

---

# 10. Conclusion

Regardless of provider, the domain and its records should be treated as security-sensitive infrastructure: registrar accounts should use strong authentication and locking, SPF/DKIM/DMARC should be fully configured to protect email deliverability and reputation, and record changes should go through the same change-control discipline as any other production infrastructure change.

---

# 11. Contact Information

| **Name** | **Team** | **Email** |
| -------- | --------- | ------- |
| Priyanshu | Kubeclan | [priyanshu.banwala.snaatak@mygurukulam.co](mailto:<email>) |

---

# 12. References

| **Description** | **Topic** |
| --------- | ---------------- |
| Official documentation for AWS's managed authoritative DNS service. | [AWS Route 53 Documentation](https://docs.aws.amazon.com/route53/) |
| Official documentation for Cloudflare's DNS, CDN, and security offerings. | [Cloudflare DNS Documentation](https://developers.cloudflare.com/dns/) |
| ICANN's overview of how domain registration and the domain name system work. | [ICANN — Beginner's Guide to Domain Names](https://www.icann.org/resources/pages/domain-names-beginners-guide-2017-07-25-en) |
