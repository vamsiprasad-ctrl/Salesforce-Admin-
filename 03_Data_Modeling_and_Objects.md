# 03 · Data Modeling & Objects

> **Module:** Data & Objects | **Difficulty:** 🟡 Beginner–Intermediate | **Est. Time:** 6 hrs

---

## 3.1 What is a Data Model?

A data model is a blueprint of how your business data is structured, stored, and connected. In Salesforce:

| Database Term | Salesforce Equivalent | Real Example |
|--------------|----------------------|-------------|
| Table | Object | `Account` |
| Column | Field | `Phone`, `Industry` |
| Row | Record | Coca-Cola's account record |
| Foreign Key | Relationship Field | `AccountId` on Contact |

---

## 3.2 Standard vs Custom Objects

**Standard Objects** — pre-built by Salesforce:
`Account` · `Contact` · `Lead` · `Opportunity` · `Case` · `Task` · `Event` · `Product2` · `Campaign`

**Custom Objects** — you create them, always suffixed `__c`:

### Real-World Example

> **LegalEase** (law firm) needs:
> - `Matter__c` — legal cases being worked on
> - `Hearing__c` — court dates
> - `Billable_Hours__c` — time logged per matter
>
> None of these are standard — admin creates them as custom objects.

---

## 3.3 Field Types

| Field Type | Stores | Example |
|-----------|--------|---------|
| Text | Free-form text (255 chars) | Company Name |
| Text Area (Long) | Multi-line up to 131,072 chars | Description, Notes |
| Number | Numeric values | Quantity, Score |
| Currency | Money values (multi-currency aware) | Deal Amount |
| Percent | Percentage | Discount Rate |
| Date | Calendar date | Close Date |
| Date/Time | Date + time | Meeting Time |
| Checkbox | True/False | Is Active |
| Picklist | Single-select dropdown | Stage, Industry |
| Multi-Select Picklist | Multiple values from a list | Products Interested In |
| Lookup | Reference to another record | Account on Contact |
| Master-Detail | Tight parent-child link | Order Line Item to Order |
| Formula | Read-only calculated field | Margin %, Full Name |
| Roll-Up Summary | Aggregated value from children | Total Opportunity Amount |
| Auto Number | Auto-incrementing ID | Case-{000001} |
| URL | Web address | Company Website |
| Email | Email (clickable) | Contact Email |
| Geolocation | Lat/Long pair | Office Location |

---

## 3.4 Dependent Picklists

Values in a **Dependent** (child) picklist change based on what was selected in the **Controlling** (parent) field.

### Real-World Example

> **AutoDeal** — car dealership
>
> Controlling: `Vehicle_Type__c` — Car / Truck / SUV / Motorcycle
> Dependent: `Model__c`
>
> | Vehicle Type | Available Models |
> |-------------|-----------------|
> | Car | Sedan, Coupe, Hatchback |
> | Truck | Pickup, Flatbed, Box Truck |
> | SUV | Compact, Full-Size, Crossover |
> | Motorcycle | Sport, Cruiser, Touring |

---

## 3.5 Field History Tracking

Tracks every change to a field — who, when, old value, new value. Up to **20 fields per object**. Retained for **18 months**.

### Real-World Example

> **InsureCo** enables history tracking on `Policy_Status__c`.
>
> Auditor asks: "Who cancelled Policy #INS-0012345 and when?"
>
> | Date | User | Field | Old Value | New Value |
> |------|------|-------|-----------|-----------|
> | 15-Jan-2026 | Sarah Admin | Policy Status | Active | Under Review |
> | 20-Jan-2026 | Mark Manager | Policy Status | Under Review | Cancelled |

---

## 3.6 Page Layouts

Controls what appears on a record detail/edit page — field placement, required fields, buttons, related lists.

### Real-World Example

> **ConsultCo** `Project__c` object:
> - **Sales Layout** — shows Client, Deal Value, Probability, Notes
> - **Delivery Layout** — shows Start Date, End Date, Resources, Deliverables, Status
>
> Each layout assigned to the appropriate profile → users see only what's relevant.

---

## 3.7 Record Types

Offer different **picklist values**, **page layouts**, and **business processes** on the same object based on record category.

### Real-World Example

> **RealEstate Co.** uses one `Property__c` object with two Record Types:
> - **Residential** — Bedrooms, Bathrooms, HOA Fee, School District
> - **Commercial** — Office Sqft, Zoning, Loading Docks, Lease Type
>
> When creating a new Property, Salesforce asks: "Which type?" → shows the correct form.

---

## 3.8 Relationships

### 3.8.1 Lookup

| Property | Value |
|----------|-------|
| Child exists without parent? | ✅ Yes |
| Cascade delete? | ❌ No (field is blanked) |
| Enables Roll-Up Summary? | ❌ No |
| Affects sharing? | ❌ No |
| Max per object | Up to 25 |

### Real-World Example

> **Contact → Account** Lookup. If Acme Corp Account is deleted, Sarah's Contact still exists — Account field becomes blank. She becomes a private contact.

---

### 3.8.2 Master-Detail

| Property | Value |
|----------|-------|
| Child exists without parent? | ❌ No — parent field required |
| Cascade delete? | ✅ Yes — deleting parent deletes all children |
| Enables Roll-Up Summary? | ✅ Yes |
| Child inherits parent sharing? | ✅ Yes |
| Max per custom object | 2 |

### Real-World Example

> **ShopNow** e-commerce: `Order__c` (Master) → `Order_Line_Item__c` (Detail)
> - Line item for "Nike Air Max x2" has no meaning without its order
> - Delete the Order → all its Line Items are deleted too
> - Order has Roll-Up Summary: `Total_Amount__c` = SUM of all Line Item prices

---

### 3.8.3 Self Relationship

A Lookup from an object back to itself — models hierarchies within a single object.

### Real-World Example

> `Employee__c` has `Reports_To__c` Lookup pointing to another `Employee__c`:
>
> | Employee | Reports To |
> |----------|-----------|
> | John (VP Sales) | Alice (CEO) |
> | Sarah (Sales Rep) | John (VP Sales) |

---

### 3.8.4 Hierarchical Relationship

A special self-relationship available **only on the User object** — builds the management org chart.

---

### 3.8.5 Many-to-Many (Junction Object)

Achieved via a custom object with **two Master-Detail relationships**.

### Real-World Example

> **EventPro**: A Speaker presents at many Events; an Event has many Speakers.
>
> ```
> Speaker__c ──(MD)── Speaker_Session__c ──(MD)──→ Event__c
> ```
>
> | Speaker | Event | Session Time |
> |---------|-------|-------------|
> | Dr. Smith | TechConf 2026 | 10:00 AM |
> | Dr. Smith | AI Summit | 2:00 PM |
> | Jane Doe | TechConf 2026 | 1:00 PM |

---

### 3.8.6 External / Indirect Lookup

Connects Salesforce objects to **external data sources** (via Salesforce Connect) without duplicating data.

### Real-World Example

> **ManufactureCo** stores inventory in SAP. Using External Lookup, sales reps can see live stock levels on the Product record — data is read directly from SAP in real time, never duplicated into Salesforce.

---

## 3.9 Schema Builder

Visual drag-and-drop canvas showing your entire data model.

**Path:** Setup → Schema Builder

Useful for **understanding** existing models quickly; Object Manager is better for **building** new ones.

---

## 3.10 Activity Management

| Object | What It Is | Example |
|--------|-----------|---------|
| **Task** | To-do item with due date | "Call customer back by Friday" |
| **Event** | Calendar appointment | "Demo call, Tues 2pm" |
| **Activity Timeline** | Full history on any record | All calls, emails, meetings for a Contact |

---

## 3.11 Custom Formula Fields

Read-only calculated fields — auto-updated on every save.

### Real-World Examples

**Text Formula:** `Full_Name__c` = `FirstName & " " & LastName` → "Sarah Johnson"

**Numeric Formula:** `Gross_Margin__c` = `(Amount - Cost__c) / Amount * 100` → 35%

**Date Formula:** `Days_Until_Renewal__c` = `Renewal_Date__c - TODAY()` → always current

**Conditional Formula:**
```
IF(Probability >= 75, "On Track",
  IF(Probability >= 40, "At Risk",
    "Needs Attention"))
```

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Create a Custom Object

**Objective:** Build a custom object from scratch.

**Business Scenario:** You are the Salesforce admin at "JobPortal Inc." They need to track `Job_Posting__c` records.

**Steps:**
1. Setup → Object Manager → **Create** → **Custom Object**
2. Fill in:
   - Label: `Job Posting`
   - Plural Label: `Job Postings`
   - Object Name: `Job_Posting` (API = `Job_Posting__c`)
   - Record Name: `Job Title` (Text type)
   - Check: Allow Reports ✅, Allow Activities ✅, Track Field History ✅
3. Click **Save**

**Verify:** Go to App Launcher → type `Job Posting` — your new object tab should appear.

---

### 🔧 Task 2 — Add Custom Fields

**Objective:** Add various field types to the Job Posting object.

**Steps — add each field:**
1. Object Manager → `Job_Posting__c` → Fields & Relationships → **New**

| # | Field Label | Field Type | Additional Config |
|---|-------------|-----------|------------------|
| 1 | Company Name | Text | Length: 100, Required |
| 2 | Salary Range | Currency | |
| 3 | Job Type | Picklist | Values: Full-Time, Part-Time, Contract, Internship |
| 4 | Application Deadline | Date | |
| 5 | Is Remote | Checkbox | Default: Unchecked |
| 6 | Job Description | Text Area (Long) | |
| 7 | Experience Years | Number | Decimal places: 0 |

2. After creating all fields, go to App Launcher → Job Postings → **New**
3. Fill in a sample record and save

---

### 🔧 Task 3 — Create a Dependent Picklist

**Objective:** Build controlling → dependent picklist relationship.

**Business Scenario:** Job Postings need to track Department and Role, where Role options depend on the Department.

**Steps:**
1. On `Job_Posting__c`, create a new Picklist field:
   - Label: `Department`
   - Values: Engineering, Marketing, Sales, Finance, HR
2. Create another Picklist field:
   - Label: `Role`
   - Values: Add all of these as options initially:
     - Software Engineer, QA Engineer, DevOps
     - Content Writer, SEO Specialist, Social Media
     - Account Executive, BDR, Sales Manager
     - Accountant, Controller, CFO
     - Recruiter, HR Business Partner
3. Setup → Object Manager → `Job_Posting__c` → Fields & Relationships → **Field Dependencies**
4. Click **New**
5. Controlling Field: `Department`, Dependent Field: `Role`
6. Map values:
   - Engineering → Software Engineer, QA Engineer, DevOps
   - Marketing → Content Writer, SEO Specialist, Social Media
   - Sales → Account Executive, BDR, Sales Manager
7. Save and **Test:** Create a new Job Posting → select Department → observe Role options change

---

### 🔧 Task 4 — Create a Lookup Relationship

**Objective:** Connect two objects with a Lookup relationship.

**Business Scenario:** Each Job Posting belongs to a Recruiter (a User).

**Steps:**
1. Object Manager → `Job_Posting__c` → Fields & Relationships → **New**
2. Field Type: **Lookup Relationship**
3. Related To: **User**
4. Field Label: `Recruiter`
5. Field Name: `Recruiter__c`
6. Save

**Test:** Open a Job Posting record → click the Recruiter field → search for a user → save.

---

### 🔧 Task 5 — Create a Master-Detail Relationship

**Objective:** Build a tight parent-child relationship that enables Roll-Up Summary.

**Business Scenario:** Job Postings can have multiple `Job_Application__c` records.

**Steps:**
1. Setup → Object Manager → **Create** → Custom Object
   - Label: `Job Application`
   - API: `Job_Application__c`
   - Record Name: `Applicant Name`
2. On `Job_Application__c`, add:
   - **Master-Detail** field → Related To: `Job_Posting__c` → Label: `Job Posting`
   - Text field: `Applicant Email` (required)
   - Picklist: `Application Status` → Values: Applied, Screening, Interview, Offered, Rejected
   - Date: `Application Date`
3. Now on `Job_Posting__c`, add a **Roll-Up Summary** field:
   - Label: `Total Applications`
   - Roll-Up Type: COUNT
   - Child Object: `Job_Application__c`
4. Submit 3 Job Applications for one Job Posting
5. Open the Job Posting — `Total Applications` should show 3

---

### 🔧 Task 6 — Enable Field History Tracking

**Objective:** Track changes to important fields.

**Steps:**
1. Object Manager → `Job_Posting__c` → Fields & Relationships
2. Click **Set History Tracking**
3. Enable tracking for: `Salary_Range__c`, `Application_Deadline__c`, `Job_Type__c`
4. Save
5. Open a Job Posting record → change the Salary Range → save
6. Scroll to the **Job Posting History** related list
7. Verify: Old value, New value, Changed by, Date/Time all appear

---

### 🔧 Task 7 — Create a Record Type

**Objective:** Offer different experiences for different record categories.

**Business Scenario:** Job Postings can be Internal (for current employees) or External (for public applicants). They need different fields.

**Steps:**
1. Object Manager → `Job_Posting__c` → **Record Types** → **New**
2. Create Record Type: `Internal Posting`
3. Create another: `External Posting`
4. On the Page Layout screen, assign different layouts to each (create simple layouts if needed)
5. **Test:** New Job Posting → choose record type → observe the correct layout loads

---

### 🔧 Task 8 — Explore Schema Builder

**Objective:** Visualize your data model.

**Steps:**
1. Setup → Schema Builder
2. In the left panel, select your custom objects: `Job_Posting__c`, `Job_Application__c`
3. Also select standard objects: `User`
4. Observe the relationship lines connecting the objects
5. Click a field on any object — its details appear in the right panel

**Try:** Click the relationship line between `Job_Application__c` and `Job_Posting__c`. Confirm it shows as Master-Detail.

---

### 🔧 Task 9 — Create a Formula Field

**Objective:** Build a formula that calculates automatically.

**Business Scenario:** Show whether a Job Posting's deadline has passed.

**Steps:**
1. Object Manager → `Job_Posting__c` → Fields & Relationships → **New**
2. Field Type: **Formula**
3. Return Type: **Text**
4. Field Label: `Deadline Status`
5. Formula:
```
IF(
  ISBLANK(Application_Deadline__c),
  "No Deadline Set",
  IF(
    Application_Deadline__c < TODAY(),
    "EXPIRED",
    IF(
      Application_Deadline__c - TODAY() <= 7,
      "CLOSING SOON",
      "OPEN"
    )
  )
)
```
6. Save
7. Create Job Postings with different deadlines and observe the formula result

---

## 📝 Practice Questions

**Q1.** What is the API name for a custom object whose Label is "Project Task"?

**Q2.** You need to store a field that calculates the number of days between two date fields. Which field type would you use?

**Q3.** What is the maximum number of Master-Detail relationships allowed on a single custom object?

**Q4.** A Roll-Up Summary field on Account shows the Sum of all related Opportunity Amounts. What type of relationship must exist between Account and Opportunity for this to work?

**Q5.** What happens to child records when a parent in a **Lookup** relationship is deleted? What about a **Master-Detail** relationship?

**Q6.** You are designing a data model for a university. A Student can enroll in many Courses, and a Course can have many Students. What Salesforce structure would you use to model this?

**Q7.** What is the maximum number of fields you can enable for Field History Tracking on a single object?

**Q8.** A Dependent Picklist's values depend on the controlling field. True or False: The controlling field must also be a picklist.

**Q9.** What is the difference between a **Page Layout** and a **Record Type**?

**Q10.** On a Formula field, a rep tries to edit the value directly. What happens?

**Q11.** You create a Self Relationship on `Employee__c`. What real-world hierarchy does this enable?

**Q12.** A Junction Object has two Master-Detail relationships. What happens to Junction Object records when one of the parent records is deleted?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Choosing the Right Relationship**

> "EventPro" is building a Salesforce data model for managing corporate training events. They have the following requirements:
> - A **Training Course** can be offered multiple times as a **Training Session** (specific date/location)
> - A **Training Session** cannot exist without a Training Course
> - They want to see the total number of sessions on each Course record
> - A **Participant** (a Contact) can attend many Sessions, and a Session can have many Participants
> - A Participant record should exist even if no sessions are assigned
>
> **Question:** Design the complete data model:
> 1. What objects do you need (standard and custom)?
> 2. What relationship type connects Training Course to Training Session, and why?
> 3. How do you model the Participant-to-Session many-to-many?
> 4. What Roll-Up Summary field can you add, and where?

---

**Scenario 2 — Field Type Decision**

> A financial services company needs to track the following data on their `Investment_Portfolio__c` custom object. For each, choose the correct field type and justify:
>
> 1. Client's risk appetite (Conservative, Moderate, Aggressive, Speculative)
> 2. Total portfolio value in multiple currencies
> 3. Percentage allocation to equities
> 4. Investment start date (date only, no time)
> 5. Whether the portfolio has been reviewed this quarter (yes/no)
> 6. A calculated field: number of days since last review
> 7. Free-form notes from the advisor (can be very long)
> 8. Client's NRI status (yes/no)
> 9. The advisor's Salesforce user record
> 10. Auto-generated portfolio reference number (e.g., PORT-000001)

---

**Scenario 3 — Page Layout vs Record Type**

> A real estate agency uses one `Property__c` object. They come to you with this request: "We need residential properties to show School District and Bedrooms fields, while commercial properties should show Zoning and Floor Area. Also, the Status picklist for residential should be: Listed, Under Offer, Sold. But for commercial it should be: Available, Letter of Intent, Leased."
>
> **Question:**
> 1. Can this be solved with Page Layouts alone? Why or why not?
> 2. Can this be solved with Record Types alone? Why or why not?
> 3. What is the correct combined approach?
> 4. How many Page Layouts and Record Types would you create?

---

**Scenario 4 — Data Model Design Challenge**

> You are building Salesforce for a Hospital. Map out the complete data model for the following requirements:
>
> - Hospitals are the top-level entity
> - Each hospital has multiple Departments (Cardiology, Neurology, etc.)
> - Each Department has multiple Doctors
> - Patients visit hospitals and are seen by Doctors
> - Each visit generates a Medical Record that belongs to one Patient and one Doctor
> - A Doctor can be in multiple Departments (e.g., a surgeon covering two departments)
> - Medical Records should be deleted if the Patient is deleted
>
> **Question:**
> 1. List all objects you would create (standard + custom)
> 2. For each relationship, specify Lookup or Master-Detail and why
> 3. Identify the many-to-many relationship and design the junction object
> 4. Where would you add Roll-Up Summary fields?
> 5. What fields would you track with Field History?

---

**Scenario 5 — Formula Field Design**

> "InsureCo" wants the following formula fields on their `Insurance_Policy__c` object. Write or describe each formula:
>
> 1. **Policy Health** (Text) — "Expired" if End Date < today, "Expiring Soon" if End Date is within 30 days, "Active" otherwise
> 2. **Annual Premium Display** (Text) — combines Currency symbol, annual premium amount, and the text "/year": e.g., "₹50,000/year"
> 3. **Days Until Renewal** (Number) — how many days until the renewal date
> 4. **Discount Applied** (Checkbox) — True if `Discount_Percentage__c > 0`
>
> **Question:** Write the formula syntax for each and identify any edge cases you need to handle (e.g., blank dates).

---

## ✔️ Answer Key — Practice Questions

1. `Project_Task__c` — Salesforce replaces spaces with underscores and appends `__c`
2. **Formula field** with Return Type = Number, using `End_Date__c - Start_Date__c`
3. **2 Master-Detail** relationships maximum on one custom object
4. **Master-Detail** relationship — Roll-Up Summary fields only work on the master side of a Master-Detail, not on Lookup relationships
5. **Lookup:** Parent deleted → child remains, Lookup field is blanked. **Master-Detail:** Parent deleted → all child records are cascade deleted permanently.
6. **Junction Object** (e.g., `Student_Enrollment__c`) with two Master-Detail relationships — one to `Student__c` and one to `Course__c`
7. **20 fields** per object can be tracked with Field History Tracking
8. **False** — the controlling field can be a Picklist or a Checkbox. A Checkbox as a controlling field would show dependent values only when the box is checked.
9. A **Page Layout** controls field arrangement, buttons, and related lists on a record page. A **Record Type** controls which picklist values are available and which Page Layout is assigned — but it does NOT control field arrangement by itself.
10. Nothing — Formula fields are **read-only**. The field cannot be edited directly by any user. It recalculates automatically on each save.
11. Employee → Manager → Director → VP → CEO — an org chart hierarchy entirely within the Employee object
12. The Junction Object record is **cascade deleted** because it has a Master-Detail to the deleted parent. The *other* parent record is not affected.

---

## 🔗 Trailhead Resources

- [Customize a Salesforce Object](https://trailhead.salesforce.com/content/learn/projects/customize-a-salesforce-object)
- [Data Modeling](https://trailhead.salesforce.com/content/learn/modules/data_modeling)
- [Build a Data Model for a Travel Approval App](https://trailhead.salesforce.com/content/learn/projects/build-a-data-model-for-a-travel-approval-app)
- [Quick Start: Lightning Object Creator](https://trailhead.salesforce.com/content/learn/projects/quick-start-customize-an-app-with-lightning-object-creator)

---

*Previous: [02 · Licenses & User Management](./02_Licenses_and_User_Management.md) | Next: [04 · Security & Access Control →](./04_Security_and_Access_Control.md)*
