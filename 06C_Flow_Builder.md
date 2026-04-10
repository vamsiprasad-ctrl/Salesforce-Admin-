# 06C · Flow Builder — Complete Guide (All Flow Types)

> **Sub-Topic:** Business Logic & Automation | **Difficulty:** 🔴 Advanced | **Est. Time:** 4 hrs

---

## What is Flow Builder?

**Flow Builder** is Salesforce's primary, most powerful automation tool. It replaces both the retired **Workflow Rules** and **Process Builder**. Flows are visual, point-and-click automation programs that can:

- Collect data from users (Screen Flows)
- React to record changes (Record-Triggered Flows)
- Run on a schedule (Scheduled Flows)
- Be called by other systems or Flows (Auto-launched Flows)
- React to platform events (Platform Event Flows)

> 🔑 **Key Rule:** All new Salesforce automation should be built in Flow Builder. If you see Workflow Rules or Process Builder in an org, they need to be migrated to Flow.

---

## Flow Builder Interface

When you open Flow Builder, you see:

| Area | Purpose |
|------|---------|
| **Canvas** | The visual workspace where you build the flow |
| **Toolbox (left)** | Elements and Resources available to add |
| **Elements palette** | Drag elements onto the canvas |
| **Properties panel (right)** | Configure the selected element |
| **Toolbar (top)** | Save, Activate, Debug, Run |

---

## Flow Elements — Complete Reference

### Data Elements

**Get Records**
Queries Salesforce for records matching conditions. Returns one or multiple records into a variable.

```
Example: Get the Account record where AccountId = {!$Record.AccountId}
```

**Create Records**
Creates one or more new records.

```
Example: Create a Task with Subject = "Follow Up", OwnerId = {!$Record.OwnerId}
```

**Update Records**
Updates one or more existing records.

```
Example: Update Opportunity Stage to "Closed Won"
```

**Delete Records**
Deletes one or more records.

```
Example: Delete all expired Leads (use with extreme care)
```

---

### Logic Elements

**Decision**
Branches the flow into multiple paths based on conditions. Like an IF/ELSE statement.

```
Decision: "Is Amount > 100000?"
  ├── YES → Create Approval Task
  └── NO  → Send Standard Email
```

**Assignment**
Sets the value of a variable or field.

```
Assignment: Set {!EmailSubject} = "Follow up: " & {!$Record.Name}
```

**Loop**
Iterates through a collection of records one by one.

```
Loop through all Contacts on an Account
  → For each Contact: create a Task
```

**Wait**
Pauses the flow until a time-based condition is met (used in Record-Triggered flows for scheduled paths).

---

### Screen Elements (Screen Flows only)

**Screen**
Displays a page to the user with input fields, dropdowns, checkboxes, etc.

**Available Components on Screen:**
- Text Input
- Long Text Area
- Number Input
- Currency Input
- Date/DateTime Input
- Picklist (single/multi-select)
- Checkbox
- Radio Buttons
- Data Table (show records)
- Display Text (static text or formula)
- File Upload
- Lookup (search for existing records)
- Lightning Web Component (custom)

---

### Action Elements

**Send Email**
Sends a single email — compose the to, subject, and body inline.

**Email Alert**
Triggers a pre-configured Email Alert (uses an Email Template). Better for consistent branded emails.

**Post to Chatter**
Posts a message to a record's Chatter feed or a group.

**Submit for Approval**
Programmatically submits a record to an Approval Process from within a Flow.

**Apex Action**
Calls a developer-built Apex method — extends Flow with custom code capabilities.

**Subflow**
Calls another Auto-launched Flow and passes variables in and out.

---

### Error Handling

**Fault Path**
Every element that performs a DML operation (Create, Update, Delete) or calls an external service can fail. A Fault Path handles failures gracefully instead of showing a generic error to the user.

```
Create Records → [Fault Path if fails] → Screen: "Sorry, an error occurred. Please contact support."
                                       → Send Email Alert to Admin: "Flow failed: {!$Flow.FaultMessage}"
```

> ⚠️ **Always add Fault Paths** to production flows. Unhandled faults show a cryptic error to users and give admins no context.

---

## Flow Type 1 — Record-Triggered Flow

**Trigger:** A record is created, updated, or deleted in Salesforce.
**Runs:** In the background — user does not see it happening.
**When to use:** Automate anything that should happen automatically when data changes.

### Sub-Types: Before-Save vs After-Save

#### Before-Save (Fast Path)

Runs **before** the record is written to the database.

| Capability | Available? |
|-----------|-----------|
| Update fields on the TRIGGERING record | ✅ Yes (directly, no DML) |
| Create NEW records (Tasks, Cases, etc.) | ❌ No |
| Update OTHER records | ❌ No |
| Call Apex | ❌ No |
| Send Emails | ❌ No |

**Performance:** Faster — no additional database operations
**Best for:** Populating calculated fields, defaulting values, setting lookups based on other fields

#### After-Save (Standard Path)

Runs **after** the record has been saved to the database.

| Capability | Available? |
|-----------|-----------|
| Create NEW records | ✅ Yes |
| Update OTHER records | ✅ Yes |
| Update the TRIGGERING record | ✅ Yes (uses DML — costs a governor limit) |
| Send Emails | ✅ Yes |
| Call Apex | ✅ Yes |
| Post to Chatter | ✅ Yes |

**Performance:** Slightly slower — additional database operations
**Best for:** Creating related records, sending notifications, updating records across objects

---

### Entry Conditions for Record-Triggered Flows

You can configure when the Flow runs:

| Condition | When It Fires |
|-----------|--------------|
| **Only when a record is created** | New records only |
| **Only when a record is updated** | Edits only (not creation) |
| **When a record is created or updated** | Both — but use ISCHANGED() to avoid running every time |
| **A record is deleted** | After cascade deletes |

**Advanced:** Add a "Run When" condition to further narrow:
- "Only when specific field changes" — `ISCHANGED({!$Record.Status__c})`
- "Only when record meets criteria" — `{!$Record.Amount} > 100000`

---

### Real-World Examples — Record-Triggered Flows

**Example 1: Auto-Set Region (Before-Save)**
> When an Opportunity is created or updated, automatically set `Region__c` based on the Account's Billing State.
>
> Elements:
> 1. Get Records → find Account by `{!$Record.AccountId}`
> 2. Decision → what is `{!Account.BillingState}`?
>    - CA, OR, WA → Assignment: `{!$Record.Region__c} = "West"`
>    - NY, NJ, CT → Assignment: `{!$Record.Region__c} = "East"`
>    - TX, FL → Assignment: `{!$Record.Region__c} = "South"`
>    - Default → Assignment: `{!$Record.Region__c} = "Central"`
> 3. End (record saved with Region populated)

**Example 2: Create Follow-Up Task When Case Closes (After-Save)**
> When a Case's Status changes to "Closed", create a Task for the Account Owner to check on satisfaction.
>
> Entry: Status = Closed AND ISCHANGED(Status)
>
> Elements:
> 1. Get Records → Account for `{!$Record.AccountId}`
> 2. Create Records → Task:
>    - Subject: `"Satisfaction Check: " & {!$Record.CaseNumber}`
>    - Due Date: TODAY() + 3
>    - Owner: `{!Account.OwnerId}`
>    - Related to: `{!$Record.AccountId}`
> 3. Fault Path → Email Admin if Task creation fails

**Example 3: Prevent Stage Regression (Before-Save)**
> If an Opportunity Stage moves backward (e.g., from Proposal back to Prospecting), block it.
>
> Entry: Updated only, stage changed
>
> Decision: Is new Stage "earlier" than prior Stage?
>
> If Yes → Custom Error via the `$GlobalConstant.True` workaround or Validation Rule (better choice here)

**Example 4: Close All Open Opportunities When Account is Deleted (Before-Delete)**
> When an Account record is deleted, close all its Open Opportunities first.
>
> Trigger: Before record is deleted
>
> Elements:
> 1. Get Records → all Opportunities where AccountId = {!$Record.Id} AND StageName ≠ "Closed Won"/"Closed Lost"
> 2. Loop through records
> 3. Assignment inside loop: set `StageName = "Closed Lost"`, `CloseDate = TODAY()`
> 4. Update Records (after loop with full collection)

---

## Flow Type 2 — Screen Flow

**Trigger:** A user explicitly launches it by clicking a button, a Quick Action, a tab, or a Community page.
**Runs:** Interactively — user sees screens and provides input.
**When to use:** Guided data entry, wizards, step-by-step processes.

### Navigation in Screen Flows

Screen Flows support:
- **Next/Previous** navigation between screens
- **Conditional navigation** (skip screens based on earlier inputs)
- **Pause** — save progress and resume later
- **Back** — go to previous screen
- **Finish** — complete the flow (redirect to a record or URL)

### Where Screen Flows Can Live

| Location | How |
|----------|-----|
| Quick Action on a record | Add as Quick Action → appears in record's action bar |
| Lightning App page tab | Add Flow component to App Builder page |
| Button on a page | Custom button launches the flow |
| Community page | Experience Builder Flow component |
| Utility Bar | Appears in the Service Console toolbar |
| Lightning Home page | Widget on the user's home page |

---

### Real-World Examples — Screen Flows

**Example 1: New Patient Onboarding Wizard**
> Healthcare clinic onboards a new patient in 4 screens:
>
> Screen 1: Personal Details — Name, DOB, Gender, Contact
> Screen 2: Insurance Information — Provider, Policy Number, Group Number
> Screen 3: Emergency Contact — Name, Relationship, Phone
> Screen 4: Appointment — Preferred Doctor, Date, Time
>
> After Screen 4:
> - Create Account (Patient)
> - Create Contact (linked to Account)
> - Create Case (Appointment Request)
> - Create Event (scheduled appointment)
> - Display confirmation screen with record numbers

**Example 2: Job Application Wizard (Built on Job Portal)**
> Quick Action on `Job_Posting__c` that lets candidates apply:
>
> Screen 1: Applicant Details — Name, Email, Phone, LinkedIn
> Screen 2: Experience — Years of Experience, Current Role, Notice Period
> Screen 3: Upload Resume (File Upload component)
>
> After Screen 3:
> - Create Job_Application__c record
> - Attach uploaded file
> - Send confirmation email to applicant
> - Send internal notification to recruiter

**Example 3: Guided Case Resolution**
> Support console button "Guided Resolution" that walks an agent through a troubleshooting checklist:
>
> Screen 1: Problem Category (picklist)
> Screen 2 (varies by category): Diagnostic questions
> Screen 3: Suggested resolution steps
> Screen 4: Resolution confirmation — did it work? (Yes/No)
>
> If Yes: Close the case automatically
> If No: Escalate to Tier 2 and set Priority = High

---

## Flow Type 3 — Scheduled Flow

**Trigger:** A date/time schedule — once, daily, weekly, monthly, or at a specific time relative to a date field on a record.
**Runs:** In the background — no user interaction.
**When to use:** Any automation that should run on a recurring time basis.

### Schedule Configuration Options

| Option | Example |
|--------|---------|
| **Run once** | A specific date and time |
| **Daily** | Every day at 8:00 AM |
| **Weekly** | Every Monday at 7:00 AM |
| **Hourly** | Every 4 hours (advanced scheduling) |
| **Relative to a date field** | 30 days before `Renewal_Date__c` |

### How Scheduled Flows Work

1. Scheduled Flow runs at the configured time
2. It queries for records meeting the "Start conditions"
3. It processes each record through the flow logic
4. Actions are applied to matching records

> ⚠️ **Time zone:** Scheduled Flows run in the **org's default time zone** (set in Company Information), NOT the admin's time zone.

---

### Real-World Examples — Scheduled Flows

**Example 1: Subscription Renewal Reminders**
> Every day at 8AM, find all Subscription records where `Renewal_Date__c = TODAY() + 30`. For each:
> - Send email: "Your subscription expires in 30 days — renew now!"
> - Create a Task for the Account Manager: "Follow up on renewal"
> - Set `Renewal_Reminder_Sent__c = TRUE`

**Example 2: Auto-Close Stale Opportunities**
> Every Monday at 6AM, find all Opportunities where:
> - Stage ≠ Closed Won/Lost
> - Last Activity Date < 90 days ago
> - Owner hasn't been active in 30 days
>
> For each: Set Stage = "Closed Lost", set `Close_Reason__c = "Stale — no activity"`, notify Sales Manager

**Example 3: Lead Aging**
> Every night at midnight, find all Leads where:
> - Status = "New" AND Created Date < 5 days ago
>
> For each: Set Status = "Aged — Needs Attention", assign to Lead Recycling Queue, send alert to Sales Ops

**Example 4: Auto-Close Expired Job Postings**
> Every night at midnight, find all `Job_Posting__c` records where:
> - `Application_Deadline__c < TODAY()`
> - `Status__c ≠ "Closed"`
>
> For each: Set `Status__c = "Closed"`, send email to Recruiter: "Your posting [Job Title] has been automatically closed."

---

## Flow Type 4 — Auto-launched Flow

**Trigger:** Called by another Flow, Apex code, REST API, or an Approval Process.
**Runs:** In the background — no UI.
**When to use:** Reusable logic that multiple other processes need to call.

### Why Use Auto-launched Flows?

**Without Subflows (bad approach):**
- Flow 1 (Lead conversion) → duplicates email-sending logic
- Flow 2 (Opportunity creation) → duplicates same email-sending logic
- Flow 3 (Case creation) → duplicates same email-sending logic

**With Subflows (good approach):**
- Auto-launched Flow: "Send Welcome Email" — build once
- Flow 1 calls it ✅
- Flow 2 calls it ✅
- Flow 3 calls it ✅
- When the email template changes, you update ONE flow, not three

### Passing Variables In and Out

Auto-launched Flows can accept **input variables** and return **output variables**.

```
Parent Flow → calls Subflow
              → passes in: {!AccountId}, {!ContactName}
              ← receives back: {!EmailSentSuccess} (Boolean), {!ErrorMessage} (Text)
```

---

### Real-World Examples — Auto-launched Flows

**Example 1: Reusable Email Notification**
> Auto-launched Flow: `Send_Assignment_Notification`
> 
> Input variables: `RecordId` (String), `RecordType` (String), `AssigneeId` (String)
>
> Logic: Build email subject and body based on `RecordType` → Send to `AssigneeId`
>
> Called by: Lead Assignment Flow, Case Assignment Flow, Opportunity Assignment Flow

**Example 2: Record Validation Helper**
> Auto-launched Flow: `Validate_Customer_Data`
>
> Input: Account record
> Logic: Check multiple data quality rules → return a list of error messages
> Output: `ErrorMessages` (Text Collection), `IsValid` (Boolean)
>
> Called by: Account creation screen flow, Lead conversion flow, bulk import data cleaning flow

---

## Flow Type 5 — Platform Event-Triggered Flow

**Trigger:** A Platform Event message is published (from Apex, another Flow, or an external system).
**Runs:** In the background.
**When to use:** Event-driven integrations — react to something that happened in an external system.

### Real-World Example

> **Scenario:** An external HR system publishes a `New_Employee_Joined__e` Platform Event when a new employee is onboarded in the HR system.
>
> **Platform Event-Triggered Flow reacts:**
> 1. Creates a Salesforce User record for the new employee
> 2. Assigns the correct Profile and Permission Sets
> 3. Creates an Account record for their department
> 4. Sends a welcome email with Salesforce login instructions
>
> All of this happens automatically the moment the external HR system fires the event — no manual admin work.

---

## Flow Variables — Complete Reference

Variables store data within a flow execution.

| Variable Type | Stores | Example |
|--------------|--------|---------|
| **Text** | String values | `{!FullName}` = "John Smith" |
| **Number** | Numeric | `{!DaysRemaining}` = 30 |
| **Currency** | Money | `{!TotalAmount}` = 50000 |
| **Boolean** | True/False | `{!IsVIP}` = True |
| **Date** | Calendar date | `{!FollowUpDate}` = 15/06/2026 |
| **Date/Time** | Date + time | `{!MeetingTime}` |
| **Record** (Single) | One Salesforce record | `{!RelatedAccount}` |
| **Record** (Collection) | Multiple records | `{!AllOpenCases}` |
| **Text Collection** | Multiple text values | `{!ErrorMessages}` |
| **Constant** | Fixed unchanging value | `{!MaxDiscount}` = 30 |
| **Formula** | Calculated | `{!DaysUntilClose}` |

---

## Flow Best Practices

### 1. Always Add Fault Paths
Every DML element (Create, Update, Delete) and every Apex Action can fail. Always connect a fault path.

```
Create Records ──[Success]──→ Continue
               └─[Fault]────→ Screen: "Error occurred" + Email Admin
```

### 2. Bulkify Your Loops

❌ **Wrong — DML inside a loop:**
```
Loop through 200 Contacts:
  → Update each Contact one by one
  = 200 separate database writes (hits governor limits)
```

✅ **Correct — collect then bulk update:**
```
Loop through 200 Contacts:
  → Add to a collection variable (no DML)
After loop:
  → Update Records using the collection (1 database write)
```

### 3. Use Before-Save for Field Updates

If you only need to update fields on the triggering record — use Before-Save. It's faster and more efficient.

❌ Wrong: After-Save Flow to update `Region__c` on the same Opportunity
✅ Right: Before-Save Flow to update `Region__c` on the same Opportunity

### 4. Use ISCHANGED() to Prevent Infinite Loops

An After-Save flow that updates the same record triggers itself again on the update — potentially creating an infinite loop.

Use entry conditions with `ISCHANGED()` to only fire when the relevant field actually changes:

```
Run only when: Status__c has changed (ISCHANGED = true)
AND: Prior value was NOT "Closed"
```

### 5. Name Everything Clearly

❌ Bad: `Get Records 1`, `Decision 1`, `Assignment 2`
✅ Good: `Get_Related_Account`, `Decision_Is_Enterprise_Customer`, `Set_Region_West`

### 6. Use Subflows for Reusable Logic

Identify patterns you build multiple times. Extract them into Auto-launched Flows. Call them via Subflow element.

### 7. Test in Debug Mode

Flow Builder has a **Debug** button. Click it to step through your flow execution, see variable values at each step, and identify exactly where failures occur.

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Record-Triggered Flow (Before-Save): Auto-Set Application Date

**Objective:** When a Job Application is created, automatically set `Application_Date__c` to today.

**Steps:**
1. Setup → Process Automation → Flows → **New Flow**
2. Select: **Record-Triggered Flow**
3. Object: `Job_Application__c`
4. Trigger: **A record is created**
5. Optimize for: **Before the record is saved**
6. Click Done → Canvas opens

**Add Assignment element:**
7. Drag **Assignment** from Toolbox to canvas
8. Connect Start → Assignment
9. Configure Assignment:
   - Variable: `{!$Record.Application_Date__c}`
   - Operator: `Equals`
   - Value: `{!$Flow.CurrentDate}`
10. Connect Assignment → End

**Save and Activate:**
11. Click **Save** → Flow Label: `Set Application Date on Create`
12. Click **Activate**

**Test:**
13. Create a new `Job_Application__c` record WITHOUT filling in Application_Date__c
14. Save
15. Open the record → Application_Date__c should be today's date automatically

---

### 🔧 Task 2 — Record-Triggered Flow (After-Save): Create Task When Deadline Approaches

**Objective:** When a Job Posting's deadline is within 7 days, create a Task for the Recruiter.

**Steps:**
1. New Flow → Record-Triggered Flow
2. Object: `Job_Posting__c`
3. Trigger: **A record is created or updated**
4. Optimize for: **After the record is saved**

**Add Entry Conditions:**
5. Add condition:
   - Resource: `{!$Record.Application_Deadline__c}`
   - Operator: Less Than Or Equal
   - Value: Formula: `{!$Flow.CurrentDate} + 7`
6. Add second condition (AND):
   - `ISCHANGED({!$Record.Application_Deadline__c})`

**Add Create Records element:**
7. Drag **Create Records** onto canvas
8. Configure:
   - Object: **Task**
   - Field: `Subject` = `"Review Applications: " + {!$Record.Name}` (use formula)
   - Field: `ActivityDate` = `{!$Record.Application_Deadline__c}`
   - Field: `OwnerId` = `{!$Record.Recruiter__c}`
   - Field: `Status` = `Not Started`
   - Field: `Priority` = `High`
   - Field: `WhatId` = `{!$Record.Id}`

**Add Fault Path:**
9. Click the gear icon on Create Records → Add Fault Connector
10. Connect fault to a **Screen** or **Custom Notification**
11. Screen: "Task creation failed. Please contact admin."

**Save → `Create Deadline Review Task` → Activate**

**Test:** Update a Job Posting's deadline to today + 5 → Task appears in the Recruiter's Tasks.

---

### 🔧 Task 3 — Screen Flow: Apply for Job Wizard

**Objective:** Create a guided application form launched from a Job Posting.

**Steps:**
1. New Flow → **Screen Flow**

**Screen 1: Applicant Details**
2. Drag **Screen** element to canvas
3. Screen Label: `Applicant Details`
4. Add components:
   - **Text Input**: API Name `ApplicantName`, Label `Full Name`, Required ✅
   - **Text Input**: API Name `ApplicantEmail`, Label `Email Address`, Required ✅
   - **Phone Input**: API Name `ApplicantPhone`, Label `Phone Number`
   - **Picklist**: API Name `ExperienceYears`, Label `Years of Experience`
     - Values: 0-1 year, 1-3 years, 3-5 years, 5-10 years, 10+ years

**After Screen 1 — Create Record:**
5. Drag **Create Records** element
6. Object: `Job_Application__c`
7. Map:
   - `Name` = `{!ApplicantName}`
   - `Applicant_Email__c` = `{!ApplicantEmail}`
   - `Application_Status__c` = `Applied`
   - `Application_Date__c` = `{!$Flow.CurrentDate}`

**Screen 2: Confirmation**
8. Drag another **Screen** element
9. Screen Label: `Thank You`
10. Add **Display Text** component:
    - Text: `Thank you, {!ApplicantName}! Your application has been submitted successfully.`

**Add Fault Path after Create Records:**
11. If create fails → show error screen

12. Save → `Apply for Job` → Activate

**Create Quick Action:**
13. Setup → Object Manager → `Job_Posting__c` → Buttons, Links, Actions → **New Action**
14. Action Type: **Flow**
15. Flow: `Apply for Job`
16. Label: `Apply for This Job`
17. Save
18. Add to page layout

**Test:** Open a Job Posting → click "Apply for This Job" → complete wizard → verify Job Application created.

---

### 🔧 Task 4 — Scheduled Flow: Auto-Close Expired Postings

**Objective:** Nightly auto-close Job Postings past their deadline.

**Steps:**
1. New Flow → **Scheduled Flow**

**Configure Schedule:**
2. Flow Label: `Auto Close Expired Job Postings`
3. Run: **Daily**
4. Time: **11:45 PM**
5. Start Date: Today

**Configure Start Object and Conditions:**
6. Object: `Job_Application__c`... wait — no. This flow targets `Job_Posting__c` records.
7. Actually for Scheduled Flows, go to the Start element → configure:
   - Object: `Job_Posting__c`
   - Conditions:
     - `Application_Deadline__c < {!$Flow.CurrentDate}`
     - AND `Status__c != Closed`
   - Automatically process each record

**Add Update Records:**
8. Drag **Update Records**
9. Update: the records from the scheduled path
10. Set: `Status__c = Closed`

**Add Email (Optional):**
11. After update, Add **Send Email**:
    - To: `{!$Record.Recruiter__c.Email}`
    - Subject: `"Auto-Closed: " + {!$Record.Name}`
    - Body: `"Your job posting has been automatically closed as the Application Deadline has passed."`

12. Save → Activate

**Verify:** Check Job Postings with past deadlines the next morning — they should be closed.

---

### 🔧 Task 5 — Add Error Handling to All Your Flows

**Objective:** Make production-ready flows with proper fault paths.

**Steps:**
1. Open `Create Deadline Review Task` flow
2. Click the **Create Records** element
3. Click the **gear icon** → **Add Fault Connector**
4. A red fault path connector appears
5. Drag a **Screen** element for user-facing error display:
   - Message: "We couldn't create the review task. Error: {!$Flow.FaultMessage}"
6. Alternatively, drag a **Send Email** element:
   - To: admin email
   - Subject: "Flow Error: Create Deadline Review Task"
   - Body: `"Error at Create Records: {!$Flow.FaultMessage} | Record ID: {!$Record.Id}"`
7. Connect the fault path to this element
8. Save → reactivate

---

### 🔧 Task 6 — Debug a Flow

**Objective:** Learn to use the Flow Debug tool.

**Steps:**
1. Open any of your flows in Flow Builder
2. Click **Debug** (top toolbar)
3. In the debug panel:
   - If Record-Triggered: enter a Record ID to simulate
   - If Screen Flow: it will run interactively
4. Click **Run**
5. Watch the flow execute step by step — each element highlights as it runs
6. Check variable values in the Debug panel at each step
7. If a fault occurs: the Debug panel shows exactly which element failed and the fault message

---

## 📝 Practice Questions — Flow Builder

**Q1.** What is the main difference between a Before-Save and After-Save Record-Triggered Flow?

**Q2.** A Scheduled Flow is configured to run at 8:00 AM. In whose time zone does it run?

**Q3.** You build a Flow that loops through 500 records and creates a Task for each inside the loop. What problem will this cause?

**Q4.** What Flow element should you always add to any element that performs a database operation?

**Q5.** Can a Screen Flow automatically run without a user clicking anything?

**Q6.** What Flow type would you use to build a wizard that guides a support agent through a 5-step troubleshooting process?

**Q7.** What is a Subflow, and what Flow type is used as a subflow?

**Q8.** A Record-Triggered Flow on Opportunity updates the Opportunity's Region field every time ANY field on the Opportunity is saved. What is the performance problem, and how do you fix it?

**Q9.** What is the `{!$Flow.FaultMessage}` variable used for?

**Q10.** What is the difference between **Send Email** and **Email Alert** elements in a Flow?

**Q11.** An After-Save Flow that updates the triggering Opportunity causes the Opportunity to be saved again, which re-triggers the Flow. What is this called and how do you prevent it?

**Q12.** Where can Screen Flows be placed in the Salesforce UI? Name at least 4 locations.

---

## 🎯 Scenario-Based Questions — Flow Builder

---

**Scenario 1 — Flow Type Selection**

> "InsureCo" has 5 automation requirements. Choose the correct Flow type for each and explain:
>
> 1. When a Policy is marked "Active", create a renewal reminder for 30 days before the end date
> 2. Every morning at 7AM, find all Policies expiring in 7 days and send reminder emails
> 3. An agent needs a guided 4-step wizard to register a new insurance claim
> 4. A reusable process that validates an address format — called by 6 different other flows
> 5. When a Claim record is Approved, automatically create an Invoice in their SAP system (via Platform Event)

---

**Scenario 2 — Flow Design: Renewal Opportunity Creator**

> "SaaS Co." needs a Flow with this exact logic:
>
> - Trigger: Account is updated
> - Run when: `Renewal_Date__c` is within 60 days AND this field was just changed
> - Logic:
>   1. Query for existing open Opportunity with `Type = "Renewal"` for this Account
>   2. If found → update its Close Date to match `Renewal_Date__c`
>   3. If NOT found → create new Opportunity: Name = Account Name + " - Renewal", Stage = Prospecting, Close Date = Renewal_Date__c, Amount = `Last_ARR__c`
>   4. In BOTH cases → create a Task for Account Owner: Subject = "Review Renewal", Due = Today + 5
>
> **Question:** Draw the flow step by step — list every element, its type, what it does, and what it connects to.

---

**Scenario 3 — Debugging a Broken Flow**

> An admin built an After-Save Record-Triggered Flow on the Opportunity object. The flow is supposed to:
> - When Stage changes to "Closed Won" → set `Won_Date__c = TODAY()` → create a "Congratulations" Chatter post
>
> Users are reporting: "Every time we save ANY field on an Opportunity, the Chatter post appears even for unrelated edits."
>
> **Question:**
> 1. What is the root cause of this bug?
> 2. What specific Flow configuration is missing?
> 3. Write the exact entry condition that would fix it
> 4. Why would Before-Save be better for setting `Won_Date__c` but After-Save is still needed for the Chatter post?

---

**Scenario 4 — Bulkification Challenge**

> A Flow loops through 300 Contact records and for each Contact:
> - Gets the related Account (Get Records inside the loop)
> - Creates a Task (Create Records inside the loop)
>
> During a data import of 300 Contacts, the Flow fails with a "Too many SOQL queries" error.
>
> **Question:**
> 1. What is the governor limit being hit?
> 2. Explain exactly why having Get Records and Create Records inside a loop causes this
> 3. Redesign the Flow structure to be bulkification-safe (describe each element and where to move them)

---

## ✔️ Answer Key — Practice Questions

1. **Before-Save:** Runs before the record is committed to the database. Can only update fields on the triggering record (no DML). Faster. **After-Save:** Runs after the record is committed. Can create/update/delete other records. Slightly slower because it performs additional database operations.
2. The **org's default time zone** as configured in Setup → Company Information → Default Time Zone.
3. **Governor limit violation** — "Too many DML statements." Salesforce limits each transaction to 150 DML operations. Creating 500 Tasks in a loop = 500 separate DML statements. Fix: collect Tasks in a variable collection inside the loop, then use ONE Create Records element after the loop.
4. A **Fault Path** (Fault connector from the element to an error-handling screen or email alert).
5. **No** — Screen Flows require a user to explicitly launch them by clicking a button, quick action, or navigating to a page that contains the flow. They are inherently interactive.
6. **Screen Flow** — it is specifically designed for interactive, guided user experiences with multiple screens and user inputs.
7. A **Subflow** is when one Flow calls another Flow using the **Subflow element**. The called flow must be an **Auto-launched Flow**. Variables can be passed in and output variables returned back.
8. The flow triggers every time ANY field on the Opportunity is saved — even unrelated fields. This is inefficient. Fix: add an entry condition — `ISCHANGED({!$Record.Region__c})` or whichever field the Region calculation depends on — so the flow only runs when relevant data actually changes.
9. `{!$Flow.FaultMessage}` is a system variable that contains the **error message text** when a flow element fails. It is used in Fault Paths to display a meaningful error to users or include in admin notification emails.
10. **Send Email** composes the email inline within the flow — you type the body and subject directly. **Email Alert** uses a pre-built Email Template and can send to multiple recipients defined in the template — better for consistently formatted, branded communications.
11. An **infinite loop** (or recursive flow). Prevent it by: (a) using Before-Save instead (doesn't re-trigger), (b) adding a condition checking `ISCHANGED()` so the flow only fires when relevant data changes, (c) using a checkbox field (`Flow_Processed__c`) that the flow sets to TRUE and checking `= FALSE` as an entry condition.
12. Screen Flows can be placed in: Quick Actions on a record, Lightning App Builder pages (App pages, Record pages, Home pages), Experience Cloud (Community) pages, Utility Bar in Service Console, Lightning Component (embedded), Flow URL (direct link), Visualforce pages.

---

## 🔗 Trailhead Resources

- [Build Flows with Flow Builder (Trail)](https://trailhead.salesforce.com/content/learn/trails/build-flows-with-flow-builder)
- [Business Process Automation](https://trailhead.salesforce.com/content/learn/modules/business_process_automation)

---

*Previous: [06B · Approval Processes](./06B_Approval_Processes.md) | Next: [06D · Automation Comparison & Migration →](./06D_Automation_Comparison.md)*
