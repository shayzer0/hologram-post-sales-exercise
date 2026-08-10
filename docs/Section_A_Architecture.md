# Section A: System Architecture Mapping & Data Flow

## 1. System of Record & Architecture Overview

For billing to be accurate and deterministic, contract parameters need to flow from execution to invoicing without manual re-entry or guesswork. Note: the PandaDoc-to-Salesforce link described below is the target architecture — the actual sync status should be confirmed (see Section 4).

```mermaid
flowchart LR
    A[PandaDoc<br>Contract Source] -- Webhook (planned) --> B[Salesforce CRM<br>System of Record]
    B -- API --> C[Billing Engine & Finance ERP]
```

- **Contract Execution (PandaDoc):** The agreement is signed in PandaDoc. Once both parties sign, contract metadata is intended to flow to Salesforce via webhook — whether this integration is currently live or contracts are entered manually needs confirming.
- **System of Record (Salesforce CRM):** Salesforce is the single source of truth for contract terms. Master terms live on custom fields under the `Contract` object, with the tiered spend commitment in a related child object (`Minimum_Spend_Schedule__c`). Hardware purchases live separately under `OpportunityLineItem`.
- **Downstream Billing:** Billing systems (Stripe Billing, Zuora, or an internal engine) query Salesforce via API or webhook when a deal closes, to set up monthly rating logic.

## 2. Field Mapping & Schema Alignment

How PandaDoc contract tokens map into Salesforce fields (once synced):

| Source (PandaDoc / Contract) | Salesforce Target Object & Field | Purpose |
| :--- | :--- | :--- |
| Org ID (`90342`) | `Account.Hologram_Org_ID__c` | Links cellular usage logs to the parent account |
| Start Date (`2026-08-01`) | `Contract.StartDate` | Baseline for term calculations and billing cycles |
| Initial Term (`36`) | `Contract.ContractTerm` | Length of the primary commitment, in months |
| Auto-Renewal (`True`) | `Contract.AutoRenewal__c` | Triggers a 12-month extension workflow 30 days before expiration |
| MRC Rate (`2.43`) | `Contract.Per_SIM_MRC_Rate__c` | Base monthly recurring charge per active SIM |
| Included Data (`10`) | `Contract.Included_Data_MB_Per_SIM__c` | Multiplier for the shared data pool (`Active SIMs × 10 MB`) |
| Overage Rate (`0.75`) | `Contract.Data_Overage_Rate_Per_MB__c` | Rate applied to MBs consumed past the pool |
| Outbound SMS (`0.57`) | `Contract.Outbound_SMS_Rate__c` | Rate per outbound text message |
| Hardware SKU Rate (`4.50`) | `OpportunityLineItem.UnitPrice` | One-time SIM hardware fee (SKU: `G3-F`) |
| Minimum Spend Schedule | `MinSpendSchedule__c` (child object) | Tiered floor amounts (`$3`, `$300`, `$600`, `$1,200`, `$2,400`) by month range |

## 3. Protecting Downstream Reporting & Invoice Accuracy

Three boundaries need to be enforced to avoid invoice errors and revenue leakage:

1. **Line-item isolation (qualifying vs. non-qualifying spend).** Hardware purchases ($4.50/SIM) live on `OpportunityLineItem`, kept separate from the `Contract` object where usage fees live.

   The contract explicitly excludes hardware purchases from qualifying minimum spend. If hardware charges synced into qualifying-spend fields, a large upfront SIM order would artificially satisfy the monthly minimum — underbilling the customer for actual usage.

2. **Minimum spend floor enforcement.** Tiered minimums (e.g. $600/month in Month 8) live as a structured array on `MinSpendSchedule__c`. Finance systems should evaluate `MAX(Qualifying Invoiced Spend, Monthly Minimum Floor)` programmatically, not via manual adjustment.

3. **Deterministic state triggers for test mode.** Conversion triggers (100 KB, 10 SMS, or 90 days) are stored as explicit numeric values so telemetry pipelines can flip a SIM from `TEST_MODE` to `ACTIVE` automatically.

## 4. Open Questions for Stakeholders

Before final integration, worth confirming:

- **Contract sync:** Is PandaDoc currently integrated with Salesforce (native connector or middleware), or is contract data entered into Salesforce manually today?
- **Billing engine:** Beyond PandaDoc and Salesforce, what actually does final rating and invoice generation — Stripe Billing, Zuora, or an internal system?
- **Telemetry pipeline:** How do usage records get into Salesforce/billing — real-time webhooks, or another method?
- **ERP sync:** Does PandaDoc/Salesforce push contract data straight to an ERP (e.g. NetSuite) for revenue recognition, or does middleware sit in between?
