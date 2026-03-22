# Thesis Proposal: Predictive Cyber-Intelligence via Dynamic Knowledge Graphs
**Field:** Big Data Analytics, Deep Learning on Graphs, Cyber Security  
**Core Technologies:** Wazuh (Telemetry), Neo4j (Graph Database), Graph Neural Networks (PyG/DGL), MISP (Threat Intel), GCVE/NVD (Vulnerabilities).

---

## 1. Abstract
Traditional SIEM solutions often operate in isolation, treating security events as flat, independent logs. This "siloed" approach fails to capture the complex, relational nature of modern Advanced Persistent Threats (APTs) and Zero-Day exploits. This thesis proposes a **Dynamic Threat Knowledge Graph (DTKG)** framework. By ingesting telemetry from **Wazuh**, vulnerability data from **GCVE**, and global intelligence from **MISP**, the system builds a holistic representation of the enterprise infrastructure. Using **Graph Neural Networks (GNNs)**, the framework performs **Link Prediction** and **Node Classification** to infer potential threats and hidden attack paths before they escalate into security breaches.

---

## 2. Problem Statement
* **Contextual Blindness:** Individual alerts in Wazuh often lack the broader context of the network's topological and historical state.
* **Reactive Posture:** Current systems rely heavily on known signatures (IoCs), making them ineffective against novel (Zero-Day) attacks that haven't been indexed yet.
* **Data Heterogeneity:** Security data is scattered across different formats (system logs, CVE databases, threat intelligence feeds), making manual correlation nearly impossible for human analysts.

---

## 3. Proposed Architecture: The Graph-Centric Intelligence Hub

### A. Data Ingestion & Knowledge Modeling (Big Data Pipeline)
* **Entity Extraction:** Developing a pipeline to extract entities (Hosts, Users, Files, Processes, IPs) from Wazuh alerts and mapping them into a **Property Graph Model**.
* **External Fusion:** Automatically enriching the graph with:
    * **MISP:** Linking IPs/Hashes to known Threat Actors and Campaigns.
    * **GCVE:** Linking Software versions on hosts to known exploits.

### B. Neuro-Symbolic Inference Layer
The system employs a two-tier inference engine:
1.  **Deep Learning (GNN):** Utilizing **Graph Convolutional Networks (GCN)** or **Graph Attention Networks (GAT)** to identify anomalous sub-graphs. The model performs *Link Prediction* to estimate the probability of a "Malicious Interaction" existing between two seemingly unrelated nodes.
2.  **Symbolic Rules:** Implementing logic-based risk scoring (e.g., "If Host A has a CVSS > 9.0 vulnerability AND is connected to an external IP with a low reputation score on MISP, increase Risk Rank by X").

### C. Visualization & Alerting
* **Attack Path Discovery:** Providing SOC analysts with a visual representation of how a threat could propagate through the network (Lateral Movement prediction).

---

## 4. Research Objectives & Innovation
1.  **Relational Detection:** Shifting the focus from "what happened" to "how entities are related," enabling the detection of stealthy multi-stage attacks.
2.  **Zero-Day Inference:** Leveraging the structural patterns of attacks via GNNs to identify suspicious behaviors that don't match known signatures.
3.  **Automated Enrichment:** Demonstrating a self-updating security architecture that consumes and correlates global threat feeds in real-time.

---

## 5. Implementation Stack
* **Data Source:** Wazuh (SIEM/XDR).
* **Graph Engine:** Neo4j or Memgraph (for high-performance streaming).
* **Machine Learning:** PyTorch Geometric (PyG) or Deep Graph Library (DGL).
* **Orchestration:** Apache Kafka or RabbitMQ for handling the real-time event flow from Wazuh to the Graph DB.

---

## 6. Expected Contributions
* A scalable schema for representing Cybersecurity Knowledge.
* A benchmark comparing GNN-based detection against standard rule-based SIEM alerting.
* An "Explainable Risk Score" that identifies the most critical nodes in the network based on their relational vulnerability.

---