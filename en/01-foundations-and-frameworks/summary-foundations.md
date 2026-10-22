# Module 1: Foundations and Security Governance

This document formalizes the fundamental concepts acquired during the first course of the Google Cybersecurity Professional Certificate, focusing on core defensive principles, governance frameworks, and Security Operations Center (SOC) tooling.

---

## Table of Contents
1. [Section 1: The CIA Triad in Practice](#section-1-the-cia-triad-in-practice)
2. [Section 2: Security Frameworks and Control Typology](#section-2-security-frameworks-and-control-typology)
3. [Section 3: SOC Operational Architecture and Detection](#section-3-soc-operational-architecture-and-detection)

---

## Section 1: The CIA Triad in Practice

The CIA Triad serves as the foundational model guiding security policy formulation, defensive architecture design, and operational incident triage.

### 1. Operational Definitions

* **Confidentiality:** Ensures that data, credentials, and systems are accessible only to authorized identities and entities.
  * *Associated Controls:* Encryption (at rest and in transit), Access Control Lists (ACLs), Multi-Factor Authentication (MFA), and the Principle of Least Privilege.
* **Integrity:** Guarantees that data, records, and configurations remain accurate, authentic, and protected against unauthorized modification or deletion.
  * *Associated Controls:* Cryptographic hashing (e.g., SHA-256), digital signatures, version control, and File Integrity Monitoring (FIM).
* **Availability:** Ensures that systems, networks, and applications remain reliably accessible to authorized users when required.
  * *Associated Controls:* Hardware redundancy, load balancers, tested data backups, and Disaster Recovery Plans (DRP).

### 2. Isolated Impact Scenarios

Accurate triage requires an analyst to determine which pillar is specifically targeted during an event:

* **Isolated Confidentiality Impact:**
  * *Scenario:* Passive network eavesdropping or the silent exfiltration of a credential database. The systems remain fully functional (Availability intact), and the original records are unaltered (Integrity intact).
* **Isolated Integrity Impact:**
  * *Scenario:* Unauthorized modification of a vendor payout routing number in an accounts payable database. The portal remains accessible and no private data is published externally, but the operational data is corrupted.
* **Isolated Availability Impact:**
  * *Scenario:* A Distributed Denial of Service (DDoS) attack flooding a public web portal with malicious HTTP requests. No data is exposed or corrupted, but the service becomes unreachable to legitimate users.

### 3. Managing Operational Friction

Excessive hardening of confidentiality safeguards (such as aggressive re-authentication requirements or rigid geographic blocks) can unintentionally degrade operational availability and user workflow. Security teams must balance these requirements according to the organization's threat model.

---

## Section 2: Security Frameworks and Control Typology

Security frameworks provide the structural roadmap and high-level objectives, whereas security controls represent the specific technical, administrative, and physical safeguards implemented to fulfill those requirements.

### 1. Alignment with the NIST Cybersecurity Framework (CSF)

The NIST Cybersecurity Framework structures risk management across five primary functions:

| NIST Function | Operational Objective | Implementation Example |
| :--- | :--- | :--- |
| **Identify** | Map critical digital assets, data inventories, and potential risks. | Asset inventory tracking, vulnerability scanning. |
| **Protect** | Implement safeguards to minimize the impact of potential security events. | Firewalls, Multi-Factor Authentication (MFA), access policies. |
| **Detect** | Identify anomalies and security events promptly. | SIEM log ingestion, IDS/IPS sensors, EDR alerts. |
| **Respond** | Execute actions to contain and neutralize a confirmed security incident. | Host network isolation, active session revocation. |
| **Recover** | Restore capabilities and impaired infrastructure back to normal operations. | System restoration from clean backups, security patch deployment. |

### 2. Security Control Classification Matrix

Security controls are categorized by implementation mechanism and functional role:

* **By Implementation Method:**
  * **Technical (Logical):** Hardware and software safeguards, such as network firewalls, encryption protocols, and access control lists.
  * **Operational (Administrative):** Organizational policies, acceptable use standards, security awareness training, and incident escalation procedures.
  * **Physical:** Tangible barriers protecting facilities, including RFID badge readers, CCTV surveillance, and server room access controls.

* **By Functional Purpose:**
  * **Preventative:** Designed to block an incident before it occurs (e.g., firewall ingress filtering, least-privilege enforcement).
  * **Detective:** Designed to identify and alert on suspicious activity as it occurs (e.g., SIEM correlation rules on failed authentications, audit logs).
  * **Corrective:** Designed to remediate damage and return the system to an operational baseline (e.g., reimaging compromised endpoints, database restoration).

---

## Section 3: SOC Operational Architecture and Detection

Within a Security Operations Center (SOC), detection depends on a coordinated workflow linking telemetry collection, human analysis, and automation.

### 1. SIEM vs. SOAR Architecture

* **SIEM (Security Information and Event Management) — Visibility and Detection:**
  * **Centralized Ingestion:** Continuous log collection across endpoints, network appliances, firewalls, and identity providers.
  * **Normalization:** Standardizing diverse log formats into a common schema (e.g., mapping divergent IP fields to a single source format).
  * **Correlation Rules:** Applying analytical logic to surface multi-source threat patterns (e.g., multiple failed authentications followed by an anomalous foreign login).
  * *NIST Positioning:* Primarily supports the **Detect** function.

* **SOAR (Security Orchestration, Automation, and Response) — Response and Orchestration:**
  * **Playbook Execution:** Automated remediation workflows targeting routine, high-volume alerts.
  * **Standardized Actions:** Indicator of Compromise (IoC) enrichment, automated firewall IP blocking, and temporary account quarantine.
  * *NIST Positioning:* Primarily supports the **Respond** function.

### 2. Incident Processing Workflow

1. **Generation:** Infrastructure endpoints produce audit and activity events (e.g., auth.log, syslog, firewall traffic).
2. **Aggregation:** The SIEM ingests, normalizes, and runs correlation rules against the telemetry stream.
3. **Alerting:** Predefined threat thresholds trigger a prioritized security alert.
4. **Tier-1 Analyst Triage:** Verification of the alert, context validation (filtering false positives), and initial containment following Standard Operating Procedures (SOP).
5. **SOAR Orchestration / Escalation:** Automated playbook remediation or escalation to Tier-2 analysts for advanced investigation.

### 3. Role of Security Culture in Threat Detection

Automated software sensors do not cover every attack surface. A positive organizational security culture turns employees into active defensive sensors:
* **Frictionless Reporting:** Staff must be empowered to flag suspicious phishing attempts immediately without fear of reprimand.
* **SOC Telemetry Enrichment:** Human reporting directly feeds the SOC detection pipeline, surfacing indicators of compromise before malicious payloads execute.
* **Reducing MTTD:** Combining technical alerts with timely employee notifications reduces the Mean Time to Detect (MTTD) operational threats.
