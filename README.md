# agentic-ai-intake-poc: Agentic AI Solution Governance & Intake Portal

<img width="1274" height="663" alt="image" src="https://github.com/user-attachments/assets/2dd92add-e379-486d-a3dc-345d380a890f" />
<img width="1272" height="663" alt="image" src="https://github.com/user-attachments/assets/624bd1d9-2e26-4c5c-adb7-48117ad4f28a" />
<img width="1272" height="703" alt="image" src="https://github.com/user-attachments/assets/a0526f47-843e-4d02-887a-b000ebbf1be1" />

An interactive, single-page web application and risk-scoring engine designed to streamline, classify, and triage incoming **Agentic AI** solutions across the enterprise.

**Fastest Method to Live Demo:** Download the index.html file to your desktop. Open it with Edge or Chrome. 

Built on top of established AI safety, cybersecurity, and risk management standards—specifically the **NIST AI Risk Management Framework (AI RMF 1.0)** and the **OWASP Top 10 for LLMs & Agentic AI**—this Proof of Concept (PoC) demonstrates how organizations can achieve rapid AI adoption without compromising security, data privacy, or operational control.

---

## Intended Purpose & ITSM / Jira Integration

The primary purpose of **agentic-ai-intake-poc** is to serve as a standardized intake mechanism that can be integrated directly into enterprise ITSM solution catalogs (such as **ServiceNow**) or **Jira Service Management (JSM)**.

By embedding this scoring logic into your existing ticketing or service catalog workflows, organizations can:
* **Streamline Intake:** Provide business units and developers with a simple, friction-free self-service form to register new AI agents.
* **Automate Classification:** Automatically assign risk levels, blast radius parameters, and target SLAs without manual security triage.
* **Optimize Resource Allocation:** Route low-risk agents through fast-track auto-approvals while directing high-risk autonomous agents to dedicated AppSec and AI Risk review queues.

---

## Governance Framework & Foundations

The underlying intake questions and scoring logic are directly anchored in two leading industry frameworks:

1. **NIST AI Risk Management Framework (AI RMF 1.0):** Operationalizes the **Govern, Map, Measure, and Manage** core functions. It shifts the focus from static IT compliance to socio-technical risk management, evaluating model autonomy, execution guardrails, and context boundaries.
2. **OWASP Top 10 for LLMs & Agentic AI:** Specifically addresses agent-centric risk vectors including **Excessive Agency** (Action Execution Scope), **Non-Human Identity & Privilege Abuse** (Identity Context), **Indirect Prompt Injection**, and **Unintended Autonomy** (Action Reversibility & Autonomy Level).

---

##  How the Portal Works

The portal provides a **lightweight, self-service intake process** that automatically evaluates an agent's capability, data scope, and privilege boundaries. It converts user responses into a real-time risk score, automatically routing requests to the appropriate review track:

* **Tier 1 (Low Risk):** Fast-track auto-approval for read-only or low-impact assistants (< 24h SLA).
* **Tier 2 (Medium Risk):** Lightweight security check focused on identity boundaries and logging (2–3 Day SLA).
* **Tier 3 (High Risk):** Full NIST AI RMF assessment for autonomous, high-impact, or restricted-data agents (5–7 Day SLA).

### The 5 Assessment Categories
Users complete five key technical and operational evaluation questions:
* **A. Autonomy & Execution Independence:** Measures how independently the agent plans and executes decisions without human intervention.
* **B. Data Classification Scope:** Identifies the highest data classification level passing through the agent's context window or prompt logs.
* **C. Action Execution Scope:** Evaluates the depth of API, system, and execution access granted to the agent.
* **D. Identity & Auth Context:** Assesses how the agent authenticates (e.g., delegated user RBAC vs. shared/over-privileged service accounts).
* **E. Action Reversibility:** Evaluates whether unintended actions or hallucinated steps can be safely rolled back.

*Note: Each header includes an interactive `ⓘ Info` modal that explains what the category measures and why it matters for governance.*

---

## Real-Time Dynamic Scoring Engine

As the user selects options, an embedded JavaScript engine calculates a cumulative risk score ($0–24$ points) in real time:

$$\text{Total Score} = \text{Autonomy} + \text{Data Sensitivity} + \text{Action Scope} + \text{Identity Context} + \text{Reversibility}$$

* **Automated Overrides:** Selecting **Restricted/Regulated Data**, **High-Impact/Admin APIs**, or **Irreversible Actions** triggers an automatic override that immediately escalates the request to **Tier 3 (High Risk)**, regardless of total points.

The sidebar continuously updates to reflect:
* Calculated Risk Score & Assigned Tier Status
* Target Review SLA (< 24h, 2–3 Days, 5–7 Days)
* Required Sign-Off Authority (Line Manager, AppSec, CISO/Risk Board)
* NIST AI RMF Review Requirement (Exempt, Lightweight Check, Full Assessment)

---

## Getting Started & Deployment

Since the PoC is contained within a single standalone file, deployment is instant:

### Running Locally
1. Clone this repository:
   git clone https://github.com/cease-uno/agentic-ai-intake-poc.git

---
## Customization & Risk Taxonomy Alignment

The scoring logic in **agentic-ai-intake-poc** is fully modular and designed to adapt to your enterprise risk appetite:

* **Custom Weights & Categories:** Easily modify numerical scores, options, or categories in `index.html` to match internal governance standards.
* **Override Policies:** Add or update policy override triggers based on your organization's compliance boundaries (e.g., PCI-DSS, HIPAA, internal data handling guidelines).
* **ITSM Mapping:** Map output tiers directly to ServiceNow approval workflows, Jira issue types, or automated gateway policies.

For a complete breakdown of the scoring architecture and instructions on adapting the model, see SCORING_MODEL.md
