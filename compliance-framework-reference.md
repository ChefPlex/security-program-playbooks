# Compliance Framework Reference

A quick reference for the compliance frameworks that come up most often in enterprise security and infrastructure programs. Written for TPMs - enough to understand what each framework requires, which industries it applies to, and what it means for how you run your program.

This isn't legal advice and it's not a substitute for your GRC team. When compliance requirements affect your program, engage GRC early. This reference helps you know what questions to ask and understand the answers you get.

*Last reviewed: September 2026. Dates and figures added or changed in that review were checked against the linked sources. Verify current requirements with your GRC team before acting on any specific compliance claim.*

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
- Section 404(a): Management must assess and report on the effectiveness of internal controls over financial reporting.
- Section 404(b): The external auditor must attest to that assessment. This applies to accelerated and large accelerated filers; non-accelerated filers are exempt. Confirm the company's filer status with Finance before scoping audit work.
- IT general controls (ITGCs) are a significant component: access controls, change management, and availability controls for financial systems

**What it means for TPMs:**

SOX compliance touches any program that affects financial systems or data (ERP, financial reporting, payment systems, billing), access to financial data (identity management, privileged access), the change management process for financial systems, or audit logging for financial system activity.

If your program touches any of these areas, engage GRC at intake. SOX controls need to be designed in from the start. They include segregation of duties, access reviews, change control, and audit log retention.

The cost of a SOX finding at audit time is high. The cost of designing for SOX from the start is much lower.

---

## PCI-DSS - Payment Card Industry Data Security Standard

**What it is:** A private industry standard developed by the major card brands (Visa, Mastercard, Amex, Discover) and administered by the PCI Security Standards Council.

**Current version:** PCI DSS v4.0.1, the only active version supported by PCI SSC as of January 1, 2025. PCI DSS v4.0 was retired on December 31, 2024. The 51 future-dated requirements introduced in v4.0 became mandatory on March 31, 2025.

*Source: PCI Security Standards Council. Last verified: September 2026.*

**Who it applies to:** Any organization that processes, stores, or transmits cardholder data - the card number (PAN), expiration date, cardholder name, and service code. Scope is determined by the cardholder data environment (CDE).

**What it requires at a high level:**

PCI-DSS has 12 requirements organized into six control objectives: build and maintain a secure network, protect cardholder data, maintain a vulnerability management program, implement strong access controls, regularly monitor and test networks, and maintain an information security policy.

**Key technical requirements for TPMs to know:**
- "Strong cryptography" for cardholder data sent over open, public networks (Requirement 4). The standard does not name a TLS version. QSAs commonly read strong cryptography as TLS 1.2 or higher with strong cipher suites, so plan to that and confirm with your assessor.
- Annual penetration testing for internet-facing systems in the CDE
- Quarterly vulnerability scans
- Log retention: 12 months, 3 months immediately available
- Annual compliance validation: SAQ for smaller merchants, QSA audit for larger ones

**What it means for TPMs:**

PCI scope creep is a real risk. Any new system that processes, stores, or transmits cardholder data is in scope. Any program that touches a system in the CDE needs PCI review. The key question at intake: does this program add to, change, or connect to the cardholder data environment?

Annual validation cycles mean timing matters. Programs that need to deliver changes to PCI-scoped systems should understand when the annual assessment window is and sequence accordingly.

---

## HIPAA - Health Insurance Portability and Accountability Act

**What it is:** US federal law governing the privacy and security of health information. The Security Rule specifically covers electronic protected health information (ePHI).

**Who it applies to:** Covered entities (healthcare providers, health plans, healthcare clearinghouses) and their business associates (any vendor or service provider that handles ePHI on their behalf).

**What it requires at a high level:**

The HIPAA Security Rule requires administrative safeguards (policies, procedures, workforce training, risk analysis), physical safeguards (facility access controls, workstation security), and technical safeguards (access controls, audit controls, integrity controls, transmission security).

**Key technical requirements:**
- Encryption of ePHI in transit is addressable under [45 CFR 164.312(e)](https://www.law.cornell.edu/cfr/text/45/164.312). Most organizations implement it, and an unencrypted transmission path is hard to defend in a risk analysis.
- Encryption of ePHI at rest is addressable - must be implemented or documented why not
- The [January 2025 Security Rule NPRM](https://www.federalregister.gov/documents/2025/01/06/2024-30983/hipaa-security-rule-to-strengthen-the-cybersecurity-of-electronic-protected-health-information) proposes removing the required/addressable distinction, which would make encryption required with limited exceptions. It is a proposed rule, not final, as of September 2026.
- Unique user identification for all users with system access
- Automatic logoff for workstations
- Audit logs of system activity involving ePHI

**What it means for TPMs:**

HIPAA doesn't specify exact technical standards the way PCI does - it uses "required" and "addressable" implementation specifications. Addressable does not mean optional. In practice, most addressable specifications get implemented.

Risk analysis is a core HIPAA requirement and a common audit finding. Business Associate Agreements (BAAs) are required with any vendor that will handle ePHI. If your program involves a vendor touching patient data, Legal and procurement need to be involved before the vendor has access to anything.

---

## FedRAMP - Federal Risk and Authorization Management Program

**What it is:** A US government program that provides a standardized approach to security assessment, authorization, and continuous monitoring for cloud services used by federal agencies. Based on NIST SP 800-53 controls.

**Who it applies to:** Cloud service providers seeking to provide cloud services to US federal agencies.

**What it requires at a high level:**

Three impact levels: Low, Moderate (most government cloud workloads), and High (law enforcement, financial, health data). The Rev 5 baselines, aligned to NIST SP 800-53 Rev 5, require 156 controls at Low, 323 at Moderate, and 410 at High ([FedRAMP Rev. 5 Transition Overview](https://www.fedramp.gov/resources/documents/Rev-5-Transition-Overview-Presentation.pdf)).

**The program is changing, so check the current rules before planning:**
- The Joint Authorization Board and its JAB P-ATO are gone. Under OMB memo M-24-15, existing JAB P-ATOs were re-designated and a [FedRAMP Board](https://www.fedramp.gov/2026/authority/m-24-15/process/) sets requirements and guidelines; it does not approve individual packages. There is one "FedRAMP Authorized" outcome instead of two tiers.
- **FedRAMP 20x** is the newer certification path, and it moves to new rules first.
- The [Consolidated Rules for 2026 (CR26)](https://www.fedramp.gov/2026/providers/updating/deadlines/) took effect July 4, 2026 and apply to 20x immediately. They become mandatory for Rev 5 on January 1, 2027. FedRAMP stops accepting new Rev 5 applications after June 11, 2027. CR26 is valid through December 31, 2028.

**What it means for TPMs:**

FedRAMP authorization is a program in itself. It involves gap assessment, security documentation, assessment by an accredited Third Party Assessment Organization, and ongoing continuous monitoring obligations post-authorization. Monthly vulnerability scanning, annual penetration testing, and ongoing evidence collection aren't optional. With the path and the rule set both in transition, confirm with GRC which path and which rule version your offering is on before you build the schedule.

---

## SOC 2 - Service Organization Control 2

**What it is:** An auditing framework developed by the American Institute of CPAs (AICPA) for service organizations. Covers five Trust Service Criteria: Security (always required), Availability, Processing Integrity, Confidentiality, and Privacy.

**Type I vs Type II:**
- Type I: Point-in-time assessment. Controls are designed appropriately as of a specific date.
- Type II: Assessment over a period (typically 6-12 months). Controls are operating effectively throughout. Type II is what enterprise customers care about.

**What it means for TPMs:**

SOC 2 Type II's most important operational implication: controls must be operating and documented throughout the entire audit period, not just at the audit date. Programs that build or change controls need to be operational before the audit window begins. Evidence collection is continuous - logs, access reviews, change tickets, incident records.

If your program introduces new systems that fall within SOC 2 scope, engage GRC to understand how those systems will be incorporated into the controls framework and the evidence collection process.

---

## GDPR - General Data Protection Regulation

**What it is:** EU regulation governing the collection, processing, and storage of personal data of EU residents. Effective since May 2018.

**Who it applies to:** Any organization that processes personal data of EU residents, regardless of where the organization is located.

**What it requires at a high level:**

Lawful basis for processing, data minimization, purpose limitation, accuracy, storage limitation, appropriate security, data subject rights (access, erasure, portability, correction), Data Protection Impact Assessment for high-risk processing, and breach notification to the supervisory authority within 72 hours of becoming aware of a qualifying breach (Article 33).

**What it means for TPMs:**

GDPR affects programs that collect new categories of personal data, change how existing personal data is processed, involve new vendors processing personal data, affect data retention or deletion processes, or build new AI or automated decision-making systems.

The 72-hour breach notification requirement is particularly relevant for incident response, because the clock starts when the controller becomes aware, not when the investigation confirms everything. Knowing the notification threshold before an incident happens is essential. DPIAs are required for high-risk processing - new technologies, large-scale processing of sensitive data, systematic monitoring. Engage your Privacy team early.

---

## NIST CSF - Cybersecurity Framework

**What it is:** A voluntary framework developed by the National Institute of Standards and Technology for improving cybersecurity risk management. Version 2.0 released in 2024.

**Who it applies to:** Anyone who wants to use it. Widely adopted across US industries and referenced by regulators. Not legally required for most organizations, but increasingly expected.

**What it covers:**

Six functions (version 2.0): Govern (establish and monitor cybersecurity risk strategy), Identify (understand assets, risks, vulnerabilities), Protect (implement safeguards), Detect (monitor for events), Respond (take action on detected incidents), and Recover (restore capabilities after incidents).

**What it means for TPMs:**

The NIST CSF is useful as a maturity model and common language for security conversations. NIST SP 800-53 (which underlies FedRAMP) and NIST SP 800-171 (for controlled unclassified information) reference the same NIST taxonomy. Fluency with the CSF makes compliance conversations easier across frameworks.

---

## Shorter Entries

These come up often enough that a TPM should recognize them and know when to call GRC or Legal.

**ISO/IEC 27001:2022** - The international standard for an information security management system (ISMS), certified by accredited certification bodies. The 2022 edition is the current one; certificates to the 2013 edition had to transition under [IAF MD 26](https://iaf.nu/iaf_system/uploads/documents/IAF_MD26_Issue_2_15012023.pdf). TPM trigger: customers who ask for ISO certification rather than SOC 2. Map its controls to the ones you already evidence for other frameworks rather than building a second set.

**SEC Form 8-K Item 1.05** - US public companies must disclose a cybersecurity incident they determine to be material, generally within four business days of that determination ([SEC press release 2023-139](https://www.sec.gov/newsroom/press-releases/2023-139)). TPM trigger: the incident process needs a defined, documented materiality determination step, because that step starts the clock.

**NIS2 Directive (EU 2022/2555)** - EU cybersecurity law for essential and important entities across many sectors, with a national transposition deadline of October 17, 2024. Significant incidents need an early warning within 24 hours of becoming aware, an incident notification within 72 hours, and a final report within one month ([Commission NIS2 FAQ](https://digital-strategy.ec.europa.eu/en/faqs/directive-measures-high-common-level-cybersecurity-across-union-nis2-directive-faqs)). TPM trigger: any EU operation in a covered sector. National laws differ in detail, so check the member state.

**DORA (EU Digital Operational Resilience Act)** - Applies to EU financial entities from January 17, 2025 ([ESMA](https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/digital-operational-resilience-act-dora)). Covers ICT risk management, third-party ICT risk, and incident reporting, among other areas. Major ICT incidents need an initial notification within 4 hours of classification and no later than 24 hours after detection, an intermediate report within 72 hours, and a final report within one month ([EBA](https://www.eba.europa.eu/activities/single-rulebook/regulatory-activities/operational-resilience/joint-technical-standards-major-incident-reporting)). TPM trigger: any program at an EU financial entity, or selling ICT services to one. Ask GRC what flows down to you.

**EU Cyber Resilience Act (CRA)** - Security requirements for products with digital elements sold in the EU. Reporting obligations apply from September 11, 2026: manufacturers report actively exploited vulnerabilities and severe incidents with an early warning within 24 hours, a notification within 72 hours, and a final report (14 days after a fix is available for a vulnerability; one month for a severe incident). The main obligations apply from December 11, 2027 ([Commission CRA](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act), [CRA reporting](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting)). TPM trigger: any software or connected product sold in the EU. The vulnerability intake and disclosure process is now a regulated process.

**EU AI Act** - Risk-tiered rules for AI systems, in force since August 1, 2024. Prohibitions and AI literacy apply from February 2, 2025, and general-purpose AI model obligations from August 2, 2025. The 2026 Digital Omnibus on AI, in force July 27, 2026, moved the high-risk dates: December 2, 2027 for stand-alone (Annex III) high-risk systems and August 2, 2028 for AI embedded in regulated products (Annex I). Article 50 transparency obligations apply from August 2, 2026 ([AI Act Service Desk timeline](https://ai-act-service-desk.ec.europa.eu/en/ai-act/timeline/timeline-implementation-eu-ai-act), [AI Omnibus](https://digital-strategy.ec.europa.eu/en/news/ai-omnibus-enters-force)). TPM trigger: any program that builds, buys, or embeds AI for EU users. Classify the use case at intake, because the tier decides the work.

---

## Framework Comparison at a Glance

| Framework | Who Must Comply | External Audit Required? | Key TPM Trigger |
|:---|:---|:---|:---|
| SOX | US public companies | Yes (annual; 404(b) attestation for accelerated filers) | Any program touching financial systems |
| PCI-DSS v4.0.1 | Card data handlers | Yes (annual for large orgs) | Any program touching payment systems |
| HIPAA | Healthcare entities and BAs | No (but OCR audits exist) | Any program touching ePHI |
| FedRAMP | Cloud providers to US gov | Yes (3PAO) | Selling cloud services to federal agencies |
| SOC 2 | Service organizations | Yes (CPA firm) | Enterprise customer requirements |
| GDPR | EU data processors globally | No (but DPA investigations) | Any program touching EU personal data |
| NIST CSF | Voluntary | No | Maturity assessments and risk conversations |
| ISO/IEC 27001:2022 | Voluntary, often contractual | Yes (certification body) | Customers asking for ISO certification |
| NIS2 / DORA | EU essential and important entities / EU financial entities | Supervisory oversight | EU operations in covered sectors; incident reporting clocks |
| EU CRA | Makers of digital products sold in the EU | Varies by product; confirm with GRC | Any software or connected product sold in the EU |
| EU AI Act | Providers and deployers of AI in the EU | Depends on risk tier | Any program that builds, buys, or embeds AI |

---

*Version 1.2. Last reviewed September 2026. Frameworks evolve - verify current requirements with your GRC team before relying on any specific compliance claim.*
