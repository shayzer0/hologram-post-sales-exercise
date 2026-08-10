# Section A: Contract Translation & Billing Configuration

| Contract Parameter | Contract Value | Data Type | Salesforce Target Field |
| :--- | :--- | :--- | :--- |
| **Account Name** | Halcyon Motors | String | `Account.Name` |
| **Org ID** | 90342 | Integer | `Account.Hologram_Org_ID__c` |
| **Customer Billing Street** | 1420 Larkin Ave | String | `Account.BillingStreet` |
| **Customer Billing City** | Portland | String | `Account.BillingCity` |
| **Customer Billing State** | OR | String | `Account.BillingState` |
| **Customer Billing Postal Code** | 97204 | String | `Account.BillingPostalCode` |
| **Customer Billing Country** | United States | String | `Account.BillingCountry` |
| **Customer Billing Contact Email** | morgan.reeves@halcyonmotors.com | String (Email) | `Account.Billing_Email__c` |
| **Customer Signer Name** | Jordan Vance | String | `Contract.CustomerSignedName__c` |
| **Customer Signer Title** | CTO | String | `Contract.CustomerSignedTitle` |
| **Customer Signer Email** | jordan.vance@halcyonmotors.com | String (Email) | `Contract.CustomerSignedEmail__c` |
| **Customer Signed Date** | 2026-08-05 | Date | `Contract.CustomerSignedDate` |
| **Hologram Signer Name** | Jamie Sutton | String | `Contract.CompanySignedName__c` |
| **Hologram Signer Title** | CEO | String | `Contract.CompanySignedTitle` |
| **Hologram Signer Email** | jamie.sutton@hologram.io | String (Email) | `Contract.CompanySignedEmail__c` |
| **Hologram Signed Date** | 2026-08-05 | Date | `Contract.CompanySignedDate` |
| **Order Form Number** | 00002891 | String | `Contract.Order_Form_Number__c` |
| **Order Form Start Date** | 2026-08-01 | Date | `Contract.StartDate` |
| **Initial Term Duration Value** | 36 | Integer | `Contract.ContractTerm` |
| **Initial Term Duration Unit** | Months | String | `Contract.ContractTermUnit__c` |
| **Calculated Initial Term End Date** | 2029-07-31 | Date | `Contract.EndDate` |
| **Auto Renewal Enabled** | True | Boolean | `Contract.AutoRenewal__c` |
| **Renewal Term Duration Value** | 12 | Integer | `Contract.Renewal_Term_Months__c` |
| **Renewal Term Duration Unit** | Months | String | `Contract.RenewalTermUnit__c` |
| **Calculated First Renewal Start Date** | 2029-08-01 | Date | `Contract.First_Renewal_Start_Date__c` |
| **Non-Renewal Notice Period Value** | 30 | Integer | `Contract.Non_Renewal_Notice_Days__c` |
| **Non-Renewal Notice Period Unit** | Days prior to auto-renewal | String | `Contract.Non_Renewal_Notice_Unit__c` |
| **Calculated First Non-Renewal Notice Deadline** | 2029-07-02 | Date | `Contract.First_Non_Renewal_Deadline__c` |
| **Billing Method** | Invoicing | String | `Contract.BillingMethod` |
| **Billing Cycle** | Calendar month | String | `Contract.BillingCycle__c` |
| **Payment Terms** | Net 30 | String | `Contract.Payment_Terms__c` |
| **Billing Currency** | USD | String | `Contract.CurrencyIsoCode` |
| **Rate Plan Name** | G3 10MB Dynamic Data Pool | String | `Contract.Rate_Plan_Name__c` |
| **Plan Supported SKUs** | G3-F, G3-G1 | String | `Contract.Supported_SKUs__c` |
| **Monthly Recurring Charge Rate** | 2.43 | Decimal | `Contract.Per_SIM_MRC_Rate__c` |
| **Monthly Recurring Charge Rate Unit** | USD ($) per SIM | String | `Contract.Per_SIM_MRC_Currency_Unit__c` |
| **Monthly Recurring Charge Frequency** | Monthly | String | `Contract.Per_SIM_MRC_Frequency__c` |
| **Included Data Per SIM Value** | 10 | Integer | `Contract.Included_Data_Volume_Per_SIM__c` |
| **Included Data Per SIM Unit** | MB per SIM | String | `Contract.Included_Data_Unit__c` |
| **Data Overage Rate** | 0.75 | Decimal | `Contract.Data_Overage_Rate__c` |
| **Data Overage Rate Unit** | USD ($) per MB | String | `Contract.Data_Overage_Currency_Unit__c` |
| **Data Overage Assessment Frequency** | Pay as you go | String | `Contract.Data_Overage_Frequency__c` |
| **G3-F Hardware Unit Price** | 4.50 | Decimal | `OpportunityLineItem.UnitPrice` |
| **G3-F Hardware Unit Pricing Basis** | USD ($) per SIM | String | `OpportunityLineItem.PricingUnit__c` |
| **Inbound SMS Rate** | 0.00 | Decimal | `Contract.Inbound_SMS_Rate__c` |
| **Inbound SMS Unit** | USD ($) per SMS (Always free) | String | `Contract.Inbound_SMS_Unit__c` |
| **Outbound SMS Rate** | 0.57 | Decimal | `Contract.Outbound_SMS_Rate__c` |
| **Outbound SMS Unit** | USD ($) per Outbound SMS | String | `Contract.Outbound_SMS_Unit__c` |
| **Support Package Tier** | Developer Support | String | `Contract.Support_Tier__c` |
| **Support Package Fee** | 0.00 | Decimal | `Contract.Support_Fee__c` |
| **Support Package Fee Frequency** | Monthly (Free of charge) | String | `Contract.Support_Frequency__c` |
| **Test Mode Duration Value** | 90 | Integer | `Contract.Test_Mode_Duration_Days__c` |
| **Test Mode Duration Unit** | Days | String | `Contract.Test_Mode_Duration_Unit__c` |
| **Test Mode Data Limit Value** | 100 | Integer | `Contract.Test_Mode_Data_Limit__c` |
| **Test Mode Data Limit Unit** | KB | String | `Contract.Test_Mode_Data_Unit__c` |
| **Test Mode SMS Limit Value** | 10 | Integer | `Contract.Test_Mode_SMS_Limit__c` |
| **Test Mode SMS Limit Unit** | Outbound SMS messages | String | `Contract.Test_Mode_SMS_Unit__c` |
| **Minimum Spend Tier 1 Period** | Months 1–3 (beginning 2026-08-01) | String | `MinSpendSchedule__c.Tier_1_Period__c` |
| **Minimum Spend Tier 1 Amount** | 3.00 | Decimal | `MinSpendSchedule__c.Tier_1_Amount__c` |
| **Minimum Spend Tier 1 Rate Unit** | USD ($) per month | String | `MinSpendSchedule__c.Tier_1_Unit__c` |
| **Minimum Spend Tier 2 Period** | Months 4–6 | String | `MinSpendSchedule__c.Tier_2_Period__c` |
| **Minimum Spend Tier 2 Amount** | 300.00 | Decimal | `MinSpendSchedule__c.Tier_2_Amount__c` |
| **Minimum Spend Tier 2 Rate Unit** | USD ($) per month | String | `MinSpendSchedule__c.Tier_2_Unit__c` |
| **Minimum Spend Tier 3 Period** | Months 7–12 | String | `MinSpendSchedule__c.Tier_3_Period__c` |
| **Minimum Spend Tier 3 Amount** | 600.00 | Decimal | `MinSpendSchedule__c.Tier_3_Amount__c` |
| **Minimum Spend Tier 3 Rate Unit** | USD ($) per month | String | `MinSpendSchedule__c.Tier_3_Unit__c` |
| **Minimum Spend Tier 4 Period** | Months 13–24 | String | `MinSpendSchedule__c.Tier_4_Period__c` |
| **Minimum Spend Tier 4 Amount** | 1200.00 | Decimal | `MinSpendSchedule__c.Tier_4_Amount__c` |
| **Minimum Spend Tier 4 Rate Unit** | USD ($) per month | String | `MinSpendSchedule__c.Tier_4_Unit__c` |
| **Minimum Spend Tier 5 Period** | Months 25–36+ | String | `MinSpendSchedule__c.Tier_5_Period__c` |
| **Minimum Spend Tier 5 Amount** | 2400.00 | Decimal | `MinSpendSchedule__c.Tier_5_Amount__c` |
| **Minimum Spend Tier 5 Rate Unit** | USD ($) per month | String | `MinSpendSchedule__c.Tier_5_Unit__c` |
| **Qualifying Spend Definition** | Invoiced charges for Rate Plans, Add-Ons, and Support Packages | String | `Contract.Qualifying_Spend_Rules__c` |
| **Spend Exclusions** | SIM hardware purchases, setup/implementation charges, late payment charges | String | `Contract.Spend_Exclusions__c` |
