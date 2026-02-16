## LLM04:2025 Data and Model Poisoning

### Description

Data poisoning occurs when pre-training, fine-tuning, or embedding data is manipulated to introduce vulnerabilities, backdoors, or biases. This can compromise model security, performance, and ethical behavior, leading to harmful outputs or impaired capabilities. Common risks include degraded model performance, biased or toxic content, and exploitation of downstream systems.

Data poisoning can target different stages of the LLM lifecycle, including pre-training (learning from general data), fine-tuning (adapting the model to specific tasks), embedding (converting text to numerical vectors), and Retrieval Augmented Generation (fetching relevant data to supplement responses). Understanding these stages helps identify where vulnerabilities may emerge. Data poisoning is considered an integrity attack because tampering with training data impacts the model's ability to make accurate predictions. The risks are especially high with external data sources that may contain unverified or malicious content.

Moreover, models distributed through shared repositories or open-source platforms can carry risks beyond data poisoning, such as malware embedded through techniques like malicious pickling, where the model's serialized format is used to distribute and execute harmful code on loading.

Additionally, consider that prompt injection (as discussed in LLM01:2025 Prompt Injection) could be used in conjunction with this vulnerability, where untrusted content is provided to the model via prompt injection, increasing the damage and broadening the attack surface.

### Common Examples of Vulnerability

1. **Malicious Training Data Injection:** An attacker or unverified source contributes files or records containing hidden biases, misleading associations, or embedded instructions that subtly influence the model's outputs during pre-training, fine-tuning, or embedding stages.
2. **Poisoned Content Insertion:** An adversary inserts manipulated content into the model's ongoing learning sources (e.g., web-crawled data, user-generated feedback, or curated datasets), causing the model to learn incorrect or harmful patterns.
3. **Compromised Data Pipeline:** An attacker targets components of the data pipeline—such as data collection tools, preprocessing workflows, or storage systems—by introducing malicious content before the data reaches the training or fine-tuning step.
4. **Backdoor Attack via Fine-Tuning:** An adversary fine-tunes a model on a poisoned dataset with specific triggers, enabling the model to behave differently when certain keywords or patterns are present. The trained model may appear normal but produces targeted malicious outputs when triggered.
5. **Contaminated Retrieval Data (RAG Poisoning):** In systems using Retrieval Augmented Generation (RAG), an attacker poisons the external knowledge base documents. When the model retrieves those documents, the poisoned content influences the model's output, producing subtly inaccurate or biased responses.

### Prevention and Mitigation Strategies

1. **Track Data Provenance:** Use tools like OWASP CycloneDX or ML BOM to track your data's origins and transformations, particularly during pre-training, fine-tuning, and embedding. Verify that data hasn't been tampered with at each stage.
2. **Verify Data Integrity:** Perform integrity checks on all datasets used in pre-training, fine-tuning, and embedding. Validate data with checksums, audits, or similar methods to detect any tampering early.
3. **Use Adversarial Robustness Techniques:** Employ adversarial training methods, including federated learning and constraints, to minimize sensitivity to outliers and defend against extreme data perturbations. These techniques reduce the model's vulnerability to poisoned inputs.
4. **Test and Detect Poisoning with Anomaly Detection:** Use statistical methods and anomaly detection tools to identify unusual patterns or outliers in training data. Monitor for signs of poisoning through analysis of model behavior on test datasets, watching for outputs that indicate bias or unexpected patterns.
5. **Apply Filtering and Sandboxing:** Use input validation and curated datasets during pre-training, fine-tuning, and RAG retrieval to filter out potentially harmful data. Sandbox the model to prevent unverified data from entering the pipeline.
6. **Use Red Teaming and Monitoring:** Regularly evaluate model outputs through red teaming exercises and adversarial testing to identify signs of poisoning. Use automated monitoring to detect unusual or harmful outputs in real-time.
7. **Implement Bias and Fairness Audits:** Conduct regular bias and fairness audits on model outputs to ensure that poisoned data hasn't introduced or reinforced undesirable biases. Techniques like fairness-aware learning can help in identifying and correcting biases.
8. **Use Models from Verified Sources:** When using external models, ensure they come from reputable, security-verified sources. Use signed model artifacts (e.g., safetensors), validate checksums, and apply strict access controls for model repositories to prevent tampering.
9. **Use Version Control and Integrity Checks:** Monitor changes in data files and model artifacts using version control. Use cryptographic hashing to verify the integrity of datasets, embeddings, and model weights, ensuring no unauthorized modifications.
10. **Review Feedback Loops:** When using user-generated content or real-time data feeds, implement quality checks to prevent poisoned data from being incorporated continuously. Validate and moderate data before using it for retraining or fine-tuning.

### Example Attack Scenarios

**Scenario #1:**
An attacker corrupts the training data of an LLM by inserting records that contain biased, false, or misleading content. This leads to the model generating outputs that are factually wrong and biased, which can have significant consequences in areas such as healthcare, legal decisions, or financial recommendations.

**Scenario #2:**
An attacker poisons a retrieval data source in a RAG-based system by injecting harmful or misleading information into the documents used for augmenting model responses. When the model retrieves these poisoned documents, the outputs could spread misinformation or make harmful recommendations.

**Scenario #3:**
An attacker fine-tunes a model with poisoned data that includes backdoor triggers. For instance, the model may behave normally unless a specific keyword or pattern is included in the prompt, at which point it generates malicious or harmful content. This creates a covert mechanism for the attacker to control when the malicious behavior is activated.

**Scenario #4:**
An attacker compromises the data pipeline by injecting malicious content into a web-crawled dataset used for pre-training or fine-tuning. The malicious data introduces subtle biases or vulnerabilities in the model, which might go unnoticed until they cause real-world damage.

**Scenario #5:**
An attacker uses prompt injection techniques to insert malicious content into a document that is subsequently indexed in a RAG system. When a user queries the system, the poisoned data is retrieved and used to generate a harmful or misleading response, undermining the credibility of the system.

### Reference Links

1. [How data poisoning attacks corrupt machine learning models: CSO Online](https://www.csoonline.com/article/3613932/how-data-poisoning-attacks-corrupt-machine-learning-models.html)
2. [MITRE ATLAS (framework) TAJ1 - Poisoning Attacks](https://atlas.mitre.org/techniques/AML.T0020)
3. [PoisonGPT: How we hid a Lobotomized LLM on Hugging Face to spread fake news: Mithril Security](https://blog.mithrilsecurity.io/poisongpt-how-we-hid-a-lobotomized-llm-on-hugging-face-to-spread-fake-news/)
4. [Poisoning Language Models During Instruction Tuning: Arxiv White Paper](https://arxiv.org/abs/2305.00944)
5. [Poisoning Web-Scale Training Datasets: Nicholas Carlini](https://www.youtube.com/watch?v=h9jf1ikcGyk)
6. [Embrace The Red: Confused Deputy Problem](https://embracethered.com/blog/posts/2023/chatgpt-cross-plugin-request-forgery-and-prompt-injection./)
7. [Backdoor Attacks on Language Models: Towards Data Science](https://towardsdatascience.com/backdoor-attacks-on-language-models-can-we-trust-our-models-weights-73108f9dcb1f)
8. [Never a dill moment: Exploiting ML pickle files: TrailofBits](https://blog.trailofbits.com/2021/03/15/never-a-dill-moment-exploiting-machine-learning-pickle-files/)
9. [Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training: Anthropic](https://www.anthropic.com/news/sleeper-agents-training-deceptive-llms-that-persist-through-safety-training)
10. [Backdoor Attacks on AI Models: Cobalt](https://www.cobalt.io/blog/backdoor-attacks-on-ai-models)
11. [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)

### Related Frameworks and Taxonomies

- [AML.T0018 - Backdoor ML Model](https://atlas.mitre.org/techniques/AML.T0018) — MITRE ATLAS
- [AML.T0020 - Poison Training Data](https://atlas.mitre.org/techniques/AML.T0020) — MITRE ATLAS
