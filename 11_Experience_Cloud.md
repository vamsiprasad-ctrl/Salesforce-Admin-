# 11 · Experience Cloud (Communities)

> **Difficulty:** 🟡 Intermediate | **Est. Time:** 5 hrs

---

## 11.1 What is Experience Cloud?

Branded external portals connecting customers, partners, or employees directly to your Salesforce data — without internal Salesforce licenses.

| Portal Type | Use Case |
|------------|---------|
| Customer Account Portal | Customer self-service: Cases, Knowledge |
| Partner Central | Deal registration, lead distribution |
| Help Center | Public Knowledge Base (no login needed) |
| Build Your Own | Fully custom |

---

## 11.2 Community Licenses

External users need Community licenses (much cheaper than internal).

| License | Access | Pricing |
|---------|--------|---------|
| Customer Community | Standard self-service | Per member/month or per login |
| Customer Community Plus | + Reporting, delegated admin | Higher |
| Partner Community | Full partner features | Per member |

---

## 11.3 Security in Communities

- **Sharing Sets** — auto-share records where `Case.AccountId = User.AccountId` → each customer sees only their own company's cases
- **Guest User** — unauthenticated visitors; very restricted profile
- **Community Roles** — separate from internal role hierarchy; account-level visibility

---

## 11.4 Key Features

- **Experience Builder** — drag-and-drop page designer
- **Audience Targeting** — show different content per user segment on the same page
- **Community Search** — surfaces Knowledge, discussions, and CRM records
- **Branding** — logo, colors, fonts, custom CSS

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Create a Basic Customer Community

**Steps:**
1. Setup → Digital Experiences → All Sites → **New**
2. Choose template: **Customer Account Portal**
3. Name: `Support Portal`
4. URL: `support-portal`
5. Click **Create**
6. Experience Builder opens automatically

---

### 🔧 Task 2 — Customize the Community Home Page

**Steps:**
1. In Experience Builder → click the **Home Page**
2. Components panel on left → drag a **Rich Content Editor** to the top
3. Type: "Welcome to TechBridge Support Portal. How can we help you today?"
4. Below it, drag a **Tile Menu** component
5. Configure tiles: "My Cases", "Submit a Case", "Search Knowledge"
6. Click **Preview** to see the page as a logged-in user
7. Click **Publish**

---

### 🔧 Task 3 — Expose Cases to Community Users

**Steps:**
1. Setup → Sharing Settings → set Case OWD to **Private** (if not already)
2. Setup → Users → Create a new user:
   - Profile: **Customer Community User** (or Customer Community Login User)
   - Account: `TechStart India`
   - Contact: `Arjun Mehta`
3. Create a **Sharing Set:**
   - Setup → Digital Experiences → Settings → Sharing Sets → **New**
   - Name: `Customer Case Sharing`
   - Profiles: Customer Community User
   - Object: Case
   - Access Mapping: `User.ContactId = Case.ContactId`
   - Access Level: Read/Write
4. Log in as Arjun (via the community URL) → he should only see his own cases
5. Create a case as a different contact → Arjun should NOT see it

---

### 🔧 Task 4 — Add a Knowledge Component

**Steps:**
1. Experience Builder → open the Home page
2. Drag a **Featured Topics** or **Search** component
3. Configure it to search Knowledge Articles
4. Publish
5. Log in as Arjun → search "password reset" → verify the Knowledge Article from Topic 10 appears

---

### 🔧 Task 5 — Configure Audience Targeting

**Objective:** Show different content to different customer tiers.

**Steps:**
1. In Experience Builder, create a **Rich Content Editor** on the Home page
2. Add text: "As a Premium Member, you have 24/7 phone support. Call: 1-800-SUPPORT"
3. In the component's properties → click **Set Visibility**
4. Add a filter: Account Type = Premium (or any available field)
5. Now add another Rich Content Editor with standard support info for non-Premium users
6. Set its visibility: Account Type ≠ Premium
7. Publish and test with users having different Account Types

---

## 📝 Practice Questions

**Q1.** What is the key difference between an internal Salesforce user and a Community (Experience Cloud) user in terms of licensing?

**Q2.** What is a Sharing Set, and why is it needed for Community users?

**Q3.** OWD for Cases is Private. A Customer Community user logs in. Without any sharing configuration, what cases can they see?

**Q4.** What is a Guest User in the context of Experience Cloud?

**Q5.** Can a Community user see internal Chatter posts by Salesforce employees?

**Q6.** What does Audience Targeting allow you to do without creating multiple communities?

**Q7.** What is the purpose of the Help Center template versus the Customer Account Portal template?

**Q8.** A Partner Community license is more expensive than a Customer Community license. What additional capabilities justify the higher cost?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Community Design**

> "MediCare" is a healthcare insurance company. They need to build three portals:
> 1. **Patient Portal** — patients view their claims, upload documents, chat with support
> 2. **Provider Portal** — doctors/hospitals submit claims, check claim status, download payment summaries
> 3. **Public Health Center** — anyone (no login) can find health articles and FAQs
>
> **Question:** For each portal:
> - Which Experience Cloud template would you use?
> - Which Community license type is appropriate?
> - What Salesforce objects would be exposed?
> - What sharing mechanism ensures data isolation (each patient sees only their own claims)?

---

**Scenario 2 — Community Security**

> "RetailMax" launches a customer community. OWD for Order is Private. A bug is reported: "Customer Priya can see orders belonging to another customer, John."
>
> **Question:**
> 1. List all possible reasons this data leak could occur
> 2. How would you investigate each cause?
> 3. What is the correct fix for each cause?
> 4. How would you verify the fix is working?

---

## ✔️ Answer Key

1. Internal users need a **Salesforce user license** (full platform access, expensive). Community users need a **Community license** (access only to the portal and explicitly shared objects, much cheaper).
2. A **Sharing Set** automatically shares records with Community users based on a relationship (e.g., Account or Contact match). Without it, Community users with Private OWD can see zero records even if they own the Account.
3. **None** — with OWD = Private and no sharing configuration, a Community user sees only records they directly own. Cases are typically owned by internal agents, so the Community user sees nothing.
4. A **Guest User** is an **unauthenticated visitor** to a public community. They are not logged in, have a special restricted profile, and can only see public content (like a public Help Center).
5. **No** — internal Chatter posts (not on a record) are not visible to Community users unless the post is specifically on a public Community group.
6. Audience Targeting lets you show **different page content, components, or layouts** to different user segments on the **same page** — without building separate community pages or communities.
7. **Help Center** is designed for public, unauthenticated users who want to self-serve via a Knowledge base — no login required. **Customer Account Portal** is for authenticated customers who need to manage their account, view cases, and interact with Salesforce data.
8. Partner Community licenses include: **Deal Registration**, **Lead Distribution**, **Partner Fund Management**, **Sales tools** — features specifically designed for channel partner relationships that Customer Community does not include.

---

## 🔗 Trailhead Resources

- [Community Cloud Basics](https://trailhead.salesforce.com/content/learn/modules/community_cloud_basics)
- [Share CRM Data with Communities](https://trailhead.salesforce.com/content/learn/projects/communities_share_crm_data)
- [Build a Community with Knowledge & Chat](https://trailhead.salesforce.com/content/learn/projects/build-a-community-with-knowledge-and-chat)
- [Community Theme & Layout](https://trailhead.salesforce.com/content/learn/projects/communities_theme_layout)

---

*Previous: [10 · Service Cloud](./10_Service_Cloud_and_Knowledge.md)*

---

## 🏆 Curriculum Complete!

| Certification | Exam Guide |
|--------------|-----------|
| Salesforce Certified Administrator | [Exam Guide](https://trailhead.salesforce.com/credentials/administrator) |
| Service Cloud Consultant | [Exam Guide](https://trailhead.salesforce.com/credentials/servicecloudconsultant) |
| Experience Cloud Consultant | [Exam Guide](https://trailhead.salesforce.com/credentials/experiencecloudconsultant) |
