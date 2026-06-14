# Data Handling Policy

**Document ID:** POL-DATA-001  
**Version:** 3.2  
**Effective Date:** 1 March 2024  
**Owner:** Chief Privacy Officer  
**Classification:** Internal  
**Review Cycle:** Annual

---

## 1. Purpose and Scope

This policy establishes the requirements for handling personal data and sensitive information at Contoso Corp ("the Company"). It applies to all employees, contractors, and third parties who collect, store, process, or transmit personal data on behalf of the Company.

---

## 2. Data Classification

### §2.1 Classification Tiers

| Tier | Description | Examples |
|---|---|---|
| **Public** | Information approved for unrestricted public disclosure | Press releases, marketing materials |
| **Internal** | Information for internal use only | Organisational charts, meeting notes |
| **Confidential** | Sensitive business information | Financial projections, contracts |
| **Restricted** | Highest sensitivity; strictly controlled | Personal data, credentials, health records |

### §2.2 Personal Data
All personal data (as defined under applicable privacy laws) shall be classified as **Restricted** at minimum.

---

## 3. Data Collection Principles

### §3.1 Purpose Limitation
Personal data shall be collected only for specified, explicit, and legitimate business purposes documented in the applicable privacy notice.

### §3.2 Data Minimisation
Only data strictly necessary for the stated purpose shall be collected. Collection of additional data fields requires written approval from the Chief Privacy Officer.

### §3.3 Consent and Legal Basis
A valid legal basis (consent, contract, legal obligation, legitimate interest) must be documented before collection begins. The Legal Basis Register maintained by the Privacy Office must be updated within 5 business days of any new data processing activity.

---

## 4. Data Incident and Breach Response

### §4.1 Incident Classification
Data incidents are classified as:
- **Level 1 (Minor):** No personal data involved; low business impact
- **Level 2 (Significant):** Personal data involved; limited scope (fewer than 100 data subjects)
- **Level 3 (Major):** Personal data involved; wide scope (100+ data subjects) or sensitive categories

### §4.2 Internal Notification Requirements

Upon discovery of a potential data security incident involving personal data, the following internal notification steps must be completed:

1. The discovering employee shall report the incident to the Information Security team **within 4 business hours** via the Incident Reporting Portal.
2. The Information Security team shall perform an initial triage and assign an incident severity level within **1 business day**.
3. The Data Protection Officer (DPO) shall be notified of any Level 2 or Level 3 incident within **1 business day** of triage completion.
4. A formal Incident Assessment Report shall be completed by the Security team and submitted to the DPO and Legal within **3 business days** of initial discovery.

### §4.3 External Notification to Supervisory Authorities

Where the Incident Assessment Report concludes that a personal data breach has occurred and is likely to result in a risk to the rights and freedoms of individuals, external notification obligations are triggered:

**The Company shall notify the relevant supervisory authority no later than 5 business days after confirming the personal data breach.**

The notification shall include:
- Nature of the breach and approximate number of data subjects affected
- Categories of personal data involved
- Contact details of the Data Protection Officer
- Likely consequences of the breach
- Measures taken or proposed to address the breach

> **Note:** The 5 business day standard above is the Company's internal compliance target. Applicable law (e.g. GDPR Art. 33) may impose a shorter absolute deadline. It is the DPO's responsibility to ensure that legal deadlines are not exceeded.

### §4.4 Notification to Data Subjects

Where the breach is likely to result in a **high risk** to data subjects, the Company shall notify affected individuals **within 10 business days** of confirming the breach, subject to any law enforcement restriction.

### §4.5 Documentation
All incidents, regardless of severity, must be logged in the Incident Register within **2 business days** of resolution. The Incident Register is maintained by the Privacy Office.

---

## 5. Data Retention and Disposal

### §5.1 Retention Schedule

| Data Category | Retention Period |
|---|---|
| Customer personal data | Duration of relationship + 7 years |
| Employee personal data | Duration of employment + 7 years |
| Marketing contact data | Until consent withdrawn, max 3 years from last interaction |
| Security logs | 12 months rolling |
| Incident records | 5 years from closure |

### §5.2 Disposal Requirements
- Paper documents: cross-cut shredding (DIN 66399 Level P-4 minimum)
- Electronic media: NIST SP 800-88 Guidelines for Media Sanitization
- Cloud data: documented deletion with vendor confirmation

---

## 6. Data Subject Rights

### §6.1 Response Timelines

The Company shall respond to data subject rights requests within the timeframes below:

| Request Type | Response Deadline |
|---|---|
| Access request | Within 30 calendar days of receipt |
| Erasure request | Within 30 calendar days of receipt |
| Portability request | Within 30 calendar days of receipt |
| Rectification request | Within 30 calendar days of receipt |
| Objection / restriction | Without undue delay; acknowledge within 5 business days |

### §6.2 Extension
The 30-day deadline may be extended by up to 2 additional months for complex or numerous requests. The data subject must be notified of the extension within the initial 30-day period.

---

## 7. Third-Party Data Processors

### §7.1 Due Diligence
Before engaging any third party that will process personal data, the Procurement and Privacy teams must complete a Data Protection Impact Assessment (DPIA) and a vendor security review.

### §7.2 Contractual Requirements
All processors must sign a Data Processing Agreement (DPA) that includes:
- Processing only on documented instructions
- Confidentiality obligations
- Security measures (Art. 32 equivalent or NIST CSF)
- Sub-processor approval requirements
- Breach notification to the Company **within 24 hours** of discovery

---

## 8. Cross-Border Data Transfers

Personal data shall not be transferred outside the European Economic Area (EEA) or to countries without an adequacy decision unless one of the following mechanisms is in place:
- Standard Contractual Clauses (SCCs) approved by the European Commission
- Binding Corporate Rules (BCRs) approved by a lead supervisory authority
- Written approval from the Chief Privacy Officer for other transfer mechanisms

---

## Document Control

| Version | Date | Author | Changes |
|---|---|---|---|
| 3.2 | 01 Mar 2024 | Privacy Office | Updated §4.3 deadline from 3 to 5 business days; added §6.2 extension clause |
| 3.1 | 15 Jan 2023 | Privacy Office | Added §7.2 sub-processor requirements |
| 3.0 | 01 Jun 2022 | Legal / Privacy | Full revision post-GDPR enforcement review |
