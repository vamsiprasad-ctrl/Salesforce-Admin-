<div align="center">

# ⚡ Salesforce Admin Topics Guide.
### Complete Trailhead Learning Resource Guide

[![Salesforce](https://img.shields.io/badge/Salesforce-Admin%20%26%20Consultant-00A1E0?style=for-the-badge&logo=salesforce&logoColor=white)](https://trailhead.salesforce.com)
[![Trailhead](https://img.shields.io/badge/Trailhead-Learning%20Path-00D2FF?style=for-the-badge&logo=salesforce&logoColor=white)](https://trailhead.salesforce.com)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)](.)
[![Resources](https://img.shields.io/badge/Resources-51%20Links-orange?style=for-the-badge)](.)

<br/>

*A comprehensive, topic-mapped collection of Trailhead modules, hands-on projects, and curated trails — structured to follow the full Salesforce Admin & Consultant training curriculum from Day 1 through certification readiness.*

</div>

---

## 📋 Table of Contents

| # | Module | Resources | Est. Time |
|---|--------|-----------|-----------|
| 01 | [Getting Started](#-01--getting-started) | 3 links | ~2.5 hrs |
| 02 | [Salesforce Licenses & User Management](#-02--salesforce-licenses--user-management) | 2 links | ~1.5 hrs |
| 03 | [Data Modeling & Objects](#-03--data-modeling--objects) | 5 links | ~6 hrs |
| 04 | [Security & Access Control](#-04--security--access-control) | 4 links | ~4 hrs |
| 05 | [Customization & UI](#-05--customization--ui) | 5 links | ~3 hrs |
| 06 | [Business Logic & Automation](#-06--business-logic--automation) | 6 links | ~10 hrs |
| 07 | [Reports & Dashboards](#-07--reports--dashboards) | 5 links | ~4 hrs |
| 08 | [Data Management](#-08--data-management) | 5 links | ~4 hrs |
| 09 | [Sales Cloud](#-09--sales-cloud) | 4 links | ~8 hrs |
| 10 | [Service Cloud & Knowledge](#-10--service-cloud--knowledge) | 6 links | ~7 hrs |
| 11 | [Experience Cloud](#-11--experience-cloud-communities) | 6 links | ~5 hrs |

> **Total curriculum:** 51 curated resources · ~55 hours of structured learning

---

## 🔖 How to Use This Guide

This guide is designed to be followed **sequentially**. Each section maps directly to a classroom module and includes:

- **Trailhead modules** — concept-first learning with quizzes
- **Hands-on projects** — step-by-step builds inside a real Salesforce org
- **Trails** — curated multi-module learning paths on a broad topic
- **External references** — supplementary deep-dives from the Salesforce community

**Before you begin**, make sure you have:

1. A free [Trailhead account](https://trailhead.salesforce.com/signup)
2. A Trailhead Playground org (provisioned through Trailhead — see Section 01)
3. Bookmarked the [Trailblazer Community](https://trailhead.salesforce.com/trailblazer-community) for peer support

> ⚠️ **Never practice in a production org.** All hands-on work must be done in a Trailhead Playground or Developer org.

---

## 🚀 01 · Getting Started

> **Goal:** Understand what Salesforce is, how its cloud architecture works, and set up your personal learning environment before any hands-on work begins.

Salesforce is the world's #1 CRM platform, built on a **multitenant cloud architecture** — meaning thousands of customers share the same infrastructure while their data remains completely isolated. Before building anything, you need to understand the landscape: product clouds, org types, license categories, and the difference between production orgs and sandboxes.

Your learning environment for this entire program is a **Trailhead Playground** — a free, fully functional Salesforce org provisioned directly through Trailhead. You can reset it anytime without affecting real data.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Salesforce Free Trials | 🌐 External | Beginner | [→ Open](https://developer.salesforce.com/free-trials) |
| Trailhead Playground Management | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/trailhead_playground_management) |
| Company-Wide Org Settings | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/company_wide_org_settings) |

### 🎯 Key Concepts

- **Salesforce as a CRM** — what CRM means, why Salesforce leads the market, and how it fits into a business
- **Products & Editions** — Sales Cloud, Service Cloud, Experience Cloud, and the different tiers (Essentials → Unlimited)
- **Multitenant Architecture** — one platform, isolated data, continuous upgrades
- **Environments** — Production, Full Sandbox, Partial Sandbox, Developer Sandbox, Developer Edition, Trailhead Playground
- **Org Setup** — Company Information, Financial Information, Support Information, Fiscal Year
- **Trailhead** — modules, projects, trails, superbadges, and how badges are earned

### 💡 Pro Tips

- Spawn your Trailhead Playground now and keep its login details handy — you will use it every day
- Salesforce releases **three major updates per year** (Spring, Summer, Winter) — stay current via [Salesforce Release Notes](https://help.salesforce.com/s/articleView?id=release-notes.salesforce_release_notes.htm)
- The difference between a **Developer Edition** and a **Trailhead Playground** is subtle — both are free, but Playgrounds are pre-configured for Trailhead challenges

---

## 👥 02 · Salesforce Licenses & User Management

> **Goal:** Learn how Salesforce licensing works, create and manage users, and configure login and session policies to control how people access the org.

Licenses are the foundation of Salesforce access — every user must have a license, and different license types unlock different features. Understanding the licensing model is critical for administrators who manage org costs and user provisioning.

User management goes beyond just creating accounts: you control **when** users can log in, **how** sessions behave, and whether admins can log in as other users for troubleshooting.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Salesforce Licensing | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/salesforce-licensing) |
| User Setup & Management | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/lex_implementation_user_setup_mgmt) |

### 🎯 Key Concepts

- **License types** — Salesforce, Platform, Chatter, Community, Identity licenses and when to use each
- **User creation** — required fields, usernames (must be globally unique email format), roles, profiles
- **Login Access Policies** — granting admins the ability to log in as any user for debugging
- **Session Settings** — session timeout, retaining admin session after logging out as another user
- **Freezing vs Deactivating** users — the difference and when each applies
- **Single Sign-On (SSO)** concepts — understanding federated authentication at a high level

### 💡 Pro Tips

- Salesforce usernames are **globally unique across all orgs** — `john@mycompany.com` can only belong to one user in the entire Salesforce ecosystem
- When troubleshooting user issues, use **Login As User** rather than asking users to share passwords
- Always deactivate rather than delete users — deletion is rarely possible once a user owns records

---

## 🗂️ 03 · Data Modeling & Objects

> **Goal:** Design and build the data structure of a Salesforce org using standard and custom objects, fields of all types, and the full range of relationship models.

Data modeling is the most important foundation skill for a Salesforce administrator. Everything in Salesforce — records, reports, automation, security — depends on a well-designed data model. Salesforce's data model is **object-oriented**: Objects are like database tables, Fields are columns, and Records are rows.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Customize a Salesforce Object | 🛠️ Project | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/projects/customize-a-salesforce-object) |
| Data Modeling | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/data_modeling) |
| Build a Data Model for a Travel Approval App | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/build-a-data-model-for-a-travel-approval-app) |
| Quick Start: Customize an App with Lightning Object Creator | 🛠️ Project | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/projects/quick-start-customize-an-app-with-lightning-object-creator) |
| Dive into Sales Cloud for Admins | 🛤️ Trail | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/trails/dive-into-sales-cloud-for-admins) |

### 🎯 Key Concepts

**Objects & Fields**
- Standard vs Custom Objects — `Account`, `Contact`, `Lead`, `Opportunity` vs your own `Invoice__c`
- Field types — Text, Number, Currency, Date, Picklist, Checkbox, Formula, Lookup, Roll-Up Summary
- **Dependent Picklists** — controlling which values appear based on a controlling field
- **Field History Tracking** — audit trail for up to 20 fields per object (who changed what and when)

**Relationships** *(highest-weighted topic — ~140 min of curriculum time)*

| Relationship Type | Key Characteristic |
|-------------------|-------------------|
| Lookup | Loose link — child can exist without parent |
| Master-Detail | Tight link — child inherits parent sharing; enables Roll-Up Summary; cascade deletes |
| Self Relationship | Object related to itself (e.g., Employee → Manager) |
| Hierarchical | User object only; used for org charts |
| Many-to-Many | Achieved via a Junction Object with two Master-Detail relationships |
| External / Indirect Lookup | Connect Salesforce objects to external data sources |

**Page Layouts & Record Types**
- Page Layouts — which fields appear on the detail/edit page, and in what order
- Record Types — different picklist values and page layouts for different business processes on the same object

**Other Key Topics**
- **Schema Builder** — visual canvas to view and create the full data model
- **Activity Management** — Tasks, Events, and the Activity Timeline
- **Custom Formula Fields** — calculated read-only fields using Salesforce formula syntax

### 💡 Pro Tips

- Use **Master-Detail** when the child record has no meaning without its parent (e.g., an Order Line Item without an Order)
- Use **Lookup** when both objects can exist independently
- You can have a maximum of **2 Master-Detail** relationships on a custom object
- **Roll-Up Summary fields** only work on the Master side of a Master-Detail relationship — a common exam question
- Schema Builder is great for visualizing an existing data model; Object Manager is preferred for building

---

## 🔐 04 · Security & Access Control

> **Goal:** Implement Salesforce's multi-layered security model to ensure users see exactly the data they are supposed to — no more, no less.

Salesforce security is built in layers. Think of it as a series of gates: the org gate, the object gate, the field gate, and finally the record gate. A user must pass through all gates to access any piece of data. The golden rule: **the most restrictive setting wins at each layer**.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Security Basics | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/security_basics) |
| Data Security | 📘 Module | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/modules/data_security) |
| Protect Your Data in Salesforce | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/protect-your-data-in-salesforce) |
| Permission Set Groups | 📘 Module | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/modules/permission-set-groups) |

### 🎯 Key Concepts

**The Four Security Layers**

```
Layer 1 — Org Access        Login hours · Trusted IPs · MFA
      ↓
Layer 2 — Object Access     Profiles · Permission Sets · Permission Set Groups
      ↓
Layer 3 — Field Access      Field-Level Security (FLS) per profile / permission set
      ↓
Layer 4 — Record Access     OWD → Role Hierarchy → Sharing Rules → Manual Sharing
```

**Record Access Deep Dive**

| Mechanism | Direction | Notes |
|-----------|-----------|-------|
| OWD (Org-Wide Defaults) | Sets the floor | Private / Public Read Only / Public Read/Write |
| Role Hierarchy | Opens up | Managers see records owned by reports |
| Sharing Rules | Opens up | Criteria-based or ownership-based sharing to groups |
| Manual Sharing | Opens up | Record owner shares individual records ad hoc |
| Restriction Rules | Locks down | Further restricts even if sharing would otherwise allow |

### 💡 Pro Tips

- OWD can **only be opened up** via Role Hierarchy, Sharing Rules, and Manual Sharing — it can never be overridden to be more restrictive per record
- If OWD is **Public Read/Write**, role hierarchy and sharing rules have no effect on record access
- **Permission Sets** are the modern best practice — Salesforce is phasing out permission-heavy profiles
- The `View All` and `Modify All` object permissions bypass record-level security entirely — assign with caution

---

## 🎨 05 · Customization & UI

> **Goal:** Configure and customize the Salesforce Lightning interface — apps, navigation, list views, and page layouts — to match the way your teams actually work.

A well-configured Salesforce UI dramatically improves user adoption. This section focuses on the tools available to admins (no code required) to tailor the experience: building apps, customizing what users see in list views, and using Lightning App Builder to design record pages.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Lightning Experience Customization | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/lex_customization) |
| Lightning App Builder | 📘 Module | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/modules/lightning_app_builder) |
| List View: Quick Check | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/list-view-quick-check) |
| Contacts List View: Step-by-Step | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/contacts-list-view-step-by-step) |
| Accounts List View: Step-by-Step | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/accounts-list-view-step-by-step) |

### 🎯 Key Concepts

- **Classic vs Lightning Experience** — understanding the migration, why Lightning is the default, and what changed
- **Apps** — sets of tabs, branding, and navigation grouped for a business function (Sales app, Service app)
- **App Manager** — Lightning apps vs Classic apps, utility bars, navigation style
- **List Views** — filtering, column selection, inline editing, Kanban view, sharing list views with teams
- **Lightning App Builder** — drag-and-drop page builder for Home pages, Record pages, and App pages
- **Dynamic Forms** — field visibility rules on record pages without page layouts (Lightning only)
- **Compact Layouts** — which fields appear in highlights panels and mobile cards

### 💡 Pro Tips

- List Views are per-object and per-user by default — share them with roles or groups to standardize team views
- Lightning App Builder lets you assign different record page layouts to different **profiles, apps, or record types** — surface only what each team needs
- Pinning a list view sets it as your personal default for that object — it does not affect other users

---

## ⚙️ 06 · Business Logic & Automation

> **Goal:** Build and compare Salesforce's no-code and low-code automation tools — from simple validation rules to complex multi-object flows — and understand when to use each one.

Automation is where admins have the most leverage. A single well-built flow can save hundreds of hours of manual work per year. This is the **largest section of the curriculum** (~10 hours), and for good reason: Salesforce Flow is the primary automation tool across the entire platform.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Point & Click Business Logic | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/point_click_business_logic) |
| Validation Rules | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/validation-rules) |
| Improve Data Quality for a Cleaning Supply App | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/improve-data-quality-for-a-cleaning-supply-app) |
| Business Process Automation | 📘 Module | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/modules/business_process_automation) |
| Build a Discount Approval Process | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/build-a-discount-approval-process) |
| Build Flows with Flow Builder | 🛤️ Trail | Advanced | [→ Open](https://trailhead.salesforce.com/content/learn/trails/build-flows-with-flow-builder) |

### 🎯 Key Concepts

**Flow Types**

| Flow Type | Trigger | Common Use Case |
|-----------|---------|-----------------|
| Record-Triggered Flow | Record create / update / delete | Auto-update related records, send emails on field change |
| Screen Flow | User-initiated | Guided data entry wizards |
| Scheduled Flow | Date / time based | Nightly data updates, reminders |
| Platform Event Flow | Platform event message | Event-driven integrations |
| Auto-launched Flow | Code, process, or another flow | Reusable sub-flows |

**Automation Tool Comparison**

| Tool | Use Case | Status |
|------|----------|--------|
| Validation Rules | Block bad data on save | ✅ Active — primary tool |
| Flow Builder | Complex multi-step logic | ✅ Active — primary tool |
| Approval Process | Human-in-the-loop approvals | ✅ Active — use for approvals |
| Workflow Rules | Simple field updates, email alerts | ❌ Retired — migrate to Flow |
| Process Builder | Point-and-click automation | ❌ Retired — migrate to Flow |

**Approval Processes**
- Multi-step human approval workflows (e.g., a discount over 20% requires manager sign-off)
- Define initial submission actions, approval steps, approval/rejection actions, and recall actions
- Route to assigned approvers, a queue, a related user field, or Apex

### 💡 Pro Tips

- **Flow is the future** — Salesforce has retired Workflow Rules and Process Builder; all new automation should be built in Flow Builder
- Use **Before-Save flows** for field updates (faster, no DML limits) and **After-Save flows** when you need to create or update other records
- Always add **Fault Paths** to your flows — unhandled faults surface confusing error messages to end users
- Subflows allow you to build reusable logic components — build once, call from anywhere
- Lookup Filters can often replace validation rules for relationship-field restrictions, keeping rules simpler

---

## 📊 07 · Reports & Dashboards

> **Goal:** Build reports and dashboards that surface the right data to the right people — enabling data-driven decisions across sales, service, and management teams.

Reports and dashboards are how Salesforce data becomes business insight. Salesforce has four report formats, powerful filtering and grouping, formula columns, and dynamic dashboards that show different data depending on who is viewing them.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Reports & Dashboards Trail | 🛤️ Trail | Beginner–Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/trails/explore-lightning-experience-reports-dashboards) |
| Quickstart Reports | 🛠️ Project | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/projects/quickstart-reports) |
| Reports & Dashboards Quick Look | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/reports-dashboards-quick-look) |
| Embed Reports & Dashboards | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/rd-embed-reports-dashboards) |
| Report Summary Formulas | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/rd-summary-formulas) |

### 🎯 Key Concepts

**Report Formats**

| Format | Structure | Best For |
|--------|-----------|----------|
| Tabular | Simple rows and columns | Export lists, row counts |
| Summary | Grouped rows with subtotals | Sales by region, pipeline by stage |
| Matrix | Grouped by rows AND columns | Quarterly comparisons across categories |
| Joined | Multiple report blocks side by side | Multi-object analysis in one report |

**Key Features**
- **Custom Report Types** — define which objects and fields are available in a report category
- **Bucket Fields** — group field values on the fly without changing underlying data (e.g., group deal sizes into Small / Medium / Large)
- **Row-Level Formulas** — calculated columns evaluated per row
- **Summary Formulas** — calculated values at the group or grand total level
- **Report Subscriptions** — schedule a report to run and email results; can trigger on conditional thresholds
- **Dynamic Dashboards** — each viewer sees data filtered to their own record access level

### 💡 Pro Tips

- You cannot create a dashboard from a Tabular report — use Summary or Matrix
- **Dynamic Dashboards** use the viewer's own access level; regular dashboards run as a specified "run as" user
- Embedded dashboards on Lightning Record Pages provide at-a-glance analytics in context
- Report subscriptions can trigger only **when a condition is met** — e.g., email me only if open case count exceeds 100

---

## 💾 08 · Data Management

> **Goal:** Confidently import, update, export, and maintain Salesforce data using the platform's native tools — and understand the critical concept of External IDs for integration-safe data operations.

Data quality and integrity underpin every other Salesforce capability. This section covers the full data lifecycle: getting data in, keeping it clean, and getting it out.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Data Management (Lightning Experience) | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/lex_implementation_data_management) |
| Data Management Fundamentals | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/data-management-fundamentals) |
| Salesforce Data Loader Guide | 🌐 External | Intermediate | [→ Open](https://www.apexhours.com/salesforce-data-loader/) |
| Import & Export with Data Management Tools | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/import-and-export-with-data-management-tools) |
| Data Quality | 📘 Module | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/modules/data_quality) |

### 🎯 Key Concepts

**Tool Comparison**

| Tool | Max Records | Use Case | Supports Upsert |
|------|-------------|----------|-----------------|
| Data Import Wizard | 50,000 | Standard objects, simple CSVs | No |
| Data Loader | 5,000,000+ | Any object, bulk operations, automation | Yes |
| Workbench | Unlimited | Developer / admin ad hoc queries via browser | Yes |
| Data Export | All records | Full org backup (weekly or monthly) | N/A |

**Key Concepts**
- **External ID** — a custom field marked as External ID enables upsert operations and importing related records without knowing Salesforce record IDs
- **Upsert** — insert new records and update existing ones in a single operation, matched by External ID or Salesforce ID
- **Importing Related Records** — use a parent record's External ID or Row ID to link parent-child on import
- **Data Loader operations** — Insert, Update, Upsert, Delete, Hard Delete, Export, Export All

### 💡 Pro Tips

- Always do a **test import with 5–10 records** before running a full load — check field mapping and error logs carefully
- Use **Upsert with External ID** for recurring data loads from external systems — it is idempotent and safe to re-run
- Back up your org's data **before** any bulk delete or update — the Recycle Bin only holds deleted records for 15 days
- **Hard Delete** in Data Loader bypasses the Recycle Bin — use with extreme caution and always keep a backup CSV

---

## ☁️ 09 · Sales Cloud

> **Goal:** Configure and use Salesforce's core CRM objects — from first lead touch through closed deal — and understand the supporting structures that help sales teams collaborate and close more effectively.

Sales Cloud is the heart of Salesforce's CRM offering. This section follows the full sales lifecycle and introduces the objects and features that support pipeline management, revenue tracking, campaign attribution, and multi-currency enterprise scenarios.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Accounts & Contacts in Lightning Experience | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/accounts_contacts_lightning_experience) |
| Leads & Opportunities in Lightning Experience | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/leads_opportunities_lightning_experience) |
| Lead Record: Step-by-Step | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/lead-record-step-by-step) |
| Leads List View: Step-by-Step | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/leads-list-view-step-by-step) |

### 🎯 Key Concepts

**The Sales Lifecycle**

```
Lead captured (Web-to-Lead / manual entry)
      ↓
Lead qualified and converted
      ↓
Account + Contact + Opportunity created
      ↓
Opportunity progresses through stages (Prospecting → Needs Analysis → Proposal → Negotiation)
      ↓
Closed Won — Products and Price Book applied → Opportunity Line Items logged
```

**Core Objects**

| Object | Purpose |
|--------|---------|
| Lead | Unqualified prospect — not yet a verified business relationship |
| Account | A company or individual you do business with |
| Contact | A person associated with an Account |
| Opportunity | A potential revenue-generating deal in progress |
| Product | Items or services your company sells |
| Price Book | A catalog of Products with associated prices |
| Opportunity Line Item | A specific Product added to an Opportunity |
| Campaign | A marketing initiative (email, event, webinar, paid ad) |
| Campaign Member | A Lead or Contact associated with a Campaign |

**Additional Features**
- **Web-to-Lead** — capture leads from a website form directly into Salesforce
- **Lead Conversion** — converts a Lead into an Account, Contact, and optionally an Opportunity; preserves full activity history
- **Account Teams / Opportunity Teams** — multiple reps collaborating on one record with defined access roles
- **Campaign Influence** — attribute closed revenue back to the marketing campaigns that touched the deal
- **Duplicate Management** — Matching Rules + Duplicate Rules to detect, alert on, or block duplicate Leads, Contacts, and Accounts
- **Multi-Currency** — support multiple currencies with dated exchange rates for historical accuracy
- **Multi-Language** — configure user-level language and locale settings across the org

### 💡 Pro Tips

- Once a Lead is **converted, it is locked** — you cannot unconvert it. Establish clear qualification criteria before teams convert at scale
- A Contact can exist **without** an Account (private contacts) — useful for personal relationships outside a company context
- **Campaign Influence** requires Opportunities to be linked to Campaigns via the Primary Campaign Source field — set this up in your sales process from day one
- Multi-Currency, once enabled, **cannot be disabled** — always enable in a sandbox first and verify all formula fields and reports behave correctly

---

## 🎧 10 · Service Cloud & Knowledge

> **Goal:** Configure a complete customer support operation in Salesforce — from case creation through resolution — and build a searchable Knowledge Base that helps agents resolve issues faster and customers find answers independently.

Service Cloud transforms Salesforce into a full-featured customer service platform. The Case object is the central unit of work: it tracks a customer issue from open to close. Knowledge Articles give agents and customers instant access to solutions, reducing average handle time and increasing first-contact resolution rates.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Email-to-Case in Salesforce | 🌐 External | Intermediate | [→ Open](https://www.apexhours.com/email-to-case-in-salesforce/) |
| Set Up Case Escalation & Entitlements | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/set-up-case-escalation-entitlements) |
| Lightning Knowledge Basics | 📘 Module | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/modules/lightning-knowledge-basics) |
| Macros Quick Look for Agents | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/macros-quick-look-for-agents) |
| Knowledge Search Basics | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/knowledge_search_basics) |
| Set Up Salesforce Knowledge | 🛠️ Project | Advanced | [→ Open](https://trailhead.salesforce.com/content/learn/projects/set-up-salesforce-knowledge) |

### 🎯 Key Concepts

**Case Management**

| Feature | What It Does |
|---------|-------------|
| Support Process | Defines the stages a Case moves through (New → Working → Escalated → Closed) |
| Case Queues | Route new cases to a pool of agents rather than a named individual |
| Case Teams | Multiple agents collaborating on a complex case with defined roles |
| Assignment Rules | Automatically route new cases to the right queue or agent based on criteria |
| Escalation Rules | Automatically escalate cases that breach defined SLA time thresholds |
| Web-to-Case | Capture customer issues from a web form directly into a Case record |
| Email-to-Case | Convert inbound support emails into Cases automatically; preserves the full thread |
| Entitlements | Define the level of support a customer is contractually entitled to receive |

**Salesforce Knowledge**

| Component | Description |
|-----------|-------------|
| Knowledge Base | A library of Articles — solutions, FAQs, how-tos, troubleshooting guides |
| Record Types | Group article types with their own fields and page layouts |
| Data Categories | Hierarchical taxonomy for filtering and controlling article visibility |
| Article Visibility | Control which articles are visible to internal agents, customers, or partners |
| Auto-Suggestion | Surfaces relevant Knowledge Articles in the Case feed as the agent types the subject line |
| Macros | One-click multi-step agent actions (e.g., send standard reply + update status + close case) |

**Lightning Service Console**
- A specialized app layout optimized for high-volume support agents: split view, multi-tab navigation, utility bar with quick access to tools
- Agents can work multiple cases simultaneously without losing context

### 💡 Pro Tips

- **Email-to-Case** has two modes — standard (Salesforce polls a mailbox) and On-Demand (your mail server pushes to Salesforce); On-Demand is more reliable for high email volumes
- Set up **Entitlement Processes before Escalation Rules** — they work together to give you full SLA tracking with milestone-based notifications
- **Data Categories** are shared across Knowledge and Experience Cloud communities — plan the taxonomy carefully before creating it, as restructuring later is disruptive
- Macros dramatically reduce agent handle time — identify your top 10 most common responses and turn them into macros on launch day

---

## 🌐 11 · Experience Cloud (Communities)

> **Goal:** Build branded, self-service digital portals that extend Salesforce data and functionality to customers, partners, and employees — outside the internal Salesforce org.

Experience Cloud (formerly Community Cloud) lets you create external-facing portals — customer self-service sites, partner deal registration portals, and employee help centers — all connected directly to your Salesforce data in real time. Community members get their own login and see only what they are allowed to see, governed by Salesforce's standard sharing model.

> **Note:** All topics in this section are covered via the Trailhead resources below. Complete each resource sequentially as they build on one another.

### 📚 Resources

| Resource | Type | Difficulty | Link |
|----------|------|------------|------|
| Community Cloud Basics | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/community_cloud_basics) |
| Community Search Basics | 📘 Module | Beginner | [→ Open](https://trailhead.salesforce.com/content/learn/modules/comm_search_basics) |
| Share CRM Data with Communities | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/communities_share_crm_data) |
| Personalize Audiences in Communities | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/communities_personalize_audiences) |
| Build a Community with Knowledge & Chat | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/build-a-community-with-knowledge-and-chat) |
| Community Theme & Layout | 🛠️ Project | Intermediate | [→ Open](https://trailhead.salesforce.com/content/learn/projects/communities_theme_layout) |

### 🎯 Key Concepts

**Community Templates**

| Template | Best For |
|----------|---------|
| Customer Account Portal | Account and case self-service for existing customers |
| Partner Central | Channel partner deal registration and pipeline visibility |
| Help Center | Public-facing Knowledge Base with optional login |
| Build Your Own | Fully custom community built from scratch |

**Key Features**
- **Experience Builder** — drag-and-drop page builder for community pages (similar to Lightning App Builder)
- **Member Assignment** — which internal profiles and community license types can access the community
- **Sharing Model** — external users are governed by Salesforce sharing (OWD, sharing rules, and community roles)
- **Community Roles** — a separate role hierarchy for external users; does not mix with internal employee roles
- **Branding & Theming** — logo, colors, fonts, and custom CSS applied through the Branding panel in Experience Builder
- **Audience Targeting** — show different page variations to different user segments (e.g., Platinum vs Standard customers)
- **Community Search** — powered by Salesforce Search; surfaces Knowledge Articles, Chatter posts, and CRM records
- **Embedded Chat** — Salesforce Messaging for Web enables live agent chat from within a community page

### 💡 Pro Tips

- Community users require a **Community license** — separate from internal Salesforce licenses; priced by login frequency or member seat count
- External users can only see records shared with them — OWD for community-exposed objects typically defaults to **Private**; use sharing rules or sharing sets to open up appropriate access
- Use **Audience Targeting** to personalize the experience without building and maintaining multiple separate communities
- Always preview your community as a **Guest User** before launch — unauthenticated visitors see exactly this view

---

## 📖 Resource Legend

| Badge | Type | What to Expect |
|-------|------|----------------|
| 📘 **Module** | Self-paced unit | Concept-first learning with multiple units, end-of-module quiz, and a Trailhead badge on completion |
| 🛠️ **Project** | Hands-on build | Step-by-step instructions executed in a real Salesforce org; requires a Trailhead Playground |
| 🛤️ **Trail** | Curated path | A multi-module series covering a broad topic end-to-end; earns a Trail completion badge |
| 🌐 **External** | Third-party | Community-authored deep-dives and practical guides from Apex Hours and Salesforce Developers |

---

## 📈 Curriculum Summary

| # | Section | Resources | Est. Duration | Difficulty |
|---|---------|-----------|---------------|------------|
| 01 | Getting Started | 3 | ~2.5 hrs | 🟢 Beginner |
| 02 | Licenses & User Management | 2 | ~1.5 hrs | 🟢 Beginner |
| 03 | Data Modeling & Objects | 5 | ~6 hrs | 🟡 Beginner–Intermediate |
| 04 | Security & Access Control | 4 | ~4 hrs | 🟡 Intermediate |
| 05 | Customization & UI | 5 | ~3 hrs | 🟢 Beginner |
| 06 | Business Logic & Automation | 6 | ~10 hrs | 🔴 Intermediate–Advanced |
| 07 | Reports & Dashboards | 5 | ~4 hrs | 🟡 Beginner–Intermediate |
| 08 | Data Management | 5 | ~4 hrs | 🟡 Intermediate |
| 09 | Sales Cloud | 4 | ~8 hrs | 🟡 Beginner–Intermediate |
| 10 | Service Cloud & Knowledge | 6 | ~7 hrs | 🟡 Intermediate |
| 11 | Experience Cloud | 6 | ~5 hrs | 🟡 Intermediate |
| | **Total** | **51** | **~55 hrs** | |

---

## 🏆 Certification Pathways

After completing this curriculum, you will be well prepared for the following Salesforce credentials:

| Certification | Relevant Sections | Exam Guide |
|---------------|-------------------|------------|
| Salesforce Certified Administrator | 01 through 09 | [Exam Guide](https://trailhead.salesforce.com/credentials/administrator) |
| Salesforce Certified Service Cloud Consultant | Deep-dive on Section 10 | [Exam Guide](https://trailhead.salesforce.com/credentials/servicecloudconsultant) |
| Salesforce Certified Experience Cloud Consultant | Deep-dive on Section 11 | [Exam Guide](https://trailhead.salesforce.com/credentials/experiencecloudconsultant) |

For a full list of credentials and official exam preparation trails, visit [trailhead.salesforce.com/credentials](https://trailhead.salesforce.com/credentials).

---

## 🔗 Additional Resources

| Resource | Description | Link |
|----------|-------------|------|
| Trailhead Home | Main learning platform | [trailhead.salesforce.com](https://trailhead.salesforce.com) |
| Trailblazer Community | Peer support, answers, groups | [Community](https://trailhead.salesforce.com/trailblazer-community) |
| Salesforce Help & Training | Official product documentation | [help.salesforce.com](https://help.salesforce.com) |
| Salesforce Release Notes | Tri-annual platform updates | [Release Notes](https://help.salesforce.com/s/articleView?id=release-notes.salesforce_release_notes.htm) |
| Salesforce Developer Docs | API references and developer guides | [developer.salesforce.com/docs](https://developer.salesforce.com/docs) |
| Apex Hours | Community blog — tutorials and how-tos | [apexhours.com](https://www.apexhours.com) |
| Admin Trailblazers Group | Official admin community group | [Join Group](https://trailhead.salesforce.com/trailblazer-community/groups/0F9300000001okuCAA) |

---

<div align="center">

`Prepared By *vamsi Prasad Purum*`

[![Made with Trailhead](https://img.shields.io/badge/Made%20with-Trailhead-00D2FF?style=flat-square&logo=salesforce)](https://trailhead.salesforce.com)

</div>
