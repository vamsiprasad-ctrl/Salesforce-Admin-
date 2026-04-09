# 07 · Reports & Dashboards

> **Module:** Analytics | **Difficulty:** 🟡 Beginner–Intermediate | **Est. Time:** 4 hrs

---

## 7.1 What are Reports?

A **Report** is a structured view of your Salesforce data — filtered, grouped, sorted, and summarized to answer a specific business question.

Reports are built on **Report Types** which define which objects and fields are available. Every report runs in real time — you always see the current state of the data.

### Real-World Business Questions Reports Answer

- "How many open opportunities do we have this quarter, by stage?"
- "Which sales reps closed the most deals last month?"
- "What is the average resolution time for support cases by category?"
- "How many leads converted this week vs last week?"

---

## 7.2 The Four Report Formats

### Tabular Report

The simplest format — just a flat table of rows and columns. Like an Excel spreadsheet.

**Use when:** You need a simple list to export, or a row count.

**Limitations:** Cannot be used as a dashboard source.

### Real-World Example — Tabular

> **Report:** "All Open Cases"
>
> | Case Number | Subject | Account | Status | Created Date |
> |------------|---------|---------|--------|-------------|
> | 00001234 | Login Issue | Acme Corp | Open | 09-Apr-2026 |
> | 00001235 | Billing Error | TechStart | New | 09-Apr-2026 |
> | 00001236 | Feature Request | MegaCo | Working | 08-Apr-2026 |

---

### Summary Report

Rows are grouped by one or more fields. Subtotals and totals are calculated per group.

**Use when:** You need to group data and see counts/sums per group.

### Real-World Example — Summary

> **Report:** "Opportunities by Stage This Quarter"
>
> Grouped by `StageName`:
>
> | Stage | Count | Total Amount |
> |-------|-------|-------------|
> | Prospecting | 12 | $234,000 |
> | Needs Analysis | 8 | $480,000 |
> | Proposal | 15 | $1,200,000 |
> | Negotiation | 6 | $890,000 |
> | **Total** | **41** | **$2,804,000** |

---

### Matrix Report

Groups data by **both rows AND columns** — like a pivot table.

**Use when:** You need to compare two dimensions simultaneously.

### Real-World Example — Matrix

> **Report:** "Closed Won Opportunities by Rep and Quarter"
>
> | Rep | Q1 | Q2 | Q3 | Q4 | Total |
> |-----|----|----|----|----|-------|
> | Sarah | $120K | $145K | $98K | $210K | $573K |
> | James | $87K | $92K | $130K | $165K | $474K |
> | Priya | $200K | $180K | $220K | $310K | $910K |
> | **Total** | **$407K** | **$417K** | **$448K** | **$685K** | **$1.96M** |

---

### Joined Report

Combines **multiple report blocks** (up to 5) in a single view. Each block has its own data source, filters, and columns — but they share groupings.

**Use when:** You need to see data from multiple related reports side by side.

### Real-World Example — Joined

> **Report:** "Account Health Overview"
>
> **Block 1:** Open Opportunities by Account
> **Block 2:** Open Cases by Account
> **Block 3:** Last Activity Date by Account
>
> Sales manager can see at a glance: "Acme Corp has $500K in pipeline, 3 open cases, and hasn't been contacted in 15 days — flag for immediate outreach."

---

## 7.3 Custom Report Types

A **Custom Report Type (CRT)** defines which objects and fields are available when building a report — like a template for your report builder.

Standard report types cover common scenarios. You create CRTs when you need:
- Reports on custom objects
- Reports spanning 2–4 related objects
- Control over which fields are available in the report builder

### Real-World Example

> **Scenario:** "ConsultCo" has `Project__c` (custom) related to `Milestone__c` (custom) related to `Task` (standard).
>
> Standard report types don't cover this 3-object chain.
>
> **Custom Report Type:**
> - Primary Object: `Project__c`
> - Related Object 1: `Milestone__c` (with at least one related milestone)
> - Related Object 2: `Task` (with or without related tasks)
>
> Now users can build reports like "All Projects with Overdue Milestones and their Associated Tasks."

---

## 7.4 Bucket Fields

A **Bucket Field** lets you categorize field values into named groups on the fly — without changing the underlying data or creating a new field.

### Real-World Example

> **Report:** "Opportunities by Deal Size"
>
> The `Amount` field has hundreds of unique values. Bucket them:
>
> | Bucket Name | Criteria |
> |-------------|---------|
> | Small | Amount < $10,000 |
> | Medium | Amount >= $10,000 AND < $100,000 |
> | Large | Amount >= $100,000 AND < $500,000 |
> | Enterprise | Amount >= $500,000 |
>
> Now you can group by the bucket and see how many deals fall in each tier.

### Bucket Field Types

| Type | Field Type | Example |
|------|-----------|---------|
| **Numeric** | Number, Currency | Deal size ranges |
| **Picklist** | Picklist | Group Industries into "Tech", "Finance", "Other" |
| **Text** | Text | Group free-form entries by keyword |

---

## 7.5 Report Formulas

### Row-Level Formulas

Calculated per row — like adding a calculated column to your spreadsheet.

### Real-World Example — Row-Level Formula

> **Report:** Opportunity Pipeline
>
> **Formula Column:** `Expected_Revenue` = `Amount * (Probability / 100)`
>
> | Opportunity | Amount | Probability | Expected Revenue |
> |------------|--------|------------|-----------------|
> | Acme Deal | $100,000 | 75% | $75,000 |
> | TechCo Deal | $50,000 | 40% | $20,000 |
> | MegaDeal | $500,000 | 90% | $450,000 |

---

### Summary Formulas

Calculated at group or grand total level — aggregate calculations across grouped data.

### Real-World Example — Summary Formula

> **Report:** Sales Rep Performance
> Grouped by `Owner`
>
> **Summary Formula:** `Win_Rate` = `Record Count of Closed Won / Record Count of All`
>
> | Rep | Total Deals | Closed Won | Win Rate |
> |-----|------------|-----------|---------|
> | Sarah | 45 | 32 | 71.1% |
> | James | 38 | 22 | 57.9% |
> | Priya | 52 | 41 | 78.8% |

---

## 7.6 Report Export & Subscriptions

### Exporting Reports

Reports can be exported to:
- **Excel (.xlsx)**
- **CSV (.csv)** — comma-separated values
- **Formatted Excel** — preserves report groupings

**Path:** Run Report → Export button

> ⚠️ Exporting is controlled by the **"Export Reports"** system permission on the user's profile or permission set. Not all users should be able to export — data governance matters.

### Report Subscriptions

Schedule a report to automatically run and email results to subscribers.

**Path:** Run Report → Subscribe

| Setting | Options |
|---------|---------|
| Frequency | Daily, Weekly, Monthly |
| Condition | Always send, OR only when a condition is met |
| Recipients | Yourself, other users (up to 5) |
| Format | Run in Salesforce or attach as Excel/CSV |

### Real-World Example — Subscription with Condition

> **Report:** "Open Cases Older Than 5 Days"
>
> **Subscription:** Daily at 7:00 AM, send only if `Record Count > 0`
>
> The support manager only receives the email when there are actually overdue cases — no "empty" alerts.

---

## 7.7 Dashboards

A **Dashboard** is a visual display of up to **20 report-based components** — charts, tables, gauges, and metrics — on a single canvas.

**Path:** Reports → Dashboards → New Dashboard

### Dashboard Component Types

| Type | Best For |
|------|---------|
| **Bar Chart** | Comparing values across categories |
| **Line Chart** | Trends over time |
| **Pie / Donut** | Showing proportions of a whole |
| **Funnel** | Pipeline stages |
| **Gauge** | Progress toward a target (quota attainment) |
| **Metric** | A single number (total pipeline value) |
| **Table** | Top N list (top 10 reps by revenue) |

### Real-World Example — Sales Dashboard

> **"Q2 Sales Performance" Dashboard:**
>
> - **Top Left (Metric):** Total Open Pipeline = $4.2M
> - **Top Center (Gauge):** Team Quota Attainment = 68% (target: 100%)
> - **Top Right (Metric):** Deals Closed This Month = 23
> - **Middle (Bar Chart):** Pipeline by Stage — bars showing deal count per stage
> - **Bottom Left (Table):** Top 5 Reps by Closed Revenue
> - **Bottom Right (Line Chart):** Weekly New Opportunities Created — last 12 weeks
> - **Bottom Center (Pie Chart):** Pipeline by Industry

---

## 7.8 Dynamic Dashboards

A **Dynamic Dashboard** runs as the **viewing user** rather than a fixed "run as" user — so each person sees data filtered to their own access level.

| | Standard Dashboard | Dynamic Dashboard |
|--|-------------------|------------------|
| Data shown | Same for everyone (based on "run as" user) | Different per viewer (based on viewer's access) |
| Use case | Org-wide/team views | Individual performance views |
| Limit per org | Unlimited | Up to 10 (Professional), 25 (Enterprise) |

### Real-World Example

> **Dashboard:** "My Pipeline"
>
> **Standard Dashboard:** All reps see VP Sales' pipeline because the dashboard runs as the VP.
>
> **Dynamic Dashboard:** Each rep sees ONLY their own pipeline because it runs as whoever is viewing.
>
> One dashboard serves 200 reps — each one personalized automatically.

---

## 7.9 Dashboard Folders & Sharing

Dashboards and reports are stored in **Folders**. Folder access controls who can view, edit, or manage the contents.

| Access Level | What they can do |
|-------------|-----------------|
| **Viewer** | See reports/dashboards in the folder |
| **Editor** | Create and edit within the folder |
| **Manager** | All editor rights + manage sharing |

### Real-World Example

> **Folder structure:**
> - `Sales Leadership Reports` — Viewer: Sales Roles; Manager: VP Sales
> - `Finance Reports` — Viewer: Finance Role; Manager: CFO
> - `All Company Dashboards` — Viewer: All Internal Users; Manager: Admins

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Tabular | Flat list — simple, exportable, no grouping |
| Summary | Grouped with subtotals — most common format |
| Matrix | Two-dimension grouping — like a pivot table |
| Joined | Multiple data blocks in one report |
| Custom Report Type | Defines available objects and fields for a report category |
| Bucket Field | Group field values into categories on the fly |
| Row-Level Formula | Calculated column per row |
| Summary Formula | Calculated aggregate at group/total level |
| Dynamic Dashboard | Each viewer sees their own data — one dashboard, many perspectives |

---

## 🔗 Trailhead Resources

- [Reports & Dashboards Trail](https://trailhead.salesforce.com/content/learn/trails/explore-lightning-experience-reports-dashboards)
- [Quickstart Reports](https://trailhead.salesforce.com/content/learn/projects/quickstart-reports)
- [Reports & Dashboards Quick Look](https://trailhead.salesforce.com/content/learn/modules/reports-dashboards-quick-look)
- [Embed Reports & Dashboards](https://trailhead.salesforce.com/content/learn/projects/rd-embed-reports-dashboards)
- [Report Summary Formulas](https://trailhead.salesforce.com/content/learn/projects/rd-summary-formulas)

---

*Previous: [06 · Business Logic & Automation](./06_Business_Logic_and_Automation.md)*
*Next: [08 · Data Management →](./08_Data_Management.md)*
