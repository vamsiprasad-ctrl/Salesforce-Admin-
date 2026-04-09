# 11 · Experience Cloud (Communities)

> **Module:** Experience Cloud | **Difficulty:** 🟡 Intermediate | **Est. Time:** 5 hrs

---

## 11.1 What is Experience Cloud?

**Experience Cloud** (formerly Community Cloud) is Salesforce's platform for building **branded digital portals** that connect external users — customers, partners, or employees — directly to your Salesforce data.

Instead of logging into Salesforce directly (which requires an internal license and full Salesforce access), external users get their own **lightweight, branded portal** with access only to what they need.

### Why Experience Cloud?

| Problem | Experience Cloud Solution |
|---------|--------------------------|
| Customers can't see their own case status | Customer portal: self-service case tracking |
| Partners need to register deals but shouldn't see your whole CRM | Partner portal: deal registration with limited access |
| Employees need an internal help center | Employee community: FAQ + ticket submission |
| Public needs to find answers without logging in | Help Center: public Knowledge Base |

---

## 11.2 Community Types & Templates

Salesforce provides pre-built **templates** to get you started quickly. Each template is optimized for a specific use case.

| Template | Best For | Key Features |
|----------|---------|-------------|
| **Customer Account Portal** | B2B customer self-service | Cases, Accounts, Contacts, Knowledge |
| **Partner Central** | Channel partner management | Deal Registration, Lead Distribution, Collaboration |
| **Help Center** | Customer FAQ / support | Public Knowledge Base, optional login |
| **Minimal** | Starting from scratch | Blank canvas — build everything yourself |
| **Build Your Own (LWR)** | Developer-built custom portals | Most flexible, uses Lightning Web Runtime |

### Real-World Example

> **Company:** "CloudSoft" builds three separate communities:
>
> 1. **Customer Portal** (Customer Account Portal template)
>    - Customers log in to: view their Cases, open new Cases, read Knowledge Articles, manage their account details
>
> 2. **Partner Hub** (Partner Central template)
>    - Reseller partners log in to: register deals, access sales collateral, submit leads, track their pipeline
>
> 3. **Help Center** (Help Center template)
>    - Public — no login required
>    - Anyone can search the Knowledge Base to find answers before contacting support
>    - Deflects 40% of support cases

---

## 11.3 Creating a Community

**Path:** Setup → Digital Experiences → All Sites → New

### Basic Steps

1. **Choose a template** — determines the starting layout and features
2. **Name your community** — becomes part of the URL: `yourcompany.my.site.com/partnerhub`
3. **Open Experience Builder** — the drag-and-drop page designer
4. **Configure pages** — home page, login page, case list, knowledge base, etc.
5. **Set up members** — which profiles can access this community
6. **Configure navigation** — what appears in the menu
7. **Brand it** — logo, colors, fonts
8. **Publish** — go live

---

## 11.4 Experience Builder

**Experience Builder** is the drag-and-drop page designer for Experience Cloud — similar to Lightning App Builder but for external-facing pages.

### Page Types

| Page Type | Description |
|-----------|-------------|
| **Home** | Landing page after login |
| **Object Pages** | List/Detail pages for objects exposed to community (Cases, Accounts) |
| **Record Detail Page** | Shows a specific record's details |
| **Knowledge Article Page** | Displays a single article |
| **Login/Register Pages** | Authentication pages |
| **Custom Pages** | Any page you design from scratch |

### Real-World Example

> **Customer Portal Home Page Layout:**
>
> - **Top banner:** "Welcome back, {User.FirstName}!" + company logo
> - **Quick Actions section:** "Submit a Case" | "Track My Cases" | "Find an Answer"
> - **My Open Cases component:** Table of the user's open cases with status
> - **Featured Knowledge Articles:** Top 5 most-viewed articles this week
> - **Account Info:** Key account details at a glance

---

## 11.5 Community Members & Licenses

### Who can access a community?

Users must have:
1. A **Community License** (specific to Experience Cloud — cheaper than internal licenses)
2. A **Profile** that is enabled for the community

### Community License Types

| License | Access Level | Pricing Model |
|---------|-------------|---------------|
| **External Apps** | Up to 10 custom objects, basic community features | Per member/month |
| **Customer Community** | Standard customer self-service features | Per member/month or per login |
| **Customer Community Plus** | + Reporting, delegated admin, more custom objects | Higher price |
| **Partner Community** | Full partner features — deal registration, lead distribution | Per member/month |

### Login vs Member Licensing

| Type | Pays for | Best for |
|------|---------|---------|
| **Member** | Each named user, regardless of usage | High-frequency users |
| **Login** | Each login session (bundles available) | Occasional users |

### Real-World Example

> **"CloudSoft" has 5,000 customers in their portal.**
> - 500 are power users who log in daily → **Member licenses** ($2/month each = $1,000/month)
> - 4,500 log in less than once a month → **Login licenses** ($0.25/login, 10 logins bundled = $2.50/month each = $11,250/month)
>
> Total: ~$12,250/month vs $25,000/month if all were on Member licenses.

---

## 11.6 Security & Sharing in Communities

External users are still Salesforce users — they are subject to all the same security layers described in Section 04.

### How Community Security Works

| Layer | Community Equivalent |
|-------|---------------------|
| Org Access | Community URL + login credentials |
| Object Access | Community profile — controls which objects they can see |
| Field Access | FLS on the community profile — which fields are visible |
| Record Access | OWD + Community Sharing (Sharing Sets, Share Groups) |

### Community-Specific Sharing

**Sharing Sets** — Grant community users access to records linked to their Account or Contact automatically.

### Real-World Example

> **Customer Portal Sharing:**
>
> OWD for Case: **Private**
>
> Without any sharing config: customer logs in but sees **zero cases** — because Private OWD means only the owner (the internal agent) can see the case.
>
> **Sharing Set:** "Grant access to Cases where `Case.AccountId = User.AccountId`"
>
> Now each customer user automatically sees all Cases belonging to their company — and only their company. A customer at Acme Corp cannot see TechStart's cases.

### Guest User Access

A **Guest User** is an unauthenticated visitor to a public community. They have their own profile (`[Community Name] Profile (Guest User)`) with very restricted access.

### Real-World Example

> **Help Center public Knowledge Base:**
>
> Guest User profile is given:
> - Read access to Knowledge Articles (public articles only)
> - No access to Cases, Accounts, Contacts, or any CRM data
>
> Anyone can search and read articles. Nobody can see any customer data without logging in.

---

## 11.7 Community Roles

Experience Cloud has its own **Role Hierarchy** for external users — separate from the internal employee role hierarchy.

### Role Structure

```
[Community] Executive
        │
[Community] Manager
        │
[Community] User
```

Community roles are tied to the Account the community user belongs to. This allows account-level data sharing — a manager at Acme Corp can see cases submitted by all Acme Corp users, but not cases from TechStart.

---

## 11.8 Branding & Theming

**Path:** Experience Builder → Branding → Branding Editor

### What you can control

| Element | Options |
|---------|---------|
| **Logo** | Upload your company logo |
| **Color Palette** | Primary, Secondary, Tertiary colors |
| **Button Style** | Rounded, rectangular, pill-shaped |
| **Typography** | Font family, sizes |
| **Background** | Header image, body color |
| **Custom CSS** | Full CSS override for advanced styling |

### Real-World Example

> **"CloudSoft" Customer Portal branding:**
> - Logo: CloudSoft cloud logo
> - Primary color: #0070D2 (Salesforce blue → CloudSoft blue: #2ECC71)
> - Font: Inter (modern, clean)
> - Header: Full-width gradient with cityscape image
> - Buttons: Rounded with CloudSoft green color
>
> Result: Customers feel they're on CloudSoft's website — not Salesforce.

---

## 11.9 Audience Targeting

**Audience Targeting** lets you show different content on the same page to different groups of users — based on profile, location, user attributes, or record fields.

### Real-World Example

> **Community Home Page — same URL, different content:**
>
> | Audience | What they see |
> |----------|---------------|
> | Enterprise customers (Account Type = Enterprise) | "Your dedicated support team is available 24/7. Call us: 1-800-555-CLOUD" + enterprise-specific widgets |
> | Standard customers | Standard FAQ links + "Submit a Case" button |
> | Partners | "Latest deal registration deadline: May 31" + partner resource links |
> | Guest (not logged in) | "Log in to access support" + public Knowledge Base only |
>
> All on the same home page — no separate pages needed.

---

## 11.10 Community Search

**Community Search** allows members to search for content across the community — Knowledge Articles, discussions, records, and pages.

### Real-World Example

> **Customer searches "cannot login":**
>
> Search results:
> 1. Knowledge Article: "How to reset your password" ⭐ Top result
> 2. Knowledge Article: "Supported browsers for CloudSoft"
> 3. Forum discussion: "Same issue — solved by clearing cache"
>
> Customer finds the answer without submitting a case. **Case deflection** — the holy grail of self-service support.

---

## 11.11 Building a Community with Knowledge & Chat

Combining **Knowledge** (articles) + **Live Chat** (Salesforce Messaging for Web) creates a powerful self-service + assisted service experience.

### Real-World Example

> **Customer visits the Help Center:**
>
> 1. Searches for "export report" → finds 3 relevant articles → resolves issue independently ✅
>
> If not resolved:
>
> 2. Clicks "Chat with Support" → Live chat opens in the same window
> 3. Bot greets them: "Hi Sarah! I see you were reading about report export. Do you need help with that?"
> 4. If bot can't resolve → escalates to live agent who can see the articles Sarah already read
> 5. Agent picks up the conversation with full context — no "can you describe your issue again?"

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Experience Cloud | Platform for building external portals — customers, partners, employees |
| Templates | Pre-built starting points — Customer Account Portal, Partner Central, Help Center |
| Experience Builder | Drag-and-drop page designer for community pages |
| Community License | Required for all external users — cheaper than internal licenses |
| Sharing Set | Auto-shares records with community users based on Account/Contact link |
| Guest User | Unauthenticated visitor — very restricted access (public content only) |
| Community Roles | Separate role hierarchy for external users — account-level data sharing |
| Audience Targeting | Different content for different user groups on the same page |
| Case Deflection | Customers find answers in Knowledge before submitting cases — reduces support volume |

---

## 🔗 Trailhead Resources

- [Community Cloud Basics](https://trailhead.salesforce.com/content/learn/modules/community_cloud_basics)
- [Community Search Basics](https://trailhead.salesforce.com/content/learn/modules/comm_search_basics)
- [Share CRM Data with Communities](https://trailhead.salesforce.com/content/learn/projects/communities_share_crm_data)
- [Personalize Audiences in Communities](https://trailhead.salesforce.com/content/learn/projects/communities_personalize_audiences)
- [Build a Community with Knowledge & Chat](https://trailhead.salesforce.com/content/learn/projects/build-a-community-with-knowledge-and-chat)
- [Community Theme & Layout](https://trailhead.salesforce.com/content/learn/projects/communities_theme_layout)

---

*Previous: [10 · Service Cloud & Knowledge](./10_Service_Cloud_and_Knowledge.md)*

---

## You've completed the full curriculum!

| Next Step | Link |
|-----------|------|
| Salesforce Certified Administrator exam | [Exam Guide](https://trailhead.salesforce.com/credentials/administrator) |
| Service Cloud Consultant exam | [Exam Guide](https://trailhead.salesforce.com/credentials/servicecloudconsultant) |
| Experience Cloud Consultant exam | [Exam Guide](https://trailhead.salesforce.com/credentials/experiencecloudconsultant) |
