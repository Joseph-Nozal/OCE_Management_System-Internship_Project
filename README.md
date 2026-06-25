# OCE Engagement Management System

> A centralized, low-code campaign management platform built on Microsoft Power Platform, developed during a 4-month cooperative internship at **DKSH (Thailand) Limited**.

![Status](https://img.shields.io/badge/status-pilot--ready-yellow)
![Platform](https://img.shields.io/badge/platform-Power%20Apps%20%7C%20SharePoint%20%7C%20Power%20BI-blue)
![Internship](https://img.shields.io/badge/internship-Dec%202025%20--%20Mar%202026-lightgrey)

> **Note:** This application was built for internal use at DKSH (Thailand) Limited and runs on the company's Microsoft 365 tenant. As such, it cannot be cloned, deployed, or linked directly from this repository. This README — along with the screenshots and diagrams below — documents the system's design, functionality, and outcomes for portfolio purposes. Some details (e.g. exact field names, sample data) have been generalized to respect company confidentiality.

---

## 📋 Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [The Solution](#the-solution)
- [Key Feature: The LinkGroupUID Mechanism](#key-feature-the-linkgroupuid-mechanism)
- [Tech Stack](#tech-stack)
- [System Architecture](#system-architecture)
- [Screens & Screenshots](#screens--screenshots)
- [Data Model](#data-model)
- [Dashboards](#dashboards)
- [Results & Impact](#results--impact)
- [Limitations](#limitations)
- [What I'd Improve Next](#what-id-improve-next)
- [What I Learned](#what-i-learned)
- [About the Internship](#about-the-internship)

---

## Overview

The **OCE Engagement Management System** is a unified campaign tracking platform built for the **Opti-Channel Engagement (OCE)** team within DKSH's Commercial Excellence (ComEx) / Digital Transformation and Customer Experience (DCX) division.

It replaces a patchwork of Excel files and disconnected SharePoint lists with a single Power Apps interface, a normalized SharePoint backend, and a suite of Power BI dashboards — giving the team one place to create, track, link, and analyze marketing campaigns across email and webinar channels.

## The Problem

Before this project, the OCE team managed three campaign types — **MTE** (Mass Triggered Email), **RTE** (Rep Triggered Email), and **ON24** (webinar operations) — across fragmented Excel spreadsheets and disconnected SharePoint lists.

This created a few recurring pain points:

- **No standardized validation** → inconsistent, error-prone data entry
- **No relational visibility** → no way to tell if an email campaign and a webinar were part of the same initiative
- **No real-time reporting** → leadership needed hours of manual consolidation just to see overall team performance

The core issue wasn't just *messy data* — it was that related activities across channels were invisible to each other, making it nearly impossible to track a full customer journey or measure multi-channel ROI.

## The Solution

A three-tier system built entirely on tools DKSH already had licensed:

| Layer | Tool | Role |
|---|---|---|
| **Frontend** | Power Apps (Canvas App) | Unified interface for creating, viewing, and linking campaigns |
| **Backend** | SharePoint Online | Relational data store (5 interconnected lists) |
| **Analytics** | Power BI | Real-time dashboards across all channels |

The result is a single application where the OCE team can log a campaign once, link it to related activities across channels, and immediately see it reflected in live dashboards — no manual spreadsheet reconciliation required.

## Key Feature: The LinkGroupUID Mechanism

The core technical innovation of this project is **LinkGroupUID** — a shared identifier that connects campaigns across different channels into one trackable initiative.

**How it works:**
1. A "parent" event (e.g., an ON24 webinar) generates a unique `LinkGroupUID`.
2. Any "child" campaign created to support it (e.g., an MTE email promoting that webinar) inherits the same `LinkGroupUID`.
3. Both campaigns then appear in each other's "Linked Channels" view — giving a complete picture of the customer journey across touchpoints.

**Example scenarios tested:**

- **MTE → ON24:** An email campaign and the webinar it promotes are linked and visible to each other.
- **MTE → RTE → ON24:** A full multi-touch campaign (email + rep outreach + webinar) is tracked as one initiative via a shared `LinkGroupUID`.

This solved a problem manual tracking simply couldn't: before this system, there was no systematic way to connect a campaign across channels at all.

## Tech Stack

- **Microsoft Power Apps** (Canvas App) — front-end, built with **Power Fx**
- **SharePoint Online** — backend data store (5 SharePoint Lists)
- **Microsoft Power BI** (Desktop + Service) — analytics, built with **DAX**
- **Microsoft Excel** — historical ON24 tracker data, imported into Power BI
- **AI-assisted development** (Claude & ChatGPT) — used as a debugging/sounding-board tool for Power Fx and DAX

**Why this stack:** DKSH already held the relevant Microsoft 365 licenses, the tools integrate natively with each other, the low-code approach allowed for a complex feature set (like cross-channel linking) within a fixed 17-week timeline, and the in-house IT team could support and maintain it after handover.

## System Architecture

A three-tier architecture: Power Apps as the UI layer, SharePoint as the relational database layer, and Power BI as the analytics/intelligence layer — all communicating natively within the Power Platform ecosystem.

*(See `architecture-diagram.png` in `/screenshots` for the full system architecture and entity-relationship diagrams.)*

## Screens & Screenshots

The application consists of **7 screens**:

| Screen | Purpose |
|---|---|
| `scrLogin` | Authenticates users and detects role (User vs. Admin) |
| `scrDirection` | Main navigation hub — the central switchboard for all functions |
| `scrOverview` | Unified view of all MTE, RTE, and ON24 campaigns with filters |
| `scrCreateEvent` | Dual-mode campaign creation form (MTE/RTE and ON24) |
| `scrDetails` | Drill-down view of a single campaign, its linked channels, and actions |
| `scrAddEngagementChannel` | Interface for linking a new campaign to an existing parent event |
| `scrActions` | Modal for managing follow-up actions tied to a campaign |

> 📸 *Add your screenshots here, e.g.:*
> ```markdown
> ### Login & Navigation
> ![Login Screen](./screenshots/scrLogin.png)
> ![Navigation Hub](./screenshots/scrDirection.png)
>
> ### Campaign Management
> ![Campaign Overview](./screenshots/scrOverview.png)
> ![Campaign Creation](./screenshots/scrCreateEvent.png)
> ![Campaign Details](./screenshots/scrDetails.png)
>
> ### Cross-Channel Linking
> ![Linking Interface](./screenshots/scrAddEngagementChannel.png)
> ```

## Data Model

Five interconnected SharePoint Lists form the backend:

| List | Purpose |
|---|---|
| **employee** | User records for authentication and role-based access (RBAC) |
| **MteRteList** | Stores MTE and RTE campaign data |
| **ON24** | Stores webinar request and event data |
| **MteRteActions** | Follow-up actions linked to MTE/RTE campaigns |
| **TargetSpecialtiesMaster** | Reference/lookup list for campaign targeting options |

**Key relationships:**
- `employee` → `MteRteList` / `ON24` (one-to-many, by owner)
- `MteRteList` → `MteRteActions` (one-to-many, by campaign)
- `MteRteList` ↔ `ON24` (many-to-many, via shared `LinkGroupUID`)

> 📸 *Add your Entity-Relationship Diagram here:*
> ```markdown
> ![ER Diagram](./screenshots/er-diagram.png)
> ```

## Dashboards

Five Power BI dashboards turn raw SharePoint data into real-time, cross-channel insight:

1. **MTE Report** — campaign volume, status breakdown, brand distribution, monthly trends
2. **RTE Report** — same structure as the MTE report, scoped to RTE campaigns
3. **ON24 Report** — request types, market group analysis, support breakdown
4. **Summary Report** — unified cross-channel overview of all campaigns and monthly activity
5. **ON24 Tracker** — historical webinar performance (registration vs. attendance, live vs. on-demand)

A unifying DAX calculated table, `AllCampaignsDetail`, merges MTE, RTE, and ON24 data into one queryable structure — enabling the cross-channel metrics that were previously impossible to generate manually.

> 📸 *Add dashboard screenshots here:*
> ```markdown
> ![MTE Dashboard](./screenshots/dashboard-mte.png)
> ![Summary Dashboard](./screenshots/dashboard-summary.png)
> ```

## Results & Impact

- ✅ All **5 technical objectives** of the project achieved (centralization, cross-channel linking, analytics, data quality, RBAC)
- ✅ System tested with **42 campaigns** across all three channels during development
- ✅ **100%** of test campaigns carry a valid `EventUID` and `LinkGroupUID`
- ✅ **0** duplicate ON24 events and **0** orphaned follow-up actions across testing
- ✅ Reduced what previously required hours of manual Excel/SharePoint consolidation to a live, filterable dashboard view
- ✅ Formal handover completed, including a User Guide, an Admin Guide, and a live walkthrough with the team's mentor/admin

## Limitations

As with any pilot-stage system, a few things were consciously scoped out or constrained by company policy:

- **No edit/delete in-app** — corrections currently require direct SharePoint access
- **Manual Power BI refresh** — dashboards update on-demand, not automatically (no scheduled refresh configured yet)
- **No embedded dashboards** — company security policy currently blocks embedding Power BI directly inside Power Apps, so dashboards are accessed via link
- **No Row-Level Security in Power BI** — access control is enforced at the Power Apps layer, but all users with dashboard access currently see all data
- **No automated approval workflows or notifications** yet

## What I'd Improve Next

- Develop Power BI dashboards in parallel with Power Apps, rather than sequentially
- Get real users testing earlier in the process, rather than relying mainly on mentor feedback
- Document features as they're built, instead of reconstructing decisions from memory afterward
- Set a clearer line up front between "internship deliverables" and "post-internship deployment scope"

## What I Learned

This was my first time working with Power Apps, SharePoint, or Power BI — I came in with general programming knowledge but no Power Platform experience.

A few of the bigger lessons:

- **Power Fx looks like Excel, but state management across screens is genuinely tricky.** The trickiest bug of the project — a `LinkGroupUID` that kept saving blank — turned out to be a timing issue: the form was submitting *before* the variable was set. Fixing it meant learning to think carefully about execution order, not just formula syntax.
- **SharePoint's internal field names can silently differ from what you see on screen**, which broke a formula until I learned to check the actual schema rather than the UI.
- **DAX requires a different way of thinking about data** — especially the distinction between calculated columns (row context) and measures (filter context) — and building a single calculated table to unify three separate campaign sources took several attempts to get right.
- **Clear, specific communication mattered as much as the code.** Vague debugging questions (to AI tools or to people) got vague answers; specific ones got specific, useful ones. The same was true explaining limitations honestly to my supervisor instead of glossing over them.
- **A working system isn't the same as a *used* system.** The technical build is done, but adoption, training, and change management are a separate challenge — and arguably the harder one.

## About the Internship

| | |
|---|---|
| **Company** | DKSH (Thailand) Limited |
| **Team** | Commercial Excellence (ComEx) — Digital Transformation & Customer Experience (DCX) |
| **Duration** | December 1, 2025 – March 31, 2026 (17 weeks) |
| **Role** | Intern — Digital Transformation & Customer Experience |
| **Methodology** | Agile, weekly sprint-style review cycles with mentors |
| **Author** | Joseph Miles Nozal, B.Eng. Digital Engineering, Thai-Nichi Institute of Technology |

---

*This project was completed as part of a cooperative education internship program and submitted as a final internship report to Thai-Nichi Institute of Technology. Company data shown in any included screenshots is test/sample data created during development, not live production data.*
