# 05 · Customization & UI

> **Module:** Customization | **Difficulty:** 🟢 Beginner | **Est. Time:** 3 hrs

---

## 5.1 Classic vs Lightning Experience

Salesforce has two UI frameworks. Lightning is the current standard.

| Feature | Salesforce Classic | Lightning Experience |
|---------|-------------------|---------------------|
| UI Generation | 2000–2015 | 2015–present |
| Design | Tabbed, older look | Modern card-based UI |
| Mobile Ready | Limited | Fully responsive |
| Lightning App Builder | ❌ No | ✅ Yes |
| Dynamic Forms | ❌ No | ✅ Yes |
| Activity Timeline | ❌ No | ✅ Yes |
| Einstein AI features | ❌ No | ✅ Yes |
| Future development | ❌ No new features | ✅ All new features |

### Real-World Example

> **Company:** "OldBank" migrated to Salesforce in 2010 on Classic. In 2024, they migrated to Lightning Experience. Their users initially resisted but quickly loved:
> - The visual Kanban pipeline view for Opportunities
> - Seeing the Activity Timeline on every Account without clicking to another page
> - The Lightning App Builder — admins can now redesign record pages without code

> ⚠️ Salesforce has announced that **Classic is in maintenance mode** — no new features are being added. All new Salesforce features are Lightning-only. Migrate now if you haven't.

---

## 5.2 Apps in Salesforce

An **App** in Salesforce is a set of **tabs, logo, and branding** grouped together for a specific purpose. Apps are what users see in the **App Launcher** (the grid icon top-left).

### Types of Apps

| Type | Description |
|------|-------------|
| **Standard App** | Pre-built by Salesforce (Sales, Service, Marketing) |
| **Custom App (Lightning)** | Admin-built app for a specific business function |
| **Connected App** | Integration with external systems via OAuth |
| **AppExchange App** | Third-party apps installed from Salesforce's marketplace |

### Creating a Custom App

**Path:** Setup → App Manager → New Lightning App

### Real-World Example

> **Company:** "ConstructCo" — a construction company that uses Salesforce for:
> - Sales (standard Sales Cloud)
> - Project management (custom objects)
> - Safety inspections (custom objects)
>
> They create three apps:
> 1. **Sales App** — Accounts, Contacts, Opportunities, Leads
> 2. **Projects App** — Projects, Milestones, Resources, Timesheets
> 3. **Safety App** — Inspection Checklists, Incidents, Equipment Certifications
>
> A sales rep sees only the Sales App. A project manager sees only the Projects App. Each person's workspace is focused and uncluttered.

---

## 5.3 Lightning App Builder

The **Lightning App Builder** is a drag-and-drop tool to build and customize three types of pages — with no code required.

| Page Type | What it customizes | Assigned via |
|-----------|-------------------|-------------|
| **Home Page** | The default landing page when users log in | App + Profile |
| **Record Page** | What a user sees when opening a specific record | App + Profile + Record Type |
| **App Page** | A standalone tab within a Lightning app | App |

### What you can add to a page

- **Standard components** — Related Lists, Activity Timeline, Chatter Feed, Highlights Panel
- **Custom Lightning components** (built by developers)
- **AppExchange components** — pre-built widgets from the marketplace
- **Report Charts** — embed a single report chart directly on a record page

### Real-World Example

> **Scenario:** The Sales team at "TechBridge" wants the Opportunity record page redesigned:
>
> - **Left column (70%):** Fields Panel, Key Metrics (custom component), Activity Timeline
> - **Right column (30%):** Chatter, Related Contacts, Related Files
> - **Bottom:** Related Opportunities (for sub-deals), Quote History
>
> The admin opens Lightning App Builder → selects the Opportunity record page → drags components into the layout → saves → activates for the "Sales" app + "Sales Rep" profile.
>
> Sales Managers get a different page with a Revenue Chart component at the top.

---

## 5.4 Dynamic Forms

**Dynamic Forms** allow individual fields (not just sections) to be shown or hidden based on conditions — directly in Lightning App Builder, without page layouts.

### Real-World Example

> **Scenario:** On an Opportunity record, the `Competitor_Name__c` field should only appear when `Has_Competition__c = True`.
>
> **Without Dynamic Forms:** You'd need a separate page layout for competitive vs non-competitive deals.
>
> **With Dynamic Forms:** Add a visibility rule on `Competitor_Name__c`: "Show when `Has_Competition__c = True`". The field simply appears or disappears based on the checkbox — on the same page layout.

---

## 5.5 List Views

A **List View** is a filtered, sortable table of records for a specific object. It is what users see when they click an object tab (e.g., clicking "Accounts" shows the Accounts list view).

### Default List Views

| List View | Shows |
|-----------|-------|
| Recently Viewed | Last 50 records you visited |
| My [Object] | Records you own |
| All [Object] | All records visible to you |

### Creating a Custom List View

**Path:** Object Tab → List View Controls → New

### Real-World Example

> **Sales Manager's custom list views:**
>
> 1. **"Hot Deals This Quarter"** — Filter: Stage = Proposal OR Negotiation, Close Date = This Quarter, Amount > $50,000. Sort: Probability descending.
>
> 2. **"Stale Opportunities"** — Filter: Last Activity Date < 30 days ago, Stage ≠ Closed Won/Lost. Sort: Last Activity Date ascending (oldest first).
>
> 3. **"Team Pipeline"** — Filter: Owner Role = Sales Rep (all subordinate roles). No date filter. Gives a full team view.

### List View Actions

From a List View, users can:
- **Inline edit** multiple records at once (click a field, edit, save without opening the record)
- **Switch to Kanban view** — drag cards between stage columns
- **Mass actions** — send list emails, change owner, update a field across multiple records

---

## 5.6 Compact Layouts

A **Compact Layout** defines which fields appear in the **Highlights Panel** at the top of a record and in **mobile cards**.

**Path:** Setup → Object Manager → [Object] → Compact Layouts

### Real-World Example

> **Object:** Opportunity
>
> **Default compact layout** shows: Opportunity Name, Account Name, Close Date
>
> **Custom compact layout for Sales team:** Opportunity Name, Account Name, Amount, Stage, Probability, Owner
>
> The Highlights Panel at the top of every Opportunity now shows these 6 fields at a glance — no scrolling needed to find key info.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Lightning vs Classic | Lightning is the present and future — Classic is deprecated |
| App | A grouped set of tabs for a specific business function |
| Lightning App Builder | Drag-and-drop page builder — Home, Record, App pages |
| Dynamic Forms | Show/hide individual fields based on conditions |
| List View | Filtered table of records — customizable per user or shared |
| Compact Layout | Fields shown in the Highlights Panel and mobile cards |

---

## 🔗 Trailhead Resources

- [Lightning Experience Customization](https://trailhead.salesforce.com/content/learn/modules/lex_customization)
- [Lightning App Builder](https://trailhead.salesforce.com/content/learn/modules/lightning_app_builder)
- [List View: Quick Check](https://trailhead.salesforce.com/content/learn/modules/list-view-quick-check)
- [Contacts List View: Step-by-Step](https://trailhead.salesforce.com/content/learn/modules/contacts-list-view-step-by-step)
- [Accounts List View: Step-by-Step](https://trailhead.salesforce.com/content/learn/modules/accounts-list-view-step-by-step)

---

*Previous: [04 · Security & Access Control](./04_Security_and_Access_Control.md)*
*Next: [06 · Business Logic & Automation →](./06_Business_Logic_and_Automation.md)*
