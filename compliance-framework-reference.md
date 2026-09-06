# Compliance Framework Reference

A quick reference for the compliance frameworks that come up most often in enterprise security and infrastructure programs. Written for TPMs - enough to understand what each framework requires, which industries it applies to, and what it means for how you run your program.

This isn't legal advice and it's not a substitute for your GRC team. When compliance requirements affect your program, engage GRC early. This reference helps you know what questions to ask and understand the answers you get.

*Last reviewed: May 2026. Verify current requirements with your GRC team before acting on any specific compliance claim.*

---

## How to Use This Reference

Each framework summary answers four questions:
1. What is it and who created it?
2. Who does it apply to?
3. What does it require at a high level?
4. What does it mean for a TPM running a program in this environment?

---

## SOX - Sarbanes-Oxley Act

**What it's:** US federal law enacted in 2002 following the Enron and WorldCom accounting scandals. Requires public companies to maintain accurate financial records and demonstrate adequate internal controls over financial reporting.

**Who it applies to:** US public companies (listed on US exchanges), their subsidiaries, and their significant service providers. Also applies to non-US companies listed on US exchanges.

**What it requires at a high level:**
- Section 302: Senior executives must personally certify the accuracy of financial reports
- Section 404: Management must assess and report on the effectiveness of internal controls over financial reporting. External auditors must attest to that assessment.
- IT general controls (ITGCs) are a significant component: access controls, change management, and availability controls for financial systems

**What it means for TPMs:**

SOX compliance touches any program that affects financial systems or data (ERP, financial reporting, payment systems, billing), access to financial data (identity management, privileged access), the change management process for financial systems, or audit logging for financial system activity.

If your program touches any of these areas, engage GRC at intake. SOX controls need to be designed in from the start. They include segregation of duties, access reviews, change control, and audit log retention.

The cost of a SOX finding at audit time is high. The cost of designing for SOX from the start is much lower.

---

## PCI-DSS - Payment Card Industry Data Security Standard

**What it's:** A private industry standard developed by the major card brands (Visa, Mastercard, Amex, Discover) and administered by the PCI Security Standards Council.

**Current version:** PCI DSS v4.0.1, the only active version supported by PCI SSC as of January 1, 2025. PCI DSS v4.0 was retired on December 31, 2024. The 51 future-dated requirements introduced in v4.0 became mandatory on March 31, 2025.

*Source: PCI Security Standards Council. Last verified: May 2026.*

**Who it applies to:** Any organization that processes, stores, or transmits cardholder data - the card number (PAN), expiration date, cardholder name, and service code. Scope is determined by the cardholder data environment (CDE).

**What it requires at a high level:**

PCI-DSS has 12 requirements organized into six control objectives: build and maintain a secure network, protect cardholder data, maintain a vulnerability management program, implement strong access controls, regularly monitor and test networks, and maintain an information security policy.

**Key technical requirements for TPMs to know:**
- TLS 1.2 minimum for all cardholder data transmission. TLS 1.3 preferred.
- Specific approved cipher suites - not all TLS configurations are PCI-compliant
- Annual penetration testing for internet-facing systems in the CDE
- Quarterly vulnerability scans
- Log retention: 12 months, 3 months immediately available
- Annual compliance validation: SAQ for smaller merchants, QSA audit for larger ones

**What it means for TPMs:**

PCI scope creep is a real risk. Any new system that processes, stores, or transmits cardholder data is in scope. Any program that touches a system in the CDE needs PCI review. The key question at intake: does this program add to, change, or connect to the cardholder data environment?

Annual validation cycles mean timing matters. Programs that need to deliver changes to PCI-scoped systems should understand when the annual assessment window is and sequence accordingly.

---

## HIPAA - Health Insurance Portability and Accountability Act

**What it's:** US federal law governing the privacy and security of health information. The Security Rule specifically covers electronic protected health information (ePHI).

**Who it applies to:** Covered entities (healthcare providers, health plans, healthcare clearinghouses) and their business associates (any vendor or service provider that handles ePHI on their behalf).

**What it requires at a high level:**

The HIPAA Security Rule requires administrative safeguards (policies, procedures, workforce training, risk analysis), physical safeguards (facility access controls, workstation security), and technical safeguards (access controls, audit controls, integrity controls, transmission security).

**Key technical requirements:**
- Encryption of ePHI in transit is required (addressable, but effectively required in practice)
- Encryption of ePHI at rest is addressable - must be implemented or documented why not
- Unique user identification for all users with system access
- Automatic logoff for workstations
- Audit logs of system activity involving ePHI

**What it means for TPMs:**

HIPAA doesn't specify exact technical standards the way PCI does - it uses "required" and "addressable" implementation specifications. Addressable does not mean optional. In practice, most addressable specifications get implemented.

Risk analysis is a core HIPAA requirement and a common audit finding. Business Associate Agreements (BAAs) are required with any vendor that will handle ePHI. If your program involves a vendor touching patient data, Legal and procurement need to be involved before the vendor has access to anything.

---

## FedRAMP - Federal Risk and Authorization Management Program

**What it's:** A US government program that provides a standardized approach to security assessment, authorization, and continuous monitoring for cloud services used by federal agencies. Based on NIST SP 800-53 controls.

**Who it applies to:** Cloud service providers seeking to provide cloud services to US federal agencies.

**What it requires at a high level:**

Three impact levels: Low, Moderate (most government cloud workloads), and High (law enforcement, financial, health data). Each level requires a specific set of NIST 800-53 controls - Moderate requires approximately 325 controls, High approximately 421.

Authorization paths: Agency ATO (a specific agency authorizes for their use) or JAB P-ATO (the Joint Authorization Board authorizes for government-wide use).

**What it means for TPMs:**

FedRAMP authorization is a program in itself. Timeline from engagement to authorization is typically 12-24 months. It involves gap assessment, System Security Plan documentation, assessment by an accredited Third Party Assessment Organization, and ongoing continuous monitoring obligations post-authorization. Monthly vulnerability scanning, annual penetration testing, and ongoing evidence collection aren't optional.

---

## SOC 2 - Service Organization Control 2

**What it's:** An auditing framework developed by the American Institute of CPAs (AICPA) for service organizations. Covers five Trust Service Criteria: Security (always required), Availability, Processing Integrity, Confidentiality, and Privacy.

**Type I vs Type II:**
- Type I: Point-in-time assessment. Controls are designed appropriately as of a specific date.
- Type II: Assessment over a period (typically 6-12 months). Controls are operating effectively throughout. Type II is what enterprise customers care about.

**What it means for TPMs:**

SOC 2 Type II's most important operational implication: controls must be operating and documented throughout the entire audit period, not just at the audit date. Programs that build or change controls need to be operational before the audit window begins. Evidence collection is continuous - logs, access reviews, change tickets, incident records.

If your program introduces new systems that fall within SOC 2 scope, engage GRC to understand how those systems will be incorporated into the controls framework and the evidence collection process.

---

## GDPR - General Data Protection Regulation

**What it's:** EU regulation governing the collection, processing, and storage of personal data of EU residents. Effective since May 2018.

**Who it applies to:** Any organization that processes personal data of EU residents, regardless of where the organization is located.

**What it requires at a high level:**

Lawful basis for processing, data minimization, purpose limitation, accuracy, storage limitation, appropriate security, data subject rights (access, erasure, portability, correction), Data Protection Impact Assessment for high-risk processing, and breach notification within 72 hours to the supervisory authority for qualifying breaches.

**What it means for TPMs:**

GDPR affects programs that collect new categories of personal data, change how existing personal data is processed, involve new vendors processing personal data, affect data retention or deletion processes, or build new AI or automated decision-making systems.

The 72-hour breach notification requirement is particularly relevant for incident response. Knowing the notification threshold before an incident happens is essential. DPIAs are required for high-risk processing - new technologies, large-scale processing of sensitive data, systematic monitoring. Engage your Privacy team early.

---

## NIST CSF - Cybersecurity Framework

**What it's:** A voluntary framework developed by the National Institute of Standards and Technology for improving cybersecurity risk management. Version 2.0 released in 2024.

**Who it applies to:** Anyone who wants to use it. Widely adopted across US industries and referenced by regulators. Not legally required for most organizations, but increasingly expected.

**What it covers:**

Six functions (version 2.0): Govern (establish and monitor cybersecurity risk strategy), Identify (understand assets, risks, vulnerabilities), Protect (implement safeguards), Detect (monitor for events), Respond (take action on detected incidents), and Recover (restore capabilities after incidents).

**What it means for TPMs:**

The NIST CSF is useful as a maturity model and common language for security conversations. NIST SP 800-53 (which underlies FedRAMP) and NIST SP 800-171 (for controlled unclassified information) reference the same NIST taxonomy. Fluency with the CSF makes compliance conversations easier across frameworks.

---

## Framework Comparison at a Glance

| Framework | Who Must Comply | External Audit Required? | Key TPM Trigger |
|-----------|----------------|------------------------|----------------|
| SOX | US public companies | Yes (annual) | Any program touching financial systems |
| PCI-DSS v4.0.1 | Card data handlers | Yes (annual for large orgs) | Any program touching payment systems |
| HIPAA | Healthcare entities and BAs | No (but OCR audits exist) | Any program touching ePHI |
| FedRAMP | Cloud providers to US gov | Yes (3PAO) | Selling cloud services to federal agencies |
| SOC 2 | Service organizations | Yes (CPA firm) | Enterprise customer requirements |
| GDPR | EU data processors globally | No (but DPA investigations) | Any program touching EU personal data |
| NIST CSF | Voluntary | No | Maturity assessments and risk conversations |

---

*Version 1.1. Last reviewed May 2026. Frameworks evolve - verify current requirements with your GRC team before relying on any specific compliance claim.*
