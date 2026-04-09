# 09 · Sales Cloud

> **Difficulty:** 🟡 Beginner–Intermediate | **Est. Time:** 8 hrs

---

## 9.1 The Sales Lifecycle

```
Lead → Qualify → Convert → Account + Contact + Opportunity
                                          ↓
                             Stages: Prospecting → Proposal → Negotiation → Closed Won
                                          ↓
                             Products + Price Book → Opportunity Line Items
                                          ↓
                             Campaign Influence tracked for marketing attribution
```

---

## 9.2 Leads

Unqualified prospects — kept separate from Accounts/Contacts until verified as real opportunities.

| Field | Purpose |
|-------|---------|
| Status | New, Working, Qualified, Unqualified, Converted |
| Lead Source | Web, Trade Show, Referral, Purchased List |
| Rating | Hot, Warm, Cold |

---

## 9.3 Web-to-Lead

Auto-creates Leads from website form submissions. Setup → Marketing → Web-to-Lead → generates HTML.

### Real-World Example
> CloudSoft embeds a "Request a Demo" form. Visitor submits → Lead created instantly → Flow routes: if Company Size > 500 → Enterprise Queue; else → SMB rep. Response time drops from 24 hrs to 15 min.

---

## 9.4 Lead Conversion

Clicking **Convert** on a Lead creates:
- A new **Account** (or matches existing)
- A new **Contact** (under the Account)
- Optionally, a new **Opportunity**

The Lead is then **locked** (Converted status). Cannot be unconverted.

All Lead activities carry over to the new Contact and Opportunity via the Activity Timeline.

---

## 9.5 Accounts & Contacts

- **Account** — company or individual. Can have a parent Account (hierarchy via self-relationship).
- **Contact** — person at an Account. Can exist without Account (private contact).
- **Contact Roles on Opportunity** — Economic Buyer, Technical Evaluator, Champion, End User.

---

## 9.6 Opportunities

| Field | Purpose |
|-------|---------|
| Stage | Prospecting → Needs Analysis → Proposal → Negotiation → Closed Won |
| Probability | Auto-set per stage; can be manually overridden |
| Amount | Expected deal value |
| Close Date | Projected close date |
| Type | New Business, Renewal, Expansion |

---

## 9.7 Products, Price Books & Opportunity Line Items

- **Product** (`Product2`) — items/services you sell
- **Standard Price Book** — master catalog; every product must have an entry here first
- **Custom Price Books** — Enterprise pricing, Partner pricing, Regional pricing
- **Opportunity Line Items** — specific products added to a deal from a selected Price Book

### Real-World Example
> CloudSoft's Enterprise customer Acme Corp selects Enterprise Price Book → adds 50 Advanced Licenses at ₹160/month → 12 months. Salesforce calculates Total = ₹160 × 50 × 12 = ₹96,000 → rolls up to Opportunity Amount.

---

## 9.8 Campaigns & Campaign Influence

- **Campaign** — a marketing initiative with budgeted cost, expected revenue, actual revenue
- **Campaign Member** — a Lead or Contact linked to a Campaign with a Response Status
- **Campaign Influence** — attributes closed revenue back to campaigns that touched the deal

---

## 9.9 Multi-Currency

- Corporate currency set at org level
- Additional active currencies with exchange rates
- **Dated Exchange Rates** for historical accuracy
- ⚠️ Once enabled, **cannot be disabled**. Always enable in Sandbox first.

---

## ✅ Hands-On Tasks

---

### 🔧 Task 1 — Create a Lead and Convert It

**Objective:** Walk through the complete Lead lifecycle.

**Steps:**
1. App Launcher → **Sales** app → **Leads** tab → **New**
2. Fill in:
   - First Name: `Arjun`
   - Last Name: `Mehta`
   - Company: `TechStart India`
   - Email: `arjun@techstart.in`
   - Phone: `9876543210`
   - Lead Source: `Web`
   - Status: `Working`
   - Rating: `Hot`
3. Save
4. Log an activity: Go to Activity Timeline → **New Task** → "Initial contact call — very interested in demo" → Due Today → Save
5. Now click **Convert** on the Lead
6. On the conversion screen:
   - Account: Create New → `TechStart India`
   - Contact: Create New → `Arjun Mehta`
   - Opportunity: Create New → Name: `TechStart India — Initial Deal`, Amount: ₹200,000, Close Date: 3 months from now
7. Convert
8. **Verify:** Open the new Opportunity → Activity Timeline → the task from the Lead should appear here

---

### 🔧 Task 2 — Set Up Web-to-Lead

**Objective:** Generate and test a Web-to-Lead form.

**Steps:**
1. Setup → Marketing → **Web-to-Lead**
2. Enable Web-to-Lead: ✅
3. Click **Create Web-to-Lead Form**
4. Add fields: First Name, Last Name, Company, Email, Phone
5. Return URL: `https://www.salesforce.com` (or your own page)
6. Generate HTML → Copy the code
7. Create a local HTML file on your computer, paste the form code, open in browser
8. Fill out the form and submit
9. In Salesforce: Leads tab → verify the new Lead appeared
10. Check Lead Source = `Web`

---

### 🔧 Task 3 — Build an Opportunity with Products

**Objective:** Add products to an Opportunity from a Price Book.

**Pre-requisite:** Create at least 2 Products in Salesforce with prices.

**Steps:**
1. Setup → Products (or App Launcher → Products tab) → **New**
   - Product Name: `Basic License`, Product Code: `LIC-001`
   - Save
   - Add Standard Price: ₹5,000/month
2. Create a second product: `Advanced License` → Standard Price ₹10,000/month
3. Open your Arjun Mehta Opportunity from Task 1
4. In the **Products** related list → click **Add Products**
5. Select Price Book: Standard Price Book
6. Select: Advanced License
7. Quantity: 10, Unit Price: ₹10,000, Date: today
8. Save
9. Observe the Opportunity **Amount** field updates automatically to ₹1,00,000

---

### 🔧 Task 4 — Create a Campaign and Track Members

**Objective:** Create a campaign and link leads/contacts to it.

**Steps:**
1. App Launcher → **Campaigns** → **New**
2. Fill in:
   - Campaign Name: `Q2 Webinar — Salesforce Demo`
   - Type: `Webinar`
   - Status: `Active`
   - Start Date: today, End Date: 30 days from now
   - Budgeted Cost: ₹50,000
   - Expected Revenue: ₹5,00,000
3. Save
4. Go to the **Campaign Members** related list → **Add Leads**
5. Add Arjun Mehta (Lead, now converted — use the Contact)
6. Status: `Responded`
7. Go to the Opportunity created from Arjun's Lead
8. Set `Primary Campaign Source` = `Q2 Webinar — Salesforce Demo`
9. When the Opportunity is Closed Won, it contributes revenue back to the Campaign

---

### 🔧 Task 5 — Set Up Account Hierarchy

**Objective:** Model a parent-child company structure.

**Steps:**
1. Create Account: `Tata Group` (Type: Parent Account)
2. Create Account: `Tata Consultancy Services` → Parent Account = `Tata Group`
3. Create Account: `Tata Motors` → Parent Account = `Tata Group`
4. Open `Tata Group` → View the **Account Hierarchy** (button top right)
5. Observe the tree structure showing TCS and Motors as subsidiaries
6. Add a Contact to `Tata Consultancy Services` — note they are linked to that specific subsidiary, not the parent

---

## 📝 Practice Questions

**Q1.** After converting a Lead, can you unconvert it?

**Q2.** A Contact exists with no Account linked. What is this Contact called?

**Q3.** What is the difference between the Standard Price Book and a Custom Price Book?

**Q4.** Can you add products to an Opportunity without selecting a Price Book?

**Q5.** A Lead Source field on a converted Lead — does it transfer to the Opportunity, Contact, or Account?

**Q6.** What is Campaign Influence, and what is the `Primary Campaign Source` field used for?

**Q7.** Multi-Currency is enabled. A UK rep creates an Opportunity in GBP. How does the VP (corporate currency USD) see this in their pipeline report?

**Q8.** What is the purpose of Opportunity Contact Roles?

---

## 🎯 Scenario-Based Questions

---

**Scenario 1 — Lead Management Design**

> "EduTech" — an online education platform — generates 2,000 leads per month from their website. Their sales process:
> 1. Marketing nurtures leads via email for 14 days
> 2. Leads who engage (open emails, attend webinars) are passed to Sales
> 3. Sales qualifies within 48 hours
> 4. Qualified leads are converted to Contacts + Opportunities
> 5. Marketing gets credit for any resulting revenue
>
> **Question:** Design the complete Salesforce setup to support this process, covering: Lead Source values, Lead Status picklist, Conversion criteria, Campaign setup, Web-to-Lead configuration, and Campaign Influence.

---

**Scenario 2 — Price Book Strategy**

> "CloudSoft" sells to three customer segments with different pricing:
> - Standard customers: list price
> - Enterprise customers (>500 employees): 20% discount
> - Channel Partners: 30% discount
>
> They have 3 products: Basic (₹5K), Advanced (₹10K), Enterprise Suite (₹25K).
>
> **Question:**
> 1. How many Price Books are needed and what are they?
> 2. Map out the prices per product per Price Book
> 3. When a rep creates an Opportunity for an Enterprise customer, what Price Book do they select?
> 4. Can a rep manually override the price on an Opportunity Line Item? What controls this?

---

## ✔️ Answer Key

1. **No** — once a Lead is converted, it cannot be unconverted. The converted flag is permanent.
2. A **Private Contact** — a Contact with no Account linked.
3. The **Standard Price Book** is the master catalog; every product must have a standard price before it can be added to any other Price Book. **Custom Price Books** are alternate catalogs with different pricing for specific customer segments.
4. **No** — you must select a Price Book before adding products to an Opportunity.
5. Lead Source is mapped to the **Opportunity** during conversion (it can also be on the Contact, depending on Lead-to-Contact field mapping configured by the admin).
6. **Campaign Influence** attributes revenue from Closed Won Opportunities back to campaigns that touched the deal. **Primary Campaign Source** is the single campaign that gets credit for influencing the Opportunity.
7. Salesforce converts the GBP amount to USD using the active exchange rate and displays the converted value. The VP's pipeline report shows all amounts in USD.
8. Opportunity Contact Roles document **which contacts are involved** in a deal and **what role** each plays (Economic Buyer, Decision Maker, Technical Evaluator, etc.) — helping the rep understand the buying committee.

---

## 🔗 Trailhead Resources

- [Accounts & Contacts in Lightning Experience](https://trailhead.salesforce.com/content/learn/modules/accounts_contacts_lightning_experience)
- [Leads & Opportunities in Lightning Experience](https://trailhead.salesforce.com/content/learn/modules/leads_opportunities_lightning_experience)
- [Lead Record: Step-by-Step](https://trailhead.salesforce.com/content/learn/modules/lead-record-step-by-step)

---

*Previous: [08 · Data Management](./08_Data_Management.md) | Next: [10 · Service Cloud →](./10_Service_Cloud_and_Knowledge.md)*

