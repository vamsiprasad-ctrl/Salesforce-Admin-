# 04 · Security & Access Control

> **Module:** Security | **Difficulty:** 🟡 Intermediate | **Est. Time:** 4 hrs

---

## 4.1 The Four Layers of Salesforce Security

```
┌──────────────────────────────────────────────────┐
│  LAYER 1 — ORG ACCESS                            │
│  Can the user log in at all?                     │
│  (Login Hours, IP Ranges, MFA)                   │
└─────────────────────┬────────────────────────────┘
                      │ ✅ passed
┌─────────────────────▼────────────────────────────┐
│  LAYER 2 — OBJECT ACCESS                         │
│  Can they see/edit this TYPE of record?          │
│  (Profiles, Permission Sets, PSGs)               │
└─────────────────────┬────────────────────────────┘
                      │ ✅ passed
┌─────────────────────▼────────────────────────────┐
│  LAYER 3 — FIELD ACCESS                          │
│  Can they see/edit this specific FIELD?          │
│  (Field-Level Security per profile/perm set)     │
└─────────────────────┬────────────────────────────┘
                      │ ✅ passed
┌─────────────────────▼────────────────────────────┐
│  LAYER 4 — RECORD ACCESS                         │
│  Can they see this specific RECORD?              │
│  (OWD → Role Hierarchy → Sharing Rules)          │
└──────────────────────────────────────────────────┘
```

> 🔑 **Golden Rule:** The most restrictive setting at each layer wins. Access at a lower layer does NOT override a block at a higher layer.

---

## 4.2 Layer 1 — Org Access

### Login Hours

Restrict when users with a specific profile can log in.

**Path:** Setup → Profiles → [Profile] → Login Hours

### Real-World Example

> **CallCenter Inc.** agents work 9AM–6PM Mon–Fri. Login Hours set to 8:30AM–6:30PM Mon–Fri. Agent trying to log in at 9PM gets: "You are not allowed to log in during this time period."

### Login IP Ranges

Restrict which IP addresses can be used to log in.

### Real-World Example

> **BankCo** mandates all Salesforce access from the corporate network only (IP: 203.0.113.0/24). Home access is blocked even with valid credentials.

### Multi-Factor Authentication (MFA)

Salesforce **requires** MFA for all users (enforced since February 2022). Admins must enable and enforce it.

---

## 4.3 Layer 2 — Profiles

A **Profile** is the foundational security record for every user — every user must have exactly **one profile**.

### What a Profile Controls

| Area | Examples |
|------|---------|
| Object Permissions | Read, Create, Edit, Delete per object |
| App Access | Which Lightning apps are visible |
| Tab Settings | Which object tabs are shown |
| Field-Level Security | Which fields are Visible, Read-Only, or Hidden |
| Record Type Access | Which record types the user can use |
| Login Hours | When users with this profile can log in |
| Login IP Ranges | From where they can log in |
| System Permissions | Export data, Manage users, View All Data |

### Standard Profiles

| Profile | Access Level |
|---------|-------------|
| System Administrator | Full access to everything |
| Standard User | Basic CRM — read/create/edit most objects |
| Read Only | Can view but not create or edit |
| Marketing User | Standard + Campaign management |
| Contract Manager | Manages contracts |

### Real-World Example

> **TechBridge** profiles:
>
> **Sales Rep:** Accounts (R/C/E), Leads (R/C/E/D), Opportunities (R/C/E) — No Delete on Accounts
> **Sales Manager:** All of above + Delete on Accounts and Opportunities, View All Reports
> **Finance User:** Accounts (R only), Contracts (R/C/E), No Leads access

---

## 4.4 Permission Sets

A **Permission Set** adds permissions on top of a profile. A user can have **many** permission sets.

### Real-World Example

> TechBridge has 50 Sales Reps on the standard Sales Rep profile. Only 5 need to:
> - Delete Opportunities
> - View All Activities org-wide
> - Export Reports
>
> ❌ **Bad approach:** Create a separate "Team Lead" profile — nearly identical, doubles maintenance.
>
> ✅ **Good approach:** Create `Sales_Team_Lead_PS` permission set with just those 3 permissions → assign to the 5 team leads.

---

## 4.5 Permission Set Groups

Bundles multiple Permission Sets into one assignable unit.

### Real-World Example

> **InsureCo** has individual permission sets:
> - `Claims_Read_PS`, `Claims_Edit_PS`, `Policy_Manage_PS`, `Reports_Export_PS`
>
> For Claims Adjusters (need first 3), they create:
> `Claims_Adjuster_PSG` = Claims_Read + Claims_Edit + Policy_Manage
>
> Assign one group → user gets all three. One change to the group updates all 200 adjusters instantly.

---

## 4.6 Field-Level Security (FLS)

Controls whether users can **see** (read) or **edit** specific fields.

| FLS Setting | What User Sees |
|-------------|---------------|
| Visible | Can see AND edit |
| Read-Only | Can see but NOT edit |
| Hidden (not checked) | Field is completely invisible |

### Real-World Example

> `Discount_Percentage__c` on Opportunity:
>
> | Profile | FLS | Effect |
> |---------|-----|--------|
> | Sales Rep | Read-Only | Sees it, can't change it |
> | Sales Manager | Visible (Edit) | Sees it, can change it |
> | Support Agent | Hidden | Field doesn't exist for them |

---

## 4.7 OWD — Org-Wide Defaults

Sets the **baseline** record visibility for every user. This is the floor — it can only be opened up, never narrowed per record.

**Path:** Setup → Sharing Settings

| OWD Setting | Meaning |
|------------|---------|
| **Private** | Only record owner (and role hierarchy above) can see |
| **Public Read Only** | All users can see, only owner can edit |
| **Public Read/Write** | All users can see and edit |
| **Controlled by Parent** | Detail objects follow the Master record's sharing |

### Real-World Example

> **SalesOrg** with 200 reps across 4 regions. Reps must only see their own Opportunities.
>
> OWD for Opportunity: **Private**
>
> Now every rep sees only their own. Managers see their team's via Role Hierarchy. Cross-region sharing is added via Sharing Rules.

---

## 4.8 Role Hierarchy

A tree structure where users in higher roles **automatically see** records owned by users in lower roles.

### Real-World Example

```
                     CEO
                      │
           ┌──────────┴──────────┐
      VP Sales East         VP Sales West
           │                     │
     ┌─────┴────┐          ┌─────┴────┐
  Mgr NY   Mgr MA       Mgr CA    Mgr TX
     │                     │
  Rep A  Rep B          Rep C  Rep D
```

**OWD for Opportunity: Private**

| User | Can See |
|------|---------|
| Rep A | Only his own Opportunities |
| Mgr NY | Rep A's + Rep B's |
| VP Sales East | All East Region Opportunities |
| CEO | All Opportunities org-wide |

---

## 4.9 Sharing Rules

Extend record access beyond OWD + Role Hierarchy to **groups** of users automatically.

| Type | Shares based on | Example |
|------|----------------|---------|
| **Ownership-based** | Who owns the record | Share East Region reps' Accounts with West Region |
| **Criteria-based** | A field value | Share all Closed Won Opps with the Finance team |

### Real-World Examples

> **Ownership-based:** Cross-Sell Team needs to see all Accounts owned by either East or West region reps. Sharing Rule grants Read Only to the Cross-Sell Public Group.
>
> **Criteria-based:** Finance needs to see `Stage = Closed Won` Opportunities for billing. Sharing Rule: `Stage = Closed Won` → Finance Role → Read Only.

---

## 4.10 Manual Sharing

Record owner shares an individual record with a specific user or group on an ad hoc basis.

**How:** Open record → **Sharing** button → Add user/group → Set access level

### Real-World Example

> Sales rep James owns an Opportunity. Engineer Lisa needs to review technical requirements. James manually shares the Opportunity with Lisa (Read Only) for the evaluation period. Removes access after the deal closes.

---

## 4.11 Restriction Rules

**Restricts** access further — even if sharing would otherwise open it.

### Real-World Example

> **HealthCo** — all doctors have Read/Write access to Patient records (OWD: Public R/W). But psychiatry records are confidential.
>
> **Restriction Rule:** Records where `Specialty__c = Psychiatry` are only visible to users with the `Psychiatrist_PS` permission set. Other doctors can no longer see these records despite the permissive OWD.

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Create a Custom Profile

**Objective:** Build a profile with specific object permissions.

**Business Scenario:** Create a "Sales Representative" profile with limited access.

**Steps:**
1. Setup → Profiles → find **Standard User** → click **Clone**
2. Name: `Sales Representative`
3. Save
4. Click into the new profile → **Object Settings**
5. Configure `Job_Posting__c` (from Topic 03):
   - Read ✅, Create ✅, Edit ✅, Delete ❌
6. Configure `Job_Application__c`:
   - Read ✅, Create ✅, Edit ❌, Delete ❌
7. Save

**Assign to user:** Setup → Users → find Practice User → Edit → Profile = `Sales Representative` → Save

---

### 🔧 Task 2 — Create and Assign a Permission Set

**Objective:** Grant additional permissions without changing the profile.

**Business Scenario:** Practice User is promoted to Senior Rep and now needs Delete access on Job Applications.

**Steps:**
1. Setup → Permission Sets → **New**
2. Label: `Senior Sales Rep`
3. License: Salesforce
4. Save
5. Object Settings → `Job_Application__c` → Edit
6. Enable: **Delete** ✅ → Save
7. Back on the Permission Set → **Manage Assignments** → **Add Assignment** → find Practice User → Assign

**Verify:** Log in as Practice User → open a Job Application → confirm the Delete button now appears.

---

### 🔧 Task 3 — Configure Field-Level Security

**Objective:** Control which fields are visible to which profiles.

**Business Scenario:** The `Salary_Range__c` field on Job Posting should be hidden from Sales Reps. Only HR users and Admins should see it.

**Steps:**
1. Setup → Object Manager → `Job_Posting__c` → Fields & Relationships
2. Click on `Salary_Range__c`
3. Click **Set Field-Level Security**
4. For profile `Sales Representative`:
   - Visible: ❌ (uncheck)
5. For profile `System Administrator`:
   - Visible: ✅
6. Save

**Verify:** Log in as Practice User (Sales Rep profile) → open a Job Posting → confirm Salary Range field is invisible. Log back in as admin → field is visible.

---

### 🔧 Task 4 — Configure OWD (Org-Wide Defaults)

**Objective:** Understand how OWD sets the baseline access floor.

**Steps:**
1. Setup → Sharing Settings
2. Observe the current OWD settings for standard objects (Account, Contact, Opportunity, Lead, Case)
3. Scroll down — find your custom object `Job_Posting__c`
4. Note its current OWD setting
5. Click **Edit** → set `Job_Posting__c` OWD to **Private** → Save

**Test:**
1. Create two users (User A and User B) — assign them both the Sales Rep profile
2. Log in as User A → create a Job Posting
3. Log out → log in as User B
4. Try to find User A's Job Posting → it should be **invisible** (Private OWD)

---

### 🔧 Task 5 — Set Up a Role Hierarchy

**Objective:** Understand how roles enable hierarchical visibility.

**Steps:**
1. Setup → Users → Roles → **Set Up Roles**
2. Create the following structure:
   ```
   CEO
    └── Sales Manager
         ├── Sales Rep East
         └── Sales Rep West
   ```
3. Assign:
   - Your admin user → CEO role
   - Practice User → Sales Rep East
4. Create User C → assign Sales Rep West role

**Test:**
1. Log in as Practice User (Sales Rep East) → create a Job Posting
2. Log in as User C (Sales Rep West) → try to see Practice User's Job Posting → **should be invisible** (OWD = Private, same role level)
3. Log in as admin (CEO) → can see **all** Job Postings (higher role)

---

### 🔧 Task 6 — Create a Sharing Rule

**Objective:** Automatically share records across role boundaries.

**Business Scenario:** The Recruitment Manager needs to see all Job Postings owned by Sales Reps (East and West).

**Steps:**
1. First create a Public Group: Setup → Users → Public Groups → New
   - Name: `Recruitment Team`
   - Add your admin user as a member
2. Setup → Sharing Settings → Scroll to `Job_Posting__c` Sharing Rules → **New**
3. Rule Type: **Based on Record Owner**
4. Records owned by: Roles — **Sales Rep East** and **Sales Rep West**
5. Share with: Public Group — `Recruitment Team`
6. Access Level: **Read Only**
7. Save

**Verify:** Log in as admin (who is in Recruitment Team group) → confirm you can see Job Postings owned by Sales Reps.

---

### 🔧 Task 7 — Create a Permission Set Group

**Objective:** Bundle permission sets for easier management.

**Steps:**
1. Create two permission sets:
   - `Job_Posting_Edit_PS` — Edit access on Job Postings
   - `Job_Application_View_PS` — Read access on Job Applications
2. Setup → Permission Set Groups → **New**
3. Label: `HR Coordinator`
4. Add both permission sets to the group
5. Assign the group to Practice User

**Verify:** Log in as Practice User → confirm they have both Edit on Job Postings AND Read on Job Applications.

---

## 📝 Practice Questions

**Q1.** A user has a profile that grants Read access to Accounts. A sharing rule grants them Edit access to specific Account records. What is their effective access to those records?

**Q2.** OWD for Opportunity is set to "Public Read Only." Can a rep edit another rep's Opportunity? Can a manager edit a subordinate's Opportunity?

**Q3.** What is the difference between a Sharing Rule and Manual Sharing?

**Q4.** A user's profile allows Read access to the Salary field. An admin removes Salary from the user's page layout. Can the user still see the Salary field?

**Q5.** What is the purpose of a Restriction Rule, and how does it differ from an OWD setting?

**Q6.** How many profiles can one user have at the same time? How many permission sets can one user have?

**Q7.** A company sets OWD for Cases to "Private." A support rep opens a case submitted by a customer they don't own. Can they see it?

**Q8.** What is the maximum level of access you can grant through Manual Sharing compared to your own access level?

**Q9.** If OWD is set to "Public Read/Write" for Accounts, does the Role Hierarchy affect record visibility for Accounts?

**Q10.** A Permission Set Group contains Permission Set A (grants Edit on Leads) and Permission Set B (grants Delete on Leads). A Muting Permission Set removes Delete on Leads. What is the user's effective access on Leads?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Layered Security Design**

> "PharmaTrack" is implementing Salesforce for their pharmaceutical sales organization. They have these requirements:
>
> - Sales Reps should only see their own Opportunities
> - Regional Managers should see all Opportunities for their region
> - The National Sales Director should see all Opportunities
> - The Finance team (not in the sales hierarchy) needs Read-Only access to all Closed Won Opportunities for billing
> - The `Discount_Rate__c` field should only be editable by Regional Managers and above
> - Sales Reps should not be able to delete any records
>
> **Question:** For each requirement, identify which specific Salesforce security mechanism addresses it (OWD, Role Hierarchy, Sharing Rule, FLS, Profile). Design the complete security model.

---

**Scenario 2 — Permission Sets vs Profiles**

> A company has 3 sales teams: SMB (30 reps), Mid-Market (20 reps), and Enterprise (10 reps). Here are their differences:
>
> - All teams: Full access to Leads, Contacts, Accounts
> - SMB only: Cannot delete Opportunities
> - Mid-Market: Can delete Opportunities, can access Campaign data
> - Enterprise: All Mid-Market permissions + can Export Reports + can View All Data
>
> **Question:**
> 1. How many profiles would you create?
> 2. How many permission sets would you create?
> 3. For each permission set, which users get it assigned?
> 4. Why is this approach better than creating 3 separate profiles?

---

**Scenario 3 — Data Visibility Investigation**

> A sales rep named Ravi calls the admin: "I submitted a large Opportunity worth ₹50 Lakhs yesterday but my manager says she can't see it in her pipeline report. She only sees my smaller deals."
>
> **Question:** Walk through each of the 4 security layers to diagnose where the visibility issue might exist. For each layer, what specific question would you check and how?

---

**Scenario 4 — Sharing Conflict**

> At "RetailMax":
> - OWD for Account = **Private**
> - There is a Sharing Rule: Share all Accounts where `Region__c = "North"` with the `North_Sales_Group` public group — Read Only
> - User Priya is in the North_Sales_Group
> - User Priya's Role Hierarchy also gives her access to Accounts owned by her subordinates
> - User Priya also received Manual Share on a specific Acme Corp Account — **Read/Write**
>
> **Questions:**
> 1. Priya encounters an Account in the North Region owned by a peer (not a subordinate). What access does she have and why?
> 2. For Acme Corp specifically, what access does Priya have and why?
> 3. If the admin removes Priya from North_Sales_Group, can she still see North Region accounts she doesn't own?
> 4. If the admin revokes the Manual Share on Acme Corp, can Priya still see it?

---

**Scenario 5 — Security Audit**

> Your company is preparing for an ISO 27001 security audit. The auditor asks: "Can you prove that no Sales Representative can see other representatives' Opportunity records, that the Salary field on employee records is only visible to HR, and that all configuration changes to security settings are logged?"
>
> **Question:** For each of the three audit requirements, explain exactly where in Salesforce you would demonstrate compliance and what screenshots/reports you would pull.

---

## ✔️ Answer Key — Practice Questions

1. **Edit** — Permission Sets and Sharing Rules are additive. If any mechanism grants a higher level of access, the user gets that higher level. Profile grants Read → Sharing Rule grants Edit → effective = Edit.
2. **Cannot edit** via Read Only OWD. A manager cannot edit a subordinate's Opportunity through Role Hierarchy alone — the Role Hierarchy only affects **visibility** (read access), not edit rights. To grant edit, the OWD must be Public Read/Write or a specific sharing mechanism must grant edit.
3. **Sharing Rule** = automatic, criteria-based, applies to a group of users. **Manual Sharing** = one-off, record-by-record, applied by the record owner or admin to a specific user/group.
4. **Yes** — Field-Level Security (FLS) and Page Layouts are independent. Removing a field from a Page Layout hides it on the standard record view. But if FLS is still set to Visible, the user can still see the field in reports, list views, and related lists. You must use FLS to truly hide a field.
5. A **Restriction Rule** further *limits* access even when sharing would normally grant it. An OWD setting is the *baseline* for all records. Restriction Rules narrow access below what OWD and sharing rules would provide; they go in the opposite direction of Sharing Rules.
6. **One profile** maximum per user. **Unlimited permission sets** — a user can have many permission sets assigned simultaneously.
7. **No** — with OWD set to Private, a support rep can only see Cases they own OR that are shared with them via Role Hierarchy, Sharing Rules, or Manual Sharing. They cannot see a random case they don't own.
8. You cannot grant access **above** what you yourself have. You can only share up to your own access level.
9. **No** — if OWD is Public Read/Write, Role Hierarchy has **no effect** on record access because everyone can already see and edit everything. Role Hierarchy only opens access when OWD is Private or Public Read Only.
10. **Edit on Leads only** — Permission Set A grants Edit, Permission Set B grants Delete, but the Muting Permission Set removes Delete from the group. Net result: Edit access remains, Delete is removed.

---

## 🔗 Trailhead Resources

- [Security Basics](https://trailhead.salesforce.com/content/learn/modules/security_basics)
- [Data Security](https://trailhead.salesforce.com/content/learn/modules/data_security)
- [Protect Your Data in Salesforce](https://trailhead.salesforce.com/content/learn/projects/protect-your-data-in-salesforce)
- [Permission Set Groups](https://trailhead.salesforce.com/content/learn/modules/permission-set-groups)

---

*Previous: [03 · Data Modeling](./03_Data_Modeling_and_Objects.md) | Next: [05 · Customization & UI →](./05_Customization_and_UI.md)*
