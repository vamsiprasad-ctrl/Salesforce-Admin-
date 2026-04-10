# 06D · Automation Tool Comparison & Migration Guide

> **Sub-Topic:** Business Logic & Automation | **Difficulty:** 🟡 Intermediate | **Est. Time:** 1 hr

---

## The Complete Automation Landscape

Salesforce has had many automation tools over the years. Understanding the full history and current state is critical — especially when you inherit an org with legacy automation.

| Tool | Introduced | Status | Should New Automation Use It? |
|------|-----------|--------|-------------------------------|
| Formula Fields | Always | ✅ Active | Yes — for calculated fields |
| Validation Rules | Always | ✅ Active | Yes — for data quality |
| Workflow Rules | 2000s | ❌ Retired 2023 | NO — migrate to Flow |
| Approval Processes | 2000s | ✅ Active | Yes — for human approvals |
| Process Builder | 2015 | ❌ Retired 2023 | NO — migrate to Flow |
| Flow Builder | 2015 (improved significantly 2019–present) | ✅ Active — PRIMARY | Yes — all new automation |
| Apex Triggers | Always | ✅ Active | Yes — for complex code logic |

---

## Workflow Rules (RETIRED — Know What They Did)

**What Workflow Rules could do:**
- Field Updates — update a field value when criteria met
- Email Alerts — send templated emails
- Tasks — create a Task record
- Outbound Messages — send data to external systems

**What Workflow Rules could NOT do:**
- Create records other than Tasks
- Update related records
- Query for other records
- Have multiple steps
- Screen interaction
- Run on Schedule (except time-triggered actions)

**Why they were retired:**
Flow Builder can do everything Workflow Rules could do, plus vastly more. Salesforce officially retired Workflow Rules in 2023.

**Migration path:** Workflow Rule → Record-Triggered Flow (After-Save)

---

## Process Builder (RETIRED — Know What It Did)

**What Process Builder could do:**
- Everything Workflow Rules could do
- Create records (any object, not just Tasks)
- Update related records (one level deep)
- Call other Processes (chaining)
- Call Flows
- Call Apex

**What Process Builder could NOT do:**
- Loop through multiple records
- Complex conditional branching
- Screen interaction
- Reliable bulkification
- Before-Save optimization

**Why it was retired:**
Flow Builder completely supersedes Process Builder with better performance, bulkification, debugging, and capabilities. Process Builder also had serious performance and governor limit issues with complex orgs.

**Migration path:** Process Builder → Record-Triggered Flow (Before-Save or After-Save depending on what actions were taken)

---

## Decision Matrix — When to Use What

```
┌─────────────────────────────────────────────────────────────────┐
│  Start: What do you need to accomplish?                         │
└────────────────────────┬────────────────────────────────────────┘
                         │
              ┌──────────▼──────────┐
              │ Block invalid data  │
              │    on save?         │
              └──────────┬──────────┘
                    YES  │  NO
              ┌──────────▼──────────┐
              │  VALIDATION RULE    │
              └─────────────────────┘
                         │ NO
              ┌──────────▼──────────┐
              │ Requires a HUMAN    │
              │ to Approve/Reject?  │
              └──────────┬──────────┘
                    YES  │  NO
              ┌──────────▼──────────┐
              │  APPROVAL PROCESS   │
              └─────────────────────┘
                         │ NO
              ┌──────────▼──────────┐
              │ Needs GUIDED USER   │
              │ screens/wizard?     │
              └──────────┬──────────┘
                    YES  │  NO
              ┌──────────▼──────────┐
              │   SCREEN FLOW       │
              └─────────────────────┘
                         │ NO
              ┌──────────▼──────────┐
              │ Triggered by TIME   │
              │ or a SCHEDULE?      │
              └──────────┬──────────┘
                    YES  │  NO
              ┌──────────▼──────────┐
              │  SCHEDULED FLOW     │
              └─────────────────────┘
                         │ NO
              ┌──────────▼──────────┐
              │ Triggered by RECORD │
              │ change (save)?      │
              └──────────┬──────────┘
                    YES  │
              ┌──────────▼──────────────────────────────────────┐
              │ Only updates SAME RECORD fields?                 │
              │ YES → Before-Save Record-Triggered Flow          │
              │ NO (creates other records, sends emails, etc.)   │
              │     → After-Save Record-Triggered Flow           │
              └──────────────────────────────────────────────────┘
```

---

## Side-by-Side Comparison

| Capability | Validation Rule | Approval Process | Record-Triggered Flow | Screen Flow | Scheduled Flow |
|-----------|----------------|-----------------|----------------------|-------------|----------------|
| Block save | ✅ | ❌ | ❌ | ❌ | ❌ |
| Human decision step | ❌ | ✅ | ❌ | ✅ (optional) | ❌ |
| Update same record | ❌ (read-only) | ✅ (via actions) | ✅ | ✅ | ✅ |
| Create other records | ❌ | ✅ (via actions) | ✅ (After-Save) | ✅ | ✅ |
| Update other records | ❌ | ✅ (via actions) | ✅ (After-Save) | ✅ | ✅ |
| Send emails | ❌ | ✅ | ✅ | ✅ | ✅ |
| User interaction | ❌ | ✅ (approval UI) | ❌ | ✅ | ❌ |
| Schedule-based | ❌ | ❌ | ✅ (Wait element) | ❌ | ✅ |
| Call Apex | ❌ | ❌ | ✅ | ✅ | ✅ |
| Query other records | ❌ | ❌ | ✅ | ✅ | ✅ |
| Loop through records | ❌ | ❌ | ✅ | ✅ | ✅ |

---

## Migration: Workflow Rules → Flow

### Workflow Rule Action: Field Update
**Before (Workflow Rule):**
- Criteria: `Stage = "Closed Won"`
- Action: Field Update → set `Won_Date__c = TODAY()`

**After (Record-Triggered Flow — Before-Save):**
- Trigger: Record Updated
- Entry Condition: `Stage = "Closed Won"` AND `ISCHANGED(Stage)`
- Assignment: `{!$Record.Won_Date__c} = {!$Flow.CurrentDate}`

---

### Workflow Rule Action: Email Alert
**Before (Workflow Rule):**
- Criteria: `Priority = High`
- Action: Email Alert using "High Priority Alert" template

**After (Record-Triggered Flow — After-Save):**
- Trigger: Record Created or Updated
- Entry Condition: `Priority = High` AND `ISCHANGED(Priority)`
- Email Alert element: same template

---

### Workflow Rule Action: Create Task
**Before (Workflow Rule):**
- Criteria: `Stage = "Closed Won"`
- Action: Create Task → Subject: "Send thank you gift", Due: +7 days

**After (Record-Triggered Flow — After-Save):**
- Trigger: Record Updated
- Entry Condition: `ISPICKVAL(Stage, "Closed Won")` AND `ISCHANGED(Stage)`
- Create Records: Task with Subject, Due Date formula, Owner, Related To

---

## Migration: Process Builder → Flow

Process Builder → Record-Triggered Flow is the most direct migration.

**Key differences to address:**
1. Process Builder evaluated criteria top-to-bottom and ran all matching criteria. Flow uses Decision elements with explicit branching.
2. Process Builder had scheduled actions (time offsets). Flow uses Wait elements or Scheduled Flows.
3. Process Builder child processes (called via "Call a process") → Subflows in Flow.

---

## Governor Limits Relevant to Automation

| Limit | Value | What Triggers It |
|-------|-------|-----------------|
| SOQL Queries per transaction | 100 | Get Records element (each = 1 SOQL) |
| DML Operations per transaction | 150 | Create/Update/Delete Records (each = 1 DML) |
| Flow interviews per transaction | 50 | Multiple flows firing on same record |
| CPU time per transaction | 10,000 ms | Complex loop logic |

**How to avoid hitting limits:**
- Never put Get Records or Create Records inside a Loop
- Collect records in variables inside loops, perform one DML outside
- Combine multiple Before-Save flows into one (multiple Start elements supported)
- Avoid chained flows that each perform DML

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Identify Old Automation in Your Org

**Objective:** Audit what automation exists.

**Steps:**
1. Setup → **Workflow Rules** — look at any existing rules and their statuses
2. Setup → **Process Builder** — look at any existing processes
3. Setup → **Flows** — look at all active flows
4. Create a table:

| Name | Type | What It Does | Still Needed? | Migration Action |
|------|------|-------------|---------------|-----------------|
| (fill in) | Workflow | (fill in) | Yes/No | Convert to Flow |

---

### 🔧 Task 2 — Choose the Right Tool (Decision Practice)

**Objective:** Practice the automation decision process.

For each requirement below, write which tool you'd use and why:

1. When a Lead is created, make Email field required if Lead Source = "Web"
2. Purchase orders above ₹50,000 need manager approval before the PO is issued
3. Every Friday at 5PM, send a weekly summary email to all Sales Managers
4. A customer service agent needs to walk through a 3-question diagnostic before logging a case
5. When an Opportunity closes, automatically create an Invoice record with the deal amount
6. An Account's phone number must be exactly 10 digits
7. New employee joining → create Salesforce user + assign permissions + send welcome email (triggered from HR system event)
8. On Opportunity edit, if Amount was changed by more than 50%, log a Chatter post alerting the Sales Manager

---

### 🔧 Task 3 — Migrate a Simple Workflow Rule to Flow

**Objective:** Practice the migration process.

**Simulate a Workflow Rule (we'll build the equivalent Flow):**

**Original (hypothetical) Workflow Rule:**
- Object: `Job_Application__c`
- Criteria: `Application_Status__c = "Interview"`
- Action: Field Update → set `Interview_Scheduled__c (Checkbox) = True`

**Build the equivalent Record-Triggered Flow:**
1. New Flow → Record-Triggered Flow
2. Object: `Job_Application__c`
3. Trigger: Created or Updated
4. Optimize for: **Before the record is saved**
5. Entry Conditions: `Application_Status__c = "Interview"` AND `ISCHANGED(Application_Status__c)`
6. Add Assignment: `{!$Record.Interview_Scheduled__c} = True`
7. Save → `Set Interview Scheduled Flag` → Activate

**Test:** Change a Job Application Status to "Interview" → Interview_Scheduled__c should automatically become checked.

---

## 📝 Practice Questions — Automation Comparison

**Q1.** Salesforce retired Workflow Rules in 2023. What should existing Workflow Rules be migrated to?

**Q2.** A company has 50 Process Builder processes. Is there a tool Salesforce provides to migrate them?

**Q3.** What are two things Process Builder could do that Workflow Rules could NOT?

**Q4.** A Validation Rule and a Before-Save Flow both fire when a record is saved. Which fires first?

**Q5.** An org has both a Workflow Rule and a Record-Triggered Flow on the same object with overlapping criteria. Do both run?

**Q6.** What is the Salesforce recommendation for any automation that was built in Workflow Rules or Process Builder?

**Q7.** Name two capabilities that Apex Triggers have that Flow Builder does NOT.

**Q8.** A Before-Save Flow and an After-Save Flow are both active on the Opportunity object. In what order do they execute?

---

## 🎯 Scenario-Based Questions — Automation Comparison

---

**Scenario 1 — Automation Audit**

> You join a company as a new Salesforce admin. The org has:
> - 12 active Workflow Rules
> - 8 active Process Builder processes
> - 3 active Flows
> - 6 active Approval Processes
>
> The org is experiencing performance issues and occasional governor limit errors during data imports.
>
> **Question:**
> 1. What is the most likely cause of the governor limit errors during imports?
> 2. Which automation types should you prioritize migrating, and why?
> 3. What is your migration strategy — do you migrate everything at once or in phases?
> 4. How do you ensure existing behavior is preserved when migrating?

---

**Scenario 2 — Right Tool for the Job**

> "FinanceFirst" has asked for automation for their loan application process:
>
> 1. A loan officer fills in a form collecting 12 pieces of customer information — should create a Loan Application record and auto-calculate the EMI
> 2. Loan applications over ₹20L need Credit Manager approval; over ₹50L need Credit Manager + VP Finance approval
> 3. Every night, find all loan applications pending for more than 5 days and send a reminder to the loan officer
> 4. When an application is Approved, automatically create a Disbursement Schedule record with 12 monthly entries
> 5. The Loan Amount cannot be negative and cannot exceed ₹1 Crore
> 6. If customer's `Credit_Score__c` < 650, set `Requires_Guarantor__c = True` automatically when the application is saved
>
> **Question:** Map each requirement to the correct automation tool and briefly explain why.

---

## ✔️ Answer Key — Practice Questions

1. **Record-Triggered Flows** — Before-Save for field updates, After-Save for creating/updating other records and sending emails.
2. Yes — Salesforce provides the **Flow Migration Tool** (also called "Migrate to Flow" within Setup) that can automatically convert many Workflow Rules and some Process Builder processes to Flows. Manual review and testing are always required after migration.
3. Process Builder could: (1) **Create records** on any object (not just Tasks), (2) **Update related records** one level away (e.g., update Account from Opportunity). Workflow Rules could only update the triggering record or create Tasks.
4. **Validation Rules** fire first, before any automation. If a Validation Rule blocks the save, the record is not saved and no Flow runs. Before-Save Flows fire after Validation Rules but before the record is committed to the database.
5. **Yes** — both can run. All active automation on an object fires for qualifying records in a defined order: Validation Rules → Before Triggers → Before Flows → Duplicate Rules → Save → After Triggers → After Flows → Workflow Rules → Process Builder → etc. Having both can cause unexpected results and governor limit issues.
6. Salesforce's official recommendation: **migrate all Workflow Rules and Process Builder processes to Flow Builder** as they are retired and will eventually be removed entirely.
7. Apex Triggers can: (1) **Call complex logic with full programming language capabilities** — loops within loops, complex data structures. (2) **Run in a specific order** with other triggers using trigger frameworks. Flow has governor limit sensitivity that Apex can be optimized around more precisely.
8. **Before-Save Flow runs first** (before the record is written to the database), then the record is saved, then **After-Save Flow runs** (after the record is committed).

---

## 🔗 Trailhead Resources

- [Business Process Automation](https://trailhead.salesforce.com/content/learn/modules/business_process_automation)
- [Point & Click Business Logic](https://trailhead.salesforce.com/content/learn/modules/point_click_business_logic)

---

*Previous: [06C · Flow Builder](./06C_Flow_Builder.md) | Next: [07 · Reports & Dashboards →](./07_Reports_and_Dashboards.md)*
