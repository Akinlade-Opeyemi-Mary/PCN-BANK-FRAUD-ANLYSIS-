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

# DATA CLEANING & PREPARATION

The dataset was reviewed and prepared to improve consistency before modelling and analysis. The raw data was preserved separately so that cleaned values and exceptions could be traced back to the source.

Initial preparation was completed in Google Sheets following Excel performance interruptions. The prepared data was then imported into Excel through Power Query for further type handling and validation.

### 1. Text Cleaning and Category Standardisation

Unnecessary spaces and non-printing characters were addressed using functions such as `TRIM`, `CLEAN` and `SUBSTITUTE`.

Selected category labels and country names were standardised for consistent grouping. Meaningful distinctions were retained: categories such as Credit Card and Card Fraud were not combined without supporting definitions.

### 2. Binary Flag Normalisation

Fraud labels, SAR filing indicators and new-device flags contained different representations of the same values.

| Field | Examples standardised to 0 | Examples standardised to 1 |
| --- | --- | --- |
| Fraud label | `0`, `N`, `No` | `1`, `Y`, `Yes` |
| SAR filing indicator | `0`, `N`, `No`, `Not Filed` | `1`, `Y`, `Yes`, `Filed` |
| New-device flag | `0`, `N`, `No`, `False` | `1`, `Y`, `Yes`, `True` |

Whitespace and case differences were handled before mapping. Unknown values were not assumed to mean zero; mappings remained specific to each field.

### 3. Date and Time Preparation

Mixed date representations were reviewed and converted into date values for analysis. Transaction time was handled separately as a time field.

Missing dates remained blank rather than being replaced with artificial dates or text inside date columns. Separate date-status fields were added to distinguish available and missing dates.

Ambiguous day/month values remain subject to source validation. A consistent display format does not establish that a date was interpreted correctly.

### 4. Numerical Data Types

Transaction amounts, activity counts, ages and risk scores were reviewed for numerical use in Power Query and the analytical model.

Missing numerical values were retained as unknown. Missing amounts were not treated as zero-value transactions, and missing behavioural counts were not treated as zero activity.

### 5. Repeated Transaction IDs

The prepared dataset retained all **1,000 records**, containing **930 unique transaction IDs**.

Repeated IDs were preserved because some associated records contain different amounts or dates. Removing them without confirming their meaning could discard relevant information or distort the analysis.

### 6. Data-Quality Checks

The prepared data and supporting investigation views identified the following exceptions:

| Check | Finding |
| --- | ---: |
| Missing transaction dates | 86 records |
| Missing posting dates | 62 records |
| Records missing either date | 143 records |
| Missing transaction amounts | 14 records |
| Missing KYC status | 15 records |
| Missing device IDs | 345 records |
| Negative posting intervals | 63 records |

Missing-date counts overlap: five records lack both dates. Negative and unusually long posting intervals were retained for investigation rather than corrected by guessing.

### Preparation Outcome

The prepared dataset supported the Power Pivot model, analytical measures and three Excel dashboards. Outstanding issues—including date interpretation, repeated transaction IDs, currency definitions and incomplete segment coverage—were documented as limitations requiring further validation.

## DATA MODELLING & KPI DEFINITIONS

The prepared data was loaded into Power Pivot to create a shared analytical model for the three Excel dashboards. DAX measures and calculated columns supported consistent calculations across PivotTables and dashboard views.

### Data Model Structure

The model contains two main tables:

| Table | Role | Description |
| --- | --- | --- |
| `Cleaned_Data` | Fact table | Contains the 1,000 prepared records, including transaction details, customer identifiers, recorded flags and behavioural information. |
| `Date_Table` | Date dimension | Contains calendar dates and fields for year, month and year-month analysis. |

The analytical grain is **one supplied record per row**, rather than one unique transaction. Repeated transaction IDs remain in the fact table, so record counts and distinct transaction counts are reported separately.

### Table Relationships

The model diagram shows two one-to-many relationships between the calendar and prepared records:
![PCN Bank Power Pivot Data Model](https://github.com/Akinlade-Opeyemi-Mary/PCN-BANK-FRAUD-ANLYSIS-/blob/e14a1f0973ac765b3de943f8d2f09efe07a1fdfa/Cleaned%20Data%20and%20Date%20Table%20Relationship.png)

The model connects `Date_Table` with `Cleaned_Data` through one-to-many relationships. The solid line represents an active relationship, while the dashed line represents an inactive relationship.

* **One side:** the calendar table, containing a unique row for each date.
* **Many side:** the prepared records, where multiple records can share a date.
* **Solid line:** the active relationship used for default date filtering.
* **Dashed line:** an inactive relationship representing an alternative date role.

Transaction-date and posting-date analysis require different date roles. The relationship endpoints must be checked to ensure transaction trends use `transaction_date`. Calculations using an inactive date relationship require explicit handling in DAX.

### Calculated Fields

Calculated and prepared fields supported grouping and investigation:

| Field | Analytical purpose |
| --- | --- |
| Fraud Status | Provides readable groups for fraud-labelled and non-fraud-labelled records. |
| Country Comparison | Compares recorded customer and merchant countries. Unknown-country treatment requires review. |
| Transaction Date Status | Distinguishes available and missing transaction dates. |
| Posting Date Status | Distinguishes available and missing posting dates. |
| Posting Interval Days | Calculates posting date minus transaction date when both are available. |
| Calendar fields | Support chronological month and year-month analysis. |

### Fraud Detection KPIs

| KPI | Definition |
| --- | --- |
| Total Records | Number of rows in `Cleaned_Data`. |
| Distinct Transactions | Number of unique transaction IDs. |
| Records Labelled Fraud | Number of records where `fraud_label = 1`. |
| Records with Known Fraud Label | Number of records with a nonmissing fraud label equal to 0 or 1. |
| Recorded Fraud Rate | Fraud-labelled records divided by records with a known fraud label. |
| Fraud-Labelled Amount USD | Sum of available amounts on fraud-labelled records, using the USD-named field. |

Recorded amounts remain provisional because currency definitions and repeated transaction IDs require validation. They are not confirmed financial losses.

### Customer Fraud Behaviour KPIs

| KPI | Definition |
| --- | --- |
| Distinct Customers | Number of unique represented customer IDs. |
| Customers with Fraud-Labelled Records | Unique customers with at least one fraud-labelled record. |
| Customer Fraud Exposure Rate | Exposed customers divided by represented customers. |
| Average Failed Login Attempts | Average of available values in `failed_login_attempts_24h`. |
| Average Hourly Transaction Count | Average of available values in `txn_count_1h`. |
| Distinct Devices Used | Number of available unique device IDs associated with a customer’s transaction records. |
| Unique Transactions with Known Countries | Distinct transaction IDs eligible for a comparison using known customer and merchant countries. |
| Unique Cross-Country Transactions | Eligible distinct transaction IDs with different customer and merchant countries. |
| Cross-Country Transaction Share | Unique cross-country transactions divided by unique transactions with known countries. |

Device counts do not establish separate login events. Cross-country shares do not measure frequency over time without an agreed observation period.

### Operational Efficiency KPIs

| KPI | Definition |
| --- | --- |
| SAR Filed Records | Number of records where `sar_flag = 1`. |
| Fraud Records with SAR Filed | Records with both `fraud_label = 1` and `sar_flag = 1`. |
| Recorded SAR Filing Share on Fraud | Fraud-labelled records with SAR filed divided by fraud-labelled records. |
| Transaction Date Completeness | Records with an available transaction date divided by total records. |
| Posting Date Completeness | Records with an available posting date divided by total records. |
| Records Missing Amount | Number of records without a transaction amount. |
| Average Posting Interval Days | Average posting interval within the defined usable-date population. |
| Records with Negative Posting Interval | Records where posting date precedes transaction date. |

Date completeness measures availability, not accuracy. Posting intervals do not measure fraud-investigation speed, and SAR filing share does not establish compliance or detection effectiveness.

### Calculation and Formatting Principles

* Rates use matching numerators, denominators and filter contexts.
* Percentage measures return fractions and are formatted as percentages; they are not divided by 100 again.
* Counts are displayed as whole numbers, while averages retain appropriate decimal precision.
* Missing numerical values are excluded from averages rather than replaced with zero.
* Distinct customer, transaction and device counts are not automatically additive across groups.
* Total rates are recalculated from their components rather than calculated by averaging segment percentages.
* Date relationships and filter behaviour require validation before interpreting time-based results.

# EXECUTIVE DASHBOARDS & ANALYSIS

The analysis begins with a central question: **where does recorded fraud exposure concentrate, and what should PCN Bank investigate first?** The Fraud Detection Dashboard examines the scale of exposure across case types, banking channels, merchants and countries. Counts, rates and recorded amounts reveal different priorities, connecting the overall picture to account-level investigation.

The story then moves beyond transactions to the customers represented in those records. The Customer Fraud Behaviour Dashboard explores device associations, country differences, verification status and transaction activity to understand which patterns warrant closer review. These comparisons reveal investigation leads while raising questions about missing information, small customer segments and the timing of recorded events.

Finally, the Operational Efficiency Dashboard examines the evidence supporting monitoring itself. SAR activity, incomplete dates, missing amounts and posting-date exceptions show where reporting visibility is limited. Together, the three dashboards connect **where exposure appears → which customer patterns deserve investigation → what evidence PCN Bank needs to make stronger decisions**.

Three Excel dashboards were developed to examine recorded fraud exposure, customer behaviour and operational monitoring. The analysis identifies investigation priorities, explains their business implications and highlights questions that the available data cannot answer.

---

## 1. Fraud Detection Dashboard

**Objective:** Understand the scale of recorded fraud, identify concentrations across cases and channels, and support account-level investigation.

**Key Insights:**

- **Total Records:** 1,000
- **Unique Transaction IDs:** 930
- **Fraud-Labelled Records:** 196
- **Recorded Fraud Rate:** 19.60%

### Fraud by Case Type

- Credit Card recorded the highest fraud-labelled count: **55 records**, followed by Card Fraud with **44**.
- Money Laundering had the largest provisional recorded amount: **$185.23K**, despite accounting for only **15 fraud-labelled records**.
- Account Takeover followed with **$151.86K** across **38 fraud-labelled records**.

**Business implication:** Fraud count and recorded amount identify different investigation priorities. Categories with fewer records can still represent substantial recorded exposure. Amounts require currency and repeated-ID validation before being interpreted as unique transaction totals or losses.

### Channel Exposure

- POS recorded the highest fraud-labelled volume: **50 records**, with a **20.83% recorded fraud rate**.
- E-Commerce followed with **36 fraud-labelled records**, with a **20.11% rate**.
- ATM, Branch and Wire Transfer displayed **100% recorded fraud rates**, but contained only **14, 6 and 5 records**, respectively.

**Business implication:** Review high-volume channels alongside small groups with high observed rates. The small-channel results do not establish bank-wide risk or control failure.

### Merchant and Geographic Concentrations

- P2P Transfer led merchant-category recorded amounts at **$105.41K**, followed by Wire Transfer at **$74.83K**.
- Electronics had the largest merchant-category fraud-labelled count: **24 records**.
- Nigeria had the highest merchant-country fraud-labelled count: **32**, followed by India with **27** and the United States with **26**.

**Business implication:** Transfer-related amounts and electronics-related record volumes warrant different reviews. Country counts describe exposure volume; they do not establish country fraud rates.

### Monthly Trend

- The saved monthly view was highest in **July: 28 fraud-labelled records**, and lowest in **June: 1**.
- The monthly view contains **180 records**; another **16 fraud-labelled records lack transaction dates**.

**Business implication:** Validate date interpretation and year filtering before describing seasonal peaks. The monthly chart does not represent all 196 fraud-labelled records.

### Investigation Gaps and Unanswered Questions

| Question | Available evidence | Remaining gap |
| --- | --- | --- |
| How many transactions are suspicious or independently flagged? | Fraud labels and SAR indicators | Confirm flag definitions and distinguish suspicion, alerts and fraud labels. |
| Which accounts are formally high risk? | Account-level recorded exposure in the investigation table | Agree classification criteria and review case evidence. |
| How accurate is fraud detection? | Recorded fraud classifications | Obtain matched alerts and confirmed outcomes. |
| What are the confirmed financial losses? | Provisional recorded amounts | Obtain verified losses, currency definitions and repeated-ID treatment. |

**Takeaway:** The dashboard identifies where recorded exposure concentrates and which accounts warrant review. It does not establish confirmed losses, formal risk classifications or detection effectiveness.

![Fraud Detection Dashboard](https://github.com/Akinlade-Opeyemi-Mary/PCN-BANK-FRAUD-ANLYSIS-/blob/235dd83b46996c1f5fb4eb97ff8e9e20ea42e3e0/FRAUD%20DETECTION%20DASHBORD.png)

---

## 2. Customer Fraud Behaviour Dashboard

**Objective:** Understand customer exposure and investigate patterns associated with devices, country comparisons, verification status and transaction activity.

**Key Insights:**

- **Unique Customers:** 313
- **Customers with Fraud-Labelled Records:** 136
- **Customer Fraud Exposure Rate:** 43.45%
- **Fraud Records with a New-Device Flag:** 138 of 196, or 70.41%

### Device and Country Associations

- New-device flags appeared on **70.41% of fraud-labelled records**.
- Recorded fraud rates were **48.40% for different-country records**, compared with **2.24% for same-country records**.
- The saved different-country comparison includes Unknown merchant countries, requiring eligibility review.

**Business implication:** Device and country associations support investigation of authentication and transaction context. They do not prove account takeover or overseas travel. The device percentage is a share of fraud records, not the fraud rate among new devices.

### KYC Exposure

| KYC Status | Total Records | Fraud-Labelled Records | Recorded Fraud Rate |
| --- | ---: | ---: | ---: |
| Pending | 13 | 13 | 100.00% |
| Expired | 12 | 12 | 100.00% |
| Under Review | 11 | 11 | 100.00% |
| Complete | 949 | 158 | 16.65% |

- Complete contains the most fraud-labelled records by count because it is a much larger group.
- Pending, Expired and Under Review show higher rates in small groups.
- The displayed KYC table excludes **15 blank-status records**, including **2 fraud-labelled records**.

**Business implication:** Reconcile missing categories and review KYC timing before assessing verification performance. These associations do not demonstrate that verification failed.

### Login and Transaction Activity

| Available-Value Average | Fraud-Labelled Records | Non-Fraud-Labelled Records |
| --- | ---: | ---: |
| Failed Login Attempts | 14.75 | 14.69 |
| Hourly Transaction Count | 36.49 | 17.01 |

- Failed-login averages were very similar.
- Hourly transaction activity was higher among fraud-labelled records.
- Missing numerical values were excluded from averages rather than treated as zero.

**Business implication:** Higher recorded hourly activity warrants review, while failed-login averages provide little separation between the groups. Neither comparison establishes causation.

### Customer Exposure by Country

- United Arab Emirates had **21 customers with fraud-labelled records**.
- Singapore followed with **20**, and the United Kingdom with **19**.

**Business implication:** These are exposed-customer counts, not country risk rates. Customer denominators are needed before comparing prevalence.

### Investigation Gaps and Unanswered Questions

| Question | Available evidence | Remaining gap |
| --- | --- | --- |
| Which customers make frequent cross-country transactions? | Customer-level distinct transaction counts and cross-country shares | Define “frequent,” establish an observation period and validate dates, countries and repeated IDs. |
| Which customers have multiple-device logins? | Device IDs associated with transaction records | Obtain actual login events, timestamps and device ownership information. |
| Does KYC reduce fraud? | Recorded KYC groups and fraud labels | Reconcile missing status, establish timing and obtain comparable control/outcome evidence. |
| Do device or country differences cause fraud? | Observed associations | Additional contextual evidence and an appropriate causal analysis. |

**Takeaway:** The dashboard identifies customer investigation leads. It does not confirm multiple-device logins, frequent overseas activity or the effectiveness of verification controls.

![Customer Fraud Behaviour Dashboard](https://github.com/Akinlade-Opeyemi-Mary/PCN-BANK-FRAUD-ANLYSIS-/blob/610ad9b612eced1d0650bd3ba5bcbfed6df60b5d/CUSTOMER%20FRAUD%20BEHAVIOUR%20DASHBOARD.png)

---

## 3. Operational Efficiency Dashboard

**Objective:** Examine recorded SAR activity, monitoring-data completeness and posting-date exceptions, while identifying the evidence needed to assess operational performance.

**Key Insights:**

- **SAR-Filed Records:** 17
- **Recorded SAR Filing Share on Fraud:** 8.67%
- **Transaction-Date Completeness:** 91.40%
- **Posting-Date Completeness:** 93.80%
- **Records Missing Transaction Amounts:** 14

### SAR Filing Activity

| Case Type | SAR-Filed Records | Filing Share on Fraud |
| --- | ---: | ---: |
| AML Suspicion | 6 | 75.00% |
| Money Laundering | 11 | 73.33% |

**Business implication:** The data describes recorded filing activity. It does not establish missed mandatory filings, compliance or control effectiveness.

### Monitoring-Data Gaps

- **86 records** lack transaction dates.
- **62 records** lack posting dates.
- **143 records** lack either date; five lack both.
- Mobile Banking had the largest missing-amount count: **4**, followed by POS with **3**.

**Business implication:** Missing information limits time analysis and recorded-amount coverage. Compare missing counts with channel denominators before ranking completeness.

### Posting-Date Exceptions

- **63 records** have posting dates before transaction dates.
- The saved ATM average posting interval was **28.43 days**, including **89-day and 294-day intervals**.
- These values require source-date validation before being classified as delays.

**Business implication:** Unusual intervals may reflect date interpretation or recording issues. Posting intervals do not measure fraud-detection or investigation speed.

### Investigation Gaps and Unanswered Questions

| Question | Available evidence | Remaining gap |
| --- | --- | --- |
| Where are investigation delays and backlogs? | Posting dates and interval exceptions | Obtain alert, assignment, investigation-start and closure timestamps, plus case status. |
| How accurate is the detection system? | Fraud labels and SAR activity | Obtain matched alerts and confirmed outcomes. |
| Are prevention measures reducing fraud? | A supplied snapshot of recorded activity | Obtain control-change history and comparable periods/populations. |
| Do long posting intervals represent genuine delays? | Prepared dates and calculated intervals | Validate original dates and posting-process definitions. |

**Takeaway:** The dashboard reveals reporting activity and monitoring-data gaps. Further evidence is required to assess detection accuracy, investigation bottlenecks and prevention effectiveness.

![Operational Efficiency Dashboard](https://github.com/Akinlade-Opeyemi-Mary/PCN-BANK-FRAUD-ANLYSIS-/blob/66318a5f53a3c2532a68c9af11db8224f9a43987/OPERATIONAL%20EFFICIENCY%20DASHBOARD.png)

---

## Executive Summary of Findings

Across the dashboards, three connected findings emerged:

- **Exposure has different dimensions:** fraud counts, rates and provisional amounts highlight different investigation priorities.
- **Customer signals need context:** device, country, KYC and activity patterns warrant review, but missingness, small groups and event timing limit interpretation.
- **Operational assessment needs better evidence:** data-quality exceptions affect visibility, while linked alerts and case outcomes are needed to evaluate performance.

## Key Insight

PCN Bank’s immediate priority is to investigate recorded exposure while strengthening the reliability of its monitoring evidence. Validated dates, agreed risk definitions, reconciled segments and linked investigation outcomes will support more defensible reporting and performance assessment.
