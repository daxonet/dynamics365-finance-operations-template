Dynamics 365 Finance Operations Implementation Project
============

[[_TOC_]]

## Get started

> Follow the [Get Started with Template](Docs/Get-Started-with-Template.md) instructions to create new implementation project from template on Customer DevOps.

See the [Get Started](Docs/Get-Started.md), development guide before start development.

## Project Overview
- **Project Name**: *[Insert Project Name]*
- **Project Scope**: Finance / SCM / Retail / Others

### Project key milestones

::: mermaid
gantt
  tickInterval 2month
  dateFormat YYYY-MM-DD
  excludes    weekends
        Analysis Phase    :2025-02-01,2025-04-30
        Design Phase      :2025-05-01,2025-07-31
        Development Phase :2025-08-01,2025-11-30
        Deployment Phase  :2025-12-01,2025-12-31
        Operation Phase   :2026-01-01,2026-02-15
:::

### Project Teams Member

- **Project Manager:** [email@example.com]
- **Solution Architect:** [email@example.com]
- **Finance Consultant:** [email@example.com]
- **SCM Consultant:** [email@example.com]
- **Technical Lead:** [email@example.com]


## Environment Access

- **LCS:** https://lcs.dynamics.com/V2/ProjectOverview/00000

- **PPAC:** https://admin.powerplatform.microsoft.com/

| Environment | Purpose       | Access Info                 |
|-------------|---------------|-----------------------------|
| DEV         | Development   | [Access guide or link]      |
| UAT         | Testing       | [Access guide or link]      |
| SIT         | Integration   | [Access guide or link]      |
| PROD        | Live System   | Restricted                  |

## Folder structure

| Folder                 | Description
| ---------------------- | ----------------------------------------------
| Build                  | Script and template files for pipeline.
| Docs                   | Technical document in markdown format.
| Licenses               | Independent software vendor (ISV) licensing file(s).
| Metadata               | Metadata for build packages.
| Packages               | Microsoft provided packages.
| PackagesLocalDirectory | Customization working matadata folder.
| Projects               | Customization working project forlder.
| Tests                  | Testing scripts.
| Vendors                | Third party submodule.
