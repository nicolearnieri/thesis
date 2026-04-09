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


## Proposal 3: **AI-Driven Cyber Threat Intelligence Pipeline**
**Focus:** NLP, Generative AI (LLMs), & Security Orchestration (ASR)

### **1. The Problem: "The Intelligence-to-Action Gap"**
* **The Manual Bottleneck**: Security analysts spend hours manually reading PDF reports or MISP feeds and translating them into technical rules (Wazuh XML, Sigma, YARA).
* **Alert Lag**: By the time a human writes a detection rule for a new exploit (Zero-Day or 1-Day), the attacker has often already moved on.
* **Format Fatigue**: Security teams must maintain multiple languages for different tools (Wazuh for logs, YARA for files, Snort for network), leading to errors and duplicated effort.

### **2. The Proposed Solution: "The Autonomous Rule Architect"**
An automated pipeline that uses Large Language Models (LLMs) to transform raw threat intelligence into deployable code in seconds.
* **Automated Synthesis**: The system "reads" unstructured text from MISP or CVE descriptions and "writes" structured detection logic (e.g., Wazuh Decoders).
* **RAG-Enhanced Context**: Uses Retrieval-Augmented Generation (RAG) to ensure the AI understands the specific network architecture it is protecting, avoiding generic or "hallucinated" rules.
* **Self-Healing Infrastructure**: A CI/CD-style loop where rules are generated, tested in a sandbox, and automatically pushed to the Wazuh Manager if they pass syntax and logic checks.

### **3. The Tech Stack**
* **LLM Engine**: GPT-4 (via API) or Llama 3 / Mistral (local for data privacy).
* **Orchestration**: Python (FastAPI) & LangChain/LlamaIndex.
* **SIEM/XDR**: Wazuh (Manager & Indexer).
* **Intelligence Feeds**: MISP (Threat Intel), NVD/GCVE (Vulnerabilities).
* **Automation**: Docker (for rule sandboxing) and GitHub Actions (for deployment).

### **4. Implementation Pipeline**
1. **Ingestion**: Scrape new IOCs/Threat descriptions from MISP/GCVE.
2. **Synthesis**: An LLM-agent parses the threat and generates a Sigma Rule (generic) or Wazuh XML (specific).
3. **Validation**: The rule is run through a syntax checker and a "Twin" sandbox to ensure it doesn't crash the production environment.
4. **Testing**: Historical logs are replayed against the new rule to measure False Positive rates.
5. **Deployment**: Verified rules are pushed to the Wazuh API and live-reloaded into the infrastructure.



---

## Proposal 4: **Topology-Aware Cyber-Threat Inference using GNNs**
**Focus:** Graph Machine Learning & Representation Learning

###  **1. The Problem:"The Fragility of Linear Detection"** 
* **Signature Bypass:** Attackers easily evade Wazuh’s static rules by renaming processes or rotating IPs, but they cannot easily change the relational logic of an exploit.
* **Non-Euclidean Data:** Cyber data is inherently a graph (Host A $\rightarrow$ Process B $\rightarrow$ Connection C). Traditional ML models treat logs as linear text, losing the spatial and structural context of the network.
* **Reactive Posture:** Current SIEMs alert you after a specific rule is triggered. They struggle to identify the "silent" preparation phases of an attack that don't match a known pattern.

 **2. The Proposed Solution:"Cyber-Security as Link Prediction"** 
This thesis treats network security as a Link Prediction problem on a Dynamic Knowledge Graph. Instead of looking for "bad strings," we look for "bad shapes."
*   **Structural Embeddings:** Using Graph Neural Networks (GNNs) to transform every entity (Host, IP, User) into a high-dimensional vector (embedding) that captures its role and risk within the topology.
*   **Predictive Inference:** The model assigns a probability score to potential interactions. A high probability for an interaction that hasn't happened yet—or shouldn't happen—signals a predicted threat path.
*   **Zero-Day Resilience:** By learning the "topology of an attack" (e.g., how Lateral Movement looks on a graph), the system can detect novel threats based purely on their structural behavior.

**3. The Tech Stack**
*   **Graph Processing:** Memgraph (in-memory streaming) or Neo4j.
*   **Deep Learning:** PyTorch Geometric (PyG) or Deep Graph Library (DGL).
*   **GNN Architectures:** GraphSAGE (for inductive learning) or Temporal Graph Networks (TGNs) to capture the sequence of events.
*   **Telemetry:** Wazuh Event Stream.
*   **Intelligence:** MISP (Threat Actors) and GCVE (Vulnerability enrichment).

**4. Implementation Pipeline**
*   **Graph Construction:** Convert raw Wazuh logs into a dynamic graph where nodes are entities and edges are actions.
*   **Node Enrichment:** Inject global context (CVSS scores from GCVE, IP reputation from MISP) as initial node features.
*   **Inductive Training:** Train the GNN to learn embeddings. Because it is inductive, the model can evaluate new hosts or subnets without being completely retrained.
*   **Anomaly Inference:** Perform Link Prediction to identify "impossible" or "highly suspicious" edges being formed in real-time.
*   **Validation:** Test the model against benchmark datasets (e.g., CIC-IDS) to prove it outperforms standard rule-based detection.