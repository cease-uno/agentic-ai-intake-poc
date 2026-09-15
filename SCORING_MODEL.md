# Agentic AI Intake PoC Scoring Model Blueprint

This document outlines the architectural blueprint, category weighting, and override logic powering the **agentic-ai-intake-poc** risk engine. 

It is provided as a reference model that organizations can adapt, re-weight, or extend to align with their specific cybersecurity frameworks, data sensitivity classifications, and enterprise risk appetite.

---

## 🎯 Architecture Overview

The scoring model evaluates an agent's **blast radius** (maximum potential operational, legal, and security impact) by combining weighted multi-choice inputs with automated policy overrides.

### Mathematical Definition
The raw risk score is calculated as the sum of five discrete ordinal variables:

$$\text{Total Score} = A + B + C + D + E$$

* **Score Range:** 0 to 28 points
* **Primary Focus:** Model Autonomy, Permission Scope, and Action Reversibility
* **Control Mechanism:** High-impact options trigger an immediate **Tier 3 (High Risk)** escalation regardless of total numerical score.

---

## 📋 Weighted Category Schemas

### A. Autonomy & Execution Independence ($A$)
Measures how independently the agent plans and executes multi-step actions without human intervention.

* **0 Points — Level 0 (Assistive):** Strictly human-in-the-loop. Suggestions and read-only text output.
* **2 Points — Level 1 (Semi-Autonomous):** Autonomous multi-step planning; requires human confirmation before tool execution.
* **4 Points — Level 2 (Autonomous):** Executes multi-step API sequences autonomously within defined guardrails.
* **6 Points — Level 3 (Fully Autonomous):** Self-directing goals, dynamic tool selection, and automated feedback loops.

### B. Data Classification Scope ($B$)
Evaluates the highest sensitivity tier passing through the model's context window or vector memory.

* **0 Points — Public Data:** Publicly accessible information only.
* **2 Points — Internal Non-Sensitive:** Standard operational documents and general internal knowledge bases.
* **4 Points — Confidential / Proprietary:** Customer data, internal code repositories, financial reports.
* **6 Points — Restricted / Regulated:** PII, PHI, PCI-DSS, corporate secrets, API keys, credentials.  
  *(⚠️ Triggers Tier 3 Override)*

### C. Action Execution Scope ($C$)
Evaluates OWASP Excessive Agency risks based on execution permissions granted to backend systems.

* **0 Points — Read-Only / Conversational:** Pure data retrieval; zero state modification.
* **2 Points — Low-Impact Write:** Low-risk state changes (e.g., ticket creation, drafting email templates).
* **4 Points — High-Impact Write:** Modifying production records, executing live customer-facing messaging or API updates.
* **6 Points — System / Admin Control:** Infrastructure changes, IAM privilege updates, database drops, financial execution.  
  *(⚠️ Triggers Tier 3 Override)*

### D. Identity & Authentication Context ($D$)
Assesses privilege delegation and account boundary controls.

* **0 Points — Delegated User Session:** Operates strictly within active user's OAuth/RBAC context.
* **2 Points — Scoped Non-Human Account:** Dedicated API key/service account with strict least-privilege scoping.
* **4 Points — Shared Service Account:** Shared credential with broad read/write access across services.
* **6 Points — Elevated Non-Human Account:** Admin or superuser non-human identity with minimal execution friction.

### E. Action Reversibility ($E$)
Evaluates blast radius impacts in the event of hallucination, goal drift, or unexpected execution loops.

* **0 Points — Fully Reversible / N/A:** Read-only operations or simple one-click undo mechanisms.
* **2 Points — Partially Reversible:** Requires manual administrative intervention or database rollback.
* **4 Points — Irreversible:** Actions cannot be undone (external broadcasts, permanent data deletion, financial transactions).  
  *(⚠️ Triggers Tier 3 Override)*

---

## 🚦 Triage Matrix & Policy Overrides

| Risk Tier | Score Range | Review SLA | Required Sign-Off Authority | NIST AI RMF Track |
| :--- | :--- | :--- | :--- | :--- |
| **Tier 1 (Low Risk)** | 0–5 points | < 24 Hours | Line Manager / Peer Review | Exempt / Self-Assessment |
| **Tier 2 (Medium Risk)** | 6–11 points | 2–3 Days | AppSec Lead / Security Reviewer | Lightweight Control Check |
| **Tier 3 (High Risk)** | 12–28 points | 5–7 Days | CISO / AppSec Board / AI Risk Council | Full NIST AI RMF Assessment |

### Automated Override Triggers
To prevent over-reliance on numerical scores alone, selecting any of the following options immediately escalates the submission to **Tier 3 (High Risk)**:
1. `Data Classification Scope` = **Restricted / Regulated (6 pts)**
2. `Action Execution Scope` = **System / Admin Control (6 pts)**
3. `Action Reversibility` = **Irreversible (4 pts)**

---

## 🛠️ How to Customize for Your Organization

1. **Adjusting Point Values:** To emphasize specific risks (e.g., data privacy over system permissions), modify option values in `index.html` or update the calculation logic in your ITSM platform.
2. **Adding Custom Override Rules:** Add additional policy overrides (e.g., external vendor hosting or custom compliance flags) in `calculateRisk()` in `index.html`.
3. **ITSM Mapping:** Map the output fields (`tier`, `sla`, `authority`) to your organization's custom fields in Jira Service Management or ServiceNow Catalog workflows.
