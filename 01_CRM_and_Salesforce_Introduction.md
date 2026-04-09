# 01 · CRM & Salesforce Introduction

> **Module:** Getting Started | **Difficulty:** 🟢 Beginner | **Est. Time:** 2.5 hrs

---

## 1.1 What is CRM?

**CRM** stands for **Customer Relationship Management**. It is a strategy, process, and technology that companies use to manage interactions with current and potential customers.

Before CRM existed, companies stored customer data in:
- Spreadsheets (Excel, Google Sheets)
- Email inboxes
- Paper notebooks
- Individual sales reps' heads

**The problem:** When a sales rep left the company, all their customer knowledge left with them. When a customer called support, the agent had no idea what the sales team had promised. Data was siloed and invisible.

**CRM solves this** by creating a single source of truth — one place where Sales, Marketing, and Support all see the same customer.

### Real-World Example

> **Company:** A mid-sized software company called "TechBridge"
>
> **Before CRM:** Sarah (sales rep) tracks her leads in a personal Excel sheet. When she goes on leave, her manager has no idea which deals are close to closing. A customer calls support angry because the sales rep promised a discount — but support sees nothing about this.
>
> **After CRM (Salesforce):** Every lead, call, email, and deal is logged in Salesforce. Sarah's manager sees the pipeline in real time. Support opens the case and immediately sees "Sales rep promised 15% discount on 3-year contract" in the account notes. Customer is happy. Deal closes.

---

## 1.2 What is Salesforce?

Salesforce is the world's **#1 cloud-based CRM platform**, founded in 1999 by Marc Benioff. It is built entirely in the cloud — meaning no software to install, no servers to manage. You access it through a browser.

**Key philosophy: "No Software"** — Salesforce pioneered the SaaS (Software as a Service) model for enterprise software.

### What makes Salesforce different from a regular database?

| Regular Database | Salesforce |
|-----------------|------------|
| Stores raw data | Stores data + workflows + automation + analytics |
| Requires developers to query | Business users can click, not code |
| No built-in communication tools | Built-in email, activity tracking, approvals |
| No app marketplace | AppExchange with 7,000+ pre-built apps |
| Needs IT to update | Three automatic updates per year |

---

## 1.3 Salesforce Products & Clouds

Salesforce is not a single product — it is a **platform of clouds**, each serving a different business function.

### Core Clouds

| Cloud | What It Does | Who Uses It |
|-------|-------------|-------------|
| **Sales Cloud** | Manage leads, opportunities, accounts, contacts, and pipeline | Sales teams |
| **Service Cloud** | Handle customer support cases, knowledge base, SLAs | Support / CS teams |
| **Marketing Cloud** | Email campaigns, customer journeys, social media | Marketing teams |
| **Experience Cloud** | Build external portals for customers, partners, employees | IT / Admins |
| **Commerce Cloud** | Online storefronts for B2B and B2C | eCommerce teams |
| **Analytics Cloud (Tableau CRM)** | Advanced data visualization and AI insights | Analysts / Executives |
| **Platform (Force.com)** | Build custom apps entirely on Salesforce infrastructure | Developers / Admins |

### Real-World Example

> **Company:** "Global Retail Co." sells furniture online and in stores.
>
> - **Sales Cloud** — B2B sales reps manage enterprise client accounts and track deal stages
> - **Service Cloud** — Support agents handle warranty claims and customer complaints
> - **Marketing Cloud** — Sends personalized promotional emails based on purchase history
> - **Experience Cloud** — A self-service portal where customers track their own orders and raise tickets
> - **Commerce Cloud** — Their online shop where consumers browse and purchase

---

## 1.4 Salesforce Editions

Salesforce comes in different **editions** — think of them like pricing tiers. Each edition unlocks more features.

| Edition | Target | Key Features |
|---------|--------|-------------|
| **Essentials** | Small businesses (≤10 users) | Basic CRM, limited customization |
| **Professional** | Growing businesses | Full CRM, email integration, limited automation |
| **Enterprise** | Mid-large companies | Full customization, workflow automation, APIs |
| **Unlimited** | Large enterprises | Unlimited custom apps, 24/7 support, full sandbox |
| **Developer Edition** | Developers / Admins | Free, full features, limited data storage — for learning |

> 💡 **Your Trailhead Playground is essentially a Developer Edition** — free and full-featured, just with storage limits. Perfect for all exercises in this program.

---

## 1.5 Multitenant Architecture

This is one of Salesforce's most important technical concepts and a **frequent exam question**.

### What is Multitenancy?

Imagine a large apartment building. Each tenant (family) has their own **private apartment** — their own furniture, locks, and rules. But they **share the building infrastructure** — the elevator, plumbing, and electricity.

Salesforce works the same way:
- **You (the tenant)** = your company's Salesforce org with your data, customizations, and users
- **Other tenants** = other companies using Salesforce at the same time
- **Shared infrastructure** = Salesforce's servers, databases, and software

### Why does this matter?

| Benefit | Explanation |
|---------|-------------|
| **Automatic upgrades** | Salesforce pushes 3 updates/year to everyone simultaneously — you never manually upgrade |
| **Lower cost** | Infrastructure costs are shared across all customers |
| **Instant scalability** | Salesforce handles traffic spikes — you never buy more servers |
| **Your data is isolated** | Despite sharing infrastructure, your data is completely private and invisible to other tenants |

### Real-World Example

> **Tenant A:** Coca-Cola's Salesforce org — 50,000 users, millions of records
> **Tenant B:** A 5-person startup's Salesforce org
>
> Both run on Salesforce's shared infrastructure. Both get the same Spring '26 update on the same day. Neither can see the other's data. Neither needed to do anything for the upgrade — it just happened.

### Contrast: Single-Tenant (Old-School)

> Old enterprise software (like Oracle EBS) installed on your own servers = single-tenant. You own the hardware. You schedule downtime for upgrades. Your IT team manages patches. Very expensive. Very slow to update.

---

## 1.6 Salesforce Environments

Different types of orgs serve different purposes. Understanding which environment to use for what is critical.

| Environment | Purpose | Data | Cost |
|-------------|---------|------|------|
| **Production** | Your live org — real business operations | Real customer data | Paid |
| **Full Sandbox** | Exact copy of production including data | Copy of prod data | Paid add-on |
| **Partial Sandbox** | Copy of production config + sample data | Subset of prod data | Paid add-on |
| **Developer Sandbox** | Configuration copy only, no data | No data | Included |
| **Developer Edition** | Standalone free org for learning | Sample data only | Free |
| **Trailhead Playground** | Pre-configured for Trailhead exercises | Sample data | Free |
| **Scratch Org** | Temporary, source-driven org for CI/CD | Empty | Free (with Dev Hub) |

### Real-World Example

> **Scenario:** "RetailCo" wants to implement a new approval process for discounts.
>
> 1. **Developer Sandbox** — admin builds and tests the approval process alone
> 2. **Partial Sandbox** — QA team tests with realistic data volumes
> 3. **Full Sandbox** — UAT (User Acceptance Testing) with real users on a full copy of production
> 4. **Production** — deploy after all testing passes
>
> ❌ **Wrong approach:** Building directly in Production. If your automation has a bug, it breaks live customer operations.

---

## 1.7 The Salesforce Release Cycle

Salesforce updates **three times per year** — automatically, for all customers, with no downtime.

| Release | Timing | Preview Available |
|---------|--------|-------------------|
| **Spring** | February | November (Sandbox preview) |
| **Summer** | June | April |
| **Winter** | October | August |

### What happens during a release?
- New features are added (some auto-on, some opt-in)
- Existing features may be deprecated or changed
- Your sandbox gets the update ~4 weeks **before** production — use this window to test

> 💡 Always check the **Release Notes** before each major update. Salesforce publishes them at [help.salesforce.com](https://help.salesforce.com). Some features may change behavior in automation or reports.

---

## 1.8 Trailhead — Your Learning Platform

Trailhead is Salesforce's free online learning platform. It is gamified — you earn points, badges, and ranks as you complete content.

### Content Types

| Type | Description | Earns |
|------|-------------|-------|
| **Module** | Self-paced units with text, videos, and quizzes | Badge |
| **Project** | Hands-on steps you complete in a real org | Badge |
| **Trail** | Curated series of modules on a topic | Trail badge |
| **Superbadge** | Advanced real-world scenario with no instructions — must figure it out yourself | Superbadge |
| **Trailmix** | A custom playlist of modules/projects you build yourself | — |

### Trailhead Ranks (by points)

`Trailblazer` → `Hiker` → `Explorer` → `Adventurer` → `Ranger` → `Double Star Ranger` → ...

### Trailhead Playground Management

A **Trailhead Playground** is a free Salesforce org pre-wired to Trailhead. You need one for all hands-on challenges.

**How to get one:**
1. Log in to Trailhead
2. Open any Module or Project with a hands-on challenge
3. Click **"Launch"** at the bottom — a Playground is created for you
4. You can have up to **10 Playgrounds** at a time
5. You can reset a Playground at any time (erases all customization and data)

> ⚠️ **Never use a Playground you've customized for one module to complete a different module's challenge** — leftover configurations can cause unexpected errors. Use a fresh Playground per major project.

---

## 1.9 Org-Wide Setup — Company Information

Once your org is provisioned, the first admin task is configuring company-level settings.

### Setup → Company Settings → Company Information

| Field | What It Does |
|-------|-------------|
| **Company Name** | Appears in emails and system messages |
| **Primary Contact** | Who Salesforce contacts for billing/support |
| **Default Locale** | Date/time/number format for all users |
| **Default Language** | Language for system labels and UI |
| **Default Time Zone** | All timestamps displayed in this zone unless user overrides |
| **Currency** | Corporate currency (once set, think carefully before changing) |
| **Fiscal Year** | Calendar or custom fiscal year — affects reports and forecasting |

### Real-World Example

> **Company:** "AsiaPac Consulting" — HQ in Singapore, sales teams in Australia, India, and the UK.
>
> - **Default Locale:** English (Singapore)
> - **Default Time Zone:** Asia/Singapore (GMT+8)
> - **Currency:** SGD (Singapore Dollar)
> - **Fiscal Year:** Custom — starts April 1 (common in India/UK)
>
> Each **user** can then override their own time zone and locale in their personal settings so their activity timestamps reflect their local time — but reports roll up in the org's default currency and fiscal year.

---

## Summary

| Concept | One-Line Definition |
|---------|---------------------|
| CRM | Technology to manage customer relationships and centralize data |
| Salesforce | World's #1 cloud CRM platform — SaaS, no software to install |
| Multitenancy | Shared infrastructure, isolated data — like apartments in a building |
| Editions | Tiers of Salesforce features from Essentials to Unlimited |
| Sandbox | A copy of production used for safe development and testing |
| Trailhead | Free gamified learning platform — modules, projects, trails, superbadges |

---

## 🔗 Trailhead Resources

- [Salesforce Free Trials](https://developer.salesforce.com/free-trials)
- [Trailhead Playground Management](https://trailhead.salesforce.com/content/learn/modules/trailhead_playground_management)
- [Company-Wide Org Settings](https://trailhead.salesforce.com/content/learn/modules/company_wide_org_settings)

---

*Next: [02 · Salesforce Licenses & User Management →](./02_Licenses_and_User_Management.md)*
