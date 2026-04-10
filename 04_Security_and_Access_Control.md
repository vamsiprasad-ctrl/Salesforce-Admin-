# 04 · Security & Access Control — Complete Deep Guide

> **Module:** Security | **Difficulty:** 🟡 Intermediate | **Est. Time:** 6 hrs
> **Covers:** Four security layers · Org Access · Profiles · Permission Sets · PSGs · FLS · OWD · Role Hierarchy · Sharing Rules · Public Groups · Manual Sharing · Restriction Rules · Scoping Rules · Setup Audit Trail · Shield

---

## Table of Contents

1. [Why Security Matters in Salesforce](#41-why-security-matters-in-salesforce)
2. [The Four Layers of Salesforce Security](#42-the-four-layers-of-salesforce-security)
3. [Layer 1 — Org Access](#43-layer-1--org-access)
4. [Layer 2 — Object Access: Profiles](#44-layer-2--object-access-profiles)
5. [Layer 2 — Object Access: Permission Sets](#45-layer-2--object-access-permission-sets)
6. [Layer 2 — Object Access: Permission Set Groups](#46-layer-2--object-access-permission-set-groups)
7. [Layer 3 — Field-Level Security](#47-layer-3--field-level-security-fls)
8. [Layer 4 — Record Access: OWD](#48-layer-4--record-access-org-wide-defaults-owd)
9. [Layer 4 — Record Access: Role Hierarchy](#49-layer-4--record-access-role-hierarchy)
10. [Layer 4 — Record Access: Sharing Rules](#410-layer-4--record-access-sharing-rules)
11. [Layer 4 — Record Access: Public Groups](#411-public-groups)
12. [Layer 4 — Record Access: Manual Sharing](#412-manual-sharing)
13. [Restriction Rules](#413-restriction-rules)
14. [Scoping Rules](#414-scoping-rules)
15. [Setup Audit Trail](#415-setup-audit-trail)
16. [Salesforce Shield — Overview](#416-salesforce-shield--overview)
17. [How All Layers Work Together](#417-how-all-layers-work-together)
18. [Hands-On Tasks](#-hands-on-tasks)
19. [Practice Questions](#-practice-questions)
20. [Scenario-Based Questions](#-scenario-based-questions)
21. [Answer Key](#️-answer-key)

---

## 4.1 Why Security Matters in Salesforce

Salesforce stores some of the most sensitive data in your organization:
- Customer contact information and purchase history
- Deal values, discount rates, pricing strategies
- Employee compensation and performance data
- Medical records, financial data, legal information
- Login credentials and authentication systems

A misconfigured security model means the wrong people see the wrong data — with real business consequences:

| Security Failure | Business Consequence |
|-----------------|---------------------|
| Sales rep sees competitor deal data | Rep poaches deals from peers, destroys team culture |
| Support agent sees salary info | Morale issues, potential legal liability |
| Finance user can edit deal amounts | Revenue recognition errors, compliance failures |
| External partner sees internal data | Competitive intelligence leak |
| Terminated employee retains access | Data theft, sabotage |
| Admin changes not logged | Cannot audit configuration changes for compliance |

Salesforce's security model is powerful but complex — understanding every layer is essential for any admin or consultant.

---

## 4.2 The Four Layers of Salesforce Security

Salesforce security works as a **series of gates**. To access any piece of data, a user must pass through ALL four gates. If blocked at any gate, access is denied — regardless of what other gates allow.

```
┌──────────────────────────────────────────────────────────────┐
│  LAYER 1 — ORG ACCESS                                        │
│  Question: Can this user log into the org at all?            │
│  Controlled by: Login Hours, Login IP Ranges, MFA            │
└──────────────────────────┬───────────────────────────────────┘
                           │ ✅ user logged in
┌──────────────────────────▼───────────────────────────────────┐
│  LAYER 2 — OBJECT ACCESS                                     │
│  Question: Can this user see / create / edit / delete        │
│            records of this OBJECT TYPE?                      │
│  Controlled by: Profiles, Permission Sets, PSGs              │
└──────────────────────────┬───────────────────────────────────┘
                           │ ✅ object access granted
┌──────────────────────────▼───────────────────────────────────┐
│  LAYER 3 — FIELD ACCESS                                      │
│  Question: On records they can access, which FIELDS can      │
│            they see and which can they edit?                 │
│  Controlled by: Field-Level Security (FLS)                   │
└──────────────────────────┬───────────────────────────────────┘
                           │ ✅ field access granted
┌──────────────────────────▼───────────────────────────────────┐
│  LAYER 4 — RECORD ACCESS                                     │
│  Question: Of all records of this type, which SPECIFIC       │
│            records can this user see?                        │
│  Controlled by: OWD → Role Hierarchy → Sharing Rules         │
│                 → Manual Sharing → Restriction Rules         │
└──────────────────────────────────────────────────────────────┘
```

### The Golden Rules

> 🔑 **Rule 1 — Most Restrictive Wins at Each Layer:** If any mechanism at a layer restricts access, it wins over any mechanism that grants it (at that same layer).

> 🔑 **Rule 2 — Sharing is Additive:** At Layer 4, access is ADDITIVE — if any sharing mechanism grants access, the user gets access. OWD sets the floor; everything else can only raise it.

> 🔑 **Rule 3 — Layers Are Independent:** Passing Layer 4 does NOT help if blocked at Layer 2. All four layers must be passed.

### Real-World Example Illustrating All Layers

> **User:** Priya, a Support Agent
> **Action:** She tries to view an Opportunity record
>
> **Layer 1 — Org Access:** It's 9 PM on a weekday. Priya's profile has Login Hours set to 8AM–6PM. She cannot log in. BLOCKED.
>
> (If she could log in:)
> **Layer 2 — Object Access:** Support Agent profile has no Opportunity object permissions. She cannot see Opportunities at all. BLOCKED.
>
> (If object access was granted:)
> **Layer 3 — Field Access:** The `Discount_Rate__c` field is set to Hidden for Support Agent profile. She cannot see discount information even on records she can access.
>
> (If field access was granted:)
> **Layer 4 — Record Access:** OWD for Opportunity is Private. Priya doesn't own this Opportunity and is not in the role hierarchy above the owner. She cannot see this specific record. BLOCKED.
>
> All four layers must be configured correctly for Priya to see the right data.

---

## 4.3 Layer 1 — Org Access

Layer 1 controls whether a user can log into the org at all.

### Login Hours

**What it does:** Restricts when users with a specific profile can log in. Outside the configured hours, the login attempt is blocked with an error message.

**Configuration path:** Setup → Profiles → [Profile] → Login Hours

**How to configure:**
1. Click the profile
2. Scroll to Login Hours
3. Click Edit
4. For each day of the week, set start and end time (or leave blank = 24-hour access that day)
5. To block a day entirely: set start = end (e.g., both 12:00 AM)

**The error message users see:**
`"You cannot log in because your company's login hours do not permit you to log in at this time."`

### Real-World Examples

**Example 1 — Call Center:**
> Profile: Call Center Agent
> Login Hours: Monday–Friday, 8:00 AM – 7:00 PM
> Saturday: 9:00 AM – 1:00 PM (half day for some agents)
> Sunday: No access
>
> An agent tries to log in at 9 PM on a Tuesday — blocked. A manager logging in to check weekend stats — also blocked. Only the System Administrator profile has 24/7 access.

**Example 2 — Shift-Based Support:**
> Profile: Tier 1 Support
> Login Hours: Monday–Friday, 9:00 AM – 6:00 PM
>
> Profile: Tier 2 Escalation
> Login Hours: Monday–Friday, 6:00 PM – 11:59 PM (evening shift)
>
> Profile: On-Call Team
> Login Hours: Saturday–Sunday, all day
>
> Different login hours enforce shift boundaries without relying on agents to log out manually.

**⚠️ Important:** Always configure System Administrator profile with 24/7 login hours. Admins need emergency access at any time — a misconfigured Login Hours on Admin profile can lock everyone out during a critical incident.

---

### Login IP Ranges

**What it does:** Restricts which IP addresses can be used to log in with a specific profile. Attempts from outside the allowed range are blocked — even with correct credentials.

**Configuration path:** Setup → Profiles → [Profile] → Login IP Ranges → New

**How to configure:**
- Enter Start IP Address and End IP Address for each allowed range
- You can add multiple ranges
- IP range format: `10.0.0.1` to `10.0.0.255` covers an entire subnet

**The error message users see:**
`"You cannot log in from this IP address."`

### Real-World Examples

**Example 1 — Corporate Network Only:**
> "BankFirst" requires all Salesforce access from within the corporate office network.
>
> Corporate IP range: `192.168.1.0` – `192.168.1.255`
> VPN IP range: `10.100.0.0` – `10.100.255.255`
>
> Added both ranges to all profiles. Employees working from coffee shops or personal home internet cannot access Salesforce without the VPN.

**Example 2 — Multi-Office Global Company:**
> "GlobalConsult" has offices in Mumbai, London, and Singapore. Each office has a corporate IP range. All three ranges are added to user profiles. Remote workers must use VPN (which assigns an IP within the corporate range).

**Example 3 — Admin Exception:**
> The IT team adds all corporate IP ranges to standard user profiles BUT leaves the System Administrator profile with no IP restriction — so admins can log in from anywhere for emergency support.

---

### Multi-Factor Authentication (MFA)

**What it is:** An additional verification step beyond username/password — requires proof of identity via a second factor (something you have, not just something you know).

**Salesforce's MFA requirement:** Salesforce has **required** MFA for all users since February 2022. This is a contractual obligation, not optional.

**Second factor options:**
| Method | Description | Security Level |
|--------|-------------|---------------|
| **Salesforce Authenticator app** | Mobile push notification — approve with one tap | High |
| **TOTP app** (Google Authenticator, Authy) | Time-based 6-digit code | High |
| **Hardware Security Key** (YubiKey) | Physical key plugged into USB | Very High |
| **Built-in authenticator** | Device biometric or PIN (newer option) | Medium-High |

**How admins enforce MFA:**
1. System Permission: "Multi-Factor Authentication for User Interface Logins" — enable on profiles or permission sets
2. OR: Identity Verification — configure as a required session security level

**MFA bypass (for service accounts):**
Automated system accounts (integration users, batch users) that cannot complete MFA challenges can be given a "waiver" — but this must be documented and minimized.

---

### Trusted IP Ranges (Org-Level)

Different from Profile Login IP Ranges — these are org-level trusted IPs that skip MFA challenges. If a user logs in from a trusted IP, they are not prompted for their second factor.

**Configuration path:** Setup → Security → Network Access → Trusted IP Ranges

**Use carefully:** Adding too many trusted IP ranges defeats the purpose of MFA.

---

## 4.4 Layer 2 — Object Access: Profiles

### What is a Profile?

A **Profile** is the primary security record for a user — defining what they can do in Salesforce. Every user must have exactly **one Profile**. The Profile determines:

- Which objects they can READ, CREATE, EDIT, DELETE
- Which fields are Visible, Read-Only, or Hidden
- Which apps appear in the App Launcher
- Which tabs are visible
- Which record types are available
- System-level permissions (Export Reports, Manage Users, etc.)
- Login Hours and Login IP Ranges

Think of a Profile as a **base security clearance** — it defines the minimum and maximum of what a user type can do.

### CRUD Object Permissions

For each object, profiles control four permissions:

| Permission | What It Allows |
|-----------|---------------|
| **Read** | See records of this object (view the list view, open records) |
| **Create** | Create new records of this object |
| **Edit** | Modify existing records of this object |
| **Delete** | Delete records of this object (also requires Read) |

**Additional object-level permissions:**
| Permission | What It Allows |
|-----------|---------------|
| **View All** | See ALL records of this object regardless of sharing and OWD |
| **Modify All** | Edit and delete ALL records regardless of sharing |

> **View All and Modify All** are extremely powerful — they bypass record-level security entirely. Grant only to users who genuinely need org-wide data access (e.g., data migration admins, reporting managers).

### Standard Profiles — Complete Reference

Salesforce ships with six standard profiles that cannot be deleted:

| Profile | Purpose | Key Characteristics |
|---------|---------|-------------------|
| **System Administrator** | Full platform control | All permissions enabled; unrestricted access to all data and config |
| **Standard User** | General CRM user | Basic CRUD on most CRM objects; no data management or admin powers |
| **Read Only** | View-only access | Read access on most objects; no Create, Edit, or Delete |
| **Solution Manager** | Manages help documentation | Special access to Solution records (legacy feature) |
| **Marketing User** | Marketing team access | Standard User + Campaign management capabilities |
| **Contract Manager** | Legal/contracts team | Standard User + Contract creation and management |

**Key constraints on standard profiles:**
- Cannot be deleted
- Cannot reduce permissions below a certain floor (some permissions are permanently enabled)
- Can be cloned to create custom profiles

### Custom Profiles — Best Practices

**How to create a custom profile:**
1. Setup → Profiles → click an existing profile → **Clone**
2. Name the clone descriptively: `Sales_Representative_Profile`, `Service_Agent_Profile`
3. Edit permissions specifically for your use case

**Profile naming conventions:**
- Use the role + function: `Sales_Rep_Profile`, `Support_Agent_EMEA_Profile`
- Never call it "Custom Profile 1" — unmanageable at scale
- Document what it's for in the Description field

**The Profile Sprawl Problem:**

As orgs grow, admins create profile after profile for every small variation. An org with 50 profiles is nearly impossible to maintain.

**Modern best practice:**
- Create minimal profiles — one per broad user category (Sales, Service, Marketing, Admin)
- Use **Permission Sets** for all variations and additions
- Profiles = minimum permissions; Permission Sets = additional permissions

### Real-World Profile Design

**Company:** "TechSales Corp" — 200 users

**Profile Design:**

| Profile | Users | Object Permissions |
|---------|-------|-------------------|
| `Salesforce_Admin` | 3 | Full access everything |
| `Sales_Rep_Base` | 120 | Leads R/C/E, Accounts R/C/E, Opportunities R/C/E, Cases R only |
| `Service_Agent_Base` | 50 | Cases R/C/E/D, Accounts R only, Contacts R only, Knowledge R |
| `Marketing_Base` | 15 | Leads R/C/E, Campaigns R/C/E, Accounts R only |
| `Finance_Read_Only` | 12 | Opportunities R only, Contracts R only, Orders R only |

**Permission Sets layered on top:**
- `Opportunity_Delete_PS` → assigned to 10 senior sales reps
- `Campaign_Management_PS` → assigned to 3 marketing managers
- `Report_Export_PS` → assigned to 8 data analysts
- `Knowledge_Author_PS` → assigned to 5 knowledge managers

---

### System Permissions — Key List

System Permissions control admin-level capabilities that cut across all objects:

| Permission | What It Allows | Grant To |
|-----------|---------------|---------|
| **View All Data** | Read ALL records regardless of sharing | Senior managers, executives |
| **Modify All Data** | Edit/delete ALL records regardless of sharing | Data admins, migration team |
| **Manage Users** | Create, edit, deactivate users | HR Admins, IT team |
| **Export Reports** | Export report data to CSV/Excel | Data teams, analysts |
| **Manage Reports in Public Folders** | Create/edit reports in shared folders | Report owners |
| **Customize Application** | Change page layouts, create fields, etc. | Junior admins |
| **Manage Profiles and Permission Sets** | Create/edit profiles and PSets | Senior admins only |
| **Manage Sharing** | Modify OWD and sharing rules | Senior admins only |
| **API Enabled** | Access Salesforce via REST/SOAP API | Integration users, devs |
| **Author Apex** | Write and deploy Apex code | Developers only |
| **Run Flows** | Execute flows (most users need this) | All standard users |
| **Manage Flow** | Create and edit flows | Admins and developers |

---

## 4.5 Layer 2 — Object Access: Permission Sets

### What is a Permission Set?

A **Permission Set** is an additive bundle of permissions that can be granted to users **on top of** their profile. Unlike profiles (one per user), a user can have **any number of permission sets**.

**Key principle:** Permission Sets can only ADD permissions — they cannot take away permissions that a profile grants.

### Profile vs Permission Set — When to Use Each

| Use Profile For | Use Permission Set For |
|----------------|----------------------|
| Baseline permissions that apply to EVERYONE in a role | Specific additional permissions needed by SOME users |
| Object CRUD that defines the role | Extra capabilities (delete, export, view all) |
| App and tab visibility | Access to specific features or modules |
| Login Hours and IP Ranges | Access to premium features (Einstein, CPQ) |
| The "floor" of permissions | Everything above the floor |

### Creating a Permission Set — Step by Step

**Path:** Setup → Permission Sets → New

**Configuration:**
1. **Label:** Descriptive name — `Senior_Sales_Delete_PS`, `Report_Export_PS`
2. **API Name:** Auto-generated from Label
3. **User License:** Must match the user's license type (or leave blank for all licenses)
4. **Description:** Document what this permission set grants and who should receive it

**After creation, configure:**
- **Object Settings** — CRUD permissions per object
- **Field Permissions** — field-level visibility per object
- **System Permissions** — system-level capabilities
- **App Permissions** — access to specific apps

### Real-World Permission Set Examples

**Example 1 — Senior Sales Rep:**
> Profile: `Sales_Rep_Base` (no delete on Opportunities)
> Permission Set: `Senior_Sales_PS`
> - Delete on Opportunities: ✅
> - View All Opportunities: ✅
> - Export Reports: ✅
>
> Assigned to: 8 Senior Account Executives out of 120 total reps

**Example 2 — Temporary Data Migration Access:**
> Profile: `Finance_Read_Only`
> Permission Set: `Q4_Data_Migration_PS`
> - Modify All on Accounts: ✅
> - Modify All on Contacts: ✅
> - Data Export: ✅
>
> Assigned to: 2 migration team members during Q4 data cleanup
> Removed after migration is complete

**Example 3 — Feature Rollout:**
> New feature: Einstein Lead Scoring
> Permission Set: `Einstein_Lead_Scoring_PS`
> - Einstein Lead Score field: Visible
> - Einstein Activity Capture: Access
>
> Rolled out initially to: 5 Sales Managers (pilot)
> Later expanded to: all sales reps after pilot success

### Permission Set Best Practices

1. **Name them for what they DO, not who they're for:** `Delete_Opportunities_PS` not `Senior_Rep_PS`
2. **One purpose per permission set:** Don't create mega-PSets that grant 50 different things
3. **Document each one:** Description field + a maintained spreadsheet
4. **Review regularly:** Remove PSets from users who no longer need them
5. **Don't duplicate profiles:** If the same set of users always gets the same PSet, it should probably be in the profile

---

## 4.6 Layer 2 — Object Access: Permission Set Groups

### What is a Permission Set Group?

A **Permission Set Group (PSG)** bundles multiple Permission Sets into a single assignable unit. Instead of assigning 5 individual permission sets to a user, you assign 1 group that includes all 5.

**Path:** Setup → Permission Set Groups → New

### How PSGs Work

```
PSG: "Claims_Adjuster_PSG"
   ├── Claims_Read_PS
   ├── Claims_Edit_PS
   ├── Policy_View_PS
   └── Reports_View_PS

Assign "Claims_Adjuster_PSG" to Priya
→ Priya automatically gets all 4 permission sets
```

### Muting Permission Sets Within a Group

A **Muting Permission Set** removes a specific permission from within a Permission Set Group — even if one of the group's member PSets would grant it.

**Use case:** A PSG grants a bundle of permissions, but for ONE specific group variant, you need to remove one specific permission.

**Example:**
> `Standard_Analyst_PSG` contains:
> - `Reports_Full_Access_PS` (includes Export Reports)
> - `Accounts_Read_PS`
> - `Dashboard_View_PS`
>
> But Junior Analysts should have everything EXCEPT Export Reports.
>
> Create a Muting Permission Set: `Remove_Export_Reports_Mute`
> Add it to the PSG configured for Junior Analysts
> → Junior Analysts get Reports + Accounts + Dashboards but NOT Export

### Real-World PSG Example

**Company:** "InsureCo" — 500 claims adjusters with complex permission needs

**Individual Permission Sets:**
- `Claims_Read_PS`
- `Claims_Edit_PS`
- `Claims_Delete_PS`
- `Policy_View_PS`
- `Policy_Edit_PS`
- `Customer_View_PS`
- `Reports_Claims_PS`
- `Reports_Finance_PS`

**Permission Set Groups:**

| PSG | Contains | Assigned To |
|-----|---------|------------|
| `Junior_Adjuster_PSG` | Claims_Read, Policy_View, Customer_View | 200 junior adjusters |
| `Senior_Adjuster_PSG` | Claims_Read, Claims_Edit, Policy_View, Policy_Edit, Customer_View, Reports_Claims | 250 senior adjusters |
| `Lead_Adjuster_PSG` | All PSets + Claims_Delete + Reports_Finance | 50 lead adjusters |

**Result:** Instead of maintaining individual PSet assignments for 500 people, IT manages 3 groups. When a junior adjuster is promoted to senior, one group change updates all permissions instantly.

---

## 4.7 Layer 3 — Field-Level Security (FLS)

### What is FLS?

**Field-Level Security (FLS)** controls whether users can **see** or **edit** specific fields on records they otherwise have access to. FLS is configured per field, per profile (and per permission set).

**The Three FLS States:**

| State | Configured How | Effect |
|-------|--------------|--------|
| **Visible (Read+Edit)** | Visible checkbox ✅ | User can see the field AND edit it |
| **Read-Only** | Visible ✅ + Read-Only ✅ | User can see the field but CANNOT edit it |
| **Hidden (not visible)** | Visible ❌ | Field is completely invisible — does not exist for this user |

### Where FLS Is Enforced

FLS applies in:
- Record detail pages and edit pages
- Reports and report builder field selectors
- List views column selectors
- Search results
- API queries (Apex, REST API) — unless using `SYSTEM` context
- Flow field references (in screen flows)

**FLS does NOT apply to:**
- Apex code running in `without sharing` context
- Automation running as system user
- Setup screens (admins always see all fields in Setup)

### FLS Configuration Paths

**Method 1 — By Profile:**
Setup → Profiles → [Profile] → scroll to Object Settings → [Object] → Field Permissions

**Method 2 — By Field:**
Setup → Object Manager → [Object] → Fields & Relationships → [Field] → Set Field-Level Security

**Method 3 — By Permission Set:**
Setup → Permission Sets → [PSset] → Object Settings → [Object] → Field Permissions

### FLS vs Page Layout — Critical Distinction

**This is one of the most commonly misunderstood concepts in Salesforce security.**

| Aspect | Page Layout | Field-Level Security |
|--------|------------|---------------------|
| Controls | Field placement and appearance on the EDIT/DETAIL page UI | Whether the field is visible/editable ANYWHERE (UI + API + reports) |
| Can hide a field? | Yes — removing from layout hides on page | Yes — Hidden FLS hides everywhere |
| Can make read-only? | Yes — set Read-Only on layout | Yes — Read-Only FLS setting |
| Applies in reports? | ❌ No | ✅ Yes |
| Applies in list views? | ❌ No | ✅ Yes |
| Applies in API? | ❌ No | ✅ Yes |
| Security enforcement | Weak (UI only) | Strong (everywhere) |

**Real-World Implication:**
> Admin removes `Annual_Salary__c` from the employee page layout.
> Users can't see it on the record page — WRONG! They can still:
> - Add it as a column in a list view
> - Include it in a report
> - See it via API
>
> To truly secure the field: set FLS = Hidden for the relevant profiles.

### Real-World FLS Examples

**Example 1 — Salary Data:**
> Field: `Annual_Salary__c` on `Employee__c`
>
> | Profile | FLS Setting | What They See |
> |---------|------------|--------------|
> | HR Manager | Visible (Edit) | Full salary, can change it |
> | HR Assistant | Read-Only | Can see salary, cannot change |
> | Department Manager | Read-Only | Can see their team's salaries |
> | Employee (self-service) | Hidden | Cannot see salary field at all |
> | Payroll Integration User | Visible (Edit) | Full access for payroll sync |

**Example 2 — Financial Terms on Opportunity:**
> Field: `Discount_Rate__c` on `Opportunity`
>
> | Profile | FLS | Rationale |
> |---------|-----|-----------|
> | Sales Rep | Read-Only | Can see what discount was offered; cannot change it |
> | Sales Manager | Visible (Edit) | Can authorize and change discounts |
> | Service Agent | Hidden | No commercial context needed |
> | Finance Read-Only | Read-Only | Sees for reporting; cannot change |

**Example 3 — Medical Data:**
> Field: `Diagnosis__c` on `Patient__c`
>
> | Profile | FLS | Rationale |
> |---------|-----|-----------|
> | Doctor | Visible (Edit) | Core part of their work |
> | Nurse | Read-Only | Needs to see diagnosis, can't change |
> | Receptionist | Hidden | No clinical access needed |
> | Billing Staff | Hidden | Only needs billing codes, not clinical diagnosis |

---

## 4.8 Layer 4 — Record Access: Org-Wide Defaults (OWD)

### What are OWD?

**Org-Wide Defaults (OWD)** set the **baseline level of access** that ALL users have to ALL records of a specific object — when no other sharing mechanism applies.

OWD is the **floor** of the building. Everything else (Role Hierarchy, Sharing Rules, Manual Sharing) can only RAISE the floor — never lower it per individual record.

**Configuration path:** Setup → Sharing Settings → Org-Wide Defaults → Edit

### OWD Settings — Complete Reference

| Setting | Who Can Read | Who Can Edit | When to Use |
|---------|-------------|-------------|-------------|
| **Private** | Record owner + role hierarchy above | Owner only (+ role hierarchy with edit access) | Sensitive records — sales pipeline, patient data |
| **Public Read Only** | All users in the org | Record owner + role hierarchy | Informational records everyone needs to see |
| **Public Read/Write** | All users | All users | Shared data no one needs to own — internal reference data |
| **Public Read/Write/Transfer** | All users | All users (can also change owner) | Leads and Cases — allows any user to reassign ownership |
| **Controlled by Parent** | Same as the Master record | Same as the Master record | Detail objects in Master-Detail relationships |
| **Public Full Access** | Community users | Community users | Experience Cloud — all community members access |

### OWD Per Object — Typical Configuration

| Object | Common OWD | Rationale |
|--------|-----------|-----------|
| Account | Private OR Public Read Only | Sales teams may need own accounts private; or everyone can see company info |
| Contact | Controlled by Parent OR Private | Follows Account sharing, or contacts are private to owner |
| Opportunity | **Private** | Revenue data is sensitive — reps see only their deals |
| Lead | Public Read/Write/Transfer | Any rep should be able to claim and work a lead |
| Case | Private OR Public Read/Write/Transfer | Cases often assigned to queues; need visibility for team support |
| Campaign | Public Read Only | Marketing campaigns are internal reference data |
| Contract | Private | Legal data — highly sensitive |

### The OWD Process: How to Set It Correctly

**Step 1: Identify your most sensitive object** — what's the worst case if the wrong person sees a record?

**Step 2: Set OWD to the MOST RESTRICTIVE option you need** — you can always open up access through Role Hierarchy and Sharing Rules, but you cannot restrict below OWD without changing it globally.

**Step 3: Configure Role Hierarchy to open up visibility upward** — managers see their team's records naturally.

**Step 4: Add Sharing Rules for cross-hierarchy access** — finance sees Closed Won deals, cross-sell team sees all accounts.

**Step 5: Use Manual Sharing for one-off exceptions.**

### Real-World OWD Design: 200-Rep Sales Org

> **Scenario:** "NationalSales" — 200 reps in 4 regions. Requirement: reps only see their own deals.
>
> **OWD for Opportunity:** `Private`
>
> **What this achieves:**
> - Rep A cannot see Rep B's Opportunities (even if they're in the same city)
> - Rep A can see ALL his own Opportunities
> - Rep A's manager can see Rep A's Opportunities (via Role Hierarchy)
> - The CEO can see all 200 reps' Opportunities (via Role Hierarchy)
> - The Finance team (not in sales hierarchy) cannot see any Opportunities by default
>
> **What still needs to be done:**
> - Create Sharing Rule: Finance team gets Read Only on Closed Won Opportunities
> - Create Sharing Rule: Cross-Sell team gets Read Only on all Accounts in all regions

---

## 4.9 Layer 4 — Record Access: Role Hierarchy

### What is the Role Hierarchy?

The **Role Hierarchy** is a tree structure that defines levels of authority in your org. Users in higher roles automatically see records owned by users in all roles below them — regardless of OWD.

**Key principle:** Role Hierarchy works UPWARD — higher roles see records owned by lower roles. Peers at the same level CANNOT see each other's records via Role Hierarchy alone.

**Configuration path:** Setup → Users → Roles

### Role vs Profile — Critical Distinction

| | Role | Profile |
|--|------|---------|
| Controls | **Record visibility** — which records the user can see | **Object access** — what types of records and what actions |
| User can have | One role | One profile |
| Purpose | Data visibility hierarchy | Permission hierarchy |
| Affects OWD? | Yes — opens up visibility upward | No |
| Affects Sharing Rules? | Yes — can target roles | No |

**Common confusion:** "My manager needs to see my records. Should I change her profile?" NO — change her Role to be above yours in the hierarchy.

### Building the Role Hierarchy

**Best practices:**
1. Mirror your actual reporting structure (not job titles — reporting relationships)
2. Roles should match the data visibility needs, not org charts
3. Keep it as flat as possible — fewer levels = easier to maintain
4. One role per meaningful data boundary (don't create a role for every job title)

### Real-World Role Hierarchy — Enterprise Sales

```
CEO
└── VP Sales
    ├── Regional Director - North
    │   ├── Area Manager - Delhi
    │   │   ├── Senior Sales Rep - Delhi
    │   │   └── Sales Rep - Delhi
    │   └── Area Manager - Mumbai
    │       ├── Senior Sales Rep - Mumbai
    │       └── Sales Rep - Mumbai
    ├── Regional Director - South
    │   ├── Area Manager - Chennai
    │   └── Area Manager - Bangalore
    └── Regional Director - West
        └── Area Manager - Ahmedabad
```

**OWD for Opportunity:** Private

**Visibility matrix:**

| User | Can See |
|------|---------|
| Sales Rep - Delhi | Only their own Opportunities |
| Area Manager - Delhi | All Delhi reps' Opportunities |
| Regional Director - North | All North region Opportunities (Delhi + Mumbai) |
| VP Sales | All regional Opportunities |
| CEO | Every Opportunity in the org |

### Role Hierarchy and "Grant Access Using Hierarchies"

For each object, there is a setting: **"Grant Access Using Hierarchies"**

- When **ENABLED** (default): Role Hierarchy opens access upward as described
- When **DISABLED**: Role Hierarchy does NOT grant access — each person only sees records they own (regardless of role). Use ONLY for objects where managers truly shouldn't see their team's records (unusual).

**Path to configure:** Setup → Sharing Settings → Org-Wide Defaults → find the object → Grant Access Using Hierarchies checkbox

### Real-World Role Hierarchy Scenarios

**Scenario A — Sales Rep Transfers Regions:**
> Sales Rep Arjun moves from North Region to South Region.
> Admin changes Arjun's Role from "Sales Rep - Delhi" to "Sales Rep - Chennai."
>
> **Immediate effect:**
> - North Regional Director can no longer see Arjun's records (he's no longer in her hierarchy)
> - South Regional Director can now see Arjun's records
> - Arjun's own records are unaffected — he still owns them
>
> This demonstrates that Role drives VISIBILITY, not ownership.

**Scenario B — Manager Promoted:**
> Area Manager - Delhi is promoted to Regional Director - North.
>
> Before promotion: Could see only Delhi records
> After promotion: Can see ALL North region records (Delhi + Mumbai)
>
> No records were transferred. Only the role assignment changed. Access changed instantly.

---

## 4.10 Layer 4 — Record Access: Sharing Rules

### What are Sharing Rules?

**Sharing Rules** extend record access automatically to groups of users — beyond what OWD and Role Hierarchy provide. They run continuously: whenever a new record is created or an existing record matches the criteria, the sharing rule applies automatically.

**Key principle:** Sharing Rules can only OPEN UP access (make it more permissive). They cannot restrict access below OWD.

**Configuration path:** Setup → Sharing Settings → scroll to the object section → Sharing Rules → New

### Two Types of Sharing Rules

#### Type 1: Owner-Based Sharing Rules

Share records based on who OWNS the record.

**Syntax:** "Share all records owned by [User/Role/Group] with [User/Role/Group] with [Access Level]"

**Use when:** You want to share all records owned by a specific team with another specific team.

**Real-World Examples:**

> **Example 1:** Cross-Sell Team needs to see ALL Accounts from ALL regions.
> - Share all Accounts owned by Role: Sales Rep (North) with Public Group: Cross-Sell Team → Read Only
> - Share all Accounts owned by Role: Sales Rep (South) with Public Group: Cross-Sell Team → Read Only
> - (Or: Share all Accounts owned by Role + Subordinates: VP Sales with Cross-Sell Team → covers everyone)

> **Example 2:** Support team needs visibility into customer Opportunities.
> - Share all Opportunities owned by Role + Subordinates: VP Sales with Role: Support Manager → Read Only

#### Type 2: Criteria-Based Sharing Rules

Share records based on a field VALUE on the record — regardless of who owns it.

**Syntax:** "Share all records where [Field] [Operator] [Value] with [User/Role/Group] with [Access Level]"

**Use when:** You want to share based on what kind of record it is, not who owns it.

**Real-World Examples:**

> **Example 1:** Finance team needs all Closed Won Opportunities for billing.
> - Share all Opportunities where `Stage = "Closed Won"` with Role: Finance Manager → Read Only

> **Example 2:** Legal team reviews high-value contracts.
> - Share all `Contract__c` where `Contract_Value__c > 5000000` with Public Group: Legal Team → Read Only

> **Example 3:** Compliance sees all records flagged for review.
> - Share all Accounts where `Compliance_Review_Required__c = TRUE` with Role: Compliance Officer → Read Only

> **Example 4:** Escalated cases visible to senior support team.
> - Share all Cases where `Priority = "High"` AND `Status = "Escalated"` with Role + Subordinates: Senior Support → Read/Write

### Access Level Options in Sharing Rules

| Access Level | What the Shared-To User Can Do |
|-------------|-------------------------------|
| **Read Only** | View the record; cannot edit or delete |
| **Read/Write** | View and edit the record; cannot delete |

*(Note: Sharing Rules cannot grant Delete access — only Read or Read/Write)*

### Sharing Rule Limits

- Up to **300 total sharing rules** per object (including owner-based and criteria-based combined)
- This limit is rarely hit in practice but important for highly complex orgs

---

## 4.11 Public Groups

### What are Public Groups?

A **Public Group** is a named collection of users (and/or roles, territories) that can be used as a target in Sharing Rules, Queue membership, Folder sharing, and Manual Sharing — instead of specifying individual users.

**Configuration path:** Setup → Users → Public Groups → New

### What Can Be Added to a Public Group

| Member Type | Description |
|-------------|-------------|
| **Users** | Individual Salesforce users |
| **Roles** | All users currently assigned to a specific role |
| **Roles + Subordinates** | A role AND all roles below it in the hierarchy |
| **Portal Roles** | Community user roles |
| **Groups** | Other Public Groups (nested groups) |
| **Territories** | Territory groups (if Territory Management enabled) |

### Real-World Public Group Examples

**Example 1 — Cross-Functional Team:**
> Group: `APAC_Deal_Review_Committee`
> Members:
> - User: Sarah Chen (CFO — not in sales hierarchy)
> - Role + Subordinates: VP Sales APAC
> - User: James Kumar (Legal Lead — not in sales hierarchy)
>
> This group includes everyone who should review large APAC deals, even though they come from different parts of the org.

**Example 2 — Regional Group:**
> Group: `North_Sales_Group`
> Members:
> - Role + Subordinates: Regional Director North
>
> All current and future users reporting under the North Regional Director are automatically included. New hires assigned to North roles join automatically.

**Example 3 — Admin and Ops:**
> Group: `Data_Management_Team`
> Members:
> - User: admin@company.com
> - User: dataops@company.com
> - User: integration@company.com
>
> Used in Sharing Rules for objects the data team needs full access to.

### Why Use Groups Instead of Individual Users?

- **Maintainability:** Add a new team member to the group → they automatically get all the sharing that the group provides
- **Scalability:** One sharing rule targets 50 users via a group, not 50 individual sharing rules
- **Clarity:** Group names explain WHY someone has access

---

## 4.12 Manual Sharing

### What is Manual Sharing?

**Manual Sharing** allows the record owner (or an admin) to share a specific record with a specific user or group on a case-by-case, ad hoc basis.

**How to access:** Open a record → click the **Sharing** button (in the action bar or dropdown) → Add Sharing

*(Note: The Sharing button only appears if the OWD for that object is NOT Public Read/Write — if it's already fully open, there's nothing to manually share)*

### Manual Sharing Access Levels

| Level | What the Shared-To User Can Do |
|-------|-------------------------------|
| **Read Only** | View only |
| **Read/Write** | View and edit |
| **Full Access (Owner)** | All rights including delete and re-sharing |

### Who Can Create Manual Shares?

- The **record owner** can share their own records
- A user with **Read/Write or higher** access to the record can share it further (but not above their own access level)
- **System Administrators** can manually share any record
- A user with **"Modify All"** object permission

### Real-World Manual Sharing Examples

**Example 1 — Cross-Department Collaboration:**
> Sales rep James owns Opportunity "Acme Enterprise Deal."
> Solutions Engineer Lisa needs access to review technical requirements.
> Lisa is not in the sales hierarchy — Sharing Rules don't cover her.
>
> James opens the Opportunity → Sharing → adds Lisa with Read/Write access.
> Lisa can now see and edit the technical notes section.
> James removes the share after the evaluation is complete.

**Example 2 — Temporary Audit Access:**
> Internal audit team needs to review 5 specific sensitive Contracts.
> OWD for Contract is Private. Auditor role is not in the contract management hierarchy.
>
> Admin manually shares each of the 5 Contract records with the Auditor Public Group → Read Only.
> After the audit, admin revokes the manual shares.

**Example 3 — Executive Sponsor Visibility:**
> A key customer deal involves the CEO as an executive sponsor.
> The CEO's role is above the rep's hierarchy — they'd normally see the record.
> BUT the OWD and this specific record's setup doesn't give the CEO access.
>
> Admin manually shares the Opportunity with the CEO → Read/Write so they can add notes from customer calls.

### Manual Sharing Limitations

- **Does not scale:** One share per record; for 1,000 records, you'd need 1,000 manual shares
- **Not automatically revoked:** If circumstances change (rep leaves, project ends), shares must be manually removed
- **Cannot grant delete:** Maximum is Read/Write via manual sharing
- **Cannot override FLS:** Even with Read/Write manual share, hidden fields remain hidden

---

## 4.13 Restriction Rules

### What are Restriction Rules?

**Restriction Rules** (introduced in 2021) further LIMIT access to records — even when sharing would otherwise grant access. They are the opposite of Sharing Rules.

- Sharing Rules: "These users should ALSO be able to see these records" (open access)
- Restriction Rules: "These users should ONLY be able to see records that meet THIS criteria" (close access)

**Configuration path:** Setup → Security → Restriction Rules → New

### Use Cases for Restriction Rules

| Scenario | Without Restriction Rules | With Restriction Rules |
|---------|--------------------------|----------------------|
| Doctors see all patient records (OWD Public R/W) | Psychiatry records visible to all doctors | Only psychiatrists see psychiatry records |
| Sales reps see all Accounts (OWD Public R/W) | Strategic accounts visible to all reps | Only Strategic Account Managers see strategic accounts |
| All agents see all Cases (Public R/W) | Confidential VIP cases visible to all | Only VIP team sees cases marked as VIP |

### How Restriction Rules Work

A Restriction Rule says: "For users with [Permission Set], ONLY show records where [Field] [Operator] [Value]"

**Example:**
> Permission Set: `Psychiatrist_PS`
> Object: `Patient__c`
> Filter: `Specialty__c = "Psychiatry"`
>
> Users with `Psychiatrist_PS` can ONLY see Patient records where Specialty = Psychiatry.
> General doctors (without this PSset) are NOT affected by this Restriction Rule — they still see all patients based on the normal sharing model.

**⚠️ Important:** Restriction Rules restrict access for users WITH the specified Permission Set — they don't apply to users WITHOUT it.

### Real-World Example — Detailed

> **Scenario:** "LegalFirm" has all attorneys using Salesforce to manage cases. OWD for `Matter__c` is Public Read/Write (all attorneys can see all matters for collaboration).
>
> **New requirement:** Partner-level attorneys must ONLY see matters they personally own or are assigned to (to prevent junior attorneys from seeing Partner-level billing rates and sensitive negotiations).
>
> **Without Restriction Rules:** No way to achieve this with Public R/W OWD without changing the entire sharing model.
>
> **With Restriction Rules:**
> 1. Create Permission Set: `Partner_Attorney_PS`
> 2. Create Restriction Rule on `Matter__c`:
>    - Applies to: Users with `Partner_Attorney_PS`
>    - Show only records where: `Owner = Current User` OR `Assigned_Attorney__c = Current User`
> 3. Assign `Partner_Attorney_PS` to all Partner-level attorneys
>
> Now Partners only see their own matters. Junior attorneys (without the PSset) are unaffected — they still see all matters per the Public R/W OWD.

---

## 4.14 Scoping Rules

### What are Scoping Rules?

**Scoping Rules** (newer feature) change the DEFAULT set of records shown in list views, searches, and lookups for users with a specific Permission Set — without blocking access to other records.

**Key difference from Restriction Rules:**
- Restriction Rules: BLOCK access to non-matching records
- Scoping Rules: FILTER the DEFAULT view, but users can still search for and find non-scoped records

### Use Case

> A company has 500,000 Account records. Sales reps only work with their 50–100 assigned accounts. Global search returns too many irrelevant results.
>
> Scoping Rule: For users with `Sales_Rep_PS`, default list views and searches show only Accounts where `Owner = Current User` OR `Account Team member includes Current User`.
>
> The rep's default view is focused. They can still search for other accounts if needed.

---

## 4.15 Setup Audit Trail

### What is the Setup Audit Trail?

The **Setup Audit Trail** logs every configuration change made in Salesforce Setup — who made the change, when, and what specifically was changed.

**Path:** Setup → Security → View Setup Audit Trail

### What Is Logged

| Category | Examples of Changes Logged |
|---------|--------------------------|
| User management | Created user, deactivated user, changed profile, reset password |
| Security settings | Changed OWD, created sharing rule, changed password policy |
| Object customization | Created field, changed page layout, added validation rule |
| Automation | Activated/deactivated flow, created/modified validation rule |
| Profile/Permission changes | Changed CRUD on profile, created permission set |
| Org settings | Changed company information, fiscal year, currency |
| Login events | Admin logged in as another user |
| Data management | Data export performed, data import run |

### Audit Trail Details

Each entry shows:
- **Date** — exact date and time
- **User** — who made the change (their username)
- **Section** — what area of Setup was changed
- **Action** — what specifically was done
- **Delegate User** — if a delegated admin made the change

### How Long Is It Retained?

- **Standard:** Last **20 changes** shown on screen (real-time)
- **Download:** Last **180 days** available for download as CSV
- **Salesforce Shield:** Full audit trail retained forever (premium)

### Real-World Use Cases

**Example 1 — Security Incident:**
> Users report they can suddenly see records they shouldn't be able to see. The admin checks Setup Audit Trail and discovers: yesterday at 11 PM, the OWD for Opportunity was changed from Private to Public Read/Write by `newadmin@company.com`. The admin reverses the change and investigates.

**Example 2 — Compliance Audit:**
> An ISO 27001 auditor asks: "Can you show me all changes to security settings in the last 6 months?"
> Admin downloads the 180-day Setup Audit Trail CSV, filters on "Sharing" and "Security" sections, and provides the evidence.

**Example 3 — Debugging:**
> A validation rule stopped working. The admin checks Audit Trail and sees: it was deactivated 3 days ago by a developer doing testing and was never reactivated.

**Example 4 — Compliance Tracking for "Login As":**
> An auditor asks: "Did any admin log in as a user in Q1 2026?"
> Setup Audit Trail shows every "Login" action taken — date, time, admin user, target user.

---

## 4.16 Salesforce Shield — Overview

**Salesforce Shield** is a premium add-on suite that enhances the standard security model with three major capabilities:

### Shield Platform Encryption

Encrypts data at REST within Salesforce's database — standard AES-256 encryption for specific fields.

**Why it matters:** Standard Salesforce data is protected in transit (HTTPS) but standard storage is not field-level encrypted. With Shield Encryption, specific fields (like SSN, medical records, salary) can be encrypted at rest.

**Real-World Use:**
> Healthcare company must comply with HIPAA. They enable Shield Encryption on `Diagnosis__c`, `SSN__c`, and `Medical_Record_Number__c` fields. Even if someone gained access to Salesforce's raw database, these fields are unreadable.

### Event Monitoring

Captures detailed logs of every user action in Salesforce — much more granular than Setup Audit Trail.

**Events tracked:**
- Every login (IP, browser, device)
- Every record view (who opened which record)
- Every report run (who ran which report, what filters)
- Every data export
- Every API call
- Every failed login attempt

**Real-World Use:**
> A company suspects a departing employee is viewing and memorizing competitor contact information. Event Monitoring shows the employee ran a "All Accounts - All Industries" report 15 times in 2 days. Evidence for the legal team.

### Field Audit Trail

Extends standard Field History Tracking retention from 18 months to **forever**.

**Standard:** History tracked for 18 months, then deleted
**Shield Field Audit Trail:** History retained indefinitely, queryable via API

**Real-World Use:**
> Financial regulator requires 7 years of audit history for all changes to `Contract_Amount__c` fields. Standard 18-month retention is insufficient. Shield Field Audit Trail maintains complete history.

---

## 4.17 How All Layers Work Together

### The Complete Security Decision Tree

When a user tries to access a record, Salesforce evaluates in this order:

```
1. Can this user log in right now?
   (Login Hours + IP Ranges + MFA)
                  ↓ YES
2. Does this user's profile (+ Permission Sets) 
   grant READ access to this object?
                  ↓ YES
3. Does FLS show this field to this user?
   (Per field per profile/PSset)
                  ↓ YES (for each field individually)
4. Does the record's sharing give this user access?
   (OWD + Role Hierarchy + Sharing Rules + Manual Share)
                  ↓ YES
5. Does a Restriction Rule REMOVE this user's access?
                  ↓ NO (not restricted)
   → USER CAN ACCESS THE RECORD (and the allowed fields)
```

### A Full Security Audit Example

> **Company:** "MedCare" — a healthcare CRM
> **User:** Rajesh (Billing Specialist)
> **Action:** Tries to view Patient #PAT-00245's `Diagnosis__c` field
>
> **Layer 1 — Org Access:**
> It's 11 AM on a Tuesday. Rajesh's profile has Login Hours 9AM–6PM Mon–Fri. ✅ PASS
>
> **Layer 2 — Object Access:**
> Rajesh's profile has Read access on `Patient__c` object. ✅ PASS
>
> **Layer 3 — FLS:**
> `Diagnosis__c` field FLS for Billing Specialist profile = **Hidden**. ❌ BLOCKED
> Rajesh cannot see the Diagnosis field — at all, in any view.
>
> (Even if FLS had passed:)
> **Layer 4 — Record Access:**
> OWD for Patient is Private. Rajesh doesn't own PAT-00245. He's not in the role hierarchy above the patient's doctor. No sharing rule covers him for this record. ❌ BLOCKED
>
> **Result:** Rajesh cannot see PAT-00245's diagnosis. Two independent security layers protect this data.

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Build the Complete Profile Structure

**Objective:** Create profiles that represent real business roles.

**Business Scenario (continuing the Job Portal from Topic 03):**

**Create Profile A — Recruiter:**
1. Setup → Profiles → Standard User → **Clone**
2. Name: `Recruiter_Profile`
3. Edit → Object Settings:
   - `Job_Posting__c`: Read ✅, Create ✅, Edit ✅, Delete ❌
   - `Job_Application__c`: Read ✅, Create ✅, Edit ✅, Delete ✅
4. System Permissions:
   - Export Reports: ❌
   - Run Reports: ✅

**Create Profile B — Hiring Manager:**
5. Clone Standard User again → Name: `Hiring_Manager_Profile`
6. Object Settings:
   - `Job_Posting__c`: Read ✅, Create ❌, Edit ❌, Delete ❌ (view-only)
   - `Job_Application__c`: Read ✅, Create ❌, Edit ✅, Delete ❌ (can update status)
7. System Permissions:
   - Export Reports: ✅
   - Run Reports: ✅

**Assign profiles:**
8. Assign Practice User (from Topic 02) → `Recruiter_Profile`
9. Create a new user → assign `Hiring_Manager_Profile`

---

### 🔧 Task 2 — Understand Profile Object Permissions by Comparison

**Objective:** See exactly what each profile can and cannot do.

**Steps:**
1. Log in as the Recruiter (Practice User)
2. Navigate to Job Postings → try to Delete a posting → is the Delete button visible?
3. Navigate to Job Applications → try to Delete an application → visible?
4. Note what you CAN and CANNOT do

5. Return to admin → Log in as Hiring Manager
6. Navigate to Job Postings → try to Create a new posting → is New button visible?
7. Navigate to Job Applications → try to Delete → visible?
8. Note what you CAN and CANNOT do

**Build a comparison table:**
| Action | Recruiter | Hiring Manager |
|--------|-----------|----------------|
| Create Job Posting | ❌/✅ | ❌/✅ |
| Edit Job Posting | ❌/✅ | ❌/✅ |
| Delete Job Posting | ❌/✅ | ❌/✅ |
| Delete Job Application | ❌/✅ | ❌/✅ |
| Export Reports | ❌/✅ | ❌/✅ |

---

### 🔧 Task 3 — Create and Layer Permission Sets

**Objective:** Add targeted extra permissions without changing profiles.

**Scenario:** A senior recruiter needs ability to delete Job Postings AND export reports.

**Steps:**
1. Setup → Permission Sets → **New**
2. Label: `Senior_Recruiter_PS`
3. License: Salesforce
4. Description: `Additional permissions for Senior Recruiters — delete Job Postings and export reports`
5. Save

6. Object Settings → `Job_Posting__c` → Edit → Enable **Delete** ✅ → Save
7. System Permissions → Edit → Enable **Export Reports** ✅ → Save

8. Manage Assignments → Add Assignment → find Practice User (Recruiter) → Assign

**Test:**
9. Log in as Practice User → try to delete a Job Posting → should now work ✅
10. Try to export a report → should now work ✅

**Reflection:** What would have been the alternative approach without Permission Sets? Why is that worse?

---

### 🔧 Task 4 — Configure Field-Level Security Comprehensively

**Objective:** Control salary visibility across profiles.

**Business Rule:**
- Recruiters: can see AND edit Salary
- Hiring Managers: can see salary (read-only) — helps them evaluate budgets
- All other profiles: salary is COMPLETELY hidden

**Steps:**
1. Setup → Object Manager → `Job_Posting__c` → Fields & Relationships → `Salary_Annual__c`
2. Click **Set Field-Level Security**
3. Configure:
   - `Recruiter_Profile`: Visible ✅, Read-Only ❌ (editable)
   - `Hiring_Manager_Profile`: Visible ✅, Read-Only ✅
   - `Standard User`: Visible ❌
   - `System Administrator`: Visible ✅, Read-Only ❌
4. Save

**Test:**
5. Log in as Recruiter → open Job Posting → Salary field visible and editable ✅
6. Log in as Hiring Manager → Salary visible but greyed out (read-only) ✅
7. Log in as a standard user → Salary field completely invisible ✅

**Now test the Page Layout vs FLS distinction:**
8. As Admin: Add Salary field back to the default page layout (if it was removed)
9. Log in as the standard user → is the Salary field visible now that it's on the layout?
10. Answer: No — FLS Hidden overrides page layout. FLS is the true security control.

---

### 🔧 Task 5 — Configure OWD and Observe the Effect

**Objective:** See exactly how OWD creates the access floor.

**Setup:**
1. Create User A: `recruiter.a@yourplayground.trailhead` — assign `Recruiter_Profile`
2. Create User B: `recruiter.b@yourplayground.trailhead` — assign `Recruiter_Profile`
3. Do NOT assign roles yet

**Set OWD:**
4. Setup → Sharing Settings → Edit
5. Set `Job_Posting__c` OWD to **Private**
6. Save

**Test — Private OWD:**
7. Log in as User A → Create 3 Job Postings
8. Log out → Log in as User B
9. Can User B see User A's Job Postings? (Should be NO — Private OWD)
10. Can User B see User B's own postings? (YES)

**Change OWD:**
11. Return to admin → Sharing Settings → Change `Job_Posting__c` to **Public Read Only**
12. Log in as User B → Can they now see User A's postings? (YES — Public Read Only)
13. Can User B EDIT User A's posting? (NO — Read Only)

**Return to Private for next tasks:**
14. Change OWD back to Private

---

### 🔧 Task 6 — Build the Role Hierarchy and Verify Upward Visibility

**Objective:** Understand exactly how role hierarchy opens up visibility.

**Steps:**
1. Setup → Users → Roles → **Set Up Roles**
2. Create this hierarchy:
   ```
   Recruitment Director
        └── Senior Recruiter
             ├── Recruiter North
             └── Recruiter South
   ```

3. Assign:
   - Admin user → `Recruitment Director`
   - User A (from Task 5) → `Recruiter North`
   - User B (from Task 5) → `Recruiter South`

4. Create User C: `senior.recruiter@yourplayground.trailhead` → Recruiter profile → `Senior Recruiter` role

**Test — Role Hierarchy:**
5. Log in as User A (Recruiter North) → Create 2 Job Postings
6. Log in as User B (Recruiter South) → Can they see User A's postings? (NO — same level, OWD Private)
7. Log in as User C (Senior Recruiter) → Can they see User A's postings? (YES — higher role)
8. Log in as Admin (Recruitment Director) → Can they see all postings? (YES — top of hierarchy)

**Key learning:** Peers cannot see each other. Superiors see everyone below them.

---

### 🔧 Task 7 — Create Sharing Rules for Cross-Hierarchy Access

**Objective:** Share records between teams that are not in the same hierarchy.

**Business Scenario:** A "Compliance Team" needs read-only access to all Job Postings for audit purposes, but they are NOT in the recruitment hierarchy.

**Steps:**
1. Create User D: `compliance@yourplayground.trailhead` → Standard User profile → No role
2. Create a Public Group: Setup → Users → Public Groups → New
   - Name: `Compliance_Team_Group`
   - Add User D as a member

3. Setup → Sharing Settings → Job Posting Sharing Rules → **New**
4. Rule Name: `Compliance_Audit_Access`
5. Rule Type: **Based on criteria** (simpler for this test)
   - Criteria: `Job_Posting__c: Record Owner Role ≠ null` (effectively all records)
   - OR use Owner-Based: Share records owned by Roles + Subordinates: Recruitment Director
6. Share with: Public Group → `Compliance_Team_Group`
7. Access Level: **Read Only**
8. Save

**Test:**
9. Log in as User D (Compliance)
10. Can User D see Job Postings owned by User A? (YES — Sharing Rule applies)
11. Can User D edit User A's Job Posting? (NO — Read Only)

---

### 🔧 Task 8 — Create a Permission Set Group with Muting

**Objective:** Build a PSG and use muting to remove a specific permission.

**Steps:**
1. Create two permission sets:
   - `JP_Full_Access_PS`: Read + Create + Edit + Delete on Job Postings
   - `JA_Full_Access_PS`: Read + Create + Edit + Delete on Job Applications
   - `Report_Export_PS`: Export Reports system permission

2. Setup → Permission Set Groups → New
   - Name: `Full_Recruitment_PSG`
   - Add all 3 PSets above

3. Create a Muting Permission Set:
   - Setup → Permission Sets → New
   - Label: `No_Export_Mute_PS`
   - System Permissions → Edit → disable Export Reports → Save
   - This is the muting PSset

4. In `Full_Recruitment_PSG` → click **Muting Permission Sets** tab → Add `No_Export_Mute_PS`

5. Assign `Full_Recruitment_PSG` to User A

**Test:**
6. Log in as User A → try to export a report → blocked (muted) ✅
7. Can User A delete Job Applications? YES (from JA_Full_Access_PS) ✅
8. Can User A delete Job Postings? YES (from JP_Full_Access_PS) ✅

---

### 🔧 Task 9 — View and Interpret the Setup Audit Trail

**Objective:** Use the Audit Trail as an investigative tool.

**Steps:**
1. Make several deliberate configuration changes:
   - Change OWD for Job Posting back to Public Read Only
   - Deactivate a Sharing Rule
   - Change a Validation Rule to inactive
   - Change a user's profile

2. Setup → Security → **View Setup Audit Trail**
3. Review the entries — for each one identify:
   - Who made the change
   - What was changed
   - When was it changed
   - Which section it falls under

4. Click **Download** to get the 180-day CSV
5. Open in Excel → filter by Section = "Sharing" → see all sharing changes
6. Filter by Section = "Users" → see all user management changes

**Scenario exercise:**
7. Undo one of your changes (return to original setting)
8. Go back to Audit Trail → confirm BOTH the change AND the undo appear in the trail
9. **Key insight:** Every action leaves a permanent record — Salesforce never forgets

---

## 📝 Practice Questions

**Q1.** A user's profile grants Read access to Opportunities. A Sharing Rule also grants them Read access to specific Opportunities. A manual share grants them Read/Write on one specific Opportunity. What is their effective access to that specific Opportunity?

**Q2.** OWD for Account is set to "Public Read Only." A sales rep wants to edit a colleague's Account. Can they? What would need to change to allow this?

**Q3.** What is the difference between setting a field as "Read-Only" on a Page Layout versus setting it as "Read-Only" in Field-Level Security?

**Q4.** A company has 200 users. They add 5 new users to a Public Group. How many Sharing Rules need to be updated to give the new users the same access the group already has?

**Q5.** What is a Muting Permission Set and when would you use it?

**Q6.** OWD for Case is Private. A support rep receives a call from a customer about Case #00001234. The rep searches for the case but cannot find it. List THREE possible reasons why.

**Q7.** What is the "Grant Access Using Hierarchies" setting on an object, and what happens if you disable it?

**Q8.** A user has "View All" object permission on Accounts granted via a Permission Set. Does OWD still apply to this user for Account records?

**Q9.** What is the difference between a Restriction Rule and an OWD setting in terms of what they control?

**Q10.** Setup Audit Trail shows someone changed the OWD for Opportunity from Private to Public Read/Write last night. What is the maximum period the trail covers for download, and what information does each entry contain?

**Q11.** Can a Sharing Rule grant Delete access to records?

**Q12.** A sales rep manually shares a Record with Read/Write access to a colleague. The colleague tries to Delete the record. Can they?

**Q13.** A user's profile has no access to Leads (no CRUD). A Permission Set they have grants Read on Leads. What is their effective access to Leads?

**Q14.** What are the three types of events that Salesforce Shield's Event Monitoring tracks that standard Salesforce does NOT?

**Q15.** A Sharing Rule is owner-based: "Share all Accounts owned by Role: Sales Rep with Public Group: Finance Team — Read Only." A user is added to the Sales Rep role. Do Finance Team members automatically see this new user's accounts?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Full Security Architecture Design**

> "PharmaTrack" is designing Salesforce for their 300-person pharmaceutical sales organization:
>
> **User Groups:**
> - 200 Sales Reps (in 4 regions: North, South, East, West)
> - 20 Regional Managers (5 per region)
> - 4 Regional VPs (1 per region)
> - 1 National Sales Director
> - 10 Finance Team Members (not in sales hierarchy)
> - 5 Legal Team Members (not in sales hierarchy)
> - 3 Salesforce Admins
>
> **Security Requirements:**
> 1. Reps only see their OWN Opportunities
> 2. Regional Managers see ALL Opportunities in their region
> 3. Regional VPs see all Opportunities across their region (all managers + reps below)
> 4. National Sales Director sees EVERYTHING
> 5. Finance Team: Read-Only on all Closed Won Opportunities (for billing)
> 6. Legal Team: Read-Only on Opportunities where `Amount > 5,000,000` (large deals)
> 7. `Discount_Rate__c` field: Reps = Read-Only, Managers+ = Edit
> 8. `Internal_Notes__c` field: Legal Team cannot see it at all
> 9. Reps cannot Delete Opportunities; Managers can
> 10. Reps cannot Export Reports; Managers and above can
>
> **Questions:**
> 1. Design the OWD for Opportunity and explain why
> 2. Design the Role Hierarchy (draw as text tree)
> 3. What Sharing Rules are needed, and of what type (owner-based or criteria-based)?
> 4. What Public Groups are needed?
> 5. How many profiles do you create? Describe each.
> 6. How many Permission Sets do you need? Describe each and who gets them.
> 7. For requirements 7 and 8, which specific security mechanism addresses each?

---

**Scenario 2 — Security Troubleshooting**

> Users at "RetailMax" are reporting the following issues. For each, identify which security layer is likely the cause and the specific configuration change needed to fix it:
>
> 1. Sarah (Sales Rep) can see her Opportunities but cannot see the `Discount_Applied__c` field even though it's on the page layout
> 2. James (Finance Manager, NOT in sales role hierarchy) needs to see all Closed Won Opportunities for quarterly reporting but can only see his own (he doesn't own any Opportunities)
> 3. Priya (Sales Rep) accidentally deleted an Account record. How do you prevent Sales Reps from deleting Accounts going forward?
> 4. Ravi (Support Agent) can see Case records but cannot see the `Internal_Priority_Score__c` field that appears in the list view
> 5. The entire Sales team can suddenly see ALL Opportunities from all regions — they previously could only see their own

---

**Scenario 3 — Permission Set Group Strategy**

> "InsureCo" has these user types in their claims department:
>
> | Role | Users | Needs |
> |------|-------|-------|
> | Claims Reviewer | 100 | Read Claims, Read Policies |
> | Claims Adjuster | 80 | Read+Edit Claims, Read+Edit Policies, View Reports |
> | Senior Adjuster | 40 | All Adjuster permissions + Delete Claims + Export Reports |
> | Claims Manager | 20 | All Senior permissions + View All Claims + Manage Users |
>
> **Questions:**
> 1. Design the Permission Set architecture — how many PSets and what does each contain?
> 2. Design the Permission Set Groups — how many PSGs and what does each contain?
> 3. For the Senior Adjuster PSG, show how you would include Delete Claims and Export Reports without granting them to Adjusters
> 4. A Claims Adjuster is promoted to Senior Adjuster. What is the exact admin action to update their permissions?
> 5. 5 new Claims Reviewers are hired. What is the minimum number of admin actions to fully provision them?

---

**Scenario 4 — OWD and Sharing Design Challenge**

> "TechConsult" is a consulting firm. Their requirements:
>
> - All Accounts should be visible to all internal users (the company is small and collaborative)
> - Opportunities should only be visible to the consulting lead on the deal and their manager
> - Cases should be visible to the assigned agent, their team, and the client's account manager
> - A "Delivery" team needs Read access to all Opportunities where `Project_Started__c = TRUE`
> - The CEO should be able to see everything without being in every hierarchy
>
> **Questions:**
> 1. For each object, recommend an OWD setting and justify it
> 2. For the Delivery team's Opportunity access, which type of Sharing Rule is most appropriate and why?
> 3. How do you give the CEO visibility into all Opportunities without making them a "Sales Manager"?
> 4. For Cases — if the assigned agent and account manager are in different hierarchies, how do you ensure the account manager sees relevant cases?

---

**Scenario 5 — Security Compliance Audit**

> Your company is undergoing a SOC 2 Type II security audit. The auditor submits the following questions. Answer each with the exact Salesforce feature/path that provides the evidence:
>
> 1. "Show us that financial data (Opportunity Amount) is only visible to authorized personnel"
> 2. "Prove that terminated employees lose access within 24 hours of termination"
> 3. "Show us a complete log of all security configuration changes made in the last 90 days"
> 4. "Demonstrate that admin users are required to use Multi-Factor Authentication"
> 5. "Show us that individual users cannot access each other's sales pipeline data"
> 6. "Prove that an administrator logging in as another user for support purposes is recorded and auditable"
> 7. "Show us your password complexity and expiration policy"
> 8. "Demonstrate that sensitive HR fields (salary, performance ratings) are only accessible to HR personnel"

---

## ✔️ Answer Key — Practice Questions

1. **Read/Write** — Access is additive at Layer 4. The most permissive access any sharing mechanism grants is what the user gets. Profile = Read, Sharing Rule = Read, Manual Share = Read/Write → effective access = Read/Write.

2. **No, they cannot edit the colleague's Account** — OWD = Public Read Only means everyone can see all Accounts (Read), but only the record OWNER can edit. To allow the rep to edit a colleague's Account, you would need to either: change OWD to Public Read/Write (grants edit to everyone), or create a Sharing Rule granting Read/Write access to specific users, or manually share that specific record with Read/Write access.

3. **Page Layout Read-Only:** Only prevents editing on the standard record edit page UI — does not apply to list views, reports, API access, or any other context. **FLS Read-Only:** Applies everywhere — on the record page, in list views, in reports, in API queries. FLS is the true security enforcement; Page Layout is a UI convenience setting.

4. **Zero** — Sharing Rules target Public Groups. When you add new users to the group, they automatically inherit all sharing rules that target that group. No Sharing Rules need to be updated.

5. A **Muting Permission Set** is a special type of Permission Set used WITHIN a Permission Set Group to REMOVE a specific permission that another PSset in the group would otherwise grant. Used when you want a PSG to grant a bundle of permissions but need to exclude one specific permission for a particular user type — without creating an entirely separate PSG.

6. Three possible reasons: (1) **OWD:** The case is owned by another agent and OWD is Private — the rep can't see cases they don't own with no sharing. (2) **Profile/Object Access:** The rep's profile doesn't have Read access to the Case object at all. (3) **Sharing/Queue:** The case is in a Queue the rep is not a member of, and no Sharing Rule covers them.

7. **"Grant Access Using Hierarchies"** is a per-object setting in Sharing Settings. When ENABLED (default), users in higher roles automatically see records owned by users in lower roles. When **DISABLED**, Role Hierarchy provides NO access to records — each user can only see records they personally own. Disabling is rare and appropriate only for objects where managers should NOT see their team's records.

8. **No — OWD does NOT apply** to a user with "View All" object permission. "View All" bypasses record-level security entirely — the user can see ALL records of that object regardless of OWD, Role Hierarchy, or sharing settings. It's the equivalent of having full org-wide visibility for that specific object.

9. **OWD** sets the baseline access level for ALL records of an object for ALL users — it's a global setting. A **Restriction Rule** applies only to users with a SPECIFIC Permission Set, further narrowing their access below what OWD and sharing would normally give them. OWD applies universally; Restriction Rules apply selectively to specific user groups.

10. The Setup Audit Trail download covers the **last 180 days**. Each entry contains: exact date and time of the change, the Salesforce username who made the change, the section of Setup that was changed (e.g., "Sharing"), a description of the specific action taken, and (if applicable) the delegate user if a delegated admin made the change.

11. **No** — Sharing Rules can only grant **Read Only** or **Read/Write** access. Delete access cannot be granted through Sharing Rules. To grant delete access, it must be in the user's Profile or a Permission Set.

12. **No** — the colleague can only VIEW and EDIT the record (Read/Write). They cannot delete it. Manual Sharing can grant a maximum of Read/Write access. Delete requires the "Delete" object permission in the profile or a permission set, which applies to ALL records of that object — not just manually shared ones.

13. **Read access** — Permission Sets are additive. The profile has no access to Leads (0 access), but the Permission Set grants Read. The net result is Read access. Permission Sets add to what the profile provides — they do not need the profile to already have the permission.

14. Shield Event Monitoring tracks: (1) **Every record VIEW** (which specific records each user opened — standard Salesforce doesn't log this), (2) **Every report run** with what filters and data was seen (3) **Every API call** with the specific queries and data accessed. Standard Salesforce logs Setup changes (Audit Trail) but not granular user data access events.

15. **Yes — automatically.** Because the Sharing Rule is owner-based targeting the "Sales Rep" Role, any new user assigned to the Sales Rep role automatically has their records shared with the Finance Team group. The Sharing Rule continuously evaluates all records owned by users in that role — new records and existing ones.

---

## 🔗 Trailhead Resources

- [Security Basics](https://trailhead.salesforce.com/content/learn/modules/security_basics)
- [Data Security](https://trailhead.salesforce.com/content/learn/modules/data_security)
- [Protect Your Data in Salesforce](https://trailhead.salesforce.com/content/learn/projects/protect-your-data-in-salesforce)
- [Permission Set Groups](https://trailhead.salesforce.com/content/learn/modules/permission-set-groups)

---

*Previous: [03 · Data Modeling & Objects](./03_Data_Modeling_and_Objects.md) | Next: [05 · Customization & UI →](./05_Customization_and_UI.md)*
