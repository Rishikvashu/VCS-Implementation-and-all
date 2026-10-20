# SSL POC
<img width="179" height="167" alt="image" src="https://github.com/user-attachments/assets/9c4af622-01ef-4443-be02-2254712f92b9" />

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ------------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Vashishtha Prakash | 31-08-2026 | v1.2 | 16-10-2026 | Sunny/Shubham | Shreya/Nikita | Piyush Upadhyay |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [Architecture / Flow](#4-architecture--flow)
5. [SSL Documentation](#5-ssl-documentation)
6. [Implementation Steps](#6-implementation-steps)
   * [6.1 - Step 1: Verify DNS](#61-step-1---verify-dns)
   * [6.2 - Step 2: Verify Website Access](#62-step-2---verify-website-access)
   * [6.3 - Step 3: Install Certbot](#63-step-3---install-certbot)
   * [6.4 - Step 4: Generate SSL/TLS Certificate](#64-step-4---generate-ssltls-certificate)
   * [6.5 - Step 5: Verify HTTPS access after SSL Setup](#65-step-5---Verify-HTTPS-access-after-SSL-Setup)
   * [6.6 - Step 6: Auto Renewal](#66-step-6---auto-renewal)
7. [SSL Verification](#7-ssl-verification)
8. [Limitations](#8-limitations)
9. [Conclusion](#9-conclusion)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

## 1. Introduction

This POC demonstrates SSL/TLS implementation using **NGINX, Certbot, and Let's Encrypt** to enable HTTPS communication for the domain:

```text
devsecurity.shop
```
The domain is configured through **Hostinger DNS** and points to an **AWS EC2 Ubuntu instance** running NGINX.

---

## 2. Objective

| Objective | Description |
|-----------|-------------|
| SSL Setup | Configure HTTPS for the domain |
| Security Validation | Ensure encrypted communication |
| DNS Configuration | Point the domain to the AWS EC2 server |
| Evidence | Validate the implementation using commands and screenshots |
| Auto Renewal | Verify the SSL certificate renewal mechanism |
| Outcome | Secure website with HTTPS |

---

## 3. Prerequisites

| Requirement | Description |
|-------------|-------------|
| Domain Name | `devsecurity.shop` |
| DNS Provider | Hostinger |
| Cloud Platform | AWS EC2 |
| Operating System | Ubuntu Linux |
| DNS Configuration | Domain pointing to the EC2 Elastic IP |
| Web Server | NGINX installed and running |
| Port 80 | Open for HTTP traffic |
| Port 443 | Open for HTTPS traffic |
| SSL Client | Certbot |
| Certificate Authority | Let's Encrypt |
| Access | AWS EC2 SSH access and Hostinger DNS access |

---

## 4. Architecture / Flow

<img width="494" height="360" alt="image" src="https://github.com/user-attachments/assets/830064f3-432f-459e-9dab-1a23b3aa83eb" />

---

<img width="494" height="367" alt="image" src="https://github.com/user-attachments/assets/48397afa-6c95-45f9-8e35-a447b97b283f" />

---

## 5. SSL Documentation

**[SSL Documentation](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-113-SHIVAM/Documentation/Domain_Security/DNS_SSL/SSL/DOC/README.md)**

---

## 6. Implementation Steps

### 6.1 Step 1 - Verify DNS

The domain DNS records were configured in Hostinger to point to the AWS EC2 Elastic IP.

The following records were configured:

| Type | Name | Points To | Purpose |
|------|------|-----------|---------|
| A | `@` | `35.154.166.200` | Root domain |
| A | `www` | `35.154.166.200` | WWW subdomain |
---
<img width="959" height="454" alt="DNS-Records" src="https://github.com/user-attachments/assets/3e1049c5-dacf-4855-86c2-a7a4223c708b" />

### Verify DNS resolution:

```bash
nslookup devsecurity.shop
```

Verify the `www` subdomain:

```bash
nslookup www.devsecurity.shop
```

Expected result:

```text
devsecurity.shop      -> 35.154.166.200
www.devsecurity.shop  -> 35.154.166.200
```
<img width="959" height="407" alt="NSLookup" src="https://github.com/user-attachments/assets/dcbcf688-78b1-4b2c-a66a-65a2ad290557" />

---

### 6.2 Step 2 - Verify Website Access

Before enabling SSL, verify that the domain is accessible over HTTP.

Open:

```text
http://www.devsecurity.shop
```

<img width="959" height="286" alt="Nginx-web" src="https://github.com/user-attachments/assets/2e7b17cc-57e0-42f2-aa92-57c97b4f1f13" />

### Verify HTTPS

Open the website using:

```text
https://www.devsecurity.shop
```
<img width="959" height="438" alt="HTTPS-No" src="https://github.com/user-attachments/assets/4be7a9c0-38d2-4f1a-ab06-3a04ab8afa7d" />

---

### 6.3 Step 3 - Install Certbot

Update the package repository:

```bash
sudo apt update
```

Install Certbot and the NGINX plugin:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

Verify the installation:

```bash
certbot --version
```

<img width="959" height="83" alt="image" src="https://github.com/user-attachments/assets/c5115f9c-5fb1-448b-bdec-3cfd9db388ab" />

---

### 6.4 Step 4 - Generate SSL/TLS Certificate

Generate the certificate for both the root domain and the `www` subdomain:

```bash
sudo certbot --nginx -d devsecurity.shop -d www.devsecurity.shop
```

Certbot validates domain ownership and configures NGINX for HTTPS.

When prompted for the HTTP-to-HTTPS redirect, select:

```text
2: Redirect
```

This ensures that HTTP requests are redirected to HTTPS.

Expected flow:

```text
http://www.devsecurity.shop
            |
            | Redirect
            v
https://www.devsecurity.shop
```
<img width="959" height="502" alt="SSL-Certificate-Renewal" src="https://github.com/user-attachments/assets/a41e2609-cc0d-4d9b-8b5f-0232fe6264af" />

---

### 6.5 Step 5 - Verify HTTPS access after SSL Setup

<img width="959" height="266" alt="HTTPS-Yes" src="https://github.com/user-attachments/assets/5cfa6dc8-1167-4c37-b47c-bfff77c55ba5" />

---

### Check the certificate details:

```bash
sudo certbot certificates
```

<img width="959" height="270" alt="image" src="https://github.com/user-attachments/assets/34bb9321-280a-4ed5-a1a4-5658ffa549d9" />

---

### 6.6 Step 6 - Auto Renewal

Check the Certbot renewal timer:

```bash
sudo systemctl status certbot.timer
```

Test the renewal process without actually renewing the certificate:

```bash
sudo certbot renew --dry-run
```

Expected result:

```text
Congratulations, all simulated renewals succeeded
```
<img width="959" height="192" alt="image" src="https://github.com/user-attachments/assets/e8a486be-365a-4057-9fb6-070c40ca8346" />
<img width="959" height="233" alt="image" src="https://github.com/user-attachments/assets/d7cf6034-adb0-47d6-939f-801eac9af593" />

---

## 7. SSL Verification

OpenSSL can be used to inspect the SSL certificate and TLS connection.

Run:

```bash
openssl s_client -connect www.devsecurity.shop:443 -servername www.devsecurity.shop
```

To display the certificate dates and subject:

```bash
echo | openssl s_client -connect www.devsecurity.shop:443 -servername www.devsecurity.shop 2>/dev/null | openssl x509 -noout -subject -issuer -dates
```

<img width="959" height="251" alt="image" src="https://github.com/user-attachments/assets/fba71303-2c54-4018-bb1c-fb4035a08a83" />

---

## 8. Limitations

| Limitation | Description |
|------------|-------------|
| Public Domain | Domain must be accessible for certificate validation |
| DNS Dependency | Incorrect DNS configuration can cause validation failure |
| Port Requirement | Ports 80 and 443 must be available as required by the configuration and validation method |
| Certificate Expiration | Let's Encrypt certificates have a limited validity period and require renewal |
| Renewal Dependency | Certbot timer/service and server connectivity must work correctly |
| Server Dependency | SSL configuration depends on the availability and correct configuration of the EC2/Nginx server |

---

## 9. Conclusion

This POC demonstrates SSL/TLS implementation using **NGINX, Certbot, and Let's Encrypt** for the domain `devsecurity.shop`. 

---

## 10. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Vashishtha Prakash | vashishtha.prakash.snaatak@mygurukulam.co |

---

## 11. References

| Topic | Description |
|-------|-------------|
| [Certbot](https://certbot.eff.org/) | Tool used to obtain and renew SSL/TLS certificates. |
| [Let's Encrypt](https://letsencrypt.org/) | Free Certificate Authority providing publicly trusted SSL/TLS certificates. |
| [NGINX](https://nginx.org/) | Web server used for HTTP and HTTPS configuration. |
| [SSL Labs](https://www.ssllabs.com/ssltest/) | Tool used to test SSL/TLS configuration. |
| [AWS EC2](https://docs.aws.amazon.com/ec2/) | AWS service used to host the NGINX server. |
| [Hostinger](https://www.hostinger.com/) | Domain and DNS provider used in this POC. |
