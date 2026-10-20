 #  Implementation of DNS 

<div align="center">

<img width="100" height="100" alt="images" src="https://github.com/user-attachments/assets/cb983991-15f0-4aa5-8f22-569c7b8927dd" />

</div>

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Sarthak Bhatnagar | 10-09-2026 | v1.1 | Sarthak Bhatnagar | 10-09-2026 | Ishaan | Anuj Jain | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [Prerequisites](#3-prerequisites)
4. [Installation Steps](#4-installation-steps)
5. [Implementation / Execution](#5-implementation--execution)
6. [Validation](#6-Validation)
7. [Troubleshooting](#7-troubleshooting)
8. [Best Practices](#8-best-practices)
9. [Conclusion](#9-conclusion)
10. [Contact Information](#10-contact-information)
11. [References](#11-references)

---

# 1. Introduction

This Proof of Concept demonstrates the **Implementation of DNS resolution and routing for microservices** using:

The goal of this POC is to **replace hardcoded IP addresses with centralized domain routing to enable reliable service discovery and prepare for SSL/TLS certificate integration**.

---

# 2. Purpose

| Objective         | Description                                         |
| ----------------- | --------------------------------------------------- |
| Centralized DNS   | Provide DNS-based resolution for microservices.     |
| Service Discovery | Avoid static IPs and use domain names for services. |
| DNS Configuration | Configure `A` and `CNAME` records using BIND9.      |
| Validation        | Verify DNS configuration and query resolution.      |
| SSL/TLS Readiness | Prepare domain routing for future SSL/TLS setup.    |


---

# 3. Prerequisites

| **Prerequisite** | **Details** |
| ---------------- | ----------- |
| Operating System | Ubuntu 22.04 / 24.04 LTS or Debian Linux |
| Required Software | `bind9`, `bind9utils`, |
| Required Tools | `dig`, `nslookup`, `systemctl`, `named-checkzone` |
| Access | Root or `sudo` privileges on target DNS server |
| Permissions | Read/Write access to `/etc/bind/` configuration files |



---

# 4. Installation Steps

### Step 1: Update System Package Repository
Refresh the local package index to fetch the latest available versions:
```bash
sudo apt-get update
```
<img width="1016" height="756" alt="Screenshot 2026-09-11 at 3 11 32 AM" src="https://github.com/user-attachments/assets/b38f492a-4ec1-408d-8a17-6dcad4fec3ea" />


---


### Step 2: Install BIND9 Server and DNS Utilities
Install the BIND9 DNS daemon along with `bind9utils` and client query tools (`dig`, `nslookup`):
```bash
sudo apt-get install -y bind9 bind9utils dnsutils
```
<img width="1217" height="756" alt="Screenshot 2026-09-11 at 3 12 39 AM" src="https://github.com/user-attachments/assets/c8bcad0f-3799-4188-a11e-5be916ae08b1" />


---



### Step 3: Verify BIND9 Service Status
Confirm that the BIND9 daemon installed successfully and is active:
```bash
sudo systemctl status bind9 
```
<img width="1217" height="756" alt="Screenshot 2026-09-11 at 3 13 09 AM" src="https://github.com/user-attachments/assets/2820dcce-2443-42b1-844e-e6f0f37f1109" />


---

### Step 4: Create Dedicated Zones Directory
Create a dedicated folder to store forward and reverse zone record files:
```bash
sudo mkdir -p /etc/bind/zones
```
<img width="1217" height="114" alt="Screenshot 2026-09-11 at 3 20 32 AM" src="https://github.com/user-attachments/assets/9a31f694-4653-4e71-b1ff-98385bc44b8e" />


---

### Step 5: Configure Global BIND Options and Forwarders
Configure `/etc/bind/named.conf.options` to allow local queries and set upstream DNS forwarders:
```bash
sudo tee /etc/bind/named.conf.options <<EOF
options {
    directory "/var/cache/bind";
    recursion yes;
    allow-query { any; };
    forwarders {
        8.8.8.8;
        1.1.1.1;
    };
    dnssec-validation auto;
    listen-on { any; };
};
EOF
```
<img width="1217" height="334" alt="Screenshot 2026-09-11 at 3 26 22 AM" src="https://github.com/user-attachments/assets/7a6109d5-1f06-45c9-baf7-bcb83844ce06" />

---

# 5. Implementation / Execution

## 5.1 Define Local Zone in `named.conf.local`

Register the custom domain zone `mygurukulam.co`:

```bash
sudo tee /etc/bind/named.conf.local <<EOF
zone "mygurukulam.co" {
    type master;
    file "/etc/bind/zones/db.mygurukulam.co";
};
EOF
```
<img width="1217" height="178" alt="Screenshot 2026-09-11 at 3 28 00 AM" src="https://github.com/user-attachments/assets/fdfb9448-0d43-4e79-ab93-49973f1edbee" />

---

## 5.2 Create Forward Zone File

Create the DNS records file `/etc/bind/zones/db.mygurukulam.co`:

```zone
$TTL    300
@       IN      SOA     ns1.mygurukulam.co. admin.mygurukulam.co. (
                              2026091101 ; Serial
                                  3600   ; Refresh
                                  1800   ; Retry
                                604800   ; Expire
                                   300 ) ; Minimum TTL

; Name Server Record
@       IN      NS      ns1.mygurukulam.co.

; A Records for Infrastructure & Microservices
ns1                     IN      A       192.168.1.10
app                     IN      A       192.168.1.100
attendance              IN      A       192.168.1.100
employee                IN      A       192.168.1.100
salary                  IN      A       192.168.1.100

; CNAME Alias Record
api                     IN      CNAME   app.mygurukulam.co.
```

<img width="1217" height="632" alt="Screenshot 2026-09-11 at 3 30 50 AM" src="https://github.com/user-attachments/assets/92d632be-56f7-4937-b395-8b540b9fd329" />


---

## 5.3 Validate Zone and Configuration Syntax

Verify configurations before starting the service:

```bash
sudo named-checkconf
sudo named-checkzone mygurukulam.co /etc/bind/zones/db.mygurukulam.co
```
<img width="1217" height="99" alt="Screenshot 2026-09-11 at 3 32 34 AM" src="https://github.com/user-attachments/assets/d0700674-dffc-4277-97dd-0cc5efc2baaa" />


---

# 6. Validation

## 6.1 Test A Record Resolution using `dig`

```bash
dig @127.0.0.1 attendance.mygurukulam.co +short
```
<img width="1217" height="182" alt="Screenshot 2026-09-11 at 3 35 50 AM" src="https://github.com/user-attachments/assets/8462ff3e-2ec1-4600-b01b-f5a93eae79d1" />


---

## 6.2 Test Query with `nslookup`

```bash
nslookup salary.mygurukulam.co 127.0.0.1
```
<img width="1217" height="182" alt="Screenshot 2026-09-11 at 3 36 25 AM" src="https://github.com/user-attachments/assets/2d866462-5111-46cb-b9a4-22d67aee659f" />

---


## 7. Troubleshooting

| **Issue** | **Solution** |
|---|---|
| **Port 53 already in use** | Disable `systemd-resolved` DNS stub and restart the service. |
| **REFUSED query** | Add `allow-query { any; };` in `named.conf.options`. |
| **Zone file error** | Check FQDN names and add `.` at the end, e.g. `ns1.mygurukulam.co.` |
| **BIND service not starting** | Check permissions of `/etc/bind/zones/` and set them to `bind:bind`. |


# 8. Best Practices

| **Best Practice** | **Description** |
| ----------------- | --------------- |
| **Trailing Dots** | Always terminate fully qualified domain names with a dot `.` in zone files. |
| **Low TTL for Dynamic Environments** | Maintain TTL at 300 seconds for development and staging environments. |
| **Automated Syntax Check** | Always run `named-checkzone` and `named-checkconf` before restarting BIND9. |
| **Redundant Name Servers** | Configure a secondary slave DNS server to prevent a single point of failure. |

---

# 9. Conclusion

This Proof of Concept successfully demonstrates an authoritative DNS setup using **BIND9** for a microservices environment.

The POC validates:
- Microservice hostname resolution using `A` and `CNAME` records.
- Domain-based service access without hardcoded IPs.
- Successful DNS configuration and query validation.

The setup is validated and ready for **SSL/TLS integration**.

---

# 10. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Sarthak Bhatnagar | [sarthak.bhatnagar.snaatak@mygurukulam.co](mailto:sarthak.bhatnagar.snaatak@mygurukulam.co) |

---

# 11. References

| **Resource** | **Link** |
|---|---|
| BIND 9 Official Documentation | [BIND 9 Documentation](https://bind9.readthedocs.io/) |
| Ubuntu BIND9 Server Guide | [Ubuntu BIND9 Guide](https://ubuntu.com/server/docs/service-domain-name-service-dns) |
