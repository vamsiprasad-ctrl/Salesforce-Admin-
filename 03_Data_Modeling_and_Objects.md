# 03 · Data Modeling & Objects

> **Module:** Data & Objects | **Difficulty:** 🟡 Beginner–Intermediate | **Est. Time:** 6 hrs

---

## 3.1 What is a Data Model?

A **data model** is a blueprint that defines how your data is structured, stored, and connected. In Salesforce, a data model answers:

- What types of data do we store? (Objects)
- What details do we capture per record? (Fields)
- How is data connected? (Relationships)

### The Database Analogy

| Database Term | Salesforce Equivalent | Real Example |
|--------------|----------------------|-------------|
| Table | Object | `Account` |
| Column | Field | `Phone`, `Industry` |
| Row | Record | Coca-Cola's account record |
| Foreign Key | Relationship Field | `AccountId` on a Contact |

---

## 3.2 Standard vs Custom Objects

### Standard Objects

Salesforce ships with pre-built objects called **Standard Objects**. They are ready to use immediately.

| Object | Purpose |
|--------|---------|
| `Account` | Companies or individuals you do business with |
| `Contact` | People associated with Accounts |
| `Lead` | Unqualified prospects (not yet Accounts/Contacts) |
| `Opportunity` | Potential revenue-generating deals |
| `Case` | Customer support issues |
| `Task` | To-do items |
| `Event` | Calendar appointments |
| `Product2` | Products/services you sell |
| `Campaign` | Marketing initiatives |

### Custom Objects

When standard objects don't fit your business need, you create **Custom Objects**. They are always suffixed with `__c` in the API.

### Real-World Example

> **Company:** "LegalEase" — a law firm using Salesforce.
>
> Standard objects cover Accounts (clients) and Contacts (people at the client).
>
> But they also need to track:
> - **Matters** (legal cases they're working on) → `Matter__c`
> - **Hearings** (court dates) → `Hearing__c`
> - **Billable Hours** → `Billable_Hours__c`
>
> None of these are standard Salesforce objects — so the admin creates them as custom objects.

### How to Create a Custom Object

**Path:** Setup → Object Manager → Create → Custom Object

| Field | Example Value |
|-------|--------------|
| Label | Matter |
| Plural Label | Matters |
| Object Name (API) | Matter (becomes `Matter__c`) |
| Record Name | Matter Name |
| Allow Reports | ✅ |
| Track Field History | ✅ (if needed) |

---

## 3.3 Fields

Every Object has **Fields** — the individual data points captured for each record.

### Standard Field Types

| Field Type | What It Stores | Real Example |
|-----------|---------------|-------------|
| **Text** | Free-form text (up to 255 chars) | Company Name, Product SKU |
| **Text Area (Long)** | Multi-line text (up to 131,072 chars) | Description, Notes |
| **Number** | Numeric values without currency | Quantity, Age, Score |
| **Currency** | Monetary values (respects multi-currency) | Annual Revenue, Deal Amount |
| **Percent** | Percentage values | Discount Rate, Win Rate |
| **Date** | A calendar date | Close Date, Date of Birth |
| **Date/Time** | Date + time | Meeting Scheduled, Last Login |
| **Checkbox** | True/False boolean | Is Active, Has Signed NDA |
| **Picklist** | Single-select dropdown | Stage, Industry, Status |
| **Multi-Select Picklist** | Multiple values from a list | Products Interested In |
| **Lookup** | Reference to another record | Account on Contact |
| **Master-Detail** | Tight relationship to parent | Order Line Item to Order |
| **Formula** | Read-only calculated field | Full Name, Margin % |
| **Roll-Up Summary** | Aggregated value from child records | Total Opportunity Amount on Account |
| **Auto Number** | Auto-incrementing ID | Case-{000001} |
| **URL** | Web address | Company Website |
| **Email** | Email address (clickable) | Contact Email |
| **Phone** | Phone number | Office Phone |
| **Geolocation** | Latitude/Longitude pair | Office Location |
| **Encrypted Text** | Encrypted storage for sensitive data | SSN, Passport Number |

### Real-World Example

> **Custom Object:** `Loan_Application__c` for a bank
>
> | Field | Type | Notes |
> |-------|------|-------|
> | Applicant Name | Text | Person's full name |
> | Loan Amount | Currency | Amount requested |
> | Interest Rate | Percent | Offered rate |
> | Application Date | Date | When submitted |
> | Status | Picklist | Pending, Approved, Rejected |
> | Is First-Time Buyer | Checkbox | True/False |
> | Annual Income | Currency | For assessment |
> | Notes | Text Area (Long) | Internal notes |
> | Credit Score | Number | From credit bureau |
> | Total Interest | Formula | `Loan_Amount__c * (Interest_Rate__c / 100) * Term_Years__c` |

---

## 3.4 Dependent Picklists

A **Dependent Picklist** shows different values based on what was selected in a **Controlling Field**.

### Real-World Example

> **Scenario:** "AutoDeal" — a car dealership
>
> **Controlling Field:** `Vehicle_Type__c` (Car / Truck / SUV / Motorcycle)
>
> **Dependent Field:** `Model__c`
>
> | If Vehicle Type = | Model picklist shows |
> |------------------|---------------------|
> | Car | Sedan, Coupe, Hatchback, Convertible |
> | Truck | Pickup, Flatbed, Box Truck |
> | SUV | Compact SUV, Full-Size SUV, Crossover |
> | Motorcycle | Sport, Cruiser, Touring, Off-Road |
>
> **Without a dependent picklist:** The Model field would show ALL models regardless of vehicle type — messy and error-prone.
>
> **Path:** Setup → Object Manager → [Object] → Fields & Relationships → Field Dependencies

---

## 3.5 Field History Tracking

**Field History Tracking** records every change made to a field — who changed it, when, what the old value was, and what the new value is.

- You can track up to **20 fields per object**
- History is stored in a related `History` object (e.g., `AccountHistory`, `OpportunityFieldHistory`)
- History is retained for **18 months** in standard storage (can be extended with Data Archive)

### Real-World Example

> **Scenario:** "InsureCo" tracks the `Policy_Status__c` field on their custom `Insurance_Policy__c` object.
>
> Auditors ask: "When was Policy #INS-0012345 changed from Active to Cancelled, and who did it?"
>
> The admin opens the policy record → Field History Tracking section:
>
> | Date | User | Field | Old Value | New Value |
> |------|------|-------|-----------|-----------|
> | 15-Jan-2026 | Sarah Admin | Policy Status | Active | Under Review |
> | 20-Jan-2026 | Mark Manager | Policy Status | Under Review | Cancelled |
>
> Without field history, this data would be gone forever.

---

## 3.6 Page Layouts

A **Page Layout** controls what users see when they open a record — which fields are visible, in what order, and whether fields are required on the layout.

### What you can control in a Page Layout

- Which fields appear (and in which section/column)
- Which fields are Required or Read-Only on the layout
- Related Lists at the bottom of the page (e.g., Contacts on Account)
- Buttons (standard and custom)
- Quick Actions (appear in the activity bar)

### Real-World Example

> **Scenario:** "ConsultCo" has a `Project__c` object used by two teams:
>
> **Sales team** needs to see: Client, Deal Value, Probability, Notes
> **Delivery team** needs to see: Start Date, End Date, Resources Assigned, Deliverables, Status
>
> **Solution:** Create two Page Layouts:
> 1. `Project - Sales Layout` — shows commercial fields
> 2. `Project - Delivery Layout` — shows operational fields
>
> Assign each layout to the appropriate Profile → users see only what's relevant to them.
>
> **Path:** Setup → Object Manager → [Object] → Page Layouts

---

## 3.7 Record Types

**Record Types** let you offer different **business processes**, **picklist values**, and **page layouts** on the same object — depending on the type of record.

### Real-World Example

> **Scenario:** "RealEstate Co." uses the `Property__c` object for two fundamentally different types of properties:
>
> - **Residential Properties** — track: Bedrooms, Bathrooms, Garden, School District, HOA Fee
> - **Commercial Properties** — track: Office Sqft, Zoning, Loading Docks, Parking Spaces, Lease Type
>
> These need completely different fields and picklist values. Rather than cramming everything onto one page, they create two Record Types:
>
> | Record Type | Page Layout | Picklist Values |
> |-------------|-------------|-----------------|
> | Residential | Residential Layout | Residential-specific |
> | Commercial | Commercial Layout | Commercial-specific |
>
> When a user creates a new Property, Salesforce asks: "Which type?" → shows the appropriate form.

### Record Type vs Page Layout

| | Record Type | Page Layout |
|--|-------------|-------------|
| Controls picklist values | ✅ Yes | ❌ No |
| Controls which layout to show | ✅ Yes (assigns a layout) | N/A |
| Can be assigned to Profiles | ✅ Yes | ✅ Yes (separately) |
| Multiple per object | ✅ Yes | ✅ Yes |

---

## 3.8 Relationships — The Most Important Topic

Relationships connect objects together. Getting relationships right is the difference between a data model that scales and one that breaks under pressure.

---

### 3.8.1 Lookup Relationship

A **Lookup** is a loose, optional link between two objects. The child can exist without the parent.

**API Name:** `Lookup`
**Suffix on field:** `__c` (e.g., `Account__c` on Contact is technically a standard Lookup)

| Property | Lookup |
|----------|--------|
| Child can exist without parent | ✅ Yes |
| Deleting parent deletes children | ❌ No (field is just cleared) |
| Enables Roll-Up Summary on parent | ❌ No |
| Affects sharing/security | ❌ No |
| Number per object | Up to 25 |

### Real-World Example — Lookup

> **Contact → Account Lookup**
>
> Sarah Johnson works at Acme Corp. Her Contact record has a Lookup to Acme Corp's Account.
>
> If Acme Corp's Account is deleted:
> - In a **Lookup**: Sarah's Contact still exists — the Account field is just blank. She becomes a private contact.
>
> **Another example:** A `Job_Application__c` has a Lookup to `Candidate__c` and a separate Lookup to `Job_Posting__c`. Both the candidate and the job posting can exist independently.

---

### 3.8.2 Master-Detail Relationship

A **Master-Detail** is a tight, mandatory relationship. The child cannot exist without the parent. The child inherits the parent's sharing settings.

| Property | Master-Detail |
|----------|--------------|
| Child can exist without parent | ❌ No — parent field is required |
| Deleting parent deletes children | ✅ Yes — cascade delete |
| Enables Roll-Up Summary on parent | ✅ Yes |
| Child inherits parent's sharing | ✅ Yes |
| Number per custom object | Up to 2 |

### Real-World Example — Master-Detail

> **Order Line Item → Order**
>
> "ShopNow" e-commerce company has `Order__c` (Master) and `Order_Line_Item__c` (Detail).
>
> - An order line item for "Nike Air Max x2" has no meaning without the order it belongs to.
> - If the Order is deleted (cancelled), all its line items are deleted too.
> - The Order has a Roll-Up Summary field: `Total_Amount__c` = SUM of all `Order_Line_Item__c.Price__c`
>
> **Another example:**
> - `Invoice__c` (Master) → `Invoice_Line__c` (Detail)
> - `Campaign` (Master) → `Campaign Member` (Detail)
> - `Opportunity` (Master) → `Opportunity Line Item` (Detail) ← standard example

---

### 3.8.3 Self Relationship

A **Self Relationship** is a Lookup from an object back to itself. Used to model hierarchies within a single object.

### Real-World Example

> **Employee → Manager (both are Employees)**
>
> `Employee__c` has a Lookup field `Reports_To__c` that points to another `Employee__c` record.
>
> | Employee | Reports To |
> |----------|-----------|
> | John (VP Sales) | Alice (CEO) |
> | Sarah (Sales Rep) | John (VP Sales) |
> | Mark (Sales Rep) | John (VP Sales) |
>
> This creates an org chart hierarchy entirely within one object.
>
> **Another example:** `Category__c` with a `Parent_Category__c` self-lookup → Electronics > Laptops > Gaming Laptops

---

### 3.8.4 Hierarchical Relationship

A **Hierarchical Relationship** is a special self-relationship available **only on the User object**. It is used to build the management/reporting hierarchy.

### Real-World Example

> **User → Manager**
>
> In Setup → Users, each user can have a "Manager" field that points to another User. This drives:
> - Org chart views
> - Certain approval routing rules ("route to user's manager")
> - WDC (coaching/performance) features

---

### 3.8.5 Many-to-Many Relationship

Salesforce does not have a native many-to-many relationship type. You achieve it using a **Junction Object** — a custom object with **two Master-Detail relationships**.

### Real-World Example

> **Scenario:** "EventPro" — an event management company
>
> - A **Speaker** can present at many **Events**
> - An **Event** can have many **Speakers**
>
> **Problem:** You can't put a single Lookup on Speaker pointing to one Event — speakers present at many events.
>
> **Solution:** Create a Junction Object `Speaker_Session__c`:
>
> ```
> Speaker__c  ←──(Master-Detail)── Speaker_Session__c ──(Master-Detail)──→  Event__c
> ```
>
> Each `Speaker_Session__c` record = one speaker at one event.
>
> | Speaker | Event | Session Time |
> |---------|-------|-------------|
> | Dr. Smith | TechConf 2026 | 10:00 AM |
> | Dr. Smith | AI Summit | 2:00 PM |
> | Jane Doe | TechConf 2026 | 1:00 PM |
>
> **Additional real examples:**
> - Student ↔ Course (Student_Enrollment__c junction)
> - Product ↔ Price Book (Price Book Entry — standard junction)
> - Contact ↔ Campaign (Campaign Member — standard junction)

---

### 3.8.6 External Lookup & Indirect Lookup

These relationships connect Salesforce objects to **external data sources** via Salesforce Connect.

| Type | Direction | Key Requirement |
|------|-----------|----------------|
| **External Lookup** | Salesforce object → External object | Standard `ExternalId` field on external object |
| **Indirect Lookup** | External object → Salesforce object | A unique, External ID field on the Salesforce object |

### Real-World Example

> **Scenario:** "ManufactureCo" stores warehouse inventory data in SAP (an ERP system). They don't want to duplicate all that data in Salesforce — it's too large and changes constantly.
>
> Using **Salesforce Connect + External Lookup:**
> - `Product__c` in Salesforce has an External Lookup to `Inventory__x` (external object sourced live from SAP)
> - Sales reps can see live stock levels on the Product record without the data ever being copied into Salesforce

---

## 3.9 Schema Builder

**Schema Builder** is a visual, drag-and-drop canvas that shows your entire data model — all objects, fields, and relationships — in one diagram.

### Path: Setup → Schema Builder

### What you can do in Schema Builder

- View all objects and their relationships as a diagram
- Create new custom objects
- Add new fields to existing objects
- See relationship lines connecting objects visually

### Real-World Example

> **Scenario:** A new admin joins "HealthCo" and needs to understand the data model fast. Instead of clicking through 30 objects one by one in Object Manager, they open Schema Builder, select the 8 key objects, and see a visual map of how everything connects. They understand the model in 20 minutes instead of 2 days.

> 💡 Schema Builder is great for **understanding** existing models. For **building** new objects and fields, Object Manager gives more control and options.

---

## 3.10 Activity Management

**Activities** are the built-in interaction-tracking objects in Salesforce: **Tasks** and **Events**.

| Object | What It Is | Example |
|--------|-----------|---------|
| **Task** | A to-do item with a due date | "Call customer back by Friday" |
| **Event** | A calendar appointment with start/end time | "Demo call with Acme Corp, Tues 2pm" |
| **Activity Timeline** | The feed on any record showing all past + upcoming activities | All calls, emails, meetings for a Contact |

### Real-World Example

> **Sales rep workflow:**
>
> 1. Calls a prospect → Logs a completed **Task**: "Called John re: Q2 proposal — he's interested, wants follow-up"
> 2. Schedules a demo → Creates an **Event**: "Product Demo" on Friday 3pm, invites John and the Sales Engineer
> 3. Sends follow-up email → Email is auto-logged as a **Task** (if Salesforce Inbox / Einstein Activity Capture is enabled)
>
> The **Activity Timeline** on John's Contact and the Opportunity record shows all of this in chronological order — any team member can pick up the conversation with full context.

---

## 3.11 Custom Formula Fields

A **Formula Field** is a read-only field whose value is calculated automatically from other fields using a formula expression — similar to Excel formulas.

### Real-World Example 1 — Simple Text Formula

> **Object:** `Contact`
> **Formula Field:** `Full_Name__c`
> **Formula:** `FirstName & " " & LastName`
> **Result:** "Sarah Johnson"

### Real-World Example 2 — Numeric Calculation

> **Object:** `Opportunity`
> **Formula Field:** `Gross_Margin__c` (Percent)
> **Formula:** `(Amount - Cost__c) / Amount * 100`
> **Result:** Shows margin % automatically whenever Amount or Cost changes

### Real-World Example 3 — Date Calculation

> **Object:** `Contract__c`
> **Formula Field:** `Days_Until_Renewal__c` (Number)
> **Formula:** `Renewal_Date__c - TODAY()`
> **Result:** Always shows how many days until the contract renews — no manual updating

### Real-World Example 4 — Conditional Logic

> **Object:** `Opportunity`
> **Formula Field:** `Deal_Health__c` (Text)
> **Formula:**
> ```
> IF(Probability >= 75, "On Track",
>   IF(Probability >= 40, "At Risk",
>     "Needs Attention"))
> ```
> **Result:** Color-coded (with additional formatting) status label automatically applied based on probability

### Important Formula Field Rules

- Formula fields are **read-only** — users cannot edit them directly
- They **recalculate on every save** (not stored statically — live calculations)
- They can reference fields on **related objects** using dot notation: `Account.Industry`
- Maximum formula length: 3,900 characters / 5,000 compiled characters

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Standard Objects | Pre-built by Salesforce — Account, Contact, Lead, Opportunity, Case |
| Custom Objects | Your own tables — always end in `__c` |
| Lookup | Loose link — child can exist without parent, no cascade delete |
| Master-Detail | Tight link — cascade delete, enables Roll-Up Summary, inherits sharing |
| Junction Object | Two Master-Details = Many-to-Many relationship |
| Dependent Picklist | Values change based on a controlling field |
| Field History Tracking | Audit trail of field value changes — who, when, what |
| Page Layout | Controls field arrangement on a record page |
| Record Types | Different processes, picklists, and layouts for different record categories |
| Formula Field | Read-only calculated field — auto-updated on save |

---

## 🔗 Trailhead Resources

- [Customize a Salesforce Object](https://trailhead.salesforce.com/content/learn/projects/customize-a-salesforce-object)
- [Data Modeling](https://trailhead.salesforce.com/content/learn/modules/data_modeling)
- [Build a Data Model for a Travel Approval App](https://trailhead.salesforce.com/content/learn/projects/build-a-data-model-for-a-travel-approval-app)
- [Quick Start: Customize an App with Lightning Object Creator](https://trailhead.salesforce.com/content/learn/projects/quick-start-customize-an-app-with-lightning-object-creator)
- [Dive into Sales Cloud for Admins](https://trailhead.salesforce.com/content/learn/trails/dive-into-sales-cloud-for-admins)

---

*Previous: [02 · Licenses & User Management](./02_Licenses_and_User_Management.md)*
*Next: [04 · Security & Access Control →](./04_Security_and_Access_Control.md)*
