# CIR

## Introduction to CIR (Cyber Issue Record)

### Context

The cybersecurity ecosystem widely relies on the Common Vulnerabilities and Exposures (CVE) system to track and share information about known security flaws. While CVEs provide a crucial reference point for vulnerability identification, they lack the contextual depth and real-world exploitation data needed for modern risk assessment, threat response, and operational prioritization.

In today’s environment — with increasingly complex attack surfaces, chained exploits, and evolving threat actors — organizations require a richer, more adaptable format to evaluate, document, and share vulnerability data beyond static scores or simplistic summaries.

### What is CIR?

CIR (Cyber Issue Record) is a human-readable, machine-parseable YAML-based format designed to enhance traditional CVE data with actionable metadata, real-world threat context, and organizational risk scoring. CIR is not intended to replace CVEs, but to extend and operationalize vulnerability tracking for modern security teams.

### Why CIR?
- **Context matters**: CIR supports additional fields such as public exploit availability, attack vectors, business impact, and active exploitation reports.
- **Custom scoring**: Organizations can define their own risk scores, allowing prioritization based on real exposure and impact, not just theoretical severity.
- **Flexible integration**: Using YAML makes it easy to embed CIR files in CI/CD pipelines, asset management tools, or incident response systems.
- **Collaboration and sharing**: CIRs can be versioned, enriched by analysts, and shared internally or publicly — much like security playbooks or detection rules.

By introducing CIR, we aim to create a modern, extensible vulnerability record format that empowers defenders, researchers, and red teams to operate with more nuance, clarity, and shared intelligence.

## CIR Specification
The CIR specification is designed to be human-readable and machine-parseable, allowing for easy integration into various tools and workflows. Below is a detailed breakdown of the CIR format, including its structure, fields, and examples.

### Structure
A CIR record is represented in YAML(seen later) format, which is a human-readable data serialization standard. Each CIR record consists of several key sections, each containing specific fields that provide context and information about the vulnerability.

#### Fields

- **id**: A unique identifier for the CIR record, typically in the format `CIR-YYYY-PC-NNNN`, where `YYYY` is the year and `NNNN` is a sequential number and `PC` is the project code.

| Élément       | Exemple     | Description                                                 |
|---------------|-------------|-------------------------------------------------------------|
| `CIR`         | —           | Préfixe constant pour tout enregistrement                  |
| `YEAR`        | `2025`      | Année de publication ou découverte                         |
| `PROJECTCODE` | `00`, `P01`, `INT`, etc. | `00` pour les CIR publics, autre pour interne/projet |
| `UNIQUEID`    | `0001`, `9864`, etc. | Identifiant local ou incrémental, unique |

| CIR ID               | Contexte                                      |
|----------------------|-----------------------------------------------|
| `CIR-2025-00-0003`   | CIR public #3 de 2025                         |
| `CIR-2025-SNOW-0007` | CIR interne de l'équipe Sn0wAlice             |
| `CIR-2025-LAB-1092`  | CIR pour un projet client ou produit LAB      |

---

- **version**: The version of the CIR specification being used. This should be updated whenever there are changes to the specification.
- **title**: A brief, descriptive title for the CIR record. This should summarize the vulnerability or issue in a few words.
- **alias**: An optional field that provides alternative names, in slug format.
- **published**: The date when the CIR record was published, in ISO 8601 format (YYYY-MM-DD).
- **last_updated**: The date when the CIR record was last updated, in ISO 8601 format (YYYY-MM-DD).
- **authors**: A list of authors or contributors to the CIR record. This can include names, email addresses, or other identifiers.
- **description**: A detailed description of the vulnerability, including its impact, affected systems, and any relevant background information. This should provide enough context for someone unfamiliar with the issue to understand its significance.

---

- **product**: A section that provides information about the affected product or software. This includes:
    - **name**: The name of the product or software affected by the vulnerability. This should be a clear and concise name that identifies the product.
    - **version**: The version(s) of the product affected by the vulnerability. This can include specific versions, ranges, or "all versions" if applicable.
    - **vendor**: The name of the vendor or organization responsible for the product. This should be the official name of the vendor.

---

- **severity**: A section that provides information about the severity of the vulnerability. This includes:
  - **technical**: A section that provides technical details about the vulnerability, including:
    - **cvss_base_score**: The CVSS base score for the vulnerability, if applicable.
    - **cvss_vector**: The CVSS vector string for the vulnerability, if applicable.
    - **exploitability_score**: A score indicating the exploitability of the vulnerability, typically on a scale from 0 to 10.
    - **impact_score**: A score indicating the potential impact of the vulnerability, typically on a scale from 0 to 10.
    - **priority_score**: A score indicating the priority for remediation, typically on a scale from 0 to 10.
  - **exposure**: A section that provides information about the exposure of the vulnerability, including:
    - **internet_exposed**: A boolean value indicating whether the vulnerability is exposed to the internet.
    - **auth_required**: A boolean value indicating whether authentication is required to exploit the vulnerability.
    - **lateral_movement_possible**: A boolean value indicating whether lateral movement is possible through exploitation of the vulnerability.
  - **detection**: A section that provides information about detection capabilities for the vulnerability. This includes:
    - **can_be_detected**: A boolean value indicating whether the vulnerability can be detected by security tools or processes.
    - **detection_tools**: A list of tools or methods that can be used to detect the vulnerability.
  - **business_impact**: A section that provides information about the potential business impact of the vulnerability. This includes:
    - **critical_asset_affected**: A boolean value indicating whether a critical asset is affected by the vulnerability.
    - **potential_data_loss**: A boolean value indicating whether there is potential for data loss due to exploitation of the vulnerability.
    - **downtime_risk**: A description of the risk of downtime associated with exploitation of the vulnerability.
  - **final_score**: A section that provides a final score for the vulnerability, including:
    - **score**: A string representation of the final score, typically in the format "cvss_base_score/exploitability_score/impact_score/priority_score".
    - **risk_level**: A description of the risk level associated with the vulnerability, such as "critical," "high," "medium," or "low."
    - **analyst_notes**: Any additional notes or comments from the analyst regarding the vulnerability, including any special considerations or context.

---

- **exploit**: A section that provides information about the exploitability of the vulnerability. This includes:
  - **known**: A boolean value indicating whether a known exploit exists for the vulnerability.
  - **exploit_db_id**: An optional field that provides a reference to an exploit database entry, if applicable.
  - **poc**: A link to a proof-of-concept (PoC) or exploit code, if available.
  - **observed_in_the_wild**: A boolean value indicating whether the vulnerability has been observed being exploited in the wild.
  - **threat_actor**: Information about any known threat actors associated with the vulnerability
    - **name**: The name of the threat actor or group.
    - **description**: A brief description of the threat actor, including their motivations and known activities.
    - **tactics**: A list of tactics used by the threat actor, such as initial access, execution, persistence, etc.

---

- **attack_surface**: A section that provides information about the attack surface associated with the vulnerability. This includes:
  - **exposed**: A boolean value indicating whether the vulnerability is exposed to the internet or requires internal access.
    - **attack_vector**: A list of attack vectors associated with the vulnerability, such as network, local, or physical.
    - **affected_components**: A list of components or modules affected by the vulnerability, such as web server, database, or API.
    - **impact**: A description of the potential impact of the vulnerability, including any data loss, system compromise, or other consequences.
    - **business_impact**: A description of the potential business impact of the vulnerability, including any financial, reputational, or operational consequences.

---

- **mitigations**: A section that provides information about any mitigations or workarounds for the vulnerability. This can include patches, configuration changes, or other recommendations for reducing risk.
  - **type**: The type of mitigation, such as "patch," "configuration change," or "workaround."
  - **description**: A detailed description of the mitigation, including any steps required to implement it.
  - **status**: The status of the mitigation, such as "available," "in progress," or "not applicable."
  - **date**: The date when the mitigation was published or made available, in ISO 8601 format (YYYY-MM-DD).

---

- **related**: A section that provides information about related vulnerabilities or issues. This can include CVEs, other CIRs, or any other relevant references.
  - **cve**: A list of CVE identifiers associated with the vulnerability.
  - **cwe**: A list of Common Weakness Enumeration (CWE) identifiers associated with the vulnerability.
  - **references**: A list of external references, such as blog posts, advisories, or research papers related to the vulnerability.
  - **links**: A list of URLs or links to additional resources, such as vendor advisories, exploit databases, or other relevant information.

---

- **tags**: A list of tags or keywords associated with the CIR record. This can include categories, severity levels, or any other relevant tags that help classify the vulnerability.

- **notes**: An optional section for any additional notes or comments related to the CIR record. This can include references to external resources, internal notes, or any other relevant information.
  - **author**: The name of the author or team responsible for creating the CIR record.
  - **comments**: Any additional comments or notes related to the CIR record.
  - **date**: The date of the comment or note, in ISO 8601 format (YYYY-MM-DD).

#### CIR YAML Example
```yaml
version: 1.0

# CIR (Cyber Issue Record) Specification
# This is a sample CIR record for demonstration purposes.
# It includes various fields and sections to provide context and information about a vulnerability.
id: CIR-2024-00-0001
alias: VULN-ARGO-001
title: Remote Code Execution in ArgoApp
published: 2025-04-12
last_updated: 2025-05-01
authors:
  - name: "Jane Smith"
    email: "jane.smith@exemple.com"

description: >
  A critical RCE vulnerability in ArgoApp due to improper input validation
  in the file upload handler. Can be exploited via a crafted HTTP POST request.

product:
  name: ArgoApp
  version: ["1.0", "1.1", "2.0"]
  vendor: ArgoCorp

severity:
  technical:
    cvss_base_score: 9.8
    cvss_vector: "AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H"
    exploitability_score: 9.5
    impact_score: 9.8
    priority_score: 9.2
  exposure:
    internet_exposed: true
    auth_required: false
    lateral_movement_possible: true
  detection:
    can_be_detected: true
    detection_tools:
      - Suricata
      - Shina Rule: win_rce_upload
  business_impact:
    critical_asset_affected: true
    potential_data_loss: true
    downtime_risk: high
  final_score:
    score: "9.8/9.5/9.8/9.2"
    risk_level: critical
    analyst_notes: "Critical due to exploitation in the wild and exposure of business-critical systems"


exploit:
  known: true
  exploit_db_id: 51423
  poc: "https://github.com/example/poc"
  observed_in_the_wild: true
  threat_actor:
    name: "APT-XYZ"
    description: "Known to exploit RCE vulnerabilities in cloud applications."
    tactics:
      - initial_access
      - execution
      - persistence

attack_surface:
    exposed: true
    attack_vector:
        - network
        - local
    affected_components:
        - web_server
        - api
    impact: >
        Full system compromise, data exfiltration, and potential lateral movement.
    business_impact: >
        Significant financial loss, reputational damage, and regulatory penalties.

mitigations:
  - type: patch
    description: "Update to version 2.1 or later."
    status: available
    date: 2025-04-15
  - type: configuration
    description: "Disable file upload feature if not needed."
    status: in progress
    date: 2025-04-20

related:
  mitre_attack:
    - T1190
  cwe:
    - CWE-20

tags:
  - remote-code-execution
  - critical
  - webapp
  - exploit-public

notes:
  - author: "John Doe"
    comments: "Initial discovery and analysis."
    date: 2025-04-10
```