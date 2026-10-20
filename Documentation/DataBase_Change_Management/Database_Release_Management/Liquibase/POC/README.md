# Proof of Concept: Liquibase Database Change Management

<p align="center">
  <img width="334" height="202" alt="image" src="https://github.com/user-attachments/assets/86c6d643-bfeb-4591-988f-6a312c8a869f" />
</p>

---

## Author Table

| Author | Created On | Version | Last Updated By | Last Edited On | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Palak Upadhyay | 13-09-2026 | 1.0 | Palak Upadhyay | 18-09-2026 | Aryan Mishra | - | - |
| Palak Upadhyay | 13-09-2026 | 1.1 | Palak Upadhyay | 06-10-2026 | Aryan Mishra | Aditya raj singh | - |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What is Database Change & Release Management](#2-what-is-database-change--release-management)
3. [Why Use Liquibase](#3-why-use-liquibase)
4. [Prerequisites](#4-prerequisites)
5. [Features of Liquibase](#5-features-of-liquibase)
6. [Architecture & Workflow](#6-architecture--workflow)
7. [Step-by-Step Setup & Execution Guide](#7-step-by-step-setup--execution-guide)
8. [Backup and Rollback Strategy](#8-backup-and-rollback-strategy)
9. [Best Practices](#9-best-practices)
10. [Conclusion](#10-conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

---

## 1. Introduction

This document presents a practical Proof of Concept (POC) on Liquibase for automated database change management.

It demonstrates table creation, column modification, and rollback execution using PostgreSQL.

The goal of this POC is to provide a clear step-by-step guide for managing database releases safely without manual risk.

---

## 2. What is Database Change & Release Management

Database Change Management is the practice of tracking and versioning database schema modifications over time.

Release Management automates pushing these schema changes across Dev, Testing, and Production environments safely.

---

## 3. Why Use Liquibase

Manual SQL execution on live databases is risky, error-prone, and lacks execution history.

Liquibase automates database migrations, maintains detailed execution audit logs, and allows instant rollbacks when errors occur.

---

## 4. Prerequisites

The following tools are required before starting the setup:

| Prerequisite | Details | Verification Command |
| :--- | :--- | :--- |
| **Operating System** | Linux (Ubuntu / RHEL / WSL2) or Windows | `cat /etc/os-release` |
| **Java Runtime** | OpenJDK / JRE (Version 17 or higher) | `java -version` |
| **Database Engine** | Target Relational DB (e.g. PostgreSQL / MySQL / Oracle) | `psql --version` or `mysql --version` |
| **JDBC Driver** | Database-specific JDBC Driver (`.jar` file) | `ls /opt/liquibase/lib/` |

---

## 5. Features of Liquibase

| Feature | Description |
| :--- | :--- |
| **Multi-Format Support** | Defines schema changes in SQL, YAML, XML, or JSON changelog files. |
| **Automated Tracking** | Stores execution history, author details, and timestamps in `DATABASECHANGELOG`. |
| **Idempotent Migration** | Checks execution logs to ensure changesets run only once without duplication. |
| **Checksum Protection** | Uses MD5 checksums to detect illegal edits to already executed scripts. |
| **Instant Rollback** | Reverts applied database changes automatically using defined rollback directives. |
| **Concurrency Control** | Uses `DATABASECHANGELOGLOCK` table to prevent simultaneous deployment collisions. |

---

## 6. Architecture & Workflow

<p align="center">
  <img width="1376" height="768" alt="liquibase_excalidraw_workflow_1789280431562" src="https://github.com/user-attachments/assets/d346d64a-2840-441c-8cf8-ebe108d22a79" />
</p>

###  Liquibase Workflow (Simplified Step-by-Step)

Here is how database changes flow from code to PostgreSQL:

1.  **Developer Pushes Code:**
   * Developer writes SQL changes in `changelog.sql` and pushes to **GitHub**.

2.  **CI/CD Triggers Liquibase:**
   * The CI/CD pipeline starts and hands `changelog.sql` to the **Liquibase Engine**.

3.  **Lock Database (`DATABASECHANGELOGLOCK`):**
   * Liquibase locks the database so two developers don't run deployments at the same time.

4.  **Check Past History (`DATABASECHANGELOG`):**
   * Liquibase checks the history table to find which SQL scripts are **new** and which were **already executed**.

5.  **Run New SQL:**
   * Liquibase executes only the **new SQL scripts** on the **PostgreSQL Database**.

6.  **Record Audit Log:**
   * Liquibase records who made the change, the timestamp, and script name into `DATABASECHANGELOG`.

7.  **Unlock Database:**
   * Liquibase unlocks `DATABASECHANGELOGLOCK`. The database migration is complete!

---

## 7. Step-by-Step Setup & Execution Guide

###   Step 1: Database & Credentials Setup

Verify that the PostgreSQL database service is running and create an isolated test database `poc` with authentication credentials:

```bash
# 1. Verify PostgreSQL service status
sudo service postgresql status || pg_isready

# 2. Create fresh test database 'poc'
sudo -u postgres psql -c "CREATE DATABASE poc;"

# 3. Set password for postgres user
sudo -u postgres psql -c "ALTER USER postgres WITH PASSWORD 'password1234';"
```

*This command checks if PostgreSQL service is running, creates database `poc`, and sets user credentials.*

<p align="center">
  <img width="1037" height="336" alt="Liq-poc-1" src="https://github.com/user-attachments/assets/c45b4f1f-64b7-46db-9a8e-7547a396264e" />
</p>

---

###  Step 2: Connection Configuration Setup

Create a dedicated project directory and configure the `liquibase.properties` file with database connection parameters:

```bash
# 1. Create project directory and navigate into it
mkdir -p ~/liquibase-poc && cd ~/liquibase-poc

# 2. Create liquibase.properties configuration file
cat <<EOF > liquibase.properties
changeLogFile=changelog.sql
url=jdbc:postgresql://localhost:5432/poc
username=postgres
password=password1234
driver=org.postgresql.Driver
EOF

# 3. Verify configuration file content
cat liquibase.properties
```

*This command verifies database URL, username, password, driver, and changelog file path.*

<p align="center">
  <img width="633" height="277" alt="liq-poc-2" src="https://github.com/user-attachments/assets/4a5c2be2-97bc-4b67-b344-25d4abd4daf1" />
</p>

---

### 🔹 Step 3: Initial Migration - Version 1.0 (Table Creation)

Define Changeset 1 in `changelog.sql` to create the `employees` table and execute the initial migration:

```bash
# 1. Create master changelog.sql file (Changeset 1)
cat <<EOF > changelog.sql
--liquibase formatted sql

--changeset palak:1
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);
--rollback DROP TABLE employees;
EOF

# 2. Execute Liquibase update
liquibase update

# 3. Verify database table creation and audit log entries
sudo -u postgres psql -d poc -c "\dt"
sudo -u postgres psql -d poc -c "SELECT id, author, filename, dateexecuted FROM databasechangelog;"
```

*This command executes pending changeset 1, creates the `employees` table, and verifies `DATABASECHANGELOG` audit log entries.*

<p align="center">
  <img width="937" height="701" alt="liq-poc-3" src="https://github.com/user-attachments/assets/03233a61-45f3-45ab-8bb0-472c77aca068" />
</p>

---

### 🔹 Step 4: Schema Evolution - Version 2.0 (Column Addition)

Append Changeset 2 to `changelog.sql` to add a new `department` column to the `employees` table without affecting existing data:

```bash
# 1. Append Changeset 2 to changelog.sql
cat <<EOF >> changelog.sql

--changeset palak:2
ALTER TABLE employees ADD COLUMN department VARCHAR(50);
--rollback ALTER TABLE employees DROP COLUMN department;
EOF

# 2. Execute Liquibase update for Version 2.0
liquibase update

# 3. Verify updated schema structure in PostgreSQL
sudo -u postgres psql -d poc -c "\d employees"
```

*This command applies schema evolution by adding `department` column without affecting existing table data.*

<p align="center">
  <img width="849" height="692" alt="liq-poc-4" src="https://github.com/user-attachments/assets/c3fed31f-d771-4668-a85a-01b9d6744ee0" />
</p>

---

### 🔹 Step 5: Backup and Rollback Strategy Execution

Demonstrate database rollback capabilities using count-based and milestone tag-based rollback commands:

```bash
# 1. Execute count-based rollback (Revert Changeset 2)
liquibase rollback-count 1

# 2. Verify column removal in PostgreSQL
sudo -u postgres psql -d poc -c "\d employees"

# 3. Tag stable release milestone (Version 1.0)
liquibase tag v1.0

# 4. Demonstrate rollback to milestone tag
liquibase rollback v1.0
```

*This command confirms that the `department` column has been safely removed and the schema is reverted back to Version 1.0.*

<p align="center">
  <img width="842" height="668" alt="liq-poc-5" src="https://github.com/user-attachments/assets/a4f23075-b843-4dc2-a071-044460d65a49" />
</p>

---

## 8. Backup and Rollback Strategy

- **Count-Based Rollback:** Reverts a specific number of applied changesets (`liquibase rollback-count <N>`).
- **Tag-Based Rollback:** Reverts schema changes back to a tagged release milestone (`liquibase rollback <tag_name>`).
- **Date-Based Rollback:** Reverts schema changes executed after a specific date (`liquibase rollback-to-date <date>`).

---

## 9. Best Practices

- Keep changesets immutable and never edit an already-deployed changeset in place.
- Always write an explicit `--rollback` statement for every changeset to ensure safe reversibility.
- Limit each changeset to a single logical schema change to maintain isolation and safety.
- Include descriptive author names and Jira ticket identifiers in changeset definitions.
- Trigger automated database backups prior to running migrations in production environments.

---

## 10. Conclusion

This Proof of Concept (POC) successfully demonstrates automated database change and release management using Liquibase.

Managing schema modifications as version-controlled code ensures full tracking, zero manual intervention, and instant rollbacks.

Adopting Liquibase eliminates deployment drift and secures database deployments across all lifecycle environments.

---

## 11. Contact Information

| Name | Email Address |
| :--- | :--- |
| Palak Upadhyay | [palak.upadhyay.snaatak@mygurukulam.co](mailto:palak.upadhyay.snaatak@mygurukulam.co) |

---

## 12. References

| Description | Reference Link |
| :--- | :--- |
| Official Liquibase documentation and concepts | [Official Liquibase Documentation](https://docs.liquibase.com/) |
| Liquibase Formatted SQL Changelogs tutorial | [Liquibase Formatted SQL Changelogs](https://docs.liquibase.com/concepts/changelogs/sql-format.html) |
| Liquibase Community Rollback Workflows guide | [Liquibase Rollback Workflows](https://docs.liquibase.com/workflows/liquibase-community/using-rollback.html) |
| Database Change Management Industry Best Practices | [Database Change Management Best Practices](https://www.liquibase.com/blog/database-change-management-best-practices) |
