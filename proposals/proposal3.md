# Thesis Proposal: AI-Driven Cyber Threat Intelligence Pipeline
**Field:** Natural Language Processing (NLP), Security Automation, DevSecOps  
**Core Technologies:** Wazuh (SIEM), LLMs (GPT-4/Llama 3), MISP (CTI), Sigma/YARA Rules, Python.

---

## 1. Abstract
The speed of modern cyber-attacks often outpaces the ability of security engineers to manually author and deploy detection rules. This thesis proposes an automated **Continuous Threat Intelligence Pipeline**. The system monitors global threat feeds (**MISP**, **GCVE**), extracts technical intelligence using **Large Language Models (LLMs)**, and automatically synthesizes detection rules (e.g., **Wazuh XML**, **Sigma**, or **YARA**). By implementing a "Human-in-the-loop" validation framework, the project aims to bridge the gap between abstract threat descriptions and concrete infrastructure defense, significantly reducing the Mean Time to Remediate (MTTR).

---

## 2. Problem Statement
* **The Manual Bottleneck:** Security analysts spend hours translating PDF reports or MISP attributes into SIEM rules.
* **Format Heterogeneity:** Different security tools require different rule formats (Wazuh, Snort, YARA, Sigma), leading to duplicated effort.
* **Information Overload:** The sheer volume of new CVEs and IoCs makes it impossible for human teams to stay updated in real-time.

---

## 3. Proposed Architecture: The Intelligence-to-Rule Engine

### A. Intelligence Ingestion (ETL & RAG)
* **Ingestion:** A pipeline to pull raw data from MISP events and GCVE descriptions.
* **Retrieval-Augmented Generation (RAG):** Using a vector database to provide the LLM with relevant context (e.g., the specific architecture of the target network) to ensure the generated rules are applicable.

### B. Rule Synthesis (LLM Layer)
* **Prompt Engineering:** Developing specialized prompts to transform unstructured text into structured security logic.
* **Multi-Format Output:** The agent can "compile" the same threat intelligence into multiple formats:
    * **Wazuh Decoders/Rules** for log analysis.
    * **Sigma Rules** for SIEM-agnostic detection.
    * **YARA Rules** for file-based malware detection.

### C. Validation & CI/CD (The Sandbox)
* **Syntax Checking:** Automatic validation of generated XML/YARA files.
* **Testing:** Deploying rules to a "Twin" environment (Wazuh Sandbox) and replaying historical logs to check for False Positive rates.
* **Deployment:** If the rule passes validation, it is pushed via API to the production Wazuh Manager.

---

## 4. Research Objectives & Innovation
1.  **Automated Rule Synthesis:** Evaluating the effectiveness of LLMs in generating syntactically correct and semantically relevant security rules.
2.  **Cross-Platform Defense:** Creating a single source of truth (CTI) that feeds multiple detection engines simultaneously.
3.  **Efficiency Analysis:** Measuring the reduction in manual workload for SOC analysts compared to traditional rule-writing workflows.

---

## 5. Implementation Stack
* **LLM Engine:** OpenAI API or local deployment of **Llama 3 / Mistral** (for data privacy).
* **SIEM:** Wazuh Manager & Indexer.
* **Automation:** Python (FastAPI), Docker for sandbox isolation.
* **CI/CD:** GitHub Actions or GitLab CI for rule versioning and deployment.

---

## 6. Expected Results
* A functional pipeline that generates a Wazuh rule from a MISP event in under 60 seconds.
* A study on the accuracy and reliability of AI-generated rules vs. human-written rules.
* A framework for "Self-Healing Infrastructure" that adapts its detection capabilities based on the global threat landscape.

