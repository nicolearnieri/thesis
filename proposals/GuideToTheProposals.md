# Guide to the thesis proposals 

## Proposal 1: **Neuro-Symbolic Autonomous Agents for Cyber-Threat Hunting**


## **Thesis Title: Neuro-Symbolic Autonomous Agents for Cyber-Threat Hunting**
**Focus:** Agentic AI, Automated Planning (PDDL), & Explainable AI (XAI)

### **1. The Problem: "Alert Fatigue & Static Response"**
* **Cognitive Overload:** SOC analysts are overwhelmed by thousands of alerts from Wazuh, many of which are ambiguous or "grey" threats.
* **Static Playbooks:** Current automated responses (SOAR) are often rigid "if-then" scripts that cannot adapt to complex, multi-step attack scenarios.
* **The "Black Box" Trust Issue:** Traditional AI can flag an anomaly, but it cannot explain *why* it chose a specific remediation action, making it risky to automate critical infrastructure defense.

### **2. The Proposed Solution: "The Autonomous Investigator"**
An intelligent agent that doesn't just "detect," but "investigates and plans." It uses a **Neuro-Symbolic** approach: **Deep Learning** for perception (detecting patterns) and **PDDL** for strategic reasoning (solving the problem).
* **Active Investigation:** The agent autonomously queries external APIs (MISP, GCVE) to validate a suspicion before acting.
* **Dynamic Planning:** Using PDDL, the agent generates a step-by-step "action plan" (e.g., isolate host → snapshot RAM → block IP) based on the current state and business constraints.
* **Explainability by Design:** Since PDDL creates a logical sequence of actions, the agent can provide a human-readable justification for its decisions.



### **3. The Tech Stack**
* **Detection Engine:** Wazuh (HIDS/SIEM).
* **Perception Layer:** Python/Transformers (for log parsing and intent extraction).
* **Reasoning Engine:** PDDL (Planning Domain Definition Language) with solvers like Fast Downward or OPTIC.
* **Knowledge Sources:** MISP (Threat Intel), GCVE (Vulnerability Data).
* **Framework:** MAPE-K (Monitor-Analyze-Plan-Execute-Knowledge) Loop.

### **4. Implementation Pipeline**
1.  **Perception (Monitor/Analyze):** A pipeline intercepts a Wazuh alert. A DL model converts the raw log into a "Symbolic State" (e.g., `is_suspicious(process_x, host_a)`).
2.  **Enrichment (Knowledge):** The agent automatically checks MISP for IP reputation and GCVE for host vulnerabilities to add predicates to the logic state.
3.  **Strategic Planning (Plan):** The agent defines a Goal (e.g., `threat_contained(host_a)`) and uses a PDDL solver to find the optimal sequence of actions given the network costs.
4.  **Action (Execute):** The plan is executed via Wazuh active responses or API calls to firewalls/EDRs.
5.  **Reporting:** The agent outputs a logical trace of its reasoning for the human analyst.


---
## Proposal 2: **Predictive Cyber-Intelligence via Dynamic Knowledge Graphs**
**Focus:** Big Data & Deep Learning (Neuro-Symbolic AI)

### **1. The Problem: "Siloed & Reactive Security"**
* **Data Fragmentation:** Security logs (Wazuh), vulnerability databases (GCVE), and threat intelligence (MISP) exist in isolation, making correlation manual and slow.
* **Signature Dependency:** Traditional SIEMs look for known "fingerprints." They are blind to **Zero-Day attacks** where the signature is unknown but the *logical behavior* is suspicious.
* **Lack of Context:** A single alert doesn't show the "attack path" or how a low-level vulnerability on one host could lead to a breach on another.

### **2. The Proposed Solution: "The Knowledge Hub"**
The project builds a **Dynamic Threat Knowledge Graph (DTKG)** that treats the entire infrastructure as a living network of relationships.
* **Contextualization:** It maps hosts, users, processes, and vulnerabilities as interconnected nodes.
* **Neuro-Symbolic Inference:** It combines **Graph Neural Networks (GNNs)** to predict hidden threats (Link Prediction) with **Symbolic Rules** to enforce security logic (e.g., CVSS scoring).
* **Zero-Day Detection:** By analyzing structural patterns (how entities interact), the system can flag suspicious "shapes" of data even if the specific attack hasn't been seen before.



### **3. The Tech Stack**
* **Telemetry/Data:** Wazuh (Host/Network logs).
* **Intelligence:** MISP (Threat Feeds), GCVE/NVD (Vulnerabilities).
* **Graph Engine:** Neo4j or Memgraph.
* **AI/ML:** PyTorch Geometric (PyG) or DGL (Deep Graph Library).
* **Data Pipeline:** Apache Kafka (Real-time ingestion).

### **4. Implementation Pipeline**
1.  **Ingestion:** Stream real-time events from Wazuh agents into a Kafka pipeline.
2.  **Graph Modeling:** Map flat logs into a Property Graph (e.g., `User` --*Executes*--> `Process` --*ConnectsTo*--> `IP`).
3.  **Enrichment:** Automatically link graph nodes to external MISP actors and GCVE exploits.
4.  **Inference:** Run GNN models to calculate "Risk Scores" and predict malicious links between seemingly unrelated nodes.
5.  **Visualization:** Generate an "Attack Path" map for SOC analysts to see potential lateral movement.

---
