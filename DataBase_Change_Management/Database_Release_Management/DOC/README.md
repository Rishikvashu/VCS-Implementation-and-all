# Database Release Management

![Database Release Management](https://img.shields.io/badge/Database-Release%20Management-blue?style=for-the-badge)

---

## Author Table

| Author       | Created on | Version | Last updated by | Last edited on | Pre Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer  |
| ------------ | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ------------ |
| Maqbool Alam | 08/09/2026 | 1.0     | Maqbool Alam    | 08/09/2026    | Team         | Rajnish | Pritam/Komal | Abhishek/Manish Nautiyal |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Why](#2-why)
3. [Version Control for Databases](#3-version-control-for-databases)
4. [Tools at a Glance](#4-tools-at-a-glance)
5. [Best Practices](#5-best-practices)
6. [Recommendation](#6-recommendation)
7. [Contact](#7-contact)
8. [References](#8-references)

---

## 1. Introduction

Database Release Management is basically CI/CD for your database. Instead of a DBA manually running scripts against production and hoping nothing breaks, schema and reference-data changes get written, reviewed, versioned, and rolled out the same way we already treat application code.

This used to be a manual, tribal-knowledge process — someone SSHs in, runs a `.sql` file, and three months later nobody remembers why Staging looks different from Prod. Managing it properly fixes that: every environment can be rebuilt to a known state at any time.

This doc covers what it is, why we need it, the version-control approaches out there, a comparison of the popular tools, and a recommendation.

---

## 2. Why

| Benefit | Description |
|---|---|
| **Consistency** | Dev, QA, Staging, and Prod stay in sync, eliminating the "works on my machine" problem. |
| **Auditability** | Every change is timestamped and attributed to someone, which is useful for audits and incident reviews. |
| **Automation** | Migrations run automatically in the pipeline without requiring manual intervention. |
| **Safer Rollbacks** | A proper process provides an undo path, so a bad release can be rolled back safely. |
| **Collaboration** | Multiple engineers can propose schema changes through PRs without conflicts. |
| **Speed** | Automation removes a common release bottleneck and makes deployments faster. |
| **Disaster Recovery** | Versioned migration history allows the database to be rebuilt from scratch when required. |

---

## 3. Version Control for Databases

Databases are trickier to version than plain code because they carry *state*, not just structure. Three common approaches:

- **Migration-based (imperative)** — Numbered scripts (`V1__create_users_table.sql`) applied in order, tracked in a history table. Flyway, Sqitch, Alembic, Django Migrations, Rails.
- **State-based (declarative)** — You define the desired end state; the tool diffs it against current state and generates the migration. Redgate, Atlas, Liquibase (in some modes).
- **Changelog-based** — Changes described in a tool-agnostic format (XML/YAML/JSON) so one changelog can target multiple databases. Liquibase is the classic example.

Either way, all DDL (and relevant seed/reference DML) should live in the same repo as the app, follow a clear naming convention, and only merge through reviewed PRs — same as any other code change.

**Typical flow:** 
<img width="1098" height="470" alt="image" src="https://github.com/user-attachments/assets/a9fb784b-d2e7-4f8c-923d-0324b41ab037" />

---

## 4. Tools at a Glance

| Tool | Style | DB Support | Ease of Use | Rollback | Good Fit For |
| --- | --- | --- | --- | --- | --- |
| **Flyway** | Imperative, plain SQL | Wide (20+) | High | Manual (auto-undo on paid tier) | Polyglot teams wanting simple SQL-first migrations |
| **Liquibase** | Hybrid, changelog-based | Wide (50+) | Medium | Built-in rollback tags | Enterprises needing multi-format changesets & audit trails |
| **Sqitch** | Imperative, dependency graph | Postgres, MySQL, SQLite, Oracle | Medium | Native revert scripts | Teams who want zero proprietary metadata |
| **Alembic** | Imperative, auto-diffed | Any SQLAlchemy DB | High (Python) | Downgrade scripts | Python/SQLAlchemy apps |
| **Django Migrations** | Imperative, auto-generated | Postgres, MySQL, SQLite, Oracle | High (Django) | Reverse migrations | Django apps |
| **Rails AR Migrations** | Ruby DSL | Rails-supported DBs | High (Rails) | Manual/reverse | Rails apps |
| **Redgate** | Declarative, GUI-driven | SQL Server/Oracle | High | Built-in | SQL Server shops with budget |
| **Atlas** | Declarative schema-as-code | Postgres, MySQL, SQLite | Medium | Auto-generated | Teams wanting "Terraform for schema" |
| **DBmate** | Imperative, plain SQL | Postgres, MySQL, SQLite | High | Manual | Small/polyglot teams wanting zero frills |
| **golang-migrate** | Imperative, SQL/Go | Wide | High | Manual | Go microservices |

---

## 5. Best Practices

- Keep every migration script in the same repo as the app, with a consistent naming scheme
- Never edit a migration that's already run anywhere shared — write a new one instead
- Keep schema changes (DDL) and data changes (DML) separate
- Write and test a rollback path wherever it's feasible, especially for destructive changes
- Only the pipeline applies migrations to Prod — not individual engineers by hand

---

## 6. Recommendation

If you need multi-format changesets, built-in rollback tagging, or you're managing many different database platforms at enterprise scale, **Liquibase** is a strong alternative.

If a team is already locked into a specific framework, just use its native tool — Alembic, Django Migrations, or Rails Active Record — since it's already wired into the ORM.

Whichever tool wins, the process stays the same: version-controlled scripts, peer review, CI validation, controlled CD rollout.

---

## 7. Contact

| Name         | Email                                                                             |
| ------------ | --------------------------------------------------------------------------------- |
| Maqbool Alam | [maqbul.alam.snaatak@mygurukulam.co](mailto:maqbul.alam.snaatak@mygurukulam.co) |

---

## 8. References

| **Topic** | **Description** |
| --------- | ---------------- |
| [Flyway Documentation](https://flywaydb.org/documentation/) | Official documentation for Flyway. |
| [Liquibase Documentation](https://docs.liquibase.com/) | Official documentation for Liquibase. |
| [Sqitch Documentation](https://sqitch.org/docs/) | Official documentation for Sqitch. |
| [Alembic Documentation](https://alembic.sqlalchemy.org/) | Official documentation for Alembic. |
| [Atlas Documentation](https://atlasgo.io/docs) | Official documentation for Atlas (schema-as-code). |
| [Django Migrations Documentation](https://docs.djangoproject.com/en/stable/topics/migrations/) | Official documentation for Django's built-in migration framework. |
| [Rails Active Record Migrations Guide](https://guides.rubyonrails.org/active_record_migrations.html) | Official Rails guide on Active Record migrations. |
