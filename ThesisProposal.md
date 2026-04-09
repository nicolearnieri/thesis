# Thesis Proposal: Predictive Cyber-Intelligence via Dynamic Knowledge Graphs
**Field:** Big Data Analytics, Deep Learning on Graphs, Cyber Security  
**Core Technologies:** Wazuh (Telemetry), Neo4j (Graph Database), Graph Neural Networks (PyG/DGL), MISP (Threat Intel), GCVE (Vulnerabilities).

---

## 1. Abstract
Traditional Security Information and Event Management (SIEM) solutions often operate in isolation, treating security events as flat, independent logs. This "siloed" approach fails to capture the complex, relational nature of modern Advanced Persistent Threats (APTs). This thesis proposes the development of a **Dynamic Threat Knowledge Graph (DTKG)** framework. By ingesting real-world, high-volume network telemetry from **Wazuh** (including granular firewall logs, TCP flows, and routing data), vulnerability intelligence from **GCVE**, and global threat indicators from **MISP**, the system builds a holistic, topological representation of enterprise infrastructure. Using **Graph Neural Networks (GNNs)** applied to this heterogeneous graph, the framework performs **Link Prediction** and **Node Classification** to infer hidden attack paths, lateral movement, and zero-day threats before they escalate into breaches.

---

## 2. Problem Statement
* **Contextual Blindness & Alert Fatigue:** Individual alerts in Wazuh (e.g., a firewall drop or an IDS trigger) lack the broader context of the network's topological state, leading to massive alert fatigue for SOC analysts.
* **Reactive Posture:** Current systems rely heavily on known signatures (IoCs). They struggle to detect novel (Zero-Day) attacks that manipulate structural network behaviors without triggering known signatures.
* **Data Heterogeneity & Fragmentation:** Security data is scattered across discrete formats (low-level network packets, CVE databases, threat intelligence feeds). Manually correlating a dropped TCP packet with a known Threat Actor campaign is nearly impossible at scale.

---

## 3. Proposed Architecture: The Graph-Centric Intelligence Hub

### A. Data Ingestion & Knowledge Modeling (Big Data Pipeline)
* **Topological Entity Extraction:** Developing an ETL pipeline to process raw Wazuh telemetry and extract entities, such as `Source/Destination IPs`, `Ports`, `Protocols`, `Geo-Locations`, and `TCP Flags`, mapping them into a **Heterogeneous Property Graph**.
* **External Intelligence Fusion:** Automatically enriching the graph context:
    * **MISP:** Linking external `IPs` to known Threat Actors, Campaigns, and malicious infrastructure.
    * **GCVE:** Mapping known vulnerabilities (CVEs) and CVSS scores to internal `Destination IPs` and exposed services.

### B. Neuro-Symbolic Inference Layer
The system employs a two-tier inference engine to combine data-driven learning with domain logic:
1.  **Deep Learning (GNN):** Utilizing models suited for heterogeneous data, such as **Heterogeneous Graph Attention Networks (HAN)** or **Graph Convolutional Networks (GCN)**, to identify anomalous sub-graphs. The model will primarily perform *Link Prediction* to estimate the probability of a "Malicious Interaction" between an external node and a vulnerable internal asset based on traffic patterns (size, fragmentation, frequency).
2.  **Symbolic Rules (Heuristics):** Implementing logic-based risk scoring to ground the neural network's predictions (e.g., "If an internal IP has a CVSS > 9.0 vulnerability AND the GNN predicts a high-probability link to an IP tagged by MISP, elevate the alert to Critical").

### C. Visualization & Attack Path Discovery
* Providing security analysts with a visual, dynamic representation of the network topology, highlighting predicted lateral movement paths and isolating the blast radius of a potential compromise.

---

## 4. Research Objectives & Innovation
1.  **Relational Threat Detection:** Shifting the analytical paradigm from "what happened at this endpoint" to "how are these network entities structurally interacting," enabling the detection of stealthy, multi-stage attacks.
2.  **Zero-Day Inference via Topology:** Leveraging the structural and flow patterns of network traffic via GNNs to identify suspicious behaviors that bypass standard SIEM rules.
3.  **Real-World Telemetry Validation:** Validating the proposed architecture against a massive, real-world, enterprise-grade Wazuh dataset, proving the scalability and effectiveness of the approach outside of synthetic environments.

---

## 5. Implementation Stack
* **Data Source:** Real-world enterprise Wazuh logs (Firewall, IDS, Network Telemetry).
* **Graph Engine:** Neo4j or Memgraph (optimized for high-performance streaming graph ingestion).
* **Machine Learning:** PyTorch Geometric (PyG) or Deep Graph Library (DGL) for training the GNN on heterogeneous nodes.
* **Orchestration:** Apache Kafka or logstash for handling the real-time event stream from Wazuh to the Graph DB.

---

## 6. Expected Contributions
* A scalable, automated pipeline for converting flat firewall/SIEM logs into a dynamic, queryable Threat Knowledge Graph.
* An empirical benchmark comparing GNN-based Link Prediction against standard rule-based SIEM alerting (e.g., default Wazuh rules) on real network data.
* An "Explainable Risk Score" that identifies the most critical nodes in the network based on both their intrinsic vulnerabilities (GCVE) and their relational exposure to external threats.

