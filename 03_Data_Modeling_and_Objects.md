# 03 · Data Modeling & Objects — Complete Deep Guide

> **Module:** Data & Objects | **Difficulty:** 🟡 Beginner–Intermediate | **Est. Time:** 8 hrs
> **Covers:** Data model concepts · Standard Objects · Custom Objects · All field types · Dependent Picklists · Field History Tracking · Page Layouts · Record Types · All relationship types · Roll-Up Summary · Schema Builder · Activity Management · Formula Fields · Object Manager

---

## 3.1 What is a Data Model?

A **data model** is the blueprint that defines how your business data is structured, stored, and connected in Salesforce. Before building anything — automation, reports, security — you must design a solid data model. Everything else depends on it.

### The Database Analogy

| Concept | Database Term | Salesforce Term | Real Example |
|---------|--------------|----------------|-------------|
| Category of data | Table | **Object** | Account |
| Individual data point | Column | **Field** | Phone, Industry |
| One row of data | Row | **Record** | Infosys's Account record |
| Link between tables | Foreign Key | **Relationship Field** | AccountId on Contact |
| Calculation | Computed Column | **Formula Field** | Margin % |
| Aggregation | View / Aggregate | **Roll-Up Summary** | Total Revenue on Account |

### Why Data Modeling Matters

A well-designed data model:
- Prevents data duplication (one source of truth per piece of information)
- Enables accurate reports and dashboards
- Makes automation simpler and more reliable
- Scales as the business grows without structural rework
- Makes security (OWD, sharing) easier to configure

A poorly designed data model causes data quality problems, impossible-to-build reports, complex automation workarounds, and performance issues at scale.

### Real-World Example — Good vs Bad Data Modeling

> **Bad model:** A company stores all customer interactions in a single `Notes__c` text field on Account — mixing sales calls, support issues, and billing disputes in one unstructured blob. "All customers with billing disputes in Q3" requires manually reading thousands of notes.
>
> **Good model:** `Account` (profile) + `Opportunity` (sales deals) + `Case` (support, with Type = Technical/Billing/General) + `Activity` (Tasks + Events). "Enterprise customers with 3+ billing disputes in last 6 months" → 30-second report.

---

## 3.2 Standard Objects — Complete Reference

Standard Objects are pre-built by Salesforce. They come with fields, relationships, and features already configured. You can add custom fields but cannot delete standard ones.

### Account

**Purpose:** Represents a company or person you do business with. The central hub — almost everything relates to an Account.

**Key Standard Fields:**

| Field | Type | Purpose |
|-------|------|---------|
| Account Name | Text | The company name |
| Type | Picklist | Prospect, Customer, Partner, Competitor |
| Industry | Picklist | Technology, Finance, Healthcare, etc. |
| Annual Revenue | Currency | Company's annual revenue |
| Number of Employees | Number | Headcount |
| Phone / Website | Phone / URL | Contact details |
| Billing / Shipping Address | Address | Physical locations |
| Parent Account | Lookup (self) | Parent company in corporate hierarchy |
| Owner | Lookup (User) | Which rep owns this account |
| Rating | Picklist | Hot, Warm, Cold |

**Account Hierarchy (Self Relationship):**
```
Tata Group (Parent Account)
  ├── Tata Consultancy Services
  ├── Tata Motors
  │     └── Jaguar Land Rover
  └── Tata Steel
```

**Real-World Use:** An enterprise software company tracks Infosys as a parent Account with Infosys BPM, Infosys Consulting, Infosys McCamish as child Accounts. Deals tracked at the subsidiary level. Parent Account shows total revenue via Roll-Up Summary.

---

### Contact

**Purpose:** Represents a specific person at an Account.

**Key Fields:** Name, Account (Lookup), Title, Email, Phone, Department, Reports To (self-lookup on Contact), Lead Source, Owner

**Private Contacts:** A Contact with no Account linked — belongs only to the contact owner.

**Contact Roles on Opportunities:** Economic Buyer, Technical Evaluator, Champion, Decision Maker, End User — maps the buying committee.

---

### Lead

**Purpose:** An unqualified prospect — kept separate from Accounts/Contacts to keep the CRM clean.

**Key Fields:** Name, Company (text — NOT linked to Account), Email, Lead Source, Status (New/Working/Qualified/Unqualified/Converted), Rating, Is Converted (read-only)

**Why Leads are Separate:** Contacts represent verified business relationships. Leads represent unverified prospects — mixing them creates noise in reports, pipeline, and service interactions.

**Lead Conversion:** Clicking Convert creates a new Account (or matches existing), new Contact, and optionally a new Opportunity. Lead is locked after conversion. Activities carry over via the Activity Timeline.

---

### Opportunity

**Purpose:** Tracks a potential revenue-generating deal from first identification to close.

**Key Fields:** Opportunity Name, Account Name (Lookup), Close Date, Stage, Amount, Probability (auto-set by Stage), Type (New Business/Renewal/Expansion), Lead Source, Next Step, Forecast Category, Owner

**Standard Stages:**

| Stage | Default Probability |
|-------|-------------------|
| Prospecting | 10% |
| Needs Analysis | 25% |
| Proposal/Price Quote | 75% |
| Negotiation/Review | 90% |
| Closed Won | 100% |
| Closed Lost | 0% |

---

### Case

**Purpose:** Tracks a customer support issue from creation to resolution.

**Key Fields:** Case Number (Auto Number), Subject, Status (New/Working/Escalated/Pending Customer/Closed), Priority (High/Medium/Low), Origin (Email/Web/Phone/Chat), Account Name, Contact Name, Type, Reason, Owner (User or Queue)

---

### Task and Event

**Task:** A to-do item with due date. Subject, Status, Priority, Due Date, Related To (any record), Owner.

**Event:** A calendar appointment. Subject, Start/End DateTime, Location, Related To, Show As (Busy/Free).

**Activity Timeline:** Shows all Tasks, Events, logged calls, and emails on any record — complete interaction history.

---

### Other Standard Objects

| Object | Purpose |
|--------|---------|
| `Product2` | Items or services you sell — used in Price Books |
| `Pricebook2` | Catalog of products with prices (Standard and Custom Price Books) |
| `OpportunityLineItem` | Products added to a specific Opportunity |
| `Campaign` | Marketing initiatives — tracks budgeted vs actual cost and ROI |
| `CampaignMember` | Junction between Campaign and Lead/Contact |
| `Contract` | Formal agreements with customers |
| `Asset` | Products a customer has purchased/owns |
| `Quote` | Pricing proposals sent to customers |

---

## 3.3 Custom Objects — Deep Dive

A custom object is a database table **you create** to store data with no equivalent in standard objects. Always identified by the `__c` suffix.

### When to Create a Custom Object

- You have a specific business entity not mapped by any standard object
- You need records with multiple fields, history, and relationships
- You need reports and automation on this data
- Multiple users create and update records of this type

### When NOT to Create a Custom Object

- You could add a custom field to an existing object instead
- The data only needs 1–2 fields (add to existing object)
- Configuration data (use Custom Settings or Custom Metadata instead)

### Object Configuration Options

**Path:** Setup → Object Manager → Create → Custom Object

| Setting | Description | When to Enable |
|---------|-------------|----------------|
| **Label / Plural Label** | What humans see | Always configure correctly |
| **Object Name (API Name)** | The `__c` reference | Set carefully — hard to change later |
| **Record Name** | The "Name" field for every record | Set label (e.g., "Policy Number") and type (Text or Auto Number) |
| **Allow Reports** | Reports can be built on this object | Almost always ✅ |
| **Allow Activities** | Tasks and Events can be logged | Enable when tracking interactions matters |
| **Track Field History** | Field-level audit trail | Enable for compliance-sensitive objects |
| **Allow Search** | Records appear in Global Search | Almost always ✅ |
| **Deployment Status** | Deployed or In Development | Keep In Development while building |

### API Naming Rules

- Spaces become underscores: "Policy Holder" → `Policy_Holder__c`
- No special characters except underscores
- Must start with a letter, max 40 chars before `__c`
- Cannot end with two underscores

### Real-World Custom Objects by Industry

| Industry | Custom Object | Purpose |
|----------|--------------|---------|
| Legal | `Matter__c` | Legal cases being worked |
| Healthcare | `Patient_Visit__c` | Patient visit records |
| Real Estate | `Property__c` | Property listings |
| Manufacturing | `Production_Run__c` | Manufacturing batches |
| Education | `Enrollment__c` | Student course enrollments |
| Banking | `Loan_Application__c` | Loan origination |
| HR | `Job_Posting__c` | Open positions |

---

## 3.4 Field Types — Complete Reference

Choosing the right field type is one of the most important data modeling decisions — it affects storage, indexing, reporting, formula usage, and data quality.

---

### Text

**Max:** 255 characters | **Indexed:** Yes | **Formula-usable:** Yes

**Configuration options:** Length (1–255), Unique (no duplicates), External ID (import key), Case Sensitive

**Use for:** Short text — names, codes, identifiers, short descriptions

**Cannot use for:** Notes, descriptions, or content that may exceed 255 characters

---

### Text Area

**Max:** 255 characters | Multi-line display (3–50 configurable rows)

**Use for:** Short multi-line text with line breaks

---

### Text Area (Long)

**Max:** 131,072 characters (~128KB) | **Indexed:** No | **Formula-usable:** No

**Cannot be used in:** Validation rules, Flow criteria, Report filters, SOQL WHERE clauses

**Use for:** Notes, instructions, lengthy descriptions, job descriptions

> ⚠️ **Important:** Long Text Area fields cannot be used in formulas or automation criteria. If you need to filter or automate based on a text field's content, use a regular Text field (255 chars) or a Picklist instead.

---

### Text Area (Rich)

**Max:** 131,072 characters | Stores HTML markup for formatting (bold, bullets, hyperlinks, images)

**Use for:** Knowledge Articles, product pages, formatted content

**Trade-off:** Stored value includes HTML tags — complicates reporting on raw values

---

### Number

**Indexed:** Yes | **No currency symbol**

**Use for:** Counts, scores, quantities, ages, ratings — numeric but NOT monetary

**Configuration:** Total digits + decimal places

**Real examples:** Quantity, Experience Years, Credit Score (300–850), Number of Employees

---

### Currency

**Indexed:** Yes | **Multi-Currency aware:** Currency fields automatically convert with exchange rates

**Use for:** Any monetary value — salaries, revenues, deal amounts, costs, prices

**Key difference from Number:** Currency fields respect Multi-Currency, display with currency symbol (₹/$/ €), participate correctly in Roll-Up Summary SUMs across currencies

---

### Percent

**Stores:** Percentage values (25% stored as 25, not 0.25)

**Use for:** Rates, probabilities, discounts, allocations

**Real examples:** Probability (Opportunity), Discount Rate, Tax Rate, Win Rate, Equity Allocation

---

### Date

**Use for:** When you only need the day, not the time

**Display:** Controlled by user's Locale setting — `2026-04-01` displays as `04/01/2026` (US) or `01/04/2026` (India/UK)

**Real examples:** Close Date, Date of Birth, Application Deadline, Contract Start Date

---

### Date/Time

**Use for:** When both day AND exact time matter

**Storage:** Internally stored in UTC. Displayed in user's configured time zone.

**Real examples:** Created Date (auto), Last Modified Date (auto), Meeting Start Time, Case Closed At

---

### Checkbox

**Stores:** True / False | **Default:** False unless configured

**In formulas:** Referenced as `TRUE` / `FALSE`
**In SOQL:** `WHERE Is_Remote__c = true`

**Use for:** Yes/No flags, boolean toggles — Is Active, Is Remote, Has Signed NDA, Requires Legal Review

---

### Picklist (Single Select)

**Display:** Dropdown selector — user picks exactly one value

**Configuration:**
- Add values (one per line)
- Set a default value
- Control sort order (alphabetical or custom)
- Mark values Inactive (remain on existing records, unavailable for new)
- Global Value Sets — shared picklist reused across multiple objects

**Use for:** When the value MUST be one specific option: Stage, Status, Type, Industry, Priority

**Global Picklists (Global Value Sets):** Create once, share across many fields. Update the global set → all fields using it update automatically.

> **Example:** Create Global Value Set "Priority" with High/Medium/Low. Both Case.Priority and Task.Priority use it. Add "Critical" → both fields get it automatically.

---

### Picklist (Multi-Select)

**Display:** Dual-list selector — user picks one or more values

**Stored as:** Semicolon-delimited: `Java;Python;SQL`

**Limitations:**
- Cannot be a Controlling Field in Dependent Picklists
- Cannot be used in Roll-Up Summary filters
- Requires INCLUDES operator in SOQL: `WHERE Skills__c INCLUDES ('Java')`
- Harder to report on — each combination of selected values is a unique value

**Use sparingly.** If you find yourself constantly filtering on specific values, consider a junction object instead.

---

### Lookup Relationship

**Stores:** A reference (ID) to another record

**Properties:**
- Child can exist WITHOUT a parent (field is optional by default)
- Parent deletion: child remains, lookup field is cleared (default behavior)
- Does NOT enable Roll-Up Summary
- Does NOT affect sharing
- Max 25 Lookup relationships per object

**"What to do if the lookup record is deleted?" setting options:**
| Option | Behavior |
|--------|----------|
| Clear the value (default) | Child stays, lookup becomes blank |
| Don't allow deletion if children exist | Blocks the parent delete |
| Delete this record also | Cascade delete (like Master-Detail behavior) |

**Lookup Filter:** Add a filter to restrict which records a user can select in the lookup.
> Example: Contact's Account lookup filtered to only show Accounts in the same Industry.

---

### Master-Detail Relationship

**Stores:** A required reference to a parent record

**Properties:**
- Child CANNOT exist without parent (required — cannot be null)
- Deleting parent CASCADE DELETES all children (permanent — goes to Recycle Bin together)
- Enables Roll-Up Summary on parent
- Child inherits parent's sharing (OWD = "Controlled by Parent")
- Max 2 Master-Detail relationships per custom object

**The Critical Decision — Master-Detail or Lookup?**

| Ask Yourself | Answer = Yes → Use |
|-------------|-------------------|
| Can the child exist without the parent? | NO → Master-Detail |
| Do you need Roll-Up Summary on the parent? | YES → Master-Detail |
| Should child inherit parent's sharing? | YES → Master-Detail |
| Should deleting parent delete children? | YES → Master-Detail |

---

### Formula

**Read-only.** Recalculates on every record load/save. Covered in detail in Section 3.13.

---

### Roll-Up Summary

**Read-only.** Aggregates values from child records in a Master-Detail relationship. Covered in Section 3.10.

---

### Auto Number

**System-managed, read-only.** Sequential, formatted identifier assigned on record creation.

**Format:** You define — `JOB-{0000}` → JOB-0001, JOB-0002...

**Cannot:** Be manually entered, changed, or reset (except by admin tools)

**Use for:** Case Number, Invoice Number, Application ID, any sequential reference number

---

### External ID

**Not a separate type** — a property enabled on Text, Number, or Email fields.

**Enables:**
1. An index for faster lookup
2. Used as a matching key in Data Loader Upsert operations
3. Linking related records by External ID during imports (not needing Salesforce ID)

---

### URL, Email, Phone

**URL:** Stores web addresses — displays as clickable link
**Email:** Stores email addresses — displays as mailto: link
**Phone:** Stores phone numbers — displays as clickable call on mobile

---

### Encrypted Text

**Stores:** Sensitive text data — encrypted at rest, displayed as masked (*)

**Limitations:** Cannot be used in reports, formulas, or SOQL WHERE

**Use for:** SSNs, passport numbers, credit card fragments, highly sensitive identifiers

---

### Geolocation

**Stores:** Latitude + Longitude coordinate pair

**Enables:** Distance calculations in SOQL (`DISTANCE` and `GEOLOCATION` functions), mapping integrations

**Use for:** Store locations, field service addresses, property coordinates

---

## 3.5 Dependent Picklists

### What and Why

A **Dependent Picklist** shows different values based on what was selected in a **Controlling Field**. This prevents invalid combinations and reduces user confusion.

### Controlling Field Types

| Field Type | Can Control? |
|-----------|-------------|
| Standard Picklist | ✅ Yes |
| Custom Picklist | ✅ Yes |
| Checkbox | ✅ Yes |
| Multi-Select Picklist | ❌ No |

### Full Real-World Examples

**Example 1: Country → State/Province**
| Country | States Shown |
|---------|-------------|
| India | Maharashtra, Karnataka, Tamil Nadu, Delhi, Gujarat, Rajasthan... |
| USA | California, New York, Texas, Florida, Illinois... |
| UK | England, Scotland, Wales, Northern Ireland |
| Australia | NSW, Victoria, Queensland, WA, SA... |

**Example 2: Department → Role (Job Portal)**
| Department | Roles Available |
|-----------|----------------|
| Engineering | Software Engineer, QA Engineer, DevOps, Data Scientist, Architect |
| Marketing | Content Writer, SEO Specialist, Social Media Manager, Brand Manager |
| Sales | Account Executive, BDR, Sales Manager, Pre-Sales Engineer |
| Finance | Accountant, Controller, CFO, Financial Analyst |
| HR | Recruiter, HR Business Partner, Compensation Analyst |

**Example 3: Checkbox as Controlling**
| Is Urgent (Checkbox) | Response Time Options |
|---------------------|----------------------|
| ✅ True (Checked) | Within 1 Hour, Within 4 Hours, Same Day |
| ❌ False (Unchecked) | Within 1 Week, Within 2 Weeks, Next Sprint |

**Example 4: Insurance Type → Coverage Sub-Type**
| Insurance Type | Coverage Sub-Type |
|---------------|-----------------|
| Life | Term Life, Whole Life, Endowment, ULIP |
| Health | Individual, Family Floater, Senior Citizen, Critical Illness |
| Vehicle | Comprehensive, Third-Party Only, Zero Depreciation |
| Property | Home, Office, Factory, Shop |

### Setup Path

**Path:** Setup → Object Manager → [Object] → Fields & Relationships → **Field Dependencies** → New

1. Select Controlling Field
2. Select Dependent Field
3. Check boxes in the dependency matrix
4. Save

### Important Rules

- A dependent picklist can only depend on ONE controlling field
- A controlling field can control MULTIPLE dependent fields
- Multi-Select Picklists cannot be controlling fields
- If no controlling value is selected, the dependent picklist shows NO values

---

## 3.6 Field History Tracking

### What It Does

Creates an audit log recording every change to specified fields:
- What the old value was
- What the new value is
- Who made the change
- Exactly when it occurred

### Limits

| Limit | Value |
|-------|-------|
| Max fields per object | **20** |
| Default retention period | **18 months** |
| Extended retention | Data Archive add-on (paid) |
| History object name | `[ObjectName]History` |

### Enabling

**Step 1:** Object Manager → [Object] → Edit → check **"Track Field History"** → Save

**Step 2:** Object Manager → [Object] → Fields & Relationships → **"Set History Tracking"** → select fields → Save

**Step 3:** Add the History related list to the Page Layout

### Fields to Always Track

| Object | Fields to Track | Why |
|--------|----------------|-----|
| Opportunity | Stage, Amount, Owner, Close Date | When did deal advance? Was amount changed? |
| Case | Status, Priority, Owner | When was case closed? When escalated? |
| Lead | Status, Owner | Lead lifecycle tracking |
| Contract | Status, Contract Amount | Compliance |
| Custom financial objects | Any monetary amount, status | Audit and compliance |

### Real-World Compliance Example

> **InsureCo** is audited by IRDAI. The auditor asks: "For Policy #INS-2024-00892, show every change made to the coverage amount, who authorized it, and when."
>
> **Without history tracking:** Only the current value is visible. The audit fails.
>
> **With history tracking on `Coverage_Amount__c`:**
>
> | Date | User | Old Value | New Value |
> |------|------|-----------|-----------|
> | 01-Jan-2025 | System Admin | — | ₹25,00,000 |
> | 15-Mar-2025 | Priya Agent | ₹25,00,000 | ₹50,00,000 |
> | 20-Jun-2025 | Mark Manager | ₹50,00,000 | ₹75,00,000 |
>
> Full audit trail. Compliance maintained.

---

## 3.7 Page Layouts

### What a Page Layout Controls

- Which fields appear (and which are hidden from the layout)
- Order and section arrangement of fields
- Which fields are Required or Read-Only **on the layout**
- Which Related Lists appear at the bottom and in what order
- Which Buttons appear on the record
- Which Quick Actions appear in the action bar

**Path:** Setup → Object Manager → [Object] → Page Layouts

### Page Layout vs Field-Level Security — Critical Distinction

| | Page Layout | Field-Level Security (FLS) |
|--|-------------|--------------------------|
| Controls | Field arrangement, required-on-layout, buttons | Visibility and editability |
| Applies to | Users on this layout (via Profile/Record Type) | Users with this profile |
| Hides a field? | From this specific layout only | From everywhere |
| Can be bypassed? | Yes — API, Data Loader, reports | No — FLS is absolute |

> **Key principle:** FLS wins. If FLS marks a field as Hidden, removing it from the layout is redundant. If FLS is Visible but the field isn't on the layout, users can still see it in list views, reports, and related lists.
>
> **True hiding = FLS.** Page layouts control presentation. FLS controls access.

### What You Configure

**Sections:** Name, number of columns (1 or 2), visibility (collapsible header)

**Fields:** Drag from palette, mark Required (on layout), mark Read-Only (on layout)

**Related Lists:** Choose related objects, columns, order, and list buttons

**Buttons:** Add/remove standard buttons (Edit, Delete, Clone), add custom buttons

### Assigning Page Layouts

**Path:** Setup → Object Manager → [Object] → Page Layout Assignment

Assign different layouts to different Profile + Record Type combinations.

### Real-World Example

> **ConsultCo** `Project__c` — four layouts for four teams:
>
> **Sales Layout:** Client, Deal Value, Expected Revenue, Probability, Proposal Date
> **Delivery Layout:** Start Date, End Date, Resources, Deliverables, Status, RAG Rating
> **Finance Layout:** Invoice Date, Payment Terms, PO Number, Revenue Recognized
> **Executive Layout:** Project Name, Client, Total Value, Status, Risk Level
>
> Each team sees only what they need. No clutter, no confusion.

---

## 3.8 Record Types

### What Record Types Control

| Controls | Yes/No |
|---------|--------|
| Picklist values available | ✅ Yes |
| Page Layout to show | ✅ Yes |
| Business Process (Stage/Status values) | ✅ Yes |
| Which fields appear on the page | ❌ No (Page Layout does this) |
| Field-level security | ❌ No (FLS does this) |

### Business Process

For standard objects with a Stage/Status picklist (Opportunity, Lead, Case), Record Types control which Stage/Status values are available per Record Type.

> **Example:** Opportunity Record Types at a software company:
>
> **New Business:** Prospecting → Discovery → Demo → Proposal → Negotiation → Closed Won/Lost
>
> **Renewal:** Renewal Identified → Proposal Sent → Renewal Negotiation → Renewed → Not Renewed
>
> A renewal deal shouldn't have "Prospecting" — Record Types with Business Processes solve this cleanly.

### Real-World Examples

**Real Estate `Property__c`:**

| Record Type | Unique Picklist Values | Unique Layout Fields |
|-------------|----------------------|---------------------|
| Residential | Listed, Under Offer, Sold, Off Market | Bedrooms, Bathrooms, HOA Fee, School District |
| Commercial | Available, LOI Signed, Leased, Expired | Floor Area, Zoning, Loading Docks, Parking |
| Industrial | Available, Under Contract, Sold | Ceiling Height, Dock Doors, Power (Amps) |

**Job Posting `Job_Posting__c`:**

| Record Type | Unique Aspects |
|-------------|---------------|
| Internal Posting | Employee-only; Status: Draft, Posted, Filled, Cancelled |
| External Posting | Public job boards; Status: Draft, Live, Applications Closed, Filled |
| Confidential Search | No public posting; Status: Active Search, Shortlisted, Offer Extended |

### Record Type Selection on New Record

When a user clicks "New" on an object with multiple accessible Record Types:
1. Dialog: "Select a record type"
2. User picks the appropriate type
3. Correct page layout loads with correct picklist values

If a user's profile only gives access to one Record Type → selection dialog is skipped.

---

## 3.9 Relationships — Complete Guide

### Overview Table

| Type | Child Requires Parent | Cascade Delete | Roll-Up Summary | Sharing Impact | Max per Object |
|------|----------------------|----------------|-----------------|----------------|----------------|
| Lookup | ❌ Optional | ❌ No | ❌ No | ❌ None | 25 |
| Master-Detail | ✅ Required | ✅ Yes | ✅ Yes | ✅ Inherits parent | 2 |
| Self (Lookup) | ❌ Optional | ❌ No | ❌ No | ❌ None | 1 (typically) |
| Hierarchical | ❌ Optional | ❌ No | ❌ No | ❌ None | 1 (User only) |
| Many-to-Many (junction) | ✅ Both required | ✅ Both cascade | ✅ Via junction | ✅ Inherits | Per design |
| External Lookup | ❌ Optional | ❌ No | ❌ No | ❌ None | Varies |

---

### Lookup — Deep Dive

**Behavior on Parent Delete** (configurable on the Lookup field):

| Setting | Behavior |
|---------|---------|
| **Clear the value** (default) | Child stays, lookup field becomes blank |
| **Don't allow deletion** if children exist | Blocks the parent delete |
| **Delete this record also** | Children are also deleted |

**Lookup Filter:** Restricts which records a user can select.
> On Opportunity, Primary Contact lookup filtered to only Contacts where `AccountId = current Opportunity's AccountId` → prevents linking to contacts at a different company.

**Real-World Example:**
> **HRTech:** `Job_Application__c` has:
> - Lookup to `Candidate__c` (the person applying)
> - Lookup to `Job_Posting__c` (the job)
>
> If a candidate deletes their profile, the application remains (field blanked). Application history is preserved for compliance.

---

### Master-Detail — Deep Dive

**Cascade Delete in Practice:**
> ShopNow deletes a cancelled `Order__c`. All related `Order_Line_Item__c` records are automatically deleted and go to the Recycle Bin with the parent.

**The 2 Master-Detail Limit:**
A custom object can have at most 2 Master-Detail relationships because each Master-Detail means the child inherits that parent's sharing — more than 2 would create an ambiguous sharing model.

**Converting Lookup to Master-Detail:**
Only possible if ALL existing child records already have a value in the lookup field (no nulls allowed).

**Master-Detail Relationship Examples:**

| Master | Detail | Why Master-Detail |
|--------|--------|------------------|
| Order__c | Order_Line_Item__c | Line items meaningless without order |
| Invoice__c | Invoice_Line__c | Same logic |
| Project__c | Project_Milestone__c | Milestones belong to projects |
| Insurance_Policy__c | Policy_Rider__c | Riders are additions to the policy |
| Job_Posting__c | Job_Application__c | Applications belong to a posting |

---

### Self Relationship — Deep Dive

A Lookup from an object to itself — models hierarchies within a single object.

**Employee → Manager (Reports To):**
```
CEO (Alice)
  ├── VP Sales (John)     — Reports To: Alice
  │     ├── Manager East (Sarah) — Reports To: John
  │     │     ├── Rep (Raj)   — Reports To: Sarah
  │     │     └── Rep (Priya) — Reports To: Sarah
  │     └── Manager West (Tom)  — Reports To: John
  └── VP Engineering (Wei) — Reports To: Alice
```

**Product Category → Parent Category:**
```
Electronics → Computers → Laptops
                       → Desktops
           → Mobile   → Smartphones
                       → Tablets
```

**Account → Parent Account (built-in on standard Account):**
Built-in self-relationship. Enables corporate hierarchy visibility, account hierarchy page, and roll-up reporting across subsidiary levels.

---

### Hierarchical Relationship

Available **only on the User object**. Used for:
- Building the management hierarchy (Manager field on User)
- Approval routing — "Route to submitter's manager"
- Role-independent manager lookups

**Different from Role Hierarchy:** The Hierarchical relationship on User tracks organizational reporting structure (who reports to whom). The Role Hierarchy controls record sharing visibility. They may or may not match.

---

### Many-to-Many (Junction Object) — Deep Dive

**The Problem:** No native many-to-many field type exists in Salesforce.

**Business scenarios requiring Many-to-Many:**
- Doctor ↔ Department (doctor works in many, department has many)
- Student ↔ Course (student takes many, course has many)
- Speaker ↔ Event (speaker at many, event has many)
- Product ↔ Category (product in many categories, category has many products)
- Tag ↔ Article (tag on many, article has many)

**The Solution:** Junction Object = custom object with TWO Master-Detail relationships.

```
Object A ──(Master-Detail)── Junction Object ──(Master-Detail)── Object B
```

**Standard Examples (already in Salesforce):**
- `CampaignMember` — junction between `Campaign` and `Lead`/`Contact`
- `OpportunityContactRole` — junction between `Opportunity` and `Contact`

**Custom Example — Conference Management:**
```
Speaker__c ──(MD)── Speaker_Session__c ──(MD)── Event__c
```

Junction adds context: Session Time, Room, Duration, Topic

**Cascade Delete Behavior:**
- Delete Speaker → their Speaker_Session records deleted → Event records NOT deleted
- Delete Event → its Speaker_Session records deleted → Speaker records NOT deleted

**Roll-Up Summary on junction:**
- Add COUNT on Speaker: "How many events has this speaker presented at?"
- Add COUNT on Event: "How many speakers are presenting at this event?"

---

### External Lookup & Indirect Lookup

Connect Salesforce to external data sources via Salesforce Connect.

**External Lookup:** Salesforce Object → External Object (live data from another system)

**Indirect Lookup:** External Object → Salesforce Object (external system pointing into Salesforce)

**Real-World Use:**
> ManufactureCo stores inventory in SAP (updates constantly). Using External Lookup, sales reps see live stock levels on the Product record — data read directly from SAP in real time, never duplicated into Salesforce. Changes in SAP appear instantly.

---

## 3.10 Roll-Up Summary Fields

### Aggregate Functions

| Function | Calculates | Supported Field Types |
|---------|-----------|----------------------|
| **COUNT** | Number of child records | Any (just counts records) |
| **SUM** | Total of a field | Number, Currency, Percent |
| **MIN** | Smallest value | Number, Currency, Percent, Date, DateTime |
| **MAX** | Largest value | Number, Currency, Percent, Date, DateTime |

### Filter Criteria Examples

On `Account`, Roll-Up Summary fields using filters:

| Field | Function | Filter | Result |
|-------|---------|--------|--------|
| Total Opportunities | COUNT | None | All opportunities |
| Open Pipeline Count | COUNT | Stage ≠ Closed Won/Lost | Active deals only |
| Won Revenue This Year | SUM of Amount | Stage = Closed Won AND Close Date = THIS_YEAR | YTD closed revenue |
| Largest Deal | MAX of Amount | Stage = Closed Won | Biggest closed deal |
| First Purchase Date | MIN of Close Date | Stage = Closed Won | Earliest win |

### Real-World Roll-Up Examples

**On `Job_Posting__c`:**
- Total Applications (COUNT of Job_Application__c)
- Applications in Interview (COUNT WHERE Status = "Technical Interview")
- Hired Count (COUNT WHERE Status = "Hired")
- Rejection Rate = Hired Count / Total Applications (Formula using the above two roll-ups)

**On `Invoice__c`:**
- Total Invoice Amount (SUM of Invoice_Line__c.Line_Amount__c)
- Number of Line Items (COUNT)
- Largest Line Item (MAX of Line_Amount__c)

### Roll-Up Summary Limitations

- Only works on the **Master** side of a Master-Detail (not Lookup)
- Cannot aggregate across multiple relationship levels (no grandparent → grandchild)
- Cannot aggregate Long Text Area or Checkbox fields
- Cross-object formula fields cannot be used in Roll-Up filter criteria
- Maximum 25 Roll-Up Summary fields per object

---

## 3.11 Schema Builder

**Path:** Setup → Schema Builder

### What You Can Do

**View:** All objects as boxes, relationship lines, field details on click, filter which objects show

**Build:** Create new Custom Objects, add Fields to existing objects, create Relationships visually

### When to Use Schema Builder vs Object Manager

| Task | Better Tool |
|------|------------|
| Understand an existing data model | Schema Builder (visual overview) |
| Create a new object (full config) | Object Manager (more options) |
| Add a field to an existing object | Object Manager (faster, more options) |
| See how objects connect visually | Schema Builder |
| Configure object settings, history, search | Object Manager |

**Real-World Use:** A new admin joins a company with 15 custom objects. Schema Builder → select all custom objects → see the full relationship diagram → understand the data model in 30 minutes instead of clicking through Object Manager for hours.

---

## 3.12 Activity Management

### Tasks vs Events

| Dimension | Task | Event |
|-----------|------|-------|
| Represents | A to-do action item | A calendar appointment |
| Time | Due Date (date only) | Start + End DateTime |
| Completion | Status: Not Started → In Progress → Completed | Occurs at scheduled time |
| Calendar sync | No | Yes — syncs with Outlook/Google Calendar |
| Typical use | "Call Sarah by Friday" | "Demo call, Tue 2pm–3pm" |

### Activity Timeline

Shows on Accounts, Contacts, Leads, Opportunities, Cases, and custom objects (if Activities enabled):
- All logged calls (completed Tasks, Type = Call)
- All Tasks (open and completed)
- All Events (past and upcoming)
- All Emails (if email logging is enabled)

### Einstein Activity Capture (EAC)

Automatically logs emails and calendar events from Gmail or Outlook into Salesforce — without manual logging by reps.

**Result:** Emails between reps and customers appear automatically in Activity Timeline. Calendar meetings with customers appear as Events. Reps save hours per week.

### Real-World Workflow

> **Priya** works on the Infosys Opportunity:
> - Monday 10AM: Creates **Event** "Discovery Call — Infosys" (syncs to Outlook)
> - During call: Takes notes in a Task
> - After call: Logs completed **Task** "Discovery Call — agreed to demo, concerned about implementation timeline" Type = Call
> - Creates follow-up **Task**: "Send implementation timeline doc" — Due Wednesday
>
> When the VP opens the Infosys Opportunity, the Activity Timeline shows the complete history. The VP knows exactly where things stand without asking Priya.

---

## 3.13 Formula Fields — Complete Guide

### What is a Formula Field?

A read-only field whose value is automatically calculated from other fields. Recalculates every time the record is loaded or saved.

**Key characteristics:**
- Read-only — users cannot type into a formula field
- Always current — reflects latest values of source fields
- Cross-object capable — can reference fields on related objects via dot notation
- No code required — point-and-click formula editor

### Formula Return Types

| Return Type | Example Formula | Output |
|-------------|----------------|--------|
| Text | `FirstName & " " & LastName` | "Priya Sharma" |
| Number | `Renewal_Date__c - TODAY()` | 45 |
| Currency | `Amount * (Discount__c / 100)` | ₹15,000 |
| Percent | `(Amount - Cost__c) / Amount` | 35% |
| Date | `CloseDate + 30` | Date 30 days after Close |
| Checkbox | `Amount > 1000000` | True/False |

### Core Formula Functions

#### Text Functions

| Function | Usage | Example |
|----------|-------|---------|
| `TEXT(value)` | Converts non-text to text | `TEXT(AnnualRevenue)` |
| `VALUE(text)` | Converts text to number | `VALUE("123") + 1` → 124 |
| `UPPER(text)` | Uppercase | `UPPER(FirstName)` → "PRIYA" |
| `LOWER(text)` | Lowercase | `LOWER(Email)` |
| `LEN(text)` | Character count | `LEN(Description)` |
| `LEFT(text, n)` | First n characters | `LEFT(Account.Name, 3)` |
| `FIND(find, text)` | Position of substring | `FIND("@", Email)` |
| `SUBSTITUTE(text, old, new)` | Find and replace | `SUBSTITUTE(Phone, " ", "")` |
| `TRIM(text)` | Remove leading/trailing spaces | `TRIM(Name)` |
| `BEGINS(text, compare)` | Starts with? | `BEGINS(Code, "CUST-")` |
| `CONTAINS(text, compare)` | Contains? | `CONTAINS(Desc, "urgent")` |

#### Math Functions

| Function | Usage |
|----------|-------|
| `ABS(n)` | Absolute value |
| `ROUND(n, digits)` | Round to decimal places |
| `CEILING(n)` / `FLOOR(n)` | Round up / down to integer |
| `SQRT(n)` | Square root |
| `MAX(n1, n2...)` / `MIN(...)` | Largest / smallest |
| `MOD(n, divisor)` | Remainder |

#### Date Functions

| Function | Usage |
|----------|-------|
| `TODAY()` | Current date |
| `NOW()` | Current date + time |
| `DATE(year, month, day)` | Construct a date |
| `DATEVALUE(datetime)` | Extract date from DateTime |
| `YEAR(date)` / `MONTH(date)` / `DAY(date)` | Extract components |
| `ADDMONTHS(date, n)` | Add months |
| `WEEKDAY(date)` | Day of week (1=Sun, 7=Sat) |

#### Logical Functions

| Function | Usage |
|----------|-------|
| `IF(cond, true, false)` | Conditional |
| `AND(c1, c2...)` | All conditions true |
| `OR(c1, c2...)` | Any condition true |
| `NOT(cond)` | Invert |
| `ISBLANK(field)` | True if empty |
| `ISNEW()` | True if creating new record |
| `ISCHANGED(field)` | True if value changed |
| `PRIORVALUE(field)` | Value before current edit |
| `CASE(expr, v1, r1, v2, r2, else)` | Switch statement |

#### Picklist Functions

| Function | Usage |
|----------|-------|
| `ISPICKVAL(field, "value")` | True if picklist = value |
| `TEXT(picklist_field)` | Convert picklist to text |

#### Cross-Object References

```
Contact formula → Account.Industry      (Contact's Account's Industry)
Opportunity formula → Account.BillingCity
Case formula → Contact.Email
```

**Up to 5 levels deep:** `Contact.Account.Owner.Manager.Name` (4 levels)

---

### Real-World Formula Examples

**Formula 1 — Deal Size Category (Text)**
```
IF(Amount >= 10000000, "Enterprise (₹1Cr+)",
  IF(Amount >= 1000000, "Large (₹10L-₹1Cr)",
    IF(Amount >= 100000, "Mid (₹1L-₹10L)",
      IF(Amount > 0, "Small (<₹1L)",
        "No Amount"))))
```

**Formula 2 — SLA Status for Cases (Text)**
```
IF(ISBLANK(Closed_Date__c),
  IF(TODAY() - DATEVALUE(CreatedDate) > 5, "⚠️ OVERDUE",
    IF(TODAY() - DATEVALUE(CreatedDate) > 3, "⏳ AT RISK",
      "✅ ON TRACK")),
  "✔️ CLOSED")
```

**Formula 3 — Annual Contract Value (Currency)**
```
Monthly_Revenue__c * 12
```

**Formula 4 — Policy Status for Insurance (Text)**
```
IF(ISBLANK(End_Date__c), "No End Date Set",
  IF(End_Date__c < TODAY(), "EXPIRED",
    IF(End_Date__c - TODAY() <= 30,
      "EXPIRING SOON (" & TEXT(End_Date__c - TODAY()) & " days)",
      "ACTIVE")))
```

**Formula 5 — Commission Amount (Currency)**
```
IF(ISPICKVAL(StageName, "Closed Won"),
  IF(Amount >= 10000000, Amount * 0.12,
    IF(Amount >= 5000000, Amount * 0.10,
      IF(Amount >= 1000000, Amount * 0.08,
        Amount * 0.05))),
  0)
```

**Formula 6 — Job Summary (Text)**
```
Job_Type__c & " | " & Department__c & " | " &
IF(Is_Remote__c, "Remote", "On-Site") & " | ₹" &
TEXT(ROUND(Salary_Range__c, 0))
```

**Formula 7 — Days Since Application (Number)**
```
IF(ISBLANK(Application_Date__c), 0,
  TODAY() - Application_Date__c)
```

**Formula 8 — Is High Value Account? (Checkbox)**
```
Account.AnnualRevenue >= 100000000
```
Returns True if Contact's Account has revenue ≥ ₹10 Crore.

---

## 3.14 Object Manager

**Object Manager** is the central hub for managing all objects — standard and custom.

**Path:** Setup → Object Manager

### Sub-Sections Within Any Object

| Section | What You Configure |
|---------|------------------|
| **Details** | Object label, API name, description, settings |
| **Fields & Relationships** | All fields — create, edit, delete, history tracking |
| **Page Layouts** | Create and assign page layouts |
| **Lightning Record Pages** | App Builder for record pages |
| **Compact Layouts** | Fields in the Highlights Panel |
| **Record Types** | Create and manage record types |
| **Business Processes** | Stage/Status value sets per record type |
| **Validation Rules** | Data quality rules |
| **Triggers** | Apex code (developer area) |
| **Search Layouts** | Fields in search results and list views |
| **Buttons, Links, Actions** | Custom buttons and Quick Actions |

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Explore Standard Object Structure

**Objective:** Understand how standard objects are built before creating custom ones.

**Steps:**
1. Setup → Object Manager → click **Account**
2. Explore every section — count: how many standard fields? What types?
3. Click the `Industry` field → it's a Picklist
4. Click `AnnualRevenue` → it's Currency
5. Click `ParentId` (Parent Account) → it's a Lookup to Account (self-relationship)
6. Now click **Contact** → find `AccountId` field → what type is it?

**Build a comparison table:**
| Object | Field | Type | Relationship Type |
|--------|-------|------|------------------|
| Account | ParentId | Lookup | Self (Account → Account) |
| Contact | AccountId | Lookup | Contact → Account |
| Opportunity | AccountId | Lookup | Opportunity → Account |
| Case | AccountId | Lookup | Case → Account |

**Question:** Why are all Account relationships on Contact/Opportunity/Case Lookup (not Master-Detail)?

---

### 🔧 Task 2 — Create the Job Posting Object (Full Config)

**Objective:** Build a production-quality custom object.

**Path:** Setup → Object Manager → Create → Custom Object

**Configuration:**
- Label: `Job Posting` | Plural: `Job Postings`
- Object Name: `Job_Posting` (→ `Job_Posting__c`)
- Record Name Label: `Job Title` | Type: Text
- ✅ Allow Reports | ✅ Allow Activities | ✅ Track Field History | ✅ Allow Sharing | ✅ Allow Search
- Deployment Status: **Deployed**

**Verify:** App Launcher → search "Job Posting" → tab appears

---

### 🔧 Task 3 — Add All Field Types

**Objective:** Practice creating every major field type.

**Path:** Object Manager → `Job_Posting__c` → Fields & Relationships → New

| # | Label | API Name | Type | Key Config |
|---|-------|---------|------|------------|
| 1 | Company Name | `Company_Name__c` | Text | Length 200, Required |
| 2 | Job Type | `Job_Type__c` | Picklist | Full-Time, Part-Time, Contract, Internship, Freelance |
| 3 | Salary Range | `Salary_Range__c` | Currency | Decimal 2 |
| 4 | Application Deadline | `Application_Deadline__c` | Date | |
| 5 | Is Remote | `Is_Remote__c` | Checkbox | Default: unchecked |
| 6 | Job Description | `Job_Description__c` | Text Area (Long) | |
| 7 | Experience Required | `Experience_Years__c` | Number | Decimal 0 |
| 8 | Job Code | `Job_Code__c` | Text | Length 10, Unique ✅, External ID ✅ |
| 9 | Department | `Department__c` | Picklist | Engineering, Marketing, Sales, Finance, HR, Operations |
| 10 | Posted Date | `Posted_Date__c` | Date | |
| 11 | Company Website | `Company_Website__c` | URL | |
| 12 | HR Contact Email | `HR_Contact_Email__c` | Email | |
| 13 | Application ID | `Application_ID__c` | Auto Number | Format: `JOB-{0000}` |
| 14 | Status | `Status__c` | Picklist | Draft, Active, Closed, Filled |

After creating fields: create a Job Posting record and fill in all fields. Observe how each type renders in the UI.

---

### 🔧 Task 4 — Dependent Picklist (Department → Role)

**Steps:**
1. Create Role Picklist field on `Job_Posting__c` with values:
   Software Engineer, QA Engineer, DevOps, Data Scientist, Content Writer, SEO Specialist, Social Media Manager, Account Executive, BDR, Sales Manager, Accountant, Financial Analyst, Controller, Recruiter, HR Business Partner, Operations Manager

2. Object Manager → `Job_Posting__c` → Fields & Relationships → **Field Dependencies** → New

3. Controlling: `Department__c` | Dependent: `Role__c`

4. Matrix mapping:
   - Engineering → Software Engineer, QA Engineer, DevOps, Data Scientist
   - Marketing → Content Writer, SEO Specialist, Social Media Manager
   - Sales → Account Executive, BDR, Sales Manager
   - Finance → Accountant, Financial Analyst, Controller
   - HR → Recruiter, HR Business Partner
   - Operations → Operations Manager

5. Save → Test: create a Job Posting, select Engineering → verify only engineering roles show. Change to Finance → roles change completely.

---

### 🔧 Task 5 — Field History Tracking

**Steps:**
1. Object Manager → `Job_Posting__c` → Fields & Relationships → **Set History Tracking**
2. Enable: `Salary_Range__c`, `Application_Deadline__c`, `Status__c`, `Job_Type__c`
3. Save

4. Add "Job Posting History" related list to the page layout

5. Open a Job Posting → change Salary Range (0 → ₹10,00,000) → Save
6. Change Status (Draft → Active) → Save
7. Change Salary Range (₹10,00,000 → ₹12,00,000) → Save

8. Scroll to "Job Posting History" related list
**Verify:** 3 history records, each showing: Date, User, Field, Old Value, New Value

---

### 🔧 Task 6 — Master-Detail + Roll-Up Summary

**Step A — Create Job Application Object:**
1. New Custom Object: `Job_Application__c`
   - Record Name: `Applicant Name` | Track Field History ✅

**Step B — Add Fields:**
| Field | Type | Config |
|-------|------|--------|
| Job Posting | Master-Detail | Related To: `Job_Posting__c` |
| Applicant Email | Email | Required |
| Application Status | Picklist | Applied, Screening, Phone Interview, Technical Interview, Offer Extended, Hired, Rejected |
| Application Date | Date | |
| Years of Experience | Number | |

**Step C — Roll-Up Summary Fields on `Job_Posting__c`:**
| Label | Type | Child | Filter |
|-------|------|-------|--------|
| Total Applications | COUNT | Job_Application__c | None |
| In Interview Stage | COUNT | Job_Application__c | Status = "Technical Interview" |
| Total Hired | COUNT | Job_Application__c | Status = "Hired" |

**Test:** Create 5 Job Applications for one Job Posting with different statuses.
Verify: Total Applications = 5, Interview count = correct, Hired count = correct.

---

### 🔧 Task 7 — Many-to-Many Junction Object

**Objective:** Model Job Postings ↔ Required Skills.

**Step A:** Create `Skill__c` custom object (Record Name: Skill Name)

**Step B:** Create `Job_Skill_Requirement__c` junction object
- Add Master-Detail → `Job_Posting__c`
- Add Master-Detail → `Skill__c`
- Add Picklist: `Proficiency_Level__c` → Beginner, Intermediate, Advanced, Expert

**Test:**
1. Create Skills: Java, Python, SQL, Salesforce Flow
2. Create a "Data Engineer" Job Posting
3. Create junction records linking it to Java (Advanced), Python (Expert), SQL (Intermediate)
4. Open the Job Posting → see Required Skills in the related list
5. Open the Java Skill → see all Job Postings requiring Java in the related list

---

### 🔧 Task 8 — Formula Fields

**Create on `Job_Posting__c`:**

**Formula 1 — Deadline Status (Text):**
```
IF(
  ISBLANK(Application_Deadline__c),
  "No Deadline Set",
  IF(
    Application_Deadline__c < TODAY(),
    "EXPIRED",
    IF(
      Application_Deadline__c - TODAY() <= 7,
      "CLOSING SOON — " & TEXT(Application_Deadline__c - TODAY()) & " days left",
      "OPEN — " & TEXT(Application_Deadline__c - TODAY()) & " days left"
    )
  )
)
```

**Formula 2 — Is High Paying? (Checkbox):**
```
Salary_Range__c >= 2000000
```

**Formula 3 — Job Summary (Text):**
```
Job_Type__c & " | " & Department__c & " | " &
IF(Is_Remote__c, "Remote", "On-Site") & " | ₹" &
TEXT(ROUND(Salary_Range__c, 0))
```

**Formula 4 — Days Since Posted (Number):**
```
IF(ISBLANK(Posted_Date__c), 0, TODAY() - Posted_Date__c)
```

**Test:** Create Job Postings with different configurations. Observe formula values update automatically. No user input needed.

---

### 🔧 Task 9 — Record Types for Job Postings

**Steps:**
1. Object Manager → `Job_Posting__c` → Record Types → **New**
2. Create: `Internal Job Posting` (Active ✅)
3. Create: `External Job Posting` (Active ✅)

**Create two Page Layouts** (clone existing for each):
- Internal Layout: add `Internal_Grade__c`, `Referral_Bonus__c` fields (create these first)
- External Layout: add `Public_Description__c`, `Job_Board_URL__c` fields (create these first)

**Assign layouts:**
4. Object Manager → Page Layout Assignment
5. Internal Job Posting → Internal Layout
6. External Job Posting → External Layout

**Test:** New Job Posting → Record Type dialog appears → choose Internal → correct layout loads with correct fields.

---

### 🔧 Task 10 — Schema Builder Visualization

**Steps:**
1. Setup → **Schema Builder**
2. Select and add to canvas: `Job_Posting__c`, `Job_Application__c`, `Job_Skill_Requirement__c`, `Skill__c`, `User`
3. Observe:
   - Solid lines = Master-Detail relationships
   - Dashed lines = Lookup relationships
4. Click a relationship line → see field name and type
5. Click any field → see configuration in the right panel

**Create a new relationship from Schema Builder:**
6. Elements panel → drag **Lookup Relationship** between `Job_Posting__c` and `Account`
7. Configure: "Hiring Company Account" — the Account this posting is for
8. Save

---

## 📝 Practice Questions

**Q1.** What is the API name for a custom object with label "Training Session"?

**Q2.** A user tries to type 300 characters in a Text field with length 255. What happens?

**Q3.** What are three things Text Area (Long) CANNOT do that a regular Text (255) field CAN?

**Q4.** When should you use Currency instead of Number? Give two specific reasons.

**Q5.** Can a Picklist field be both a Dependent field (controlled by another) AND a Controlling field (controlling a third field)?

**Q6.** What is the maximum number of fields trackable with Field History Tracking per object? How long is history retained?

**Q7.** What condition must be met before you can convert a Lookup relationship to a Master-Detail?

**Q8.** A Roll-Up Summary on Account sums all Opportunity Amounts where Stage = "Closed Won". An Opportunity is deleted. What happens?

**Q9.** A field is "Required" on the Page Layout. Can an admin insert a record via Data Loader without providing this field?

**Q10.** A Junction Object has two Master-Detail relationships — one to Object A, one to Object B. Object A record is deleted. What happens to Object B?

**Q11.** Can you create a Roll-Up Summary field on a Lookup relationship?

**Q12.** A formula on Contact uses `Account.Industry`. What is this called and how many levels of traversal does Salesforce support?

**Q13.** What does marking a Text field as "External ID" enable? Give three specific capabilities.

**Q14.** Write the formula to count Total Applications and display "None Applied" if 0, otherwise show the number. *(Uses Roll-Up field `Total_Applications__c`)*

**Q15.** What is the difference between making a field Required on a Page Layout vs Required at the field level?

**Q16.** What is `PRIORVALUE()` and give a real use case where you'd need it?

**Q17.** Auto Number fields generate sequential numbers. What is the only way to reset the sequence?

**Q18.** A Multi-Select Picklist has Java and Python selected. What is the raw stored value in the database?

**Q19.** What is the maximum number of Master-Detail relationships per custom object?

**Q20.** A company creates a custom object `Order__c` with a Roll-Up Summary on `Account` summing `Total_Order_Amount__c`. The relationship between Order and Account is a Lookup. Does the Roll-Up Summary work?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Hospital Data Model**

> You are building Salesforce for "CityHospital":
> - Multiple **Departments** (Cardiology, Orthopaedics, Neurology, Oncology)
> - Each Department has multiple **Doctors**
> - A Doctor can work in multiple Departments (many-to-many)
> - **Patients** register and get a unique Patient ID
> - Each visit generates a **Medical Record** (patient, doctor, diagnosis, treatment, date)
> - Medical Records should auto-delete if the Patient is deleted
> - Hospital needs: "Total visits per doctor" and "Total patients per department"
> - Patient changes (Name, DOB, Blood Group) must be audited
> - Appointment scheduling tracks patient, doctor, department, and time
>
> **Design the complete data model:**
> 1. List every object (standard + custom) with API names
> 2. Specify relationship type (Lookup vs Master-Detail) for each and justify
> 3. Identify all junction objects and their two parent relationships
> 4. Specify all Roll-Up Summary fields (object, function, filter)
> 5. Which fields need Field History Tracking?
> 6. Draw a text-based relationship diagram

---

**Scenario 2 — Field Type Decision**

> "FinanceApp" is building a loan management system. Choose the correct field type for each:
>
> 1. Loan Application ID — system-generated, unique, sequential (e.g., "LOAN-000042")
> 2. Loan Amount — monetary, used in roll-up summaries, supports multi-currency
> 3. Annual income of applicant — monetary
> 4. Interest rate (e.g., 8.5%)
> 5. Loan Type — Home, Vehicle, Personal, Education, Business (exactly one)
> 6. Collateral assets — Property, Gold, Fixed Deposit, Shares (multiple allowed)
> 7. Application date (date only, no time)
> 8. Credit score (integer, 300–850)
> 9. Co-applicant's name (text, max 200 chars)
> 10. Whether applicant has existing loan (yes/no)
> 11. Loan purpose — detailed multi-paragraph description
> 12. EMI calculated field (mathematical formula)
> 13. Applicant's PAN number — unique in org, used as import key
> 14. Assigned loan officer — link to a Salesforce User
> 15. Days since application — calculated from application date to today

---

**Scenario 3 — E-Commerce Data Model**

> "ShopNow" is building B2B e-commerce on Salesforce:
> - Companies buy products (use standard Account)
> - Each Company has multiple Buyers (standard Contact)
> - Products exist in a catalog (standard Product2)
> - Products belong to Categories — a Product can be in multiple Categories; a Category contains many Products
> - Orders placed by a Buyer for their Company — Order cannot exist without Company
> - Each Order has multiple Order Line Items — Line Item cannot exist without Order
> - Suppliers supply Products — a Product can have multiple Suppliers; a Supplier supplies many Products
>
> **Questions:**
> 1. List all objects with type (Standard/Custom) and API names
> 2. For each relationship: Lookup or Master-Detail? Why?
> 3. Identify all many-to-many relationships and design the junction objects
> 4. List at least 4 Roll-Up Summary fields with aggregate function and placement
> 5. What standard objects can be reused vs what requires custom?

---

**Scenario 4 — Formula Field Design**

> "SalesMax" needs formula fields on Opportunity. Write the complete formula for each:
>
> 1. **Commission Tier (Text):** "Platinum" ≥₹1Cr, "Gold" ₹50L–₹1Cr, "Silver" ₹10L–₹50L, "Bronze" <₹10L, "No Amount" if blank
>
> 2. **Expected Commission (Currency):** 12% Platinum, 10% Gold, 8% Silver, 5% Bronze — only if Stage = "Closed Won", else 0
>
> 3. **Deal Age (Number):** Days open — if Closed: Created to Close Date; if still open: Created to today
>
> 4. **Account Region (Text):** Map Account's BillingState:
>    - Maharashtra/Gujarat/Rajasthan → "West"
>    - Karnataka/Tamil Nadu/Kerala/Andhra Pradesh → "South"
>    - Delhi/UP/Haryana/Punjab → "North"
>    - West Bengal/Odisha/Bihar → "East"
>    - Everything else → "Central/Other"
>
> 5. **Deal Health Score (Number 1–10):** Probability × 5 points (max 5) + 2 points if Next Step is filled + 1 point if Close Date is future + 1 point if Amount > 0 + 1 point if Primary Contact is filled

---

**Scenario 5 — Record Type and Page Layout Design**

> "GlobalInsurance" tracks three policy types on `Policy__c`:
>
> **Life Insurance fields:** Sum Assured, Premium Amount, Policy Term (Years), Nominee Name, Nominee Relationship
> **Life Status values:** Draft, Active, Lapsed, Surrendered, Matured
>
> **Health Insurance fields:** Coverage Amount, Annual Premium, Family Size, Pre-existing Conditions, Network Hospitals
> **Health Status values:** Draft, Active, Renewal Pending, Expired, Cancelled
>
> **Vehicle Insurance fields:** Vehicle Make, Vehicle Model, Registration Number, IDV, NCB Percentage
> **Vehicle Status values:** Draft, Active, Expired, Claimed, Cancelled
>
> All types share: Policy Number, Policy Holder (Contact), Start Date, End Date, Agent (User)
>
> **Questions:**
> 1. How many Record Types? Name them.
> 2. How many Page Layouts? Name them.
> 3. How many Business Processes?
> 4. Record Type → Page Layout assignments?
> 5. List unique fields to create in Object Manager per type
> 6. A new "Insurance Agent" profile user should only create Life and Health policies (not Vehicle). How?
> 7. The Status picklist has all 11 values. When a Life policy agent selects Status, they should only see Life-relevant values. How does Record Type configuration handle this?

---

## ✔️ Answer Key — Practice Questions

1. `Training_Session__c` — spaces become underscores, `__c` appended.

2. Salesforce prevents typing beyond 255 characters at the UI level. If attempted via API, an error is returned. The field enforces the limit unconditionally.

3. Text Area (Long) CANNOT: (a) be used in Validation Rule formulas, (b) be used in SOQL WHERE clauses (not indexed), (c) be used in Flow/Automation entry criteria. Regular Text (255) CAN do all three.

4. Use Currency instead of Number when: (1) the value represents money and you need the currency symbol to display automatically, (2) you have or may enable Multi-Currency — Currency fields convert based on exchange rates automatically, Number fields do not.

5. Yes — a picklist can be both a Dependent field (controlled by Field A) AND simultaneously a Controlling field (controlling Field C). This creates a 3-level dependency chain: A controls B, B controls C.

6. Maximum **20 fields** per object. History retained for **18 months** in standard storage (extendable with Data Archive).

7. Converting a Lookup to Master-Detail requires that NO existing child records have a null value in that lookup field — all existing records must already have a parent linked.

8. The Roll-Up Summary **automatically recalculates** and the value decreases by the deleted Opportunity's amount. Deleted records are excluded from Roll-Up Summary calculations immediately.

9. Yes — Page Layout requirements apply only to saves made through the standard Lightning UI. Admins, API calls, Data Loader, and automation can all save without meeting page layout "Required" settings. Field-level Required (set at the field level) applies to ALL saves including API.

10. Object B is **completely unaffected** — it still exists. The Junction Object record is cascade deleted (it had a Master-Detail to Object A). The relationship between that specific Object A record and Object B no longer exists, but Object B remains.

11. **No.** Roll-Up Summary fields only work on the Master side of a Master-Detail relationship. Lookup relationships do not support Roll-Up Summary.

12. This is called a **Cross-Object Formula**. Salesforce supports up to **5 levels** of relationship traversal (e.g., `Contact.Account.Owner.Manager.Name` = 4 levels deep).

13. Marking a Text field as External ID: (1) Creates a database **index** for faster lookup performance, (2) Allows the field to be used as a **matching key in Data Loader Upsert** operations, (3) Enables **linking related records by this field during data imports** instead of needing Salesforce IDs.

14. `IF(Total_Applications__c = 0, "None Applied", TEXT(ROUND(Total_Applications__c, 0)))` — though since Total_Applications__c is a Number Roll-Up, you could also use `IF(Total_Applications__c = 0, "None Applied", TEXT(Total_Applications__c))`

15. **Required on Page Layout:** Cannot save through the standard Lightning UI without the field. Can be bypassed via API, Data Loader, automation, and Setup imports. **Required at field level:** Applies to ALL saves — UI, API, Data Loader, automation, everything. No save can succeed without the field having a value.

16. `PRIORVALUE(field)` returns the value a field had **before** the current edit/save. Use case: preventing a Stage from moving backwards — `AND(ISPICKVAL(PRIORVALUE(StageName), "Proposal"), ISPICKVAL(StageName, "Prospecting"))` — this detects when Stage was "Proposal" and someone tries to change it back to "Prospecting".

17. Auto Number sequences can only be reset by an admin using the **Rename Object Field** option in Object Manager (involves renaming and reconfiguring the field) or through a data deletion + sequence reset process. Individual values cannot be set manually by any user.

18. `Java;Python` — Multi-Select Picklist stores values as semicolon-delimited text in the database.

19. **2 Master-Detail relationships** per custom object maximum.

20. **No** — Roll-Up Summary fields only work on the Master side of a Master-Detail relationship. If the relationship between Order and Account is a Lookup, not Master-Detail, the Roll-Up Summary field cannot be created there. The relationship must be changed to Master-Detail first.

---

## 🔗 Trailhead Resources

- [Data Modeling](https://trailhead.salesforce.com/content/learn/modules/data_modeling)
- [Customize a Salesforce Object](https://trailhead.salesforce.com/content/learn/projects/customize-a-salesforce-object)
- [Build a Data Model for a Travel Approval App](https://trailhead.salesforce.com/content/learn/projects/build-a-data-model-for-a-travel-approval-app)
- [Quick Start: Customize an App with Lightning Object Creator](https://trailhead.salesforce.com/content/learn/projects/quick-start-customize-an-app-with-lightning-object-creator)
- [Formulas and Validations](https://trailhead.salesforce.com/content/learn/modules/point_click_business_logic)

---

*Previous: [02 · Licenses & User Management](./02_Licenses_and_User_Management.md) | Next: [04 · Security & Access Control →](./04_Security_and_Access_Control.md)*
