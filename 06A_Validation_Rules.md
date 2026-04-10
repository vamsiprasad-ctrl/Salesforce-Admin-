# 06A · Validation Rules — Complete Guide

> **Sub-Topic:** Business Logic & Automation | **Difficulty:** 🟡 Intermediate | **Est. Time:** 2 hrs

---

## What is a Validation Rule?

A **Validation Rule** is a formula-based rule that evaluates data on a record **before it is saved**. If the formula returns `TRUE`, the save is **blocked** and the user sees an error message. If the formula returns `FALSE`, the save proceeds normally.

> 🔑 **Most Important Concept:** You write the formula to describe the **INVALID** state. When the formula is TRUE = invalid = save blocked.

This is the opposite of how most people think. You are NOT writing "when should the record be saved?" — you are writing "when should the record be REJECTED?"

### Why Validation Rules Exist

Without validation rules, Salesforce will save whatever a user types — even nonsense data. Validation rules enforce business logic at the data layer, not just the UI layer.

| Without Validation | With Validation |
|-------------------|-----------------|
| Close Date = 1999 saved without error | Close Date in the past → blocked |
| Phone = "abc" saved | Non-numeric phone → blocked |
| ₹500K deal with no legal review date | Amount > ₹1L without Legal Date → blocked |
| Discount = 50% saved freely | Discount > 30% → blocked with message |

---

## Where Validation Rules Live

**Path:** Setup → Object Manager → [Object Name] → Validation Rules → New

Validation rules can be created on **any Standard or Custom Object**.

---

## Anatomy of a Validation Rule

| Component | Description | Example |
|-----------|-------------|---------|
| **Rule Name** | Internal API name | `Close_Date_Must_Be_Future` |
| **Active** | Toggle on/off without deleting | ✅ checked = enforced |
| **Description** | Why this rule exists | "Prevents backdated opportunities" |
| **Error Condition Formula** | The formula that returns TRUE when data is INVALID | `CloseDate < TODAY()` |
| **Error Message** | What the user sees when blocked | "Close Date must be today or a future date." |
| **Error Location** | Where the message appears | Top of Page OR specific Field |

---

## Core Formula Functions for Validation Rules

### ISBLANK( field )
Returns TRUE if the field is empty (null or empty string).

```
ISBLANK(Email)
```
> TRUE if Email field is empty.

---

### NOT( condition )
Inverts the result of a condition.

```
NOT(ISBLANK(Email))
```
> TRUE if Email is NOT empty. (Becomes false = saves when Email has a value.)

---

### AND( condition1, condition2, ... )
Returns TRUE only when ALL conditions are true.

```
AND(
  ISPICKVAL(Stage, "Closed Won"),
  ISBLANK(Contract_Signed_Date__c)
)
```
> TRUE when Stage = Closed Won AND Contract Date is blank → blocked.

---

### OR( condition1, condition2, ... )
Returns TRUE when ANY condition is true.

```
OR(
  Amount < 0,
  Amount > 10000000
)
```
> TRUE when Amount is negative OR exceeds 1 crore → blocked.

---

### IF( condition, true_result, false_result )
Conditional logic — returns different values based on a condition.

```
IF(
  ISPICKVAL(Job_Type__c, "Full-Time"),
  ISBLANK(Salary_Range__c),
  FALSE
)
```
> If Full-Time then check salary is blank. If not Full-Time, return FALSE (never blocks).

---

### ISPICKVAL( picklist_field, "value" )
Returns TRUE if a picklist field equals a specific value.

```
ISPICKVAL(Lead_Status__c, "Converted")
```
> TRUE if Lead Status = Converted.

---

### REGEX( text_field, "pattern" )
Returns TRUE if the text field matches a regular expression pattern.

```
NOT(REGEX(Phone, "[0-9]{10}"))
```
> TRUE if Phone does NOT match exactly 10 digits → blocked.

---

### LEN( text_field )
Returns the number of characters in a text field.

```
LEN(Description__c) < 50
```
> TRUE if Description is less than 50 characters → blocked.

---

### TODAY() / NOW()
Returns the current date or date-time.

```
CloseDate < TODAY()
```
> TRUE if Close Date is before today → blocked.

---

### ISNEW()
Returns TRUE only when a record is being **created** (not edited).

```
AND(
  ISNEW(),
  Application_Deadline__c < TODAY()
)
```
> Only runs on new records. Existing records can be saved even if deadline has passed.

---

### ISCHANGED( field )
Returns TRUE when a field's value has changed during this edit.

```
AND(
  ISCHANGED(Amount),
  Amount > PRIORVALUE(Amount) * 2
)
```
> TRUE if Amount was more than doubled in one edit → blocked.

---

### PRIORVALUE( field )
Returns the value a field had **before** the current edit.

```
PRIORVALUE(Stage__c)
```
> The Stage value before the rep changed it in this edit session.

---

### DATEVALUE( datetime_field )
Converts a Date/Time field to a Date for comparison.

```
DATEVALUE(CreatedDate) = TODAY()
```

---

## Real-World Validation Rule Examples

---

### VR 01 — Close Date Must Not Be in the Past (Active Opportunities Only)

**Object:** Opportunity
**Business Rule:** Active Opportunities cannot have a past Close Date.

```
AND(
  NOT(ISPICKVAL(StageName, "Closed Won")),
  NOT(ISPICKVAL(StageName, "Closed Lost")),
  CloseDate < TODAY()
)
```

**Error:** "Close Date must be a future date for active opportunities."
**Location:** `CloseDate` field

---

### VR 02 — Phone Must Be Exactly 10 Digits

**Object:** Contact / Lead
**Business Rule:** Phone must be a 10-digit number with no spaces or special characters.

```
AND(
  NOT(ISBLANK(Phone)),
  NOT(REGEX(Phone, "[0-9]{10}"))
)
```

**Error:** "Phone number must be exactly 10 digits. Do not include spaces, dashes, or country codes."
**Location:** `Phone` field

---

### VR 03 — Email Format Must Be Valid

**Object:** Lead
**Business Rule:** Email must follow a valid format.

```
AND(
  NOT(ISBLANK(Email)),
  NOT(REGEX(Email, "^[a-zA-Z0-9._%+\\-]+@[a-zA-Z0-9.\\-]+\\.[a-zA-Z]{2,}$"))
)
```

**Error:** "Please enter a valid email address (e.g., name@company.com)."
**Location:** `Email` field

---

### VR 04 — Large Deals Require Legal Review Date

**Object:** Opportunity
**Business Rule:** Any deal over ₹10 Lakhs must have a Legal Review Date before saving.

```
AND(
  Amount >= 1000000,
  ISBLANK(Legal_Review_Date__c)
)
```

**Error:** "Deals of ₹10 Lakhs or more require a Legal Review Date. Please coordinate with the Legal team."
**Location:** `Legal_Review_Date__c` field

---

### VR 05 — Discount Cannot Exceed 30%

**Object:** Opportunity
**Business Rule:** No rep is allowed to enter a discount above 30%.

```
Discount_Percentage__c > 30
```

**Error:** "Discount cannot exceed 30%. For discounts above 30%, contact your VP of Sales."
**Location:** `Discount_Percentage__c` field

---

### VR 06 — End Date Must Be After Start Date

**Object:** Contract / Event / Project
**Business Rule:** End Date must always come after Start Date.

```
AND(
  NOT(ISBLANK(End_Date__c)),
  NOT(ISBLANK(Start_Date__c)),
  End_Date__c <= Start_Date__c
)
```

**Error:** "End Date must be after Start Date."
**Location:** `End_Date__c` field

---

### VR 07 — Full-Time Jobs Must Have Salary

**Object:** Job_Posting__c
**Business Rule:** Full-Time job postings must include a Salary Range.

```
AND(
  ISPICKVAL(Job_Type__c, "Full-Time"),
  ISBLANK(Salary_Range__c)
)
```

**Error:** "Salary Range is required for Full-Time positions."
**Location:** `Salary_Range__c` field

---

### VR 08 — Cannot Reopen a Closed Won Opportunity

**Object:** Opportunity
**Business Rule:** Once marked Closed Won, a rep cannot change the Stage to anything else.

```
AND(
  ISPICKVAL(StageName, "Closed Won"),
  NOT(ISPICKVAL(PRIORVALUE(StageName), "Closed Won")),
  NOT(ISNEW())
)
```

Wait — this is backwards. The correct formula is:

```
AND(
  ISPICKVAL(PRIORVALUE(StageName), "Closed Won"),
  NOT(ISPICKVAL(StageName, "Closed Won"))
)
```

**Reads as:** If the stage WAS Closed Won (before edit) and is now being changed to something else → block.
**Error:** "A Closed Won Opportunity cannot be reopened. Contact your manager if this is an error."

---

### VR 09 — Job Description Must Be At Least 100 Characters

**Object:** Job_Posting__c
**Business Rule:** Job descriptions must be substantive — no one-liners.

```
AND(
  NOT(ISBLANK(Job_Description__c)),
  LEN(Job_Description__c) < 100
)
```

**Error:** "Job Description must be at least 100 characters. Please provide a complete description."
**Location:** `Job_Description__c` field

---

### VR 10 — Non-Remote Jobs Must Have a Location

**Object:** Job_Posting__c
**Business Rule:** If the job is not remote, a physical location is required.

```
AND(
  Is_Remote__c = FALSE,
  ISBLANK(Location__c)
)
```

**Error:** "Location is required for on-site or hybrid positions."
**Location:** `Location__c` field

---

### VR 11 — Cannot Create Job Posting with Past Deadline (New Records Only)

**Object:** Job_Posting__c
**Business Rule:** New postings cannot have a deadline in the past. But existing records can be saved even if deadline passed (to allow status updates on closed postings).

```
AND(
  ISNEW(),
  NOT(ISBLANK(Application_Deadline__c)),
  Application_Deadline__c < TODAY()
)
```

**Error:** "Application Deadline must be a future date for new job postings."

---

### VR 12 — Amount Cannot More Than Double in One Edit

**Object:** Opportunity
**Business Rule:** Reps cannot suddenly double an opportunity amount — likely a data entry error.

```
AND(
  NOT(ISNEW()),
  ISCHANGED(Amount),
  NOT(ISBLANK(PRIORVALUE(Amount))),
  Amount > PRIORVALUE(Amount) * 2
)
```

**Error:** "Opportunity Amount cannot more than double in a single edit. If intentional, contact your Sales Manager."

---

## Validation Rule Bypass Techniques (Know These!)

| Technique | How It Works |
|-----------|-------------|
| **System Admin bypass** | System Admins are NOT exempt from Validation Rules by default — they are enforced for everyone |
| **Data Loader** | By default, Validation Rules run during Data Loader imports. You can disable them in Data Loader settings under the "Use Bulk API" option |
| **Deactivate the rule** | Admins can temporarily deactivate a rule to allow bulk edits, then reactivate |
| **ISNEW() / ISCHANGED()** | Use these functions to limit when the rule fires |

---

## Validation Rules vs Required Fields

| Feature | Required Field | Validation Rule |
|---------|---------------|-----------------|
| Setup location | Field settings | Object → Validation Rules |
| Conditions possible? | No — always required | Yes — required only when criteria met |
| Custom error message | No | Yes |
| Cross-field logic | No | Yes |
| Formula capability | No | Full formula language |

**Best Practice:** Use Required Fields only for fields that are ALWAYS required. Use Validation Rules for conditionally required fields.

---

## Common Mistakes with Validation Rules

### ❌ Mistake 1 — Writing the formula backwards

Wrong thinking: "I want Close Date to be in the future."
Wrong formula: `CloseDate >= TODAY()` — this would BLOCK valid future dates!

Right thinking: "I want to BLOCK when Close Date is in the PAST."
Right formula: `CloseDate < TODAY()` — this blocks past dates, allows future dates.

### ❌ Mistake 2 — Forgetting ISNEW() when needed

If you write `Application_Deadline__c < TODAY()` without `ISNEW()`, then admins can NEVER save an existing record after the deadline passes — even just to update the status to "Closed."

Always ask: "Should this rule apply to existing records being edited?"

### ❌ Mistake 3 — Not handling blank values

`Amount > 100000` will evaluate to FALSE (not fire) if Amount is blank — which means the rule won't catch cases where Amount is missing entirely. If you also want to require Amount, add:

```
OR(
  ISBLANK(Amount),
  AND(Amount >= 100000, ISBLANK(Legal_Review_Date__c))
)
```

### ❌ Mistake 4 — Using AND when you mean OR (and vice versa)

"Block if Amount is negative OR above 10 crore" → use `OR`
"Block if Stage is Closed Won AND Contract Date is blank" → use `AND`

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Basic Validation Rule

**Objective:** Build and test a simple date validation.

**Steps:**
1. Setup → Object Manager → `Job_Posting__c` → Validation Rules → **New**
2. Rule Name: `Deadline_Must_Be_Future`
3. Active: ✅
4. Formula:
```
AND(
  NOT(ISBLANK(Application_Deadline__c)),
  Application_Deadline__c < TODAY()
)
```
5. Error Message: `Application Deadline must be today or a future date.`
6. Error Location: Field → `Application_Deadline__c`
7. Save

**Test Matrix:**
| Test Case | Input | Expected Result |
|-----------|-------|----------------|
| Past date | Yesterday | ❌ Blocked |
| Today's date | Today | ✅ Saved |
| Future date | Next month | ✅ Saved |
| Blank date | (empty) | ✅ Saved (blank is OK) |

---

### 🔧 Task 2 — Conditional Required Field

**Objective:** Make a field required based on another field's value.

**Steps:**
1. New Validation Rule on `Job_Posting__c`
2. Name: `Salary_Required_For_FullTime`
3. Formula:
```
AND(
  ISPICKVAL(Job_Type__c, "Full-Time"),
  OR(
    ISBLANK(Salary_Range__c),
    Salary_Range__c <= 0
  )
)
```
4. Error Message: `Salary Range must be entered and must be greater than 0 for Full-Time positions.`
5. Save

**Test Matrix:**
| Job Type | Salary | Expected |
|----------|--------|---------|
| Full-Time | (blank) | ❌ Blocked |
| Full-Time | -1 | ❌ Blocked |
| Full-Time | 50000 | ✅ Saved |
| Contract | (blank) | ✅ Saved |
| Part-Time | (blank) | ✅ Saved |

---

### 🔧 Task 3 — Cross-Field Validation

**Objective:** Compare two date fields against each other.

**Business Scenario:** Add `Posted_Date__c` and `Application_Deadline__c` fields. Deadline must be at least 7 days after Posted Date.

**Steps:**
1. Add a `Posted_Date__c` Date field to `Job_Posting__c`
2. New Validation Rule:
3. Name: `Deadline_After_Posted_Date`
4. Formula:
```
AND(
  NOT(ISBLANK(Posted_Date__c)),
  NOT(ISBLANK(Application_Deadline__c)),
  Application_Deadline__c < Posted_Date__c + 7
)
```
5. Error Message: `Application Deadline must be at least 7 days after the Posted Date.`
6. Save

**Test:** Set Posted Date = today. Try Deadline = tomorrow → blocked. Try Deadline = 10 days from now → saved.

---

### 🔧 Task 4 — ISNEW() vs Always-On

**Objective:** Understand the difference between ISNEW() and always-on rules.

**Steps:**
1. Create Rule A (ALWAYS ON):
   - Name: `Deadline_Future_AlwaysOn`
   - Formula: `Application_Deadline__c < TODAY()`
2. Create Rule B (NEW RECORDS ONLY):
   - Name: `Deadline_Future_NewOnly`
   - Formula: `AND(ISNEW(), Application_Deadline__c < TODAY())`
3. Create a Job Posting with a future deadline → saves fine
4. Wait... (or manually change your system date, or use a test approach)
5. **Simulate:** Create a posting, then edit it and change the deadline to a past date
   - Rule A blocks the save (even editing)
   - Rule B would NOT block (only fires on new records)
6. Deactivate both rules after testing

**Lesson:** Rule A is better for enforcing data integrity at all times. Rule B is needed when existing records legitimately need to be saved after conditions change.

---

### 🔧 Task 5 — PRIORVALUE() and ISCHANGED()

**Objective:** Use change-detection functions.

**Scenario:** Once a Job Posting status changes to "Closed", it cannot be reopened.

**Steps:**
1. Add a `Status__c` Picklist field to `Job_Posting__c`:
   Values: Draft, Active, Closed
2. New Validation Rule:
3. Name: `Cannot_Reopen_Closed_Posting`
4. Formula:
```
AND(
  NOT(ISNEW()),
  ISPICKVAL(PRIORVALUE(Status__c), "Closed"),
  NOT(ISPICKVAL(Status__c, "Closed"))
)
```
5. Error Message: `A Closed Job Posting cannot be reopened. Please create a new posting instead.`

**Test:**
- Create posting with Status = Active → save ✅
- Edit: change Status to Closed → save ✅
- Edit: try to change Status back to Active → blocked ❌

---

### 🔧 Task 6 — REGEX() Pattern Matching

**Objective:** Enforce a specific text format.

**Scenario:** A new `Job_Code__c` field must follow the format: 3 capital letters + dash + 4 digits (e.g., `ENG-0042`).

**Steps:**
1. Add a `Job_Code__c` Text field (length 8) to `Job_Posting__c`
2. New Validation Rule:
3. Name: `Job_Code_Format`
4. Formula:
```
AND(
  NOT(ISBLANK(Job_Code__c)),
  NOT(REGEX(Job_Code__c, "[A-Z]{3}-[0-9]{4}"))
)
```
5. Error Message: `Job Code must follow the format: 3 capital letters, dash, 4 digits. Example: ENG-0042`

**Test:**
| Input | Expected |
|-------|---------|
| `ENG-0042` | ✅ Saved |
| `eng-0042` | ❌ (lowercase) |
| `ENG0042` | ❌ (no dash) |
| `EN-0042` | ❌ (only 2 letters) |
| `ENG-042` | ❌ (only 3 digits) |
| (blank) | ✅ Saved |

---

## 📝 Practice Questions — Validation Rules

**Q1.** A Validation Rule formula evaluates to `FALSE`. What happens?

**Q2.** Write a formula that blocks an Opportunity from being saved if the `Amount` field is blank.

**Q3.** What is the difference between using `ISBLANK()` and checking `= null` or `= ""`?

**Q4.** A validation rule uses `ISNEW()`. Will it fire when an existing record is edited?

**Q5.** What function would you use to check if a phone number contains exactly 10 digits?

**Q6.** An admin deactivates a validation rule, imports records with invalid data via Data Loader, then reactivates the rule. Will the records with invalid data now be affected?

**Q7.** Write a formula that blocks a record if `Start_Date__c` is after `End_Date__c`.

**Q8.** What is `PRIORVALUE()` and when would you use it?

**Q9.** Can a validation rule check fields on a related record (e.g., the Account's Industry from a Contact record)?

**Q10.** A record is being created. The formula is `NOT(ISNEW())`. When does this rule fire?

**Q11.** What is the maximum number of validation rules per object?

**Q12.** Error Location is set to a specific field. Where exactly does the error message appear?

---

## 🎯 Scenario-Based Questions — Validation Rules

---

**Scenario 1 — Insurance Policy Validation Design**

> "InsureCo" tracks insurance policies on `Policy__c` with these fields:
> - `Start_Date__c` (Date)
> - `End_Date__c` (Date)
> - `Coverage_Amount__c` (Currency)
> - `Premium_Amount__c` (Currency)
> - `Policy_Type__c` (Picklist: Life, Health, Vehicle, Property)
> - `Vehicle_Registration__c` (Text)
> - `Policy_Status__c` (Picklist: Draft, Active, Expired, Cancelled)
>
> **Requirements — write the formula for each:**
> 1. End Date must be after Start Date
> 2. Coverage Amount must be greater than Premium Amount
> 3. For Vehicle policies, Vehicle Registration is required
> 4. A Cancelled policy cannot be reactivated (Status cannot change from Cancelled to anything else)
> 5. Premium Amount must be between ₹1,000 and ₹10,00,000
> 6. Start Date cannot be more than 1 year in the past on new records

---

**Scenario 2 — Debugging a Broken Validation Rule**

> An admin reports: "I created a validation rule to make Email required when Lead Status = Qualified. But reps are complaining it's blocking EVERY lead save, even when Status is not Qualified."
>
> The admin shows you this formula:
> ```
> OR(
>   ISPICKVAL(Status, "Qualified"),
>   ISBLANK(Email)
> )
> ```
>
> **Question:**
> 1. Identify the bug in the formula
> 2. Explain exactly why it blocks all saves
> 3. Write the corrected formula
> 4. What function is critical here that the admin missed?

---

**Scenario 3 — Validation Rule vs Required Field Decision**

> "HRCo" is designing their Job Posting object. For each field below, decide: should it be a **Required Field** (always required) or a **Validation Rule** (conditionally required)?
>
> 1. `Job_Title__c` — always needed
> 2. `Salary_Range__c` — needed only for Full-Time jobs
> 3. `Application_Deadline__c` — needed only when Status = Active
> 4. `Company_Name__c` — always needed
> 5. `Partner_Agency__c` — needed only when `Is_Third_Party_Recruitment__c = True`
> 6. `Internal_Grade__c` — needed only for internal job postings
>
> **For each Validation Rule case, write the formula.**

---

## ✔️ Answer Key — Practice Questions

1. The record **saves normally**. Validation Rules only block when the formula returns TRUE.
2. `ISBLANK(Amount)`
3. `ISBLANK()` works on all field types including numbers (blank number), text (empty string), and null. Checking `= null` or `= ""` is field-type specific and can produce unexpected results in formula context. Always use `ISBLANK()`.
4. **No** — `ISNEW()` returns TRUE only when a brand new record is being created. When editing an existing record, `ISNEW()` returns FALSE, so `AND(ISNEW(), ...)` will always return FALSE and never fire during edits.
5. `REGEX(Phone, "[0-9]{10}")` — returns TRUE if exactly 10 digits. Use inside `NOT()` to block when it does NOT match.
6. **No** — Validation Rules check data at the time of save. Records already in the database are not retroactively validated. However, those records cannot be edited and re-saved without meeting the rule going forward.
7. `AND(NOT(ISBLANK(Start_Date__c)), NOT(ISBLANK(End_Date__c)), Start_Date__c > End_Date__c)`
8. `PRIORVALUE(field)` returns the value a field had **before** the current edit. It's used when you want to detect what changed — for example, preventing a stage from moving backward, or detecting a specific transition.
9. **Yes** — using dot notation: `Account.Industry` on a Contact record accesses the related Account's Industry field in a Validation Rule formula.
10. The formula `NOT(ISNEW())` returns TRUE when the record is NOT new — i.e., when an **existing record is being edited**. It never fires on record creation.
11. There is **no documented maximum** per object — Salesforce does not publish a hard limit. However, too many validation rules impact save performance. Best practice is to keep rules focused and combine related logic where possible.
12. The error message appears **below the specific field** on the record's edit page, highlighted in red — similar to how a browser form shows field-level validation errors.

---

## 🔗 Trailhead Resources

- [Validation Rules Module](https://trailhead.salesforce.com/content/learn/modules/validation-rules)
- [Improve Data Quality for a Cleaning Supply App](https://trailhead.salesforce.com/content/learn/projects/improve-data-quality-for-a-cleaning-supply-app)

---

*Next in Series: [06B · Approval Processes →](./06B_Approval_Processes.md)*
