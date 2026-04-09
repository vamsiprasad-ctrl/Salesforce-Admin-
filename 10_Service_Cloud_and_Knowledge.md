# 10 · Service Cloud & Knowledge

> **Difficulty:** 🟡 Intermediate | **Est. Time:** 7 hrs

---

## 10.1 Cases

Central service object. Tracks one customer issue from open → closed.

| Field | Purpose |
|-------|---------|
| Status | New, Working, Escalated, Pending Customer, Closed |
| Priority | High, Medium, Low |
| Origin | Email, Web, Phone, Chat |
| Subject | Short description |

---

## 10.2 Support Process

Defines available Status picklist values per Record Type. Different case categories have different workflow stages.

---

## 10.3 Queues, Assignment & Escalation Rules

- **Queue** — pool of agents; cases wait here until claimed
- **Assignment Rule** — auto-routes new cases to queue/agent by criteria (only 1 active at a time)
- **Escalation Rule** — fires when SLA time thresholds are breached

---

## 10.4 Web-to-Case & Email-to-Case

- **Web-to-Case** — case from website form
- **Email-to-Case** — inbound email converts to case; full thread preserved on the case record

---

## 10.5 Entitlements & Milestones

- **Entitlement** — defines SLA commitments (response time, resolution time)
- **Milestone** — required step with a time target (e.g., First Response within 1 hour)

---

## 10.6 Lightning Service Console

Agent productivity app: split view, multi-tab navigation, utility bar, Knowledge suggestions, Macros.

---

## 10.7 Salesforce Knowledge

| Component | Description |
|-----------|-------------|
| Article | A document: FAQ, solution, how-to |
| Data Categories | Hierarchical taxonomy for visibility and filtering |
| Article Visibility | Internal / Customer / Partner / Public |
| Auto-Suggestion | Articles surface in Case feed as agent types |
| Macros | One-click multi-step agent actions |

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Create a Case Manually

**Objective:** Understand the Case object structure.

**Steps:**
1. App Launcher → **Service** app → **Cases** tab → **New**
2. Fill in:
   - Subject: `Cannot log in to portal`
   - Status: `New`
   - Priority: `High`
   - Origin: `Phone`
   - Account: `TechStart India` (from previous topic)
   - Contact: `Arjun Mehta`
   - Description: `Customer reports login page is returning a 404 error since 9am today.`
3. Save
4. Observe: auto-generated Case Number, Activity Timeline, related lists

---

### 🔧 Task 2 — Create a Case Queue and Assignment Rule

**Objective:** Auto-route cases to the right team.

**Steps:**
1. **Create a Queue:**
   - Setup → Queues → **New**
   - Label: `Tier 1 Support`
   - Objects: Cases ✅
   - Members: add yourself
   - Save

2. **Create Assignment Rule:**
   - Setup → Case Assignment Rules → **New**
   - Name: `Standard Case Routing`
   - Mark as Default: ✅
   - Save → click into the rule → **New Rule Entry**
   - Order: 1
   - Criteria: `Priority = High`
   - Assign to: Queue → `Tier 1 Support`
   - Save

3. **Test:** Create a new High Priority case → verify it is assigned to `Tier 1 Support` queue.

---

### 🔧 Task 3 — Configure Web-to-Case

**Objective:** Capture customer issues from a form.

**Steps:**
1. Setup → Self-Service → **Web-to-Case**
2. Enable Web-to-Case: ✅
3. Default Case Origin: `Web`
4. Default Case Owner: yourself or a queue
5. Auto-Response Rule: skip for now
6. Click **Generate the HTML** link in the page
7. Select fields: Name, Email, Subject, Description
8. Generate → copy HTML
9. Create a local HTML file, paste, open in browser, submit a test case
10. In Salesforce → Cases tab → verify the case appeared with Origin = Web

---

### 🔧 Task 4 — Enable Salesforce Knowledge and Create an Article

**Objective:** Set up Knowledge and publish your first article.

**Steps:**
1. Setup → Knowledge Settings → Enable Lightning Knowledge: ✅
2. Assign yourself the **Knowledge User** feature license:
   - Setup → Users → your user → Edit → check Knowledge User → Save
3. App Launcher → **Knowledge** → **New**
4. Fill in:
   - Title: `How to reset your portal password`
   - Summary: `Step-by-step guide to resetting your login password`
   - Article Body: Write 5 clear steps for resetting a password
5. Assign visibility: check **Customer** (customers can see it)
6. Click **Publish**
7. Open a Case → look at the Knowledge sidebar → search "password" → your article should appear
8. Click **Attach to Case** → verify it's attached in the Case's Knowledge related list

---

### 🔧 Task 5 — Create a Macro

**Objective:** Build a one-click case resolution macro.

**Steps:**
1. App Launcher → **Macros** → **New**
2. Name: `Standard Close — Issue Resolved`
3. Instructions:
   - **Add Instruction** → Select: Case → Field: Status → Value: Closed
   - **Add Instruction** → Select: Case → Field: Priority → Value: Low
   - **Add Instruction** → Select: Send Email → choose a simple email template (create one first if needed): Subject: "Your case is resolved", Body: "Hi {Contact.FirstName}, your case has been resolved."
   - **Add Instruction** → Log a Call → Subject: "Case closed by macro"
4. Save
5. Open the Case from Task 1 → in the Utility Bar, open Macros → find your macro → **Run**
6. Verify: Status = Closed, email sent, activity logged — all from one click

---

### 🔧 Task 6 — Set Up an Escalation Rule

**Objective:** Auto-escalate overdue cases.

**Steps:**
1. Setup → Escalation Rules → **New**
2. Name: `SLA Escalation`
3. Mark as Default: ✅
4. Save → click into rule → **New Rule Entry**
5. Order: 1
6. Criteria: `Priority = High`
7. Age over (hours): **4**
8. Business Hours: use org business hours (or create them first)
9. Escalation Action: Assign to Queue → `Tier 1 Support` + Notify: Admin email
10. Save

---

## 📝 Practice Questions

**Q1.** What is the maximum number of Case Assignment Rules that can be active at one time?

**Q2.** What is the difference between a Case Queue and a Case Team?

**Q3.** What happens to the email thread when a customer replies to a case email?

**Q4.** Auto-Suggestion in Knowledge surfaces articles based on which field of the Case?

**Q5.** What is the difference between Web-to-Case and Email-to-Case in terms of how the case is created?

**Q6.** A Macro runs multiple actions in one click. True or False: A Macro can be used by any user with access to cases.

**Q7.** What is an Entitlement, and how is it different from an Escalation Rule?

**Q8.** Which feature license must be enabled on a user's record before they can create or manage Knowledge Articles?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Service Center Design**

> "CloudBank" is launching a customer support center for 1,500 agents handling banking queries. Requirements:
> - Credit card queries → Credit Card team
> - Loan queries → Loans team
> - Fraud reports → always highest priority → Fraud Investigation team
> - Enterprise clients (Gold tier) → dedicated Gold Support team regardless of query type
> - All cases must be responded to within 2 hours; Fraud cases within 30 minutes
> - Agents must be able to work 10+ cases simultaneously without navigating away
>
> **Question:** Map each requirement to the specific Salesforce Service Cloud feature that addresses it and describe the exact configuration.

---

**Scenario 2 — Knowledge Base Strategy**

> "SupportFirst" processes 10,000 cases per month. Analysis shows:
> - 3,500 cases are about password resets
> - 2,000 cases are about billing queries
> - 1,200 cases are about a known product bug
> - 3,300 are unique issues
>
> **Question:**
> 1. How many Knowledge Articles should they create based on this analysis?
> 2. Design the Data Category structure for their knowledge base
> 3. Which articles should be visible to customers (self-service), and which internal only?
> 4. How can Auto-Suggestion reduce their case volume? What configuration is needed?
> 5. Calculate: if Knowledge deflects 40% of the top 3 case categories, how many cases are saved monthly?

---

## ✔️ Answer Key

1. **One** Assignment Rule can be active at a time. That single rule can have multiple rule entries (criteria rows) evaluated in priority order.
2. A **Queue** is a holding pool where unassigned cases wait — any queue member can accept the next case. A **Case Team** is a group of named individuals collaborating on one specific complex case, each with a defined role.
3. When a customer replies to a case email, the reply is **automatically appended to the Case's email thread** and the Case status can be updated based on a configured rule. The full conversation history remains on the case.
4. Auto-Suggestion surfaces relevant articles based on the **Case Subject** field as the agent types.
5. **Web-to-Case** is triggered by a customer submitting an HTML form (push mechanism). **Email-to-Case** is triggered by an inbound email arriving at a monitored support email address.
6. **False** — Macros require specific setup. Users need the Macros utility in the Service Console, and the Macro must be shared with them or be public.
7. An **Entitlement** is a contractual SLA definition tied to an Account — it defines what level of support they're entitled to. An **Escalation Rule** is a time-based action that fires when a case hasn't been resolved/updated in a defined period, regardless of entitlement.
8. The **Knowledge User** feature license must be checked on the user's record.

---

## 🔗 Trailhead Resources

- [Lightning Knowledge Basics](https://trailhead.salesforce.com/content/learn/modules/lightning-knowledge-basics)
- [Set Up Case Escalation & Entitlements](https://trailhead.salesforce.com/content/learn/projects/set-up-case-escalation-entitlements)
- [Macros Quick Look for Agents](https://trailhead.salesforce.com/content/learn/modules/macros-quick-look-for-agents)

---

*Previous: [09 · Sales Cloud](./09_Sales_Cloud.md) | Next: [11 · Experience Cloud →](./11_Experience_Cloud.md)*

