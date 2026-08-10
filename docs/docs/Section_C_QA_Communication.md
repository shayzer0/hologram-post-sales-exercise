# Section C: Go-Live QA, Customer Communication, & Process Scaling

## 1. Contract Ambiguities & QA Checklist

Before flipping the billing engine live, these under-specified contract terms need to be flagged to Deal Desk and the Account Executive (AE) rather than silently assumed:

### QA Item 1: Mid-Month Test Mode Activation & Prorating

- **Contract reference:** Test Mode terms state a SIM transitions to ACTIVE upon consuming 100 KB of data, sending 10 outbound SMS messages, or reaching 90 days.
- **Ambiguity:** The agreement doesn't specify whether a SIM converting mid-month incurs the full $2.43 MRC for that billing cycle, or a prorated amount based on remaining active days.
- **Billing impact:** Charging full MRC on late-month conversions could trigger invoice disputes; prorating without authorization risks underbilling.

### QA Item 2: Un-migrated G1 SIM Spend vs. Minimum Spend Commitment

- **Contract reference:** Hologram will attempt to migrate G1 SIMs to the G3 profile at no cost. Any SIMs that can't be migrated stay on standard plans and rates.
- **Ambiguity:** The contract states "Actual Spend" includes invoiced charges for rate plans, add-ons, and support packages on the specified Org ID (90342), but doesn't confirm whether spend from SIMs stuck on standard plans counts toward the monthly minimum spend commitment ($600/month in Month 8).
- **Billing impact:** If standard-plan spend is excluded, the customer could be hit with minimum spend penalty charges despite total account spend exceeding the threshold.

### QA Item 3: System Setup Lag vs. Effective Start Date

- **Contract reference:** The order form start date is August 1, 2026, but the contract notes it can take up to 4 weeks for new plans to appear or update in the customer's dashboard.
- **Ambiguity:** It's unclear how usage and active SIMs during this 4-week setup window are rated and reconciled against the Month 1 minimum spend floor ($3.00/month).
- **Billing impact:** Delaying billing setup could cause back-billing confusion on Month 2 invoices.

---

## 2. Communication Draft (Internal Escalation to Deal Desk & AE)

**To:** Deal Desk (dealdesk@hologram.io), Sales Account Executive
**From:** GTM Engineering
**Subject:** QA Flag – Halcyon Motors (Org ID: 90342) – Test Mode Billing Prorating Rule

Hi Team,

While configuring the automated billing parameters for Halcyon Motors (Order Form #00002891) ahead of go-live, I flagged a billing edge case around Test Mode conversions that needs alignment.

**Issue:**
The order form defines clear triggers for moving a SIM from Test Mode to ACTIVE (100 KB data, 10 outbound SMS, or 90 days), but doesn't state whether mid-month conversions incur a full $2.43 MRC or a prorated charge for the remaining days of the month.

**Operational risk:**
If a large batch of fleet SIMs converts on day 28 of a month, billing the full $2.43 MRC without prorating may cause customer disputes. Applying prorating logic without explicit contract authorization, on the other hand, could cause discrepancies during financial audits.

**Proposed action:**
Unless Deal Desk instructs otherwise, we'll configure the default rating rule to bill the full $2.43 MRC upon activation for the current billing cycle. Please confirm if an addendum or specific prorating exception should be applied in Salesforce before August 1, 2026.

Thanks,
GTM Engineering

---

## 3. Process Scaling: Automation & Elimination

If this contract-to-billing configuration process were replicated for every new customer onboarding at Hologram, here's how the workflow could be optimized:

> **Note:** As flagged in Section A (System Architecture Mapping), the PandaDoc-to-Salesforce webhook connection is not confirmed to be live today. The recommendation below assumes it still needs to be built, and depends on the answer to the "Contract sync" open question raised there.

### First step to automate: PandaDoc-to-Salesforce intake pipeline

- **Automation:** Build a webhook pipeline connecting PandaDoc signature events directly to Salesforce. On document execution, custom fields on the Salesforce Contract object (per-SIM MRC, overage rates, minimum spend tier arrays) populate automatically via parsed metadata tokens.
- **Value:** Eliminates human translation errors and manual data entry between deal sign-off and billing configuration.

### Step to eliminate: manual spreadsheet billing verification

- **Elimination:** Remove human spreadsheet reviews for standard order forms.
- **Replacement:** Deploy automated schema validation checks within Salesforce. Standard deals pass directly into the billing engine via API; only deals with custom clauses or flagged ambiguities trigger a manual GTM Engineering review.
