# 06B · Approval Processes — Complete Guide

> **Sub-Topic:** Business Logic & Automation | **Difficulty:** 🟡 Intermediate | **Est. Time:** 2 hrs

---

## What is an Approval Process?

An **Approval Process** is a formal workflow that routes a Salesforce record to one or more designated people for **review and human decision** — Approve or Reject — before a business action is taken.

Unlike Flows that run automatically, Approval Processes require a **human being** to make a decision at each step. This makes them ideal for:

- Financial approvals (discounts, budgets, expenses)
- Compliance sign-offs (legal review, compliance check)
- HR decisions (offer letters, promotions, leave requests)
- IT requests (access grants, software purchases)

---

## Anatomy of an Approval Process

```
Record meets Entry Criteria
         ↓
  Initial Submission Actions fire
         ↓
    Step 1: Route to Approver
         ↓
    Approver Decision:
    ├── Approved → Step 2 (if exists) OR Final Approval Actions
    └── Rejected → Rejection Actions for this step
         ↓
    (If multiple steps, repeat)
         ↓
  Final Approval Actions (all steps approved)
  OR Final Rejection Actions (any step rejected)
```

---

## Key Components of an Approval Process

### 1. Entry Criteria

Defines which records trigger this Approval Process. Only records meeting these criteria can be submitted.

**Example:**
- `Discount_Percentage__c > 10` — only discounts above 10% need approval
- `Amount >= 500000` — only deals ₹5L or above
- `Job_Type__c = "Full-Time"` — only full-time offers need approval

### 2. Approval Steps

Each step represents one level of review. You can have multiple sequential steps.

**Step Configuration:**
- **Approver type options:**
  - A specific user (e.g., always the CFO)
  - The submitter's manager (from the User hierarchy field)
  - A specific role (e.g., VP Sales role)
  - A queue (any member can approve)
  - A related user field (e.g., Account Owner)
- **Approval Criteria:** You can add criteria to determine if a step is relevant (skip if amount < 1L)
- **Reject Behavior:** Final rejection (stops the entire process) or go back to first step

### 3. Actions

| Action Type | When It Fires |
|-------------|--------------|
| **Initial Submission Actions** | The moment a user submits the record for approval |
| **Approval Actions (per step)** | When an approver approves at that specific step |
| **Rejection Actions (per step)** | When an approver rejects at that specific step |
| **Final Approval Actions** | When ALL steps are approved |
| **Final Rejection Actions** | When any step is rejected and the process ends |
| **Recall Actions** | When the submitter recalls (withdraws) the submission |

### 4. Action Types Available

- **Field Update** — change a field value (e.g., `Approval_Status__c = "Approved"`)
- **Email Alert** — send a templated email to specified recipients
- **Task** — create a Task record
- **Outbound Message** — send data to an external system
- **Flow** — trigger a Flow (powerful for complex post-approval logic)

---

## Locking Records During Approval

When a record is submitted for approval, it is **locked by default** — no one can edit it while the approval is pending. This prevents:
- The submitter from changing the record after submission
- Other users from modifying data that is under review

**Who can edit locked records:**
- System Administrators (always)
- Designated Approval Administrators

The record is automatically unlocked when:
- Final approval is granted
- The record is rejected
- The submitter recalls the submission

---

## How Submission Works

### Manual Submission
A user clicks the **"Submit for Approval"** button on the record. This button only appears if the record meets the Entry Criteria.

### Automatic Submission
Configure the Approval Process to submit automatically when a record is saved and meets Entry Criteria — no button needed.

**Use case:** A rep saves an Opportunity with Discount > 10% → automatically submitted for approval without any additional action.

---

## Multi-Step Approval Example

### Real-World Example — Expense Report Approval

> **Company:** "ConsultCo" has this expense approval policy:
> - Expenses ≤ ₹5,000: no approval needed
> - Expenses ₹5,001 – ₹25,000: Project Manager must approve
> - Expenses ₹25,001 – ₹1,00,000: Project Manager + Finance Director must both approve
> - Expenses > ₹1,00,000: PM + Finance Director + CFO (3-step sequential approval)

**Approval Process Setup:**

**Entry Criteria:** `Amount__c > 5000`

**Step 1: Project Manager Approval**
- Approver: Submitter's Manager (assumes PM is direct manager)
- Step Criteria: Always include (all submitted expenses need PM sign-off first)
- If Approved: Proceed to Step 2
- If Rejected: Final Rejection

**Step 2: Finance Director Approval**
- Approver: Finance Director role
- Step Criteria: `Amount__c > 25000` (skip this step if ≤ ₹25,000, go straight to Final Approval)
- If Approved: Proceed to Step 3
- If Rejected: Final Rejection

**Step 3: CFO Approval**
- Approver: CFO role
- Step Criteria: `Amount__c > 100000` (only for expenses above ₹1L)
- If Approved: Final Approval
- If Rejected: Final Rejection

**Initial Submission Actions:**
- Field Update: `Expense_Status__c = "Pending Approval"`
- Lock record

**Final Approval Actions:**
- Field Update: `Expense_Status__c = "Approved"`
- Email Alert: notify Finance team to process payment
- Unlock record

**Final Rejection Actions:**
- Field Update: `Expense_Status__c = "Rejected"`
- Email Alert: notify submitter with rejection reason
- Unlock record

---

## Real-World Example — Discount Approval at a Sales Org

> **Company:** "SalesMax" has a tiered discount approval policy.
>
> - Discount 0–10%: Reps can set freely — no approval (handled by Validation Rule blocking > 10% for unapproved reps)
> - Discount 11–20%: Direct manager must approve
> - Discount > 20%: Manager + VP Sales both must approve
>
> **Entry Criteria:** `Discount_Percentage__c > 10`
>
> **Step 1: Manager Approval**
> - Approver: Submitter's Manager
> - Step Criteria: None (all submitted discounts need manager first)
> - If Approved AND Discount ≤ 20%: Final Approval
> - If Approved AND Discount > 20%: Go to Step 2
>
> **Step 2: VP Sales Approval**
> - Approver: VP Sales role
> - Step Criteria: `Discount_Percentage__c > 20`
> - If Approved: Final Approval
>
> **Initial Submission Actions:**
> - Field Update: `Discount_Approval_Status__c = "Pending Manager"`
> - Lock Opportunity record
>
> **Final Approval Actions:**
> - Field Update: `Discount_Approval_Status__c = "Approved"`
> - Email Alert: "Your discount request has been approved!"
> - Unlock record
>
> **Final Rejection Actions:**
> - Field Update: `Discount_Percentage__c = 10` (reset to max allowed)
> - Field Update: `Discount_Approval_Status__c = "Rejected"`
> - Email Alert: "Your discount request was not approved. Discount reset to 10%."
> - Unlock record

---

## Approval Routing Options in Detail

### Option 1: Automatically Assigned Approver

Salesforce uses a hierarchy field on the User record to find the approver.

**Standard hierarchy field:** "Manager" field on User

**Example:** Rep Sarah submits → Salesforce looks at Sarah's User record → finds her Manager = John → sends approval request to John.

### Option 2: User Chooses the Approver

When submitting, the user selects from eligible approvers. Useful when the appropriate approver isn't always predictable from a hierarchy.

### Option 3: Specific User

Always routes to the same person — e.g., always the Legal team lead.

### Option 4: Role / Queue

Routes to any user in a specific role or queue. Whoever picks it up first processes it. Good for team-based approvals.

### Option 5: Related User Field

Uses a Lookup field on the record itself to determine the approver. 

**Example:** `Opportunity.Account_Manager__c` — the account manager listed on the Opportunity is the approver, regardless of the rep's management chain.

---

## Approval History Related List

Every record has an **Approval History** related list (once an Approval Process is active) that shows:

| Column | Shows |
|--------|-------|
| Date | When each action occurred |
| Status | Submitted, Approved, Rejected, Recalled |
| Assigned To | Who the approval was sent to |
| Actual Approver | Who actually approved/rejected |
| Comments | Approver's comments (optional) |

---

## Approval Notifications

Approvers receive notifications via:
1. **Email** — a notification email with Approve/Reject links (approvers can approve directly from email)
2. **Salesforce Home Page** — "Items to Approve" component shows pending approvals
3. **Bell Notification** — in-app notification in Lightning Experience
4. **Salesforce Mobile App** — push notification

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Build a Single-Step Approval Process

**Business Scenario:** Any Job Application with Status = "Offered" must be approved by HR Manager before the offer is formalized.

**Steps:**
1. Setup → Process Automation → **Approval Processes**
2. Object: `Job_Application__c`
3. Click **Create New Approval Process** → **Use Standard Setup Wizard**

**Page 1 — Name:**
- Process Name: `Job Offer Approval`
- API Name: `Job_Offer_Approval`
- Description: `Requires HR Manager approval before any job offer is sent`

**Page 2 — Entry Criteria:**
- Criteria: `Field: Application_Status__c, Operator: equals, Value: Offered`

**Page 3 — Approver:**
- Automatically assign using: `Manager` (submitter's manager)
- Allow approver to delegate: ✅

**Page 4 — Initial Submission Actions:**
- Add Action → **Field Update**
  - Name: `Set Status to Pending`
  - Field: `Application_Status__c`
  - Value: `Pending Approval`

**Page 5 — Review:**
- Save

**Now configure Actions:**
4. Click into the saved process → **Approval Steps** → **New Approval Step**
5. Step Name: `HR Manager Review`, Order: 1
6. Step Criteria: All records enter this step
7. Approver: Submitter's manager
8. If Approved → Final Approval | If Rejected → Final Rejection

9. **Final Approval Actions:**
   - Field Update: `Application_Status__c = "Approved-Offer Sent"`
   - Email Alert: notify applicant

10. **Final Rejection Actions:**
    - Field Update: `Application_Status__c = "Rejected"`
    - Field Update: record unlocked

11. **Activate** the process

**Test:**
1. Open a Job Application → change Status to "Offered" → Save
2. "Submit for Approval" button appears → click it
3. Log in as the manager user → check Home page for pending approval
4. Approve the request
5. Verify Status = "Approved-Offer Sent"

---

### 🔧 Task 2 — Build a Two-Step Sequential Approval

**Business Scenario:** High-salary job postings (Salary > ₹20L) need both Team Lead AND HR Director approval.

**Steps:**
1. New Approval Process on `Job_Posting__c`
2. Entry Criteria: `Salary_Range__c > 2000000`
3. Step 1: Route to submitter's Manager (Team Lead approval)
4. Step 2: Route to a specific user (HR Director) — only after Step 1 is approved
5. Initial Submission: Lock record + set `Posting_Status__c = "Under Review"`
6. Final Approval: Unlock + set `Posting_Status__c = "Approved"`
7. Final Rejection: Unlock + set `Posting_Status__c = "Draft"` + email to submitter
8. Activate

---

### 🔧 Task 3 — Test Recall Action

**Objective:** Understand what happens when a submitter recalls a submission.

**Steps:**
1. Submit a Job Application for approval (from Task 1)
2. BEFORE the manager approves, return to the record
3. Notice the record is locked (fields not editable, no Save button)
4. Click **Recall Approval Request**
5. Verify: record is unlocked, Status reverts to previous value
6. Check Approval History — shows "Recalled" entry

---

### 🔧 Task 4 — View Approval History

**Steps:**
1. Open any Job Application that has been through an approval
2. Scroll to the **Approval History** related list
3. Identify each entry: what happened, when, by whom
4. Note the Comments column — approvers can leave notes when approving/rejecting
5. Try adding a comment during a test approval: Manager logs in → clicks Approve → adds comment "Reviewed with HR team — approved" → confirm comment shows in history

---

### 🔧 Task 5 — Approve via Email

**Objective:** Test the email-based approval flow.

**Steps:**
1. Make sure your admin user has a valid email set
2. Submit a Job Application for approval
3. Check the approver's email inbox — find the approval notification
4. The email should have **Approve** and **Reject** links
5. Click Approve from the email
6. Return to Salesforce → verify the record status updated correctly

---

## 📝 Practice Questions — Approval Processes

**Q1.** What is the key difference between an Approval Process and a Record-Triggered Flow?

**Q2.** When a record is submitted for approval, what happens to it by default?

**Q3.** A rep submits an Opportunity for approval. The manager rejects it. What determines whether the record goes back to "Draft" or is permanently rejected?

**Q4.** What is the "Recall Approval Request" action, and who can perform it?

**Q5.** Can an Approver approve a record via email without logging into Salesforce?

**Q6.** What are "Initial Submission Actions" and when do they fire exactly?

**Q7.** A company has 3 sequential approval steps. An approver at Step 2 rejects the request. What happens to Step 3?

**Q8.** What is the difference between "Approval Actions (per step)" and "Final Approval Actions"?

**Q9.** Can you have more than one active Approval Process on the same object at the same time?

**Q10.** What happens if the designated approver is inactive or unavailable?

---

## 🎯 Scenario-Based Questions — Approval Processes

---

**Scenario 1 — Expense Reimbursement System**

> "AccountingFirm" processes employee expense reimbursements. Their policy:
>
> | Amount | Approval Required |
> |--------|------------------|
> | ≤ ₹5,000 | No approval — auto-approved |
> | ₹5,001 – ₹25,000 | Team Manager |
> | ₹25,001 – ₹1,00,000 | Team Manager + Finance Head |
> | > ₹1,00,000 | Team Manager + Finance Head + CFO |
>
> Additional rules:
> - During submission, the expense report must be locked
> - Rejected expenses must notify the employee with the reason
> - Approved expenses must trigger a payment notification to Finance
> - Employees can recall their submission before it's processed
>
> **Question:** Design the complete Approval Process including all steps, criteria, and all actions at each stage.

---

**Scenario 2 — Approval Routing Decision**

> "TechBridge" is designing an Approval Process for new Contracts. They have 3 options for routing to the approver:
> 1. Always route to the CFO (specific user)
> 2. Route to the submitter's manager (hierarchy-based)
> 3. Route to whoever is listed in the Contract's `Legal_Reviewer__c` lookup field (related user field)
>
> **Question:** For each of the following business situations, which routing option is best and why?
>
> A. All contracts must be reviewed by the same legal counsel regardless of who submits
> B. Contracts should be reviewed by the relevant department head (sales contracts → Sales VP, HR contracts → HR Director) and this is tracked in a field on the contract
> C. Contracts under ₹5L are reviewed by the direct manager, contracts above by the department VP

---

**Scenario 3 — Recall vs Rejection**

> A sales rep, James, submits an Opportunity for discount approval. His manager is on vacation and won't be back for 2 weeks. The deal is time-sensitive — the customer needs a decision today.
>
> **Question:**
> 1. What options does James have to get this resolved quickly?
> 2. If James recalls the submission, what happens to the Opportunity?
> 3. If James's company has set up "Delegated Approvers," how would that help?
> 4. What preventive measure could an admin implement to avoid this situation in the future?

---

## ✔️ Answer Key — Practice Questions

1. An **Approval Process** requires a **human** to approve or reject at each step — it cannot proceed without human action. A **Record-Triggered Flow** executes **automatically** based on data conditions — no human decision is required.
2. The record is **locked** by default. No users (except System Admins and designated approval admins) can edit the record while it is pending approval.
3. The behavior on rejection is configured per step in the "Rejection Behavior" setting. Options: "Final Rejection" (process ends, Final Rejection Actions fire) or "Go Back to [Previous Step]" (sends back for re-review).
4. **Recall** withdraws a pending approval submission. It can be performed by the person who **submitted** the request (the original submitter). Admins can also recall on behalf of users.
5. **Yes** — Salesforce sends an email with Approve and Reject links. An approver can click directly from the email. Their decision is recorded in Salesforce without logging in.
6. **Initial Submission Actions** fire at the **exact moment** a user clicks "Submit for Approval" — before any approver sees the request. Common actions: lock the record, set a status field to "Pending," send a confirmation email.
7. Step 3 is **skipped** (not executed). When Step 2 is rejected with "Final Rejection" behavior, the process ends immediately and Final Rejection Actions fire. Step 3 never gets a chance to run.
8. **Approval Actions (per step)** fire when that specific step is approved — they run before moving to the next step. **Final Approval Actions** fire only when ALL steps have been approved (the entire process is complete).
9. **Yes** — you can have multiple Approval Processes on the same object. Records can only be in one active approval process at a time, but multiple processes can exist to handle different scenarios (e.g., one for discounts, one for lost deals).
10. The approval request remains **pending indefinitely** until someone with the appropriate role/delegation acts on it. Best practice: configure **Delegate Approvers** and set up escalation email reminders in your approval notifications.

---

## 🔗 Trailhead Resources

- [Build a Discount Approval Process](https://trailhead.salesforce.com/content/learn/projects/build-a-discount-approval-process)
- [Business Process Automation](https://trailhead.salesforce.com/content/learn/modules/business_process_automation)

---

*Previous: [06A · Validation Rules](./06A_Validation_Rules.md) | Next: [06C · Flow Builder — All Types →](./06C_Flow_Builder.md)*
