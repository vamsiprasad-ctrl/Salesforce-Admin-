# 02 · Salesforce Licenses & User Management — Complete Deep Guide

> **Module:** Platform & Org Setup | **Difficulty:** 🟢 Beginner | **Est. Time:** 4 hrs
> **Covers:** License types · Feature licenses · PSLs · User creation · Profiles · Permission Sets · PSGs · Login policies · Session settings · Password policies · Freeze vs Deactivate · Delegated Admin · Locale & Time Zone

---

## Table of Contents

1. [What is a Salesforce License?](#21-what-is-a-salesforce-license)
2. [User License Types — Complete Reference](#22-user-license-types--complete-reference)
3. [Feature Licenses](#23-feature-licenses)
4. [Permission Set Licenses (PSLs)](#24-permission-set-licenses-psls)
5. [Creating and Managing Users](#25-creating-and-managing-users)
6. [User Fields — Deep Dive](#26-user-fields--deep-dive)
7. [Password Policies](#27-password-policies)
8. [Login Access Policies](#28-login-access-policies)
9. [Session Settings](#29-session-settings)
10. [Freezing vs Deactivating vs Deleting Users](#210-freezing-vs-deactivating-vs-deleting-users)
11. [Delegated Administration](#211-delegated-administration)
12. [User Locale, Language & Time Zone](#212-user-locale-language--time-zone)
13. [Single Sign-On (SSO) — Overview](#213-single-sign-on-sso--overview)
14. [Hands-On Tasks](#-hands-on-tasks)
15. [Practice Questions](#-practice-questions)
16. [Scenario-Based Questions](#-scenario-based-questions)
17. [Answer Key](#️-answer-key)

---

## 2.1 What is a Salesforce License?

A **license** is a contractual entitlement that:
1. Grants a user the right to **log in** to Salesforce
2. Defines which **objects and features** they can access
3. Sets **limits** on what they can do

Every active user must have **exactly one user license**. Without a license, a user record cannot be created or activated.

Think of it as a **membership card** at a club:
- The **card type** (license) determines which areas of the club you can enter
- The **membership add-ons** (feature licenses) give access to premium rooms
- The **special passes** (Permission Set Licenses) temporarily unlock VIP areas

### Why Licensing Matters to an Admin

Salesforce licensing is one of the most impactful decisions in an implementation:

| Decision | Impact |
|---------|--------|
| Assign the wrong license type | User can't access what they need, or you overpay |
| Over-provision licenses | Wasted money — unused license seats still cost |
| Under-provision licenses | Users blocked from tools they need to do their jobs |
| Wrong feature licenses | Key features like Knowledge or Marketing unavailable |
| Ignoring PSLs | Paying full license price when a cheaper PSL would suffice |

### License Count Management

**Path:** Setup → Company Information → scroll to **User Licenses** section

This shows for each license type:
- Total purchased licenses
- Licenses currently in use
- Licenses remaining

If "Licenses Remaining" = 0, you cannot create any more users of that type. You must either purchase more or deactivate existing users to free up seats.

---

## 2.2 User License Types — Complete Reference

### Salesforce License (Full CRM)

**Who it's for:** Sales reps, service agents, marketing managers, admins — anyone who needs full CRM functionality.

**What it includes:**
- Access to all standard CRM objects: Leads, Accounts, Contacts, Opportunities, Cases, Campaigns
- Access to custom objects (unlimited)
- Reports and Dashboards
- All standard and custom apps
- Full automation access (Flows, Approval Processes)
- API access (Enterprise Edition and above)

**What it does NOT include by default:**
- Marketing User features (separate feature license needed)
- Knowledge User features (separate feature license needed)

**Approximate cost:** Most expensive user license (~$25–$300+/user/month depending on Edition)

**Real-World Example:**
> At "TechBridge," these users need Salesforce licenses:
> - 50 Sales Reps — manage Leads, Opportunities, Accounts
> - 20 Service Agents — manage Cases, use Service Console
> - 5 Marketing Managers — run Campaigns (+ Marketing User feature license)
> - 3 Salesforce Admins — configure and manage the platform

---

### Salesforce Platform License

**Who it's for:** Internal users who only use **custom applications** built on Salesforce — and don't need standard CRM objects like Leads, Opportunities, or standard Forecasting.

**What it includes:**
- Access to custom objects (unlimited)
- Custom apps
- Standard objects: Accounts, Contacts, Tasks, Events (read-only on some)
- Reports and Dashboards on custom data
- Chatter

**What it does NOT include:**
- Leads
- Opportunities
- Standard Sales forecasting
- Campaigns
- Cases (limited)

**Approximate cost:** Significantly cheaper than Salesforce license (~60–70% less)

**Real-World Example:**
> **ManufactureCo** built a custom **Inventory Management System** on Salesforce. Their 40 warehouse supervisors need to track stock levels, record deliveries, and manage purchase orders — all on custom objects. They never touch Leads or Opportunities.
>
> → Assign **Salesforce Platform licenses** to all 40 supervisors.
>
> Savings: If Salesforce license = $150/user/month and Platform = $25/user/month, that's $125 × 40 users × 12 months = **$60,000 saved per year**.

---

### Force.com License

**Who it's for:** Internal users with very limited access needs — only specific custom apps.

**What it includes:**
- Access to up to **10 custom objects**
- One custom app
- Chatter (read only)

**What it does NOT include:**
- Most standard objects
- More than 10 custom objects
- Reports and Dashboards (very limited)

**When to use:** Rarely used today. For extremely basic internal app users.

---

### Chatter Free License

**Who it's for:** Internal users who only need to participate in collaboration — posting, commenting, following records — but don't need to see or edit Salesforce data.

**What it includes:**
- Chatter feed access
- Group participation
- File sharing via Chatter
- Profile and following

**What it does NOT include:**
- Any CRM objects (no Leads, Accounts, Cases, etc.)
- Custom objects
- Reports

**Real-World Example:**
> **LegalFirm** has 200 support staff (paralegals, administrative assistants) who need to participate in internal project discussions and see company announcements via Chatter — but they don't need access to client CRM data.
>
> → Assign **Chatter Free** licenses to all 200 support staff.
>
> Chatter Free is often **no additional cost** — making it ideal for large internal communities.

---

### Chatter External License

**Who it's for:** External users (outside your company — contractors, vendors, agencies) who you want to include in specific Chatter groups.

**What it includes:**
- Access to Chatter groups they are explicitly invited to
- File sharing within those groups

**What it does NOT include:**
- Internal Chatter feeds
- Any Salesforce records
- Company-wide announcements

**Real-World Example:**
> **AdvertisingAgency** works with a design agency on marketing campaigns. The agency's creative team needs to participate in the "Campaign Design" Chatter group to share files and give feedback — but they should NOT see any customer data.
>
> → Assign **Chatter External** licenses to the 5 design agency contacts.

---

### Identity License

**Who it's for:** Users who only need Salesforce for **authentication purposes** — Single Sign-On (SSO). They use Salesforce as an identity provider to access OTHER applications, not Salesforce itself.

**What it includes:**
- Login capability
- User profile management
- Connected App access (SSO to other apps)

**What it does NOT include:**
- Any Salesforce CRM features whatsoever
- Objects, records, reports — nothing

**Real-World Example:**
> **TechCorp** uses Salesforce as their central identity provider. When an employee logs into their HR system, their project management tool, or their expense management app, they all use Salesforce SSO behind the scenes. These employees never actually open Salesforce — they just authenticate through it.
>
> → Assign **Identity licenses** to these employees.

---

### Guest User License

**Who it's for:** Unauthenticated external visitors to an Experience Cloud (Community) site.

**What it includes:**
- Access to public pages on an Experience Cloud site
- Can see publicly shared Knowledge Articles
- Can submit a Web-to-Case or Web-to-Lead form

**What it does NOT include:**
- Any logged-in functionality
- Access to CRM records
- Personal profile or preferences

**Cost:** Free — automatically included with Experience Cloud

---

### Community / Experience Cloud Licenses

For external users who log into an Experience Cloud portal:

| License | Use Case | Features |
|---------|---------|---------|
| **Customer Community** | Customer self-service portals | Cases, Accounts, limited custom objects |
| **Customer Community Plus** | Advanced customer portals | + Reporting, roles, more custom objects |
| **Partner Community** | Channel partner portals | Deal registration, lead distribution, full custom objects |
| **External Apps** | Fully custom external apps | Maximum flexibility, pay-per-login or per-member |

---

## 2.3 Feature Licenses

**Feature Licenses** are **add-ons** that unlock specific capabilities on top of a user license. They are assigned per user individually — not all users on a license type need the same feature licenses.

### How to Assign a Feature License

**Path:** Setup → Users → Users → [click user] → Edit → scroll to "Feature License Assignments" section → check the box next to the feature → Save

---

### Marketing User

**Unlocks:**
- Access to the Campaigns tab and Campaign object
- Access to the Campaign Influence features
- Ability to import Leads directly from the Campaigns area
- Campaign wizard for creating and managing campaigns

**Who needs it:** Marketing Managers, Campaign Managers, anyone managing Campaigns

**Important:** Without this, users with a Salesforce license can still SEE Campaigns (if their profile allows) but cannot CREATE or MANAGE them through the Campaign wizard.

**Real-World Example:**
> "MarketFirst" has 80 Salesforce users. Only 5 are on the marketing team and actually manage Campaigns. Assigning Marketing User to all 80 would be wasteful.
>
> → Assign Marketing User feature license only to the 5 marketing team members.

---

### Knowledge User

**Unlocks:**
- Ability to CREATE Knowledge Articles (not just read them)
- Access to the Knowledge authoring interface
- Ability to submit articles for review and publish them
- Article version management

**Who needs it:** Content writers, Knowledge Managers, Support Leads who create Help documentation

**Important:** All users with profile access to Knowledge can READ published articles. Only Knowledge Users can CREATE and MANAGE articles.

**Real-World Example:**
> "CloudSoft" has 150 support agents who read Knowledge Articles daily. But only 8 Knowledge Managers actually write and maintain articles.
>
> → Assign Knowledge User to only those 8 → saves significant cost on premium feature licensing.

---

### Service Cloud User

**Unlocks:**
- Access to the Lightning Service Console app
- Advanced service features (Omni-Channel routing, Supervisor tools)
- SLA and entitlement management features
- Service-specific reporting

**Who needs it:** Support agents using the Service Console as their primary workspace

---

### Flow User

**Unlocks:**
- Ability to run certain Flow types that require the Flow User feature license
- Specifically needed for some automated process features

**Note:** Most Flow execution does not require this license — regular Salesforce users can trigger and run most Flows. This is for advanced/specific flow types.

---

### Sales Console User

**Unlocks:**
- Access to the Lightning Sales Console — a split-view workspace for high-volume sales reps, similar to Service Console but for sales

---

### Live Agent User

**Unlocks:**
- Ability to be assigned as a Live Chat agent
- Access to the agent chat interface

---

## 2.4 Permission Set Licenses (PSLs)

**Permission Set Licenses (PSLs)** are a modern, flexible licensing model. They allow you to assign **specific premium features** to individual users without changing their base license — and without paying for those features for users who don't need them.

### PSL vs Feature License — What's the Difference?

| | Feature License | Permission Set License |
|--|----------------|----------------------|
| Assigned via | Checkboxes on User record | Permission Set → assign to user |
| Granularity | A bundle of features for a role | Very specific individual features |
| Can be revoked | Yes | Yes |
| Affects license count | No | Yes (tracked separately) |
| Example | Marketing User | Einstein Analytics for Sales |

### Common PSLs

| PSL | What It Gives Access To |
|----|------------------------|
| **Analytics Cloud Einstein Analytics Growth** | Tableau CRM (Einstein Analytics) |
| **Einstein Prediction Builder** | AI-powered custom predictions |
| **Salesforce Maps** | Territory mapping and route optimization |
| **Revenue Intelligence** | Advanced revenue analytics |
| **High Velocity Sales** | Cadence management for SDR teams |
| **Pardot** | B2B marketing automation |
| **CPQ** | Configure, Price, Quote functionality |

### Real-World Example — PSL Cost Optimization

> **InsureCo** has 500 insurance agents on Salesforce licenses. The Analytics team has built custom Einstein Analytics dashboards for the top 25 Regional Managers — they need to drill into complex pipeline and policy data.
>
> **Option A:** Upgrade all 500 users to Tableau CRM license → $75/user/month extra = $37,500/month extra = $450,000/year
>
> **Option B:** Assign **Einstein Analytics PSL** to just 25 Regional Managers → $75/user/month × 25 = $1,875/month = $22,500/year
>
> **Saving: $427,500/year** by using PSLs correctly.

---

## 2.5 Creating and Managing Users

### User Creation — Step by Step

**Path:** Setup → Users → Users → New User

#### Step 1 — Basic Information

| Field | Required | Notes |
|-------|---------|-------|
| First Name | ❌ | Appears in the org |
| Last Name | ✅ | Must be filled |
| Alias | ✅ | Short identifier, auto-generated, max 8 characters |
| Email | ✅ | Where password reset and notifications go |
| Username | ✅ | The login ID — globally unique across ALL Salesforce orgs |

#### Step 2 — Role, Profile, License

| Field | Required | Notes |
|-------|---------|-------|
| Role | ❌ | Affects record visibility via Role Hierarchy |
| User License | ✅ | Determines which features are available |
| Profile | ✅ | Determines object permissions, app access, page layouts |

#### Step 3 — Locale Settings

| Field | Notes |
|-------|-------|
| Time Zone | User's local time zone — overrides org default |
| Locale | User's date, number, currency format — overrides org default |
| Language | UI language — overrides org default |
| Email Encoding | Character encoding for outbound emails |

#### Step 4 — Save

Upon saving, Salesforce automatically sends a **welcome email** to the user's email address with a link to set their password.

---

## 2.6 User Fields — Deep Dive

### The Username Field — Critical Understanding

The **Username** field is the single most misunderstood field in Salesforce user management.

**Key facts:**
1. It must be formatted like an email address: `name@something.com`
2. It does NOT need to be a real email address
3. It does NOT need to match the user's actual email address
4. It must be **unique across ALL Salesforce orgs in the entire world** — not just your org

**Why globally unique?**
Salesforce is a multitenant platform. When a user types their username at login.salesforce.com, Salesforce must uniquely identify which org they belong to. If `john@acme.com` could exist in both Acme Corp's org and a test org, Salesforce wouldn't know which org to route them to.

**Username Conflict Scenarios:**

> **Scenario A:** You create `sarah@techbridge.com` in Production. Later, you try to create the same in your sandbox. **Error:** Username already in use.
>
> **Solution:** Use a sandbox suffix: `sarah@techbridge.com.sandbox`

> **Scenario B:** Your company acquires another company that also uses Salesforce. The acquired company has an employee with username `john@bothcompanies.com`. Your company also has an employee with the same username. **Cannot merge** — one must be renamed.

**Naming Convention Best Practices:**

| Environment | Convention | Example |
|-------------|-----------|---------|
| Production | `firstname.lastname@company.com` | `priya.sharma@infosys.com` |
| Full Sandbox | `firstname.lastname@company.com.fullsandbox` | `priya.sharma@infosys.com.fullsandbox` |
| Dev Sandbox | `firstname.lastname@company.com.dev` | `priya.sharma@infosys.com.dev` |
| UAT Sandbox | `firstname.lastname@company.com.uat` | `priya.sharma@infosys.com.uat` |

---

### The Email Field vs Username Field

Many beginners confuse these:

| Field | Purpose | Must be real email? | Must be unique? |
|-------|---------|--------------------|-----------------| 
| **Email** | Where notifications and password resets go | ✅ Yes | ❌ No (multiple users can share an email) |
| **Username** | The login ID | ❌ No | ✅ Yes (globally unique) |

**Real example:**
> An admin sets up a test user for training purposes:
> - Email: `admin@techbridge.com` (their own real email — they want to receive the test user's notifications)
> - Username: `testuser2026@techbridge.com.training` (fake, just needs to be unique)
>
> This is perfectly valid. The test user's notifications go to the admin's real email.

---

### The Alias Field

- Maximum 8 characters
- Auto-generated from first name initial + last name
- Used in some list views as a compact identifier
- Must be unique within your org (not globally)

Example: "Priya Sharma" → Alias = `psharm` (auto-generated) or `psharma` (custom)

---

### The Role Field

- Determines position in the **Role Hierarchy** — directly impacts record visibility
- NOT the same as the Profile
- Users with no Role assigned cannot see records based on the hierarchy (they are outside the hierarchy)
- Changing a user's Role immediately changes their record visibility

**Example:** Moving Sarah from "Sales Rep East" role to "Sales Manager East" role — she immediately gains visibility into all records owned by everyone below "Sales Manager East" in the hierarchy.

---

## 2.7 Password Policies

**Path:** Setup → Security → Password Policies

Password Policies control how users create and manage their Salesforce passwords.

### Available Settings

| Setting | Options | Recommended |
|---------|---------|-------------|
| **User passwords expire in** | Never, 30, 60, 90, 180, 365 days | 90 days |
| **Enforce password history** | 0–24 previous passwords | 5–10 passwords |
| **Minimum password length** | 5–50 characters | 10+ characters |
| **Password complexity** | No restriction, Alpha, Alphanumeric, Special Characters | Must mix alpha, numeric, and special |
| **Maximum invalid login attempts** | 0 (no limit), 3, 5, 10 | 5 attempts |
| **Lockout effective period** | 15 min, 30 min, 60 min, until admin unlocks | 30 minutes |
| **Require a minimum 1 day between password changes** | Yes/No | Yes |

### Password Complexity Requirements

You can require passwords to contain:
- Uppercase AND lowercase letters
- At least one number
- At least one special character (`!@#$%^&*`)

**Enterprise example:**
> "BankFirst" password policy:
> - Minimum length: 12 characters
> - Must include: uppercase, lowercase, number, special character
> - Expires: 90 days
> - Cannot reuse last 12 passwords
> - Locked out after 5 failed attempts for 30 minutes

### Profile-Level Password Policies

You can set **different password policies per Profile** — useful when you have different security requirements for different user groups.

**Example:**
> Regular users: 8-character minimum, expires every 90 days
> Admins: 12-character minimum, expires every 60 days, must include special characters
> Read-Only external users: 8-character minimum, never expires

---

## 2.8 Login Access Policies

**Login Access Policies** control whether **administrators** can log in AS another user — seeing exactly what that user sees — for troubleshooting purposes.

**Path:** Setup → Security → Login Access Policies

### The Two Settings

**1. Administrators Can Log in as Any User**
- When enabled: any System Administrator can click "Login" next to any user's name and access Salesforce as that user
- The admin sees exactly what the user sees — same page layouts, same records visible, same buttons available
- The user's session is NOT interrupted — they can both be "logged in" as the same user simultaneously

**2. Grant Account Login Access**
- A user can grant Salesforce Customer Support the ability to log in as them for support purposes
- Controlled in: User's avatar → Settings → Grant Account Login Access

### How to Log In As Another User

1. Setup → Users → Users
2. Find the user in the list
3. Click **"Login"** next to their name (button appears if Login Access is enabled)
4. A banner appears at the top: "You are logged in as [User Name]. Return to [Admin Name]."
5. Navigate the org as that user
6. Click **"Return to [Admin Name]"** in the banner to exit

### What You CAN Do While Logged In As Another User

- See exactly what records they can see
- See exactly what fields are visible (or hidden)
- See exactly what page layouts they see
- Navigate every page they can navigate
- Identify permission and access issues immediately

### What You CANNOT Do While Logged In As Another User

- Change Setup settings (restricted to prevent admin abuse)
- Access Setup pages (you're in the user's context, not admin context)
- Some managed package features may also restrict login-as

### Why This Is Critical for Troubleshooting

**Without Login Access:**
> User: "I can't see the Discount field on the Opportunity."
> Admin: "Can you tell me what you see? What fields are showing?"
> User: "Umm... Account Name, Stage, Amount..."
> Admin: "Is there a Discount field anywhere on the page?"
> User: "I don't think so."
> Admin makes a guess, changes FLS, asks user to check again. Still wrong. 3 more calls.

**With Login Access:**
> User: "I can't see the Discount field on the Opportunity."
> Admin: Clicks Login → opens same Opportunity → immediately sees the field is indeed missing → checks FLS → field is hidden for this profile → fixes in 2 minutes → user confirms.

### Audit Trail for Login-As

Every "Login As" action is logged in **Setup Audit Trail** with:
- Date and time of the login-as
- Which admin performed it
- Which user was impersonated
- Duration (when they returned to admin)

This creates accountability and is required for compliance in regulated industries.

---

## 2.9 Session Settings

**Path:** Setup → Security → Session Settings

Session Settings control how active user sessions are managed — a key security configuration.

### Session Timeout

Controls how long an **idle** session remains active before the user is automatically logged out.

| Timeout Value | Appropriate For |
|--------------|----------------|
| 15 minutes | Banking, healthcare, high-security orgs |
| 30 minutes | Financial services, legal |
| 1 hour | General enterprise |
| 2 hours | Standard — most orgs |
| 4–8 hours | Internal tools where users work long sessions |
| Never | Not recommended for security |

**What "idle" means:** The timeout clock resets whenever the user clicks, types, or navigates. If a user opens a page and then walks away from their computer, the timeout begins from that moment of inactivity.

**Real-World Example:**
> "BankFirst" sets timeout to 15 minutes. A loan officer walks away from their desk to help a customer at the counter. When they return 20 minutes later, they are automatically logged out. They re-authenticate to continue. This prevents unauthorized access if someone walks up to an unattended computer.

---

### Lock Sessions to the IP Address from Which They Originated

When enabled: If a user's IP address changes during an active session (e.g., they disconnect from office WiFi and switch to 4G), Salesforce terminates their session and requires re-authentication.

**When to enable:** High-security environments where session hijacking is a concern.
**Trade-off:** Users who move between networks frequently (e.g., on laptops that switch between WiFi and Ethernet) may experience frequent forced logouts.

---

### Force Logout on Session Timeout

When enabled: Users are completely logged out (session destroyed) when timeout occurs.
When disabled: Users may see a timeout warning and can extend their session.

---

### Require Secure Connections (HTTPS)

Ensures all Salesforce traffic uses HTTPS (encrypted). This should **always be enabled** in production orgs. Prevents data interception via man-in-the-middle attacks.

---

### Retain Admin Session After Logging Out as Another User

**Scenario without this enabled:**
1. Admin logs in as User A to troubleshoot
2. Admin clicks "Return to Admin"
3. Admin is completely logged out of Salesforce
4. Admin must log in again with their own credentials

**Scenario with this enabled:**
1. Admin logs in as User A to troubleshoot
2. Admin clicks "Return to Admin"
3. Admin is returned to their own active session — still logged in, still on the same page
4. No re-authentication needed

**Recommendation:** Enable this. It saves time and frustration for admins who regularly use Login-As for troubleshooting.

---

### Enable Clickjack Protection

Prevents Salesforce pages from being embedded inside iframes on malicious websites — a security attack called "clickjacking." Always enable in production.

---

### Enable Content Security Policy (CSP)

Controls which external resources (scripts, styles, images) can be loaded on Salesforce pages. Important for preventing cross-site scripting (XSS) attacks.

---

### Login IP Ranges (on Profiles)

**Path:** Setup → Profiles → [Profile] → Login IP Ranges

Restricts users with this profile to only log in from specific IP address ranges.

**Real-World Example:**
> "BankCo" requires all Salesforce access to originate from within the corporate network (IP range: 203.0.113.0/24). They add this IP range to all profiles. If an employee tries to log in from home internet, they are blocked even with correct credentials.

**Exception:** Admins often have a broader IP range or no restriction — to allow emergency access from anywhere.

---

### Login Hours (on Profiles)

**Path:** Setup → Profiles → [Profile] → Login Hours

Restricts when users with a profile can log in.

**Real-World Example:**
> "CallCenter Inc." — agents work shifts: 9AM–6PM Monday–Friday. Their profile has Login Hours set to Monday–Friday, 8:30AM–6:30PM. An agent trying to log in at 9PM on Saturday receives: "You cannot log in outside your company's login hours."

**Exception:** Always set System Administrator profiles with 24/7 login hours — admins need emergency access at any time.

---

## 2.10 Freezing vs Deactivating vs Deleting Users

This is a very commonly tested concept. Understand the exact differences.

### Comparison Table

| Action | How | Effect on Login | License | Records | Reversible | When to Use |
|--------|-----|----------------|---------|---------|------------|-------------|
| **Freeze** | Edit user → check Frozen | Immediately blocked | Still consumed | Untouched | ✅ Yes — unfreeze anytime | Temporary access suspension |
| **Deactivate** | Edit user → uncheck Active | Immediately blocked | Freed for reuse | Untouched, still owned by user | ✅ Yes — reactivate anytime | Permanent departure |
| **Delete** | Cannot delete most users | N/A | Freed | Orphaned | ❌ No | Only if user never logged in and owns no records |

---

### Freezing — When and Why

**Use Freeze when:**
- Employee is under HR investigation (can't risk destroying evidence)
- Maternity/paternity leave (might return)
- Account suspected of compromise (immediate block needed while investigating)
- Temporary contract worker between projects

**What Freeze does:**
- User cannot log in to Salesforce immediately
- User's profile, permissions, record ownership — all unchanged
- License count is NOT reduced (you still pay for it)
- User's API tokens and connected app access are also blocked
- Can be reversed in seconds

**What Freeze does NOT do:**
- It does not transfer record ownership
- It does not change any permissions
- It does not reduce your license count

**How to Freeze:**
1. Setup → Users → Users → find user
2. Click the user's name to open their record
3. Click **Edit**
4. Check the **"Frozen"** checkbox
5. Save
6. User is immediately blocked

**Alternative quick method:**
1. From the Users list view
2. Check the checkbox next to the user
3. Click **"Freeze"** from the actions dropdown

---

### Deactivating — When and Why

**Use Deactivate when:**
- Employee has permanently left the company
- Contractor engagement has ended
- User account is no longer needed

**What Deactivate does:**
- User cannot log in to Salesforce immediately
- License is freed up (you can now assign this license to a new user)
- User's records remain in the org with the deactivated user listed as owner
- User's name still appears in record history and Chatter posts
- Can be reactivated (useful if someone returns from a long leave)

**What Deactivate does NOT do:**
- Records are NOT automatically transferred — you must do this separately
- History is NOT deleted — past activities and record changes remain
- The user record is NOT deleted — it stays in the system

**How to Deactivate:**
1. Setup → Users → Users → find user
2. Click the user's name
3. Click **Edit**
4. Uncheck the **"Active"** checkbox
5. Save

**After Deactivating — Transfer Records:**
Records owned by the deactivated user remain with them as owner. To transfer:
- **Path:** Setup → Users → Users → find deactivated user → **Mass Transfer Records**
- Transfers all records owned by the user to another user

---

### Deleting Users — The Reality

You almost **never delete** a Salesforce user. Here's why:

**You can only delete a user if ALL of these are true:**
- They have never logged into Salesforce
- They have no record ownership (never created or owned records)
- They have no history (no changes, no activities)

If a user has ever logged in, they are referenced in the system's audit trail — and Salesforce won't allow deletion to preserve data integrity.

**What to do instead:** Deactivate. The user record remains but they cannot log in and their license is freed.

---

### Impact on Reporting and Audit

When a user is deactivated:
- Their name still appears on records they created or edited
- Their name still appears in the Setup Audit Trail for changes they made
- Reports that filter by "My Team" or Role Hierarchy still show records they owned
- Field history still shows their name for changes they made

This is intentional — Salesforce preserves full audit trails even after a user leaves.

---

## 2.11 Delegated Administration

**Delegated Administration** allows you to give non-system-admin users limited admin capabilities — specifically for managing users in a defined group.

**Path:** Setup → Security → Delegated Administrators

### What a Delegated Administrator Can Do

Configurable — you choose which of these they can do:
- Create and edit users in a specified subset of profiles
- Assign specified permission sets to users
- Manage users in specified roles and their subordinates
- Reset passwords for specified users
- Unlock locked-out users

### What a Delegated Admin CANNOT Do

- Access Setup beyond user management
- Change profiles, permission sets, or their definitions
- Modify org settings
- Access data they don't otherwise have access to via their own profile

### Real-World Example — Enterprise Use Case

> **GlobalCorp** has 10,000 Salesforce users across 25 regional offices. The central IT team of 3 Salesforce Admins cannot handle all user provisioning requests — they are constantly backlogged.
>
> **Solution:** Each regional office manager is made a **Delegated Administrator** with permission to:
> - Create users in their region only (restricted to "Sales Rep" and "Service Agent" profiles)
> - Reset passwords for users in their region
> - Unlock locked accounts in their region
>
> **Result:**
> - IT team's user management tickets drop by 80%
> - Regional managers handle day-to-day user needs (new hires, password resets)
> - Central IT retains full control — they set the rules; regional managers execute within those rules

### Setting Up Delegated Administration

1. Setup → Security → Delegated Administrators → **New**
2. Create a Delegated Group — name it (e.g., "North Region User Admins")
3. Add **Members** — which users are delegated admins
4. Add **Users to Administer** — which users/roles they can manage
5. Add **Assignable Profiles** — which profiles they can assign to new users
6. Add **Assignable Permission Sets** — which PSets they can grant

---

## 2.12 User Locale, Language & Time Zone

### The Three Settings

| Setting | Controls | Set At |
|---------|---------|--------|
| **Language** | UI text labels, menus, button names, system messages | User record + Org default |
| **Locale** | Date format, number format, currency display, name order | User record + Org default |
| **Time Zone** | When timestamps display and how "today" is calculated | User record + Org default |

### Hierarchy: Org Default → User Override

Salesforce applies settings in this order:
1. **Org Default** (set in Company Information) — applies to everyone
2. **User-level setting** — overrides the org default for that individual user

**Path for user to change their own settings:**
Click Avatar → Settings → Personal → Language & Time Zone → Edit

**Path for admin to change another user's settings:**
Setup → Users → [User] → Edit → scroll to locale section

---

### Time Zone — Deep Dive

**The Storage Model:**
Salesforce stores all date/time values in **UTC (Coordinated Universal Time)** internally. When a value is displayed, Salesforce converts from UTC to each user's configured time zone.

**Example:**
> An event is created at 3:00 PM IST (India Standard Time = UTC+5:30)
>
> Salesforce stores it as: **9:30 AM UTC**
>
> | User | Their Time Zone | Displayed Time |
> |------|----------------|---------------|
> | Mumbai team | IST (UTC+5:30) | 3:00 PM IST |
> | London team | BST (UTC+1) | 10:30 AM BST |
> | New York team | EST (UTC-4) | 5:30 AM EST |
>
> All three users see the same meeting in their local time — no conversion needed from them.

**What breaks when time zone is wrong:**
- Activity timestamps show in the wrong time
- Scheduled Flows run at the wrong time (Flows use org default, not user)
- "Today" in date filters may be off by a day at midnight boundaries
- Reports with date/time filters may exclude or include wrong records

---

### Locale — Deep Dive

**The Date Format Problem:**
This is the most common locale mistake — and it causes real data errors.

| Locale | Date Format | How "04/05/2026" reads |
|--------|------------|----------------------|
| English (US) | MM/DD/YYYY | April 5, 2026 |
| English (India) | DD/MM/YYYY | May 4, 2026 |
| English (UK) | DD/MM/YYYY | May 4, 2026 |

**Same text, completely different date.** In a global org, if some users have US locale and others have India locale, dates entered by one user may be misread by another.

**Best Practice:** Set the Default Locale to match where the majority of your users are. Ensure all users set their own locale correctly.

**Number Format Differences:**

| Locale | One Million | One Thousand |
|--------|------------|--------------|
| English (US) | 1,000,000.00 | 1,000.00 |
| English (India) | 10,00,000.00 | 1,000.00 |
| German | 1.000.000,00 | 1.000,00 |

Currency amounts in reports will display differently based on the **running user's** locale — even if the underlying data is the same.

---

### Language — Deep Dive

Salesforce supports **fully translated UI** in many languages. The Language setting changes:
- All menu labels (Home, Accounts, Reports)
- All standard button text (Save, Edit, Delete)
- All standard field labels
- System error messages
- Picklist values (if translated)

**What Language does NOT change:**
- Custom labels you created in English (unless you added translations in Translation Workbench)
- Data you've entered in records
- Custom picklist values (unless translated)

**Translation Workbench:**
For global orgs, admins use the Translation Workbench to translate custom labels, picklist values, and other custom text into multiple languages.

---

## 2.13 Single Sign-On (SSO) — Overview

**Single Sign-On (SSO)** allows users to authenticate once (to an Identity Provider) and then access Salesforce without re-entering credentials.

### Why SSO Matters for User Management

- Users don't need a separate Salesforce password — they use their corporate credentials
- Password resets happen in the corporate directory (Active Directory, Okta, etc.) — not Salesforce
- When an employee is terminated and their corporate account is disabled, they automatically lose Salesforce access too — no need to remember to deactivate in Salesforce
- Admins can centrally manage all user access from one place

### SSO Types

| Type | How It Works |
|------|-------------|
| **SAML-based SSO** | User authenticates to Identity Provider (e.g., Okta, Azure AD) → IdP sends a SAML token to Salesforce → Salesforce creates/validates session |
| **OAuth-based SSO** | User authorizes access via OAuth flow (used for Connected Apps, mobile apps) |
| **Salesforce as Identity Provider** | Other apps (not Salesforce) use Salesforce to authenticate users |

### User Provisioning with SSO

**Just-In-Time (JIT) Provisioning:**
When a user with SSO logs into Salesforce for the first time, Salesforce can automatically CREATE their user record from attributes in the SAML assertion — no manual user creation needed.

**Real-World Example:**
> "TechCorp" uses Azure Active Directory (AAD) as their Identity Provider. When a new employee joins:
> 1. IT creates them in Azure AD
> 2. First time they click the Salesforce tile in their company portal → AAD sends SAML assertion to Salesforce
> 3. Salesforce automatically creates the user record with the correct profile and role (from AAD attributes)
> 4. User is in Salesforce — IT admin never manually created them

---

## ✅ Hands-On Tasks

> All tasks performed in your **Trailhead Playground**. Each task builds on the previous.

---

### 🔧 Task 1 — View Current License Usage

**Objective:** Understand what licenses your org has and how many are in use.

**Steps:**
1. Setup → Company Information
2. Scroll down to the **User Licenses** section
3. Note the table showing:
   - License Name
   - Status
   - Total Licenses
   - Used Licenses
   - Remaining Licenses
4. Find the "Salesforce" license row — note Total, Used, Remaining
5. Find the "Chatter Free" license — how many are available?

**Analysis question:** If your org has 10 Salesforce licenses and 9 are in use, how many more users can you create with full CRM access before needing to purchase more?

---

### 🔧 Task 2 — Create Multiple User Types

**Objective:** Create users with different license types and understand the impact.

**Create User A — Sales Rep:**
1. Setup → Users → Users → **New User**
2. Fill in:
   - First Name: `Sales`, Last Name: `Rep One`
   - Email: your email address
   - Username: `salesrep1@[yourplayground].trailhead` (make it unique)
   - Alias: `srep1`
   - User License: `Salesforce`
   - Profile: `Standard User`
   - Role: leave blank
3. Save → note the user was created

**Create User B — Limited Access:**
4. New User again
5. Fill in:
   - First Name: `Platform`, Last Name: `User One`
   - Username: `platformuser1@[yourplayground].trailhead`
   - User License: `Salesforce Platform` (if available in your Playground; if not, create a second Standard User)
   - Profile: note which profiles are available for this license type
6. Save

**Compare the users:**
7. Log in as Sales Rep One (using Login As)
8. Note what tabs and objects they can access
9. Return to admin → log in as Platform User One
10. Note what tabs and objects are available — what's missing compared to Sales Rep?

---

### 🔧 Task 3 — Assign a Feature License

**Objective:** Enable a Feature License for a specific user.

**Steps:**
1. Setup → Users → Users → click on `Sales Rep One`
2. Click **Edit**
3. Scroll to the **"Feature License Assignments"** section
4. Look for available feature licenses (may include Chatter Answers, Knowledge User, etc.)
5. Check any available feature license
6. Save

**Alternative — check from Feature License perspective:**
7. Setup → Users → Users → click **Permission Set Licenses** tab
8. See which PSLs are available in the org

---

### 🔧 Task 4 — Explore Profile Differences

**Objective:** Understand how profiles control access.

**Steps:**
1. Setup → Profiles → click **Standard User**
2. Note the following in Standard User:
   - Object Permissions: check Lead → what CRUD is allowed?
   - Object Permissions: check Opportunity → what CRUD?
   - System Permissions: is "Export Reports" enabled?
   - System Permissions: is "Manage Users" enabled?

3. Now open **System Administrator** profile
4. Compare:
   - Lead CRUD — what can Sys Admin do that Standard User can't?
   - System Permissions — list 5 things Sys Admin has that Standard User doesn't

**Create a table:**
| Permission | Standard User | System Admin |
|-----------|--------------|--------------|
| Delete Leads | ❌/✅ | ❌/✅ |
| Export Reports | ❌/✅ | ❌/✅ |
| Manage Users | ❌/✅ | ❌/✅ |
| View All Data | ❌/✅ | ❌/✅ |
| Customize Application | ❌/✅ | ❌/✅ |

---

### 🔧 Task 5 — Test Login-As and Audit Trail

**Objective:** Use Login Access to troubleshoot, and verify audit trail.

**Setup first:**
1. Setup → Security → **Login Access Policies**
2. Make sure "Administrators Can Log in as Any User" is enabled
3. Save if you changed anything

**Perform the Login-As:**
4. Setup → Users → Users → find `Sales Rep One`
5. Click **"Login"** next to their name
6. You are now in Sales Rep One's session — note the banner
7. Try to navigate to Setup — observe you have limited access
8. Go to Accounts → try to view a record
9. Note what you can and cannot do
10. Click **"Return to [Your Name]"**

**Check Audit Trail:**
11. Setup → Security → **View Setup Audit Trail**
12. Look for your Login-As action at the top
13. Note: what exact information is captured?

---

### 🔧 Task 6 — Configure Password Policy

**Objective:** Set org-wide password policy.

**Steps:**
1. Setup → Security → **Password Policies**
2. Note existing settings
3. Change:
   - User passwords expire in: **90 days**
   - Enforce password history: **3 passwords remembered**
   - Minimum password length: **10**
   - Complexity requirement: **Must mix alpha and numeric**
   - Maximum login attempts: **5**
   - Lockout effective period: **30 minutes**
4. Save

**Discussion:** Why would you set different password policies for different profiles? (Admin vs standard user)

---

### 🔧 Task 7 — Freeze and Deactivate a User

**Objective:** Practise all three user status changes.

**Freeze Test:**
1. Setup → Users → Users → find `Sales Rep One`
2. Click the user → Edit → check **Frozen** → Save
3. Try to Login As them → you should be blocked with a "user is frozen" message
4. Return → Edit → uncheck Frozen → Save
5. Confirm Login As works again

**Deactivate Test:**
6. Edit `Sales Rep One` → uncheck **Active** → Save
7. Return to Users list → can you find them? (Use filter: "All Users" not "Active Users")
8. Try Login As → blocked
9. Reactivate: Edit → check Active → Save

**Observe:** After deactivation, the user still appears in the system and their records still exist. Only their access is blocked.

---

### 🔧 Task 8 — Configure Session Settings

**Objective:** Implement a session security policy.

**Steps:**
1. Setup → Security → **Session Settings**
2. Configure:
   - Session timeout: **2 Hours**
   - Check: **"Force logout on session timeout"** ✅
   - Check: **"Require secure connections (HTTPS)"** ✅
   - Check: **"Retain admin session after logging out as another user"** ✅
   - Check: **"Enable clickjack protection for customer Visualforce pages..."** ✅
3. Save

**Test the Retain Admin Session setting:**
4. Log in as Sales Rep One
5. Click Return to Admin
6. Are you still logged in as admin? (Should be yes, with this setting enabled)
7. Temporarily UNCHECK the "Retain admin session" option → Save
8. Log in as Sales Rep One again → Return to Admin
9. Are you still logged in? (Should be NO — logged out completely)
10. Re-enable the setting

---

### 🔧 Task 9 — Explore User Locale Settings

**Objective:** Understand how locale affects date and number display.

**Steps:**
1. Open your own user record: click avatar top-right → Settings → Personal → **Language & Time Zone**
2. Note your current settings: Time Zone, Locale, Language
3. Change Time Zone to `America/New_York (EST)` → Save
4. Navigate to Accounts → open a record with a date field
5. Note how the timestamp on recent activities changed
6. Change Time Zone back to your original → Save

**Research task:**
7. What is the date format for English (United Kingdom) locale?
8. What is the date format for English (India) locale?
9. If you enter `01/02/2026` with US locale, what date does this represent?
10. If the same entry was made with UK locale, what date does this represent?

---

## 📝 Practice Questions

**Q1.** A company has 100 users. 70 are sales reps needing full CRM access. 30 are factory floor workers using a custom `Production_Log__c` app only — they never touch Leads or Opportunities. What licenses do you assign to each group, and why?

**Q2.** What is the globally unique constraint on Salesforce usernames? Why does this constraint exist?

**Q3.** You create a user in Production with username `john.smith@acme.com`. You try to create a sandbox user with the same username. What happens and how do you fix it?

**Q4.** What is the difference between a Feature License and a Permission Set License? Give one example of each.

**Q5.** A user, Priya, leaves the company. What is the correct action to take — Freeze or Deactivate? List three things that happen to her records and one thing that does NOT happen automatically.

**Q6.** An admin logs in as a user, Tom, to investigate a permissions issue. Where is this action logged, and what information is captured in the log?

**Q7.** What are Login Hours on a Profile? Give a real-world scenario where you would configure them.

**Q8.** What is the difference between the **Email** field and the **Username** field on a User record? Can they be the same value?

**Q9.** A company enables the "Lock sessions to IP address" Session Setting. A sales rep starts work on the office WiFi, then moves to their phone's hotspot. What happens to their Salesforce session?

**Q10.** What is Delegated Administration? Give a scenario where a company with 50 regional offices would use it.

**Q11.** Salesforce stores all timestamps in UTC internally. A meeting is created at 10:00 AM by a user in Mumbai (IST = UTC+5:30). What time does a user in New York (EST = UTC-5) see for the same meeting?

**Q12.** A user's account has been frozen. Does this free up their license for reassignment to another user?

**Q13.** What is the purpose of the "Enforce password history" setting in Password Policies?

**Q14.** When would you use Just-In-Time (JIT) provisioning with SSO?

**Q15.** An admin at "RetailCo" wants to give regional HR managers the ability to create users and reset passwords for employees in their region, but without giving them full System Administrator access. What Salesforce feature enables this?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — License Architecture for a Mixed Org**

> "HealthFirst" hospital is implementing Salesforce with the following user groups:
>
> | Group | Count | What They Need |
> |-------|-------|---------------|
> | Doctors | 120 | Custom Patient App (custom objects only), no standard CRM |
> | Nurses | 80 | Same custom Patient App as doctors |
> | Admin staff | 40 | Custom Appointment Scheduler app, need to run Reports |
> | Sales reps (pharma partnerships) | 25 | Full CRM — Leads, Accounts, Opportunities, Campaigns |
> | Marketing team | 5 | Full CRM + Campaign management |
> | Support agents | 30 | Cases, Knowledge, Service Console |
> | Senior executives | 10 | Full dashboards, reports, CRM visibility |
> | External lab partners | 15 | Need to collaborate via Chatter only — no CRM data |
>
> **Questions:**
> 1. What license type would you assign to each group?
> 2. What feature licenses are needed, and for which users?
> 3. If a full Salesforce license costs ₹12,000/user/month and Platform costs ₹2,000/user/month, calculate the monthly savings from correct license allocation vs assigning everyone a full Salesforce license.
> 4. Which group might benefit from a Permission Set License instead of a full license upgrade?

---

**Scenario 2 — Security Incident: Employee Data Breach**

> On Tuesday morning, the IT security team at "TechCorp" receives an alert from their SIEM (Security Information and Event Management) tool: "Salesforce user `arun.kumar@techcorp.com` has exported 15,000 records via Data Loader between 11 PM and 2 AM last night."
>
> Arun Kumar is a senior sales analyst. It is suspected he may be preparing to leave the company and take customer data.
>
> **Questions:**
> 1. What is the FIRST action the Salesforce admin should take, and why specifically that action over the alternatives?
> 2. What Setup tool would you check immediately to see a full log of Arun's recent Salesforce activity?
> 3. The legal team says: "Do not touch his records — they are evidence." What does this mean for record transfer?
> 4. Three weeks later, the investigation concludes — Arun is guilty. What is now the correct admin action?
> 5. What preventive measures would you recommend implementing to detect or prevent this type of incident in the future?

---

**Scenario 3 — Global Username Strategy**

> "GlobalBank" operates in India, UK, UAE, and Singapore. They are implementing Salesforce and will have:
> - Production org (live banking system)
> - Full Sandbox (UAT environment)
> - 2 Developer Sandboxes (for Dev Team A and Dev Team B)
>
> They onboard an average of 30 new employees per month globally. Previous admins have already created username conflicts multiple times during sandbox creation.
>
> **Questions:**
> 1. Design a comprehensive username naming convention that prevents all conflicts
> 2. Show example usernames for: Rahul Mehta (India), Sarah Johnson (UK), Ahmed Al-Farsi (UAE), Wei Chen (Singapore) — across all 4 environments
> 3. What process would you implement for the monthly onboarding of 30 new employees to ensure consistency?
> 4. What happens to a user's username if the Production org and sandbox are ever merged or reorganized?

---

**Scenario 4 — Session Policy for a Bank**

> The Chief Information Security Officer (CISO) at "PremiumBank" hands you a security requirements document:
>
> **Requirements:**
> - Loan officers must be logged out after 10 minutes of inactivity
> - All Salesforce access must be from within the corporate office IP range (192.168.1.0/24) or the bank's VPN (10.0.0.0/8)
> - System Administrators must be reachable for emergencies — they should be able to log in from anywhere at any time
> - If a user connects from outside the approved IP range using a compromised credential, the session must be invalidated if the IP changes mid-session
> - All access must be encrypted
> - Password minimum: 12 characters, must include uppercase, lowercase, number, and special character, expires every 60 days
> - Admins logging in as users to troubleshoot should not be forced to re-authenticate when returning to their own session
>
> **Questions:**
> 1. For each requirement, identify the exact Salesforce security setting and the configuration value
> 2. Are there any requirements that cannot be fully met through standard Salesforce configuration alone?
> 3. The CISO asks: "How do we ensure that when a loan officer is terminated, their Salesforce access is revoked within 5 minutes, even if the HR team forgets to notify IT?" — what feature addresses this?
> 4. What would you recommend for the IP restriction on System Administrators specifically?

---

**Scenario 5 — Delegated Administration Rollout**

> "RetailGroup" has Salesforce deployed across 40 retail stores nationally. Each store has:
> - 1 Store Manager
> - 5–15 Store Associates (Salesforce users)
> - High turnover — associates join and leave frequently
>
> The central IT team of 2 Salesforce Admins is spending 60% of their time on user creation, password resets, and account deactivations for store associates. This leaves no time for development and improvement work.
>
> **Questions:**
> 1. Design a Delegated Administration solution for this scenario — what permissions would you give Store Managers?
> 2. What permissions would you NOT give Store Managers, and why?
> 3. Walk through the exact Setup steps to configure Delegated Administration for one store (Store #001 — Manager: Priya Mehta)
> 4. What risks does Delegated Administration introduce, and how would you mitigate them?
> 5. After implementing this, how would you measure whether it has reduced the IT team's workload?

---

## ✔️ Answer Key — Practice Questions

1. **70 sales reps → Salesforce license** (need Leads, Opportunities, full CRM). **30 factory workers → Salesforce Platform license** (only need custom objects — specifically `Production_Log__c` — no standard CRM objects). Platform license is significantly cheaper, saving 30 × (price difference) per month.

2. Salesforce usernames must be **unique across all Salesforce orgs globally** — not just within your company's org. This constraint exists because Salesforce is a multitenant platform where all customers share the same authentication infrastructure. When a user logs in at login.salesforce.com, Salesforce uses the username to uniquely identify which org to route them to. If two orgs had the same username, the system couldn't determine which org the user belongs to.

3. **Error: "This username already exists"** — because usernames are global and `john.smith@acme.com` is already taken by the Production user. Fix: append a sandbox-specific suffix to the username, e.g., `john.smith@acme.com.sandbox` or `john.smith@acme.com.uat`.

4. A **Feature License** is assigned directly on the User record (via checkboxes) and unlocks a bundle of related features for a role — e.g., "Marketing User" unlocks Campaign management. A **Permission Set License (PSL)** is assigned through a Permission Set and unlocks very specific premium features — e.g., "Einstein Analytics" PSL gives access to Tableau CRM. Feature License example: Knowledge User. PSL example: Einstein Prediction Builder.

5. **Deactivate** — because she has permanently left. Three things that happen: (1) Her license is freed for reuse, (2) Her records remain in the org with her listed as owner, (3) Her name remains in audit trail history for all changes she made. One thing that does NOT happen automatically: **record ownership is NOT transferred** — the admin must manually run Mass Transfer Records to reassign her Opportunities, Cases, etc. to another user.

6. The action is logged in the **Setup Audit Trail** (Setup → Security → View Setup Audit Trail). The log captures: (a) The exact date and time of the login-as action, (b) Which administrator performed the login, (c) Which user was impersonated, (d) The type of action recorded as "User" and "Logged in as user."

7. **Login Hours** restrict when users with a specific profile can log in to Salesforce — outside these hours, they receive an error and cannot access the system. Real-world scenario: A call center company sets Login Hours for "Call Center Agent" profile to Monday–Friday, 8:30 AM–6:30 PM. This prevents agents from logging in on weekends or outside shift hours, reducing the risk of unauthorized off-hours access.

8. The **Email** field stores the user's real email address — this is where Salesforce sends password reset links, notifications, and system emails. The **Username** field is the login identifier — it must be formatted like an email but doesn't need to be a real email address. Yes, they CAN be the same value, and often are in simple setups. They can also be completely different (e.g., email = `priya@company.com`, username = `priya.sharma.prod@company.com`).

9. The rep's **Salesforce session is immediately terminated**. They are logged out and must re-authenticate from the new network. This is by design — the setting protects against session hijacking, where an attacker could potentially steal a session token and use it from a different network. The trade-off is inconvenience for users who legitimately switch networks during the workday.

10. **Delegated Administration** allows non-admin users to perform specific user management tasks (create users, reset passwords, unlock accounts) for a defined subset of users — without having full System Administrator access. Scenario: A company with 50 regional offices makes each regional HR manager a Delegated Administrator with rights to create/manage users only in their region. Central IT sets the rules; regional managers execute within those rules. This dramatically reduces IT's user management workload.

11. Mumbai creates meeting at 10:00 AM IST. IST = UTC+5:30, so Salesforce stores this as **4:30 AM UTC**. New York (EST = UTC-5) sees: 4:30 AM UTC - 5 hours = **11:30 PM previous day EST**. (Or in EDT = UTC-4: 12:30 AM EST the same night.)

12. **No.** Freezing a user does NOT free up their license. The user account still exists and still counts against your license total. To free the license, you must **Deactivate** the user.

13. "Enforce password history" prevents users from **reusing recent passwords**. When set to, for example, 5 passwords remembered — a user cannot change their password back to any of their last 5 passwords. This prevents the common security bypass of changing a password and immediately changing it back to the old one to circumvent password expiration.

14. **JIT (Just-In-Time) Provisioning** is used when a company uses SSO and wants Salesforce user records to be created **automatically** the first time a user logs in via SSO — without an admin manually creating the user in Salesforce first. It's ideal for large organizations where users are managed in a central directory (Azure AD, Okta) and administrators don't want to maintain a separate manual process for Salesforce user creation. The SSO SAML assertion contains attributes (name, email, profile mapping) that Salesforce uses to auto-create the user record.

15. **Delegated Administration** — this feature allows admins to grant regional HR managers specific user management capabilities (create users, reset passwords) for employees in their region only, without giving them full System Administrator access.

---

## 🔗 Trailhead Resources

- [Salesforce Licensing](https://trailhead.salesforce.com/content/learn/modules/salesforce-licensing)
- [User Setup & Management](https://trailhead.salesforce.com/content/learn/modules/lex_implementation_user_setup_mgmt)
- [Identity Basics](https://trailhead.salesforce.com/content/learn/modules/identity_basics)

---

*Previous: [01 · CRM & Salesforce Introduction](./01_CRM_and_Salesforce_Introduction.md) | Next: [03 · Data Modeling & Objects →](./03_Data_Modeling_and_Objects.md)*
