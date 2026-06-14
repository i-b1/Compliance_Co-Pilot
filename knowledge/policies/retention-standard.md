# Data Retention Standard

**Document ID:** STD-RET-002  
**Version:** 2.1  
**Effective Date:** 15 February 2024  
**Owner:** Records Management Office  
**Classification:** Internal  
**Review Cycle:** Biennial

---

## 1. Purpose

This standard defines the minimum and maximum retention periods for data assets held by Contoso Corp. It supports compliance with legal obligations, operational needs, and data minimisation principles.

---

## 2. Scope

This standard applies to all data assets — electronic and physical — owned, managed, or processed by the Company or on its behalf by third parties.

---

## 3. Retention Schedule

### §3.1 Customer and Partner Data

| Record Type | Minimum Retention | Maximum Retention | Disposal Method |
|---|---|---|---|
| Customer contracts | Duration + 7 years | Duration + 10 years | Secure deletion / shredding |
| Customer personal data (account) | Duration of relationship + 7 years | Duration + 10 years | Certified erasure |
| Customer support tickets | 3 years from closure | 5 years | Secure deletion |
| Marketing opt-in records | Duration of consent + 3 years | Duration + 5 years | Secure deletion |
| Transaction records | 7 years from transaction date | 10 years | Secure deletion |

### §3.2 Employee Data

| Record Type | Minimum Retention | Maximum Retention | Disposal Method |
|---|---|---|---|
| Personnel files | Duration of employment + 7 years | Duration + 10 years | Secure deletion / shredding |
| Payroll records | 7 years from payment date | 10 years | Certified erasure |
| Pre-employment screening | Hire date + 5 years | Hire date + 7 years | Secure deletion |
| Disciplinary records | 5 years from incident date | 7 years | Secure deletion |

### §3.3 Financial Records

| Record Type | Minimum Retention | Maximum Retention |
|---|---|---|
| Annual financial statements | 10 years | Permanent |
| Tax records | 7 years | 10 years |
| Audit reports | 10 years | Permanent |
| Expense reports | 7 years | 10 years |

### §3.4 Security and Incident Records

| Record Type | Minimum Retention | Maximum Retention |
|---|---|---|
| Security event logs | 12 months | 24 months |
| Incident investigation records | 5 years from closure | 7 years |
| Penetration test reports | 3 years | 5 years |
| Access review records | 3 years | 5 years |
| Data breach notification records | 5 years from notification date | 7 years |

### §3.5 Legal and Compliance Records

| Record Type | Minimum Retention | Maximum Retention |
|---|---|---|
| Legal hold documents | Duration of hold + 7 years | Duration + 10 years |
| Regulatory correspondence | 7 years from date | 10 years |
| Privacy notices | 5 years from withdrawal | 7 years |
| DPIAs | 5 years from completion | 7 years |
| Data processing agreements | Duration + 7 years | Duration + 10 years |

---

## 4. Legal Hold

### §4.1 Legal Hold Process
When a legal hold is imposed, the standard retention periods are suspended for affected records. The Legal department is responsible for issuing and lifting legal holds. Records under legal hold must not be disposed of until the hold is formally released.

### §4.2 Legal Hold Notification
The Legal department shall notify Records Management of any new legal hold **within 1 business day** of the hold being approved.

---

## 5. Disposal Procedures

### §5.1 Electronic Data
- **Standard deletion:** Overwrite with DoD 5220.22-M standard (3-pass minimum) or NIST SP 800-88 Clear
- **Certified erasure:** Blancco or equivalent certified tool; retain erasure certificate for 5 years
- **Physical destruction:** degaussing followed by physical shredding for HDDs; physical destruction for SSDs

### §5.2 Physical Records
- **Confidential/Restricted:** Cross-cut shredding by approved vendor; retain certificate of destruction
- **Internal/Public:** Standard paper recycling

### §5.3 Cloud and SaaS Data
- Submit formal deletion request to vendor
- Obtain written confirmation of deletion
- Verify against Data Processing Agreement terms
- Document confirmation in the Asset Disposal Register

---

## 6. Roles and Responsibilities

| Role | Responsibility |
|---|---|
| Records Management Officer | Maintain retention schedule; approve exceptions; oversee disposal |
| Data Protection Officer | Ensure retention periods comply with applicable privacy law |
| IT Security | Implement technical deletion controls; provide erasure certificates |
| Line Managers | Ensure team members comply with this standard for records they create/own |
| Legal | Issue legal holds; advise on regulatory retention requirements |

---

## 7. Exceptions

Requests for retention beyond the maximum period require written approval from the Records Management Officer and the DPO. All approved exceptions must be documented in the Exceptions Register.

---

## Document Control

| Version | Date | Author | Changes |
|---|---|---|---|
| 2.1 | 15 Feb 2024 | Records Management | Added §3.4 incident records row; aligned with POL-DATA-001 v3.2 |
| 2.0 | 01 Apr 2022 | Records Management / Legal | Full revision; added cloud disposal procedures |
