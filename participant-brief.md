# Capstone Case Study — Re-Imagining the Deal Approval Process at Nexlara Technologies

> **Format:** Full-day design sprint + prototype build · Teams of 3–5
> **Goal:** Diagnose the broken deal-approval machine, identify where the most value is trapped, and demonstrate a working reimagination of the highest-value slice.

---

## 1. Executive Summary

Nexlara Technologies AG ("Nexlara") is a €620M ARR enterprise software company with an 18% growth target for FY2025. The company sells a cloud-first product suite — HCM, ERP, and Analytics — to mid-market and enterprise customers across DACH, North America, Benelux, the Nordics, and ANZ.

Nexlara's commercial finance and sales leadership are aligned on one uncomfortable fact: **the company's deal approval process is actively costing it revenue.** Deals take too long to approve. Nearly half of all quotes are rejected on the first submission. Sellers spend more time navigating the approval machinery than closing customers. And the data trail needed to understand *why* a deal was approved or rejected lives scattered across a quoting tool, email threads, spreadsheets, and the institutional memory of a small group of specialists.

Leadership has commissioned this initiative under the name **"Re-Imagine Deal Approval."** You and your team have been engaged as innovation partners. Your mandate is not to tune the existing process — it is to **reimagine it from first principles**, prototype the most valuable slice on real-shaped synthetic data, and present a credible business case for the investment.

You are not expected to solve everything. You are expected to analyse the current state, make defensible choices about where to focus, and show — concretely — how your future-state design unlocks measurable value.

---

## 2. About Nexlara Technologies

| | |
|---|---|
| Headquarters | Frankfurt, Germany |
| Annual Recurring Revenue (FY2024) | ~€620M |
| Growth target (FY2025) | +18% YoY total ARR |
| Employee base | ~8,400 globally |
| Key geographies | DACH (largest), North America, Benelux, Nordics, ANZ |
| Products | Nexlara Suite: Cloud HCM · Cloud ERP · Cloud Analytics |
| Customer segments | Corporate (SME–mid-market) and Enterprise (large accounts) |
| Deal motions | Net New · Upsell · Cross-sell · Renewal · Restructure |
| Quoting system | **Microsoft Dynamics 365 (D365) Sales + D365 Configure-Price-Quote (CPQ)** — the integrated quoting and opportunity management platform |
| CRM | **Microsoft Dynamics 365 (D365) CRM** — system of record for opportunities, accounts, customer history, and forecasting; tightly integrated with D365 CPQ |
| Supporting tools | Pricing App (Stock-Keeping Unit lookup) · Deal Modelling Tool (DMT; offline phasing simulation) · AskQ2C (internal policy Question-and-Answer chatbot) · ServiceNow (IT support tickets and error escalation) |
| Approval governance | Delegation of Authority (DoA) framework with multiple approver tiers |

Nexlara's commercial teams operate a Quote-to-Cash process that spans opportunity creation, quote configuration, deal approval, contract generation, and provisioning. The focus of this case is the **Quote Configuration through Deal Approval** segment — the part that most consistently delays revenue recognition and frustrates sellers.

---

## 3. The Commercial Process Today

### 3.1 Who is involved

| Role | What they do |
|---|---|
| **Account Executive (AE)** | Owns the customer relationship; creates and submits quotes; ultimately accountable for closing the deal. |
| **Field Business Support (FBS)** | Commercial specialist who assists AEs on complex deals — pricing calculations, T&C guidance, submission preparation. Not always involved; threshold-based engagement. |
| **Contract Renewal Executive (CRE)** | Dedicated renewal specialist. Manages active renewal negotiation, contract extension, and prevention of churn. |
| **First Approver** | First tier of the approval chain; reviews all risks in the quote, checks commercial completeness. One approver per market unit. |
| **Super Approver** | Steps in for specific scenarios to reduce the chain length; pre-authorised for certain deal types. |
| **Market Unit (MU) Chief Financial Officer (CFO)** | Approves deals with low contract gross margin, non-standard billing terms, or deals requiring senior authority. |
| **Board** | Triggered for the highest-value or most complex approvals; adds significant latency. |
| **GCF Manager (Global Commercial Finance)** | Orchestrates the commercial finance function for a market unit; manages the Commercial Business Partner (CBP) team; escalation owner. |

### 3.2 How a net-new deal flows today

1. **Opportunity creation.** The Account Executive (AE) identifies a prospect, pre-qualifies the opportunity, and creates an entry in D365 Customer Relationship Management (CRM). Often done manually after early conversations.

2. **Pricing indication.** The Account Executive (AE) builds an initial, non-binding pricing indication in a custom Excel template, referencing the Pricing App for current Stock-Keeping Unit (SKU) rates. This Excel-based offer is sent to the customer — typically within 24 hours of qualification. D365 CPQ is not yet involved at this stage.

3. **Quote creation.** AE opens D365 CPQ and creates a quote attached to the D365 CRM opportunity. The quote header requires a set of fields (contract dates, close date, currency, distribution channel) that may not all be available yet.

4. **Bill of Materials (BOM) configuration.** AE manually transcribes SKUs from the Excel offer into D365 CPQ, one line at a time. Pricing is recalculated against the Lower Eligibility Price (LEP) — the floor below which a deal cannot be priced without escalation. Auto-attached units (typically AI capacity SKUs) appear automatically and must be explicitly justified or removed via a separate ServiceNow ticket process.

5. **Phasing.** If the contract is multi-year with a ramp structure, the AE configures phasing — specifying how Annual Contract Value (ACV) is distributed across years. A key rule: Year-1 ACV must be at least 50% of total contract ACV. Phasing is done manually and is a common source of errors.

6. **Terms and Conditions (T&Cs) and Master Cloud and Commerce Agreement (MCCA).** For corporate-segment customers, standard MCCA terms apply and are largely automated. For enterprise customers, T&Cs are negotiated field-by-field and can trigger dozens of additional approvers.

7. **Error handling.** D365 CPQ raises errors and Deal Health Index (DHI) flags throughout configuration. Error messages are verbose and rarely actionable. AEs must diagnose errors manually, consult colleagues, or raise ServiceNow tickets to the support team. Every rejection resets all downstream checks.

8. **Approval submission.** AE (sometimes with Field Business Support (FBS)) fills in a submission narrative for approvers, adds comments to flagged items, and submits. The approver receives an email notification.

9. **Approval — the black box.** The approval process is perceived by sellers as opaque. Approvers may leave comments, ask for changes, or reject without explanation. If an approver doesn't know the AE personally, they are less likely to reach out before rejecting. There is no in-system communication channel — approvers and AEs communicate by replying to notification emails or over Teams.

10. **Rejection and re-submission.** If rejected, the AE reads the approver's comments, adjusts the quote, and resubmits. All checks — including Global Trade Services (GTS), revenue recognition, and compliance — restart from zero. The median deal goes through **1.8 rejection-resubmission cycles** before approval.

11. **Contract and close.** Once approved, the AE generates the contract from D365 CPQ, sends it to the customer (via email or DocuSign), negotiates any remaining points, and books the deal. Any change to the signed contract triggers full re-approval.

### 3.3 Renewals

Active renewals follow a similar but distinct path:

- A renewal opportunity is auto-generated in D365 CRM at the time of the original booking, sometimes years in advance.
- A Contract Renewal Executive (CRE) is assigned by a Business Execution Partner (BEP) based on deal size and risk.
- The CRE proactively engages the customer ahead of expiry, negotiates terms, and creates a renewal quote in D365 CPQ. The existing BOM is pre-loaded as a starting point — but must be reviewed for deprecated SKUs, pricing changes, and new product generations.
- The renewal quote goes through the same approval chain as net-new deals.

---

## 4. What Is Broken — The Problem Landscape

Leadership has shared the following headline metrics from internal reporting. **Before accepting these targets at face value, your team should interrogate them.** Are they ambitious enough? Are some targets inconsistent with each other? Are there metrics that leadership has not yet defined targets for? Part of your diagnosis is to question whether the goals themselves are correctly framed.

| Metric | Current State | Leadership Target |
|---|---|---|
| Quote-to-approval cycle time | **9.2 days** | 4.0 days |
| First-pass approval rate | **54%** | 80% |
| Median re-submissions before approval | **1.8** | 0.5 |
| Auto-approval rate (no human review needed) | **12%** | 35% |
| Account Executive (AE) time spent on quote admin per deal (hours) | **6.4 hrs** | 2.5 hrs |
| Field Business Support (FBS) cost per quote | **€380** | €150 |
| Approval bypass rate | **3.4%** | <1% |
| Annual Recurring Revenue (ARR) lost to approval-related deal delays (estimated) | **~€25M annually** | <€5M |
| Number of approvals per quote revision | *Deduce from data* | *Define your own target* |

> **Note on the last metric.** Nexlara has not yet defined a target for the number of approvals required per quote revision. The current state must be deduced from the approval events and quote header datasets. Consider: is one approval per revision the right model? What would zero human approvals on certain revision types look like, and what would it require?

The problems are not random. Analysis points to several recurring root causes:

**Pricing and LEP errors.** AEs configure pricing in a custom Excel file and then manually re-enter it into D365 CPQ. Transcription errors are common. The system does not surface Lower Eligibility Price (LEP) compliance status proactively — it raises an error only when the AE tries to submit.

**Auto-attached AI units.** D365 CPQ automatically attaches AI capacity Stock-Keeping Units (SKUs) to qualifying deals. These appear on the customer-facing order form at prices that differ from the initial non-binding offer, creating awkward conversations and sometimes causing customers to question the deal. Removing them requires a separate ServiceNow ticket process outside D365 CPQ.

**DHI flags without guidance.** The Deal Health Index (DHI) flags deals that deviate from policy — but the flag descriptions are technical, jargon-heavy, and provide no guidance on resolution. AEs and FBS spend significant time decoding flags before they can act.

**The approval black box.** Sellers describe the approval process as a "black box." There is no visibility into where a quote sits in the approval chain, how long it has been waiting, or what is likely to happen. Approvers' decisions are influenced by personal relationships and familiarity with the submitting AE or FBS — a structural fairness and process risk.

**Re-approval on any change.** Once a quote has been submitted, any modification — including minor corrections — triggers a full re-run of all compliance checks and restarts the approval clock. This discourages proactive quality improvement.

**Offline approvals and bypasses.** In practice, a subset of deals are approved through offline mechanisms (phone calls, email threads, verbal commitments from Chief Financial Officers) without D365 CPQ recording the decision. This creates audit gaps, concentrates risk in individuals, and makes it impossible to systematically improve approval quality.

**Quarter-end congestion.** Approval volume spikes dramatically in the final three days of each fiscal quarter. Approvers are overwhelmed; turnaround time collapses; and some deals miss quarter-end deadlines — directly impacting revenue recognition for that period.

**License model complexity and ASC 606 compliance.** Nexlara currently derives approximately 80% of its software revenue from perpetual licenses — a model where revenue is recognised largely upfront at contract execution. Leadership has set a strategic target to flip this ratio: reaching 80% ARR from subscription and consumption-based licenses within five years. This transition has significant implications for deal approval. Under ASC 606 (Accounting Standards Codification 606 — the US Generally Accepted Accounting Principles standard governing revenue from contracts with customers), subscription and consumption arrangements require more rigorous stand-alone selling price (SSP) documentation, allocation of Transaction Price across performance obligations, and variable consideration estimation. The current quoting and approval process was designed for perpetual deals and does not enforce or validate ASC 606 requirements at the point of quoting. Any solution must account for the compliance demands of the target license model mix — particularly for Subscription plus Overage and pure Consumption deals, where recognised revenue can differ materially from contracted value.

---

## 5. The Deal Health Index

The Deal Health Index (DHI) is Nexlara's internal policy-compliance flagging system. Every quote in D365 CPQ is continuously evaluated against a set of business rules. Flags are categorised by product type (Cloud, On-Premise, Services) and by severity:

- **Blocker** — the quote cannot be approved until this is resolved or explicitly waived by an authorised party.
- **Warning** — the flag is raised for approver awareness; it does not prevent submission but typically increases scrutiny.
- **Info** — informational only; does not affect approval probability.

Common flag types encountered in the data:

| Flag Code | Description | Frequency |
|---|---|---|
| DHI-C-003 | Auto-attached AI unit present without explicit rationale | Very high |
| DHI-C-002 | Year-1 ACV below 50% of total ACV (phasing rule) | High |
| DHI-C-005 | Pricing below Lower Eligibility Price (LEP) | High |
| DHI-C-006 | Deprecated SKU included | Medium |
| DHI-C-001 | Contract duration outside standard range | Medium |
| DHI-AB-001 | Approval bypass referenced without documentation | Low |
| DHI-SV-001 | Fixed-fee services without signed Statement of Work | Low |

**Important nuance:** DHI flags correlate with rejection, but imperfectly. Some quotes with zero flags are still rejected (for reasons captured only in approver comments). Some quotes with multiple Blocker flags are approved — often through relationships or offline authorisations.

---

## 6. Stakeholders and Their Priorities

| Stakeholder | What they want | What they're worried about |
|---|---|---|
| **AE / CRE** | Faster approvals, less rework, clear guidance on what will and won't pass | Commission delays; losing deals to slow process |
| **FBS** | Smarter tooling; less time on repetitive calculation and error-chasing | Being cut out of the process by automation |
| **First Approver** | Cleaner submissions; less time on obvious errors | Losing control; being blamed for approved-but-problematic deals |
| **GCF Manager** | Visibility across the portfolio; consistent compliance; defensible decisions | Quarter-end fires; audit findings; DHI flags on closed deals |
| **CRO / Revenue Leadership** | ARR growth; shorter cycles; higher win rates | Missing quarterly targets due to process latency |
| **CFO** | Margin protection; compliance; no audit surprises | Approval bypass rates; post-close compliance issues |

---

## 7. Growth Targets and the Strategic Context

Nexlara's FY2025 ARR target is €730M — an 18% increase on FY2024's €620M. Leadership has not prescribed *how* that €110M in incremental ARR should be won. The options are:

- **Net new customer acquisition** — currently 22% of deal volume, 35% of ACV. CAC is high; sales cycles are longest.
- **Upsell and cross-sell to existing customers** — 38% of volume, 31% of ACV. Faster cycles, lower CAC, but require AE attention and often trigger re-approval of existing contracts.
- **Churn reduction on renewals** — 40% of volume, 34% of ACV. Analysis suggests Nexlara loses approximately 4.1% of its renewal ACV annually to process-related friction (delays, quote errors, poor CRE engagement). Eliminating this alone would recover ~€25M.

**No single path is mandated.** Part of your innovation challenge is to identify which growth lever your proposed solution most directly enables, and build your business case around that.

---

## 8. The ML-Powered Deal Approval Score

Nexlara's data science team has built a Machine Learning (ML) model that predicts, at the point of submission, the probability that a given quote will be approved on the first attempt. The model is trained on historical D365 CPQ data. Current characteristics:

- **Accuracy:** ~80% (as of Q1 2025)
- **Training features:** DHI flag count and types, product mix, pricing relative to LEP, contract duration, AE tenure, geography, deal motion, Terms and Conditions (T&C) type, quarter timing
- **Known weaknesses:** Does not incorporate deal-specific approver context; does not account for approver Out-Of-Office (OOO) status; approval probability is not explained (black box to the AE)
- **Current usage:** Displayed on submission screen; rarely acted on by AEs

The model's outputs are included in `quote_header.csv` as `ml_approval_score`. Participants may use, extend, challenge, or replace this model as part of their solution.

---

## 9. Technology Context

Nexlara's current commercial tech stack:

| System | Role |
|---|---|
| **Microsoft Dynamics 365 (D365) Sales** | Customer Relationship Management (CRM) — opportunity and account management, forecasting, customer history |
| **Microsoft Dynamics 365 (D365) CPQ** | Configure-Price-Quote — quote configuration, Bill of Materials (BOM) management, approval workflow, contract generation |
| **ONE360** | Customer 360 view — contract history, entitlement sets, cloud tenants |
| **Pricing App** | Current SKU rates, LEP thresholds, product catalogue lookup |
| **Deal Modelling Tool (DMT)** | Offline phasing simulation; used by FBS for complex deals; no integration with D365 CPQ |
| **ServiceNow** | IT support tickets; auto-attached unit removal requests; error escalation |
| **AskQ2C** | Internal policy Question-and-Answer chatbot; accuracy degrades on edge cases |
| **Microsoft 365** | Email, Teams, OneNote — where much deal context lives informally |
| **DocuSign** | Electronic signature for contracts |

> **A note on supporting tools.** The Pricing App, Deal Modelling Tool, ServiceNow ticketing, and AskQ2C chatbot have each grown up independently to solve a specific pain point. Together they represent significant maintenance overhead, create integration complexity, and fragment the seller experience. Teams should consider whether a reimagined solution requires all of these to exist in their current form.

**For your prototype, assume you are building on top of the existing Microsoft Dynamics 365 (D365) platform.** The D365 suite — including D365 Sales, D365 CPQ, Power Platform (Power Automate, Power Apps, Copilot Studio), and Azure AI services — is Nexlara's strategic platform of record and the assumed foundation for any future solution. You may use any native D365 or Azure AI capabilities, integrate external AI services (Large Language Model (LLM) Application Programming Interfaces (APIs), ML endpoints, document intelligence), or propose extensions. Document your platform choices and rationale.

---

## 10. Key Constraints

Your solution must respect the following:

1. **The approval authority framework (DoA) is non-negotiable for deals above defined thresholds.** You may redesign *how* approvals are presented, supported, and tracked — and you may propose auto-approval for deals that fall below a defined risk and value threshold — but you may not eliminate approval requirements for deals mandated by Nexlara's Delegation of Authority (DoA) policy.

2. **D365 CPQ remains the system of record for quotes.** Your solution writes back to or operates within D365 CPQ — it does not replace it.

3. **Compliance and audit traceability are required.** Every approval decision, bypass, waiver, and DHI flag resolution must be logged with actor, timestamp, and rationale.

4. **ASC 606 compliance is non-negotiable — and the current process does not enforce it.** ASC 606 (Accounting Standards Codification 606) governs how revenue from contracts with customers is recognised. As Nexlara shifts from perpetual to subscription and consumption-based licensing, every quote must correctly classify performance obligations, allocate transaction price using defensible Stand-Alone Selling Prices (SSPs), and handle variable consideration (overage, usage-true-up). The current D365 CPQ configuration was built for perpetual deal structures and does not validate ASC 606 requirements at the quoting stage. Any solution must address this gap — particularly for Subscription + Overage and pure Consumption deals.

5. **License model transition is a strategic constraint, not a preference.** Nexlara currently earns approximately 80% of software revenue from perpetual licenses. The five-year strategic target is 80% from recurring (subscription and consumption) models. Any solution that entrenches perpetual deal workflows, makes subscription quoting harder, or ignores the revenue recognition implications of the new models will not receive Board support.

6. **Seller experience matters.** A solution that requires AEs to do substantially more work will not be adopted, regardless of its technical merits. Design for adoption.

7. **No solution should disadvantage sellers on compensation without an explicit compensation policy change.** If your solution affects commission timing or eligibility, flag this explicitly — do not assume it away.

---

## 11. Your Mission

By end of day, your team will:

1. **Diagnose the current state.** Use the data to identify the highest-impact failure points and quantify their cost in time, revenue, and AE effort. Not all problems are equal — show your prioritisation reasoning.

2. **Reimagine the process.** Design a future-state deal approval process. This is an open brief — you may redesign quoting workflows, approval logic, data capture, ML integration, seller experience, or any combination. You are not limited to incremental improvements. Consider: what would this look like if it were built from scratch today?

3. **Build a demonstrable prototype** of the highest-value slice of your reimagined process on the provided synthetic data. The prototype should be runnable and demonstrable — not a mockup.

4. **Build the business case** — what does this investment cost, what does it return, and when does it pay back?

---

## 12. Candidate Innovation Directions

These are starting points, not a menu. You may pursue any combination, or a direction not listed here. Defend your choice.

### Direction A — Intelligent Quote Preparation
Reduce first-pass rejection rates by building an AI-assisted quoting experience within D365 CPQ that surfaces LEP compliance, DHI flag implications, phasing rules, and approval probability in real time — before submission. The agent guides the AE through configuration, explains each flag in plain language, and suggests specific changes to increase approval likelihood.

### Direction B — Approval Process Reimagination
Redesign the approval workflow itself. Options include: Machine Learning (ML)-powered auto-approval for low-risk quotes, structured approver-AE communication channels within D365, approval probability dashboards for GCF Managers, dynamic approval routing based on deal risk, and capturing offline approvals into the structured system with full audit trails.

### Direction C — Renewal Churn Recovery
Focus on the ~€25M in estimated annual ARR lost to renewal process failures. Design a renewal-specific intelligence layer within D365 that identifies at-risk renewals early, guides Contract Renewal Executives (CREs) on optimal re-pricing and Terms and Conditions (T&C) configuration, and reduces the time from renewal opportunity creation to contracted close.

### Direction D — Deal Health Automation
Build an automated deal health engine that replaces the current static DHI flag system with a dynamic, contextual, and actionable compliance advisor. The system explains *why* a flag was raised, links to the specific Bill of Materials (BOM) line or T&C field responsible, suggests resolution paths, and tracks resolution quality over time.

### Direction E — Growth Strategy Optimisation
Use the pipeline, BOM, and renewal data to identify where Nexlara's €110M incremental ARR target is most efficiently reachable — net new, upsell/cross-sell, or churn reduction — and design a data product or sales intelligence tool that directs AE and CRE effort toward the highest-probability, highest-value opportunities.

### Direction F — Structured Auto-Approval Through Pre-Approved Configuration
Reimagine the deal configuration experience so that a meaningful portion of deals — particularly in the corporate segment — never require human approval at all. The central idea: replace the current open-input quoting model with a **pre-approved configuration catalogue**. Instead of allowing AEs to enter any product, price, discount, or term combination into D365 CPQ, the system offers a curated set of pre-validated deal configurations (product bundles, pricing tiers, standard contract durations, approved discount bands) for each customer segment and deal motion. Any deal built entirely from the pre-approved catalogue is auto-approved by the system without human review — subject to defined ACV (Annual Contract Value) thresholds. Deals that deviate from the catalogue — through custom pricing, non-standard T&Cs, or configurations outside the pre-approved set — escalate to the standard DoA approval chain. This direction requires teams to address: how the catalogue is defined and maintained; how AEs are prevented from bypassing it; how the system handles edge cases; and how ASC 606 compliance is validated at configuration time rather than approval time. Teams should also consider the seller experience implications — this model trades flexibility for speed, and AEs who currently use custom configurations as a negotiation tool will push back.

### Direction G — Supporting Tool Consolidation
The current commercial process depends on a fragmented set of supporting tools: the Pricing App for SKU lookup, the Deal Modelling Tool (DMT) for phasing simulation, ServiceNow for error escalation, and AskQ2C for policy queries. These tools exist outside D365, require context-switching, and produce outputs that must be manually re-entered into D365 CPQ. Reimagine the seller's workflow with these tools either eliminated or fully absorbed into D365 — using D365 Copilot, Power Platform, and Azure AI services to natively surface pricing data, simulate phasing, resolve common errors, and answer policy questions inside the quoting experience. Consider: which tools can be fully retired? Which require a transition period? What is the impact on the teams who currently support and maintain each tool?

---

## 13. KPIs Your Solution Should Address

Select the metrics most relevant to your chosen direction. Not all are required. For the metrics marked *deduce from data*, no current-state figure is provided — your team should derive it from the datasets in Appendix A and document your methodology.

| KPI | Current | Target |
|---|---|---|
| Quote-to-approval cycle time (days) | 9.2 | 4.0 |
| First-pass approval rate (%) | 54% | 80% |
| Median re-submissions per quote | 1.8 | 0.5 |
| Number of approvals per quote revision | *Deduce from data* | *Define your own target* |
| Auto-approval rate (%) | 12% | 35% |
| AE time on quote admin (hrs/deal) | 6.4 | 2.5 |
| FBS cost per quote (€) | €380 | €150 |
| Approval bypass rate (%) | 3.4% | <1% |
| ARR lost to approval delay (€M) | ~€25M | <€5M |
| Renewal churn rate (ACV %) | 4.1% | <1.5% |
| Quarter-end approval backlog (% of deals closing in final 3 days of quarter) | 38% | 20% |
| % of revenue from subscription / consumption licenses | 20% | 80% (5-year target) |

---

## 14. Clarification Sessions and Demo Requirements

### How to get information from Nexlara

You have access to five Nexlara stakeholders who can answer questions, provide context, and react to your thinking:

| Name | Role |
|---|---|
| **Mia Hoffmann** | Global Commercial Finance (GCF) Manager, DACH |
| **Lars Brandt** | Senior Account Executive (AE), Enterprise segment |
| **Priya Nair** | VP Commercial Finance |
| **James Kaur** | Head of Customer Experience (CX) Transformation |
| **Sasha Lindqvist** | Chief Revenue Officer (CRO) |

### The rules

**You have exactly three free clarification sessions** — moments where you can ask any of the five personas questions with no preconditions. Use them when you genuinely need information you cannot extract from the data or the brief. Once all three are used, they are gone.

**Every subsequent clarification costs a demo.** After your three free sessions are exhausted, any further conversation with a persona must be accompanied by a working demonstration of the thing you are asking about. You cannot walk in and ask a question cold — you show them something first, then ask. This simulates the real-world dynamic: stakeholders give more time and better answers when they can see something concrete.

**You must run at least two targeted demos before end of day.** These are not just Q&A sessions with a screen visible — they are structured demonstrations of a running prototype slice, shown to a specific persona because that persona's reaction is meaningful to your design. A demo to Mia about the DHI (Deal Health Index) flag experience is targeted. A demo to Lars about the seller quoting experience is targeted. Showing the same thing to both without differentiation is not. Each targeted demo should be designed to surface a specific reaction, test a specific assumption, or unlock a specific piece of information.

### How to request a session

Raise your hand or approach the facilitation desk. State which persona you want and whether this is a free clarification or a demo session. If it is a demo session, briefly describe what you will show before the persona enters the conversation.

### Tracking

Your team's session count is tracked on the scoreboard. Free sessions remaining and demos completed are visible to judges. A team that reaches end of day without two demos completed will have its final score adjusted.

### Strategic advice

Three free sessions go faster than you expect. Teams that burn them all in the first two hours on questions that the data could have answered are at a disadvantage. Before requesting a clarification, ask yourself: *can we deduce this from the datasets? Can we make a defensible assumption and test it with a demo later?* Reserve free sessions for things where the answer genuinely changes your direction — not things where any reasonable answer leads to the same design.

---

## 15. Deliverables (by end of day)

| Deliverable | Format |
|---|---|
| **Current-state diagnosis** | 1–2 pages or equivalent slide: What is broken, where, and what it costs. Show your prioritisation rationale. Include your derived metrics (quote revisions per opportunity, win rate, average deal value). |
| **Future-state process design** | Swim-lane diagram or written narrative. Current vs. future. What changes, what stays, where humans remain in the loop. Must specify which Delegation of Authority (DoA) approval tiers remain and what triggers auto-approval. |
| **Agentic architecture** | Architecture diagram + short narrative. Specify components, data flows, integrations to D365 Configure-Price-Quote (CPQ) and D365 Customer Relationship Management (CRM), Human-in-the-Loop (HITL) touchpoints, failure modes and how they are handled, and why the design is AI-native rather than AI-assisted. Phase 1 scope must be credibly deliverable in 90 days. |
| **Working prototype** | Runnable code, User Interface (UI), or notebook. Runs on the provided synthetic datasets — not hardcoded placeholder data. Output must be interpretable by a non-technical stakeholder. Demonstrated to at least one persona during the day. |
| **Adoption, change, and workforce transition plan** | Embedded in the pitch or as a separate section. Identify early adopters, change mechanism, Field Business Support (FBS) workforce impact, day-90 ownership model (named artifacts, AI governance model, AI champions), and specific guardrails preventing harmful agent actions. |
| **Business case** | 1–2 pages or slide section. See structure below. |
| **Pitch deck** | 5–7 slides: Problem framing → Agentic architecture → Prototype walkthrough → Value quantification → Adoption and change plan → Growth story → Asks. |

### Business case structure

- **Investment required.** One-off cost: build effort, integration, change management, training. Order-of-magnitude with clear assumptions.
- **Run cost.** Ongoing per-deal or per-month cost at current and scaled volume.
- **Value delivered — displacement and augmentation.** Distinguish activities eliminated (displacement) from better outcomes at the same or lower cost (augmentation). Quantify both.
- **Payback and Return on Investment.** When does cumulative value exceed cumulative investment?
- **Measurement plan.** How will Nexlara know at 30, 60, and 90 days whether the solution is working? Name the leading indicators.
- **Growth enablement.** How does your solution contribute to the €110M ARR growth target — and which growth lever does it unlock?
- **Risk-adjusted view.** What if you capture only 60% of projected value?
- **Phasing.** What is your phase 1 — the smallest thing that proves value before the firm commits to the full investment?

---

## 16. Guiding Questions

Use these to sharpen your thinking. They are not a checklist.

- Are the leadership targets in Section 4 the right ones? Are any inconsistent with each other? Are there targets that are missing entirely?
- Which single failure point, if fixed, releases the most revenue per euro of investment?
- What information is reusable across deals versus what is truly deal-specific? How does that change what an AI agent should remember versus recalculate?
- How would your design prevent the same rejection reason from appearing on the same AE's deals repeatedly?
- Is the approval process broken because of bad data, bad rules, bad tooling, or bad incentives? Does your answer change what you build?
- If you could capture every offline approval conversation into a structured record, what would you do with it?
- How does your design behave at quarter-end — when volume spikes and patience is short?
- What does a seller's day look like in your future state? Is it better? Would they adopt it without being told to?
- How do seller incentives and compensation models interact with quote quality? Is this a risk in your design?
- Under what conditions should a deal be approvable with zero human involvement? What data signals, thresholds, audit requirements, and guardrails would make that safe?
- If an AE can only choose from a pre-approved set of configurations, how do you handle the customer who asks for something that isn't on the list? Does this create a competitive disadvantage?
- The Pricing App, Deal Modelling Tool, ServiceNow integration, and AskQ2C each solve a real problem. What would it take to eliminate each one? Which could be retired immediately? Which need a replacement first?
- Nexlara is moving from 80% perpetual to 80% subscription and consumption revenue. How does that change the quoting and approval process? What breaks under ASC 606 (Accounting Standards Codification 606) that currently works fine for perpetual deals?
- How does variable consideration — usage overages, consumption true-ups — complicate both the quoting process and the approval framework?
- What is the minimum viable first phase that is demonstrably valuable within 90 days of go-live?
- How does your solution scale from DACH alone to all five geographies — and what changes?
- **The Disrupt or Defend question:** Is Nexlara improving a process that belongs in their core business in 2027 — or optimising something that a competitor without this process at all will outcompete them on within 18 months? You will be asked this question directly after the build sprint. Think about your answer before then.

---

## 17. What's in the Data Package (Appendix A)

| File | Contents |
|---|---|
| `opportunity_pipeline.csv` | 120 open opportunities across deal motions, geographies, and segments |
| `quote_header.csv` | 85 quotes with status, approval outcomes, rejection reasons, and ML scores |
| `bom_line_items.csv` | ~640 Bill of Materials (BOM) line items with pricing, LEP compliance, and auto-attach flags |
| `dhi_flags.csv` | ~180 Deal Health Index (DHI) flag instances with codes, severity, status, and comments |
| `approval_events.csv` | ~310 approval event log entries with timestamps and actor roles |
| `renewal_risk_signals.csv` | 48 renewal accounts with adoption, Net Promoter Score (NPS), churn risk, and CRE engagement data |
| `sku_catalogue.csv` | 68 Stock-Keeping Unit (SKU) definitions with pricing, LEP, lifecycle status, and commercialisation rules — includes license model classification (Perpetual / Subscription / Consumption / Subscription+Overage) |
| `kpi_benchmarks.csv` | Internal and industry benchmark data for key commercial metrics |

Two restricted files exist and may be released during the session if the right questions are asked:
- `seller_quota_and_compensation.csv` — AE-level quota attainment, Special Performance Incentive for the Field Force (SPIFF) incentives, and commission structures
- `approval_bypass_log.csv` — Records of deals that bypassed the standard approval workflow

**Metrics your team must deduce from the data — not given directly:**

| Metric to Deduce | Primary Source Files |
|---|---|
| Number of quote revisions per opportunity | `opportunity_pipeline.csv` + `quote_header.csv` |
| Opportunity win rate (by segment, deal motion, geography) | `opportunity_pipeline.csv` + `quote_header.csv` |
| Average deal value (by deal motion and product line) | `opportunity_pipeline.csv` + `bom_line_items.csv` |

Your methodology for deriving these metrics should be documented and presented as part of your current-state diagnosis.

You are free to extend, mock, or generate additional synthetic data as needed. Do not use any real-world data.

---

*Good luck. Defend your choices. Be specific about what you would build — and what you would not.*
