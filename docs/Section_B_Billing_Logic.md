# Section B: Encoded Billing Logic & Worked Month 8 Calculation

## 1. Core Billing Logic Overview

The monthly billing engine computes the customer's total charge from three inputs:

1. Month number (1 through 36+)
2. Active SIM count
3. Total pooled data usage (MB)

### A. Pooled Data Calculation

Each active SIM contributes 10 MB to a shared organizational data pool for the billing cycle:

- Total Pooled Allowance (MB) = Active SIM Count × 10 MB
- Data Overage (MB) = Max(0, Total Pooled Data Usage − Total Pooled Allowance)
- Data Overage Charge ($) = Data Overage (MB) × $0.75/MB

### B. Monthly Recurring Charge (MRC)

- Base MRC ($) = Active SIM Count × $2.43/SIM

### C. Qualifying Spend Rules & Minimum Spend Floor

Under the order form, Qualifying Spend includes only invoiced charges for:

- Rate Plan Monthly Recurring Charges (MRC)
- Data overages
- Outbound SMS ($0.57/SMS)
- Support package fees

SIM hardware purchases ($4.50/SIM), setup fees, and late charges are excluded from qualifying spend.

The customer pays the greater of total Qualifying Spend or the applicable Minimum Spend Floor for that month:

- Final Monthly Charge ($) = Max(Qualifying Spend, Applicable Minimum Spend Floor)

### Tiered Minimum Spend Schedule

- Months 1–3: $3.00/month
- Months 4–6: $300.00/month
- Months 7–12: $600.00/month
- Months 13–24: $1,200.00/month
- Months 25–36+: $2,400.00/month

---

## 2. Worked Example: Month 8 Calculation

### Scenario Inputs

- Month number: 8 (Tier 3 — Months 7–12, minimum spend floor = $600.00)
- Active SIM count: 150
- Total pooled data usage: 2,400 MB
- Outbound SMS sent: 0
- Support tier: Developer Support ($0.00)

### Step-by-Step Line-Item Breakdown

| Line Item | Formula / Input | Calculation | Subtotal |
| :--- | :--- | :--- | :--- |
| Base MRC | 150 SIMs × $2.43/SIM | 150 × $2.43 | $364.50 |
| Shared pooled allowance | 150 SIMs × 10 MB/SIM | 150 × 10 MB | 1,500 MB (included) |
| Pooled data overage volume | 2,400 MB total − 1,500 MB pool | 2,400 − 1,500 | 900 MB overage |
| Data overage fee | 900 MB × $0.75/MB | 900 × $0.75 | $675.00 |
| Outbound SMS fee | 0 SMS × $0.57/SMS | 0 × $0.57 | $0.00 |
| Developer support fee | Tier included | Fixed | $0.00 |
| Total qualifying spend | Base MRC + data overage | $364.50 + $675.00 | **$1,039.50** |
| Applicable minimum spend floor | Month 8 tier (Months 7–12) | Contract threshold | **$600.00** |
| Minimum spend floor adjustment | Max(0, Floor − Actual Spend) | Max(0, $600.00 − $1,039.50) | $0.00 |
| **Final invoice total** | Max(Actual Spend, Floor) | Max($1,039.50, $600.00) | **$1,039.50** |

Since actual qualifying spend ($1,039.50) exceeds the Month 8 minimum ($600.00), Halcyon Motors is billed $1,039.50.

---

## 3. Test Mode → Active Transition Logic

### Conversion Triggers

A SIM stays in Test Mode for up to 90 days and automatically transitions to ACTIVE (becoming billable) as soon as any of the following are met:

1. **Data threshold:** cumulative usage reaches 100 KB
2. **SMS threshold:** outbound SMS count reaches 10
3. **Time threshold:** 90 days in Test Mode

### Transition Billing Impact

- **When billed:** on conversion, the SIM becomes ACTIVE and immediately starts incurring the $2.43 MRC and contributing 10 MB to the data pool.
- **Proration ambiguity:** the contract doesn't specify whether mid-month activations incur the full $2.43 MRC or a prorated amount. Flagging this for alignment with Deal Desk before go-live.
