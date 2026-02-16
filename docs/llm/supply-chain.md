## LLM03:2025 Supply Chain

### Description

LLM supply chains are susceptible to various vulnerabilities, which can affect the integrity of training data, ML models, deployment platforms, and operational workflows. These vulnerabilities can result in biased outcomes, security breaches, or complete system failures. Traditionally, vulnerabilities focused on software components, but AI extends this with pre-trained models and training data supplied by third parties susceptible to tampering and poisoning attacks.

Finally, LLM Plugin extensions can bring their own vulnerabilities. These are described in LLM07 - Insecure Plugin Design, which covers writing LLM Plugins and provides helpful information on evaluating third-party plugins.

### Common Examples of Risk

1. **Traditional Third-Party Package Vulnerabilities:** Outdated or deprecated components used without verification, including situations where components running on top of the model's execution pathway — e.g., through the `exec` or `eval` Python functions — are not regularly checked for vulnerabilities.
2. **Licensing Risks:** AI development often involves diverse licensing frameworks. A model's license governs how it can be used and distributed. Model weights may carry unique licensing terms on top of permissive licenses like Apache-2.0 or MIT that apply to source code. When training data includes web-scraped or copyrighted material, copyright laws may further restrict model usage.
3. **Outdated or Deprecated Models:** Using outdated or deprecated models that are no longer maintained leads to security issues.
4. **Vulnerable Pre-Trained Model:** Models are binary blobs that are opaque, and unlike open-source software, static inspection can offer very little clue to safety. Pre-trained models could contain hidden biases, backdoors, or other malicious features that have not been identified through safety evaluations of the model repository. Such vulnerabilities could be created both by poisoned datasets and by direct model tampering using techniques such as ROME plus LoRA, which may be offered by poisoned adapters.
5. **Weak Model Provenance:** Currently, there are no strong assurances that the model came from a trustworthy source. Model cards and associated documentation provide model information and relied upon information from the creators, but there are no guarantees offered by model hosting repositories for a model and associated artifacts' origins.
6. **Vulnerable LoRA Adapters:** LoRA is a popular fine-tuning technique that enhances modularity by allowing pre-trained layers to be bolted onto an existing LLM. The method increases efficiency but creates new risks where a malicious layer from an untrusted adapter introduces attack vectors into a pre-trained base model. The adapters could be hosted on public model repositories or at other locations.
7. **Exploiting Collaborative Development Processes:** Shared model merging and MOE (Mixture of Experts) based Model-Merge processing uploaded by untrusted sources or using poisoned and out-of-date shared models with community collaboration could exploit these as attack vectors. This is relevant in community and co-development environments.
8. **On-Device ML Model Supply Chain Risks:** On-device models face specific supply chain risks from supplier compromise, insertion of malicious functionality through specialized hardware or software attacks, and side-channel attacks through the physical supply chain itself.
9. **Unclear Terms and Conditions and Data Privacy Policies:** Unclear T&Cs and data privacy policies of the model operators lead to the application's sensitive data being used for model training and subsequent sensitive information exposure, often combined with risks from using copyrighted material.

### Prevention and Mitigation Strategies

1. Carefully vet data sources and suppliers, including T&Cs and their privacy policies, only using trusted suppliers. Regularly review and audit supplier Security and Access, ensuring no changes in their security posture or T&Cs.
2. Understand and apply the mitigations found in the OWASP Top Ten's [A06:2021 – Vulnerable and Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/). This includes vulnerability scanning, management, and patching components. For development environments with access to sensitive data, apply these controls in those environments, too.
3. Apply comprehensive AI Red Teaming on supplied models and verify they have been red teamed, evaluated and have been checked for vulnerabilities as well. Check the OWASP entry for [AI Red Teaming](https://www.llmtop10.com/llm). Use the model as a starting point and apply extensive testing rather than assuming the model in its initial state is safe.
4. Maintain an up-to-date inventory of components using a Software Bill of Materials (SBOM) to ensure you have an up-to-date, accurate, and signed inventory, preventing tampering with deployed packages. SBOMs can be used to detect and alert for new, zero-day vulnerabilities quickly.
5. At the time of writing, AI SBOMs are a work in progress. Wherever an SBOM is available, the SBOM should cover model provenance, datasets sourced from third parties, and should include a supply chain security analysis of the model's fitness for the application.
6. Use MLOps best practices and platforms offering secure model repositories with data, model, and experiment tracking to ensure supply chain integrity.
7. Use model and code signing when using external models and suppliers.
8. Anomaly detection and adversarial robustness tests on supplied models and data are core tools to detect tampering and poisoning, as discussed in Training Data Poisoning; ideally, this should be part of MLOps and LLM pipelines; however, these are emerging techniques and may be easier implemented as part of red teaming exercises.
9. Implement sufficient monitoring to cover component and environment vulnerabilities, the use of unauthorized plugins, and out-of-date components, including the model and its artifacts.
10. Implement a patching policy to mitigate vulnerable or outdated components. Ensure that the application relies on a maintained version of APIs and the underlying model.

### Example Attack Scenarios

**Scenario #1:**
An attacker exploits a vulnerable Python library to compromise an LLMs application. This happened in the first Open AI data breach.

**Scenario #2:**
An attacker provides an LLM plugin for flight search that generates fake links leading to scamming users.

**Scenario #3:**
An attacker exploits the PyPi package registry to trick model developers into downloading a compromised package and exfiltrating data or escalating privilege in a model development environment. Attack that happened in practice.

**Scenario #4:**
An attacker poisons a publicly available pre-trained model specializing in economic analysis and social research to create a backdoor that generates misinformation and fake news. They deploy it on a model marketplace (e.g., Hugging Face) for victims to use.

**Scenario #5:**
An attacker poisons publicly available datasets to help create a back door when fine-tuning models. The back door subtly favors certain companies in different markets.

**Scenario #6:**
A compromised employee of a supplier (outsourcing dev, hosting company, etc.) exfiltrates data, model, or code, stealing IP.

**Scenario #7:**
An LLM operator changes its T&Cs and Privacy Policy so that it requires an explicit opt out from using application data for model training, which could lead to memorization of sensitive data.

**Scenario #8:**
A model operator changes its T&Cs so the enterprise using its services bears the burden of harm from AI-generated content.

**Scenario #9:**
An attacker exploits a vulnerable component in a model weights-sharing platform to deploy a model with hidden backdoors. The compromised model may spread through collaborative development environments and model merging, potentially affecting other models that incorporate it.

**Scenario #10:**
An attacker exploits a vulnerable LoRA adapter hosted on a public model repository, that has been uploaded by a seemingly trusted contributor, to inject malicious behavior into any base model that loads the adapter's weights.

**Scenario #11:**
An attacker exploits model merge and Mixture of Experts functionality to poison models or inject backdoors by contributing compromised components.

**Scenario #12:**
An attacker uses a technique such as ROME (Rank-One Model Editing) to modify factual associations in a base model. This can be used to embed triggers or persistent misinformation in a model that is then offered as a legitimate pre-trained model.

**Scenario #13:**
An attacker modifies a model's safetensor files on a hosted platform, embedding hidden behaviors that can activate under specific conditions, creating a "sleeper agent" model.

### Reference Links

1. [PoisonGPT: How we hid a Lobotomized LLM on Hugging Face to spread fake news: Mithril Security](https://blog.mithrilsecurity.io/poisongpt-how-we-hid-a-lobotomized-llm-on-hugging-face-to-spread-fake-news/)
2. [Poisoning Language Models During Instruction: Arxiv White Paper 2305.00944](https://arxiv.org/abs/2305.00944)
3. [Poisoning Web-Scale Training Datasets - Nicholas Carlini | Stanford MLSys #75: Stanford MLSys Seminars YouTube Video](https://www.youtube.com/watch?v=h9jf1ikcGyk)
4. [ML Model Repositories: The Next Big Supply Chain Attack Target: OffSecML](https://www.darkreading.com/cloud-security/ml-model-repositories-next-big-supply-chain-attack-target)
5. [Data Scientists Targeted by Malicious Hugging Face ML Models with Silent Backdoor: JFrog](https://jfrog.com/blog/data-scientists-targeted-by-malicious-hugging-face-ml-models-with-silent-backdoor/)
6. [Backdoor Attacks on Language Models: Towards Data Science](https://towardsdatascience.com/backdoor-attacks-on-language-models-can-we-trust-our-models-weights-73108f9dcb1f)
7. [Never a dill moment: Exploiting machine learning pickle files: TrailofBits](https://blog.trailofbits.com/2021/03/15/never-a-dill-moment-exploiting-machine-learning-pickle-files/)
8. [arXiv:2401.05566 Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training: Anthropic (arXiv)](https://www.anthropic.com/news/sleeper-agents-training-deceptive-llms-that-persist-through-safety-training)
9. [Backdoor Attacks on AI Models: Cobalt](https://www.cobalt.io/blog/backdoor-attacks-on-ai-models)

### Related Frameworks and Taxonomies

- [AML.T0018 - Backdoor ML Model](https://atlas.mitre.org/techniques/AML.T0018) — MITRE ATLAS
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) — Strategies for ensuring AI integrity
