# 05 · Customization & UI

> **Difficulty:** 🟢 Beginner | **Est. Time:** 3 hrs

---

## 5.1 Classic vs Lightning Experience

| Feature | Classic | Lightning |
|---------|---------|-----------|
| Design | 2000–2015 tabbed UI | Modern card-based |
| Mobile Ready | Limited | Fully responsive |
| Lightning App Builder | ❌ | ✅ |
| Dynamic Forms | ❌ | ✅ |
| Activity Timeline | ❌ | ✅ |
| Future features | ❌ None | ✅ All new features |

> Salesforce is in **maintenance mode** for Classic. All new features are Lightning-only. Migrate now.

---

## 5.2 Apps

An **App** groups tabs, branding, and navigation for a specific business purpose. Accessed from the App Launcher (grid icon top-left).

### Real-World Example
> **ConstructCo** creates three apps:
> 1. **Sales App** — Accounts, Contacts, Opportunities, Leads
> 2. **Projects App** — Projects, Milestones, Resources, Timesheets
> 3. **Safety App** — Inspection Checklists, Incidents, Equipment Certs

---

## 5.3 Lightning App Builder

Drag-and-drop page builder for three page types — no code required.

| Page Type | What It Customizes | Assigned Via |
|-----------|-------------------|-------------|
| **Home Page** | Default landing page | App + Profile |
| **Record Page** | What appears when opening a specific record | App + Profile + Record Type |
| **App Page** | Standalone tab within a Lightning app | App |

### Real-World Example
> Sales team wants Opportunity record redesigned:
> - Left 70%: Fields, Key Metrics component, Activity Timeline
> - Right 30%: Chatter, Related Contacts, Related Files
> - Bottom: Related Quotes, Quote History
>
> Admin opens App Builder → selects Opportunity record page → drags components → activates for "Sales" app + "Sales Rep" profile.

---

## 5.4 Dynamic Forms

Show or hide **individual fields** based on conditions — directly in the App Builder, no extra page layouts.

### Real-World Example
> `Competitor_Name__c` only appears when `Has_Competition__c = True`.
> Without Dynamic Forms: separate page layouts for each scenario.
> With Dynamic Forms: one layout, one visibility rule on the field.

---

## 5.5 List Views

Filtered, sortable tables of records for any object.

### Real-World Example
> **Sales Manager custom list views:**
> 1. **"Hot Deals This Quarter"** — Stage = Proposal/Negotiation, Close Date = This Qtr, Amount > $50K
> 2. **"Stale Opportunities"** — Last Activity < 30 days, Stage ≠ Closed
> 3. **"Team Pipeline"** — Owner Role includes all Sales Rep subordinates

---

## 5.6 Compact Layouts

Defines which fields appear in the **Highlights Panel** at the top of a record and on mobile cards.

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Create a Custom Lightning App

**Objective:** Build a focused app for a specific team.

**Steps:**
1. Setup → App Manager → **New Lightning App**
2. App Name: `Job Portal`
3. Developer Name: `Job_Portal`
4. Navigation Style: Standard
5. Add tabs: Job Postings, Job Applications, Accounts, Contacts, Reports
6. Assign to profiles: System Administrator, Sales Representative
7. Save
8. Test: Click App Launcher → find "Job Portal" → open it

---

### 🔧 Task 2 — Customize a Record Page with App Builder

**Objective:** Build a purpose-driven record page for Job Postings.

**Steps:**
1. Open a `Job_Posting__c` record
2. Click the **gear icon** on the record → **Edit Page**
3. Lightning App Builder opens
4. Rearrange the layout:
   - Move the **Highlights Panel** to the very top
   - Add a **two-column section** below
   - Left column: **Details** component (all fields)
   - Right column: **Related List** component → select Job Applications
   - Bottom: **Activity Timeline**
5. Click **Save** → **Activate**
6. Set as default for the "Job Portal" app
7. Verify your layout is live on the Job Posting record

---

### 🔧 Task 3 — Add Dynamic Form Visibility Rules

**Objective:** Show fields conditionally based on values.

**Steps:**
1. Open Lightning App Builder on your `Job_Posting__c` record page
2. Click on the Details component → switch to **Dynamic Forms** (if prompted)
3. Click on the `Salary_Range__c` field
4. In the right panel, find **Visibility** → click **Add Filter**
5. Set filter: Field = `Job_Type__c`, Operator = `Equals`, Value = `Full-Time`
6. Save and activate
7. **Test:** Open a Contract-type Job Posting → Salary Range field should be hidden. Change type to Full-Time → field appears.

---

### 🔧 Task 4 — Create and Share a Custom List View

**Objective:** Build a shared list view for a team.

**Steps:**
1. Go to the Job Postings tab
2. Click the List View Controls icon → **New**
3. Name: `Open Postings This Month`
4. Visibility: Share with all users
5. Add filters:
   - `Application_Deadline__c >= THIS_MONTH`
   - `Is_Remote__c = True` (or any active filter)
6. Select columns: Job Title, Company, Job Type, Application Deadline, Total Applications
7. Save

---

### 🔧 Task 5 — Configure a Compact Layout

**Objective:** Surface key fields in the record's highlights panel.

**Steps:**
1. Setup → Object Manager → `Job_Posting__c` → Compact Layouts → **New**
2. Name: `Job Posting Compact`
3. Add fields (in order): Job Title, Company Name, Job Type, Application Deadline, Is Remote
4. Save
5. Click **Compact Layout Assignment** → assign `Job Posting Compact` as default
6. Open any Job Posting → observe the Highlights Panel at the top now shows your selected fields

---

## 📝 Practice Questions

**Q1.** What are the three types of pages you can build in Lightning App Builder?

**Q2.** What is the difference between a List View and a Report? When would you use each?

**Q3.** Dynamic Forms are a feature of which UI framework — Classic or Lightning?

**Q4.** A Lightning App can be assigned to multiple profiles. True or False?

**Q5.** What does the Compact Layout control, and where does it appear?

**Q6.** You want to show different fields on an Opportunity record page depending on whether the user is a Sales Rep or a Sales Manager. What tool would you use to achieve this?

**Q7.** A user says "I can't see the Job Portal app in my App Launcher." What is the most likely cause?

**Q8.** Can you add a Report Chart component directly to a Lightning Record Page?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — App Design**

> "TalentCo" uses Salesforce for three teams: Recruiters (manage Job Postings, Candidates), Account Managers (manage client Accounts, Contacts, Opportunities), and HR (manage Employee records). Each team complains that the current app shows too many irrelevant tabs.
>
> **Question:** Design three separate apps — name them, list the tabs each should contain, and explain how you control which app each team sees.

---

**Scenario 2 — Dynamic Forms Design**

> On the Opportunity record, the sales team wants:
> - `Competitor_Name__c` — only visible when `Has_Competition__c = True`
> - `Legal_Review_Date__c` — only visible when `Amount > 500,000`
> - `Partner_Name__c` — only visible when `Deal_Type__c = "Partner-Led"`
>
> **Question:** Can all three be achieved with Dynamic Forms? Walk through the visibility rule configuration for each.

---

**Scenario 3 — User Adoption Issue**

> After going live with Lightning Experience, 30% of sales reps have switched themselves back to Classic (Salesforce allows this toggle). The sales manager says "the new UI is confusing, there's too much on the screen."
>
> **Question:** What three App Builder/customization actions would you take to improve the Lightning Experience for sales reps and reduce their desire to switch back to Classic? Be specific about which components you would add, remove, or rearrange.

---

## ✔️ Answer Key — Practice Questions

1. **Home Page**, **Record Page**, **App Page**
2. A List View is a live filtered table for one object — real-time, interactive. A Report is a structured query across objects with grouping, summaries, and formulas — better for analysis and scheduling. Use List Views for daily working; Reports for analysis and dashboards.
3. **Lightning Experience** — Dynamic Forms is a Lightning-only feature.
4. **True** — a Lightning App can be assigned to multiple profiles. Users with those profiles see the app in their App Launcher.
5. Compact Layout controls the fields shown in the **Highlights Panel** at the top of a record page and in **Mobile list cards**.
6. **Lightning App Builder** with **component visibility filters** — add a filter like "Profile = Sales Manager" to show/hide specific sections or components per profile.
7. The app has not been **assigned to that user's profile**. Go to App Manager → edit the app → add the user's profile to the Assigned Profiles.
8. **Yes** — the Report Chart component can be added to any Lightning Record Page. It displays a single chart from an existing report.

---

## 🔗 Trailhead Resources

- [Lightning Experience Customization](https://trailhead.salesforce.com/content/learn/modules/lex_customization)
- [Lightning App Builder](https://trailhead.salesforce.com/content/learn/modules/lightning_app_builder)
- [List View: Quick Check](https://trailhead.salesforce.com/content/learn/modules/list-view-quick-check)

---

*Previous: [04 · Security](./04_Security_and_Access_Control.md) | Next: [06 · Automation →](./06_Business_Logic_and_Automation.md)*
