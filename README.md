# AI Threat Modeling

A curated, comprehensive guide and collection of frameworks, repositories, and methodologies dedicated to **Threat Modeling for Artificial Intelligence and Machine Learning Systems**.

Traditional threat modeling frameworks (like STRIDE or DREAD) were designed for deterministic software systems with clear boundaries between code and data. Artificial Intelligence introduces a paradigm shift: models are probabilistic, logic is derived from data rather than explicit programming, and the attack surface includes mathematical perturbations, semantic manipulations, and supply chain poisoning. This document explores the specialized resources required to systematically identify, assess, and mitigate risks in AI systems before they are deployed.

## 📑 Table of Contents

1. [OWASP Top 10 for AI](#-owasp-top-10-for-ai)
   * [LLM Top 10 (Large Language Models)](#llm-top-10)
   * [ML Security Top 10 (Machine Learning)](#ml-security-top-10)
2. [MIT AI Risk Repository](#-mit-ai-risk-repository)
3. [MITRE ATLAS™](#-mitre-atlas)
4. [CVE AI Working Group](#-cve-ai-working-group)
5. [AI Threat Modeling Frameworks](#-ai-threat-modeling-frameworks)

## 🌐 OWASP Top 10 for AI

The Open Web Application Security Project (OWASP) has recognized that AI requires its own distinct vulnerability taxonomies. They have split this effort into two primary tracks: one for generative AI (LLMs) and one for traditional predictive/analytical Machine Learning.

### LLM Top 10

Focused entirely on the unique vulnerabilities introduced by Large Language Models and Generative AI applications (like RAG systems, AI agents, and chatbots). In LLMs, the line between "instructions" (code) and "context" (data) is non-existent; everything is processed as text tokens, making them highly susceptible to semantic attacks.

* **LLM01: Prompt Injection:** (Direct and Indirect) Attackers manipulate the LLM by embedding hidden commands in benign-looking inputs (direct) or external websites the model reads (indirect), forcing it to execute unauthorized actions or ignore its system instructions.
* **LLM02: Insecure Output Handling:** Treating LLM output as inherently safe. If an LLM generates a malicious XSS payload or a destructive SQL query, and the backend application executes it blindly without sanitization, the system is compromised.
* **LLM03: Training Data Poisoning:** Maliciously altering the pre-training data or fine-tuning datasets to introduce backdoors, biases, or logical blind spots into the foundation model.
* **LLM04: Model Denial of Service:** Attackers craft unusually heavy or repetitive prompts that force the LLM to consume massive amounts of GPU compute, slowing down the service for legitimate users or racking up massive API billing costs.
* **LLM05: Supply Chain Vulnerabilities:** The compromise of third-party datasets, pre-trained model weights (e.g., from Hugging Face), or plugin extensions that introduce malicious code into the AI ecosystem.
* **LLM06: Sensitive Information Disclosure:** The LLM inadvertently revealing confidential data, PII (Personally Identifiable Information), or proprietary algorithms that were embedded in its training data or system prompt.
* **LLM07: Insecure Plugin Design:** Plugins that grant the LLM access to external APIs but lack proper access controls, allowing an attacker to exploit the plugin through the LLM to access unauthorized backend systems.
* **LLM08: Excessive Agency:** Granting an LLM excessive autonomy, privileges, or permissions to take actions (e.g., deleting files, sending emails) without adequate Human-in-the-Loop (HITL) verification or strict boundary controls.
* **LLM09: Overreliance:** The tendency of users or systems to trust the LLM's output without verification. This leads to incorporating "hallucinations," insecure code, or factually incorrect data into production environments.
* **LLM10: Model Theft:** Unauthorized access, exfiltration, or reverse-engineering of proprietary LLM models, exposing highly valuable intellectual property and potentially allowing attackers to analyze the model for bypasses locally.

* **Repository:** [OWASP/GenAI-LLM-Top10](https://github.com/GenAI-Security-Project/GenAI-LLM-Top10)

### ML Security Top 10

Focused on traditional, predictive, and classification Machine Learning models (e.g., computer vision, fraud detection, recommendation engines). Threat modeling here focuses on how attackers can manipulate the statistical boundaries of the model.

* **ML01: Adversarial Attack (Evasion):** Crafting inputs with imperceptible mathematical perturbations (e.g., altering a few pixels in an image or adding noise to audio) to force the model to misclassify the input during inference (e.g., making an autonomous vehicle see a stop sign as a speed limit sign).
* **ML02: Data Poisoning Attack:** Injecting malicious or manipulated data into the training pool. Unlike LLMs, this often involves subtly shifting the decision boundaries of a classifier over time so it fails to detect specific malicious behavior.
* **ML03: Model Inversion Attack:** Repeatedly querying the model's API and analyzing the confidence scores to mathematically reconstruct the sensitive, original data it was trained on (e.g., reconstructing a recognizable face from a facial recognition model).
* **ML04: Membership Inference Attack:** Determining whether a specific individual's record or a specific data point was included in the model's training dataset, which can lead to severe privacy breaches (especially in healthcare ML).
* **ML05: Model Stealing (Extraction):** Replicating a proprietary model's functionality and decision boundaries by sending it thousands of carefully crafted queries and training a "shadow model" on the outputs, stealing the IP without needing backend access.
* **ML06: AI Supply Chain Attacks:** Compromising the dependencies of the ML pipeline, such as injecting malicious code into popular Python libraries (PyTorch, TensorFlow components) or tampering with pre-compiled model binaries (like `.pkl` files which can execute arbitrary code upon loading).
* **ML07: Transfer Learning Attack:** Exploiting known vulnerabilities in a publicly available "base" model. If a company fine-tunes a compromised base model for their own use, they inherit the base model's backdoors and weaknesses.
* **ML08: Model Skewing (Drift Manipulation):** In systems that continuously learn from new user data (online learning), attackers feed the system skewed data to intentionally degrade its accuracy or change its behavior over time.
* **ML09: Output Integrity Attack:** Intercepting and altering the model's prediction *after* it has been generated but *before* it reaches the downstream system that acts upon it, bypassing the AI entirely to force a desired outcome.
* **ML10: Model Poisoning (Federated Learning):** In decentralized training environments (like Federated Learning on mobile devices), malicious nodes submit poisoned gradient updates to corrupt the central global model without needing access to the central server.

* **Repository:** [OWASP/www-project-machine-learning-security-top-10](https://github.com/OWASP/www-project-machine-learning-security-top-10)

## 🏛️ MIT AI Risk Repository

A comprehensive, meticulously categorized database of AI risks developed by researchers at the Massachusetts Institute of Technology (MIT).

* **Deep Dive:** While OWASP focuses on technical vulnerabilities, the MIT AI Risk Repository takes a holistic view. It aggregates hundreds of risks extracted from academic literature, industry reports, and government frameworks. It is essential for threat modeling at the *enterprise and societal* level, ensuring that architects don't just secure the code, but also evaluate the systemic impact of the AI.
* **How it works:** The repository categorizes risks across multiple dimensions:
  * **Cause:** Is the risk caused by an intentional attacker, an accidental system failure, or environmental changes?
  * **Domain:** Does this impact Human-Computer Interaction, System Safety, Fairness, or Privacy?
  * **Sub-domain:** Highly granular classifications (e.g., "Automation bias," "Erosion of critical thinking," "Toxic output").
* **Threat Modeling Use Case:** Security architects use this repository during the early design phase to conduct tabletop exercises, ensuring that non-technical risks (like algorithmic discrimination or copyright infringement) are modeled alongside technical exploits.

## 🗺️ MITRE ATLAS

**ATLAS** (Adversarial Threat Landscape for Artificial-Intelligence Systems) is the AI-specific evolution of the industry-standard MITRE ATT&CK framework.

* **Deep Dive:** ATLAS provides a structured, matrix-based taxonomy of real-world adversary tactics and techniques against AI systems. It is arguably the most important tool for Red Teams and SOCs to understand the exact lifecycle of an AI attack.
* **The ATLAS Matrix (Key Tactics):**
  * **Reconnaissance & Resource Development:** How attackers map the AI's architecture, determine what framework (PyTorch, TensorFlow) is used, and acquire pre-trained models to search for inherited vulnerabilities.
  * **ML Model Access:** Techniques for gaining physical, API-level, or supply-chain access to the model.
  * **Evade ML Model:** The specific techniques used to bypass AI detection mechanisms (e.g., padding malware to bypass AI static analysis).
  * **Exfiltration:** Stealing the model weights (the intellectual property) or the sensitive training data stored within its neural network.
* **Threat Modeling Use Case:** Security teams use ATLAS to map out "Kill Chains." By simulating an attacker moving through the ATLAS matrix, defenders can identify gaps in their telemetry and place appropriate defensive controls at each step.
* **Resource:** [MITRE ATLAS GitHub / Website](https://github.com/mitre-atlas)

## 🐞 CVE AI Working Group

The Common Vulnerabilities and Exposures (CVE) system is the global standard for identifying cybersecurity vulnerabilities. However, AI breaks the traditional CVE model.

* **Deep Dive:** In traditional software, a vulnerability is a deterministic bug (e.g., a buffer overflow in OpenSSL) that has a specific, testable patch. In AI, if a model "hallucinates" a fake software package, or if it can be "jailbroken" to provide bomb-making instructions, is that a vulnerability, a feature, or a safety misalignment? How do you "patch" a neural network?
* **The Working Group's Goal:** The CVE AI Working Group is actively developing a unified taxonomy and set of guidelines for how to disclose, categorize, and track AI-specific vulnerabilities.
* **Threat Modeling Use Case:** Threat modelers must follow this group's output to understand how to document and report flaws in their proprietary AI systems, ensuring they align with emerging global vulnerability disclosure standards and legal compliance requirements.

## 🏗️ AI Threat Modeling Frameworks

Adapting traditional threat modeling methodologies to handle the probabilistic and data-centric nature of AI.

### Adapting STRIDE for Machine Learning

Microsoft's traditional STRIDE model (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege) must be translated for AI:
* **Spoofing** $\rightarrow$ **Identity Evasion:** Fooling facial recognition or biometric AI systems.
* **Tampering** $\rightarrow$ **Data Poisoning:** Altering the training dataset in the S3 bucket before the model is trained.
* **Information Disclosure** $\rightarrow$ **Model Inversion:** Extracting PII embedded in the model's weights.
* **Denial of Service** $\rightarrow$ **Sponge Examples:** Crafting specific inputs (Sponge Examples) that intentionally maximize the computational complexity of the neural network, burning GPU cycles and causing an algorithmic Denial of Service.

### NIST AI RMF (Risk Management Framework)

* **Deep Dive:** Published by the US National Institute of Standards and Technology, this is a highly structured framework for mapping, measuring, and managing AI risks. It is less about technical exploitation and more about rigorous governance.
* **Core Functions:** GOVERN, MAP, MEASURE, and MANAGE. It forces organizations to explicitly define the intended use case of the AI, measure its empirical safety limitations, and manage the residual risks through human-in-the-loop (HITL) oversight.

### Microsoft AI Threat Modeling Guidance

* **Deep Dive:** Microsoft has released comprehensive, open-source guidelines specifically for threat modeling AI systems. It introduces new data flow diagram (DFD) elements specifically for AI, such as "Training Data Store," "Model Endpoint," and "Data Pipeline." It forces engineers to ask questions like: *"What happens if the data pipeline is compromised? What if the endpoint receives an adversarial perturbation?"*
