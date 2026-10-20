# DNS POC

<p align="center">
  <img width="90" height="auto" alt="DNS Icon" src="https://img.icons8.com/fluency/96/domain.png" />
</p>

---

## Author Table

| **Author** | **Created On** | **Version** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
|---|---|---|---|---|---|---|
| Anshul Kotiya | 18-9-2026 | 1.1 | 12-10-2026 | Shubham Rathi / Sunny | Shreya / Nikita | Piyush Upadhyay |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [POC Setup](#3-poc-setup)
   - [3.1 EC2 Instance](#31-ec2-instance)
   - [3.2 NGINX Configuration](#32-nginx-configuration)
   - [3.3 Application Availability](#33-application-availability)
   - [3.4 Domain Setup](#34-domain-setup)
   - [3.5 DNS Record Configuration](#35-dns-record-configuration)
   - [3.6 DNS Validation](#36-dns-validation)
4. [DNS Flow](#4-dns-flow)
5. [POC Result](#5-poc-result)
6. [Contact Information](#6-contact-information)
7. [References](#7-references)

---

# 1. Introduction

This POC demonstrates how to connect a custom domain to an application hosted on AWS EC2 using Hostinger DNS and NGINX.

---

# 2. Prerequisites

The following components are required:

* AWS Account
* Running EC2 Instance
* Application hosted on EC2
* Hostinger Account
* Registered Domain
* NGINX
---

# 3. POC Setup

## 3.1 EC2 Instance

The website is hosted on an Ubuntu EC2 instance.
### EC2 Details

| **Configuration** | **Value**        |
| ----------------- | ---------------- |
| Instance Name     | `dns-server` |
| Instance Type     | `t3.micro`       |
| Region            | `eu-north-1`     |
| Public IPv4       | `16.171.181.250`  |
| Instance State    | Running          |

Steps:

1. Open **AWS Console → EC2 → Instances**.
2. Select the website instance.
3. Verify its state and Public IPv4 address.



## 3.2 NGINX Configuration

NGINX serves the website files and handles incoming HTTP requests.

Install and start NGINX:

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable --now nginx
```

Create the website directory:

```bash
sudo mkdir -p /var/www/onepiece
```

Website file location:

```text
/var/www/onepiece/index.html
```

Create the NGINX configuration:

```bash
sudo nano /etc/nginx/sites-available/onepiece
```

Add:

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name piratekingluffy.online www.piratekingluffy.online;

    root /var/www/onepiece;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable and test the configuration:

```bash
sudo ln -s /etc/nginx/sites-available/onepiece /etc/nginx/sites-enabled/onepiece
sudo nginx -t
sudo systemctl reload nginx
```

<img width="1920" height="1140" alt="Screenshot 2026-10-12 183950" src="https://github.com/user-attachments/assets/d1e1804a-1fd1-4905-9cb6-085dfdf48ab8" />


## 3.3 Application Availability

Before configuring DNS, verify that the website opens through the EC2 public IP.

```text
http://16.171.181.250
```

Confirm that the website is displayed and the Security Group allows HTTP traffic on port `80`.

<img width="1920" height="1140" alt="Screenshot 2026-10-12 194624" src="https://github.com/user-attachments/assets/249a14e8-de67-4590-97a0-7ef339d030a8" />


## 3.4 Domain Setup

The domain used for this POC is:

```text
piratekingluffy.online
```

Steps:

1. Log in to Hostinger.
2. Open **Domains → piratekingluffy.online**.
3. Navigate to **DNS / Nameservers → DNS Records**.

## 3.5 DNS Record Configuration

Configure an A record to point the domain to the EC2 public IPv4 address.

| **Type** | **Name** | **Value** | **TTL** |
|---|---|---|---|
| A | `@` | `16.171.181.250` | 3600 |

Steps:

1. Add or update the A record.
2. Set Name to `@`.
3. Enter the EC2 public IPv4 address.
4. Save the record.

`@` represents the root domain `piratekingluffy.online`.

<img width="1920" height="1140" alt="Screenshot 2026-10-12 201755" src="https://github.com/user-attachments/assets/da24185c-510d-4889-bc19-1abf9124b754" />


## 3.6 DNS Validation

Run the following command on your computer:

```bash
nslookup piratekingluffy.online
```

Verify that the resolved IP matches the EC2 public IPv4 address. 

Open the domain in a browser:

```text
http://piratekingluffy.online
```

<img width="1920" height="1140" alt="Screenshot 2026-10-12 201914" src="https://github.com/user-attachments/assets/f58bdf9e-481b-4453-8218-9e8ad73d32a0" />

---

# 4. DNS Flow

<img width="326" height="796" alt="Screenshot 2026-10-12 210130" src="https://github.com/user-attachments/assets/a5fe21ec-20be-4480-8762-af43a7abff4c" />


---

# 5. POC Result

The custom domain was connected to the application hosted on AWS EC2 using a Hostinger DNS A record and NGINX.

**Note:** An Elastic IP was not used. If the EC2 public IPv4 address changes, the DNS A record must be updated.

---

# 6. Contact Information

| **Name** | **Email** |
|---|---|
| Anshul Kotiya | [anshul.kotiya.snaatak@mygurukulam.co](mailto:anshul.kotiya.snaatak@mygurukulam.co) |

---

# 7. References

| **Reference** | **Description** |
|---|---|
| [DNS Documentation — Sprint 1](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-110-PRIYANSHU/Documentation/Domain_Security/DNS_SSL/DNS/DOC/README.md) | DNS reference |
