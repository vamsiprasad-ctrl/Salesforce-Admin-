# 01 · CRM & Salesforce Introduction — Complete Deep Guide

> **Module:** Getting Started | **Difficulty:** 🟢 Beginner | **Est. Time:** 4 hrs
> **Covers:** CRM concepts · Salesforce history · Products · Architecture · Editions · Environments · Release cycle · Trailhead · Org Setup

---

## Table of Contents

1. [What is CRM?](#11-what-is-crm)
2. [History of Salesforce](#12-history-of-salesforce)
3. [What is Salesforce?](#13-what-is-salesforce)
4. [Salesforce Products & Clouds](#14-salesforce-products--clouds)
5. [Salesforce Editions](#15-salesforce-editions)
6. [Multitenant Architecture](#16-multitenant-architecture)
7. [Salesforce Environments & Sandboxes](#17-salesforce-environments--sandboxes)
8. [The Salesforce Release Cycle](#18-the-salesforce-release-cycle)
9. [Trailhead — Your Learning Platform](#19-trailhead--your-learning-platform)
10. [Org-Wide Setup](#110-org-wide-setup)
11. [Hands-On Tasks](#-hands-on-tasks)
12. [Practice Questions](#-practice-questions)
13. [Scenario-Based Questions](#-scenario-based-questions)
14. [Answer Key](#️-answer-key)

---

## 1.1 What is CRM?

**CRM** stands for **Customer Relationship Management**. It is simultaneously:

- A **strategy** — the business approach of putting the customer at the center of all decisions
- A **process** — the workflows and steps a company follows to manage customer interactions
- A **technology** — the software platform that stores, tracks, and automates customer data

### The Problem CRM Solves

Before CRM technology existed, businesses managed customer relationships in completely fragmented ways:

| Data Type | Where It Was Stored | The Problem |
|-----------|-------------------|------------|
| Customer contact details | Sales rep's personal Outlook | Lost when rep resigns |
| Call notes | Paper notebooks | Not searchable, not shared |
| Email history | Individual inboxes | No one else can see what was promised |
| Deal status | Excel spreadsheet | Only the rep knows where each deal stands |
| Support history | Separate ticketing tool | Disconnected from sales data |
| Revenue data | Finance ERP system | Marketing and Sales can't see it |

**The result of fragmentation:**
- A customer calls support → agent has no idea what the sales team promised them
- A sales rep leaves → their pipeline disappears with them
- Marketing runs a campaign → sales doesn't know which customers responded
- A customer buys again → no record of their previous purchase preferences
- Management wants forecasting → someone spends 3 days manually compiling Excel reports

### What CRM Changes

A CRM creates a **single source of truth** — one central location where every customer-facing team sees the same data.

```
Without CRM (Siloed):                With CRM (Unified):
━━━━━━━━━━━━━━━━━━━━━                ━━━━━━━━━━━━━━━━━━━━━━
Sales ──── own Excel                  Sales ──────┐
Marketing ─ own tool                  Marketing ──┤──→ SALESFORCE ←── Customer
Support ─── own inbox                 Support ────┘         ↕
Finance ─── own ERP                   (Everything synced, everyone sees everything)
```

### The 3 Pillars of CRM

**1. Sales CRM**
Track every interaction from the first prospect contact to closed deal. Know which deals are open, what stage they're in, who owns them, and when they're expected to close.

**2. Service CRM**
Every customer support interaction — calls, emails, chat, cases — is logged and linked to the customer's full history. Agents see the complete picture before picking up the phone.

**3. Marketing CRM**
Track which campaigns generated which leads. Understand which marketing activities actually produce revenue. Segment customers for targeted communications.

### Real-World Example — Before and After CRM

> **Company:** ConsultEdge — a 50-person IT consulting firm
>
> **BEFORE Salesforce:**
>
> Scenario: A potential client, Acme Corp, was contacted by three different salespeople over 6 months — none of them knew the others had already spoken to the same company. Acme Corp's CEO called the managing director to complain: "Your company keeps calling us with the same pitch. We've already said we're not interested."
>
> Cost: Lost deal. Damaged reputation. No way to know how often this happened.
>
> **AFTER Salesforce:**
>
> Every rep logs every call against Acme Corp's Account record. Before calling, a rep sees: "James called on March 15, spoke to the CEO, not interested in cloud services until Q4 2026." The rep changes their approach — or decides not to call at all. Professional. Coordinated.
>
> Additional benefit: The MD opens Salesforce on Monday morning and sees: Pipeline = ₹8.5 Crore, 12 deals expected to close this month, average deal size = ₹35 Lakhs. No Excel report needed. Real-time.

---

## 1.2 History of Salesforce

Understanding the origin of Salesforce explains its design philosophy and why it became dominant.

### The Founding (1999)

**Marc Benioff** founded Salesforce in 1999 with a radical idea: enterprise software should work like a website — accessed through a browser, no installation, subscription pricing, automatic updates.

At the time, enterprise CRM software meant:
- Installing a multi-gigabyte application on every computer
- Paying hundreds of thousands of dollars upfront for licenses
- Hiring an implementation team for 18 months
- Buying your own servers to run it on
- Managing upgrades yourself every few years (often skipped because it was too risky)

Benioff's philosophy: **"No Software"** — a provocative marketing slogan that became their brand identity.

### Key Milestones

| Year | Milestone |
|------|-----------|
| 1999 | Founded in a San Francisco apartment |
| 2000 | First version launched — basic CRM in a browser |
| 2003 | Dreamforce conference started (now the world's largest tech conference) |
| 2004 | IPO on NYSE — raised $110M |
| 2006 | **AppExchange** launched — first enterprise app marketplace |
| 2007 | **Force.com platform** — developers can build apps on Salesforce infrastructure |
| 2009 | **Chatter** — social collaboration inside Salesforce |
| 2013 | **ExactTarget** acquired — became Marketing Cloud |
| 2014 | **$5B revenue** — fastest software company to reach this milestone at the time |
| 2015 | **Lightning Experience** — complete UI redesign |
| 2016 | **Einstein AI** — artificial intelligence built into the platform |
| 2019 | **Tableau** acquired — analytics powerhouse |
| 2020 | **Slack** acquired ($27.7B) — enterprise communication |
| 2023 | **Einstein GPT / Agentforce** — generative AI embedded into CRM |
| Today | **$35B+ revenue**, 150,000+ customers, 73,000+ employees |

### Why Salesforce Won

Before Salesforce, companies like **Siebel Systems** (owned by Oracle) dominated CRM. Their software cost millions, took years to implement, and was notoriously difficult to use.

Salesforce offered:
- **Subscription pricing** — monthly/annual, cancel anytime (no upfront millions)
- **Browser access** — no installation, works anywhere, works on any device
- **Automatic upgrades** — Salesforce updates the software for you, 3 times a year
- **Faster implementation** — weeks/months instead of years
- **AppExchange** — a marketplace of pre-built apps (no custom development needed for common needs)

---

## 1.3 What is Salesforce?

Salesforce is a **cloud-based CRM platform** — but calling it just a "CRM" undersells it significantly. Today Salesforce is a complete business operating platform.

### What Salesforce Actually Is

| Layer | What It Means | Example |
|-------|--------------|---------|
| **CRM** | Manages customer relationships — leads, deals, cases | Track 500 open opportunities |
| **Platform** | Build custom apps without buying servers | Build a custom Loan Application app |
| **Automation Engine** | Automate business processes | Auto-route cases, send emails, approve discounts |
| **Analytics Platform** | Reports, dashboards, AI insights | Real-time pipeline dashboard |
| **Integration Hub** | Connect to external systems | Sync with SAP, send SMS via Twilio |
| **App Marketplace** | 7,000+ pre-built apps | Install Docusign for e-signatures |

### Salesforce vs Competitors

| CRM Platform | Who It's For | Key Strength |
|-------------|-------------|-------------|
| **Salesforce** | Enterprise, mid-market | Most powerful, most customizable, largest ecosystem |
| **HubSpot** | SMBs, startups | Free tier available, easier for beginners |
| **Microsoft Dynamics 365** | Microsoft shops | Tight integration with Office 365 |
| **Zoho CRM** | Budget-conscious | Affordable, decent feature set |
| **Pipedrive** | Small sales teams | Simple, visual pipeline |

**Why Salesforce leads:** It's the only CRM that is simultaneously a true platform — you can build virtually any business application on top of it, not just CRM.

### The Salesforce Ecosystem

```
┌─────────────────────────────────────────────────────┐
│                  SALESFORCE PLATFORM                 │
│                                                      │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐            │
│  │  Sales   │ │ Service  │ │Marketing │  ← Clouds  │
│  │  Cloud   │ │  Cloud   │ │  Cloud   │            │
│  └──────────┘ └──────────┘ └──────────┘            │
│                                                      │
│  ┌──────────────────────────────────────────────┐   │
│  │           Force.com Platform                  │   │
│  │  Custom Objects · Apex · Lightning · Flow     │   │
│  └──────────────────────────────────────────────┘   │
│                                                      │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐            │
│  │AppExchange│ │  Slack   │ │ Tableau  │  ← Add-ons │
│  │ (7,000+) │ │  Comms   │ │Analytics │            │
│  └──────────┘ └──────────┘ └──────────┘            │
└─────────────────────────────────────────────────────┘
```

---

## 1.4 Salesforce Products & Clouds

Salesforce is not one product — it is a **family of cloud products**, each targeting a specific business function. Companies buy the clouds relevant to their needs.

---

### Sales Cloud

**Purpose:** Manage the entire sales process from first lead to closed deal.

**Key Features:**
- Leads — track unqualified prospects
- Accounts & Contacts — manage companies and people
- Opportunities — track deals in progress with stages and amounts
- Forecasting — predict future revenue
- Products & Price Books — what you sell and for how much
- Reports & Dashboards — real-time pipeline visibility
- Email integration — Gmail/Outlook sync
- Mobile app — log calls and update deals on the go

**Who Uses It:** Sales Development Reps, Account Executives, Sales Managers, Revenue Ops

**Real-World Example:**
> **RetailTech Inc.** — 200 sales reps use Sales Cloud to manage 3,000 active deals totaling ₹450 Crore pipeline. Their VP of Sales opens Salesforce at 8AM every day and sees the full team pipeline by rep, by region, by product line — without a single Excel report.

---

### Service Cloud

**Purpose:** Manage customer support — from case creation to resolution.

**Key Features:**
- Cases — track individual support issues
- Case Queues — route cases to the right team
- Knowledge Base — searchable library of solutions
- Email-to-Case / Web-to-Case — capture cases from email or web forms
- Escalation Rules — auto-escalate SLA breaches
- Live Chat — real-time customer chat
- Lightning Service Console — productivity workspace for agents
- Entitlements — track SLA commitments per customer

**Who Uses It:** Support Agents, Service Managers, Customer Success

**Real-World Example:**
> **BankFirst** — 500 support agents handle 8,000 customer cases per day using Service Cloud. Cases from email, phone, web, and chat all arrive in one queue. Knowledge Base deflects 30% of cases — customers find answers themselves. Average handle time reduced from 12 minutes to 7 minutes.

---

### Marketing Cloud

**Purpose:** Execute, automate, and measure marketing campaigns.

**Key Features:**
- Email Studio — design and send emails
- Journey Builder — automated multi-step customer journeys
- Social Studio — manage social media
- Advertising Studio — targeted ads
- Mobile Studio — SMS and push notifications
- Analytics — campaign performance tracking

**Who Uses It:** Marketing teams, Digital marketers, Campaign managers

**Real-World Example:**
> **FashionBrand** — a customer buys a dress → Journey Builder automatically sends a "Complete the look" email 2 days later → if they click but don't buy → sends a 10% discount 3 days after → if still no purchase → adds them to a "re-engagement" segment for the next seasonal campaign. All automated.

---

### Experience Cloud (formerly Community Cloud)

**Purpose:** Build branded external portals for customers, partners, or employees.

**Key Features:**
- Customer Portals — self-service support, case tracking
- Partner Portals — deal registration, lead sharing
- Employee Communities — internal knowledge base, HR self-service
- Commerce Sites — B2B and B2C storefronts
- Experience Builder — drag-and-drop page designer

**Who Uses It:** IT Admins, Salesforce Admins, Customer Success

**Real-World Example:**
> **InsureCo** — policyholders log into a branded customer portal (looks like InsureCo's website, not Salesforce). They submit claims, track status, download policy documents, and chat with support agents — all directly connected to InsureCo's Salesforce org. Zero manual handoffs.

---

### Commerce Cloud

**Purpose:** Build and run e-commerce storefronts.

**Types:**
- **B2C Commerce** — consumer-facing online shops
- **B2B Commerce** — business-facing ordering portals

**Real-World Example:**
> **RetailChain** — their B2C Commerce site handles 50,000 online orders per day. Customer history, preferences, and purchase records sync directly into Salesforce CRM — giving support agents full context when a shopper calls with a question.

---

### Analytics Cloud (Tableau CRM / Einstein Analytics)

**Purpose:** Advanced data visualization, AI insights, and business intelligence.

**Key Features:**
- Interactive dashboards beyond standard Salesforce dashboards
- AI-powered predictions ("Which leads are most likely to convert?")
- Data blending from multiple sources
- Mobile-optimized analytics

**Real-World Example:**
> **PharmaCo** — their regional sales directors use Tableau CRM to see territory performance maps, doctor visit frequencies, prescription trends, and competitive data — all in one dashboard that updates in real time.

---

### Platform (Force.com)

**Purpose:** Build custom business applications on Salesforce infrastructure — without buying servers or managing infrastructure.

**Key Features:**
- Custom Objects and Fields
- Apex (Java-like programming language for Salesforce)
- Lightning Web Components (JavaScript UI framework)
- Flow Builder (no-code automation)
- REST/SOAP APIs

**Real-World Example:**
> **LegalEase** — a law firm builds a custom "Matter Management" app entirely on the Salesforce Platform. It tracks legal cases, hearings, billable hours, client communications, and document management. It has nothing to do with standard CRM — it is a fully custom application running on Salesforce infrastructure.

---

### Slack

**Purpose:** Team communication and collaboration — acquired by Salesforce in 2021.

**Integration with Salesforce:**
- Get Salesforce record updates directly in Slack channels
- Approve discounts from Slack
- Create cases from Slack messages
- Sales reps notified in Slack when a deal changes stage

---

### MuleSoft

**Purpose:** Integration platform — connects Salesforce to any external system via APIs.

**Real-World Example:**
> **ManufactureCo** uses MuleSoft to connect Salesforce (CRM) → SAP (ERP) → Oracle HCM (HR) → Twilio (SMS) → all in real time. When a deal closes in Salesforce, MuleSoft automatically creates a purchase order in SAP and notifies the delivery team via SMS.

---

## 1.5 Salesforce Editions

Salesforce is sold in **Editions** — think of these as pricing tiers that control which features are included.

### Sales Cloud Editions Comparison

| Feature | Essentials | Professional | Enterprise | Unlimited |
|---------|-----------|-------------|-----------|-----------|
| Max Users | 10 | Unlimited | Unlimited | Unlimited |
| Accounts, Contacts, Leads | ✅ | ✅ | ✅ | ✅ |
| Opportunities | ✅ | ✅ | ✅ | ✅ |
| Email Integration | ✅ | ✅ | ✅ | ✅ |
| Reports & Dashboards | Basic | ✅ | ✅ | ✅ |
| API Access | ❌ | ❌ | ✅ | ✅ |
| Workflow Rules / Flow | Limited | ✅ | ✅ | ✅ |
| Custom Profiles | Limited | ✅ | ✅ | ✅ |
| Sandboxes | ❌ | Developer only | ✅ | Full + Partial |
| Custom Objects | 0 | 50 | Unlimited | Unlimited |
| 24/7 Phone Support | ❌ | ❌ | ❌ | ✅ |
| Approx. Price (per user/month) | ₹1,900 | ₹5,500 | ₹9,300 | ₹18,600 |

*(Prices are approximate and change — always check salesforce.com/pricing for current)*

### Key Edition Decision Points

**Essentials → Professional:** When you exceed 10 users or need unlimited customization
**Professional → Enterprise:** When you need API access (to integrate with other systems), full sandboxes, and unlimited custom objects
**Enterprise → Unlimited:** When you need maximum sandbox environments, 24/7 phone support, and the highest governor limits

### Developer Edition and Trailhead Playground

| | Developer Edition | Trailhead Playground |
|--|-----------------|---------------------|
| Cost | Free | Free |
| Features | Full Enterprise-level | Full Enterprise-level |
| Data storage | 5MB / 20MB | 5MB / 20MB |
| Expiry | Never expires | Never expires |
| Pre-configured | No | Yes — for Trailhead challenges |
| How to get | developer.salesforce.com/signup | Launch from any Trailhead module |
| Max per account | Unlimited | 10 |

---

## 1.6 Multitenant Architecture

This is one of the most fundamental concepts in Salesforce and a **guaranteed exam topic**.

### The Problem Multi-Tenancy Solves

Traditional enterprise software required every company to:
- Buy and maintain their own servers
- Install and manage their own database software
- Manage their own backups, security patches, and disaster recovery
- Upgrade software on their own schedule (often years behind)
- Scale infrastructure when usage grew

This was massively expensive and required dedicated IT teams.

### What Multi-Tenancy Means

**Multitenancy** = a single instance of software serves multiple customers (tenants) simultaneously, with **complete data isolation** between them.

### The Apartment Building Analogy (Deep)

Think of Salesforce like a luxury apartment building:

```
┌────────────────────────────────────────────────┐
│            SALESFORCE BUILDING                  │
│                                                  │
│  ┌──────────────┐  ┌──────────────┐            │
│  │ Apartment 1  │  │ Apartment 2  │            │
│  │  Coca-Cola   │  │   StartupXY  │            │
│  │  50,000 users│  │   5 users    │            │
│  │  Private data│  │  Private data│            │
│  └──────────────┘  └──────────────┘            │
│                                                  │
│  Shared: Elevators (servers), Plumbing (DB),    │
│  Electricity (network), Security desk (Salesforce│
│  security team), Janitors (SF maintenance team) │
│                                                  │
│  Private: Your apartment (your org, your data,  │
│  your customizations — nobody else can enter)   │
└────────────────────────────────────────────────┘
```

**What's shared:** Physical servers, database software, networking, Salesforce's engineering team maintaining the system, security infrastructure, disaster recovery.

**What's private:** Your org's data, your custom objects, your flows, your users, your configurations. Completely invisible to other tenants.

### The Metadata Architecture

Salesforce achieves multitenancy through a clever technical approach:

- **Your data** is stored in a shared database, but tagged with your **Org ID** — queries always filter by your Org ID, making it impossible to accidentally retrieve another company's data
- **Your customizations** are stored as **metadata** — descriptions of objects, fields, and logic — not as separate code deployments
- When Salesforce upgrades the platform, they upgrade the engine — your metadata (customizations) rides on top and is unaffected by the engine upgrade

### Benefits of Multitenancy for You

| Benefit | What It Means in Practice |
|---------|--------------------------|
| **Automatic upgrades** | Salesforce engineers upgrade the platform 3x/year. You wake up and new features are there. You didn't pay for it or schedule anything. |
| **No IT infrastructure** | You have zero servers to buy, zero databases to manage, zero patches to apply. Salesforce handles all of it. |
| **Instant scalability** | A startup with 5 users and a company with 100,000 users use the same infrastructure. If you hire 500 new reps, Salesforce handles the load automatically. |
| **Always-current security** | Salesforce's security team continuously updates defenses. You benefit automatically — you didn't need to hire a security team. |
| **Global disaster recovery** | Salesforce has multiple data centers. If one goes down, your org fails over to another automatically. |
| **Lower cost** | Infrastructure costs are amortized across hundreds of thousands of customers. |

### Single-Tenant vs Multi-Tenant

| Aspect | Single-Tenant (On-Premise) | Multi-Tenant (Salesforce) |
|--------|--------------------------|--------------------------|
| Infrastructure | Your own servers | Shared, managed by Salesforce |
| Upgrades | Your team does it | Salesforce does it automatically |
| Scalability | Buy more hardware | Automatic |
| Maintenance | Your IT team | Salesforce team |
| Security | Your responsibility | Shared responsibility — Salesforce handles infra |
| Cost model | Large upfront + ongoing IT | Subscription per user/month |
| Customization | Unlimited (you control everything) | Within Salesforce's platform boundaries |
| Downtime risk | High (you manage) | Low (Salesforce manages, 99.9%+ SLA) |

### Real-World Example — Why This Matters

> **OldSchool ERP Company** runs SAP on-premise:
> - Upgrades planned every 2 years — costs ₹5 Crore and 6 months of consulting
> - During upgrade: 3 months of testing, 1 weekend of downtime for migration
> - Upgrade often postponed — running 5-year-old software
> - Bug in version 4.2 → only fixed in version 4.3 → wait 18 months for next upgrade cycle
>
> **Salesforce Customer:**
> - Spring '26 release pushes out automatically in February
> - New features appear in the platform — no action needed
> - Zero downtime (upgrades happen during low-traffic windows, usually Saturdays)
> - Bug fixed in a patch → all customers get the fix within days

---

## 1.7 Salesforce Environments & Sandboxes

Understanding environments is critical for every Salesforce professional. Using the wrong environment for the wrong purpose is how data gets destroyed.

### The Core Principle

**Never build or test in Production.** Production is where your real customers' data lives. A broken Flow or wrong Sharing Rule in Production affects real users immediately.

### Complete Environment Reference

#### Production Org
- **What it is:** Your live, running Salesforce environment
- **Who uses it:** All your real users doing real business
- **Data:** Real customer data — Accounts, Contacts, Opportunities, Cases
- **Risk level:** MAXIMUM — any error here affects real business
- **Changes allowed:** Only after thorough testing in sandbox

#### Full Sandbox
- **What it is:** An exact copy of Production — configuration AND data
- **Data:** A complete snapshot of Production data at the time of sandbox creation
- **Refresh cycle:** Up to once every 29 days
- **Use for:** User Acceptance Testing (UAT), final pre-deployment validation
- **Cost:** Included with Unlimited Edition; paid add-on for Enterprise
- **Limit:** 1 per Unlimited org (typically)

**Real-World Use:**
> A bank is deploying a new loan approval Flow. Before going to Production, they test in the Full Sandbox with real loan officers using real-looking production data. They discover the Flow doesn't handle a specific edge case (loans over ₹50L with a co-applicant). They fix it in sandbox first. Then deploy to Production.

#### Partial Sandbox
- **What it is:** Production configuration + a configurable subset of data
- **Data:** You choose which objects to include and what percentage of records to copy (e.g., 10,000 of your 500,000 Accounts)
- **Refresh cycle:** Up to once every 5 days
- **Use for:** Performance testing, integration testing with realistic (but not full) data volumes
- **Advantage over Full Sandbox:** Refreshes faster, smaller, cheaper — but has enough data to test realistic scenarios

#### Developer Sandbox
- **What it is:** A copy of Production's configuration (objects, fields, automation) with NO data
- **Data:** Empty — you manually create test records
- **Refresh cycle:** Up to once per day
- **Use for:** Building and unit-testing new features in isolation
- **Limit:** Included with most paid editions; Enterprise gets more
- **Size:** Smallest sandbox type

**Real-World Use:**
> An admin needs to build a new Approval Process for expense reports. They create it in a Developer Sandbox, configure the steps, test with 5 manually created expense records, verify it works correctly, then promote the configuration to the Full Sandbox for UAT.

#### Developer Pro Sandbox
- **What it is:** Like Developer Sandbox but with more storage (1GB instead of 200MB)
- **Use for:** Building features that require importing larger test datasets

#### Developer Edition Org
- **What it is:** A standalone, completely free Salesforce org — NOT connected to any Production org
- **Data:** Sample data provided by Salesforce, or you create your own
- **Use for:** Learning, personal projects, ISV (app) development
- **Expiry:** Never — it's yours forever
- **How to get:** developer.salesforce.com/signup

#### Trailhead Playground
- **What it is:** A Developer Edition org pre-configured for Trailhead exercises
- **How to get:** Launch from any Trailhead module or project
- **Max per account:** 10 at a time
- **Special features:** Pre-installed Trailhead-specific packages and configurations needed for challenge verification
- **Resettable:** You can reset a Playground to a fresh state anytime

#### Scratch Org
- **What it is:** A temporary, ephemeral org created programmatically from source code
- **Data:** Completely empty — no configuration either (starts from scratch)
- **Duration:** Up to 30 days, then it expires and is deleted
- **Created via:** Salesforce CLI (command line)
- **Use for:** Modern DevOps — CI/CD pipelines, developer feature branches
- **Think of it as:** A disposable test environment created on demand, used, then thrown away
- **Cost:** Free with Dev Hub enabled

### The Deployment Pipeline

Most professional Salesforce implementations follow this environment progression:

```
Dev Sandbox → Developer Pro → Partial Sandbox → Full Sandbox → Production
    (build)    (unit test)     (integration)      (UAT)          (live)
```

Each promotion to the next environment is done via:
- **Change Sets** (basic, point-and-click)
- **Salesforce CLI + metadata** (intermediate)
- **CI/CD pipelines with Scratch Orgs** (advanced)

### Sandbox Refresh

When you **refresh** a sandbox, Salesforce takes a new snapshot of Production and overwrites your sandbox. This is important because:
- After a refresh, sandbox data matches Production at that point in time
- Any customizations you made in the sandbox that haven't been deployed to Production are **lost**

**Always deploy your sandbox changes to Production BEFORE refreshing the sandbox.**

---

## 1.8 The Salesforce Release Cycle

### Three Times Per Year

Salesforce updates its platform **three times per year** — automatically, to all customers, with no downtime.

| Release | Production Deployment | Sandbox Preview Available |
|---------|--------------------|--------------------------|
| **Spring** | February | November (prior year) |
| **Summer** | June | April |
| **Winter** | October | August |

### The Release Timeline (Example: Summer '26)

```
April 2026:     Sandbox Preview available — Summer '26 features live in Sandboxes
April–May:      Test your customizations in sandbox against new release features
May 2026:       Salesforce publishes final Release Notes
June 2026:      Production orgs updated — Summer '26 is live for all customers
```

### What a Release Contains

Each release contains hundreds of changes, sorted into:

| Category | Description | Example |
|----------|-------------|---------|
| **New Features** | Brand new functionality | New Flow element type |
| **Enhancements** | Improvements to existing features | Flow Builder UI redesign |
| **Deprecated Features** | Features being phased out | Notification: Workflow Rules retiring |
| **Critical Updates** | Security or performance fixes that may require opt-in | New session security policy |
| **Auto-On** | Features enabled automatically with no admin action | UI tweaks |
| **Opt-In** | Features you must manually enable | New field type (requires testing) |

### Why the Preview Window Matters

When a Sandbox gets the Summer '26 update in April, your Production org still has the old version. This gives you **6-8 weeks** to:

1. Open your Sandbox → explore new features
2. Check if any of your existing customizations behave differently
3. Test automations, reports, and integrations against the new version
4. Raise a Salesforce Support case if anything breaks
5. Update your configurations to be compatible before Production gets the update

**Real-World Example:**
> Spring '25 introduced changes to how certain Flow formula functions evaluated null values. Companies that tested in their Preview Sandbox found that 3 of their Flows were producing incorrect results with the new behavior. They fixed the Flows in sandbox before Spring '25 hit Production. Companies that didn't test were surprised by broken automation in Production.

### How to Prepare for a Release

1. **Monitor Release Notes** — released 6–8 weeks before Production update: help.salesforce.com/release-notes
2. **Enable Preview Sandbox** — request your sandbox be on the Preview release
3. **Test critical automation** — run through your most important Flows, reports, and integrations
4. **Check Critical Updates** — review any pending critical updates and evaluate impact
5. **Train users** — if new features change the UI, prepare training materials
6. **Communicate** — send a pre-release communication to your users so UI changes aren't a surprise

### Salesforce Trust Status

**trust.salesforce.com** — Salesforce's public status page showing:
- Current system status for all environments
- Scheduled maintenance windows
- Past incidents and their resolutions

Bookmark this page. When users report Salesforce is slow or unavailable, check Trust first before investigating your customizations.

---

## 1.9 Trailhead — Your Learning Platform

### What is Trailhead?

**Trailhead** (trailhead.salesforce.com) is Salesforce's **free, gamified online learning platform**. It covers everything from CRM basics to advanced developer topics — with hands-on challenges verified in real Salesforce orgs.

It is completely free to use and has become the standard way to learn Salesforce globally.

### Content Types — In Detail

#### Module
- A self-paced learning unit covering one topic
- Consists of **Units** — text + diagrams + videos
- Each unit ends with a **quiz** (multiple choice, must pass to earn credit)
- Final unit has a **Challenge** — usually a multiple-choice quiz
- **Earns:** A Badge (displayed on your Trailhead profile)
- **Time:** Typically 30 minutes to 2 hours

#### Project
- A hands-on, step-by-step guided build
- You follow instructions and perform real actions in your Salesforce Playground
- At the end, Trailhead **automatically verifies** your work by checking your org's configuration
- If it's correct → Challenge passed → Badge earned
- **Earns:** A Badge
- **Time:** Typically 1–3 hours

**Why Projects are powerful:** You build real skills by doing, not just reading. Trailhead verifies you actually built what was asked — not just answered a quiz.

#### Trail
- A curated collection of Modules and Projects on a related topic
- Like a syllabus for a specific role or skill
- **Example:** "Admin Beginner" Trail — 32 hours of Modules and Projects covering all admin fundamentals
- **Earns:** A Trail Completion Badge when all content is done

#### Superbadge
- The most advanced Trailhead content
- Presents a realistic business scenario with minimal guidance
- You must figure out how to build the solution entirely on your own
- Verifies advanced, real-world skill
- **Example:** "Admin Super Set" — must build a complete Salesforce configuration from a business requirements document
- **Earns:** A Superbadge — highly valued by employers
- **Time:** 10–20+ hours each

#### Trailmix
- A custom playlist of content YOU create
- Combine Modules, Projects, and Trails from across Trailhead
- Can be shared with your team as a learning assignment

### Trailhead Ranks

As you earn points (from modules, projects, trails), you advance through ranks:

| Points | Rank |
|--------|------|
| 0 | Trailblazer |
| 500 | Hiker |
| 2,500 | Explorer |
| 5,000 | Adventurer |
| 15,000 | Ranger |
| 50,000 | Double Star Ranger |
| 100,000+ | Triple Star Ranger |

**Ranger** is the most recognized milestone — it's what employers look for on resumes.

### Trailhead Profile

Your Trailhead profile shows:
- Total points earned
- All badges achieved
- Superbadges earned
- Certifications earned (if connected)
- Rank
- Trail completion status

**Share your profile URL:** `trailhead.salesforce.com/me/[your-username]`

Hiring managers for Salesforce roles routinely check Trailhead profiles during recruitment.

### Getting Your Trailhead Playground — Step by Step

1. Go to **trailhead.salesforce.com** and log in (or create a free account)
2. Click any Module or Project that has a hands-on challenge
3. Scroll to the bottom of any Unit → find the **"Launch"** button
4. Click Launch → Salesforce creates a free Playground org for you
5. A new browser tab opens — you are logged into your Playground
6. **IMPORTANT:** Go to the Playground → click your avatar → My Settings → note your username and reset your password so you can log back in directly
7. Your Playground URL: `https://[random].lightning.force.com`

**Managing Multiple Playgrounds:**
- You can have up to **10 Playgrounds** at once
- Click your avatar in Trailhead → Hands-on Orgs → see all your Playgrounds
- You can name them for easy identification (e.g., "Admin Training," "Flow Practice")
- You can **Reset** a Playground to a clean state (erases all customizations — useful between modules)

**When to use a fresh Playground:**
- Each major project or module should ideally start in a clean Playground
- Some modules have pre-configured data — the auto-launched Playground from that module will have the right setup

---

## 1.10 Org-Wide Setup

When you first provision a Salesforce org — whether Production or Sandbox — there is foundational configuration to complete before users start working.

### Company Information

**Path:** Setup → Company Settings → Company Information

This is the foundational identity of your org.

| Setting | What It Controls | Getting It Wrong |
|---------|-----------------|-----------------|
| **Company Name** | Appears in system emails and the org header | Confusing for users if wrong |
| **Primary Contact** | Who Salesforce contacts for billing/support | Important for license renewals |
| **Default Locale** | Date format (MM/DD/YYYY vs DD/MM/YYYY), number format, currency display | Dates show in wrong format globally |
| **Default Language** | Language for all system labels and picklist values | Interface confusing for non-English speakers |
| **Default Time Zone** | Baseline time zone for all timestamps | Activities show in wrong time globally |
| **Currency** | Corporate currency for financial records | Financial data shows in wrong currency |
| **Fiscal Year** | When your business year starts | Quarterly reports are wrong |

#### Deep Dive: Default Locale

The **Default Locale** setting controls:
- **Date format:** `01/04/2026` (US: Jan 4) vs `01/04/2026` (India: April 1) — same numbers, completely different meanings!
- **Number format:** `1,000,000.00` (US) vs `10,00,000.00` (India — lakh format)
- **Currency display:** Where the symbol appears and how decimals are shown
- **Name order:** First + Last (Western) vs Last + First (some Asian locales)

**Real-World Example:**
> "GlobalConsult" set their Default Locale to English (US) but their entire team is in India. Every date field shows in MM/DD/YYYY format. A deadline of "04/05/2026" causes confusion — is that April 5th or May 4th? Reports show revenue in `1,000,000` format instead of `10,00,000`. They should have used English (India) locale from day one.

#### Deep Dive: Fiscal Year

Two options:
- **Standard fiscal year** — matches the calendar year (January–December)
- **Custom fiscal year** — starts on a different month (e.g., April 1 for Indian FY, July 1 for Australian FY)

**Why it matters:**
- "This Quarter" and "This Year" filters in reports use the fiscal year definition
- Quota and forecasting features align to fiscal quarters
- Getting this wrong means all your quarterly reports show the wrong time periods

**Setup:** If your fiscal year doesn't start January 1st, configure this BEFORE adding data. Changing it after data exists requires significant rework.

### Financial Settings

**Path:** Setup → Company Settings → Fiscal Year

For companies with multi-currency needs:

**Path:** Setup → Company Settings → Manage Currencies

| Setting | Description |
|---------|-------------|
| Corporate Currency | Your base/home currency — set once, difficult to change |
| Active Currencies | Additional currencies your reps can use |
| Exchange Rates | Set manually or automatically updated |
| Dated Exchange Rates | Historical rates for accurate period reporting |

**⚠️ Warning:** Once Multi-Currency is enabled, it cannot be disabled. Always enable in Sandbox first and thoroughly test all formula fields, roll-up summaries, and reports before enabling in Production.

### Support Settings

**Path:** Setup → Service → Support Settings

| Setting | Description |
|---------|-------------|
| Default Case Owner | Who gets unassigned cases |
| Default Case Team | Default collaborators on cases |
| Automated Case User | System user for automated case actions |
| Email-to-Case | How support emails become cases |
| Business Hours | When support operates (used by Escalation Rules) |

### Business Hours

**Path:** Setup → Company Settings → Business Hours

Defines when your support team is working. Used by:
- Escalation Rules (SLA timers only count during business hours)
- Entitlement Milestones (response time measured in business hours)

**Real-World Example:**
> "SupportFirst" has Business Hours set to Monday–Friday, 9AM–6PM IST. A High Priority case comes in at 5:55 PM on Friday. The escalation rule says "escalate if not responded to within 4 business hours." Salesforce starts the 4-hour countdown at 9AM Monday — not immediately. So the case won't escalate until 1PM Monday. Without Business Hours configured correctly, the timer would count the weekend.

### Holidays

**Path:** Setup → Company Settings → Holidays

Define holidays that pause SLA timers. If your Escalation Rule uses Business Hours and a holiday falls on a business day — Salesforce subtracts that day from the SLA calculation.

**Example:** Diwali falls on a Tuesday. With Diwali added as a Holiday, your support SLA doesn't count Tuesday — the timer pauses and resumes Wednesday.

---

## ✅ Hands-On Tasks

> All tasks performed in your **Trailhead Playground**. Each task is linked to a specific concept above.

---

### 🔧 Task 1 — Complete Org Setup (Company Information)

**Objective:** Configure company-level settings like a real admin would on Day 1.

**Steps:**
1. Log in to your Trailhead Playground
2. Click the **gear icon (⚙️)** top-right → **Setup**
3. Quick Find → `Company Information` → click result
4. Note existing values — write them down
5. Click **Edit** and change:
   - Company Name: `[Your Name] Training Org`
   - Default Time Zone: `Asia/Kolkata (IST)` (or your actual time zone)
   - Default Locale: `English (India)` (or your actual locale)
6. Click **Save**
7. Navigate to Quick Find → `View Setup Audit Trail`
8. Verify your Company Information change appears at the top of the trail

**Reflection:** What would happen if you set the Default Time Zone incorrectly for a global team?

---

### 🔧 Task 2 — Explore Lightning Experience Navigation

**Objective:** Master the Lightning Experience UI navigation.

**Steps:**
1. Identify each element of the Lightning UI:
   - **App Launcher (⊞)** — top left grid
   - **Navigation bar** — tabs across the top
   - **Global Search** — search bar at top center
   - **Notifications bell** — top right
   - **Help & Training** — question mark icon
   - **Setup gear** — top right
   - **User avatar** — top right

2. Click **App Launcher (⊞)** → observe all available apps
3. Click **Sales** → note the navigation tabs
4. Click **Service** → note how navigation tabs completely change
5. Click **App Launcher** → click **View All** → search for "Jobs" — it won't exist yet

**Create a navigation shortcut:**
6. In the Sales app, click the **pencil icon** next to the navigation bar
7. Add `Accounts` and `Reports` tabs if not already there
8. Rearrange the order
9. Click **Save**

---

### 🔧 Task 3 — Understand the Setup Hierarchy

**Objective:** Navigate Setup like an expert admin.

**Steps:**
1. Go to Setup
2. Explore the main Setup sections by clicking each:
   - **Administration** → Users, Data → Import/Export
   - **Platform Tools** → Objects and Fields → Object Manager
   - **Settings** → Company Settings, Security

3. Use Quick Find to navigate (fastest method):
   - Type `Profiles` → open Profiles
   - Type `Permission Sets` → open Permission Sets
   - Type `Sharing Settings` → open Sharing Settings
   - Type `Flows` → open Flow Builder list

4. Open **Object Manager**:
   - See all Standard Objects listed
   - Click **Account** → explore: Fields & Relationships, Page Layouts, Validation Rules
   - Click **Lead** → compare structure to Account

**Key insight:** Every customization in Salesforce lives somewhere in Setup. Knowing how to navigate Setup quickly is an essential admin skill.

---

### 🔧 Task 4 — Create Records and Understand Record Anatomy

**Objective:** Understand the structure of a Salesforce record.

**Steps:**
1. Sales app → Accounts → **New**
2. Fill in:
   - Account Name: `Infosys Ltd`
   - Phone: `08042742742`
   - Industry: `Technology`
   - Type: `Customer - Direct`
   - Annual Revenue: `₹1,00,000`
3. **Save**

4. On the Account record, identify:
   - **Highlights Panel** (top) — key fields at a glance
   - **Details tab** — all field values
   - **Activity tab** — all tasks, events, emails, calls
   - **Related tab** — related Contacts, Opportunities, Cases

5. **Add a Contact:**
   - Related → Contacts → **New Contact**
   - First Name: `Salil`, Last Name: `Parekh`
   - Title: `CEO`
   - Email: `salil@infosys.com`
   - Save

6. **Log an Activity:**
   - Activity tab → **Log a Call**
   - Subject: `Initial discussion about Salesforce implementation`
   - Comments: `Very interested. Wants a demo next week.`
   - Save
   - Observe the call in the Activity Timeline

7. **Create an Opportunity:**
   - Related → Opportunities → **New**
   - Opportunity Name: `Infosys — Salesforce Implementation`
   - Close Date: 3 months from today
   - Stage: `Prospecting`
   - Amount: `₹50,00,000`
   - Save

**Observe:** The Opportunity is now in the Related section of the Account. The Account shows a related Opportunity AND a related Contact AND an Activity — all connected.

---

### 🔧 Task 5 — Explore Salesforce Mobile

**Objective:** Understand Salesforce's mobile capabilities.

**Steps:**
1. On your smartphone, download **Salesforce** from the App Store or Play Store
2. Open the app → Log in with your Playground credentials
3. Navigate to your Infosys Account
4. Log a new call from your phone
5. Create a Task from mobile

**Observe:**
- The mobile app is fully functional — not just a view
- Any change made on mobile immediately reflects on the desktop version
- This is possible because everything is stored in Salesforce's cloud — not on your device

---

### 🔧 Task 6 — Trailhead Badge Hunt

**Objective:** Earn your first Trailhead badge.

**Steps:**
1. Go to trailhead.salesforce.com
2. In Search, find: **"Salesforce Platform Basics"** module
3. Open the module — read through each unit
4. Complete the quiz at the end of each unit
5. Complete the final Challenge
6. Check your profile — the badge should appear

**Then:** Find and complete: **"CRM for Lightning Experience"** module

**Goal:** Earn at least 2 badges by the end of this topic.

---

### 🔧 Task 7 — Review Salesforce Trust Status

**Objective:** Understand how to check Salesforce's operational status.

**Steps:**
1. Visit **trust.salesforce.com**
2. Find your org's **instance** — go to Setup → Company Information → look for the "Instance" field (e.g., NA100, AP28)
3. On Trust, click your instance
4. See the 90-day maintenance history
5. Bookmark this page — you will use it whenever users report Salesforce performance issues

---

### 🔧 Task 8 — Explore AppExchange

**Objective:** Understand the Salesforce app ecosystem.

**Steps:**
1. Visit **appexchange.salesforce.com**
2. Browse categories — notice the variety (Marketing, Finance, HR, Industry-specific)
3. Search for `DocuSign` — read the listing
4. Search for `Conga` — read the listing
5. Search for `OwnBackup` — read the listing
6. Answer: What is the difference between a **Free** and **Paid** listing on AppExchange?
7. Find a free app relevant to job recruiting — what does it do?

**Key insight:** Before building a custom solution, always check AppExchange. There may already be a free or affordable app that does exactly what you need.

---

## 📝 Practice Questions

> Answer from memory first. Then verify against the notes. All questions are exam-relevant.

**Q1.** What does CRM stand for? List three specific business problems CRM solves that spreadsheets cannot.

**Q2.** Who founded Salesforce and in what year? What was their revolutionary business model called?

**Q3.** A company wants to integrate Salesforce with their SAP ERP system to automatically sync orders. Which Salesforce product/cloud handles enterprise integrations?

**Q4.** What Salesforce Edition is the minimum required to access the Salesforce API?

**Q5.** Explain Salesforce's multitenant architecture using an analogy that a non-technical business manager would understand.

**Q6.** What is the difference between a Full Sandbox and a Partial Sandbox? When would you use each?

**Q7.** A company's fiscal year starts on April 1. Why is it critical to configure the Fiscal Year in Salesforce correctly before data is entered?

**Q8.** Salesforce releases updates three times a year. What is the purpose of the "Preview Sandbox" period, and how long does it last before Production is updated?

**Q9.** What is the difference between a Trailhead **Module** and a **Project**?

**Q10.** What is a Superbadge, and how does it differ from a regular Trailhead badge?

**Q11.** What does **trust.salesforce.com** provide, and when would an admin use it?

**Q12.** A company enables Salesforce Multi-Currency. Two months later, they realize it was a mistake and want to disable it. What happens?

**Q13.** What is the maximum number of Trailhead Playgrounds one Trailhead account can have simultaneously?

**Q14.** A company has a Developer Sandbox. Their admin makes 200 configuration changes over 3 months. The admin then refreshes the sandbox. What happens to those 200 changes?

**Q15.** What is the Salesforce **AppExchange**, and why should an admin check it before building a custom solution?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — CRM Business Case**

> You are the IT Manager at "MediEquip" — a medical equipment distributor with 80 employees. Currently:
> - 15 sales reps each have their own Excel files with customer lists
> - Support tickets are managed through personal email inboxes
> - Marketing sends campaigns from a standalone email tool with no connection to sales data
> - The Sales Director produces a monthly pipeline report by emailing all reps every Friday and manually compiling their replies into a master Excel sheet — takes 4 hours every week
>
> **Question:**
> 1. Identify 5 specific business problems this creates
> 2. For each problem, explain specifically how Salesforce solves it
> 3. Which Salesforce Clouds would you recommend and why?
> 4. The CEO asks: "What's the ROI?" — give 3 quantifiable arguments

---

**Scenario 2 — Edition Selection**

> "EduTech Startup" is launching a B2B sales operation. Current situation:
> - 6 sales reps, expecting to grow to 25 within 18 months
> - Need: Lead tracking, Opportunity management, email integration
> - Plan: In 12 months, integrate Salesforce with their custom LMS (Learning Management System) via REST API
> - Budget: Tight — want the cheapest option that will still work
>
> **Questions:**
> 1. Which Edition should they start with and why?
> 2. What specific limitation of that Edition will they hit when they try to integrate with their LMS?
> 3. What Edition will they need to upgrade to, and what is the key difference?
> 4. Is there a risk of upgrading mid-year vs at contract renewal? Explain.

---

**Scenario 3 — Environment Strategy**

> "RetailBank" is implementing a complex Salesforce project — a custom loan origination system. They have:
> - 5,000 production users
> - 2 million loan records in Production
> - A 6-developer team
> - A 10-person QA team
> - 200 business users for UAT
> - A hard go-live deadline in 90 days
>
> **Question:** Design the complete environment strategy:
> 1. What environments would you request and why?
> 2. Who works in each environment?
> 3. What is the deployment promotion path from development to Production?
> 4. How do you handle the UAT environment needing realistic data without using real customer PII?

---

**Scenario 4 — Release Management Crisis**

> It is April 15, 2026. The "Summer '26" Sandbox Preview was released on April 1. Your team did NOT test in the Preview Sandbox. Production gets the Summer '26 update on June 5, 2026. On June 6th, users start reporting that 5 critical Flows are failing and a key approval process is sending notifications to the wrong people.
>
> **Question:**
> 1. Who is responsible for this situation and why?
> 2. What specific steps should have been taken in April and May to prevent this?
> 3. What is the immediate remediation plan for June 6th?
> 4. What process change would you implement to prevent this from happening again?

---

**Scenario 5 — Org Setup Decisions**

> "GlobalTrade" is a trading company with offices in India (HQ), UAE, UK, and Singapore. They are setting up Salesforce for the first time. They need to make these configuration decisions:
>
> - Corporate currency: INR (but reps in UAE deal in AED, UK in GBP, Singapore in SGD)
> - Company works Monday–Friday but UAE works Sunday–Thursday
> - Financial year starts April 1
> - Some data contains personal information of EU customers (GDPR)
> - UK office is 5.5 hours behind India; Singapore is 2.5 hours ahead
>
> **Question:** For each of the following settings, what would you configure and explain your reasoning:
> 1. Default Currency and Multi-Currency
> 2. Business Hours (and how to handle multiple time zone offices)
> 3. Fiscal Year
> 4. Default Time Zone and per-user Time Zone settings
> 5. GDPR data concerns — what Salesforce feature addresses this?

---

## ✔️ Answer Key — Practice Questions

1. CRM = Customer Relationship Management. Three problems it solves that spreadsheets cannot: (1) Centralized customer data — a rep leaving doesn't take customer knowledge with them; (2) Real-time visibility — manager sees live pipeline without asking each rep; (3) Cross-team context — support sees sales promises, marketing sees conversion rates from campaigns.

2. **Marc Benioff**, founded in **1999**. The revolutionary model was called **SaaS (Software as a Service)** — subscription-based, browser-accessed, no installation — branded as **"No Software"**.

3. **MuleSoft** — Salesforce's integration platform for connecting Salesforce to external systems via APIs. For lighter integrations, Salesforce also has built-in connectors and the Salesforce Connect feature.

4. **Enterprise Edition** — Professional Edition does NOT include API access. Enterprise is the minimum edition for API integration with external systems.

5. Multitenancy is like an apartment building: every tenant (company) has their own private apartment (their org, their data, their customizations) — completely locked and private. But they all share the building's infrastructure (Salesforce's servers, security team, maintenance team). When the building upgrades the elevator (Salesforce releases a new version), all tenants benefit automatically without doing anything themselves. One tenant's plumbing problem doesn't affect other tenants' apartments.

6. **Full Sandbox** = exact copy of Production data — every record, every file. Used for final UAT where users need to work with real data volumes. Refreshes every 29 days. More expensive. **Partial Sandbox** = copy of Production configuration + a configurable subset of records (e.g., 5% of Accounts). Used for integration testing and performance testing where you need realistic but not full data volumes. Refreshes every 5 days. Less storage, faster to set up.

7. If the Fiscal Year is configured incorrectly (e.g., as calendar year January–December instead of April–March), then Salesforce's "This Quarter," "Last Quarter," "This Year," and "Last Year" date filters in reports will show the wrong time periods. Forecasting, quota attainment, and seasonal analysis will all be wrong. Fixing this after data exists requires reviewing and potentially rebuilding all reports and dashboards.

8. The Preview Sandbox period means your sandbox receives the new release approximately **4–6 weeks before Production**. This gives you a testing window to validate your customizations against the new platform version before it affects your live users. For Summer '26, Production updates in June — sandbox Preview available from April.

9. A **Module** is self-paced reading with quizzes — no hands-on org work required to earn the badge (just pass the quiz). A **Project** requires you to perform actual configuration steps in a real Salesforce org. Trailhead automatically verifies your org's configuration to confirm you built what was asked.

10. A **Superbadge** represents advanced real-world skill. Unlike a regular badge (which tests a specific, guided topic), a Superbadge gives you a business scenario with minimal instructions — you must figure out the solution yourself. Superbadges take 10–20+ hours and are highly valued by employers as proof of practical ability.

11. **trust.salesforce.com** is Salesforce's public system status page. It shows: real-time status of all Salesforce services by instance, scheduled maintenance windows, past incident history. An admin uses it when users report Salesforce is slow or unavailable — checking Trust confirms whether it's a Salesforce platform issue or a local/custom problem.

12. **Multi-Currency cannot be disabled once enabled.** This is a permanent, irreversible change to the org. All records that had a currency field now have a currency value, and removing Multi-Currency would make all that data inconsistent. This is why Multi-Currency should always be enabled in a Sandbox first, thoroughly tested, and only enabled in Production after confirming all impacts.

13. Up to **10 Trailhead Playgrounds** per Trailhead account simultaneously.

14. The 200 configuration changes are **permanently lost**. Refreshing a sandbox overwrites it with a new snapshot of Production. Any changes made in the sandbox that were not deployed to Production are gone. This is why the deployment process exists — always promote your sandbox changes to Production (or at least document them) before refreshing.

15. The **AppExchange** is Salesforce's marketplace of pre-built applications, components, and consulting services — over 7,000 listings. An admin should check AppExchange before building a custom solution because there may already be a free or affordable app that solves the need — saving weeks of development time and ongoing maintenance costs.

---

## 🔗 Trailhead Resources

- [Salesforce Free Trials](https://developer.salesforce.com/free-trials)
- [Trailhead Playground Management](https://trailhead.salesforce.com/content/learn/modules/trailhead_playground_management)
- [Company-Wide Org Settings](https://trailhead.salesforce.com/content/learn/modules/company_wide_org_settings)
- [Salesforce Platform Basics](https://trailhead.salesforce.com/content/learn/modules/salesforce_platform_basic)
- [CRM for Lightning Experience](https://trailhead.salesforce.com/content/learn/modules/crm_for_lightning_experience)

---

*Next: [02 · Salesforce Licenses & User Management →](./02_Licenses_and_User_Management.md)*
