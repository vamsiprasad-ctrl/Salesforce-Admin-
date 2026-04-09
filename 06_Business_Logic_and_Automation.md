# 06 · Business Logic & Automation

> **Difficulty:** 🔴 Intermediate–Advanced | **Est. Time:** 10 hrs

---

## 6.1 Why Automate?

Manual processes are slow, inconsistent, and error-prone. Automation eliminates repetitive work, enforces data quality, and triggers actions the moment conditions are met — 24/7 with zero human intervention.

---

## 6.2 Validation Rules

Blocks a save when the formula evaluates to `TRUE`. You write the formula to describe the **INVALID** condition.

### Real-World Examples

**1. Future date required:**
```
AND(
  NOT(ISPICKVAL(StageName, "Closed Won")),
  NOT(ISPICKVAL(StageName, "Closed Lost")),
  CloseDate < TODAY()
)
```
Error: "Close Date must be a future date for active opportunities."

**2. Phone format (10 digits):**
```
NOT(REGEX(Phone, "\\d{10}"))
```
Error: "Phone number must be exactly 10 digits."

**3. Conditional required field:**
```
AND(Amount > 100000, ISBLANK(Legal_Review_Date__c))
```
Error: "Deals over ₹1,00,000 require a Legal Review Date."

---

## 6.3 Approval Processes

Multi-step human approval workflow for formal reviews.

### Real-World Example — Discount Approval

> **SalesOrg:** No rep can offer >10% discount without manager sign-off. Deals >₹5L need VP approval.
>
> **Entry Criteria:** `Discount_Percentage__c > 10`
> **Step 1:** Route to user's Manager. If Approved AND `Amount <= 500000` → Final Approve. If Approved AND `Amount > 500000` → go to Step 2. If Rejected → notify rep, reset discount.
> **Step 2:** Route to VP Sales Role. If Approved → lock discount, notify rep. If Rejected → notify manager + rep.
> **Initial Submission Actions:** Lock record (no edits during review)
> **Final Approval Actions:** Set `Approval_Status__c = Approved`, send email
> **Recall Actions:** Unlock record

---

## 6.4 Flow Builder

### Flow Types

| Type | Trigger | Interaction | Best For |
|------|---------|-------------|---------|
| **Record-Triggered** | Record create/update/delete | None (background) | Auto-field updates, related record creation |
| **Screen Flow** | User clicks a button | Step-by-step screens | Guided wizards, data entry |
| **Scheduled** | Date/time schedule | None | Nightly batch jobs, reminders |
| **Platform Event** | Platform event received | None | Event-driven integrations |
| **Auto-launched** | Called by another flow/Apex | None | Reusable sub-flows |

### Before-Save vs After-Save

| | Before-Save | After-Save |
|--|-------------|------------|
| Updates triggering record | ✅ Fast, no DML | ✅ Uses DML |
| Creates/updates other records | ❌ | ✅ |
| Performance | Faster | Slightly slower |

### Real-World Examples

**Record-Triggered (Before-Save):** Auto-set `Region__c` on Opportunity from Account's Billing State.

**Record-Triggered (After-Save):** When Case closes, create a follow-up Task for the account manager 3 days later.

**Screen Flow:** Patient onboarding wizard — 4 screens collecting demographics, insurance, emergency contact, appointment. Creates Account + Contact + Case + Appointment at the end.

**Scheduled Flow:** Daily 8AM — find all Subscriptions expiring in 30 days → send renewal reminder emails.

---

## 6.5 Automation Tool Comparison

| Tool | Use For | Status |
|------|---------|--------|
| Validation Rule | Block bad data on save | ✅ Active |
| Approval Process | Human review required | ✅ Active |
| Flow Builder | All automated logic | ✅ Active — PRIMARY TOOL |
| Workflow Rules | Simple field updates, emails | ❌ Retired — migrate to Flow |
| Process Builder | Point-and-click automation | ❌ Retired — migrate to Flow |

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Build a Validation Rule (Deadline Must Be Future)

**Objective:** Prevent saving a Job Posting with a past deadline.

**Steps:**
1. Setup → Object Manager → `Job_Posting__c` → Validation Rules → **New**
2. Rule Name: `Deadline_Must_Be_Future`
3. Active: ✅
4. Error Condition Formula:
```
AND(
  NOT(ISBLANK(Application_Deadline__c)),
  Application_Deadline__c < TODAY()
)
```
5. Error Message: `Application Deadline must be a future date.`
6. Error Location: Field → `Application_Deadline__c`
7. Save

**Test:** Create a Job Posting with yesterday's date → should block. Try today+1 → should save.

---

### 🔧 Task 2 — Build a Validation Rule (Salary Required for Full-Time)

**Objective:** Make Salary Range required when Job Type = Full-Time.

**Steps:**
1. New Validation Rule on `Job_Posting__c`
2. Name: `Salary_Required_For_FullTime`
3. Formula:
```
AND(
  ISPICKVAL(Job_Type__c, "Full-Time"),
  ISBLANK(Salary_Range__c)
)
```
4. Error Message: `Salary Range is required for Full-Time positions.`
5. Error Location: Field → `Salary_Range__c`
6. Save

**Test:** Create Full-Time posting without salary → blocked. Add salary → saves. Create Contract posting without salary → allowed.

---

### 🔧 Task 3 — Build an Approval Process

**Objective:** Create an approval workflow where applications above a threshold need manager approval.

**Business Scenario:** Any Job Application marked "Offered" requires HR Manager approval before finalizing.

**Steps:**
1. Setup → Process Automation → Approval Processes → **New Approval Process** → Use Standard Setup Wizard
2. Object: `Job_Application__c`
3. Name: `Offer_Approval_Process`
4. Entry Criteria: `Application_Status__c = "Offered"`
5. Approval Assignment: Automatically assign using standard **User** field → select a user as approver
6. **Initial Submission Actions:** Add "Field Update" → set `Application_Status__c = "Pending Approval"`
7. **Approval Actions:** Add "Field Update" → set `Application_Status__c = "Approved-Offer Sent"`
8. **Rejection Actions:** Add "Field Update" → set `Application_Status__c = "Rejected"`
9. Save → **Activate**

**Test:**
1. Open a Job Application → change Status to "Offered" → Save
2. Click **Submit for Approval** button (appears after saving)
3. Log in as the approver user → find the approval request in their Home page or email
4. Click Approve → verify Status changes to "Approved-Offer Sent"

---

### 🔧 Task 4 — Build a Record-Triggered Flow (Before-Save)

**Objective:** Auto-populate a field when a record is created.

**Business Scenario:** When a Job Application is created, automatically set `Application_Date__c` to today.

**Steps:**
1. Setup → Process Automation → Flows → **New Flow**
2. Choose: **Record-Triggered Flow**
3. Object: `Job_Application__c`
4. Trigger: A record is **Created**
5. Optimize for: **Before the record is saved**
6. Add **Assignment** element:
   - Variable: `{!$Record.Application_Date__c}`
   - Operator: Equals
   - Value: `{!$Flow.CurrentDate}`
7. Connect Start → Assignment → End
8. Save as: `Set Application Date on Create`
9. Activate

**Test:** Create a new Job Application → observe `Application_Date__c` is automatically set to today without entering it.

---

### 🔧 Task 5 — Build a Record-Triggered Flow (After-Save)

**Objective:** Create a related record when a parent record is updated.

**Business Scenario:** When a Job Posting's `Application_Deadline__c` is set to within 7 days, automatically create a Task for the Recruiter to review applications.

**Steps:**
1. New **Record-Triggered Flow** on `Job_Posting__c`
2. Trigger: Record is **Created or Updated**
3. Optimize for: **After the record is saved**
4. Entry Condition:
   - Condition: `{!$Record.Application_Deadline__c} - {!$Flow.CurrentDate} <= 7`
   - AND `ISCHANGED({!$Record.Application_Deadline__c})`
5. Add **Create Records** element:
   - Object: Task
   - Subject: `"Review Applications: " & {!$Record.Name}`
   - Due Date: `{!$Record.Application_Deadline__c}`
   - Owner: `{!$Record.Recruiter__c}`
   - Status: Not Started
   - Priority: High
6. Save as `Create Deadline Review Task` → Activate

**Test:** Update a Job Posting's deadline to 5 days from now → a Task should be created for the recruiter automatically.

---

### 🔧 Task 6 — Build a Screen Flow

**Objective:** Create a guided wizard for applying to a job.

**Steps:**
1. New **Screen Flow**
2. Add a **Screen** element:
   - Screen Label: `Applicant Details`
   - Add: Text Input → Label: `Your Name`, Required: ✅
   - Add: Text Input → Label: `Your Email`, Required: ✅
   - Add: Picklist → Label: `Years of Experience` → Values: 0-1, 1-3, 3-5, 5+
3. Add **Create Records** element after the screen:
   - Object: `Job_Application__c`
   - Set `Applicant_Name__c = {!Name_input_variable}`
   - Set `Applicant_Email__c = {!Email_input_variable}`
   - Set `Application_Status__c = "Applied"`
4. Add a final **Screen** with a confirmation message: "Your application has been submitted!"
5. Save as `Apply for Job` → Activate
6. Create a **Quick Action** on `Job_Posting__c` → Action Type: Flow → select your Screen Flow
7. Add the Quick Action to the Job Posting page layout
8. **Test:** Open a Job Posting → click the "Apply for Job" button → walk through the wizard → a Job Application is created

---

### 🔧 Task 7 — Build a Scheduled Flow

**Objective:** Automatically close expired Job Postings.

**Steps:**
1. New **Scheduled Flow**
2. Schedule: Daily, run at 8:00 AM
3. Object: `Job_Posting__c`
4. Filter: `Application_Deadline__c < {!$Flow.CurrentDate}` AND `Status__c ≠ "Closed"` *(add a Status field if you haven't already)*
5. Add **Update Records** element:
   - Records to update: the records from the scheduled path
   - Set `Status__c = "Closed"`
6. Save → Activate

---

## 📝 Practice Questions

**Q1.** A Validation Rule formula is: `Amount < 0`. When does this rule fire and block the save?

**Q2.** What is the difference between a Before-Save and After-Save Record-Triggered Flow? Give one use case for each.

**Q3.** An Approval Process has an Initial Submission Action that locks the record. What does this mean for the submitting rep?

**Q4.** Workflow Rules have been retired by Salesforce. What tool should all new automation be built in?

**Q5.** A Scheduled Flow runs at 8AM. Does it run in the System Admin's time zone or the org's default time zone?

**Q6.** What element in a Flow should you always add to handle unexpected errors?

**Q7.** Can you call one Flow from another? What element do you use?

**Q8.** What is the difference between an Approval Process and a Record-Triggered Flow in terms of human involvement?

**Q9.** A rep creates an Opportunity with Amount = -50. The Validation Rule `Amount < 0` fires. What does the rep see?

**Q10.** Why is Before-Save faster than After-Save for updating the triggering record's own fields?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Choosing the Right Tool**

> "InsureCo" has the following automation requirements. For each, identify the best automation tool (Validation Rule, Approval Process, Record-Triggered Flow, Screen Flow, Scheduled Flow) and explain why:
>
> 1. When a policy is saved, the `End_Date__c` must always be after the `Start_Date__c`
> 2. Any policy with `Coverage_Amount__c > ₹50 Lakh` must be approved by the Underwriting Manager before being issued
> 3. 30 days before a policy expires, automatically send a renewal reminder email to the policy holder
> 4. A customer service agent needs to guide customers through a 5-step claim registration process, creating a Claim record at the end
> 5. When a Claim is marked "Approved", automatically create an Invoice record with the claim amount

---

**Scenario 2 — Validation Rule Design**

> "HRSolutions" needs the following data quality rules on their `Job_Posting__c` object. Write the validation rule formula for each:
>
> 1. The Application Deadline cannot be in the past (only when creating new postings, not when editing existing ones)
> 2. If Job Type = "Full-Time", Salary Range must be greater than 0
> 3. The Job Description must be at least 100 characters long
> 4. If Is_Remote = False, a Location field must not be blank
> 5. Experience Years cannot exceed 30

---

**Scenario 3 — Flow Design Challenge**

> "SubscriptionApp" needs a Flow with the following logic:
>
> When an `Account` record is updated and the `Renewal_Date__c` is within 60 days:
> 1. Check if an open Opportunity with Type = "Renewal" already exists for this Account
> 2. If NO → create a new Opportunity: Name = Account Name + " Renewal", Close Date = Renewal Date, Stage = "Prospecting", Amount = `Last_Contract_Value__c`
> 3. If YES → update the existing Opportunity's Close Date to match the Renewal Date
> 4. In both cases → create a Task for the Account Owner: "Review renewal opportunity", Due = Today + 5
>
> **Question:** Design the Flow step by step — list each element (type + purpose), the decision logic, and any variables you'd need.

---

**Scenario 4 — Approval Process Design**

> "DealMaker Corp" needs an Approval Process for Opportunity discounts:
> - Discounts 0-10%: No approval needed
> - Discounts 11-20%: Direct manager must approve (1 step)
> - Discounts 21-30%: Direct manager + VP Sales must both approve (2 steps, sequential)
> - Discounts >30%: Not allowed at all (should be blocked before approval even starts)
>
> **Question:**
> 1. Should the >30% rule be in the Approval Process or a Validation Rule? Why?
> 2. Design the complete Approval Process for 11-30% discounts
> 3. What should the Initial Submission Action do?
> 4. What should the Final Rejection Action do?

---

## ✔️ Answer Key — Practice Questions

1. The rule fires when `Amount` is a **negative number** (less than 0) — meaning the save is blocked. If Amount = 0 or positive, the formula is FALSE and the record saves normally.
2. **Before-Save:** Runs before the record is written to the database; used for updating fields on the same record — faster, no DML count. Example: auto-populate Region based on Billing State. **After-Save:** Runs after the record is committed; used for creating/updating other records. Example: create a Task when a Case is closed.
3. The record is **locked** — the submitting rep cannot make any edits to the record while it is pending approval. They must either wait for the decision or recall the submission to unlock the record.
4. **Flow Builder** — Salesforce has retired both Workflow Rules and Process Builder. All new automation should be built in Flow.
5. The **org's default time zone** as configured in Company Information.
6. A **Fault Path** — connect a fault connector from every element that can fail (Create Records, Update Records, etc.) to a fault-handling screen or email alert.
7. **Yes** — using the **Subflow** element. You call an Auto-launched Flow from within another flow, passing variables in and out.
8. An **Approval Process** requires a **human decision** (approve or reject) at each step. A **Record-Triggered Flow** runs automatically with no human involvement — it executes instantly based on data conditions.
9. The rep sees the error message defined in the Validation Rule, displayed either below the Amount field or at the top of the page. The record is **not saved**. They must correct the Amount to a non-negative value before saving.
10. Before-Save flows update the triggering record's fields **in memory** before it's written to the database — a single database write. After-Save flows must use a separate DML (database write) operation to update the record after it's already been committed, consuming governor limits and being slightly slower.

---

## 🔗 Trailhead Resources

- [Validation Rules](https://trailhead.salesforce.com/content/learn/modules/validation-rules)
- [Business Process Automation](https://trailhead.salesforce.com/content/learn/modules/business_process_automation)
- [Build a Discount Approval Process](https://trailhead.salesforce.com/content/learn/projects/build-a-discount-approval-process)
- [Build Flows with Flow Builder](https://trailhead.salesforce.com/content/learn/trails/build-flows-with-flow-builder)

---

*Previous: [05 · Customization](./05_Customization_and_UI.md) | Next: [07 · Reports & Dashboards →](./07_Reports_and_Dashboards.md)*
