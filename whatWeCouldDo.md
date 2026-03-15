# Technical Proposal: Advanced SIEM Dataset Analysis
SIEM Dataset: [text](https://huggingface.co/datasets/darkknight25/Advanced_SIEM_Dataset)

Description provided by the dataset creator:

> The advanced_siem_dataset is a synthetic dataset of 100,000 security event records designed for training machine learning (ML) and artificial intelligence (AI) models in cybersecurity.
>
>It simulates logs from Security Information and Event Management (SIEM) systems, capturing diverse event types such as firewall activities, intrusion detection system (IDS) alerts, authentication attempts, endpoint activities, network traffic, cloud operations, IoT device events, and AI system interactions.
>
>The dataset includes advanced metadata, MITRE ATT&CK techniques, threat actor associations, and unconventional indicators of compromise (IOCs), making it suitable for tasks like anomaly detection, threat classification, predictive analytics, and user and entity behavior analytics (UEBA).


---
# Analysis Of The Dataset

TODO : che colonne contiene, quali sono i tipi di dati, se ci sono valori mancanti, ecc.

### Points of Interest:
- *Multimodal Data*: The dataset combines structured (IP, Timestamp), semi-structured (JSON), and unstructured (description, raw logs) data, offering rich opportunities for advanced analytics, Machine Learning, and Natural Language Processing (NLP).
- *Realistic Anomalies*: Synthetic generation ensures diverse and realistic anomaly patterns for robust model training.
- *Baseline deviation*: UEBA techniques can be applied to detect deviations from established user/device behavior, crucial for identifying insider threats and lateral movement. This can be done basing on the `risk_score` and `behavioral_analytics` fields.
- *Comprehensive Threat Coverage*: Inclusion of MITRE ATT&CK mappings provides a holistic view of potential threats.

---
# Potential Analytical Approaches
There are three main paths that could possibly be explored. 

### 1 Anomaly Detection (Unsupervised Learning)
The model learns the "normal" behavior of the system and flags deviations as potential threats. This is particularly useful for detecting zero-day exploits or insider threats that do not have known signatures.
- *The idea*: Using algorithms such as Isolation Forest, Local Outlier Factor (LOF), or Autoencoders to identify outliers in the dataset based on features extracted from the structured and unstructured data.

### 2 Threat Classification and Prioritization (Supervised Learning)
The model is trained on labeled data to classify events into categories (e.g., low, medium, high severity) or to predict a risk score. This helps in automating the triage process in a Security Operations Center (SOC), that is normally bombarded with alerts, and reducing alert fatigue.
- *The idea*: Using algorithms such as Random Forest, Gradient Boosting (XGBoost), or Neural Networks (to create a classifier) to predict the severity or risk score based on the available features. 
    Could also apply NLP techniques (like BERT embeddings) to the unstructured data (description, raw_log) to extract additional features that could improve the model's performance.

### 3 Temporal Analysis and Predictive Modeling (Time-Series Analysis)

---

# 1: Data Engineering & Pre-processing (The Foundation)

**Hierarchical Flattening**: Deconstruct nested JSON objects (advanced_metadata, behavioral_analytics) into a flat relational structure.
- Why: Standard ML libraries (Scikit-Learn, XGBoost) require a 2D matrix input.

**Time-Series Feature Extraction**: Decompose timestamp into cyclical features (hour of day, day of week) using sine/cosine transformations.
- Why: Security threats often follow temporal patterns (e.g., brute force during off-hours).

**Categorical Encoding**: Implement Target Encoding or Hash Encoding for high-cardinality fields like source and geo_location.
- Why: Traditional One-Hot Encoding would create a sparse, computationally expensive matrix.

---

# 2: Supervised Learning: Severity & Risk Prediction

**Multi-class Classification**: Predict the severity level based on event_type, source, and metadata.
- Why: Automates triage in a SOC (Security Operations Center), reducing "alert fatigue" by filtering low-priority noise.

**Regression on Risk Scores**: Use the risk_score as a continuous target variable to identify the intensity of a threat.
- Why: Provides a more granular view of danger than a simple category, allowing for better resource allocation.

---

# 3. Unsupervised Learning: Anomaly Detection
**Behavioral Outlier Detection**: Use the behavioral_analytics subset (10% of records) to train an Isolation Forest or Local Outlier Factor (LOF).
- Why: Zero-day exploits do not have signatures; they are detected only because they deviate from the established "baseline" behavior.

**Clustering for Threat Intelligence:** Apply K-Means or DBSCAN to group events.
- Why: Helps identify "campaigns" where a single threat actor uses multiple IP addresses or techniques to perform a coordinated attack.

--- 

# 4. Natural Language Processing (NLP) on Logs
**Log Parsing & Vectorization**: Apply TF-IDF or Word2Vec on the description and raw_log fields.
- Why: Raw logs contain unstructured data that often holds the "smoking gun" (e.g., specific CLI commands) missed by structured fields.

**Sentiment/Urgency Analysis**: Analyze the semantic structure of the description to correlate text urgency with confidence levels.
- Why: Validates if the human-readable summary matches the machine-generated risk metrics.

# 5. Advanced Analytics: UEBA & MITRE Mapping
**User & Entity Behavior Analytics (UEBA)**: Aggregate events by session_id or device_hash to create a "user profile."
- Why: Detects insider threats or lateral movement where individual events look "normal" but the sequence is malicious.

**MITRE ATT&CK Correlation**: Map event_type and description to specific adversarial tactics (TTPs).
- Why: Bridges the gap between pure Data Science and Domain Expertise, making the model's output actionable for security analysts.

