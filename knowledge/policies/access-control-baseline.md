# Access Control Baseline Standard

**Document ID:** STD-SEC-003  
**Version:** 4.0  
**Effective Date:** 1 January 2024  
**Owner:** Chief Information Security Officer (CISO)  
**Classification:** Internal  
**Review Cycle:** Annual

---

## 1. Purpose

This standard establishes the minimum access control requirements for all information systems, applications, and data assets operated by or on behalf of Contoso Corp. It implements the access control requirements of SOC 2 CC6 and aligns with ISO/IEC 27001:2022 Annex A.5.

---

## 2. Scope

This standard applies to:
- All Company-owned and managed information systems
- Cloud services (IaaS, PaaS, SaaS) processing Company data
- Third-party systems with access to Company networks or data
- All workforce members, contractors, and service accounts

---

## 3. Identity and Authentication

### §3.1 Identity Lifecycle

| Event | Required Action | Deadline |
|---|---|---|
| New hire / contractor start | Provision access as per Role Matrix | On or before start date |
| Role change | Modify access to new role; revoke old privileges | Within 1 business day of effective date |
| Termination / offboarding | Revoke all access | **Within 4 hours of effective termination time** |
| Extended leave (>30 days) | Suspend (not delete) all access | Before leave start date |

### §3.2 Authentication Requirements

| System Sensitivity | Authentication Requirement |
|---|---|
| Public-facing systems | Password (min 12 chars) |
| Internal systems (standard) | Password + MFA (TOTP or FIDO2) |
| Privileged / admin access | MFA (FIDO2 hardware key preferred) + PAM tool |
| Production databases | MFA + just-in-time (JIT) access via PAM |

### §3.3 Password Policy
- Minimum length: 14 characters
- Complexity: uppercase, lowercase, digit, and special character
- Maximum age: 365 days (or immediate reset on suspected compromise)
- History: last 12 passwords may not be reused
- Lockout: 5 failed attempts → 15-minute lockout; 10 failed attempts → helpdesk unlock required

---

## 4. Authorisation and Role-Based Access Control

### §4.1 Principle of Least Privilege
Access rights must be limited to the minimum required for the individual to perform their job function. Access beyond the documented Role Matrix requires a formal access request and manager approval.

### §4.2 Segregation of Duties
No single individual should have both the ability to initiate and approve a high-risk transaction. Compensating controls (audit logs, regular review) are acceptable where full segregation is not operationally feasible, subject to CISO approval.

### §4.3 Privileged Access
- Privileged accounts (admin, root, DBA) must be distinct from standard user accounts
- Privileged sessions must be recorded via the Privileged Access Management (PAM) tool
- Privileged access reviews must be conducted **quarterly**

### §4.4 Service Accounts
- Service accounts must be non-interactive (no human login allowed)
- Credentials must be stored in the approved secrets management vault (e.g. Azure Key Vault)
- Rotation period: 90 days maximum; 30 days for accounts with elevated privileges

---

## 5. Access Reviews

### §5.1 Review Frequency

| Access Type | Review Frequency | Reviewer |
|---|---|---|
| Standard user access | Semi-annual | Line manager + IT Security |
| Privileged access | Quarterly | CISO + IT Security |
| Third-party / vendor access | Quarterly | Vendor manager + IT Security |
| Service accounts | Semi-annual | System owner + IT Security |

### §5.2 Review Process
1. IT Security generates access review reports from the Identity Governance system
2. Reviewers certify or revoke each access grant within **10 business days**
3. Revocations must be actioned by IT Security within **1 business day** of reviewer decision
4. Results are logged in the Access Review Register; exceptions documented and risk-accepted

---

## 6. Physical Access Controls

### §6.1 Facility Tiers

| Tier | Description | Control Requirements |
|---|---|---|
| Tier 0 (Data Centre) | Primary/secondary data centres | Mantraps, biometric + badge, CCTV, 24×7 guard, visitor escort |
| Tier 1 (Restricted Office) | Server rooms, comms rooms | Badge access, CCTV, visitor log |
| Tier 2 (General Office) | Standard office space | Badge access, visitor sign-in |
| Tier 3 (Common Areas) | Lobbies, reception | Unmanned or receptionist control |

### §6.2 Visitor Management
All visitors to Tier 0 and Tier 1 areas must be:
- Registered in the Visitor Management System at least 24 hours in advance (except emergencies)
- Escorted at all times by an authorised employee
- Issued a visitor badge that must be visibly worn

---

## 7. Remote Access

### §7.1 VPN and Remote Connectivity
- All remote access to internal systems must use the corporate VPN (Zero Trust Network Access gateway)
- Split-tunnel VPN is prohibited for access to Restricted or Confidential systems
- Remote sessions must time out after **15 minutes** of inactivity

### §7.2 Bring Your Own Device (BYOD)
- Personal devices may only access Company resources through the Mobile Device Management (MDM) enrolled profile
- BYOD access is limited to email, calendar, and approved collaboration tools
- BYOD devices must meet minimum OS patch level (within 30 days of vendor release) and have endpoint protection installed

---

## 8. Monitoring and Audit

### §8.1 Logging Requirements
All systems processing Restricted or Confidential data must generate audit logs capturing:
- Authentication events (success and failure)
- Access to sensitive data objects
- Privileged operations
- Configuration changes

Logs must be forwarded to the centralised SIEM within **5 minutes** of generation and retained for a minimum of **12 months**.

### §8.2 Anomaly Detection
The Security Operations Centre (SOC) monitors for access anomalies including impossible-travel logins, off-hours privileged access, and mass-download events. Alerts must be triaged within **2 hours** (business hours) or **4 hours** (out of hours).

---

## 9. Exceptions

Exceptions to this standard require:
1. Written request from the requesting manager detailing the business justification and risk acceptance
2. Written approval from the CISO
3. Documentation in the Exceptions Register with defined expiry date (maximum 90 days; renewable)

---

## Document Control

| Version | Date | Author | Changes |
|---|---|---|---|
| 4.0 | 01 Jan 2024 | CISO Office | Added §3.2 FIDO2 requirement; updated termination deadline to 4 hours |
| 3.5 | 01 Jul 2023 | CISO / IT Security | Added BYOD section; tightened PAM requirements |
| 3.0 | 01 Jan 2022 | CISO Office | Full revision aligned to ISO 27001:2022 |
