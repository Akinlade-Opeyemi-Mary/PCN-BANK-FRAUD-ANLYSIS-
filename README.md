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
