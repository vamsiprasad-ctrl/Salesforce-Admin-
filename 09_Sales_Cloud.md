# 09 · Sales Cloud

> **Module:** Sales Cloud | **Difficulty:** 🟡 Beginner–Intermediate | **Est. Time:** 8 hrs

---

## 9.1 The Sales Lifecycle in Salesforce

Sales Cloud maps directly to the stages of a typical B2B sales process:

```
Marketing generates interest
         ↓
    LEAD created (unqualified prospect)
         ↓
    Qualification — is this a real opportunity?
         ↓
    LEAD CONVERTED → ACCOUNT + CONTACT + OPPORTUNITY
         ↓
    Opportunity moves through STAGES
         ↓
    CLOSED WON → Products, Price Book, Line Items applied
         ↓
    Campaign Influence tracked for marketing attribution
```

---

## 9.2 Leads

A **Lead** represents an unqualified prospect — someone who has expressed interest but hasn't yet been verified as a real business opportunity.

Leads are kept **separate** from Accounts and Contacts deliberately — to keep your CRM clean until the lead is qualified.

### Lead Fields

| Field | Purpose |
|-------|---------|
| Name | First + Last name |
| Company | Prospect's company |
| Email / Phone | Contact details |
| Lead Source | How they found you (Web, Trade Show, Referral) |
| Status | New, Working, Qualified, Unqualified, Converted |
| Rating | Hot, Warm, Cold |
| Industry | Prospect's industry |

### Real-World Example

> **Scenario:** "TechBridge" — software company. At a trade show, rep Sarah scans 300 badges. Each person becomes a **Lead** in Salesforce with Status = "New" and Lead Source = "Trade Show".
>
> Over the next week, the sales team calls each lead:
> - Most → "Not interested" → Status = "Unqualified"
> - Some → "Interested, send proposal" → Status = "Qualified" → Convert

---

## 9.3 Web-to-Lead

**Web-to-Lead** automatically creates a Lead record when someone fills out a form on your website.

**Path:** Setup → Marketing → Web-to-Lead

Salesforce generates an HTML form snippet you embed on your website. When someone submits the form, a Lead is created automatically.

### Real-World Example

> **Scenario:** "CloudSoft" embeds a "Request a Demo" form on their website.
>
> Form fields: First Name, Last Name, Company, Email, Phone, Job Title, Company Size
>
> When a visitor submits the form:
> 1. Salesforce creates a Lead record instantly
> 2. A Flow triggers: If `Company_Size__c > 500` → assign to Enterprise Sales Queue; else → assign to SMB rep
> 3. An email alert is sent to the assigned rep: "New demo request from Sarah at Acme Corp!"
>
> Response time goes from 24 hours to 15 minutes.

---

## 9.4 Lead Conversion

**Lead Conversion** is the moment a Lead becomes a qualified prospect. When you convert a Lead, Salesforce creates:
- A new **Account** (or matches to an existing one)
- A new **Contact** (under that Account)
- Optionally, a new **Opportunity**

The Lead record is then **locked** (Status = Converted). It cannot be unconverted.

### Real-World Example

> **Scenario:** Lead: "John Smith, VP Engineering at Acme Corp — interested in a $200K deal"
>
> Rep clicks **Convert**:
> - Account: "Acme Corp" — Salesforce finds an existing Account → links to it
> - Contact: "John Smith" — created under Acme Corp
> - Opportunity: "Acme Corp — Enterprise License Q2" — created with Amount = $200,000, Close Date = June 30
>
> All activities logged on the Lead (calls, emails) now appear on John's Contact and the new Opportunity via the Activity Timeline.

### What happens to Lead data?

Lead fields are **mapped** to Account, Contact, and Opportunity fields on conversion. Admins can customize this mapping.

**Path:** Setup → Lead Settings → Map Lead Fields

---

## 9.5 Accounts

An **Account** represents a company (or individual consumer) that you do business with or track.

### Account Types

| Type | Description |
|------|-------------|
| **Business Account** | A company — B2B relationships |
| **Person Account** | An individual consumer — B2C (must be enabled separately) |

### Account Hierarchy

Accounts can be related to each other in a **parent-child hierarchy** using the `Parent Account` field (a self-relationship).

### Real-World Example

> **Company:** "GlobalTech" is the parent company.
> - GlobalTech USA (child)
> - GlobalTech EMEA (child)
>   - GlobalTech UK (grandchild of GlobalTech, child of EMEA)
>
> By navigating the Account hierarchy, reps and account managers can see the full corporate family tree and understand the total relationship value across the enterprise.

---

## 9.6 Contacts

A **Contact** is a person — associated with an Account. Contacts represent the humans you actually speak to at companies.

### Key Contact Features

| Feature | Description |
|---------|-------------|
| **Account relationship** | Every Contact can be linked to an Account (but can be private if no Account) |
| **Role on Account** | Economic Buyer, Technical Evaluator, Champion, End User |
| **Roles on Opportunities** | A Contact can play a specific role in an Opportunity (see Contact Roles) |
| **Activity Timeline** | All calls, emails, meetings logged against the Contact |

### Real-World Example

> **Account:** "Acme Corp"
> **Contacts at Acme Corp:**
> - John Smith — VP Engineering (Technical Evaluator on the deal)
> - Lisa Chen — CFO (Economic Buyer — signs the contract)
> - Mike Patel — IT Admin (End User — will actually use the software)
>
> The sales rep tracks all three contacts on the Opportunity Contact Roles — understanding the buying committee is critical to closing enterprise deals.

---

## 9.7 Account Teams & Opportunity Teams

### Account Team

Multiple internal users collaborating on a single **Account**, each with a defined role and access level.

### Real-World Example — Account Team

> **Account:** "MegaCorp" — $5M annual contract
> - **Account Owner:** Sarah (Enterprise Sales Rep) — primary relationship, Read/Write/Delete
> - **Account Team Member:** James (Solutions Engineer) — technical demos, Read/Write
> - **Account Team Member:** Priya (Customer Success) — post-sale relationship, Read Only

### Opportunity Team

Multiple users collaborating on a specific **Opportunity**, each with a role.

### Real-World Example — Opportunity Team

> **Opportunity:** "MegaCorp Digital Transformation — $2M"
> - **Opportunity Owner:** Sarah — leads the deal
> - **Team Member:** James (Solutions Architect) — builds the technical proposal
> - **Team Member:** Rohan (Legal) — reviews the contract terms
> - **Team Member:** Wei (Finance) — approves the pricing model

---

## 9.8 Opportunities

An **Opportunity** is a potential revenue-generating deal. It tracks the entire sales cycle from initial contact to closed/lost.

### Opportunity Key Fields

| Field | Purpose |
|-------|---------|
| Opportunity Name | Descriptive deal name |
| Account Name | Which company is this deal with |
| Close Date | Projected close date |
| Amount | Expected deal value |
| Stage | Current stage in the sales process |
| Probability | Auto-populated based on stage; can be overridden |
| Lead Source | How the opportunity originated |
| Type | New Business, Renewal, Expansion |

### Opportunity Stages

Each org customizes their stages to match their sales process. A typical B2B setup:

| Stage | Probability | Meaning |
|-------|------------|---------|
| Prospecting | 10% | Initial qualification |
| Needs Analysis | 20% | Discovering the problem |
| Proposal | 50% | Quote submitted |
| Negotiation | 75% | Contract terms being discussed |
| Closed Won | 100% | Deal signed |
| Closed Lost | 0% | Deal did not close |

---

## 9.9 Products, Price Books & Opportunity Line Items

### Products (`Product2`)

A **Product** is an item or service your company sells.

### Price Books (`Pricebook2`)

A **Price Book** is a catalog that associates Products with specific prices.

| Price Book Type | Description |
|----------------|-------------|
| **Standard Price Book** | The default, master price list — every product must have an entry here first |
| **Custom Price Book** | A specialized list (e.g., Enterprise pricing, Partner pricing, Regional pricing) |

### Opportunity Line Items

**Opportunity Products** (called Line Items) are the specific products added to an Opportunity from a Price Book.

### Real-World Example

> **Company:** "CloudSoft" sells three products:
>
> | Product | Standard Price | Enterprise Price Book | Partner Price Book |
> |---------|---------------|----------------------|-------------------|
> | Basic License | $100/mo | $80/mo | $70/mo |
> | Advanced License | $200/mo | $160/mo | $140/mo |
> | Enterprise Suite | $500/mo | $400/mo | $350/mo |
>
> **Deal:** Acme Corp (Enterprise customer) wants 50 Advanced Licenses for 12 months.
> 1. Rep opens the Opportunity
> 2. Selects **Enterprise Price Book**
> 3. Adds Product: "Advanced License" — Quantity: 50, Unit Price: $160
> 4. Salesforce calculates Total Price = $160 × 50 × 12 = **$96,000**
> 5. This rolls up to the Opportunity's Amount field automatically

---

## 9.10 Campaigns & Campaign Members

### Campaigns

A **Campaign** tracks a marketing initiative and its effectiveness.

| Field | Example |
|-------|---------|
| Campaign Name | Q2 Email Nurture Series |
| Type | Email, Event, Webinar, Social, Direct Mail |
| Status | Planned, Active, Completed, Aborted |
| Start/End Date | May 1 – June 30, 2026 |
| Expected Revenue | $500,000 |
| Actual Revenue | Populated from Closed Won Opportunities |
| Budgeted Cost | $15,000 |
| Actual Cost | $12,400 |

### Campaign Members

A **Campaign Member** links a Lead or Contact to a Campaign and tracks their response.

| Status | Meaning |
|--------|---------|
| Sent | Email sent, no response yet |
| Opened | Email was opened |
| Clicked | Clicked a link in the email |
| Responded | Replied or filled out a form |
| Converted | Became a Lead or opportunity |

### Campaign Influence

**Campaign Influence** attributes revenue from Closed Won Opportunities back to the campaigns that touched the deal.

### Real-World Example

> **Scenario:** "CloudSoft's" Q2 webinar had 200 attendees. Over the next 3 months:
> - 40 attendees became Leads (Responded)
> - 15 Leads converted to Opportunities
> - 8 Opportunities closed as Won, total $480,000
>
> **Campaign Influence report shows:** The Q2 Webinar influenced $480,000 in revenue. Marketing can now justify the $20,000 webinar cost.

---

## 9.11 Duplicate Management

*(Covered in depth in Section 08 — Data Management)*

For Sales Cloud specifically:

- **Accounts:** Salesforce checks for duplicate company names or website domains
- **Contacts:** Checks for matching email or name + company combinations
- **Leads:** Checks email, name + company

Duplicate rules on Leads are especially important to prevent the same trade show contact from being entered 5 times by 5 different reps.

---

## 9.12 Multi-Currency

Enable **Multi-Currency** when your org does business in more than one currency.

**Path:** Setup → Company Profile → Currencies

| Feature | Description |
|---------|-------------|
| Corporate Currency | Your org's base currency — cannot be changed after setting |
| Active Currencies | Additional currencies reps can use |
| Exchange Rates | Set manually or automatically updated |
| Dated Exchange Rates | Historical rates for accurate period-based reporting |

### Real-World Example

> **Company:** "GlobalSales Inc." — HQ in US, deals in USD, GBP, EUR, JPY.
>
> - Corporate currency: USD
> - UK rep creates an Opportunity in GBP (£150,000)
> - Salesforce converts to USD at the current rate ($190,000) for pipeline reports
> - If the deal closes in 3 months at a different rate, **Dated Exchange Rates** ensure the original conversion rate at deal creation is used for historical accuracy

> ⚠️ **Multi-Currency once enabled CANNOT be disabled.** Always enable in a Sandbox first and test all currency-related formulas, roll-up fields, and reports.

---

## Summary

| Object | Purpose |
|--------|---------|
| Lead | Unqualified prospect — keeps pipeline clean |
| Account | Company you do business with |
| Contact | Person at an Account |
| Opportunity | Potential deal — tracks from prospect to close |
| Product | Item/service you sell |
| Price Book | Catalog of products + prices |
| Opportunity Line Item | Products added to a specific deal |
| Campaign | Marketing initiative |
| Campaign Member | Lead or Contact linked to a Campaign |

---

## 🔗 Trailhead Resources

- [Accounts & Contacts in Lightning Experience](https://trailhead.salesforce.com/content/learn/modules/accounts_contacts_lightning_experience)
- [Leads & Opportunities in Lightning Experience](https://trailhead.salesforce.com/content/learn/modules/leads_opportunities_lightning_experience)
- [Lead Record: Step-by-Step](https://trailhead.salesforce.com/content/learn/modules/lead-record-step-by-step)
- [Leads List View: Step-by-Step](https://trailhead.salesforce.com/content/learn/modules/leads-list-view-step-by-step)

---

*Previous: [08 · Data Management](./08_Data_Management.md)*
*Next: [10 · Service Cloud & Knowledge →](./10_Service_Cloud_and_Knowledge.md)*
