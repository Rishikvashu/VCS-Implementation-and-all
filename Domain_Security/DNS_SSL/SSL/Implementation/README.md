# SSL Certificate Implementation

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | -------------------- | ------------------- | --------------- | --------------- | --------------- |
| Priyanshu | 12-09-2026 | 1.0 | Priyanshu | 17-09-2026 | Aryan Mishra | Aditya | |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [Project Setup](#4-project-setup)
5. [Implementation / Execution](#5-implementation--execution)
6. [Validation](#6-validation)
7. [Observations](#7-observations)
8. [Use Cases](#8-use-cases)
9. [Troubleshooting](#9-troubleshooting)
10. [Best Practices](#10-best-practices)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

This Proof of Concept demonstrates securing a custom domain with a publicly trusted SSL/TLS certificate issued by **Let's Encrypt**, obtained and installed directly on the origin web server using **Certbot** with its Nginx plugin. Unlike a managed-service approach, the certificate here is issued and deployed straight onto the **Nginx** server running on an **Ubuntu EC2 instance**, with Certbot automatically editing the Nginx configuration to add TLS termination and HTTP → HTTPS redirects.

---

# 2. Objective

The objective of this POC is to:

- Confirm the existing Nginx server block correctly serves the site on port 80 for `ot-microservices.in` and `www.ot-microservices.in`.
- Use **Certbot** (with the Nginx plugin) to obtain/deploy a Let's Encrypt certificate for both the apex domain and its `www` subdomain.
- Confirm Certbot automatically updates the Nginx configuration with the required `listen 443 ssl` block and HTTP → HTTPS redirect rules.
- Validate the certificate is live and trusted by inspecting it directly in a browser.

---

# 3. Prerequisites

| **Prerequisite** | **Details** |
| ----------------- | ----------- |
| Operating System | Ubuntu (EC2 instance) |
| Required Tools | Nginx, Certbot with the Nginx plugin|
| Access | SSH or AWS Systems Manager access to the EC2 instance; `sudo` privileges |
| Dependencies | DNS name `ot-microservices.in` and `www.ot-microservices.in` ; ports **80** and **443** open in the instance's security group |

---

# 4. Project Setup

## 4.1 Confirm the Existing Nginx Server Block

Before requesting/installing a certificate, the existing Nginx site configuration was reviewed to confirm it was correctly serving the application on port 80:

```bash
cd sites-enabled
ls
cat ot-microservice
```

```nginx
server {
    listen 80;
    server_name ot-microservices.in www.ot-microservices.in;

    root /home/ubuntu/frontend/build;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

The configuration syntax was then validated, and the intended `server_name` confirmed:

```bash
sudo nginx -T | grep "server_name"
```

## 4.2 Install Certbot with the Nginx Plugin

Certbot and its Nginx plugin are installed via `apt`, which lets Certbot automatically read and edit the existing Nginx server block instead of requiring manual SSL configuration:

```bash
sudo apt-get update
sudo apt-get install -y certbot python3-certbot-nginx
```

---

# 5. Implementation / Execution

## 5.1 Install / Deploy the Let's Encrypt Certificate via Certbot

With the Nginx site verified, Certbot was used to deploy the certificate for `ot-microservices.in` and `www.ot-microservices.in`, and Nginx was reloaded to pick up the change:

```bash
sudo systemctl reload nginx
sudo certbot install --cert-name ot-microservices.in
```

Certbot confirmed the certificate was deployed for both hostnames directly into the site's Nginx config file:

<img width="1366" height="614" alt="image" src="https://github.com/user-attachments/assets/7f89f7ed-2de1-4592-abbf-ca5869142a82" />

## 5.2 Certbot Auto-Updates the Nginx Configuration

Certbot rewrote the site's Nginx config, adding a `listen 443 ssl` block referencing the issued certificate and key, the shared TLS options file, and the Diffie-Hellman parameters file:

<img width="1366" height="604" alt="image" src="https://github.com/user-attachments/assets/d645ef67-a1f4-436a-ab6c-1c42bef893da" />


## 5.3 Reload Nginx and Confirm HTTPS Locally

After the config change, Nginx was reloaded and HTTPS was confirmed to be served correctly from the instance itself:

```bash
sudo systemctl reload nginx
curl -I https://ot-microservices.in
```

Expected Output:
```text
HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)
Content-Type: text/html
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/bea2a427-eeac-4308-9908-69c73315398f" />

---

# 6. Validation

## 6.1 Browser-Level Certificate Verification

Navigating to `https://ot-microservices.in` and opening the certificate viewer confirmed the certificate is live, trusted, and correctly bound to the domain:

<img width="1366" height="723" alt="image" src="https://github.com/user-attachments/assets/b4e0bf7b-112b-42e6-927a-b7182d7a746c" />


---

# 7. Observations

## 7.1 Successfully Executed Components

| **Component / Module** | **Status** |
| ------------------------ | ----------- |
| Nginx site configuration verified for `ot-microservices.in` / `www.ot-microservices.in` | PASS |
| Certbot with Nginx plugin installed | PASS |
| Certificate deployed for apex + `www` via Certbot | PASS |
| Nginx auto-reconfigured with SSL block and HTTP → HTTPS redirects | PASS |
| Nginx reload and HTTPS verification via `curl` (`200 OK`) | PASS |
| Browser-level certificate trust verification | PASS |

## 7.2 Failed Components

| **Component / Module** | **Issue** |
| ------------------------ | ---------- |
| None (certificate flow) | An unrelated, non-blocking `xsel` clipboard error occurred during an incidental local `npx serve` test; it did not affect the SSL setup |

## 7.3 Key Findings

- Certbot's Nginx plugin can parse an existing server block and inject the required `listen 443 ssl` directives, certificate paths, and HTTP → HTTPS redirect rules automatically, without hand-editing the Nginx config.
- Let's Encrypt certificates are free but short-lived (**~90 days**), unlike ACM's ~13-month validity — making automated renewal (`certbot renew`, run via the `certbot.timer`/cron job) essential rather than optional.
- Because the certificate and private key live on the origin server's filesystem (`/etc/letsencrypt/live/...`), the certificate's lifecycle is tied to that specific EC2 instance, unlike ACM certificates, which are decoupled from any single compute resource.

---

# 8. Use Cases

| **Scenario** | **Commands / Actions** |
| ------------- | ------------------------ |
| Secure a new Nginx-served site with HTTPS | `sudo certbot --nginx -d domain -d www.domain` |
| Automate certificate renewal | Rely on the `certbot.timer` (or cron job) installed with Certbot to run `certbot renew` automatically before the ~90-day expiry |
| Force HTTP → HTTPS redirects | Accept Certbot's redirect prompt during `certbot --nginx`, or let it auto-insert the `301` blocks as shown above |
| Audit the certificate/trust chain | Inspect via the browser padlock → "Certificate Viewer" |

---

# 9. Troubleshooting

| **Issue** | **Cause** | **Solution** |
| ---------- | ---------- | ------------- |
| Certbot can't find a matching server block | The `server_name` in the Nginx config doesn't match the domain passed to Certbot | Confirm `server_name` in the relevant `sites-enabled` file exactly matches the domain(s) requested |
| `curl` still shows plain HTTP, no redirect to HTTPS | Nginx wasn't reloaded after Certbot edited the config | Run `sudo systemctl reload nginx` after any Certbot install/renewal |
| Certificate isn't renewing automatically | The `certbot.timer`/cron job isn't installed or is disabled | Check with `systemctl list-timers \| grep certbot`; test with `sudo certbot renew --dry-run` |
| Browser still shows an old or untrusted certificate | Browser cache, or `ssl_certificate`/`ssl_certificate_key` pointing at the wrong path | Clear the browser cache; confirm the paths point to `/etc/letsencrypt/live/<domain>/` |

---

# 10. Best Practices

1. Run `sudo certbot renew --dry-run` right after initial setup to confirm auto-renewal works, since Let's Encrypt's ~90-day validity makes renewal automation mandatory, not optional.
2. Always **reload** (not restart) Nginx after a certificate install/renewal, so the new certificate is picked up without dropping active connections.
3. Restrict SSH/SSM and `sudo` access to the instance, since the private key (`privkey.pem`) is stored directly on its filesystem.

---

# 11. Conclusion

Based on the validation results, Certbot with the Nginx plugin (backed by Let's Encrypt) is a simple, free, and effective way to provision and auto-renew SSL/TLS certificates directly on a self-managed Nginx origin server. It is recommended for setups where the application is served directly from an EC2 instance rather than behind an AWS-managed edge service (ALB/CloudFront), where ACM would otherwise be the more natural fit.

---

# 12. Contact Information

| **Name** | **Team** | **Email** |
| -------- | --------- | --------- |
| Priyanshu | Kubeclan | [priyanshu.banwala.snaatak@mygurukulam.co](mailto:priyanshu.banwala.snaatak@mygurukulam.co) |

---

# 13. References

| **Link** | **Resource** |
| -------- | ------------- |
| https://certbot.eff.org/instructions | Certbot — official setup instructions |
| https://eff-certbot.readthedocs.io/en/stable/using.html | Certbot User Guide |
| https://letsencrypt.org/docs/ | Let's Encrypt — official documentation |
| https://nginx.org/en/docs/http/configuring_https_servers.html | Nginx — configuring HTTPS servers |
