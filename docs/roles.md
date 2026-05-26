# Roles

The CO2 Calculator has two distinct areas, each with its own set of roles:

- **CO2 Calculator** — where units enter their data. Governed by **Standard User** and **Principal User** roles.
- **Back-office** — the administration interface. Governed by **Backoffice Administrator** and **Super Admin** roles.

A person can hold multiple roles simultaneously. For example, a lab manager might be a Principal User for their unit and also a Backoffice Administrator.

---

## Overview

| Role identifier            | Display name                 | Area           | Purpose                                                                                         |
| -------------------------- | ---------------------------- | -------------- | ----------------------------------------------------------------------------------------------- |
| `calco2.user.standard`     | **Standard User**            | CO2 Calculator | Unit member with access to their own travel and cloud/AI module entries                         |
| `calco2.user.principal`    | **Principal User**           | CO2 Calculator | Unit manager with full access to all modules for their unit, and can assign Standard User roles |
| `calco2.backoffice.metier` | **Backoffice Administrator** | Back-office    | Day-to-day back-office operations: reporting, user management, documentation                    |
| `calco2.superadmin`        | **Super Admin**              | Back-office    | Full back-office access including sensitive configuration and pipeline controls                 |

---

## Module permissions (unit roles)

Module permissions are unit-scoped — a user only holds them for the unit(s) they are assigned to.

### Standard User

| Module                          | view | edit (self) | edit (unit) | sync |
| ------------------------------- | ---- | ----------- | ----------- | ---- |
| `modules.headcount`             | ❌   | ❌          | ❌          | ❌   |
| `modules.equipment`             | ❌   | ❌          | ❌          | ❌   |
| `modules.professional_travel`   | ✅   | ✅          | ❌          | ❌   |
| `modules.buildings`             | ❌   | ❌          | ❌          | ❌   |
| `modules.purchase`              | ❌   | ❌          | ❌          | ❌   |
| `modules.research_facilities`   | ❌   | ❌          | ❌          | ❌   |
| `modules.external_cloud_and_ai` | ✅   | ✅          | ❌          | ❌   |
| `modules.process_emissions`     | ❌   | ❌          | ❌          | ❌   |

### Principal User

| Module                          | view | edit (self) | edit (unit) | sync |
| ------------------------------- | ---- | ----------- | ----------- | ---- |
| `modules.headcount`             | ✅   | ✅          | ✅          | ✅   |
| `modules.equipment`             | ✅   | ✅          | ✅          | ✅   |
| `modules.professional_travel`   | ✅   | ✅          | ✅          | ✅   |
| `modules.buildings`             | ✅   | ✅          | ✅          | ✅   |
| `modules.purchase`              | ✅   | ✅          | ✅          | ✅   |
| `modules.research_facilities`   | ✅   | ✅          | ✅          | ✅   |
| `modules.external_cloud_and_ai` | ✅   | ✅          | ✅          | ✅   |
| `modules.process_emissions`     | ✅   | ✅          | ✅          | ✅   |

---

## Back-office tab access

Standard User and Principal User have no access to the back-office. Super Admin has full back-office access but no access to unit module data. The table below covers back-office roles only.

| Tab                   | Backoffice Administrator | Super Admin |
| --------------------- | ------------------------ | ----------- |
| Reporting             | ✅                       | ✅          |
| User Management       | ✅                       | ✅          |
| Documentation Editing | ✅                       | ✅          |
| UI Texts Editing      | ✅                       | ✅          |
| Configuration         | ❌                       | ✅          |
| Pipeline Operations   | ❌                       | ✅          |
| Logs                  | ❌                       | ✅          |

---

## Back-office permission reference

| Permission                   | Action | Backoffice Administrator | Super Admin | Tab                                                |
| ---------------------------- | ------ | ------------------------ | ----------- | -------------------------------------------------- |
| `backoffice.users`           | view   | ✅                       | ✅          | Reporting, Documentation Editing, UI Texts Editing |
| `backoffice.users`           | edit   | ✅                       | ✅          | User Management                                    |
| `backoffice.users`           | export | ✅                       | ✅          | Reporting (CSV exports)                            |
| `backoffice.data_management` | view   | ✅                       | ✅          | Configuration (read-only API calls)                |
| `backoffice.data_management` | export | ✅                       | ✅          | Configuration (CSV downloads)                      |
| `backoffice.data_management` | edit   | ❌                       | ✅          | Configuration, Pipeline Operations                 |
| `backoffice.data_management` | sync   | ❌                       | ✅          | Pipeline Operations                                |
