# 07 · Reports & Dashboards

> **Difficulty:** 🟡 Beginner–Intermediate | **Est. Time:** 4 hrs

---

## 7.1 Report Formats

| Format | Structure | Use For | Dashboard Source? |
|--------|-----------|---------|------------------|
| **Tabular** | Flat rows & columns | Lists, exports | ❌ No |
| **Summary** | Grouped rows with subtotals | Sales by region, pipeline by stage | ✅ Yes |
| **Matrix** | Rows AND columns grouped | Quarterly comparison by rep | ✅ Yes |
| **Joined** | Multiple blocks side by side | Multi-object analysis | ✅ Yes |

---

## 7.2 Custom Report Types

Define which objects and fields are available in the report builder. Needed for custom objects and multi-level relationships.

---

## 7.3 Bucket Fields

Group field values into named ranges **on the fly** without changing data.

> **Example:** Bucket `Amount` into Small (<10K), Medium (10K–100K), Large (100K–500K), Enterprise (>500K)

---

## 7.4 Report Formulas

- **Row-Level:** Calculated per row — `Amount * (Probability/100)` → Expected Revenue column
- **Summary:** Calculated per group — `Closed Won Count / Total Count` → Win Rate %

---

## 7.5 Dashboards & Dynamic Dashboards

- **Dashboard:** Up to 20 components — bar, line, pie, funnel, gauge, metric, table
- **Dynamic Dashboard:** Each viewer sees their own data (based on their access). One dashboard, personalized for everyone.

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Build a Summary Report

**Objective:** Analyze Job Applications by Status.

**Steps:**
1. Reports tab → **New Report**
2. Report Type: **Job Applications** *(or create a CRT first — see Task 2)*
3. Format: **Summary**
4. Group Rows by: `Application_Status__c`
5. Add columns: Applicant Name, Job Posting, Application Date, Application Status
6. Add filter: `Application_Date__c = THIS_YEAR`
7. Run report → observe counts per status group
8. Save as `Applications by Status — This Year`

---

### 🔧 Task 2 — Create a Custom Report Type

**Objective:** Enable reports spanning Job Postings AND Job Applications.

**Steps:**
1. Setup → Report Types → **New Custom Report Type**
2. Primary Object: `Job Postings`
3. Label: `Job Postings with Applications`
4. Category: Other
5. Deployment Status: Deployed
6. Next → add related object: `Job Applications` (via the Master-Detail relationship)
7. Relationship: "Each A must have at least one related B" — or choose the appropriate option
8. Save
9. Return to Reports → New Report → find your new type
10. Build a report showing Job Postings with columns: Job Title, Company, Total Applications, Application Deadline

---

### 🔧 Task 3 — Build a Matrix Report

**Objective:** Compare applications across Job Types by month.

**Steps:**
1. New Report using your Custom Report Type
2. Format: **Matrix**
3. Group Rows by: `Job_Type__c`
4. Group Columns by: `Application_Date__c` → grouped by **Calendar Month**
5. Summary: Count of records in each cell
6. Run → observe the matrix grid
7. Save as `Applications by Type and Month`

---

### 🔧 Task 4 — Add a Bucket Field

**Objective:** Categorize job postings by salary range tiers.

**Steps:**
1. Open or create a report on Job Postings
2. In the report builder, click **Add Column** → find `Salary Range`
3. Click the column dropdown → **Bucket this Field**
4. Create buckets:
   - Entry Level: 0 to 500,000
   - Mid Level: 500,001 to 1,500,000
   - Senior Level: 1,500,001 to 3,000,000
   - Executive: > 3,000,000
5. Name the bucket column: `Salary Tier`
6. Group the report by `Salary Tier`
7. Run and save

---

### 🔧 Task 5 — Build a Dashboard

**Objective:** Create a Job Portal operational dashboard.

**Steps:**
1. Dashboards → **New Dashboard**
2. Name: `Job Portal Operations`
3. Add 5 components:
   - **Metric** → Source: Applications by Status → Total record count → label: "Total Applications"
   - **Donut Chart** → Source: Applications by Status → show breakdown by status
   - **Bar Chart** → Source: Applications by Type and Month → applications per job type
   - **Table** → Source: Open Postings report → top 10 by Total Applications
   - **Metric** → Source: new report → count of Postings with deadline this week
4. Arrange components in a 2-column layout
5. Save and run

---

### 🔧 Task 6 — Set Up a Report Subscription

**Objective:** Schedule a report to email you automatically.

**Steps:**
1. Open the `Applications by Status — This Year` report
2. Click **Subscribe**
3. Frequency: **Weekly**, every **Monday at 8:00 AM**
4. Conditions: Send only if **Record Count > 0**
5. Add yourself as a recipient
6. Format: Salesforce formatted report
7. Save

---

## 📝 Practice Questions

**Q1.** Which report format can NOT be used as a dashboard source?

**Q2.** What is the difference between a Row-Level Formula and a Summary Formula?

**Q3.** A Dynamic Dashboard has a maximum limit per org based on Edition. What is the limit for Enterprise Edition?

**Q4.** Can a Tabular report be used to power a Dashboard chart?

**Q5.** What does "grouping" mean in a Summary report?

**Q6.** You want to show "Top 10 reps by revenue" on a dashboard. Which component type would you use?

**Q7.** A Bucket Field groups the values of the `Amount` field into Small/Medium/Large. Does this change the underlying field data?

**Q8.** Who can see a report saved in a private folder?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Report Design**

> The Sales Director asks for a single report that shows, for each sales rep:
> - Total number of Opportunities created this quarter
> - Total Closed Won amount this quarter
> - Win Rate (%) for the quarter
> - Average deal size
>
> **Question:** What report format would you use? How would you configure groupings? Which of the four metrics require a Summary Formula vs just a standard field?

---

**Scenario 2 — Dashboard Design**

> The HR Head wants a daily dashboard for the Job Portal showing:
> - Total open positions
> - Total applications received this month
> - Application status breakdown (pie chart)
> - Top 5 most applied-for positions (table)
> - Trend of applications over the last 12 weeks (line chart)
>
> **Question:** For each component, specify: component type, source report format, and any special configuration needed. Can a Tabular report be used for any of these?

---

**Scenario 3 — Dynamic vs Standard Dashboard**

> "TechBridge" has 200 sales reps. The VP wants each rep to see a dashboard called "My Pipeline" showing only their own Opportunities. The VP also wants his own dashboard "Full Pipeline" showing everyone's data.
>
> **Question:**
> 1. Which type of dashboard (standard or dynamic) suits each use case and why?
> 2. How do you configure the "run as" setting for each?
> 3. What is the limit for Dynamic Dashboards in Enterprise Edition?
> 4. If the limit is reached, what alternative approach could you use?

---

## ✔️ Answer Key

1. **Tabular** report format cannot be used as a dashboard component source.
2. **Row-Level Formula:** Calculated per individual record row (like a spreadsheet column). **Summary Formula:** Calculated at group subtotals or grand total level — aggregates across records.
3. **25 Dynamic Dashboards** per org in Enterprise Edition.
4. **No** — Tabular reports cannot power dashboard charts. Use Summary or Matrix.
5. Grouping organizes records into categories based on a field value, showing subtotals for each group (e.g., group by Stage → see count and sum per stage).
6. **Table** component — configured to show top 10 records sorted by revenue descending.
7. **No** — Bucket Fields categorize values for display in the report only. The underlying field data in Salesforce is unchanged.
8. Only the **owner** of the private folder (and System Administrators) can see reports in private folders.

---

## 🔗 Trailhead Resources

- [Reports & Dashboards Trail](https://trailhead.salesforce.com/content/learn/trails/explore-lightning-experience-reports-dashboards)
- [Quickstart Reports](https://trailhead.salesforce.com/content/learn/projects/quickstart-reports)
- [Report Summary Formulas](https://trailhead.salesforce.com/content/learn/projects/rd-summary-formulas)

---

*Previous: [06 · Automation](./06_Business_Logic_and_Automation.md) | Next: [08 · Data Management →](./08_Data_Management.md)*

---
---

# 08 · Data Management

> **Difficulty:** 🟡 Intermediate | **Est. Time:** 4 hrs

---

## 8.1 Tool Comparison

| Tool | Max Records | Objects | Requires Install | Upsert | Best For |
|------|------------|---------|-----------------|--------|---------|
| Data Import Wizard | 50,000 | Limited (Accounts, Contacts, Leads, Custom) | No | Yes | One-time imports |
| Data Loader | 5M+ | All | Yes (Java) | Yes | Large, automated, scheduled |
| Workbench | API limits | All | No (browser) | Yes | Dev/admin ad hoc |
| Data Export | All | All | No | N/A | Full org backup |

---

## 8.2 External ID

A custom field marked "External ID" — enables Upsert and relationship-based imports.

### Real-World Example
> ManufactureCo syncs products nightly from SAP using `ERP_ID__c` as External ID. Data Loader Upsert: new SAP products → inserted, changed products → updated, unchanged → skipped. Zero duplicates.

---

## 8.3 Upsert

**Insert** new records + **Update** existing records in one operation, matched by External ID or Salesforce ID.

---

## 8.4 Duplicate Management

- **Matching Rule:** Defines what makes two records duplicates (field comparison logic)
- **Duplicate Rule:** Defines what happens when a match is found — Block, Alert, or Allow + log

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Import Records with Data Import Wizard

**Objective:** Import Job Postings from a CSV file.

**Prepare CSV** (create in Excel/Google Sheets):
```
Job_Title,Company_Name,Job_Type,Application_Deadline,Salary_Range
Software Engineer,TechCorp,Full-Time,2026-12-31,1200000
Marketing Manager,BrandCo,Full-Time,2026-11-30,900000
QA Analyst,QualityFirst,Contract,2026-10-15,600000
Data Scientist,DataLabs,Full-Time,2026-12-15,1500000
HR Recruiter,PeopleFirst,Part-Time,2026-10-31,400000
```

**Steps:**
1. App Launcher → **Data Import Wizard**
2. "What kind of records are you importing?" → Custom Objects → `Job_Postings`
3. "What do you want to do?" → **Add new records**
4. Upload your CSV
5. Map columns to fields:
   - `Job_Title` → `Name` (Job Title field)
   - `Company_Name` → `Company_Name__c`
   - `Job_Type` → `Job_Type__c`
   - `Application_Deadline` → `Application_Deadline__c`
   - `Salary_Range` → `Salary_Range__c`
6. Start Import
7. Monitor progress in the import queue

---

### 🔧 Task 2 — Create an External ID Field and Upsert

**Objective:** Perform an upsert operation using External ID.

**Steps:**
1. Object Manager → `Job_Posting__c` → Fields → **New**
2. Field Type: **Text**
3. Label: `External Job ID`
4. Check: **External ID** ✅, **Unique** ✅
5. API Name: `External_Job_ID__c`
6. Save

**Prepare upsert CSV** (update 2 existing + insert 2 new):
```
External_Job_ID,Job_Title,Company_Name,Job_Type
EXT-001,Senior Software Engineer,TechCorp,Full-Time
EXT-002,Content Strategist,BrandCo,Full-Time
EXT-003,DevOps Engineer,TechCorp,Full-Time
EXT-004,Data Analyst,DataLabs,Contract
```

7. Download and install **Data Loader** from Setup → Data Management → Data Loader
8. Open Data Loader → **Upsert**
9. Select `Job_Posting__c`
10. Upload CSV → Map fields
11. External ID field: `External_Job_ID__c`
12. Complete the upsert
13. Check results: 2 records inserted (EXT-003, EXT-004), 2 updated (EXT-001, EXT-002)

---

### 🔧 Task 3 — Export Data with Data Loader

**Objective:** Export all Job Applications to a CSV.

**Steps:**
1. Data Loader → **Export**
2. Object: `Job_Application__c`
3. Create a SOQL query:
```sql
SELECT Id, Name, Application_Status__c, Application_Date__c,
       Job_Posting__r.Name, Applicant_Email__c
FROM Job_Application__c
WHERE Application_Date__c = THIS_YEAR
ORDER BY Application_Date__c DESC
```
4. Choose export location → Finish
5. Open the CSV — verify all records exported correctly

---

### 🔧 Task 4 — Configure Duplicate Management

**Objective:** Prevent duplicate Job Applications from the same email.

**Steps:**
1. Setup → Duplicate Management → Matching Rules → **New**
2. Object: `Job_Application__c`
3. Rule Name: `Duplicate Application by Email`
4. Matching Criteria: Field `Applicant_Email__c`, Match Blank Fields: No, Matching Method: Exact
5. Save → **Activate**

6. Setup → Duplicate Management → Duplicate Rules → **New**
7. Object: `Job_Application__c`
8. Name: `Block Duplicate Applications`
9. Record-Level Security: Enforce sharing rules
10. Action on Create: **Block**
11. Action on Edit: **Allow** + Report
12. Conditions: Use your matching rule
13. Save → **Activate**

**Test:** Create two Job Applications with the same `Applicant_Email__c` for the same Job Posting → second one should be blocked.

---

### 🔧 Task 5 — Run a Data Export

**Objective:** Understand org-level data backup.

**Steps:**
1. Setup → Data Management → **Data Export**
2. Click **Export Now** (or Schedule Export)
3. Include images and documents: as needed
4. Select objects to export (select all for a full backup)
5. Start Export
6. When complete, download the ZIP file
7. Open and examine the CSV files — one per object

---

## 📝 Practice Questions

**Q1.** What is the maximum number of records the Data Import Wizard can handle in one import?

**Q2.** Which Data Loader operation permanently deletes records without sending them to the Recycle Bin?

**Q3.** You want to import 500 Contact records that belong to Accounts. Your CSV has the Account's External ID but not the Salesforce Account ID. How do you link Contacts to Accounts during import?

**Q4.** What is the difference between a Matching Rule and a Duplicate Rule?

**Q5.** Can Data Import Wizard import Opportunity records?

**Q6.** What happens to a record in Salesforce's Recycle Bin? How long does it stay there?

**Q7.** You run a Data Loader Export and a separate Data Export from Setup. What is the key difference in what they produce?

**Q8.** What are three operations Data Loader supports that Data Import Wizard does NOT?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Data Migration Planning**

> "NewCo" is migrating from HubSpot to Salesforce. They have:
> - 15,000 Company records (become Accounts)
> - 45,000 Contact records (linked to Companies)
> - 8,000 Deal records (become Opportunities, linked to Accounts)
>
> **Question:**
> 1. In what order must the records be imported, and why?
> 2. Which import tool would you use for each object, and why?
> 3. How would you link Contacts to Accounts during import without knowing Salesforce Account IDs?
> 4. What pre-import steps would you take to ensure data quality?

---

**Scenario 2 — External ID Strategy**

> "RetailChain" has a master product catalog in their ERP system. They sync product data to Salesforce nightly. The ERP product ID is a 10-digit number like `0012345678`. They want to ensure that:
> - New ERP products are created in Salesforce
> - Updated ERP products are reflected in Salesforce
> - No duplicate products are created even if the sync runs twice
>
> **Question:** Design the complete External ID and Upsert strategy for this use case.

---

## ✔️ Answer Key

1. **50,000 records** per import in Data Import Wizard.
2. **Hard Delete** operation — bypasses the Recycle Bin permanently.
3. In Data Loader, map the `Account_External_ID__c` column to `Account:ExternalIDFieldName`. Salesforce will look up the Account by External ID and link the Contact automatically.
4. A **Matching Rule** defines *how* to identify duplicates (which fields to compare). A **Duplicate Rule** defines *what to do* when duplicates are found (Block, Alert, or Allow).
5. **No** — Data Import Wizard does not support Opportunities. Use Data Loader for Opportunities.
6. Deleted records go to the Recycle Bin and remain there for **15 days**, after which they are permanently deleted automatically. Admins can restore records within this window.
7. **Data Loader Export** runs a SOQL query and exports specific fields you select — granular and targeted. **Data Export from Setup** exports your entire org's data as a set of CSV files per object — a full backup.
8. Data Loader supports: **Delete**, **Hard Delete**, and **Export All** (includes Recycle Bin records) — none of which are available in Data Import Wizard.

---

## 🔗 Trailhead Resources

- [Data Management Fundamentals](https://trailhead.salesforce.com/content/learn/modules/data-management-fundamentals)
- [Import & Export with Data Management Tools](https://trailhead.salesforce.com/content/learn/projects/import-and-export-with-data-management-tools)
- [Data Quality](https://trailhead.salesforce.com/content/learn/modules/data_quality)

---

*Previous: [07 · Reports](./07_Reports_and_Dashboards.md) | Next: [09 · Sales Cloud →](./09_Sales_Cloud.md)*
