# 02 · Salesforce Licenses & User Management

> **Module:** Platform & Org Setup | **Difficulty:** 🟢 Beginner | **Est. Time:** 1.5 hrs

---

## 2.1 What is a Salesforce License?

A **license** grants a user the right to log into Salesforce and defines which features they can access. Every active user **must** have a license. Think of it like a theme park ticket — the ticket type determines which rides you can access.

---

## 2.2 User License Types

| License | Who It's For | Key Restriction |
|---------|-------------|-----------------|
| **Salesforce** | Full CRM users — sales reps, service agents, admins | No major restrictions |
| **Salesforce Platform** | Users of custom apps only | No Leads, Opportunities, Forecasting |
| **Force.com** | Very limited internal app users | Up to 10 custom objects |
| **Chatter Free** | Collaboration-only internal users | Chatter feed only, no CRM records |
| **Chatter External** | External contractors in Chatter | Chatter groups only |
| **Identity** | SSO / authentication only | No Salesforce features |

### Real-World Example

> **ManufactureCo** has:
> - 50 Sales Reps → **Salesforce license** (full CRM)
> - 20 Warehouse Staff → **Salesforce Platform license** (custom Inventory app only — they never touch Leads or Opportunities)
> - 5 Executives → **Salesforce license** (dashboards and reports across all objects)
>
> Warehouse staff on Platform licenses instead of full Salesforce saves ManufactureCo significant monthly licensing costs.

---

## 2.3 Feature Licenses

Feature licenses are **additive** — they unlock specific capabilities on top of a user license.

| Feature License | Unlocks |
|----------------|---------|
| **Marketing User** | Campaigns and the Campaign wizard |
| **Knowledge User** | Create and manage Knowledge Articles |
| **Flow User** | Run certain Flow types |
| **Service Cloud User** | Lightning Service Console, advanced service features |

---

## 2.4 Permission Set Licenses (PSLs)

PSLs give individual users access to premium features without changing their base license.

### Real-World Example

> You have 100 users on standard licenses. Only 10 need Einstein Analytics (a premium feature). Assign the Analytics PSL to just those 10 — pay only for 10 premium seats, not 100.

---

## 2.5 Creating Users

**Path:** Setup → Users → Users → New User

### Required Fields

| Field | Notes |
|-------|-------|
| First / Last Name | Display name in the org |
| Email | Used for notifications and password resets |
| **Username** | Must be **globally unique** across ALL Salesforce orgs worldwide. Formatted like an email but need not be real. |
| Role | Position in role hierarchy (affects record visibility) |
| Profile | Object permissions, app access, page layouts |
| User License | The license type category |

### Real-World Username Trap

> TechBridge creates `john@techbridge.com` in Production — works fine.
> They try the same username in Sandbox → **error** — it's already taken globally.
>
> **Solution:** Use a naming convention: `john@techbridge.com.sandbox` or `john+uat@techbridge.com`

---

## 2.6 Login Access Policies

Controls whether admins can impersonate users — critical for troubleshooting.

**Path:** Setup → Security → Login Access Policies

### Real-World Example

> Sales rep Mark says "I can't see the Discount field." Without Login Access: admin guesses and calls back 3 times. With Login Access enabled: admin logs in AS Mark, sees the missing field immediately, fixes FLS in 2 minutes.

**How to log in as another user:**
1. Setup → Users → Users
2. Find user → click **"Login"**
3. See exactly what they see
4. Click **"Return to Admin"** to go back

> All login-as actions are logged in **Setup Audit Trail**.

---

## 2.7 Session Settings

**Path:** Setup → Security → Session Settings

| Setting | Purpose | Recommendation |
|---------|---------|----------------|
| Session Timeout | Idle session expiry | 2 hours for most orgs; 30 min for banks |
| Lock sessions to IP | Re-authenticate if IP changes | Enable for high-security orgs |
| Retain admin session after login-as logout | Return to admin session when exiting user impersonation | Enable for convenience |

---

## 2.8 Freezing vs Deactivating Users

| Action | Effect | Reversible | When to Use |
|--------|--------|------------|-------------|
| **Freeze** | Cannot log in; license still consumed | Yes | Temporary suspension (HR investigation) |
| **Deactivate** | Cannot log in; license freed | Yes | Employee has left company |
| **Delete** | Permanent | Rarely possible | Only if user has never logged in or owned records |

### Real-World Example

> **Freeze:** Alex is under investigation. Admin freezes his account — he cannot log in, his records and history remain untouched for the audit.
>
> **Deactivate:** Sarah resigns. Admin deactivates her account — her license is freed for the new hire. Her Opportunities remain and are transferred to her manager.

---

## 2.9 User Locale & Time Zone

Individual users can override org defaults for their own locale and time zone.

**Path:** User → My Settings → Personal → Language & Time Zone

### Real-World Example

> New York rep and London rep both log an activity at the same moment. New York rep sees it as 3:00 PM EST. London rep sees 8:00 PM GMT. Salesforce stores in UTC, displays in each user's local time zone. Reports reflect the **running user's** time zone.

---

## ✅ Hands-On Tasks

> All tasks performed in your Trailhead Playground.

---

### 🔧 Task 1 — Create a New User

**Objective:** Practice creating a user with a proper license and profile.

**Steps:**
1. Setup → Users → Users → **New User**
2. Fill in:
   - First Name: `Practice`
   - Last Name: `User`
   - Email: your own email address
   - Username: `practiceuser@yourplayground.trailhead` *(unique name)*
   - Alias: `puser`
   - Role: leave blank for now
   - Profile: **Standard User**
   - User License: **Salesforce**
3. Click **Save**
4. Check your email — Salesforce sends a welcome/activation email

**Observe:** The user record shows their profile, role, license type, and last login date (currently blank).

---

### 🔧 Task 2 — Login As Another User

**Objective:** Practice the admin "Login As User" feature.

**Steps:**
1. Setup → Users → Users
2. Find `Practice User` → click **"Login"** next to their name
3. Notice the banner at top: *"You are logged in as Practice User"*
4. Navigate to Accounts → try creating a record
5. Try clicking Setup — notice you may have limited access
6. Click **"Return to [Your Name]"** in the top bar
7. Go to Setup → Security → View Setup Audit Trail → confirm your login-as action is logged

---

### 🔧 Task 3 — Freeze a User

**Objective:** Practice temporarily blocking access without losing data.

**Steps:**
1. Setup → Users → Users → click `Practice User`
2. Click **Edit**
3. Find the **Frozen** checkbox → check it → Save
4. Try logging in as them — you should be blocked
5. Return to the user → uncheck Frozen → Save
6. Confirm they can be logged into again

---

### 🔧 Task 4 — Deactivate a User

**Objective:** Understand how deactivation frees a license.

**Steps:**
1. Setup → Users → Users → click `Practice User`
2. Click **Edit** → uncheck **Active** checkbox → Save
3. Try logging in as them — blocked
4. Note: their records (if any) still exist in the org
5. Reactivate by clicking Edit → check Active again

**Observe:** When a user is deactivated, their name may still appear on records they owned.

---

### 🔧 Task 5 — Explore Profiles

**Objective:** Understand how profiles control object access.

**Steps:**
1. Setup → Profiles → click **Standard User**
2. Scroll to **Object Settings** section → click **Leads**
3. Note the CRUD permissions (Read, Create, Edit, Delete)
4. Scroll to **System Permissions**
5. Note what's enabled — e.g., "Run Reports", "Export Reports"
6. Now open **System Administrator** profile → compare the same sections

**Answer:** What can the System Admin do with Leads that the Standard User cannot?

---

### 🔧 Task 6 — Configure Session Settings

**Objective:** Practice setting security policies.

**Steps:**
1. Setup → Security → Session Settings
2. Note the current **Timeout Value**
3. Change it to **30 minutes**
4. Check the box: **"Require secure connections (HTTPS)"** (if not already checked)
5. Save
6. Change it back to the original setting

---

### 🔧 Task 7 — Explore User License Types (Research Task)

**Objective:** Understand the commercial impact of license choice.

**Steps:**
1. Setup → Company Information → scroll to the **User Licenses** section
2. Note what license types are available in your Playground and how many are available vs used
3. Visit [Salesforce Pricing](https://www.salesforce.com/editions-pricing/sales-cloud/) to compare license costs
4. Answer: If a company has 100 users and only 30 need full CRM access, what is the license strategy?

---

## 📝 Practice Questions

**Q1.** What makes a Salesforce username unique compared to a regular login username?

**Q2.** A user needs access to Campaigns to manage marketing activities. They already have a Salesforce user license. What additional license must be assigned?

**Q3.** What is the difference between a **User License** and a **Feature License**?

**Q4.** An admin logs in as another user to troubleshoot an issue. Where can the admin prove this action was taken, and what information is recorded?

**Q5.** A sales rep, James, is transferring to a different company. What should the admin do — freeze or deactivate his account? What happens to James's records?

**Q6.** What is the maximum number of Trailhead Playgrounds one Trailhead account can have at the same time?

**Q7.** A user in London and a user in New York both log activities at the exact same moment. How does Salesforce handle the time display for each user?

**Q8.** A company has 80 employees. 50 are sales reps who need Leads, Opportunities, and CRM. 30 are warehouse staff who only use a custom Inventory app. What license type should each group have?

**Q9.** What is the "Retain admin session after logging out as another user" setting used for?

**Q10.** Can a user have more than one profile at the same time?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — License Cost Optimization**

> "HealthCare Solutions" is buying Salesforce for 200 employees:
> - 80 are doctors who use a custom Patient Management app built on Salesforce
> - 60 are receptionists who use a custom Appointment Scheduling app
> - 40 are billing staff who use a custom Billing app and need to run basic reports
> - 20 are senior executives who need full CRM access, reports, and dashboards
>
> **Question:** Which license type would you assign to each group, and what is your cost-optimization rationale? What would happen if you assigned everyone a Salesforce full license instead?

---

**Scenario 2 — Security Incident Response**

> On a Monday morning, the HR manager calls the Salesforce admin: "We believe Marcus, a senior sales rep, has been downloading and sharing confidential customer lists with a competitor. We need to block his access **immediately** while legal investigates. However, we cannot delete any of his records — they are needed as evidence and for business continuity."
>
> **Question:** What specific action should the admin take right now? Why is this better than deactivating? What would the admin's next step be after the legal investigation concludes (two possible outcomes)?

---

**Scenario 3 — Username Convention Design**

> "GlobalRetail" uses Salesforce with three environments: Production, Full Sandbox, and Developer Sandbox. They onboard 20 new employees every month. Admins are constantly getting errors trying to create sandbox users with the same usernames as production.
>
> **Question:** Design a company-wide username naming convention that:
> - Works across all three environments
> - Is easy for admins to follow consistently
> - Prevents username conflicts
> - Keeps usernames readable and identifiable
>
> Show 3 example usernames for a user named "Priya Sharma" (email: priya@globalretail.com) across all three environments.

---

**Scenario 4 — Troubleshooting with Login Access**

> A sales rep, Tom, calls the helpdesk: "I submitted a quote for approval 2 days ago and it's still pending. I can't see the approval history or who it's assigned to. Something is wrong."
>
> **Question:** Describe step by step how the admin would:
> 1. Use Login Access to diagnose the issue
> 2. What specific things they would look for while logged in as Tom
> 3. How they would fix the issue
> 4. What they would verify after the fix

---

**Scenario 5 — Session Security Policy**

> "BankFirst" is implementing Salesforce for their loan officers. The Chief Security Officer has the following requirements:
> - Users must be logged out after 15 minutes of inactivity
> - Users must re-authenticate if they change networks (e.g., move from office WiFi to 4G)
> - Admins should be able to debug as any user but their own session should remain active after exiting
> - All access must be over HTTPS only
>
> **Question:** Map each of the CSO's requirements to a specific Salesforce Session Setting and explain the exact configuration for each. Are there any requirements that cannot be met purely through Session Settings?

---

## ✔️ Answer Key — Practice Questions

1. A Salesforce username must be unique **globally across all Salesforce orgs in the world** — not just within your own company. Even test orgs, sandboxes, and competitor orgs share the same namespace.
2. The **Marketing User feature license** must be assigned in addition to their standard Salesforce user license.
3. A User License defines the **category of access** (full CRM, platform only, Chatter only). A Feature License adds **specific features** on top — like Marketing or Knowledge access.
4. The **Setup Audit Trail** (Setup → Security → View Setup Audit Trail) records: the admin who performed the login, the date/time, the target user, and the action type.
5. **Deactivate** James's account. His license is freed for reassignment. His records (Opportunities, Activities, Accounts) remain in the org and ownership must be transferred to another user via a separate process.
6. Up to **10 Trailhead Playgrounds** per Trailhead account.
7. Salesforce stores all timestamps in **UTC**. When displayed, it converts to each user's configured time zone. The London user sees GMT time, the New York user sees EST time — both representing the same universal moment.
8. 50 sales reps → **Salesforce license** (full CRM). 30 warehouse + 40 billing staff → **Salesforce Platform license** (custom apps only, no standard CRM objects needed).
9. It controls whether the admin's own session remains active after they log out of a user's session via "Login As". Without it enabled, the admin is completely logged out when they exit the user's session and must log in again.
10. **No.** A user can have exactly **one** profile at any time. Additional permissions are granted via Permission Sets (multiple allowed).

---

## 🔗 Trailhead Resources

- [Salesforce Licensing](https://trailhead.salesforce.com/content/learn/modules/salesforce-licensing)
- [User Setup & Management](https://trailhead.salesforce.com/content/learn/modules/lex_implementation_user_setup_mgmt)

---

*Previous: [01 · CRM & Introduction](./01_CRM_and_Salesforce_Introduction.md) | Next: [03 · Data Modeling & Objects →](./03_Data_Modeling_and_Objects.md)*
