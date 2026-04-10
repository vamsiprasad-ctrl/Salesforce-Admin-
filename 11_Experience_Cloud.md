# 11 · Experience Cloud (Communities) — Complete Deep Guide

> **Module:** Experience Cloud | **Difficulty:** 🟡 Intermediate | **Est. Time:** 7 hrs
> **Covers:** What is Experience Cloud · Templates · Licenses · Creating Communities · Experience Builder · Security model · Sharing Sets · Share Groups · Guest Users · Community Roles · Branding · Audience Targeting · Community Search · Moderation · Reports & Dashboards · Headless · Certification path

---

## Table of Contents

1. [What is Experience Cloud?](#111-what-is-experience-cloud)
2. [Why Experience Cloud Exists](#112-why-experience-cloud-exists)
3. [Experience Cloud Templates](#113-experience-cloud-templates)
4. [Community License Types](#114-community-license-types)
5. [Creating an Experience Cloud Site](#115-creating-an-experience-cloud-site)
6. [Experience Builder — Deep Dive](#116-experience-builder--deep-dive)
7. [Community Members and Access](#117-community-members-and-access)
8. [Security in Experience Cloud](#118-security-in-experience-cloud)
9. [Sharing Sets](#119-sharing-sets)
10. [Share Groups](#1110-share-groups)
11. [Guest Users](#1111-guest-users)
12. [Community Roles](#1112-community-roles)
13. [Branding and Theming](#1113-branding-and-theming)
14. [Audience Targeting](#1114-audience-targeting)
15. [Community Search](#1115-community-search)
16. [Case Deflection and Knowledge Integration](#1116-case-deflection-and-knowledge-integration)
17. [Moderation](#1117-moderation)
18. [Reports and Dashboards in Communities](#1118-reports-and-dashboards-in-communities)
19. [Headless Experience Cloud](#1119-headless-experience-cloud)
20. [Hands-On Tasks](#-hands-on-tasks)
21. [Practice Questions](#-practice-questions)
22. [Scenario-Based Questions](#-scenario-based-questions)
23. [Answer Key](#️-answer-key)

---

## 11.1 What is Experience Cloud?

**Experience Cloud** (formerly known as Community Cloud, rebranded in 2021) is Salesforce's platform for building **branded digital portals** that connect external users — customers, partners, dealers, employees — directly to your Salesforce data.

The fundamental idea: instead of giving external users a full internal Salesforce license (expensive, gives access to everything), you build a targeted, branded portal where external users see ONLY what is relevant to them — at a fraction of the cost.

### The Before / After

**Before Experience Cloud:**
> "InsureCo" has 50,000 policyholders. When a policyholder wants to check their claim status:
> 1. They call the support center
> 2. Agent answers, verifies identity (2 minutes)
> 3. Agent searches for the claim in Salesforce (1 minute)
> 4. Agent reads out the status (1 minute)
> 5. Call ends — total: 4–5 minutes × 50,000 claims/month = 200,000 support agent-minutes/month
>
> Cost: ~₹15,00,000/month in agent time just for status checks

**After Experience Cloud (Customer Portal):**
> Policyholders log into InsureCo's branded portal (powered by Salesforce Experience Cloud).
> They see their own policies and claims — directly from Salesforce — without agent involvement.
> Self-service resolves 60% of status inquiries.
>
> Cost reduction: ₹9,00,000/month saved
> Agent time freed: Available for complex, high-value interactions

---

## 11.2 Why Experience Cloud Exists

### The Three Use Cases

#### 1. Customer Self-Service
Customers handle their own common tasks — check order status, submit support tickets, read knowledge articles, update account information — without calling support.

**Industries:** E-commerce, Insurance, Banking, Healthcare, Telecom, SaaS

**Real-World Example:**
> "Airtel" — customers log into the Airtel portal to:
> - View current bill and payment history
> - Raise complaints and track ticket status
> - Change plan, add services
> - Chat with support if self-service doesn't resolve the issue
> All directly connected to Airtel's Salesforce data.

#### 2. Partner Relationship Management
Give channel partners (resellers, distributors, agents) a dedicated workspace to collaborate with you — register deals, access leads, download sales materials, check commission status.

**Industries:** Manufacturing, Technology, Financial Services, Insurance

**Real-World Example:**
> "Dell" has 10,000 reseller partners worldwide. Through the Dell Partner Portal:
> - Partners register deals to claim protected pricing
> - Dell assigns leads to partners in their territory
> - Partners access marketing materials, product training, certification paths
> - Partners track their quarterly performance and incentive payouts
> All managed through Salesforce Experience Cloud.

#### 3. Employee Communities
Internal employee portals — not the full Salesforce org — for specific audiences like field service technicians, part-time staff, or employees who don't need full Salesforce access.

**Real-World Example:**
> A hospital has 500 full-time employees on Salesforce licenses and 2,000 contractual nurses, technicians, and administrators who need limited access:
> - Submit leave requests
> - Check duty rosters
> - Access internal knowledge base
> - Submit expense reports
> Built on Experience Cloud — no Salesforce licenses needed for the 2,000 contractors.

---

## 11.3 Experience Cloud Templates

Salesforce provides pre-built **templates** as starting points. Each template comes with pre-configured pages, navigation, and components optimized for a specific use case.

### Customer Account Portal

**Purpose:** Customer self-service for B2B relationships.

**Pre-built pages:** Home, My Cases, Account Info, Knowledge Search, Case Detail, File Upload

**Key features:**
- Case management — submit, track, comment on cases
- Account and Contact information viewing and editing
- Knowledge Base browsing and search
- File upload capability
- Chatter discussion threads

**Who uses it:** B2B customers who have an Account relationship with you

**Real-World Example:**
> "Tally Solutions" — a business software company builds a Customer Account Portal. Their business customers (Accountants, CAs, businesses using Tally software) log in to:
> - Submit support tickets when Tally isn't working
> - Track existing tickets
> - Download product updates and documentation
> - Access training articles

---

### Help Center

**Purpose:** Public-facing Knowledge Base — no login required.

**Pre-built pages:** Home with search, Article detail page, Topic browsing

**Key features:**
- Publicly accessible — no user account needed
- Salesforce Knowledge Article display
- Full-text search across articles
- Topic/category navigation
- Optional: "Escalate to Case" button for logged-in users

**Who uses it:** Anyone who finds your portal through Google, your website, or direct link

**Real-World Example:**
> Salesforce's own help.salesforce.com — you don't need to log in to search help articles. This is essentially a Help Center template deployment. Google indexes these articles, reducing support volume from search traffic.

---

### Partner Central

**Purpose:** Channel partner collaboration and deal management.

**Pre-built pages:** Home, Deal Registration, Opportunities, Leads, Reports, Files, Training

**Key features:**
- Deal Registration — partners submit deals for protected pricing
- Lead Distribution — you push leads to appropriate partners
- Opportunity collaboration — partners manage their pipeline
- Partner Fund Management — track MDF (Market Development Funds)
- Certification and training tracking
- Partner scorecard and performance reports

**Who uses it:** Resellers, distributors, agents, ISVs, system integrators

**Real-World Example:**
> "Cisco" has 50,000 certified resellers. Partner Central allows each reseller to:
> - Register new deals (protecting them from competitive resellers)
> - Request price discounts for registered deals
> - Access Cisco product training and certifications
> - Track their quarterly performance and rebate eligibility

---

### Customer Service (Napili / Aloha — Legacy)

Older templates that are still supported but Help Center / Customer Account Portal are now preferred.

---

### Build Your Own (Aura)

**Purpose:** Maximum flexibility — start with a blank canvas using the Aura component framework.

**Who uses it:** Organizations with specific design requirements that templates can't meet, or those building highly customized portals.

---

### Build Your Own (LWR — Lightning Web Runtime)

**Purpose:** Highest-performance, most modern template using Lightning Web Runtime (a different rendering engine than standard Salesforce).

**Key advantages:**
- Significantly faster page load times
- Better SEO (search engine indexing)
- Better mobile performance
- Required for headless implementations

**Who uses it:** Organizations that need superior performance, heavy SEO requirements, or complex custom builds.

---

### Microsites (LWR)

**Purpose:** Standalone, simple content sites — landing pages, campaign microsites, event pages.

**Key features:**
- Fast to build
- Optimized for content publishing
- No need for Salesforce login
- Better for marketing use cases

---

## 11.4 Community License Types

External users accessing an Experience Cloud site need a **Community license** — distinct from internal Salesforce licenses and significantly cheaper.

### License Comparison Table

| License | Monthly Cost Model | Who It's For | Key Access | Objects Accessible |
|---------|-------------------|-------------|-----------|-------------------|
| **Customer Community** | Per login OR per member | B2C customers with moderate usage | Self-service portal | 10 custom objects + standard objects |
| **Customer Community Plus** | Per login OR per member | B2B customers needing more | Reports, advanced features | 10+ custom objects + reporting |
| **Partner Community** | Per member | Channel partners | Full partner features | Unlimited custom objects |
| **External Apps** | Per login OR per member | Custom external apps | Very flexible | Up to 10 custom objects (lower tier) |
| **Channel Account** | Per member | Large-scale partner networks | Partner features | Used with channel programs |

### Per Login vs Per Member Pricing

**Per Member:** Pay a fixed monthly fee per named user, regardless of how often they log in.
- Best for: High-frequency users (log in daily/weekly)
- Example: A partner who logs in every day to check their pipeline

**Per Login:** Purchase a bundle of login "credits" that are consumed when users log in.
- Best for: Low-frequency users (log in monthly or occasionally)
- Example: A customer who only logs in when they have an issue (once every few months)

**Cost Optimization Example:**
> "CloudSoft" has 10,000 customers in their portal:
> - 500 are active businesses who log in weekly → **Per Member** licenses
> - 9,500 are end customers who log in 1–2 times a year → **Per Login** licenses (bundle of 50 logins/user/year)
>
> Per Member: 500 × ₹500/month = ₹2,50,000/month
> Per Login: 9,500 × ₹100/year = ₹9,50,000/year = ₹79,167/month
> Total: ₹3,29,167/month
>
> vs assigning all 10,000 Per Member: ₹50,00,000/month
> Savings: 93% cost reduction

---

## 11.5 Creating an Experience Cloud Site

### Prerequisites

1. **Enable Digital Experiences in your org:**
   - Setup → Digital Experiences → Settings → Enable Digital Experiences ✅
   - Choose your domain (e.g., `techbridge.my.site.com`)
   - This cannot be changed after setup — choose carefully

2. **Enable necessary features:**
   - Knowledge (if using Knowledge Base)
   - Chatter (if using community discussions)
   - Cases (if building a support portal)

### Step-by-Step Site Creation

**Path:** Setup → Digital Experiences → All Sites → **New**

**Step 1 — Choose Template:**
Select from: Customer Account Portal, Help Center, Partner Central, Build Your Own, etc.

**Step 2 — Name Your Site:**
- **Name:** "TechBridge Support Portal" (internal identifier)
- **URL Path:** `support` (becomes `techbridge.my.site.com/support`)

**Step 3 — Create:**
Experience Builder opens automatically.

### Site Status Lifecycle

| Status | Meaning | Who Can Access |
|--------|---------|---------------|
| **Preview** | Under construction | Admin users only (via admin URL) |
| **Active** | Live and published | Members based on profile/license |
| **Inactive** | Disabled | No one |

### The Site URL Structure

```
[YourDomain].my.site.com/[SitePathName]

Examples:
techbridge.my.site.com/support      → Customer Support Portal
techbridge.my.site.com/partners     → Partner Portal
techbridge.my.site.com/help         → Public Help Center
```

Custom domains can be configured:
`support.techbridge.com` → maps to `techbridge.my.site.com/support`

---

## 11.6 Experience Builder — Deep Dive

**Experience Builder** is the drag-and-drop visual editor for designing community pages. It is similar to Lightning App Builder but purpose-built for Experience Cloud.

**Path:** From Setup → Digital Experiences → All Sites → find site → **Builder**

### The Experience Builder Interface

| Area | Purpose |
|------|---------|
| **Left Panel** | Component palette, Pages list, Theme settings |
| **Canvas** | Visual page preview — drag and drop here |
| **Right Panel** | Properties of the selected component |
| **Top Bar** | Device preview toggle, Publish, Preview, Settings |

### Page Types

| Page Type | Description | Example |
|-----------|-------------|---------|
| **Standard Page** | Fixed URL pages | Home, About Us, Contact |
| **Object Pages** | Auto-generated list and detail pages for CRM objects | Cases list, Case detail |
| **Record Detail Page** | Shows one specific record's information | A specific Case record |
| **Record List Page** | Shows all records of a type the user can access | My Cases list |
| **Login / Register Pages** | Authentication entry points | Login, Self-Registration, Forgot Password |
| **Error Pages** | Handle invalid URLs, permission denied | 404 page, Access Denied |
| **Custom Pages** | Pages you design from scratch | Marketing landing page |

### Standard Components Available in Experience Builder

**Content Components:**
- Rich Content Editor — HTML content block
- Image — display an image
- Banner — hero image with text overlay
- Tile Menu — grid of clickable tiles with icons
- Accordion — expandable FAQ sections
- HTML Editor — raw HTML for advanced customization

**Data Components:**
- Record List — shows records from a Salesforce object
- Record Detail — shows detail of one record
- Related Record List — shows child records
- Record Form — create/edit a record

**Navigation Components:**
- Navigation Menu — main site navigation
- Breadcrumbs — where-am-I trail
- Featured Topics — topic grid for Knowledge

**Service Components:**
- Knowledge Article List
- Case Deflection — shows articles before user submits a case
- Feed — Chatter discussion feed
- Ask — ask a question in the community

**Utility Components:**
- Search Bar
- User Profile Menu
- Notifications

### Component Visibility Rules

Every component in Experience Builder can have **visibility rules** — conditions that control whether the component is shown to a specific user:

| Rule Type | Example |
|-----------|---------|
| **User fields** | Show only to logged-in users |
| **Profile** | Show only to users with Customer Community Plus profile |
| **Permission** | Show only to users with a specific permission set |
| **Record field** | Show a component only when a field has a specific value |
| **Device** | Show different layout on mobile vs desktop |
| **Audience** | Show based on audience segment |

---

## 11.7 Community Members and Access

### Who Can Access an Experience Cloud Site?

**Step 1:** The user must be a **Salesforce User record** — with either an internal license or a Community license.

**Step 2:** The user's **Profile** must be added to the site's membership:
- Path: Experience Builder → Administration → Members → Add profiles

**Step 3:** The user can then log in via the community URL.

### Profile Requirements for Community Users

| Community Type | Profile Type |
|---------------|-------------|
| Customer Community | Customer Community User profile |
| Customer Community Plus | Customer Community Plus User |
| Partner Community | Partner Community User |
| Guest (unauthenticated) | Guest User profile (auto-generated per site) |

### Creating a Community User

A community user must be:
1. A **Contact** in Salesforce (linked to an Account)
2. "Enabled" for community access — right-click on Contact → Enable Customer User

**Path:** Open a Contact record → (top right dropdown) → **Enable Customer User**

This creates a User record with a Community license linked to that Contact.

### Self-Registration

Instead of manually creating community users, you can enable **Self-Registration** — allowing external users to create their own accounts through a registration form.

**Path:** Experience Builder → Administration → Login & Registration → Self-Registration

When enabled:
1. User visits the site's registration page
2. Fills in name, email, password
3. Salesforce creates a Contact (linked to an Account you specify) and a Community User
4. User is automatically logged in

**Real-World Example:**
> "SaasApp" allows any customer to self-register for their support portal:
> - Customer visits `saasapp.my.site.com/support/SelfRegister`
> - Fills in Company Name, Name, Email, Password
> - Salesforce creates:
>   - Account: "Acme Corp" (if not existing)
>   - Contact: "John Smith" at Acme Corp
>   - User: `john@acme.com` with Customer Community license
> - John is automatically logged in and can see the portal

---

## 11.8 Security in Experience Cloud

Experience Cloud uses the SAME Salesforce security model as internal users — OWD, Role Hierarchy, Sharing Rules, FLS all apply. However, there are additional community-specific sharing mechanisms.

### The Security Challenge for Communities

**The problem:** Standard Salesforce sharing (Role Hierarchy, Sharing Rules) is designed for internal users with specific roles. Community users often don't fit neatly into the internal role hierarchy.

**Example:**
> OWD for Case = Private
> An internal support agent owns the case.
> A customer logs into the community — they are associated with the Account.
> But they don't own the Case (the agent does) and they're not in the internal role hierarchy.
>
> **Without community-specific sharing:** Customer sees ZERO cases — even their own!
> **With Sharing Set:** Cases where `Case.AccountId = User.AccountId` are shared automatically.

### Community Security Mechanisms

| Mechanism | Purpose |
|-----------|---------|
| **Sharing Sets** | Share records with community users based on Account/Contact relationship |
| **Share Groups** | Give community users access to records owned by OTHER community users in the same account |
| **Community Roles** | Hierarchical roles specifically for community users — Account Manager, Account Member |
| **Guest User Profile** | Controls what unauthenticated visitors can see |

---

## 11.9 Sharing Sets

A **Sharing Set** is a community-specific sharing mechanism that automatically grants community users access to records where the community user's Account or Contact matches a field on the record.

**Path:** Setup → Digital Experiences → Settings → Sharing Sets → New

### How Sharing Sets Work

```
Community User Priya (Account: Acme Corp) logs in
                    ↓
Sharing Set checks: Does any Case.AccountId = Priya's AccountId?
                    ↓
Cases where AccountId = Acme Corp's ID → SHARED with Priya
Cases belonging to other accounts → NOT shared with Priya
```

### Sharing Set Configuration

**Access Mapping:** The core of a Sharing Set — defines how to match the community user to records.

| Field on Record | Match With | Example Meaning |
|----------------|-----------|----------------|
| `Case.AccountId` | `User.AccountId` | Share Cases belonging to the user's Account |
| `Case.ContactId` | `User.ContactId` | Share Cases submitted by the user's Contact |
| `Order.AccountId` | `User.AccountId` | Share Orders for the user's Account |
| `Contract.AccountId` | `User.AccountId` | Share Contracts for the user's Account |

### Real-World Sharing Set Examples

**Example 1 — B2B Customer Support:**
> "CloudSoft" has enterprise customers. Each company has multiple employees who log into the portal.
>
> Sharing Set: "Customer Case Access"
> - Object: Case
> - Access Mapping: `Case.AccountId = User.AccountId`
> - Access Level: Read/Write
>
> Result: ALL employees of Acme Corp can see ALL Acme Corp's cases. Any Acme employee can update cases.

**Example 2 — B2C Consumer Support:**
> "Bank" has individual retail customers. Each customer should only see THEIR OWN cases.
>
> Sharing Set: "Individual Customer Cases"
> - Object: Case
> - Access Mapping: `Case.ContactId = User.ContactId`
> - Access Level: Read/Write
>
> Result: Each customer only sees cases submitted BY THEM — not cases submitted by other customers at the same address/account.

**Example 3 — Order Visibility:**
> "E-Commerce Co." — customers should see their own orders.
>
> Sharing Set: "Customer Order Access"
> - Object: Order
> - Access Mapping: `Order.AccountId = User.AccountId`
>
> Result: Customer logs in → sees all orders for their account.

### Supported Objects for Sharing Sets

Sharing Sets support:
- Standard objects: Cases, Accounts, Contacts, Opportunities, and others
- Custom objects with Lookup relationships to Account or Contact
- Objects accessible via the community license

---

## 11.10 Share Groups

**Share Groups** extend the Sharing Set concept — they allow community users to see records **owned by other community users** within the same Account.

### When You Need Share Groups

**Scenario:**
> Priya and Raj both work at Acme Corp. Both are community users.
> Priya creates a Case.
> The Sharing Set gives Priya access to Cases WHERE `AccountId = Acme Corp`. ✅
> BUT the Case is now OWNED by Priya.
> OWD = Private → Raj cannot see the Case (he's not in the role hierarchy, and he doesn't own it).
>
> **Problem:** Raj (Priya's colleague at Acme Corp) cannot see a case Priya submitted on behalf of their company.

**Solution: Share Group**

A Share Group says: "Share all records owned by community users in [Profile] with all other community users in the same Account."

**Path:** Setup → Digital Experiences → Settings → Sharing Sets → [Sharing Set] → Share Groups

### Real-World Share Group Example

> **Company:** CloudSoft's enterprise Customer Portal
>
> Multiple employees of Acme Corp are community users.
> Any of them can submit a Case on behalf of Acme Corp.
> All of them should be able to see and collaborate on all Cases for Acme Corp.
>
> **Share Group Setup:**
> - Sharing Set: Customer Case Access (already configured)
> - Share Group: Enable → sets access so all Customer Community Users within the same Account can see each other's submitted records.
>
> Now Raj can see cases submitted by Priya (same Account). Priya can see Raj's cases. External users at a DIFFERENT Account (TechStart) cannot see either Acme Corp's cases.

---

## 11.11 Guest Users

### What is a Guest User?

A **Guest User** is anyone who visits your Experience Cloud site WITHOUT logging in. They are completely anonymous — Salesforce has no identity information for them.

Every Experience Cloud site automatically gets a **Guest User profile** — a system-generated profile that controls what unauthenticated visitors can access.

**Profile naming convention:** `[Site Name] Profile (Guest User)`

### What Guest Users Can Access (by default)

Very little — by design. The default Guest User profile has:
- Read access to publicly available Knowledge Articles (if configured)
- Access to the Login and Registration pages
- Access to public page content (static text, images)

### What Guest Users CANNOT Access by default

- Any Salesforce records (Cases, Accounts, Contacts, Orders, etc.)
- Personal account information
- Submit cases (unless explicitly enabled with a form)
- View any CRM data

### Why Restrict Guest Users?

**Security principle:** A guest user could be anyone — a competitor, a bad actor, or just an anonymous browser. Giving them access to CRM data would be a massive security breach.

**Salesforce has significantly tightened Guest User security in recent releases:**
- Guest Users cannot access records they don't own
- Guest Users cannot access records via certain sharing mechanisms
- Admins must explicitly grant any access Guest Users receive
- There are specific settings to enable Web-to-Case or Web-to-Lead from guest users

### Real-World Guest User Configuration

**Public Help Center (Help Center template):**
> - Guest User can: browse all published Knowledge Articles, search articles, view article detail pages
> - Guest User cannot: submit cases, view account info, see any customer data
> - Optional: "Contact Support" button → Web-to-Case form → creates a case even for non-logged-in users

**Customer Account Portal (before login):**
> - Guest User sees: login page, "Create Account" self-registration form, "Forgot Password" link
> - Guest User cannot: see any portal content until logged in

---

## 11.12 Community Roles

### What are Community Roles?

Community users exist in a **separate Role Hierarchy** from internal Salesforce users. Community Roles define visibility levels within the external user base.

**Three standard community roles (for accounts with multiple community users):**

| Role | Who | Access |
|------|-----|--------|
| **Account Executive** | Senior users at the partner/customer account | Can see all community records for their account AND records of subordinate Account Managers and Account Members |
| **Account Manager** | Mid-level users | Can see their own records + Account Members' records |
| **Account Member** | Regular users | Can only see their own records |

### When Community Roles Matter

Community Roles become important when:
1. Multiple users from the same external Account are in the community
2. You want some users to have more visibility than others within the same Account
3. You need a manager-subordinate relationship among external users

**Real-World Example:**
> "Dell" Partner Portal: Acme Resellers has 5 employees in the partner community.
>
> | Employee | Community Role | Sees |
> |----------|---------------|------|
> | Anand (Sales Director) | Account Executive | All of Acme's deals and leads |
> | Priya (Sales Manager) | Account Manager | Her deals + her team's deals |
> | Raj (Sales Rep) | Account Member | Only his own deals |
>
> Anand can see everything — he's accountable for the full Acme relationship.
> Raj can only see his own registered deals — for sales privacy.

### Configuring Community Roles

Community Roles are enabled per-site and configured through Sharing Sets and Community settings.

**Path:** Experience Builder → Administration → Members → configure role assignment rules

---

## 11.13 Branding and Theming

Experience Builder provides comprehensive branding tools to make your community look like YOUR company's website — not like generic Salesforce.

### The Branding Editor

**Path:** Experience Builder → Theme → Branding

### What You Can Brand

| Element | Options |
|---------|---------|
| **Company Logo** | Upload logo image — appears in header |
| **Primary Color** | Main button and accent color |
| **Secondary Color** | Secondary elements color |
| **Tertiary Color** | Background and border accents |
| **Header Color** | Top navigation bar background |
| **Footer Color** | Bottom footer background |
| **Typography** | Font family for headings and body text |
| **Button Style** | Rounded, rectangular, pill-shaped |
| **Background** | Page background image or color |

### Custom CSS

For advanced styling beyond the Branding Editor, you can inject **Custom CSS**:

**Path:** Experience Builder → Theme → Edit CSS

```css
/* Change the header background to company blue */
.siteforceBody .comm-header {
  background-color: #003087;
}

/* Style primary buttons */
.siteforceBody .comm-button-primary {
  background-color: #FF6600;
  border-radius: 4px;
}

/* Custom font */
body {
  font-family: 'Roboto', sans-serif;
}
```

### Real-World Branding Example

> "TechBridge" builds their Support Portal. Without branding, it looks like a generic Salesforce community. After branding:
>
> - Logo: TechBridge blue and white logo in the header
> - Primary color: TechBridge blue (#003087)
> - Header background: Dark navy
> - Fonts: Roboto (matches their main website)
> - Custom CSS: Rounded card corners, custom hover effects on knowledge article tiles
>
> Result: Customers landing on the portal think they're on TechBridge's own website — not Salesforce.

---

## 11.14 Audience Targeting

**Audience Targeting** allows you to show DIFFERENT content, components, or page layouts to DIFFERENT groups of users — on the SAME page and URL — without building separate pages or sites.

**Path:** Select a component in Experience Builder → Component Properties → **Set Visibility**

### Audience Types

| Audience Type | Based On | Example |
|--------------|---------|---------|
| **Profile** | User's community profile | Premium members see different header |
| **Field value** | A field on the logged-in user's record | Account.Type = "Enterprise" |
| **Permission** | A specific permission the user has | Users with "VIP Support" permission |
| **Authentication** | Logged in or guest | Logged-in users see dashboard; guests see login prompt |
| **Device type** | Mobile, tablet, or desktop | Different layout on mobile |
| **Geographic** | User's location | Indian users see INR prices; US users see USD |

### How to Configure Audience Targeting

1. Select a component on the canvas
2. In the right properties panel → find **"Set Visibility"**
3. Click **"Add Filter"**
4. Choose filter type → configure condition
5. Component will only show when condition is true

**Multiple conditions:** You can combine AND/OR conditions for complex targeting.

### Real-World Audience Targeting Examples

**Example 1 — Tier-Based Support Messaging:**
> Home page has two "contact support" banners:
>
> Banner A: "As a Premium customer, call our dedicated line: 1800-PREMIUM. Available 24/7."
> Visibility: Account.Type = "Premium"
>
> Banner B: "Submit a ticket below. We respond within 24 hours."
> Visibility: Account.Type ≠ "Premium"
>
> Both banners on the same page — each customer sees only the one relevant to them.

**Example 2 — Authenticated vs Guest:**
> The Home page shows:
>
> Component A: "My Cases" dashboard widget
> Visibility: User is logged in
>
> Component B: "Please log in to view your cases and account information"
> Visibility: User is a guest (not logged in)
>
> One URL, one page — completely different experience depending on authentication state.

**Example 3 — Language-Based:**
> Portal serves both Hindi and English speaking customers.
>
> Banner A (Hindi): "नमस्ते! हम आपकी मदद के लिए यहाँ हैं।"
> Visibility: User.Language = "Hindi"
>
> Banner B (English): "Welcome! We're here to help."
> Visibility: User.Language ≠ "Hindi"

**Example 4 — Partner Level:**
> Partner Portal with different content for:
> - Authorized Resellers → basic sales tools
> - Gold Partners → gold-specific pricing and leads
> - Platinum Partners → platinum pricing, dedicated account manager contact info, exclusive products
>
> Each partner level sees their relevant information on the same home page.

---

## 11.15 Community Search

**Community Search** is the built-in search capability in Experience Cloud — allowing users to search across multiple types of content in one search bar.

### What Community Search Indexes

| Content Type | Indexed? | Configuration |
|-------------|---------|---------------|
| **Knowledge Articles** | ✅ Yes | Published articles appear automatically |
| **Chatter Posts** | ✅ Yes (in groups the user can access) | Auto-indexed |
| **CRM Records** | ✅ Yes (if user has access) | Based on sharing model |
| **Files** | ✅ Yes (if user has access) | Published community files |
| **Topics** | ✅ Yes | Community discussion topics |

### Search Components in Experience Builder

**Search Bar:** Displays a search input — placed in the header for global search

**Search Results Page:** Displays results with tabs for different content types

**Featured Topics:** Grid or list of predefined topic areas — clicking navigates to a filtered search

**Global Search for Peer-to-Peer Sites:** Optimized for discussion communities

### Promoted Search Results

Admins can **promote specific articles** to appear at the top of search results for specific keywords.

**Path:** Setup → Promoted Search Terms → New

**Real-World Example:**
> Customer searches "password reset" → the Knowledge Article "How to Reset Your Portal Password" is promoted → it always appears first, regardless of search algorithm ranking. Reduces "password reset" support calls by 40%.

### Knowledge Search Optimization

For Knowledge articles to appear well in search:
- Use descriptive titles with common keywords
- Add keywords and synonyms in the article's metadata
- Use Data Categories to organize articles by topic
- Keep articles updated — Salesforce search weights freshness

---

## 11.16 Case Deflection and Knowledge Integration

**Case Deflection** is one of the most valuable ROI features of Experience Cloud — automatically suggesting Knowledge Articles to users BEFORE they submit a case, potentially resolving their issue without agent involvement.

### How Case Deflection Works

```
Customer clicks "Submit a Case"
         ↓
Types their issue in Subject/Description
         ↓
Salesforce searches Knowledge Articles in real time
         ↓
Suggested articles appear: "Did you find what you were looking for?"
         ↓
Customer reads article → Issue resolved → No case submitted ✅
OR
Customer reads article → Still needs help → Continues submitting case
```

### Setting Up Case Deflection

**Component:** "Case Deflection" — available in Experience Builder component palette

**Configuration:**
- Which knowledge base to search
- Minimum number of characters before searching begins
- Number of articles to display
- What happens when "Yes, this resolved my issue" is clicked

### Deflection Metrics

Salesforce tracks:
- How many case submissions were deflected
- Which articles deflected the most cases
- Deflection rate over time

**Real-World Example:**
> "CloudSoft" implements case deflection. After 3 months:
> - 8,000 case submissions started per month
> - 2,400 deflected (users found their answer in Knowledge)
> - Deflection rate: 30%
> - Agent time saved: 2,400 cases × 8 minutes average handle time = 320 hours/month
> - Cost savings: ₹4,80,000/month in agent labor

---

## 11.17 Moderation

**Community Moderation** controls what content community members can post and provides tools to manage user-generated content.

**Path:** Experience Builder → Administration → Moderation

### Moderation Capabilities

| Feature | Purpose |
|---------|---------|
| **Word-Level Blocking** | Prevent specific words/phrases from being posted |
| **Content Rules** | Define rules for what triggers moderation review |
| **Freezing Users** | Temporarily prevent a community user from posting |
| **Banning Users** | Remove all posting privileges |
| **Flag Content** | Members can flag inappropriate content for review |
| **Moderation Queue** | Review flagged content before it's published |
| **Automated Moderation** | Einstein AI can flag potential violations automatically |

### Real-World Moderation Example

> "FinanceApp" builds a peer-to-peer community where customers share investment tips. They configure moderation:
>
> - **Blocked words:** List of profanity, competitor product names, "guaranteed returns" (regulatory)
> - **Content Rules:** Any post containing "100% safe investment" → auto-flagged for compliance review
> - **Moderators:** 2 community managers who review the moderation queue daily
> - **User Reputation System:** Users earn reputation points; high-reputation users can post without moderation; new users go through moderation queue for first 5 posts

---

## 11.18 Reports and Dashboards in Communities

Experience Cloud supports embedding Salesforce Reports and Dashboards in community pages — giving external users self-service analytics.

### Reports in Communities

**Who can see reports in communities:**
- Customer Community Plus users (not standard Customer Community)
- Partner Community users
- Users with appropriate report folder permissions

**What they can see:**
- Reports in folders shared with their community profile
- Only data the community user has access to (sharing model applies to report results too)

### Dashboard Embedding in Experience Builder

**Path:** Experience Builder → Add Component → "Salesforce Report Chart" or "Dashboard"

This embeds a specific Salesforce dashboard directly into a community page — community users see real-time data from their allowed records.

**Real-World Example:**
> "Dell" Partner Portal — each partner can see a "My Performance" dashboard showing:
> - Their Registered Deals by Stage
> - This Quarter vs Last Quarter Revenue
> - Lead Conversion Rate
> - Pending Certifications
>
> All data is filtered to their Account — they only see THEIR data.

---

## 11.19 Headless Experience Cloud

**Headless Experience Cloud** allows you to use Salesforce as the **backend data and logic layer** while building the **frontend UI entirely yourself** — using React, Vue.js, Angular, or any web framework.

### Why Headless?

| Reason | Explanation |
|--------|-------------|
| **Brand consistency** | Your dev team builds the exact UI matching your brand system |
| **Performance** | Custom-optimized front end without Salesforce's UI framework overhead |
| **Existing website** | Integrate Salesforce data into an existing website without rebuilding |
| **Mobile apps** | Native iOS/Android apps backed by Salesforce data |
| **Complex UI** | Sophisticated interfaces that Experience Builder can't achieve |

### How Headless Works

```
Custom Frontend (React/Vue/Angular)
         ↓
Salesforce Experience Cloud APIs:
  - Experience Cloud API
  - Connect REST API
  - Managed Content Delivery API
  - User Management API
         ↓
Salesforce Backend:
  - CRM Data (Cases, Accounts, Orders)
  - Knowledge Articles
  - User authentication (via OAuth)
  - Sharing and security model
```

**Real-World Example:**
> "HDFC Bank" has their own mobile banking app built natively in iOS and Android. They want to add Salesforce-powered case management (log a complaint, track status) to the app.
>
> With Headless Experience Cloud:
> - The mobile app's UI is built by HDFC's mobile team (native iOS/Android)
> - When user submits a complaint → API call to Salesforce → Case created
> - When user checks status → API call → returns Case status and comments
> - Authentication → OAuth flow using Salesforce as identity provider
>
> Users see HDFC's app. Salesforce provides all the data and case management logic behind the scenes.

---

## ✅ Hands-On Tasks

> Complete these in your Trailhead Playground. Some tasks require Digital Experiences to be enabled.

---

### 🔧 Task 1 — Enable Digital Experiences and Create Your First Site

**Objective:** Enable Experience Cloud and provision a community site.

**Steps:**
1. Setup → Digital Experiences → **Settings**
2. Check **"Enable Digital Experiences"** ✅
3. Set your domain (your Playground will auto-assign one like `abc123-dev-ed.my.site.com`)
4. Click **Save**

5. Setup → Digital Experiences → **All Sites** → **New**
6. Choose template: **Customer Account Portal**
7. Name: `TechBridge Support Portal`
8. URL: `support`
9. Click **Create**

**Experience Builder opens automatically. Explore the interface:**
10. Note the left panel: Pages, Components, Theme
11. Note the canvas: current page preview
12. Note the top: Device toggle, Preview, Publish buttons
13. Click through the pre-built pages: Home, My Cases, Case Detail

---

### 🔧 Task 2 — Customize the Home Page with Components

**Objective:** Build a branded home page for the customer portal.

**Steps:**
1. In Experience Builder, click **Pages** → **Home**
2. Click the existing default header text → press Delete to remove
3. From the left Components panel → drag a **Rich Content Editor** to the top of the page
4. Double-click the component → type:
   ```
   Welcome to TechBridge Support Portal
   
   We're here to help. Search our Knowledge Base or submit a support request below.
   ```
5. Make "TechBridge Support Portal" bold, larger font

6. Drag a **Tile Menu** component below the rich text editor
7. Configure tiles:
   - Tile 1: Label = "Submit a Case", URL = /NewCase (or your case creation path), Icon = "Cases"
   - Tile 2: Label = "My Cases", URL = /MyCases, Icon = "Activity"
   - Tile 3: Label = "Knowledge Base", URL = /Knowledge, Icon = "Knowledge"

8. Click **Preview** → see how it looks as a logged-in user
9. Toggle device preview → check mobile view
10. Click **Publish**

---

### 🔧 Task 3 — Apply Branding

**Objective:** Make the portal match a company's brand identity.

**Steps:**
1. Experience Builder → **Theme** (paint roller icon in left panel) → **Branding**
2. Set:
   - Company Logo: Upload any logo image (or leave default for now)
   - Primary Color: `#1F4E79` (dark blue)
   - Secondary Color: `#2E75B6` (medium blue)
   - Header Color: `#003087` (navy)
3. Click **Save & Publish**

4. Preview the site → observe how branding colors now apply to buttons, links, and header

5. Go to **Theme** → **Edit CSS** → add:
```css
/* Round the corner of the main container */
.siteforceBody .comm-layout-region-main {
    border-radius: 8px;
}
```
6. Publish → refresh preview

---

### 🔧 Task 4 — Add a Community Member and Configure Access

**Objective:** Create a community user and give them access.

**Pre-requisite:** You should have `Arjun Mehta` as a Contact linked to `TechStart India` Account (from Topic 09).

**Steps:**
1. Open the `Arjun Mehta` Contact record
2. Click the dropdown at top right → **"Enable Customer User"**
3. On the Create User form:
   - Email: `arjun@techstart.in`
   - Username: `arjun@techstart.in.supportportal` (must be unique)
   - License: **Customer Community**
   - Profile: **Customer Community User**
4. Save → a welcome email is sent (goes to your email since you used your email as admin)

5. Add the profile to the site:
   - Experience Builder → Administration → Members
   - Add: **Customer Community User** profile

6. Find the site URL (Experience Builder top → display site URL) → open it
7. Log in as Arjun using his credentials
8. Explore what he can and cannot see

---

### 🔧 Task 5 — Configure a Sharing Set for Case Visibility

**Objective:** Give community users access to their company's cases.

**Setup:**
1. Create a Case in Salesforce owned by the internal admin, linked to `TechStart India` Account and `Arjun Mehta` Contact
   - Subject: "Cannot access product dashboard"
   - Status: Open
   - Origin: Phone

**Configure Sharing Set:**
2. Setup → Digital Experiences → Settings → **Sharing Sets** → **New**
3. Name: `Customer Support Case Access`
4. Select License: **Customer Community**
5. Select Object: **Case**
6. Configure Access Mapping:
   - For Access, select: **Read/Write**
   - Add Mapping: `User.AccountId` maps to `Case.AccountId`
7. Save

**Test:**
8. Log in as Arjun (community user via the site URL)
9. Navigate to "My Cases" → Arjun should NOW see the case you created
10. Try to edit the case (should be allowed — Read/Write)

**Without Sharing Set (for comparison):**
11. Delete the Sharing Set temporarily → log in as Arjun → cases should disappear
12. Re-create the Sharing Set → cases reappear

---

### 🔧 Task 6 — Configure Audience Targeting

**Objective:** Show different content based on user attributes.

**Scenario:** Users with Account Type = "Customer - Direct" see a premium support banner; others see a standard banner.

**Steps:**
1. Make sure Arjun's Account (`TechStart India`) has Type = "Customer - Direct"
2. Create a second community user linked to a different Account with Type = "Prospect"

3. Experience Builder → Home page
4. Add two **Rich Content Editor** components:

   **Component A (Premium):**
   - Text: "🌟 As a valued TechBridge customer, your calls are prioritized. Call us: 1800-TECH"
   - Set Visibility → Add Filter → Field → `Account.Type` → Equals → `Customer - Direct`

   **Component B (Standard):**
   - Text: "Submit a case below. We respond within 24 business hours."
   - Set Visibility → Add Filter → Field → `Account.Type` → Does Not Equal → `Customer - Direct`

5. Publish

6. Log in as Arjun (Customer - Direct) → should see Component A
7. Log in as the Prospect user → should see Component B

---

### 🔧 Task 7 — Configure Knowledge Integration

**Objective:** Surface Knowledge Articles in the community.

**Pre-requisite:** At least one published Knowledge Article from Topic 10.

**Steps:**
1. Experience Builder → add a new page or edit Home page
2. Add component: **Knowledge Article List** or **Search** component
3. Configure to show articles from the appropriate Data Category
4. Publish

5. Log in as Arjun
6. Search "password reset" → verify the Knowledge Article appears in results
7. Click the article → verify full content is readable
8. Check: can Arjun edit the article? (Should be No — he's a reader, not an author)

---

### 🔧 Task 8 — Add Case Deflection

**Objective:** Show Knowledge Articles when a user tries to create a case.

**Steps:**
1. Experience Builder → find the "New Case" page (or create one)
2. From the Components panel → search for "Case Deflection" → drag it onto the page
3. Configure:
   - Knowledge base to search: your org's knowledge base
   - Minimum characters: 5
   - Number of results to show: 3
4. Publish

5. Log in as Arjun → click "Submit a Case"
6. Start typing in the Subject: "Cannot access"
7. Observe: Knowledge Articles should appear below suggesting possible solutions
8. Click one → does it resolve without submitting a case?

---

## 📝 Practice Questions

**Q1.** What is the difference between a "Customer Community" license and a "Customer Community Plus" license?

**Q2.** A company wants to allow any website visitor (without a login) to search their Knowledge Base. Which Experience Cloud template is most appropriate, and what user type handles unauthenticated visitors?

**Q3.** OWD for Case is set to Private. An internal support agent owns a Case for Acme Corp. Priya (a community user from Acme Corp) logs in. Without any community sharing configuration, can Priya see the Case?

**Q4.** What is the difference between a Sharing Set and a Share Group in Experience Cloud?

**Q5.** A Partner Community has three community roles: Account Executive, Account Manager, Account Member. Who can see the most data, and what exactly can an Account Member see?

**Q6.** What is Case Deflection, and what is its primary business benefit?

**Q7.** Can a Customer Community (standard) user access Salesforce Reports and Dashboards within the community?

**Q8.** What is "headless" Experience Cloud, and when would you choose it over standard Experience Builder?

**Q9.** How is Audience Targeting different from creating separate pages or separate communities for different user groups?

**Q10.** A Guest User visits your Customer Account Portal before logging in. Which Salesforce profile controls what they can see, and what is the default level of access?

**Q11.** What is a Promoted Search Term, and how does it help community users?

**Q12.** A community user (Customer Community license) tries to edit an Account record that they can see via a Sharing Set. Can they edit it?

**Q13.** What is the maximum number of Sharing Set access mappings you can configure per object?

**Q14.** Experience Cloud sites can be accessed at `[company].my.site.com/[path]`. What must be configured BEFORE creating any sites in a Salesforce org?

**Q15.** A Partner Community user wants to register a deal to claim protected pricing. Which specific feature within Partner Central supports this?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Complete Community Design for Healthcare**

> "MediCare Insurance" needs three Experience Cloud portals:
>
> **Portal 1 — Policyholder Portal:**
> - 500,000 individual customers (retail)
> - Customers view their own policies, claims, and premium payments
> - Customers submit new claims and upload documents
> - Customers chat with support agents
> - Each customer must ONLY see their own data — not other customers' data
>
> **Portal 2 — Hospital Network Portal:**
> - 2,000 hospital administrators (B2B)
> - Hospitals submit reimbursement claims on behalf of patients
> - Multiple hospital staff access the same hospital's claims
> - Hospital managers can see all claims for their hospital; staff see only their own submissions
> - Hospitals access standard claim processing documentation (Knowledge Articles)
>
> **Portal 3 — Public Health Education Center:**
> - Completely public — no login required
> - Contains health articles, policy summaries, FAQ
> - Customers can find the portal via Google search
>
> **For each portal, answer:**
> 1. Which Experience Cloud template would you use?
> 2. What Community license type is appropriate? Per member or per login?
> 3. What Salesforce objects are exposed, and at what access level?
> 4. How do you ensure data isolation (each user only sees their own data)?
> 5. What sharing mechanism(s) would you configure?
> 6. What branding considerations are important?

---

**Scenario 2 — Partner Portal Architecture**

> "TechSystems" is a technology manufacturer selling through 500 reseller partners. They want to build a Partner Central portal. Requirements:
>
> - Partners register deals to claim protected pricing from TechSystems
> - TechSystems distributes leads to qualified partners based on territory
> - Partners can see all their registered deals and leads
> - Partner Managers (senior users at each reseller) can see all deals for their company; Partner Reps see only their own deals
> - TechSystems' internal channel managers should see all deals for their assigned partners
> - Partners can download product datasheets, price lists, and training materials
> - Partners can view a performance dashboard: quota, pipeline, closed won revenue this quarter
>
> **Questions:**
> 1. Which template would you use?
> 2. What license type for partner users?
> 3. How do you implement the "Manager sees all company deals; Rep sees own deals" requirement? Which Community Roles apply?
> 4. How do TechSystems' internal channel managers access partner deal data — are they community users or internal users?
> 5. For the performance dashboard, what ensures each partner only sees THEIR OWN data?
> 6. How would you make product datasheets available to partners while keeping them from competitors who might register as a partner?

---

**Scenario 3 — Community Security Breach Investigation**

> "RetailMax" deployed a Customer Community 3 months ago. OWD for all objects is Private. The security team reports: "Customer Priya Sharma (Account: Small Electronics Co.) can see Orders belonging to GlobalTech Corp — a completely different customer."
>
> **Questions:**
> 1. List ALL possible root causes of this data leak (hint: there are at least 5 distinct causes)
> 2. For each cause, describe exactly how you would investigate it (which Setup path, what to look for)
> 3. For each cause, describe the specific configuration fix
> 4. After fixing, how do you PROVE to the security team that the breach is resolved?
> 5. What monitoring would you put in place to detect if this happens again in the future?

---

**Scenario 4 — Audience Targeting Design**

> "CloudBank" has a customer community with three customer segments:
> - **Retail Banking** — individual account holders (basic banking)
> - **Premium Banking** — high net-worth individuals (priority service)
> - **Business Banking** — company accounts (business services)
>
> The bank wants the same portal URL but different experiences for each segment:
> - Retail: Basic FAQ, ticket submission, account balance (read-only)
> - Premium: All Retail features + dedicated relationship manager contact info + priority call queue number + exclusive offers section
> - Business: All Retail features + bulk payment tools + multi-user account management + business analytics dashboard
>
> **Questions:**
> 1. What field on the user's related Account would drive the audience segmentation?
> 2. For each of the three segments, design 3 specific audience rules that differentiate their experience
> 3. How would you implement the "Premium Banking" section so it is NEVER accidentally visible to Retail or Business customers?
> 4. Can a single user see content targeted at multiple audiences? When would this be desirable? When would it be a problem?
> 5. How would you test that the targeting is working correctly before going live?

---

**Scenario 5 — Case Deflection ROI Analysis**

> "SupportFirst" processes 15,000 support cases per month. Average case handle time = 12 minutes. Agent hourly cost = ₹800/hour.
>
> They implement Experience Cloud with:
> - Customer self-service portal
> - 500 Knowledge Articles covering common issues
> - Case Deflection component on the case submission form
>
> After 6 months, analytics show:
> - Portal registration: 12,000 unique customers
> - Monthly portal sessions: 40,000
> - Cases deflected (users found their answer): 3,600/month (24% deflection rate)
> - Average portal session for deflected case: 4 minutes
>
> **Questions:**
> 1. Calculate the monthly cost savings from case deflection (use the numbers above)
> 2. If Experience Cloud costs ₹15,00,000/month (licenses + infrastructure), what is the monthly ROI?
> 3. What is the payback period if there was a one-time implementation cost of ₹50,00,000?
> 4. What additional benefits (beyond pure cost savings) should be included in the ROI calculation?
> 5. The Knowledge team wants to identify which articles are responsible for the most deflections. Which Salesforce feature provides this data?

---

## ✔️ Answer Key — Practice Questions

1. **Customer Community:** Standard self-service access — Cases, Accounts, 10 custom objects, basic community features. Per login or per member pricing. No reporting access. **Customer Community Plus:** Everything in Customer Community PLUS: Salesforce Reports and Dashboards access, Delegated Administration capabilities, more custom objects. Higher price point. Use Customer Community Plus when partners or customers need self-service reporting or portal admin delegation.

2. **Help Center template** — designed specifically for public Knowledge Base access without requiring login. **Guest User** handles all unauthenticated visitors — every Experience Cloud site automatically generates a Guest User profile that controls what anonymous visitors can see and do.

3. **No — Priya cannot see the Case.** OWD = Private means only the record owner sees the record. The internal support agent owns the Case. Priya (community user) is not the owner, is not in the internal role hierarchy above the agent, and no Sharing Set or Share Group has been configured. Result: zero Cases visible to Priya.

4. **Sharing Set:** Automatically shares records FROM internal Salesforce (owned by internal users/agents) WITH community users — based on a field match like `Case.AccountId = User.AccountId`. Solves: "customer can't see their company's cases because agents own them." **Share Group:** Allows community users to see records OWNED BY OTHER community users within the same Account. Solves: "one customer employee can't see cases submitted by their colleague" even though a Sharing Set handles records owned by internal users.

5. **Account Executive** can see the most — all records for their Account, including those owned by Account Managers and Account Members below them. **Account Member** can see only their own records — records they personally created or that were explicitly shared with them. They cannot see peers' records or superiors' records through the community role hierarchy.

6. **Case Deflection** shows relevant Knowledge Articles to users as they type their issue description on the "New Case" form — before they submit the case. If the user finds their answer in a suggested article, they don't submit the case at all. **Primary business benefit:** Reduces inbound case volume — agents spend time on genuinely complex issues rather than repeated common questions. Measurable benefit: deflection rate × (cases × average handle time × agent cost) = direct cost saving.

7. **No** — standard Customer Community license does NOT include access to Salesforce Reports and Dashboards. This capability requires **Customer Community Plus** or **Partner Community** license. Without the higher-tier license, reports and dashboards cannot be surfaced in the community even if embedded in Experience Builder pages.

8. **Headless Experience Cloud** uses Salesforce as the data/logic backend while an entirely custom frontend (built with React, Vue, native mobile, etc.) handles the user interface — connected via Experience Cloud APIs. **Choose headless when:** (1) You have an existing website or app you want to connect to Salesforce data without rebuilding in Experience Builder, (2) You need native mobile app functionality, (3) You need UI complexity or performance that Experience Builder cannot achieve, (4) Your dev team prefers working with standard web technologies (React, Vue) rather than Salesforce's framework.

9. **Audience Targeting** shows different content on the SAME PAGE at the SAME URL — no duplication of pages or sites. If you created separate pages, you'd need to maintain each page independently (update both when content changes). If you created separate communities, you'd need to manage two complete sites. Audience Targeting allows one page with conditional component visibility — single source of truth, maintained once.

10. The **[Site Name] Guest User Profile** (automatically generated for each Experience Cloud site) controls what unauthenticated visitors see. By default: nearly nothing — only public page content (static text, images, any components explicitly configured for public access). No CRM records are accessible. No user-specific data. The profile must be explicitly configured to allow any object or field access.

11. A **Promoted Search Term** is a specific keyword configured to return a designated Knowledge Article at the TOP of search results — overriding the search algorithm's natural ranking. **How it helps:** When you know users frequently search for a specific term and a specific article solves it, you guarantee that article always appears first — reducing the time users spend finding the right answer and increasing the probability they resolve their issue without submitting a case.

12. **It depends on the Sharing Set's configured Access Level.** If the Sharing Set grants **Read Only**: no, cannot edit. If the Sharing Set grants **Read/Write**: yes, they can edit. The Access Level in the Sharing Set configuration (Read Only vs Read/Write) determines whether community users can edit the shared records.

13. There is no specific documented maximum for Sharing Set access mappings per object — Salesforce allows multiple mappings per Sharing Set. However, each Sharing Set is limited in total configuration complexity. In practice, most implementations use 1–3 mappings per object.

14. **Digital Experiences must be enabled** in the org AND a **My Domain** must be configured. My Domain creates the company-specific subdomain (`company.my.site.com`) that all Experience Cloud sites use as their base URL. Digital Experiences cannot be enabled without My Domain being configured and deployed first.

15. **Deal Registration** — the specific Partner Central feature where partners formally claim a new deal with a customer, establishing their protected relationship with that customer for pricing and commissions. TechSystems' channel managers receive the registration, review it, and approve/reject based on territory and partner qualification.

---

## 🔗 Trailhead Resources

- [Community Cloud Basics](https://trailhead.salesforce.com/content/learn/modules/community_cloud_basics)
- [Community Search Basics](https://trailhead.salesforce.com/content/learn/modules/comm_search_basics)
- [Share CRM Data with Communities](https://trailhead.salesforce.com/content/learn/projects/communities_share_crm_data)
- [Personalize Audiences in Communities](https://trailhead.salesforce.com/content/learn/projects/communities_personalize_audiences)
- [Build a Community with Knowledge & Chat](https://trailhead.salesforce.com/content/learn/projects/build-a-community-with-knowledge-and-chat)
- [Community Theme & Layout](https://trailhead.salesforce.com/content/learn/projects/communities_theme_layout)
- [Set Up Salesforce Knowledge](https://trailhead.salesforce.com/content/learn/projects/set-up-salesforce-knowledge)

---

*Previous: [10 · Service Cloud & Knowledge](./10_Service_Cloud_and_Knowledge.md)*

---

## 🏆 Curriculum Complete!

You have now completed all 11 modules of the Salesforce Admin & Consultant training curriculum. You are prepared to pursue:

| Certification | Focus | Exam Guide |
|--------------|-------|-----------|
| **Salesforce Certified Administrator** | Topics 01–09 | [Exam Guide](https://trailhead.salesforce.com/credentials/administrator) |
| **Salesforce Certified Service Cloud Consultant** | Deep specialization on Topic 10 | [Exam Guide](https://trailhead.salesforce.com/credentials/servicecloudconsultant) |
| **Salesforce Certified Experience Cloud Consultant** | Deep specialization on Topic 11 | [Exam Guide](https://trailhead.salesforce.com/credentials/experiencecloudconsultant) |

**Recommended next steps:**
1. Complete the Salesforce Admin Trailmix on Trailhead
2. Earn the Admin Superbadge
3. Practice with mock exams (Focus on Force, Salesforce Ben)
4. Schedule your certification exam at webassessor.com
