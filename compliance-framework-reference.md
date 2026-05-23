# Compliance Framework Reference

A quick reference for the compliance frameworks that come up most often in enterprise security and infrastructure programs. Written for TPMs - enough to understand what each framework requires, which industries it applies to, and what it means for how you run your program.

This is not legal advice and it is not a substitute for your GRC team. When compliance requirements affect your program, engage GRC early. This reference helps you know what questions to ask and understand the answers you get.

---

## How to Use This Reference

Each framework summary answers four questions:
1. What is it and who created it?
2. Who does it apply to?
3. What does it require at a high level?
4. What does it mean for a TPM running a program in this environment?

---

## SOX - Sarbanes-Oxley Act

**What it is:** US federal law enacted in 2002 following the Enron and WorldCom accounting scandals. Requires public companies to maintain accurate financial records and demonstrate adequate internal controls over financial reporting.

**Who it applies to:** US public companies (listed on US exchanges), their subsidiaries, and their significant service providers. Also applies to non-US companies listed on US exchanges.

**What it requires at a high level:**
- Section 302: Senior executives must personally certify the accuracy of financial reports
- Section 404: Management must assess and report on the effectiveness of internal controls over financial reporting. External auditors must attest to that assessment.
- IT general controls (ITGCs) are a significant component: access controls, change management, and availability controls for financial systems

**What it means for TPMs:**

SOX compliance touches any program that affects:
- Financial systems or data (ERP, financial reporting, payment systems, billing)
- Access to financial data (identity management, privileged access)
- The change management process for financial systems
- Audit logging for financial system activity

If your program touches any of these areas, engage GRC at intake. SOX controls need to be designed in from the start. They include:
- Segregation of duties: no single person should be able to initiate and approve a financial transaction
- Access reviews: periodic review of who has access to financial systems
- Change control: financial system changes require documented approval before deployment
- Audit logs: financial system activity must be logged and retained

The cost of a SOX finding at audit time is high. The cost of designing for SOX from the start is much lower.

---

## PCI-DSS - Payment Card Industry Data Security Standard

**What it is:** A private industry standard developed by the major card brands (Visa, Mastercard, Amex, Discover) and administered by the PCI Security Standards Council. Currently at version 4.0.

**Who it applies to:** Any organization that processes, stores, or transmits cardholder data - the card number (PAN), expiration date, cardholder name, and service code. Scope is determined by the cardholder data environment (CDE), which includes all systems that touch or could affect the security of cardholder data.

**What it requires at a high level:**

PCI-DSS has 12 requirements organized into six control objectives:

| Objective | Requirements |
|-----------|-------------|
| Build and maintain a secure network | Firewalls, no vendor defaults |
| Protect cardholder data | Encryption at rest and in transit |
| Maintain a vulnerability management program | Antivirus, secure systems |
| Implement strong access controls | Need-to-know, unique IDs, physical access |
| Regularly monitor and test networks | Logging, monitoring, penetration testing |
| Maintain an information security policy | Policy, risk assessment, awareness |

**Key technical requirements for TPMs to know:**
- TLS 1.2 minimum for all cardholder data transmission. TLS 1.3 preferred.
- Specific approved cipher suites - not all TLS configurations are PCI-compliant
- Annual penetration testing for internet-facing systems in the CDE
- Quarterly vulnerability scans
- Log retention: 12 months, 3 months immediately available
- Annual compliance validation: SAQ (Self-Assessment Questionnaire) for smaller merchants, QSA (Qualified Security Assessor) audit for larger ones

**What it means for TPMs:**

PCI scope creep is a real risk. Any new system that processes, stores, or transmits cardholder data is in scope. Any program that touches a system in the CDE needs PCI review. The key question at intake: does this program add to, change, or connect to the cardholder data environment?

Annual validation cycles mean timing matters. Programs that need to deliver changes to PCI-scoped systems should understand when the annual assessment window is and sequence accordingly.

---

## HIPAA - Health Insurance Portability and Accountability Act

**What it is:** US federal law governing the privacy and security of health information. The Security Rule specifically covers electronic protected health information (ePHI).

**Who it applies to:** Covered entities (healthcare providers, health plans, healthcare clearinghouses) and their business associates (any vendor or service provider that handles ePHI on their behalf).

**What it requires at a high level:**

The HIPAA Security Rule requires covered entities and business associates to implement:
- Administrative safeguards: policies, procedures, workforce training, risk analysis
- Physical safeguards: facility access controls, workstation security
- Technical safeguards: access controls, audit controls, integrity controls, transmission security

**Key technical requirements:**
- Encryption of ePHI in transit is required (addressable, but effectively required in practice)
- Encryption of ePHI at rest is addressable - must be implemented or documented why not
- Unique user identification for all users with system access
- Automatic logoff for workstations
- Audit logs of system activity involving ePHI
- Integrity controls to verify ePHI has not been improperly altered

**What it means for TPMs:**

HIPAA does not specify exact technical standards the way PCI does - it uses "required" and "addressable" implementation specifications. Addressable does not mean optional; it means you must implement it or document a risk-based decision for why you are not. In practice, most addressable specifications get implemented.

Risk analysis is a core HIPAA requirement and a common audit finding. If your program affects ePHI systems, a risk analysis is required. Engage your Privacy and GRC teams early.

Business Associate Agreements (BAAs) are required with any vendor that will handle ePHI. If your program involves a vendor touching patient data, Legal and procurement need to be involved before the vendor has access to anything.

---

## FedRAMP - Federal Risk and Authorization Management Program

**What it is:** A US government program that provides a standardized approach to security assessment, authorization, and continuous monitoring for cloud services used by federal agencies. Based on NIST SP 800-53 controls.

**Who it applies to:** Cloud service providers (CSPs) seeking to provide cloud services to US federal agencies. If a company wants to sell cloud services to the US government, FedRAMP authorization is typically required.

**What it requires at a high level:**

FedRAMP has three impact levels based on the sensitivity of the data:
- **Low:** Non-sensitive public data
- **Moderate:** Controlled unclassified information (most government cloud workloads fall here)
- **High:** Highly sensitive data (law enforcement, financial, health)

Each level requires implementation of a specific set of NIST 800-53 controls. Moderate requires ~325 controls. High requires ~421 controls.

Authorization paths:
- **Agency ATO:** A specific federal agency authorizes the service for their use
- **JAB P-ATO:** The Joint Authorization Board (DoD, DHS, GSA) authorizes the service for government-wide use

**What it means for TPMs:**

FedRAMP authorization is a program in itself. The timeline from engagement to authorization is typically 12-24 months. It involves:
- Gap assessment against required controls
- System Security Plan (SSP) documentation
- Assessment by an accredited Third Party Assessment Organization (3PAO)
- Authorization process with the authorizing agency or JAB
- Continuous monitoring obligations post-authorization

If your company is pursuing FedRAMP, or maintaining an existing authorization, the compliance work is significant and ongoing. Continuous monitoring is not optional - it includes monthly vulnerability scanning, annual penetration testing, and ongoing evidence collection.

---

## SOC 2 - Service Organization Control 2

**What it is:** An auditing framework developed by the American Institute of CPAs (AICPA) for service organizations - cloud providers, SaaS companies, managed service providers. Covers five Trust Service Criteria: Security, Availability, Processing Integrity, Confidentiality, and Privacy. Security is always required; the others are optional.

**Who it applies to:** Service organizations that store, process, or transmit customer data and need to demonstrate controls to enterprise customers. SOC 2 is not legally required in most cases, but enterprise customers increasingly require it as a vendor qualification.

**The difference between Type I and Type II:**
- **Type I:** Point-in-time assessment. Confirms that controls are designed appropriately as of a specific date.
- **Type II:** Assessment over a period of time (typically 6-12 months). Confirms that controls are operating effectively throughout the period.

Type II is what enterprise customers care about. Type I is a stepping stone.

**What it requires at a high level:**

The Security criterion (always required) covers:
- Access controls and logical access restrictions
- System operations monitoring
- Change management
- Risk mitigation
- Vendor management
- Incident response

**What it means for TPMs:**

SOC 2 Type II's most important operational implication: controls must be operating and documented throughout the entire audit period, not just at the audit date. This means:
- Programs that build or change controls need to be operational before the audit window begins
- Evidence collection is continuous - logs, access reviews, change tickets, incident records
- A control that existed but was not documented does not satisfy the auditor

If your program introduces new systems or processes that fall within the SOC 2 scope, engage GRC to understand how those systems will be incorporated into the controls framework and the evidence collection process.

---

## GDPR - General Data Protection Regulation

**What it is:** EU regulation governing the collection, processing, and storage of personal data of EU residents. Effective since May 2018.

**Who it applies to:** Any organization that processes personal data of EU residents, regardless of where the organization is located. If a US company has EU customers or employees, GDPR applies.

**What it requires at a high level:**
- Lawful basis for processing personal data (consent, contract, legitimate interest, etc.)
- Data minimization - collect only what is necessary
- Purpose limitation - use data only for the purpose it was collected
- Accuracy - keep personal data accurate and current
- Storage limitation - keep data only as long as necessary
- Security - appropriate technical and organizational measures
- Data subject rights - right to access, erasure, portability, correction
- Data Protection Impact Assessment (DPIA) for high-risk processing
- Breach notification - 72 hours to the supervisory authority for qualifying breaches

**What it means for TPMs:**

GDPR affects programs that:
- Collect new categories of personal data
- Change how existing personal data is processed
- Involve new vendors processing personal data
- Affect data retention or deletion processes
- Build new AI or automated decision-making systems

The breach notification requirement is particularly relevant for incident response: 72 hours is not a lot of time. Knowing the notification threshold before an incident happens is essential.

Data Protection Impact Assessments (DPIAs) are required for high-risk processing - new technologies, large-scale processing of sensitive data, systematic monitoring. If your program involves any of these, a DPIA may be required. Engage your Privacy team early.

---

## NIST CSF - Cybersecurity Framework

**What it is:** A voluntary framework developed by the National Institute of Standards and Technology (NIST) for improving cybersecurity risk management. Originally developed for critical infrastructure but widely adopted across industries. Version 2.0 released in 2024.

**Who it applies to:** Anyone who wants to use it. Widely adopted in US industries and referenced by regulators. Not legally required for most organizations, but increasingly expected.

**What it covers:**

The CSF is organized around six functions (version 2.0):
- **Govern:** Establish and monitor cybersecurity risk strategy
- **Identify:** Understand assets, risks, and vulnerabilities
- **Protect:** Implement safeguards
- **Detect:** Monitor for events
- **Respond:** Take action on detected incidents
- **Recover:** Restore capabilities after incidents

**What it means for TPMs:**

The NIST CSF is useful as a maturity model and a common language for security conversations. When an executive asks "how mature is our security posture," the CSF gives you a framework for the answer.

More practically: NIST SP 800-53 (the control catalog that underlies FedRAMP) and NIST SP 800-171 (for controlled unclassified information) reference the same NIST taxonomy. Fluency with the CSF makes compliance conversations easier across frameworks.

---

## Framework Comparison at a Glance

| Framework | Who Must Comply | External Audit Required? | Timeline to Compliance | Key TPM Trigger |
|-----------|----------------|------------------------|----------------------|----------------|
| SOX | US public companies | Yes (annual) | Ongoing | Any program touching financial systems |
| PCI-DSS | Card data handlers | Yes (annual for large orgs) | 6-12 months | Any program touching payment systems |
| HIPAA | Healthcare entities and BAs | No (but OCR audits exist) | Ongoing | Any program touching ePHI |
| FedRAMP | Cloud providers to US gov | Yes (3PAO) | 12-24 months | Selling cloud services to federal agencies |
| SOC 2 | Service organizations | Yes (CPA firm) | 6-18 months | Enterprise customer requirements |
| GDPR | EU data processors globally | No (but DPA investigations) | Ongoing | Any program touching EU personal data |
| NIST CSF | Voluntary | No | N/A | Maturity assessments and risk conversations |

---

*Version 1.0. Frameworks evolve - verify current requirements with your GRC team. Last reviewed May 2026.*
