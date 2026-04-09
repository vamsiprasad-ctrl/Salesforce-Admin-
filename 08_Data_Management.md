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
