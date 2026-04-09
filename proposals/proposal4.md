# Thesis Proposal: Topology-Aware Cyber-Threat Inference using Graph Neural Networks (GNNs)
**Field:** Graph Machine Learning, Network Security, Representation Learning  
**Core Technologies:** Wazuh (Event Stream), Neo4j/Memgraph, PyTorch Geometric, GraphSAGE/GAT, MISP.

---

## 1. Abstract
Current detection systems (SIEM/XDR) are primarily event-based and reactive, relying on predefined signatures or statistical outliers in linear logs. This thesis proposes a paradigm shift: treating cybersecurity as a **Link Prediction problem on a Dynamic Knowledge Graph**. By mapping Wazuh telemetry, CVE vulnerabilities, and MISP threat actors into a unified graph structure, we can leverage **Graph Neural Networks (GNNs)** to learn the underlying topology of cyber-attacks. The objective is to predict malicious interactions before they are explicitly flagged by traditional rules, identifying threats based purely on their structural behavior within the network.

---

## 2. Problem Statement
* **The Fragility of Rules:** Attackers easily bypass signature-based detection (Wazuh rules) by obfuscating payloads or rotating infrastructure.
* **Context Fragmentation:** Logs tell us *what* happened, but not *where* it sits in the global context of organizational vulnerabilities and external threat landscapes.
* **Non-Euclidean Complexity:** Network data is inherently a graph. Traditional ML models (Random Forests, CNNs) struggle to capture the long-range dependencies and relational context of a modern IT infrastructure.

---

## 3. Proposed Architecture: The Pure-Graph Inference Engine

### A. Graph Construction (The Knowledge Base)
* **Dynamic Ingestion:** Converting Wazuh events (Process created, Network connection, User login) into nodes and edges in real-time.
* **Feature Engineering:** Assigning rich attribute vectors to nodes (e.g., Host OS, Vulnerability Score from GCVE, Reputation score from MISP).

### B. The GNN Model (The Intelligence)
* **Architecture:** Implementing a **GraphSAGE** or **Graph Attention Network (GAT)** to generate inductive embeddings for every node.
* **Task: Link Prediction:** The model is trained to assign a probability score to the existence of an edge between an internal asset and an external/internal entity. A high probability for a non-existent edge indicates a **predicted threat path**.
* **Temporal Evolution:** (Optional/Advanced) Using **Temporal Graph Networks (TGNs)** to account for the fact that the order of events (A followed by B) is crucial in security.

### C. Proactive Alerting
* Instead of "Alert: Login Failed", the system outputs: "High Probability of Malicious Path: Host A is showing structural patterns consistent with Credential Dumping based on its current neighborhood."

---

## 4. Research Objectives & Innovation
1.  **Rule-less Detection:** Moving away from manual heuristics to purely topological feature learning.
2.  **Inductive Learning:** Ensuring the model can handle new hosts or IPs (unseen during training) without needing a full retrain.
3.  **Cross-Domain Correlation:** Demonstrating how global intelligence (MISP) changes the "geometric shape" of local network risks.

---

## 5. Implementation Stack
* **Graph Storage:** Neo4j (for persistence) or Memgraph (for high-speed streaming analytics).
* **Machine Learning:** PyTorch Geometric (PyG) or Deep Graph Library (DGL).
* **Data Flow:** Wazuh -> Logstash/Kafka -> Python Pipeline -> Graph DB.
* **Evaluation:** Using datasets like CIC-IDS2017 or a custom lab environment to benchmark GNN performance against standard Wazuh rules.

---

## 6. Expected Contributions
* A novel framework for real-time cybersecurity graph embedding.
* Evidence that structural patterns (topology) are more resilient indicators of compromise than static IoCs.
* A prototype capable of "Zero-Step" detection (predicting the next move of an attacker).
