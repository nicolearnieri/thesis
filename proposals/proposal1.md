# Thesis Proposal: 
**Field:** Cyber Security, Artificial Intelligence, Automated Planning  
**Core Technologies:** Wazuh (SIEM/XDR), PDDL (Planning Domain Definition Language), Deep Learning, MISP/GCVE Integration

---


## 1. Abstract
Modern Security Operations Centers (SOCs) are overwhelmed by "alert fatigue." Traditional SIEMs like **Wazuh** excel at detection but require human intervention for triage and contextual investigation. This thesis proposes the development of an **Autonomous Intelligent Agent** that bridges the gap between detection and response. By combining **Deep Learning** (for anomaly perception) with **Symbolic Reasoning (PDDL)**, the agent mimics a Tier-1 analyst: it investigates alerts, queries external Intelligence (MISP/CVE), and generates provably logical response plans.

---

## 2. Problem Statement
* **Static Response:** Current Automated Response (Active Response) in Wazuh is often hardcoded (if X, then Y), failing to adapt to complex, multi-stage attacks.
* **Context Gap:** Alerts often lack external context (e.g., Is this IP a known Tor exit node? Is this binary vulnerable?).
* **Explainability (XAI):** Black-box AI models can classify threats but cannot explain the *logic* behind a suggested mitigation strategy, which is critical for high-stakes infrastructure.

---

## 3. Proposed Architecture: The Neuro-Symbolic Loop
The system follows the **MAPE-K framework** (Monitor, Analyze, Plan, Execute - Knowledge) but with a novel twist: the "Analyze" and "Plan" phases are tightly integrated through a **Neuro-Symbolic Loop**.

### A. Perception Layer (Deep Learning & ETL)
* **Data Source:** Real-time event streams from Wazuh Manager (JSON alerts/Archives).
* **Processing:** An ETL pipeline normalizes heterogeneous logs.
* **Anomaly Detection:** A specialized model (e.g., **Log-Transformer** or **Isolation Forest**) evaluates the "intent" and "severity" of the alert, transforming raw data into **Logical Predicates**.
    * *Example:* A raw log becomes `(is_suspicious_process powershell.exe host_01)`.

### B. Enrichment Layer (Dynamic Intelligence)
The agent autonomously interacts with:
* **MISP (Malware Information Sharing Platform):** To retrieve IoC (Indicators of Compromise) reputation.
* **GCVE/NVD:** To check if the alerted host has unpatched vulnerabilities that match the observed behavior.

### C. Reasoning & Planning Layer (PDDL)
This is the core innovation. The agent uses a **PDDL Planner** to find the optimal sequence of actions.
* **Domain:** Defines the "physics" of the network (actions like `isolate_host`, `block_ip`, `dump_memory`).
* **Problem:** Defines the current state (from Perception) and the goal (e.g., `(threat_neutralized)`).
* **Output:** A high-level, human-readable **Action Plan**.

---

## 4. Research Objectives & Innovation
1.  **Autonomous Investigation:** Moving beyond simple "detection" to "active investigation" without human triggers.
2.  **Hybrid AI:** Combining the pattern recognition of **Neural Networks** with the rigorous logic of **Symbolic AI (PDDL)**.
3.  **Cost-Aware Mitigation:** PDDL allows assigning "costs" to actions (e.g., isolating a production server is more "expensive" than blocking a single IP), ensuring the agent chooses the least disruptive path.

---

## 5. Potential Implementation Stack
* **SIEM:** Wazuh (Manager & Agents).
* **Language:** Python (for the Agent logic and API integration).
* **Planning:** Fast Downward or Unified Planning Framework (UPF).
* **Intelligence:** PyMISP for threat data retrieval.
* **Environment:** Virtualized lab (Proxmox/ESXi) or Dockerized microservices.

---

## 6. Expected Results
* A prototype agent capable of reducing the time-to-triage (MTTR) for specific attack scenarios (e.g., Brute force, Lateral movement).
* A comparative analysis between standard Wazuh Active Response and the PDDL-based Autonomous Agent.
* A demonstration of **Explainable Security**, where the agent provides a logical trace of *why* a specific sequence of actions was chosen.

