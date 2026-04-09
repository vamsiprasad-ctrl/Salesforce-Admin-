# 10 · Service Cloud & Knowledge

> **Module:** Service Cloud | **Difficulty:** 🟡 Intermediate | **Est. Time:** 7 hrs

---

## 10.1 What is Service Cloud?

**Service Cloud** is Salesforce's customer support platform. It transforms Salesforce into a complete help desk — routing customer issues to the right agents, tracking resolution, enforcing SLAs, and giving agents everything they need in one screen.

The central object is the **Case** — every customer issue becomes a Case.

---

## 10.2 Cases

A **Case** records a customer support issue from the moment it's raised until it's resolved.

### Case Key Fields

| Field | Purpose |
|-------|---------|
| Subject | Short description of the issue |
| Description | Full details |
| Status | New, Working, Escalated, Pending Customer, Closed |
| Priority | High, Medium, Low |
| Case Origin | How it came in — Email, Web, Phone, Chat |
| Account / Contact | Who raised it |
| Owner | Assigned agent or queue |
| Case Number | Auto-generated unique identifier |

### Real-World Example

> **Scenario:** "CloudSoft" customer Acme Corp calls support: "Our login page is broken since this morning."
>
> Support agent creates a Case:
> - Subject: "Login page not loading — Acme Corp"
> - Priority: High (enterprise customer)
> - Case Origin: Phone
> - Account: Acme Corp
> - Contact: John Smith (caller)
> - Status: Working
> - Owner: Tier 1 Support Queue → auto-assigned to next available agent

---

## 10.3 Support Process

A **Support Process** defines the **Status picklist values** available for Cases on a given Record Type. Different case types can have different workflow stages.

**Path:** Setup → Support Processes → New

### Real-World Example

> **"CloudSoft" has two support processes:**
>
> **Technical Support Process** (Status values):
> New → Investigating → Replication Confirmed → Fix In Progress → Testing → Resolved → Closed
>
> **Billing Support Process** (Status values):
> New → Under Review → Awaiting Finance → Adjustment Applied → Closed
>
> A billing case doesn't go through "Replication Confirmed" — it goes through "Awaiting Finance." Different processes, same object.

---

## 10.4 Case Queues

A **Queue** is a holding area where cases wait to be claimed by an available agent — instead of being assigned to a specific person.

**Path:** Setup → Queues → New

### Real-World Example

> **"CloudSoft" Support Queues:**
> - `Tier1_Support_Queue` — general product questions; all Tier 1 agents are members
> - `Tier2_Technical_Queue` — bugs and technical issues; senior engineers only
> - `Enterprise_VIP_Queue` — issues from Enterprise-tier customers; dedicated team
>
> **Assignment Logic (via Assignment Rule):**
> - Case Priority = Low/Medium AND Account Type ≠ Enterprise → Tier1_Support_Queue
> - Case Priority = High → Tier2_Technical_Queue
> - Account Type = Enterprise → Enterprise_VIP_Queue (regardless of priority)
>
> Agents open the queue and "Accept" the next case in line — like taking a ticket.

---

## 10.5 Case Teams

A **Case Team** allows multiple people to collaborate on a single complex case — each with a defined role and access level.

**Path:** Setup → Case Teams → New Team Role

### Real-World Example

> **Case:** "Acme Corp — Data Migration Failure" (complex enterprise issue)
>
> **Case Team:**
> - **Lead Agent (Read/Write):** Sarah — primary contact for customer
> - **Technical Specialist (Read/Write):** Dev who owns the affected feature
> - **Account Manager (Read Only):** James — relationship owner, monitoring
> - **Legal (Read Only):** Rohan — in case SLA breach triggers contract clause

---

## 10.6 Assignment Rules

**Case Assignment Rules** automatically route new or updated cases to the right owner or queue based on criteria.

**Path:** Setup → Case Assignment Rules → New

> Only **one assignment rule** can be active at a time. That rule can have multiple rule entries ordered by priority.

### Real-World Example

> **Assignment Rule:** "CloudSoft Case Routing"
>
> | Order | Criteria | Assign To |
> |-------|----------|-----------|
> | 1 | Account.Type = Enterprise | Enterprise_VIP_Queue |
> | 2 | Priority = High AND Origin = Email | Tier2_Technical_Queue |
> | 3 | Subject contains "billing" OR "invoice" | Billing_Queue |
> | 4 | Default (catch-all) | Tier1_Support_Queue |
>
> Cases are evaluated top to bottom — first matching rule wins.

---

## 10.7 Escalation Rules

**Escalation Rules** automatically escalate cases that haven't been responded to or resolved within a defined time period — enforcing SLAs.

**Path:** Setup → Escalation Rules → New

### Real-World Example

> **Escalation Rule:** "Priority-Based SLA"
>
> | Priority | Hours Before Escalation | Action |
> |----------|------------------------|--------|
> | High | 4 hours | Reassign to Senior Agent Queue + Email manager |
> | Medium | 24 hours | Email agent's supervisor |
> | Low | 72 hours | Notify team lead |
>
> **Practical scenario:** It's Friday afternoon. A High Priority case comes in at 4:00 PM. The assigned agent leaves for the weekend. By 8:00 PM, the escalation rule fires — the case moves to the Senior Agent Queue and the support manager gets an email. The customer isn't left waiting until Monday.

---

## 10.8 Web-to-Case

**Web-to-Case** captures customer issues submitted through a form on your website and automatically creates a Case.

**Path:** Setup → Self-Service → Web-to-Case

Similar to Web-to-Lead — Salesforce generates HTML you embed on your site. Form submissions become Cases automatically.

### Real-World Example

> **"CloudSoft" Help Center Website:**
> - Customer fills out: Name, Email, Subject, Description, Severity
> - Case created instantly in Salesforce
> - Auto-response email sent to customer: "Your case #00001234 has been received. We'll respond within 4 hours."
> - Assignment Rule routes it to the correct queue
> - Agent picks it up and customer is updated — all without a phone call

---

## 10.9 Email-to-Case

**Email-to-Case** converts inbound support emails into Cases automatically — preserving the full email thread on the Case record.

### Two Modes

| Mode | How It Works | Best For |
|------|-------------|---------|
| **Email-to-Case (standard)** | Salesforce polls your support inbox periodically | Lower volume inboxes |
| **On-Demand Email-to-Case** | Your email server forwards to Salesforce in real time | High volume, mission-critical |

**Path:** Setup → Email-to-Case

### Real-World Example

> **"CloudSoft" support email:** `support@cloudsoftapp.com`
>
> Customer Sarah sends: "Hi, I can't export my reports. The button is greyed out. Please help."
>
> **What Salesforce does automatically:**
> 1. Creates Case: Subject = "Can't export reports — button greyed out"
> 2. Sets Case Origin = Email
> 3. Links to Sarah's Contact record (matched by email address)
> 4. Populates Description with the email body
> 5. Applies Assignment Rule → routes to Tier1 queue
> 6. Sends auto-response: "Hi Sarah, we received your request. Case #00001235..."
>
> When the agent replies from within Salesforce, the reply sends from `support@cloudsoftapp.com` and the thread is logged on the Case — customers never know the agent is in Salesforce.

---

## 10.10 Entitlements & Milestones

**Entitlements** define what level of support a customer is contractually entitled to — their SLA.

**Milestones** are required steps within an entitlement process with time targets (e.g., "First Response within 1 hour").

### Real-World Example

> **"CloudSoft" Support Tiers:**
>
> | Tier | First Response | Resolution | Business Hours |
> |------|---------------|-----------|----------------|
> | Standard | 8 hours | 5 business days | Mon-Fri 9-5 |
> | Premium | 2 hours | 1 business day | Mon-Fri 7-7 |
> | Enterprise | 30 min | 4 hours | 24/7/365 |
>
> Acme Corp (Enterprise) submits a High Priority case at 11:00 PM Saturday.
> - **Milestone 1:** First Response → Agent must respond by **11:30 PM** (30 min)
> - **Milestone 2:** Resolution → Issue must be resolved by **3:00 AM** (4 hours)
>
> If milestones are breached, an alert fires and escalation rules kick in.

---

## 10.11 Lightning Service Console

The **Lightning Service Console** is a specialized app layout built for high-volume support agents — optimized for multi-tasking.

### Features

| Feature | What it does |
|---------|-------------|
| **Split View** | Case list on the left, case detail on the right — no page-to-page navigation |
| **Multi-Tab Navigation** | Open 10+ cases at once in browser-like tabs |
| **Utility Bar** | Quick-access tools at the bottom (Macros, Phone, Notes, Knowledge) |
| **Highlights Panel** | Key case info always visible at the top |
| **Activity Timeline** | Full history of all interactions with the customer |
| **Knowledge Integration** | Suggested articles appear automatically as agent types |

### Real-World Example

> **Agent Maria's workday without the Console:**
> - Opens a case → clicks to Account to check history → clicks back → loses her place → repeats for 50 cases/day
>
> **Agent Maria's workday with Console:**
> - Case list on left — clicks to open cases in tabs (never leaves the page)
> - Types case subject → suggested Knowledge Articles appear on the right
> - Clicks article → attaches it to case → closes in 3 minutes
> - Macro: one click sends standard response, updates status, and logs activity
> - 50 cases/day becomes 80 cases/day with higher customer satisfaction

---

## 10.12 Salesforce Knowledge

**Knowledge** is Salesforce's built-in knowledge base — a library of articles that agents and customers use to find solutions quickly.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Article** | A document in the knowledge base (FAQ, how-to, troubleshooting guide) |
| **Article Type / Record Type** | Categorizes articles (FAQ, Solution, Product Manual) with their own fields and layouts |
| **Data Category** | A hierarchical taxonomy for filtering and controlling article visibility |
| **Article Visibility** | Internal (agents only), Partner, Customer, Public (no login required) |
| **Article Lifecycle** | Draft → Review → Published → Archived |
| **Knowledge User** | A feature license required to create/manage articles |

### Data Categories

Data Categories are hierarchical and shared across Knowledge and Experience Cloud.

### Real-World Example

> **"CloudSoft" Knowledge base structure:**
>
> **Data Category Group:** Product Area
> - **Category:** Reports & Dashboards
>   - Sub-category: Custom Report Types
>   - Sub-category: Dashboard Filters
> - **Category:** Integrations
>   - Sub-category: REST API
>   - Sub-category: Connected Apps
>
> **Article Visibility:**
> - "How to reset your password" → Public (no login required)
> - "API Rate Limits Reference" → Customer (must be logged into Community)
> - "Internal Escalation Playbook" → Internal Only (agents only)

---

## 10.13 Auto-Suggestion of Knowledge Articles

When an agent is working a Case, Salesforce can **automatically suggest relevant Knowledge Articles** based on the Case subject — without the agent having to search manually.

### Real-World Example

> Agent opens Case: Subject = "Cannot export reports to Excel"
>
> Knowledge panel on the right automatically shows:
> 1. "Troubleshooting Report Export Issues" ⭐ 4.8/5
> 2. "Supported Browsers for Report Export"
> 3. "How to Export Reports to CSV"
>
> Agent attaches the first article to the Case and shares it with the customer. Case resolved in 2 minutes instead of 20.

---

## 10.14 Macros

A **Macro** is a set of instructions that perform multiple actions with a single click — designed to speed up repetitive agent tasks.

### Real-World Example

> **Macro: "Standard Close — Issue Resolved"**
>
> When clicked, the macro automatically:
> 1. Selects an email template: "Issue Resolution Confirmation"
> 2. Fills in the customer's name and case number
> 3. Sends the email
> 4. Sets `Status = "Closed"`
> 5. Logs an activity: "Case closed via standard resolution"
>
> What used to take an agent 5 minutes of clicking now happens in 3 seconds. Across 100 cases a day, that's 8+ hours saved daily.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Case | Central service object — tracks one customer issue end to end |
| Support Process | Defines status values for a case type |
| Queue | Holding pool for unassigned cases |
| Case Team | Multiple agents collaborating on one complex case |
| Assignment Rule | Auto-routes cases to correct queue/agent by criteria |
| Escalation Rule | Auto-escalates cases that breach SLA time targets |
| Email-to-Case | Converts inbound emails to Cases automatically |
| Entitlement | SLA contract defining response/resolution commitments |
| Lightning Console | Productivity-optimized UI for high-volume agents |
| Knowledge Article | Searchable solution document — reduces handle time |
| Macro | One-click automation of multi-step agent tasks |

---

## 🔗 Trailhead Resources

- [Email-to-Case in Salesforce](https://www.apexhours.com/email-to-case-in-salesforce/)
- [Set Up Case Escalation & Entitlements](https://trailhead.salesforce.com/content/learn/projects/set-up-case-escalation-entitlements)
- [Lightning Knowledge Basics](https://trailhead.salesforce.com/content/learn/modules/lightning-knowledge-basics)
- [Macros Quick Look for Agents](https://trailhead.salesforce.com/content/learn/modules/macros-quick-look-for-agents)
- [Knowledge Search Basics](https://trailhead.salesforce.com/content/learn/modules/knowledge_search_basics)
- [Set Up Salesforce Knowledge](https://trailhead.salesforce.com/content/learn/projects/set-up-salesforce-knowledge)

---

*Previous: [09 · Sales Cloud](./09_Sales_Cloud.md)*
*Next: [11 · Experience Cloud →](./11_Experience_Cloud.md)*
