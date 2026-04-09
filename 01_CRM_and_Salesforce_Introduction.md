# 01 · CRM & Salesforce Introduction

> **Module:** Getting Started | **Difficulty:** 🟢 Beginner | **Est. Time:** 2.5 hrs

---

## 1.1 What is CRM?

**CRM** stands for **Customer Relationship Management**. It is a strategy, process, and technology that companies use to manage interactions with current and potential customers — across the entire lifecycle from first contact to long-term loyalty.

Before CRM existed, companies stored customer data in spreadsheets, email inboxes, paper notebooks, or inside individual sales reps' heads. The result: when a rep left the company, all their customer knowledge left with them. When a customer called support, the agent had no idea what the sales team had promised. Data was siloed and invisible.

**CRM solves this** by creating a single source of truth — one place where Sales, Marketing, and Support all see the same customer.

### Real-World Example

> **Company:** TechBridge — a mid-sized software company
>
> **Before CRM:** Sarah (sales rep) tracks leads in a personal Excel sheet. When she goes on leave, her manager has no idea which deals are close to closing. A customer calls support angry because the sales rep promised a discount — but support sees nothing about it.
>
> **After Salesforce:** Every lead, call, email, and deal is logged centrally. Sarah's manager sees the pipeline in real time. Support opens the case and immediately sees "Sales rep promised 15% discount on 3-year contract." Customer is happy. Deal closes.

---

## 1.2 What is Salesforce?

Salesforce is the world's **#1 cloud-based CRM platform**, founded in 1999 by Marc Benioff. It pioneered the **SaaS (Software as a Service)** model — no software to install, no servers to manage, access via browser from anywhere.

### Salesforce vs a Regular Database

| Regular Database | Salesforce |
|-----------------|------------|
| Stores raw data | Data + workflows + automation + analytics |
| Developers must query data | Business users click, not code |
| No built-in communication tools | Built-in email, activity tracking, approvals |
| Manual updates | Three automatic upgrades per year |
| No app marketplace | AppExchange with 7,000+ pre-built apps |

---

## 1.3 Salesforce Products & Clouds

| Cloud | What It Does | Who Uses It |
|-------|-------------|-------------|
| **Sales Cloud** | Leads, Opportunities, Accounts, pipeline management | Sales teams |
| **Service Cloud** | Cases, knowledge base, SLA tracking | Support / CS teams |
| **Marketing Cloud** | Email campaigns, journeys, social | Marketing teams |
| **Experience Cloud** | External portals for customers, partners | IT / Admins |
| **Commerce Cloud** | Online storefronts B2B and B2C | eCommerce |
| **Analytics Cloud** | Advanced AI-powered data visualization | Analysts |
| **Platform (Force.com)** | Build custom apps on Salesforce infrastructure | Developers / Admins |

---

## 1.4 Salesforce Editions

| Edition | Target | Key Limit |
|---------|--------|-----------|
| **Essentials** | Small businesses ≤10 users | Basic CRM only |
| **Professional** | Growing businesses | No API access |
| **Enterprise** | Mid-large companies | Full customization + API |
| **Unlimited** | Large enterprises | Max sandboxes, 24/7 support |
| **Developer Edition** | Learning and dev | Free, full features, limited storage |
| **Trailhead Playground** | Training | Free, pre-wired to Trailhead |

---

## 1.5 Multitenant Architecture

**Multitenancy** = multiple companies (tenants) share the same Salesforce infrastructure, but each company's data is completely isolated — like apartments in a building sharing plumbing but having private locks.

### Why It Matters

| Benefit | Explanation |
|---------|-------------|
| Automatic upgrades | 3 releases/year pushed to everyone simultaneously |
| Lower cost | Infrastructure shared across all customers |
| Instant scalability | No servers to buy; Salesforce handles spikes |
| Data isolation | Despite shared infrastructure, your data is private |

### Real-World Example

> Coca-Cola (50,000 users) and a 5-person startup both run on the same Salesforce infrastructure. Both get the Spring '26 update on the same day. Neither sees the other's data. Neither had to do anything — it just happened.

---

## 1.6 Salesforce Environments

| Environment | Purpose | Data | Cost |
|-------------|---------|------|------|
| **Production** | Live business operations | Real customer data | Paid |
| **Full Sandbox** | Exact production copy + data | Full copy | Paid add-on |
| **Partial Sandbox** | Config + sample data subset | Subset | Paid add-on |
| **Developer Sandbox** | Config copy only | No data | Included |
| **Developer Edition** | Standalone free learning org | Sample data | Free |
| **Trailhead Playground** | Pre-configured for exercises | Sample data | Free |
| **Scratch Org** | Temporary, source-driven CI/CD | Empty | Free with Dev Hub |

### Real-World Example — Why Sandboxes Exist

> RetailCo wants to implement a new approval process. The correct path:
> 1. **Developer Sandbox** — admin builds and tests alone
> 2. **Partial Sandbox** — QA tests with realistic data volumes
> 3. **Full Sandbox** — UAT with real users on a full copy of production
> 4. **Production** — deploy after all testing passes
>
> ❌ Never build directly in Production — bugs in automation break live customer operations.

---

## 1.7 The Salesforce Release Cycle

| Release | Timing | Sandbox Preview |
|---------|--------|-----------------|
| **Spring** | February | November |
| **Summer** | June | April |
| **Winter** | October | August |

Your sandbox gets the update ~4 weeks before production — always test during this window.

---

## 1.8 Trailhead — Your Learning Platform

| Content Type | Description | Earns |
|-------------|-------------|-------|
| **Module** | Self-paced units with quizzes | Badge |
| **Project** | Hands-on steps in a real org | Badge |
| **Trail** | Curated series of modules | Trail Badge |
| **Superbadge** | Real-world scenario, no instructions | Superbadge |

### Getting Your Trailhead Playground

1. Log in at [trailhead.salesforce.com](https://trailhead.salesforce.com)
2. Open any Module or Project with a hands-on challenge
3. Scroll to the bottom → click **"Launch"**
4. Playground is created — save the username and password
5. You can have up to **10 Playgrounds** at a time

---

## 1.9 Org-Wide Setup — Company Information

**Path:** Setup → Company Settings → Company Information

| Field | Example |
|-------|---------|
| Company Name | TechBridge Pvt Ltd |
| Default Locale | English (India) |
| Default Language | English |
| Default Time Zone | Asia/Kolkata (IST) |
| Currency | INR |
| Fiscal Year | Custom — starts April 1 |

---

## ✅ Hands-On Tasks

> Complete these tasks in your **Trailhead Playground**. Each task reinforces a concept from this topic.

---

### 🔧 Task 1 — Explore Your Org's Setup

**Objective:** Navigate Setup and understand company-level configuration.

**Steps:**
1. Log in to your Trailhead Playground
2. Click the **gear icon (⚙️)** top-right → select **Setup**
3. In the Quick Find box, type `Company Information` → click the result
4. Note the following fields and write them down:
   - Company Name
   - Default Locale
   - Default Time Zone
   - Primary Contact

**Expected Outcome:** You can navigate Setup and read org configuration settings.

**Try this:** Change the Default Time Zone to `America/New_York` → Save → Switch back to your original time zone → Save.

---

### 🔧 Task 2 — Explore the App Launcher

**Objective:** Understand how apps and tabs are organized in Lightning Experience.

**Steps:**
1. Click the **grid icon (⊞)** (App Launcher) in the top-left
2. Browse all available apps — note the names (Sales, Service, etc.)
3. Click **Sales** to open the Sales app
4. Explore the navigation tabs: Accounts, Contacts, Leads, Opportunities
5. Go back to App Launcher → open the **Service** app
6. Notice how the navigation tabs change completely

**Question to answer:** What tabs are available in Sales that are not in Service?

---

### 🔧 Task 3 — Create Your First Record

**Objective:** Understand the basic anatomy of a Salesforce record.

**Steps:**
1. In the Sales app, click the **Accounts** tab
2. Click **New**
3. Fill in:
   - Account Name: `Practice Company`
   - Phone: `9876543210`
   - Industry: `Technology`
   - Type: `Prospect`
4. Click **Save**
5. Observe the record — notice the sections: Details, Activity, Related

**Explore:** Click the **Activity** tab on the account. Click **New Task** → create a task "Follow up call" due tomorrow. Save. The task now appears in the activity timeline.

---

### 🔧 Task 4 — Navigate Setup Audit Trail

**Objective:** Understand how Salesforce tracks configuration changes.

**Steps:**
1. Setup → Quick Find: `View Setup Audit Trail`
2. Click **View Setup Audit Trail**
3. Read the last 10 entries — each shows: Date, User, Section, Action
4. Now go back and change Company Information (e.g., change Default Locale)
5. Return to Audit Trail → your change should appear at the top

**Expected Outcome:** Understand that every Setup change is permanently logged.

---

### 🔧 Task 5 — Compare Editions (Research Task)

**Objective:** Understand the commercial differences between Salesforce editions.

**Steps:**
1. Visit [salesforce.com/editions-pricing/sales-cloud](https://www.salesforce.com/editions-pricing/sales-cloud/)
2. Compare Professional vs Enterprise editions
3. Answer:
   - Which edition first allows API access?
   - Which edition includes unlimited custom apps?
   - What feature is exclusive to Unlimited Edition?

---

## 📝 Practice Questions

> Answer these from memory first, then verify by checking the notes.

**Q1.** What does CRM stand for, and what business problem does it solve?

**Q2.** What is the difference between a **Developer Edition** and a **Trailhead Playground**?

**Q3.** Salesforce releases updates how many times per year? Name all three release seasons.

**Q4.** What is the key benefit of Salesforce's **multitenant architecture** for end customers?

**Q5.** A company is about to implement a new automation process. They want to test it with full production data before going live. Which **sandbox type** should they use?

**Q6.** What is the difference between a **Full Sandbox** and a **Partial Sandbox**?

**Q7.** A Trailhead badge is earned by completing which type of content — a Module, a Project, or both?

**Q8.** What field in Company Information controls how dates appear (e.g., MM/DD/YYYY vs DD/MM/YYYY) for all users by default?

**Q9.** What is a **Scratch Org**, and how is it different from a Developer Sandbox?

**Q10.** If a company's Salesforce sandbox gets the Summer '26 update in April, when will their Production org receive the same update?

---

## 🎯 Scenario-Based Questions

> These questions test your ability to apply concepts to real business situations.

---

**Scenario 1 — Choosing the Right Environment**

> ABC Healthcare is live on Salesforce with 5,000 patient records. Their admin has built a new Flow that automatically reassigns Cases when an agent goes on leave. Before deploying, the testing team needs to verify the Flow works correctly with realistic data volumes — at least 2,000 records. A full data copy is not needed.
>
> **Question:** Which sandbox type is most appropriate for this testing, and why? What would be the risk of testing in a Developer Sandbox instead?

---

**Scenario 2 — Multitenant Architecture**

> During a Salesforce release window, the finance team at "InsureCo" hears that "Salesforce is doing maintenance." They worry their custom workflows might be affected. The CEO asks: "Do other companies' releases affect our org? Could another customer's bug break our system?"
>
> **Question:** How would you explain Salesforce's multitenant architecture to the CEO in non-technical terms? Address their specific concern about other companies' issues affecting them.

---

**Scenario 3 — Edition Decision**

> A startup with 8 users wants to use Salesforce for basic lead tracking and contact management. They don't need any API integrations today but may want them within 2 years. Their budget is tight.
>
> **Question:** Which Salesforce Edition would you recommend now, and what would you recommend they plan for in 2 years? What is the key limitation of the lower edition they should be aware of?

---

**Scenario 4 — Release Cycle Planning**

> "RetailMax" has a critical customization — a custom formula field — that they're worried might break during the upcoming Summer release. They want to test it before it impacts their 500 users.
>
> **Question:** Walk through the steps they should take to safely test before the Production release. When should they start testing? What environment should they use?

---

**Scenario 5 — CRM Value Justification**

> The CEO of a 50-person consulting firm asks: "We've been managing client relationships in Excel and Outlook for 10 years. It works. Why should we spend money on Salesforce?"
>
> **Question:** Build a case for CRM adoption using 3 specific business problems that Excel/Outlook cannot solve but Salesforce can. Use real examples relevant to a consulting firm.

---

## ✔️ Answer Key — Practice Questions

1. Customer Relationship Management. It centralizes customer data across sales, service, and marketing — eliminating silos and lost knowledge when staff leave.
2. Both are free with full features. A Developer Edition is a standalone org. A Trailhead Playground is also a Salesforce org but is pre-configured for Trailhead challenges and can be launched directly from a Trailhead module or project.
3. Three times per year: **Spring** (February), **Summer** (June), **Winter** (October).
4. Customers receive automatic upgrades three times per year with no action required — no manual patches, no server maintenance, no downtime window to plan.
5. **Full Sandbox** — it is an exact copy of production including all data, perfect for UAT.
6. A Full Sandbox copies all production data. A Partial Sandbox copies only a configurable subset (sample) of production data. Partial Sandboxes refresh faster and are less expensive.
7. Both — completing a Module earns a badge. Completing a Project also earns a badge. Trails earn a Trail Completion badge when all included modules/projects are done.
8. **Default Locale** — controls date, time, number, and name formats for the org.
9. A Scratch Org is a temporary, empty, source-code-driven org used for developer CI/CD workflows. Unlike a Developer Sandbox, it is created and deleted programmatically, contains no data, and is not a copy of production.
10. If Sandbox gets Summer '26 in April, Production typically receives it approximately **4 weeks later** — in June.

---

## 🔗 Trailhead Resources

- [Salesforce Free Trials](https://developer.salesforce.com/free-trials)
- [Trailhead Playground Management](https://trailhead.salesforce.com/content/learn/modules/trailhead_playground_management)
- [Company-Wide Org Settings](https://trailhead.salesforce.com/content/learn/modules/company_wide_org_settings)

---

*Next: [02 · Salesforce Licenses & User Management →](./02_Licenses_and_User_Management.md)*
