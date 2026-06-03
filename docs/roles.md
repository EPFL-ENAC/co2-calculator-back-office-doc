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

## Permission matrix

Which actions each role grants on each permission. One permission per
back-office page; module permissions are held only for the unit(s) a user is
assigned to.

**Actions:** `V` = view · `E` = edit · `X` = export · `S` = sync · `–` = no
grant.
**Scope** (where data is restricted): `(U)` = unit · `(O)` = own (self) ·
`(A)` = affiliation / ACCRED sub-perimeter. Back-office grants with no tag are
global.

| Permission                       | Super Admin | Backoffice Administrator | Principal User | Standard User |
| -------------------------------- | ----------- | ------------------------ | -------------- | ------------- |
| `backoffice.reporting`           | V, X        | V, X (A)                 | –              | –             |
| `backoffice.users`               | V, E, X     | V, E, X                  | –              | –             |
| `backoffice.documentation`       | V, E        | V, E                     | –              | –             |
| `backoffice.ui_texts`            | V, E        | V, E                     | –              | –             |
| `backoffice.configuration`       | V, E        | –                        | –              | –             |
| `backoffice.pipeline_operations` | V, E        | –                        | –              | –             |
| `backoffice.logs`                | V           | –                        | –              | –             |
| `modules.headcount`              | –           | –                        | V, E, S (U)    | –             |
| `modules.equipment`              | –           | –                        | V, E, S (U)    | –             |
| `modules.professional_travel`    | –           | –                        | V, E, S (U)    | V, E (O)      |
| `modules.buildings`              | –           | –                        | V, E, S (U)    | –             |
| `modules.purchase`               | –           | –                        | V, E, S (U)    | –             |
| `modules.research_facilities`    | –           | –                        | V, E, S (U)    | –             |
| `modules.external_cloud_and_ai`  | –           | –                        | V, E, S (U)    | V, E (O)      |
| `modules.process_emissions`      | –           | –                        | V, E, S (U)    | –             |

Super Admin has full back-office access but no unit module data. Standard User
and Principal User have no back-office access. A Backoffice Administrator's
reporting is restricted to their own ACCRED sub-perimeter `(A)`; a Super Admin
sees everything.
