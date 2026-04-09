# 02 · Salesforce Licenses & User Management

> **Module:** Platform & Org Setup | **Difficulty:** 🟢 Beginner | **Est. Time:** 1.5 hrs

---

## 2.1 What is a Salesforce License?

A **license** is a contract that grants a user the right to access Salesforce and defines which features they can use. Every active user in Salesforce **must** have a license. No license = no login.

Think of a Salesforce license like a ticket to a theme park — the ticket type determines which rides (features) you can access.

### Why does licensing matter to an admin?

- You pay per license. Over-assigning wastes money.
- Under-assigning means users can't access what they need.
- The wrong license type blocks users from features they require.

---

## 2.2 Salesforce License Types

### User License Types

| License | Who It's For | What They Get |
|---------|-------------|---------------|
| **Salesforce** | Full CRM users — sales reps, service agents, admins | Full access to standard and custom objects; most features |
| **Salesforce Platform** | Users who only need custom apps, not CRM objects | Access to custom objects and most platform features; **no** Leads, Opportunities, Forecasting |
| **Force.com** | Internal app users who need very limited access | Up to 10 custom objects, no standard CRM objects |
| **Chatter Free** | Internal collaboration-only users | Chatter (social feed) only; no CRM records |
| **Chatter External** | External users (contractors) in Chatter only | Chatter groups only, no internal data |
| **Identity** | SSO / authentication only | Login and profile management; no Salesforce features |

### Real-World Example

> **Company:** "ManufactureCo" has three types of internal users:
>
> - **50 Sales Reps** → Salesforce license (full CRM — Leads, Accounts, Opportunities)
> - **20 Warehouse Staff** → Salesforce Platform license (they use a custom Inventory app, never touch Leads or Opportunities)
> - **5 Executives** → Salesforce license (they need dashboards and reports across all objects)
>
> By using Platform licenses for warehouse staff instead of full Salesforce licenses, ManufactureCo saves significant licensing costs.

---

## 2.3 Feature Licenses

On top of user licenses, **feature licenses** add specific capabilities to a user account.

| Feature License | What It Unlocks |
|----------------|-----------------|
| **Marketing User** | Access to Campaigns and the Campaign wizard |
| **Knowledge User** | Ability to create and manage Knowledge Articles |
| **Flow User** | Ability to run certain types of Flows |
| **Service Cloud User** | Service console and advanced service features |
| **WDC (Work.com)** | Coaching, performance, goals features |

> 💡 Feature licenses are **additive** — they work on top of a user license. A user with a Salesforce license + Marketing User feature license can manage both CRM and campaigns.

---

## 2.4 Permission Set Licenses

**Permission Set Licenses (PSLs)** are a newer licensing model that gives individual users access to specific features without changing their base license.

### Real-World Example

> **Scenario:** You have 100 users on standard Salesforce licenses. Only 10 of them need access to Einstein Analytics (a premium feature). Instead of upgrading all 100 to an Analytics license, you assign the **Analytics Cloud Integration User PSL** only to those 10 users.
>
> Result: You pay only for 10 advanced seats, not 100.

---

## 2.5 Creating and Managing Users

### How to Create a User

**Path:** Setup → Users → Users → New User

### Required Fields

| Field | Notes |
|-------|-------|
| **First Name / Last Name** | Display name in the org |
| **Alias** | Short identifier (auto-generated, max 8 chars) |
| **Email** | Used for notifications and password reset |
| **Username** | Must be **globally unique** across all Salesforce orgs worldwide; usually formatted as an email address but does not need to be a real email |
| **Role** | Position in the org's role hierarchy (affects record visibility) |
| **Profile** | Determines object permissions, app access, and page layouts |
| **User License** | Sets the category of access |

### Real-World Example

> **Username uniqueness trap:**
>
> Company: "TechBridge" wants to create a user with username `john@techbridge.com`. This works fine in Production.
>
> Later, they try to create the **same username** in their Sandbox. They get an error — because Salesforce usernames are global across ALL orgs, including sandboxes.
>
> **Solution:** Use a naming convention for sandbox users like `john@techbridge.com.sandbox` or `john+sandbox@techbridge.com` to ensure uniqueness.

---

## 2.6 Login Access Policies

**Login Access Policies** control whether administrators can log in as other users — a critical troubleshooting capability.

### Path: Setup → Security → Login Access Policies

### Options

| Setting | What It Means |
|---------|---------------|
| **Administrators Can Log in as Any User** | Admins can impersonate any user to see exactly what they see |
| **Grant Account Login Access** | User grants Salesforce Support the ability to log in as them |

### Real-World Example

> **Scenario:** A sales rep, Mark, calls the admin saying "I can't see the Discount field on the Opportunity page." The admin suspects it's a profile or field-level security issue.
>
> **Without Login Access:** Admin has to guess what Mark sees and make changes blindly, then ask Mark to verify. Multiple back-and-forth calls.
>
> **With Login Access Policies enabled:** Admin clicks "Login As User" → "Mark Johnson" → immediately sees Mark's exact view, spots that the Discount field is hidden by his Profile's FLS setting → fixes it on the spot.
>
> Total time: 2 minutes instead of 30.

### How to log in as another user

1. Setup → Users → Users
2. Find the user → click **"Login"** next to their name
3. You are now in their session — you see exactly what they see
4. Click **"Return to Admin"** in the top bar to go back

> ⚠️ This action is logged in the **Setup Audit Trail** — Salesforce records who logged in as whom and when.

---

## 2.7 Session Settings

**Session Settings** control how Salesforce manages active user sessions — a key security configuration.

### Path: Setup → Security → Session Settings

### Key Settings

| Setting | What It Controls | Recommended |
|---------|-----------------|-------------|
| **Session Timeout** | How long before an idle session expires | 2 hours for most orgs |
| **Lock sessions to the IP address** | Forces user to re-authenticate if IP changes mid-session | Enable for high-security orgs |
| **Require HttpOnly attribute** | Prevents JavaScript from reading session cookies | Always enabled |
| **Retain admin session after logging out as another user** | When admin uses "Login As" and then logs out, do they return to their own session or are they fully logged out? | Enable for convenience |

### Real-World Example

> **Scenario:** At "BankCo", security policy requires that any idle session expire after 30 minutes — similar to online banking websites. The admin sets Session Timeout to 30 minutes. Sales reps who walk away from their desks without locking are automatically logged out.
>
> **"Retain admin session" setting:** Without this enabled, when the admin logs in as Mark and then logs out of Mark's session, the admin is completely logged out of Salesforce too — they have to log in again with their own credentials. With this enabled, they are returned to their own admin session automatically.

---

## 2.8 Freezing vs Deactivating Users

When a user leaves the company or needs to be suspended, you have two options.

| Action | Effect | Reversible? | When to Use |
|--------|--------|-------------|-------------|
| **Freeze** | User cannot log in, but their license is still consumed | Yes — unfreeze anytime | Temporary suspension (e.g., employee under HR investigation) |
| **Deactivate** | User cannot log in, license is freed up for reassignment | Yes — reactivate anytime | Employee has left the company |
| **Delete** | Permanent removal | ❌ No — rarely possible | Only if user has never logged in or owned records |

### Real-World Example

> **Scenario 1 — Freeze:** "TechBridge" discovers that an employee, Alex, may have been exporting customer data inappropriately. HR needs 48 hours to investigate. The admin **freezes** Alex's account immediately — Alex can no longer log in, but his records and history remain intact for the investigation.
>
> **Scenario 2 — Deactivate:** Sarah, a sales rep, resigns. The admin **deactivates** her account. Her license is now available to be reassigned to the new hire. Her records (Opportunities, Activities) still exist and are reassigned to her manager via a Transfer Records process.

> ⚠️ You **cannot delete** a user who owns records, has logged in, or is referenced in workflows. Deactivate instead.

---

## 2.9 User Locale and Time Zone

Individual users can have their own **locale** and **time zone** settings, which override the org default.

### Path: User Profile → My Settings → Personal → Language & Time Zone

| Setting | Effect |
|---------|--------|
| **Locale** | Controls date format (MM/DD/YYYY vs DD/MM/YYYY), number format (1,000.00 vs 1.000,00), name order |
| **Language** | Language for Salesforce UI labels |
| **Time Zone** | All activity timestamps show in this time zone |

### Real-World Example

> **Company:** "GlobalSales Inc." has reps in New York and London.
>
> - New York rep sets their time zone to **America/New_York (EST)**
> - London rep sets their time zone to **Europe/London (GMT)**
>
> When the New York rep logs a call at 3:00 PM EST, it appears as **8:00 PM GMT** on the London rep's screen — because Salesforce stores everything in UTC and converts to each user's local time zone on display.
>
> **Reports** always reflect the **running user's time zone** — important for consistency in shared dashboards.

---

## 2.10 Delegated Administration

For large orgs, delegated administration lets you give non-admin users limited admin powers for a subset of users.

### Real-World Example

> **Company:** "RetailGroup" has 3,000 users across 15 regional offices. The central IT team cannot manage every user creation request from every region.
>
> **Solution:** Each regional office manager is set up as a **Delegated Administrator** with permission to:
> - Create/edit users in their own region only
> - Assign specific profiles to those users
> - Reset passwords for their region
>
> Central IT retains full admin control. Regional managers handle day-to-day user tasks. IT ticket volume drops by 60%.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| User License | Every user needs one; type determines feature access |
| Salesforce License | Full CRM access — for sales, service, admin users |
| Platform License | Custom apps only — no standard CRM objects |
| Feature License | Add-on for specific features (Marketing, Knowledge, etc.) |
| Username | Globally unique across ALL Salesforce orgs — use naming conventions |
| Login Access Policies | Allow admins to log in as users for troubleshooting |
| Session Settings | Control timeout, IP locking, and session retention |
| Freeze vs Deactivate | Freeze = temporary, Deactivate = permanent departure |

---

## 🔗 Trailhead Resources

- [Salesforce Licensing](https://trailhead.salesforce.com/content/learn/modules/salesforce-licensing)
- [User Setup & Management](https://trailhead.salesforce.com/content/learn/modules/lex_implementation_user_setup_mgmt)

---

*Previous: [01 · CRM & Salesforce Introduction](./01_CRM_and_Salesforce_Introduction.md)*
*Next: [03 · Data Modeling & Objects →](./03_Data_Modeling_and_Objects.md)*
