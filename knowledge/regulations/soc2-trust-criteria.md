# SOC 2 Trust Services Criteria Summary

**Framework:** SOC 2 (System and Organization Controls 2)  
**Issued by:** American Institute of Certified Public Accountants (AICPA)  
**Standard:** TSP Section 100, 2017 Trust Services Criteria  
**Applicable to:** Service organisations that store, process, or transmit customer data  
**Audit types:** SOC 2 Type I (point-in-time design) | SOC 2 Type II (operational effectiveness over a period)

---

## Common Criteria (CC) — Security Trust Service Category

All SOC 2 audits must include the Security (Common Criteria) category. The other four categories (Availability, Processing Integrity, Confidentiality, Privacy) are optional.

---

### CC1 — Control Environment

**CC1.1:** The entity demonstrates a commitment to integrity and ethical values.  
**CC1.2:** The board of directors demonstrates independence from management and exercises oversight of the development and performance of internal control.  
**CC1.3:** Management establishes structure, reporting lines, and appropriate authorities and responsibilities in the pursuit of objectives.  
**CC1.4:** The entity demonstrates a commitment to attract, develop, and retain competent individuals in alignment with objectives.  
**CC1.5:** The entity holds individuals accountable for their internal control responsibilities.

---

### CC2 — Communication and Information

**CC2.1:** The entity obtains or generates and uses relevant, quality information to support the functioning of internal control.  
**CC2.2:** The entity internally communicates information, including objectives and responsibilities for internal control.  
**CC2.3:** The entity communicates with external parties regarding matters affecting the functioning of internal control.

---

### CC3 — Risk Assessment

**CC3.1:** The entity specifies objectives with sufficient clarity to enable the identification and assessment of risks relating to objectives.  
**CC3.2:** The entity identifies risks to the achievement of its objectives across the entity.  
**CC3.3:** The entity considers the potential for fraud in assessing risks to the achievement of objectives.  
**CC3.4:** The entity identifies and assesses changes that could significantly impact the system of internal control.

---

### CC4 — Monitoring Activities

**CC4.1:** The entity selects, develops, and performs ongoing and/or separate evaluations to ascertain whether the components of internal control are present and functioning.  
**CC4.2:** The entity evaluates and communicates internal control deficiencies in a timely manner to those parties responsible for taking corrective action.

---

### CC5 — Control Activities

**CC5.1:** The entity selects and develops control activities that contribute to the mitigation of risks to the achievement of objectives.  
**CC5.2:** The entity also selects and develops general control activities over technology to support the achievement of objectives.  
**CC5.3:** The entity deploys control activities through policies that establish what is expected and procedures that put policies into action.

---

### CC6 — Logical and Physical Access Controls

**CC6.1:** The entity implements logical access security software, infrastructure, and architectures over protected information assets to protect them from security events to meet the entity's objectives.  
Requirements include:
- Identification and authentication of users
- Role-based access controls
- Access restriction to authorised users only

**CC6.2:** Prior to issuing system credentials and granting system access, the entity registers and authorises new internal and external users.  
**CC6.3:** The entity authorises, modifies, or removes access to data, software, functions, and other protected information assets based on approved and documented requests.  
**CC6.4:** The entity restricts physical access to facilities and protected information assets.  
**CC6.5:** The entity discontinues logical and physical protections over physical assets only after the ability to read or recover data and software from those assets has been diminished and is no longer required.  
**CC6.6:** The entity implements logical access security measures to protect against threats from sources outside its system boundaries.  
**CC6.7:** The entity restricts the transmission, movement, and removal of information to authorised internal and external users and processes.  
**CC6.8:** The entity implements controls to prevent or detect and act upon the introduction of unauthorised or malicious software.

---

### CC7 — System Operations

**CC7.1:** To meet its objectives, the entity uses detection and monitoring procedures to identify changes to configurations or new vulnerabilities, and uses threat intelligence to identify emerging threats.  
**CC7.2:** The entity monitors system components and the operation of those components for anomalies that are indicative of malicious acts, natural disasters, and errors affecting the entity's ability to meet its objectives.  
**CC7.3:** The entity evaluates security events to determine whether they could or have resulted in a failure of the entity to meet its objectives (security incidents) and, if so, takes actions to prevent or address such failures.  
**CC7.4:** The entity responds to identified security incidents by executing a defined incident response program to understand, contain, remediate, and communicate security incidents.

Specifically, **CC7.4** requires:
- Assigning roles and responsibilities for incident response
- Containing security incidents
- Eradicating the effects of the incident
- **Notifying affected parties of the incident and remediation actions**  
- **Reporting security incidents to appropriate parties in a timely manner** — the criteria does not specify a universal deadline; this must be defined in the entity's own incident response policy

**CC7.5:** The entity identifies, develops, and implements activities to recover from identified security incidents.

---

### CC8 — Change Management

**CC8.1:** The entity authorises, designs, develops or acquires, configures, documents, tests, approves, and implements changes to infrastructure, data, software, and procedures to meet its change management objectives.

---

### CC9 — Risk Mitigation

**CC9.1:** The entity identifies, selects, and develops risk mitigation activities for risks arising from potential business disruptions.  
**CC9.2:** The entity assesses and manages risks associated with vendors and business partners.

---

## Additional Trust Service Categories

### A-Series — Availability
**A1.1:** The entity maintains, monitors, and evaluates current processing capacity and use of system components to manage capacity demand and to enable the implementation of additional capacity as needed.  
**A1.2:** The entity authorises, designs, develops or acquires, implements, operates, approves, maintains, and monitors environmental protections, software, data back-up processes, and recovery infrastructure.  
**A1.3:** The entity tests recovery plan procedures supporting system recovery to meet its objectives.

### C-Series — Confidentiality
**C1.1:** The entity identifies and maintains confidential information to meet the entity's objectives related to confidentiality.  
**C1.2:** The entity disposes of confidential information to meet the entity's objectives related to confidentiality.

### P-Series — Privacy
**P1.0 through P8.0** cover notice, choice and consent, collection, use/retention/disposal, access, disclosure, quality, and monitoring and enforcement.

---

## Key SOC 2 Requirements for Incident/Breach Notification

| Criterion | Requirement |
|---|---|
| CC7.3 | Evaluate security events to determine if they are security incidents |
| CC7.4 | Execute defined incident response program; **notify affected parties** |
| CC7.4 | **Report security incidents to appropriate parties in a timely manner** |
| P6.7 (Privacy) | Notify affected data subjects and authorities as required by applicable law |

**Note:** SOC 2 does not prescribe a specific breach notification deadline — it requires the entity to define one in its incident response policy and then adhere to that policy. Compliance with GDPR (72 hours), HIPAA (60 days), or other applicable regulations is expected to be reflected in the entity's own documented policy.
