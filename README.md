# BEYOND THE TRANSACTION: FRAUD AND FINANCIAL CRIME ANALYTICS — PCN BANK

# EXECUTIVE SUMMARY
This project examines fraud and financial crime records supplied for PCN Bank to understand recorded exposure, investigate customer behaviour and identify gaps in operational monitoring.

Using Google Sheets, Excel, Power Query, Power Pivot and DAX, the data was prepared and analysed to develop three dashboards: Fraud Detection, Customer Fraud Behaviour and Operational Efficiency. Supporting investigation tables, an executive presentation and a recommendation report were also created.

The analysis identified 196 fraud-labelled records among 1,000 supplied records, representing a recorded fraud rate of 19.60%. Of 313 represented customers, 136 had at least one fraud-labelled record. POS recorded the highest fraud-labelled volume, while Money Laundering had the largest provisional recorded amount among case types.

Customer analysis highlighted new-device associations, differences in recorded fraud rates across country-comparison and KYC groups, and higher average hourly transaction activity among fraud-labelled records. These patterns support further investigation but do not establish account takeover, customer wrongdoing or verification failure.

The operational review identified missing dates, missing amounts and posting-date exceptions that limit reporting reliability. Recorded SAR activity was also examined, although the available data cannot establish detection accuracy, investigation speed or control effectiveness.

Recommendations focus on reviewing exposed accounts and customers, resolving data-quality issues, confirming risk definitions and linking alerts with investigation outcomes to support stronger fraud-monitoring decisions.

# COMPANY OVERVIEW

PCN Bank is the banking client featured in this fraud and financial crime analytics project. The project brief focuses on monitoring transaction activity, understanding customer behaviour and improving visibility into potential financial-crime risks across banking channels.

The analytical scope includes card fraud, identity theft, account takeover, money laundering and suspicious activity reporting. Transaction records are examined alongside customer verification status, device indicators and behavioural information to identify patterns that warrant investigation.

This project supports those objectives through three Excel dashboards covering fraud detection, customer fraud behaviour and operational efficiency. Together, they provide a structured view of recorded exposure, investigation priorities and the data gaps that must be addressed before assessing detection performance.

# BUSINESS CHALLENGES

PCN Bank’s project brief highlights the need to improve visibility into fraud exposure, customer behaviour and operational performance. The analysis identified several challenges affecting the interpretation of the supplied records:

* **Fraud Exposure Across Channels:**  
  Fraud counts and rates highlight different priorities. High-volume channels require attention, while high rates in small groups need careful interpretation.

* **Identifying Accounts for Investigation:**  
  Accounts with fraud-labelled records can be identified, but formal high-risk classification requires agreed criteria and supporting case evidence.

* **Understanding Customer Behaviour:**  
  Device indicators, country differences, KYC status and transaction activity provide investigation leads. However, these signals alone cannot confirm account takeover, frequent overseas activity or verification failure.

* **Data Quality and Reporting Consistency:**  
  Missing dates and amounts, repeated transaction IDs and incomplete KYC coverage limit reliable comparisons and require reconciliation.

* **Interpreting Posting Intervals:**  
  Negative and unusually long posting intervals require source-date validation before they can be classified as processing delays.

* **Separating Fraud Labels from Suspicious Activity:**  
  Fraud labels and SAR filing indicators represent different concepts. Confirmed definitions are needed before suspicious transactions and flagged activity can be measured separately.

* **Assessing Detection and Investigation Performance:**  
  Linked alerts, confirmed outcomes and investigation timestamps are needed to evaluate detection accuracy, case-handling delays and operational bottlenecks.

These challenges establish two priorities: investigate the recorded exposure and strengthen the evidence needed for reliable monitoring and performance assessment.

# DATA ARCHITECTURE & TOOLS

## Data Source

The client-provided dataset is available [here](https://docs.google.com/spreadsheets/d/1FqYXQi01P7SjB5wWxljvJbIgr7BOtlxc/edit).

The analysis covers **1,000 records**, representing **930 unique transaction IDs** and **313 unique customer IDs**. Raw data was preserved separately from the prepared dataset to maintain traceability.

## Data Structure and Column Definitions

The prepared dataset contains 37 fields covering identifiers, transaction details, customer behaviour, verification information and investigation context.

| Column | Definition |
| --- | --- |
| `record_id` | Identifier for each supplied record, used to trace individual rows and data-quality exceptions. |
| `transaction_id` | Recorded transaction identifier. Some IDs appear more than once, so record counts differ from unique transaction counts. |
| `customer_id` | Identifier used to group records belonging to a represented customer. |
| `account_id` | Identifier used for account-level exposure analysis. |
| `customer_name` | Recorded customer name. |
| `case_type` | Recorded case category, such as Card Fraud, Account Takeover or Money Laundering. |
| `fraud_label` | Standardised fraud indicator: 1 for fraud-labelled records and 0 for non-fraud-labelled records. |
| `sar_flag` | Recorded Suspicious Activity Report filing indicator: 1 for filed and 0 for not filed, according to the supplied encoding. |
| `transaction_date` | Prepared date of the transaction. |
| `transaction_date_status` | Indicates whether a transaction date is available or missing. |
| `transaction_time` | Recorded transaction time of day. |
| `posting-date` | Prepared date on which the transaction was recorded as posted. |
| `posting_date_status` | Indicates whether a posting date is available or missing. |
| `transaction_amount_usd` | Recorded amount in the field named USD. Its currency-conversion basis requires confirmation. |
| `transaction_currency` | Recorded transaction currency code. |
| `merchant_name` | Recorded merchant or counterparty name. |
| `merchant_category` | Recorded merchant or activity category, such as Electronics, P2P Transfer or Retail. |
| `channel` | Recorded banking channel, such as POS, Mobile Banking, ATM or Branch. |
| `merchant_country` | Recorded merchant country, including an Unknown category. |
| `customer_country` | Recorded customer country; it does not establish the customer’s location during the transaction. |
| `ip_address` | Recorded IP address associated with the activity. |
| `device_id` | Device identifier associated with a transaction record. |
| `new_device_flag` | Recorded indicator distinguishing new-device from existing-device activity. The definition of “new” requires confirmation. |
| `failed_login_attempts_24h` | Recorded number of failed login attempts within the named 24-hour window. |
| `txn_count_1h` | Recorded transaction count within the named one-hour window. |
| `account_age_days` | Recorded account age in days. |
| `customer_age` | Recorded customer age. |
| `kyc_status` | Recorded Know Your Customer verification status, such as Complete, Pending, Expired or Under Review. |
| `aml_risk_score` | Recorded Anti-Money Laundering risk score. Its scale and classification thresholds require confirmation. |
| `email` | Recorded customer email address. |
| `phone` | Recorded customer telephone number. |
| `billing_address` | Recorded billing address. |
| `shipping_address` | Recorded shipping address. |
| `billing_shipping_match` | Recorded indicator of whether billing and shipping addresses match. |
| `card_number_masked` | Masked card information supplied in the record. |
| `notes` | Supplementary information associated with the record. |
| `Posting_Interval_Days` | Calculated difference between posting date and transaction date, available when both dates exist. |

## Date Table

A separate calendar table supports time-based analysis.

| Column | Definition |
| --- | --- |
| `Date` | Calendar date used to connect calendar information with transaction records. |
| `Year` | Calendar year. |
| `Month Number` | Numerical month, used for chronological sorting. |
| `Month Name` | Readable month label. |
| `Year Month` | Combined year-month label for comparisons across years. |
| `Year Month Sort` | Numerical key used to sort year-month labels correctly. |

## Important Data Notes

* **Records and transactions are different:** repeated transaction IDs remain in the dataset and require review before deduplication.
* **Missing values remain unknown:** missing dates, amounts and behavioural values were not automatically replaced with zero.
* **Date availability does not establish accuracy:** ambiguous dates and negative or unusually long posting intervals require source validation.
* **Fraud labels describe recorded classifications:** they are not independently confirmed investigation outcomes.
* **Amounts remain provisional:** the USD-named amount field and repeated transaction IDs require validation before interpreting totals as unique transaction exposure.
* **Device IDs describe transaction-associated devices:** they do not establish separate login events.
* **Customer and merchant country differences do not prove overseas travel.**
* **KYC status timing is unknown:** segment comparisons cannot establish verification effectiveness.
* **Contact and address fields contain customer information:** public portfolio materials require appropriate permission and sanitisation.

## Tools and Technologies Used

| Tool | Purpose |
| --- | --- |
| **Google Sheets** | Initial cleaning, whitespace removal, flag normalisation and selected category standardisation. |
| **Microsoft Excel 2021** | PivotTables, charts, linked KPI cards, dashboard views and investigation tables. |
| **Power Query** | Data import, transformation, type handling and date-quality review. |
| **Power Pivot** | Shared analytical model, calendar data and centralised calculations. |
| **DAX** | Measures and calculated columns for fraud exposure, customer behaviour, SAR activity and completeness. |
| **VBA** | Chart-switching controls for case/channel and failed-login/hourly-activity views. |
| **PowerPoint** | Dashboard layout design and executive presentation. |
| **GitHub** | Documentation of the project’s methods, findings, limitations and recommendations. |

## Data Architecture Workflow

**Raw dataset → Google Sheets preparation → Power Query → Power Pivot and DAX → PivotTable investigation → Excel dashboards → Executive presentation and recommendations**

This workflow provides a shared reporting foundation for the project. It does not establish integration with live banking systems or continuous real-time monitoring.
