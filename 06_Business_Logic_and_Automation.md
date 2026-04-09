# 06 · Business Logic & Automation

> **Module:** Automation | **Difficulty:** 🔴 Intermediate–Advanced | **Est. Time:** 10 hrs

---

## 6.1 Why Automate?

Manual processes are slow, inconsistent, and error-prone. Automation in Salesforce lets you:
- Enforce data quality rules before saving
- Trigger actions automatically when data changes
- Route approvals through a formal review process
- Replace hours of repetitive admin work with zero-click workflows

This section covers **all Salesforce automation tools** and, critically, **when to use each one**.

---

## 6.2 Validation Rules

A **Validation Rule** checks data quality when a record is saved. If the rule's formula evaluates to `TRUE`, the save is **blocked** and an error message is shown to the user.

> 🔑 **Remember:** You write the formula to describe the **INVALID** condition. If the formula is TRUE, the record is invalid and the save fails.

### Syntax

```
[CONDITION THAT IS INVALID] = TRUE → Block save, show error
```

### Real-World Example 1 — Required field logic

> **Rule:** "Opportunity must have a Close Date in the future when Stage is not Closed Won or Closed Lost"
>
> **Formula:**
> ```
> AND(
>   NOT(ISPICKVAL(StageName, "Closed Won")),
>   NOT(ISPICKVAL(StageName, "Closed Lost")),
>   CloseDate < TODAY()
> )
> ```
> **Error Message:** "Close Date must be a future date for active opportunities."

### Real-World Example 2 — Phone format validation

> **Rule:** "Phone must be 10 digits"
>
> **Formula:**
> ```
> NOT(REGEX(Phone, "\\d{10}"))
> ```
> **Error Message:** "Phone number must be exactly 10 digits with no spaces or special characters."

### Real-World Example 3 — Conditional field requirement

> **Rule:** "If Opportunity Amount > $100,000, a Legal Review Date must be entered"
>
> **Formula:**
> ```
> AND(
>   Amount > 100000,
>   ISBLANK(Legal_Review_Date__c)
> )
> ```
> **Error Message:** "Deals over $100,000 require a Legal Review Date before saving."

### Key Validation Rule Functions

| Function | Purpose | Example |
|----------|---------|---------|
| `ISBLANK(field)` | True if field is empty | `ISBLANK(Email)` |
| `ISPICKVAL(field, "value")` | True if picklist = specific value | `ISPICKVAL(Stage, "Closed Won")` |
| `REGEX(field, pattern)` | Text matches a pattern | `REGEX(Phone, "\\d{10}")` |
| `TODAY()` | Current date | `CloseDate < TODAY()` |
| `AND(...)` | All conditions true | `AND(A, B, C)` |
| `OR(...)` | Any condition true | `OR(A, B)` |
| `NOT(...)` | Inverts a condition | `NOT(ISBLANK(Email))` |
| `ISNEW()` | True if creating a new record | Prevents rules on edit |
| `ISCHANGED(field)` | True if field value changed | Apply rule only on change |

---

## 6.3 Approval Processes

An **Approval Process** is a formal multi-step workflow that routes a record to specific people for review and approval — before an action is taken.

### Real-World Example — Discount Approval

> **Company:** "SalesForce Training" — no rep can offer more than 10% discount without management sign-off. Deals over $500K need the CEO's approval.
>
> **Approval Process: Discount Approval**
>
> **Entry Criteria:** `Discount_Percentage__c > 10`
>
> **Step 1:** Approve if `Discount_Percentage__c <= 20` → Route to user's direct manager
> - If Approved → continue to Step 2 (if applicable) or final approval
> - If Rejected → notify rep, unlock record, reset discount to 10%
>
> **Step 2:** If `Amount > 500,000` → Route to CEO role
> - If Approved → lock discount, send confirmation email to rep and customer
> - If Rejected → notify manager + rep, unlock record
>
> **Initial Submission Actions:** Lock the record (no edits during review)
> **Final Approval Actions:** Change `Approval_Status__c` to Approved, send email, update a field
> **Final Rejection Actions:** Change `Approval_Status__c` to Rejected, send email, unlock record
> **Recall Actions:** Unlock record when rep retracts the submission

### Approval Process Components

| Component | What It Does |
|-----------|-------------|
| **Entry Criteria** | When should this process trigger? |
| **Approver** | Specific user, user's manager, related user field, or queue |
| **Initial Submission Actions** | What happens the moment the record is submitted |
| **Approval Step Actions** | What happens at each approval or rejection within a step |
| **Final Approval Actions** | What happens when all steps are approved |
| **Final Rejection Actions** | What happens when any step is rejected |
| **Recall Actions** | What happens when the submitter recalls the request |

---

## 6.4 Flow Builder — The Heart of Salesforce Automation

**Flow Builder** is Salesforce's primary automation tool. It replaces Workflow Rules and Process Builder (both retired). Every new automation should be built in Flow.

A Flow is a **set of instructions** that tells Salesforce to collect data, make decisions, and perform actions — automatically or through a guided user experience.

---

### 6.4.1 Flow Types

| Flow Type | What Triggers It | User Interaction | Best For |
|-----------|-----------------|-----------------|---------|
| **Record-Triggered Flow** | A record is created, updated, or deleted | None (runs in background) | Auto-updating fields, sending notifications, creating related records |
| **Screen Flow** | A user clicks a button or link | Step-by-step screens | Guided data entry, self-service wizards |
| **Scheduled Flow** | A date/time schedule | None | Nightly batch jobs, reminders, data cleanup |
| **Platform Event-Triggered Flow** | A platform event is received | None | Event-driven integrations |
| **Auto-launched Flow** | Called by another flow, Apex, or REST API | None | Reusable logic modules |

---

### 6.4.2 Record-Triggered Flow

**When to use:** When you want something to happen automatically when a record is saved.

#### Before-Save vs After-Save

| | Before-Save | After-Save |
|--|-------------|------------|
| When it runs | Before record is written to DB | After record is committed |
| Can update triggering record fields? | ✅ Yes (fast, no extra DML) | ✅ Yes (but uses extra DML) |
| Can create/update other records? | ❌ No | ✅ Yes |
| Performance | Faster | Slightly slower |
| Use for | Field updates on the same record | Creating related records, sending emails, posting to Chatter |

### Real-World Example — Before-Save Record-Triggered Flow

> **Scenario:** When an Opportunity is created or updated, automatically set `Region__c` based on the Account's `BillingState`.
>
> **Flow type:** Record-Triggered (Before Save)
> **Object:** Opportunity
> **Trigger:** Created or Updated
>
> **Logic:**
> 1. Get the related Account record (using `{!$Record.AccountId}`)
> 2. Decision: What is `Account.BillingState`?
>    - CA, OR, WA → Set `Region__c = "West"`
>    - NY, NJ, CT → Set `Region__c = "East"`
>    - TX, FL → Set `Region__c = "South"`
>    - All others → Set `Region__c = "Central"`
> 3. Update `{!$Record.Region__c}` (Assignment element)
>
> **Result:** Every Opportunity always has the correct Region — no manual entry, no errors.

### Real-World Example — After-Save Record-Triggered Flow

> **Scenario:** When a Case is marked Closed, automatically create a follow-up Task for the account manager to check on customer satisfaction 3 days later.
>
> **Flow type:** Record-Triggered (After Save)
> **Object:** Case
> **Trigger:** Updated
> **Entry Condition:** `Status = "Closed"` AND `ISCHANGED(Status)`
>
> **Actions:**
> 1. Create Task:
>    - Subject: "Follow up: {!$Record.CaseNumber} satisfaction check"
>    - Due Date: `TODAY() + 3`
>    - Owner: Account's Owner
>    - WhatId: {!$Record.AccountId}

---

### 6.4.3 Screen Flow

**When to use:** When you want to guide a user through a structured data-entry process.

### Real-World Example

> **Scenario:** "HealthClinic" onboards new patients. The receptionist must:
> 1. Collect patient demographics
> 2. Collect insurance information
> 3. Collect emergency contact
> 4. Schedule first appointment
> 5. Create Account + Contact + Case + Appointment records
>
> **Without Screen Flow:** 4 separate records created manually — slow, error-prone, inconsistent.
>
> **With Screen Flow:** A 4-screen wizard guides the receptionist step by step. Salesforce creates all 4 records at the end automatically from the collected information.
>
> Screen Flow is launched from a Quick Action button on the Home page.

---

### 6.4.4 Scheduled Flow

**When to use:** Run automation on a recurring schedule — nightly, weekly, or at a specific time relative to a date field.

### Real-World Example

> **Scenario:** "SubscriptionCo" wants to automatically email customers whose subscriptions expire in 30 days.
>
> **Scheduled Flow:**
> - Schedule: Daily at 8:00 AM
> - Start Condition: `Subscription_End_Date__c = TODAY() + 30`
> - Action: Send email from template "Subscription Renewal Reminder"
>
> **Another example:** Every night at midnight, find all Leads with `Created Date < 90 days ago` and `Status = Open` → set `Status = "Aged Out"` and notify the owner.

---

### 6.4.5 Flow Elements

| Element | What It Does |
|---------|-------------|
| **Get Records** | Query Salesforce for records |
| **Create Records** | Insert new records |
| **Update Records** | Update existing records |
| **Delete Records** | Delete records |
| **Decision** | Branch the flow based on conditions (like an IF statement) |
| **Assignment** | Set or update a variable value |
| **Loop** | Iterate over a collection of records |
| **Screen** | Display a user interface (Screen Flows only) |
| **Subflow** | Call another flow |
| **Send Email** | Send a single email |
| **Email Alert** | Use a pre-built email template |
| **Post to Chatter** | Post a message to a record's Chatter feed |
| **Apex Action** | Call a developer-built Apex method |

---

### 6.4.6 Flow Best Practices

1. **Always add a Fault Path** — every element that can fail (Get Records, Create Records, etc.) should have a fault connector leading to an error-handling screen or email alert to the admin.

2. **Use Before-Save for field updates** on the same record — it's faster and doesn't count against governor limits.

3. **Bulkify your flows** — avoid putting DML inside loops. Instead, use collection variables and a single Create/Update element after the loop.

4. **Build subflows for reusable logic** — if two flows both need to "send a welcome email," extract that into a reusable Auto-launched flow.

5. **Name everything clearly** — `Get_Related_Account_By_Id` is much clearer than `Get Records 1`.

---

## 6.5 Automation Tool Comparison

| Feature | Validation Rule | Approval Process | Flow Builder | Workflow Rule (Retired) | Process Builder (Retired) |
|---------|----------------|-----------------|-------------|------------------------|--------------------------|
| Fires on | Save | Manual submission | Many triggers | Record save | Record save |
| Human decision step | ❌ | ✅ | ❌ (use Approval) | ❌ | ❌ |
| Create related records | ❌ | ✅ (via Flow) | ✅ | ❌ | ✅ |
| Multiple conditions | ✅ | ✅ | ✅ | Limited | ✅ |
| Scheduled actions | ❌ | ❌ | ✅ | ✅ | ✅ |
| Call Apex | ❌ | ❌ | ✅ | ❌ | ✅ |
| Future investment | N/A | N/A | ✅ Actively developed | ❌ Retired | ❌ Retired |

### Decision Guide

```
Need to BLOCK bad data on save?
  → Validation Rule

Need a HUMAN to review and approve?
  → Approval Process

Need to AUTOMATE based on record changes?
  → Record-Triggered Flow

Need a GUIDED USER EXPERIENCE?
  → Screen Flow

Need to run on a SCHEDULE?
  → Scheduled Flow

Is it currently in Workflow Rules or Process Builder?
  → Migrate to Flow
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Validation Rule | Formula-based save blocker — formula = TRUE means INVALID |
| Approval Process | Multi-step human approval with configurable routing and actions |
| Record-Triggered Flow | Automatic action when a record is created/updated/deleted |
| Screen Flow | Guided UI wizard for data entry |
| Scheduled Flow | Runs on a time schedule — batch automation |
| Before-Save Flow | Updates the triggering record — fast, no DML limits |
| After-Save Flow | Creates/updates other records — slightly slower |
| Workflow Rules / Process Builder | Retired — migrate to Flow |

---

## 🔗 Trailhead Resources

- [Point & Click Business Logic](https://trailhead.salesforce.com/content/learn/modules/point_click_business_logic)
- [Validation Rules](https://trailhead.salesforce.com/content/learn/modules/validation-rules)
- [Improve Data Quality for a Cleaning Supply App](https://trailhead.salesforce.com/content/learn/projects/improve-data-quality-for-a-cleaning-supply-app)
- [Business Process Automation](https://trailhead.salesforce.com/content/learn/modules/business_process_automation)
- [Build a Discount Approval Process](https://trailhead.salesforce.com/content/learn/projects/build-a-discount-approval-process)
- [Build Flows with Flow Builder](https://trailhead.salesforce.com/content/learn/trails/build-flows-with-flow-builder)

---

*Previous: [05 · Customization & UI](./05_Customization_and_UI.md)*
*Next: [07 · Reports & Dashboards →](./07_Reports_and_Dashboards.md)*
