# SSL Documentation

<img width="345" height="133" alt="image" src="https://github.com/user-attachments/assets/f03d441f-1677-4c5e-b728-b2caf3b1ba4f" />

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Shivam Tomar | 11-09-2026 | v1.3 | Shivam Tomar | 22-09-2026 | Ishaan | Anuj Jain | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is SSL?](#2-what-is-ssl)
3. [Why SSL is Required](#3-why-ssl-is-required)
4. [Without SSL vs. With SSL](#4-without-ssl-vs-with-ssl)
5. [How SSL Works](#5-how-ssl-works)
6. [How to Get an SSL Certificate](#6-how-to-get-an-ssl-certificate)
7. [Different SSL Certificate Providers](#7-different-ssl-certificate-providers)
8. [Detailed Comparison](#8-detailed-comparison)
9. [Conclusion](#9-conclusion)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

# 1. Introduction

This document provides a standardized technical guide on SSL Certificates, explaining their operational concepts, working mechanisms, procurement methods, and provider options for DevOps and Cloud infrastructure environments.

---

# 2. What is SSL?

SSL (Secure Sockets Layer) is a security protocol that creates an encrypted and secure connection between a user's web browser and a server. It verifies the identity of the website and protects sensitive data from being stolen or changed while traveling over the internet, powering secure HTTPS connections.

---

# 3. Why SSL is Required

When information is sent over regular HTTP, it travels as plain text, meaning anyone on the same network can read or alter it. SSL solves this issue by adding security, verification, and privacy:

* Data Encryption in Transit: Protects sensitive information like passwords, credit card numbers, session tokens, and personal details so unauthorized users cannot read them.
* Server Authentication: Confirms that users are connecting to the real, legitimate website rather than a fake or malicious duplicate site.
* Data Integrity: Guarantees that data sent between the browser and server is not modified, corrupted, or injected with harmful scripts during transit.
* Man-in-the-Middle (MitM) Protection: Prevents attackers on public Wi-Fi or intermediate network routers from eavesdropping on or tampering with user sessions.
* Browser Trust and Protocol Support: Removes browser "Not Secure" warnings, shows the secure connection lock icon, and enables faster modern web protocols like HTTP/2 and HTTP/3.

---

# 4. Without SSL vs. With SSL

| **Parameter** | **Without SSL (Plain HTTP)** | **With SSL (HTTPS)** |
| ------------- | ---------------------------- | -------------------- |
| Data Privacy | Plain text traffic; visible to anyone on the network | Encrypted traffic; unreadable to third parties |
| Server Identity | Not verified; vulnerable to fake or duplicate websites | Verified and authenticated by a trusted Certificate Authority |
| Data Integrity | Can be easily modified or tampered with in transit | Protected from unauthorized changes and tampering |
| Browser Status | Shows "Not Secure" warning in the browser | Displays secure connection padlock indicator |

---

# 5. How SSL Works

The SSL handshake establishes a secure connection by verifying the server's identity and creating shared encryption keys before any website data is sent:

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/ce74bb82-3006-4a60-a75c-35ab6998aef4" />

### Handshake Overview

1. Client Hello: The browser contacts the server and shares the security settings and SSL versions it supports.
2. Server Hello & Certificate: The server selects the best security setting and sends back its digital certificate containing its public key.
3. Certificate Validation: The browser checks the certificate to ensure it was issued by a trusted Certificate Authority and is valid.
4. Key Exchange: The browser and server securely create a shared temporary encryption key.
5. Encrypted Communication: All web pages, API requests, and data are encrypted and safely exchanged using this shared key.

---

# 6. How to Get an SSL Certificate

### General Procurement Flow

Select Domain ➔ Generate Key & CSR (if manual) ➔ Verify Domain Ownership ➔ CA Issues Certificate ➔ Install Certificate on Server / Load Balancer ➔ Enable Automatic Renewal

### Common Procurement Approaches

* Free / Automated CAs: Best for regular Linux servers, containers, and Kubernetes environments using automated tools like Certbot.
* Cloud-Managed Services: Built-in cloud services that automatically create, install, and renew certificates on cloud load balancers and CDNs without manual work.
* Commercial CAs: Paid certificates used when companies need formal business verification, legal warranties, or custom enterprise contracts.

---

# 7. Different SSL Certificate Providers

SSL providers are grouped into three primary categories based on infrastructure needs:

### 1. Free / Automated
* Let's Encrypt: A free and open Certificate Authority that provides automated Domain Validated (DV) certificates via Certbot.

### 2. Cloud-Managed
* AWS Certificate Manager (ACM): Managed service that provides and automatically renews free certificates for AWS services like Load Balancers and CloudFront.
* Google Cloud Certificate Manager: Native certificate management for Google Cloud Load Balancing and GKE.
* Microsoft Azure App Service Certificates: Built-in certificate management for Azure web apps and gateways.

### 3. Commercial / Enterprise CAs
* DigiCert: Enterprise-level Certificate Authority offering high-assurance business validation and dedicated support.
* Sectigo: Commercial provider offering multi-domain, wildcard, and business certificate solutions.
* GlobalSign: Enterprise security provider specializing in corporate identity and internal PKI management.

---

# 8. Detailed Comparison

| **Provider** | **Category** | **Cost Model** | **Main Use Case** | **Automation** |
| ------------ | ------------ | -------------- | ----------------- | -------------- |
| Let's Encrypt | Free / Automated | Free | Linux VMs, web servers, containers, and Kubernetes | Native ACME (Certbot, cert-manager) |
| AWS ACM | Cloud-Managed | Free (on AWS) | AWS Load Balancers, CloudFront, and API Gateways | Fully automated cloud renewals |
| DigiCert | Commercial | Paid | Enterprise portals, e-commerce, and high-security systems | Enterprise APIs and ACME integration |
| Sectigo | Commercial | Paid | Multi-domain setups and commercial business platforms | Web portal and ACME connectors |
| GlobalSign | Commercial | Paid | Large enterprise networks and private PKI management | Enterprise PKI and automation APIs |

---

# 9. Conclusion

SSL secures network communication by providing encryption, authentication, and data integrity. For our infrastructure and deployment setup, we will use **Certbot (Let's Encrypt)** to issue and automatically renew free, trusted SSL certificates, ensuring secure HTTPS access with zero manual renewal overhead.

---

# 10. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Shivam Tomar | [shivam.tomar.snaatak@mygurukulam.co](mailto:shivam.tomar.snaatak@mygurukulam.co) |

---

# 11. References

| **Topic / Provider** | **Official Reference Link** |
| -------------------- | --------------------------- |
| Let's Encrypt Official Documentation | [letsencrypt.org/docs](https://letsencrypt.org/docs/) |
| Certbot Official Documentation | [certbot.eff.org](https://certbot.eff.org/) |
| AWS Certificate Manager Documentation | [docs.aws.amazon.com/acm](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) |
| OpenSSL Official Documentation | [openssl.org/docs](https://www.openssl.org/docs/) |
