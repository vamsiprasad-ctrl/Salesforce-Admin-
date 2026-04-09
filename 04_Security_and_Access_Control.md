# 04 · Security & Access Control

> **Module:** Security | **Difficulty:** 🟡 Intermediate | **Est. Time:** 4 hrs

---

## 4.1 The Four Layers of Salesforce Security

Salesforce security works like a series of locked doors. A user must have a key for **every door** to access a piece of data. If they fail at any layer, access is denied — regardless of what other layers allow.

```
┌─────────────────────────────────────────────────┐
│  LAYER 1 — ORG ACCESS                           │
│  Can the user log into the org at all?          │
│  (Login hours, IP restrictions, MFA)            │
└────────────────────┬────────────────────────────┘
                     │ ✅ passed
┌────────────────────▼────────────────────────────┐
│  LAYER 2 — OBJECT ACCESS                        │
│  Can the user see/edit this type of record?     │
│  (Profiles, Permission Sets)                    │
└────────────────────┬────────────────────────────┘
                     │ ✅ passed
┌────────────────────▼────────────────────────────┐
│  LAYER 3 — FIELD ACCESS                         │
│  Can the user see/edit this specific field?     │
│  (Field-Level Security)                         │
└────────────────────┬────────────────────────────┘
                     │ ✅ passed
┌────────────────────▼────────────────────────────┐
│  LAYER 4 — RECORD ACCESS                        │
│  Can the user see this specific record?         │
│  (OWD → Role Hierarchy → Sharing Rules)         │
└─────────────────────────────────────────────────┘
```

> 🔑 **Golden Rule:** The most restrictive setting at each layer wins. Granting access at Layer 4 does not override a block at Layer 2.

---

## 4.2 Layer 1 — Org Access

### Login Hours

Restrict when users can log in — useful for compliance or shift-based work environments.

**Path:** Setup → Profiles → [Profile] → Login Hours

### Real-World Example

> **Company:** "CallCenter Inc." — agents work 9 AM to 6 PM, Monday to Friday.
>
> The admin sets Login Hours on the **Call Center Agent** profile to Mon–Fri, 8:30 AM – 6:30 PM.
>
> If an agent tries to log in at 9 PM from home, they get an error: "You are not allowed to log in during this time period." This prevents unauthorized off-hours access.

### Login IP Ranges

Restrict which IP addresses can be used to access the org.

**Path:** Setup → Profiles → [Profile] → Login IP Ranges

### Real-World Example

> **Company:** "BankCo" — for compliance, all Salesforce access must come from within the corporate network (IP: 203.0.113.0/24).
>
> Admin sets the Login IP Range on all profiles to the corporate IP block. If a user tries to log in from home on their personal internet connection, login is blocked.

### Multi-Factor Authentication (MFA)

Requires a second verification step (Salesforce Authenticator app, hardware token) in addition to username/password.

> 💡 Salesforce has **required MFA** for all users since February 2022. Admins must enforce it.

---

## 4.3 Layer 2 — Object Access: Profiles

A **Profile** is the foundational security record for every user. Every user must have exactly **one profile**.

### What a Profile Controls

| Area | Examples |
|------|---------|
| **Object Permissions** | Can the user Read, Create, Edit, Delete Accounts? |
| **App Access** | Which Lightning apps appear in the App Launcher? |
| **Tab Settings** | Which object tabs are visible? |
| **Field-Level Security** | Which fields on each object are Visible or Read-Only? |
| **Record Type Access** | Which record types can the user access? |
| **Login Hours** | When can users with this profile log in? |
| **Login IP Ranges** | From where can users with this profile log in? |
| **Apex / VF Access** | Which custom code can this user run? |
| **System Permissions** | Can they export data? Manage users? View all data? |

### Standard Profiles (cannot be deleted)

| Profile | Intended For |
|---------|-------------|
| System Administrator | Full access to everything — the most powerful profile |
| Standard User | Basic CRM access — read/create/edit most objects |
| Read Only | Can view records but not create or edit |
| Solution Manager | Manages knowledge solutions |
| Marketing User | Standard access + Campaigns |
| Contract Manager | Manages contracts |

### Real-World Example — Profile

> **Company:** "TechBridge" needs three types of users:
>
> **Sales Rep** profile:
> - Accounts: Read, Create, Edit ✅ | Delete ❌
> - Leads: Read, Create, Edit, Delete ✅
> - Opportunities: Read, Create, Edit ✅ | Delete ❌
> - Contracts: Read only ✅ | Create/Edit ❌
>
> **Sales Manager** profile:
> - All of the above PLUS Delete on Accounts and Opportunities
> - View All Reports ✅
>
> **Finance User** profile:
> - Accounts: Read only ✅
> - Opportunities: Read only ✅
> - Contracts: Read, Create, Edit ✅
> - Leads: No access ❌

---

## 4.4 Layer 2 — Object Access: Permission Sets

A **Permission Set** is an additive collection of settings and permissions that can be granted to users **on top of** their profile. One user can have many permission sets.

### Why Permission Sets exist

Profiles are blunt instruments — they apply the same permissions to everyone with that profile. Permission Sets let you be surgical — give one specific user one specific additional permission without creating a separate profile.

### Real-World Example — Permission Set

> **Scenario:** TechBridge has 50 Sales Reps on the standard Sales Rep profile. Only 5 of them are "Team Leads" who also need to:
> - Delete Opportunity records
> - View all Activities org-wide
> - Export Reports
>
> **Bad approach:** Create a "Sales Team Lead" profile — now you maintain two nearly identical profiles.
>
> **Good approach:** Create a Permission Set called `Sales_Team_Lead_PS` with just those 3 extra permissions → assign it only to the 5 team leads.
>
> When team lead responsibilities change, you update one Permission Set — not 5 individual profiles.

### Creating a Permission Set

**Path:** Setup → Permission Sets → New

| Field | Value |
|-------|-------|
| Label | Sales Team Lead |
| API Name | Sales_Team_Lead |
| License | Salesforce (must match user's license) |
| Description | Additional permissions for senior reps |

---

## 4.5 Permission Set Groups

A **Permission Set Group** bundles multiple Permission Sets into a single assignable unit.

### Real-World Example

> **Company:** "InsureCo" builds permission sets for granular capabilities:
> - `Claims_Read_PS` — read Claims
> - `Claims_Edit_PS` — edit Claims
> - `Policy_Manage_PS` — manage Policies
> - `Reports_Export_PS` — export reports
>
> For their **Claims Adjuster** role, they need: Claims_Read + Claims_Edit + Policy_Manage
>
> Instead of assigning 3 permission sets to every Claims Adjuster, they create:
> `Claims_Adjuster_PSG` (Permission Set Group) = Claims_Read_PS + Claims_Edit_PS + Policy_Manage_PS
>
> Assign the one group → user gets all three. Clean and maintainable.

### Muting Permission Sets in a Group

Inside a Permission Set Group, you can add a **Muting Permission Set** to selectively remove a permission that would otherwise be granted by another set in the group.

> **Example:** `Claims_Adjuster_PSG` grants "Export Reports" via one of the included permission sets. But Claims Adjusters shouldn't export reports. Add a Muting Permission Set that removes "Export Reports" from the group.

---

## 4.6 Layer 3 — Field-Level Security (FLS)

**Field-Level Security** controls whether users can **see** (read) or **edit** fields on records — regardless of their object access.

### FLS Settings Per Field Per Profile

| Setting | What the User Sees |
|---------|-------------------|
| **Visible** | Can see and edit the field |
| **Read-Only** | Can see the field but cannot edit it |
| **Hidden (not visible)** | Field is completely invisible on pages and in reports |

### Real-World Example

> **Object:** `Opportunity`
> **Field:** `Discount_Percentage__c`
>
> | Profile | FLS Setting | What they see |
> |---------|-------------|---------------|
> | Sales Rep | Read-Only | Sees the discount, cannot change it |
> | Sales Manager | Visible (Edit) | Sees and can change the discount |
> | Support Agent | Hidden | Field doesn't exist for them |
>
> **Why?** Discounts are sensitive. Only managers should set them. Support doesn't need to know about pricing.

### Where to Set FLS

**Option 1 (by profile):** Setup → Profiles → [Profile] → Object Settings → [Object] → [Field]

**Option 2 (by field):** Setup → Object Manager → [Object] → Fields → [Field] → Set Field-Level Security

---

## 4.7 Layer 4 — Record Access: Org-Wide Defaults (OWD)

**Org-Wide Defaults** set the **baseline** level of access for records across the entire org. OWD defines what users can see when no other sharing mechanism gives them access.

**Path:** Setup → Sharing Settings → Org-Wide Defaults

### OWD Options

| Setting | What it means |
|---------|--------------|
| **Private** | Users can only see records they own. Others are invisible. |
| **Public Read Only** | All users can see all records, but only owners can edit |
| **Public Read/Write** | All users can see and edit all records |
| **Public Read/Write/Transfer** | For Leads/Cases — all users can also reassign ownership |
| **Controlled by Parent** | For Detail objects — access follows the Master record's sharing |

### Real-World Example

> **Company:** "SalesForce Training Ltd" — Sales org with 200 reps across 4 regions (North, South, East, West)
>
> **Business requirement:** Reps should only see their own Opportunities. They should NOT see deals belonging to reps in other regions.
>
> **OWD setting for Opportunity:** `Private`
>
> Now every rep sees only their own Opportunities. Managers see their team's (via Role Hierarchy). Cross-region sharing can be granted explicitly via Sharing Rules.

### Critical Rule

> OWD can **ONLY be opened up** (made more permissive) by Role Hierarchy, Sharing Rules, and Manual Sharing.
> OWD **CANNOT be made more restrictive** per record — it is the floor, not the ceiling.

---

## 4.8 Layer 4 — Record Access: Role Hierarchy

The **Role Hierarchy** is a tree structure of roles in your organization. A user in a higher role **automatically sees all records** owned by users in lower roles — regardless of OWD.

**Path:** Setup → Users → Roles

### Real-World Example

> **Company:** "NationalSales Inc."
>
> ```
>                     CEO
>                      │
>           ┌──────────┴──────────┐
>      VP Sales East         VP Sales West
>           │                     │
>     ┌─────┴────┐          ┌─────┴────┐
>  Mgr NY   Mgr MA       Mgr CA    Mgr TX
>     │                     │
>  Rep A  Rep B          Rep C  Rep D
> ```
>
> **OWD for Opportunity:** Private
>
> | User | Can See |
> |------|---------|
> | Rep A | Only his own Opportunities |
> | Mgr NY | Rep A's + Rep B's Opportunities |
> | VP Sales East | Mgr NY's + Mgr MA's + all their reps' Opportunities |
> | CEO | Everyone's Opportunities |

### Important Notes

- Role Hierarchy is **about data visibility**, NOT organizational reporting structure
- A user's Role is set on their User record — it is not the same as their Profile
- Role Hierarchy **only works upward** — a peer in the same role cannot see another peer's records (OWD controls that)

---

## 4.9 Layer 4 — Record Access: Sharing Rules

**Sharing Rules** extend record access beyond what OWD + Role Hierarchy provides. They share records with a **group of users** automatically based on a criterion.

**Path:** Setup → Sharing Settings → [Object] → Sharing Rules → New

### Two types of Sharing Rules

| Type | Shares records based on... | Example |
|------|---------------------------|---------|
| **Ownership-based** | Who owns the record | Share all Opportunities owned by East Region reps with the West Region team |
| **Criteria-based** | A field value on the record | Share all Opportunities with Stage = "Closed Won" with the Finance team |

### Real-World Example — Ownership-Based

> **Scenario:** "NationalSales" has a **Cross-Sell Team** that needs to see all Accounts owned by either the East or West teams.
>
> **OWD for Account:** Private
>
> Sharing Rule: "Share all Accounts owned by roles East Region OR West Region with the Cross-Sell Team (Public Group) with Read Only access."
>
> Now the Cross-Sell Team can see all regional accounts to identify upsell/cross-sell opportunities — without being able to edit them.

### Real-World Example — Criteria-Based

> **Scenario:** "ContractCo" — Finance team needs to see any Opportunity where `Stage = Closed Won` to process billing.
>
> **Sharing Rule:** Share all Opportunities where `Stage = Closed Won` with the Finance role with Read Only access.
>
> Finance sees exactly what they need — won deals — without seeing the entire active pipeline.

---

## 4.10 Public Groups

A **Public Group** is a named collection of users, roles, roles + subordinates, territories, or other groups — used as a target in Sharing Rules, Folder Sharing, and Queue membership.

### Real-World Example

> **Group:** `EMEA_Sales_Group` contains:
> - Role: VP Sales EMEA
> - Role + Subordinates: Sales Manager EMEA (includes all their reports)
> - User: john@company.com (a contractor not in the role hierarchy)
>
> Now you can share records with `EMEA_Sales_Group` in one sharing rule instead of creating 3 separate rules.

---

## 4.11 Manual Sharing

**Manual Sharing** allows the record **owner** (or an admin) to share an individual record with a specific user or group — on a case-by-case basis.

**How:** Open a record → Sharing button (top right) → Add → Select user/group → Set access level

### Real-World Example

> **Scenario:** Sales rep James owns an Opportunity with a key client. The client's technical evaluation involves an engineer, Lisa, who is not normally in the sharing path for James's records.
>
> James manually shares his Opportunity with Lisa (Read Only) so she can review the technical requirements and add her notes.
>
> When the Opportunity closes, James can remove Lisa's manual share.

> 💡 Manual Sharing cannot be granted above the sharing level the granter has (you can't share what you don't have).

---

## 4.12 Restriction Rules

**Restriction Rules** (introduced in 2021) let you **further limit** which records a user can access — even if sharing would normally open that access up.

This is the opposite of Sharing Rules — instead of opening access, Restriction Rules lock it down.

### Real-World Example

> **Scenario:** "HealthCo" — all doctors should normally see all Patient records (OWD: Public Read/Write for internal staff).
>
> However, **psychiatry records** are confidential — only psychiatrists should see them.
>
> **Restriction Rule on Case:** Restrict access to records where `Specialty__c = Psychiatry` — only users with the Psychiatrist permission set can see them.
>
> Other doctors (who normally have Read/Write via OWD) can no longer see psychiatric records.

---

## 4.13 Setup Audit Trail

The **Setup Audit Trail** logs every configuration change made in Setup — who did it, when, and what changed.

**Path:** Setup → Security → View Setup Audit Trail

### Real-World Example

> **Scenario:** Users start reporting they can no longer access the Discount field on Opportunities. The admin checks Setup Audit Trail:
>
> | Date | User | Action |
> |------|------|--------|
> | 09-Apr-2026 2:34 PM | admin@techbridge.com | Changed FLS on Discount__c for Sales Rep profile from Visible to Hidden |
>
> Mystery solved — an admin made an unintentional change. The audit trail is the forensic record.

---

## Summary

| Layer | Mechanism | What It Controls |
|-------|-----------|-----------------|
| 1 — Org | Login Hours, IP Ranges, MFA | Can they log into the org? |
| 2 — Object | Profiles, Permission Sets, PSGs | Can they see/edit this object? |
| 3 — Field | Field-Level Security (FLS) | Can they see/edit this field? |
| 4 — Record | OWD, Role Hierarchy, Sharing Rules, Manual Sharing | Can they see this specific record? |

| Concept | Key Takeaway |
|---------|-------------|
| OWD | The baseline floor — can only be opened up, not narrowed per record |
| Role Hierarchy | Upward visibility — managers see their reports' records |
| Sharing Rules | Automatic, rule-based broadening of access to groups |
| Permission Set | Additive permissions on top of a profile |
| Restriction Rule | Narrows access — opposite of Sharing Rule |

---

## 🔗 Trailhead Resources

- [Security Basics](https://trailhead.salesforce.com/content/learn/modules/security_basics)
- [Data Security](https://trailhead.salesforce.com/content/learn/modules/data_security)
- [Protect Your Data in Salesforce](https://trailhead.salesforce.com/content/learn/projects/protect-your-data-in-salesforce)
- [Permission Set Groups](https://trailhead.salesforce.com/content/learn/modules/permission-set-groups)

---

*Previous: [03 · Data Modeling & Objects](./03_Data_Modeling_and_Objects.md)*
*Next: [05 · Customization & UI →](./05_Customization_and_UI.md)*
