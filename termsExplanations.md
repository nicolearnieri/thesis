
### 1. Unsupervised Learning (Anomaly Detection)

* **Isolation Forest:** Instead of profiling "normal" points, it isolates anomalies. It randomly selects a feature and a split value; since anomalies are rare and different, they require fewer "splits" to be isolated in a tree structure. This makes it very effective for high-dimensional data like SIEM logs.
* **Local Outlier Factor (LOF):** Measures the local density deviation of a data point relative to its neighbors. If a point’s density is much lower than its neighbors, it’s an outlier. This is particularly useful for detecting subtle anomalies that may not be globally rare but are unusual in their local context (e.g., a user suddenly accessing a resource they never accessed before).
* **Autoencoders (Deep Learning):** Neural networks trained to compress data into a lower-dimensional "bottleneck" and then reconstruct it.
  * *Application:* You train it only on "normal" logs. When it sees an attack, the **Reconstruction Error** will be high because the model doesn't know how to "rebuild" that specific malicious pattern.



---

### 2. Supervised Learning (Classification)

* **Random Forest:** An ensemble of many Decision Trees. It uses "bagging" (Bootstrap Aggregating, sampling with replacement) to reduce variance and prevent overfitting.
* **XGBoost (Gradient Boosting):** Builds trees sequentially. Each new tree tries to correct the errors (residuals) made by the previous ones. It is currently the "gold standard" for tabular data. It can handle missing values and has built-in regularization to prevent overfitting, which is crucial in a SIEM context where the data can be noisy.
* **Neural Networks:** Layers of interconnected "neurons" that learn non-linear relationships. Good if you have a massive amount of data, but harder to interpret than XGBoost. In a SIEM context, you could use a simple feedforward network or even a more complex architecture like a Convolutional Neural Network (CNN) if you treat the log data as a "text image" (e.g., using embeddings).

---

### 3. Natural Language Processing (NLP)

* **TF-IDF:** A statistical measure evaluating how relevant a word is to a document in a collection. It filters out common "stop words" (like *the, is, at*) and highlights rare security keywords (like *Exploit, Injection*). In SIEM, this can help identify which logs contain critical indicators of compromise (IOCs) based on the presence of specific terms.
* **Word2Vec:** A shallow neural network that maps words into a vector space where words with similar meanings (e.g., "Admin" and "Root") are mathematically close to each other. This can help in understanding the context of log descriptions and in clustering similar events together.
* **BERT Embeddings:** A Transformer-based model that understands **context**. Unlike Word2Vec, BERT knows that "Bank" in "Bank Account" is different from "River Bank." In SIEM, it helps understand the intent behind a log description. For example, "Failed login attempt" and "Successful login" might have similar words but very different implications. BERT can capture that nuance.

---

### 4. Statistical Correlation

* **Theil’s U (Uncertainty Coefficient):** Based on Information Theory (Entropy). It measures how much knowing the value of Variable A reduces the uncertainty of Variable B. Unlike standard correlation, it works perfectly for **Categorical vs. Categorical** data. For example, it can tell you how much knowing the `event_type` reduces the uncertainty of the `severity_level`. A high Theil’s U (close to 1) means that `event_type` is a strong predictor of `severity_level`, while a low value (close to 0) means it’s not informative at all.
* **Correlation Ratio ($\eta$):** Measures the relationship between a **Categorical** variable (e.g., `event_type`) and a **Numerical** variable (e.g., `risk_score`). It tells you if certain event types consistently produce higher risk scores. A high Correlation Ratio (close to 1) indicates that the mean risk score varies significantly across different event types, suggesting that `event_type` is a strong predictor of `risk_score`. A low value (close to 0) suggests that the risk scores are similar across event types, indicating a weak relationship.

---

### 5. Domain-Specific Frameworks

* **UEBA (User and Entity Behavior Analytics):** Moving from "Event-centric" to "User-centric." You create a profile (baseline) for every user. If a user suddenly logs in from a new country at 3 AM, UEBA flags the **deviation**. This is crucial for detecting insider threats or compromised accounts that behave "normally" most of the time but have occasional malicious activity.
* **MITRE ATT&CK:** A globally accessible knowledge base of adversary tactics and techniques based on real-world observations. It provides a "common language" for security researchers. Mapping your SIEM events to MITRE techniques can help in understanding the attack patterns and in communicating findings to security teams. For example, if you identify a log that matches the "Tactic: Initial Access" and "Technique: Spear Phishing," you can immediately understand the attack vector and recommend specific mitigations.

---

### 6. Essential Terms for The Thesis

* **SMOTE (Synthetic Minority Over-sampling Technique):** Since you will have many "Low" severity events and very few "Critical" ones, SMOTE creates synthetic "Critical" examples to balance the dataset.
* **SHAP (SHapley Additive exPlanations):** A method to explain *why* your model made a prediction. It shows which specific feature (e.g., `src_ip` or `geo_location`) contributed most to a "High" severity alert. This is **Explainable AI (XAI)**.
* **Feature Importance:** A technique to rank which columns in your dataset are actually useful for the model and which are just noise.
* **Confusion Matrix (Precision/Recall):** In SIEM, a **False Negative** (missing an attack) is much worse than a **False Positive** (a false alarm). You must explain your results using these metrics, not just "Accuracy."

