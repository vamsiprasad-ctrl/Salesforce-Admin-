# 08 · Data Management

> **Module:** Data Management | **Difficulty:** 🟡 Intermediate | **Est. Time:** 4 hrs

---

## 8.1 Why Data Management Matters

Salesforce is only as powerful as the data inside it. Poor data quality leads to:
- Wrong reports → wrong business decisions
- Duplicate records → confused sales reps and duplicate outreach
- Missing records → lost revenue opportunities
- Inconsistent formats → broken automation rules

This section covers how to **get data in**, **keep it clean**, and **get it out** safely.

---

## 8.2 Data Import Wizard

The **Data Import Wizard** is Salesforce's browser-based, point-and-click import tool. No software to install.

**Path:** Setup → Data Import Wizard (or App Launcher → Data Import Wizard)

### Supported Objects

- Accounts and Contacts
- Leads
- Solutions
- Custom Objects

> ⚠️ Cannot import Opportunities, Cases, or Products via Data Import Wizard — use Data Loader for those.

### Limits

- Maximum **50,000 records** per import
- CSV file must be UTF-8 encoded

### How It Works — Step by Step

1. Choose object (Accounts, Contacts, Leads, or Custom)
2. Choose operation: Insert, Update, or Upsert
3. For Upsert: pick the matching field (Salesforce ID or External ID)
4. Upload CSV file
5. Map CSV columns to Salesforce fields
6. Review and import
7. Check results in the import status page

### Real-World Example

> **Scenario:** "StartupCo" just closed a deal to acquire 2,000 business cards from a trade show. The marketing team has a CSV with: First Name, Last Name, Company, Email, Phone, Job Title.
>
> **Steps:**
> 1. Import Wizard → Leads → Insert
> 2. Upload `tradeshow_leads.csv`
> 3. Map: "First Name" → `First Name`, "Company" → `Company`, etc.
> 4. Import — 2,000 Leads created in minutes

---

## 8.3 Data Loader

**Data Loader** is a desktop application (requires Java) that handles large-scale data operations — millions of records, any object.

**Download:** Setup → Data Management → Data Loader

### Operations

| Operation | What It Does |
|-----------|-------------|
| **Insert** | Create new records |
| **Update** | Modify existing records (must have Salesforce ID in CSV) |
| **Upsert** | Insert new + update existing in one operation (uses External ID) |
| **Delete** | Move records to Recycle Bin |
| **Hard Delete** | Permanently delete — bypasses Recycle Bin |
| **Export** | Extract records to a CSV file |
| **Export All** | Export including records in Recycle Bin |

### Real-World Example — Upsert

> **Scenario:** "ManufactureCo" syncs product data nightly from their ERP system into Salesforce. The ERP has a Product ID field (`ERP_ID__c`) which they've set as an External ID in Salesforce.
>
> Each night, their integration:
> 1. Exports all products from ERP to a CSV
> 2. Runs Data Loader **Upsert** on `Product2` using `ERP_ID__c` as the matching field
> 3. New products in ERP → **inserted** into Salesforce
> 4. Changed products in ERP → **updated** in Salesforce
> 5. Unchanged products → skipped (no duplicate inserts)
>
> Result: Salesforce always has accurate, current product data without manual intervention.

---

## 8.4 External ID

An **External ID** is a custom field marked with the "External ID" attribute. It allows:

1. **Upsert operations** — match on this field instead of Salesforce ID
2. **Importing related records** — reference a parent by External ID instead of Salesforce ID
3. **Deduplication** — External ID fields are automatically indexed and can be set to unique

### Real-World Example — Importing Related Records

> **Scenario:** Importing Contacts that belong to Accounts. Your CSV has Account names, not Salesforce Account IDs.
>
> **Without External ID:** You'd need a separate step to look up every Account's Salesforce ID and add it to the CSV — tedious for 10,000 contacts.
>
> **With External ID:**
> - Accounts have `Account_ERP_ID__c` (External ID, Unique) = the ERP's account number
> - Your Contact CSV has a column `Account_ERP_ID__c` = the account's ERP number
> - Data Loader Upsert maps this column to `Account:Account_ERP_ID__c`
> - Salesforce automatically looks up the Account by ERP ID and links the Contact
>
> No manual ID lookup needed.

---

## 8.5 Workbench

**Workbench** is a free, browser-based tool for data operations and Salesforce API exploration.

**URL:** [workbench.developerforce.com](https://workbench.developerforce.com)

| Capability | Details |
|-----------|---------|
| SOQL Queries | Query any object directly |
| Insert/Update/Delete/Upsert | Via CSV upload |
| REST Explorer | Test REST API calls |
| SOSL | Full-text search queries |
| Bulk API | Large data operations |

### Real-World Example

> **Scenario:** Admin needs to find all Contacts where `Email = null` that were created in the last 90 days.
>
> **Workbench SOQL:**
> ```sql
> SELECT Id, FirstName, LastName, AccountId, CreatedDate
> FROM Contact
> WHERE Email = null
> AND CreatedDate = LAST_N_DAYS:90
> ORDER BY CreatedDate DESC
> ```
>
> Results downloaded to CSV → imported back with corrected emails.

---

## 8.6 Data Export

**Data Export** creates a full backup of your org's data as a set of CSV files.

**Path:** Setup → Data Management → Data Export

| Schedule | Detail |
|----------|--------|
| Weekly | Available in Enterprise, Unlimited |
| Monthly | Available in all editions |
| Manual | Export on demand at any time |

> 💡 Always export before any bulk data migration or major automation deployment. Data Export is your safety net.

---

## 8.7 Duplicate Management

### Matching Rules

A **Matching Rule** defines what makes two records "duplicates" — which fields to compare and how.

**Path:** Setup → Duplicate Management → Matching Rules

### Duplicate Rules

A **Duplicate Rule** defines what Salesforce does when a matching rule detects a duplicate: Block, Alert, or Allow but report.

**Path:** Setup → Duplicate Management → Duplicate Rules

### Real-World Example

> **Company:** "SalesMax" — reps keep creating duplicate Lead records for the same person.
>
> **Matching Rule:** Two Leads are duplicates if `Email` is an exact match OR (`First Name` + `Last Name` + `Company`) match with fuzzy text comparison.
>
> **Duplicate Rule on Lead:** When a match is found during **Insert**:
> - Action: **Block** — show error message "A Lead already exists for this person. Please update the existing record instead."
> - Show duplicate list: ✅ (let user see which record already exists)
>
> Result: Duplicate Leads drop by 85%. Reps are forced to work existing records.

---

## 8.8 Tool Comparison

| Tool | Max Records | Objects Supported | Requires Install | Supports Upsert | Ideal Use |
|------|------------|------------------|-----------------|-----------------|-----------|
| Data Import Wizard | 50,000 | Limited (Accounts, Contacts, Leads, Custom) | No | Yes | Quick one-time imports |
| Data Loader | 5M+ | All objects | Yes (Java) | Yes | Large-scale, scheduled, automated |
| Workbench | API limits | All objects | No (browser) | Yes | Dev/admin ad hoc work |
| Data Export | All records | All objects | No | N/A | Backup / full export |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Data Import Wizard | Browser-based, up to 50K records, limited objects |
| Data Loader | Desktop app, millions of records, all objects |
| External ID | Custom field that enables upsert and relationship linking |
| Upsert | Insert new + update existing in one pass |
| Workbench | Browser tool for SOQL, API testing, and data operations |
| Duplicate Management | Matching Rules define what = duplicate; Duplicate Rules define what to do |

---

## 🔗 Trailhead Resources

- [Data Management (Lightning Experience)](https://trailhead.salesforce.com/content/learn/modules/lex_implementation_data_management)
- [Data Management Fundamentals](https://trailhead.salesforce.com/content/learn/modules/data-management-fundamentals)
- [Salesforce Data Loader Guide](https://www.apexhours.com/salesforce-data-loader/)
- [Import & Export with Data Management Tools](https://trailhead.salesforce.com/content/learn/projects/import-and-export-with-data-management-tools)
- [Data Quality](https://trailhead.salesforce.com/content/learn/modules/data_quality)

---

*Previous: [07 · Reports & Dashboards](./07_Reports_and_Dashboards.md)*
*Next: [09 · Sales Cloud →](./09_Sales_Cloud.md)*
