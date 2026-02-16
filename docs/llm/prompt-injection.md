## LLM01:2025 Prompt Injection

### Description

A Prompt Injection Vulnerability occurs when user prompts alter the LLM's behavior or output in unintended ways. These inputs can affect the model even if they are imperceptible to humans, therefore prompt injections do not need to be human-visible/readable, as long as the content is parsed by the LLM.

Prompt Injection Vulnerabilities exist in how models process prompts, and how input may force the model to incorrectly pass prompt data to other parts of the model, potentially causing them to violate guidelines, generate harmful content, enable unauthorized access, or influence critical decisions. While techniques like Retrieval Augmented Generation (RAG) and fine-tuning aim to make LLM outputs more relevant and accurate, research shows that they do not fully mitigate prompt injection vulnerabilities.

Both prompt injection and jailbreaking are important concepts in LLM security, often discussed together. While both involve manipulating AI inputs to produce unintended responses, they target different aspects. **Jailbreaking** is designed to override the model's built-in safety alignment. A **Prompt Injection** uses crafted or unexpected inputs to manipulate the model's responses—sometimes resulting in the bypassing of safety mechanisms (i.e., jailbreaking as an outcome), but not always. Successfully jailbreaking a model typically requires iterative prompt injection techniques.

**Prompt Injection** can be classified into two types:

- **Direct Prompt Injection** occurs when user prompts directly change the behavior of the underlying model. This can be both intentional and unintentional. It acts as a direct input to the LLM, potentially leading to data exfiltration, social engineering, and other issues.
- **Indirect Prompt Injection** occurs when the LLM accepts input from external sources such as websites or files. The content may have in the external content data, which when parsed by the LLM changes the behavior. Similar to direct injections, indirect injections can result in data exfiltration, social engineering, and other issues. Additionally, these injections can produce harmful/unexpected results in agentic systems.

The severity and nature of the impact of a successful prompt injection attack can vary greatly and is largely dependent on both the business context in which the LLM operates, and the agency with which the LLM has been architecturally assigned. However, in general, prompt injection can lead to but is not limited to:

- Disclosure of sensitive information
- Disclosure of information about AI system infrastructure and system prompts
- Content manipulation leading to inaccurate or biased outputs
- Providing unauthorized access to functions available to the LLM
- Executing arbitrary commands in connected systems
- Manipulating critical decision-making processes

The rise of multimodal AI introduces additional prompt injection risks. Malicious instructions may be embedded in images, audio, or other modalities that the model processes. As models become capable of handling diverse inputs, traditional defenses may not cover all vectors, and new prompt injection techniques may emerge by exploiting cross-modal interactions.

### Prevention and Mitigation Strategies

Prompt injection vulnerabilities are possible due to the nature of foundation models, which function by predicting the next token based on the input they receive. LLMs are trained on language patterns, meaning that specific and reliable defenses against prompt injection may not yet exist. However, the following measures can mitigate the impact of prompt injections:

1. **Constrain Model Behavior:** Provide specific instructions about the model's role, capabilities, and limitations within the system prompt. Enforce strict context adherence, limit responses to specific tasks or topics, and instruct the model to ignore attempts to alter baseline instructions.
2. **Define and Validate Expected Output Formats:** Specify clear output formats, request detailed reasoning and source citations, and use deterministic code to validate adherence to these formats.
3. **Design Input and Output Filtering:** Define and enforce criteria for the content that can be present in interactions. This should be used for both input from the user and output responses from the model.
4. **Enforce Privilege Control and Least Privilege Access to external systems:** Provide the LLM application with its own API tokens for extensible functionality—such as plugins, data access, and function-level permissions—and follow the principle of least privilege by restricting the LLM to the minimum access level necessary for its operations.
5. **Require Human Approval for High-Risk Actions:** Implement human-in-the-loop controls for privileged operations to prevent unauthorized actions without human validation.
6. **Segregate and Identify External Content:** Separate and clearly denote untrusted content to limit its influence on user prompts.
7. **Monitor and Anomaly Detection:** Monitor LLM inputs and outputs regularly to ensure they fall within expected parameters. While this is not a complete prevention, monitoring and anomaly detection can help detect and respond to prompt injection attempts.
8. **Implement Secure Prompt Engineering Practices:** As a defense-in-depth strategy, even though prompt injection is hard to fully eliminate in LLMs, understanding common prompt engineering techniques and applying them carefully can make attacks more difficult.
9. **Use an LLM as a verifier:** A separate LLM instance can be used to verify outputs and detect prompt injection attempts, though this is not foolproof.

### Example Attack Scenarios

**Scenario #1: Direct Injection**
An attacker injects a prompt into a customer support chatbot, instructing it to ignore previous guidelines, query private data stores, and exploit vulnerabilities in backend systems, leading to unauthorized data access and privilege escalation.

**Scenario #2: Indirect Injection**
A user employs an LLM to summarize a webpage that contains hidden instructions causing the LLM to insert an image linking to a URL, which leads to exfiltration of the private conversation.

**Scenario #3: Unintentional Injection**
A company includes a prompt in a job description to identify AI-generated applications. An applicant, unaware of this, uses an LLM to optimize their resume, inadvertently triggering the AI detection prompt and resulting in their application being rejected despite being qualified and having submitted a genuine application.

**Scenario #4: Intentional Model Influence**
An attacker modifies a document in a repository used by a RAG application. When a user's query retrieves the modified content, the malicious instructions alter the LLM's output, generating misleading results.

**Scenario #5: Code Injection**
An attacker exploits a vulnerability (CVE-2024-5184) in an LLM-powered email assistant to inject malicious prompts, allowing access to sensitive information and manipulation of email content.

**Scenario #6: Payload Splitting**
An attacker uploads a resume with split malicious prompts. When an LLM is used to evaluate the candidate, the combined prompts manipulate the model's response, resulting in a positive recommendation despite the actual resume contents.

**Scenario #7: Multi-Modal Injection**
An attacker embeds a malicious prompt within an image that accompanies benign text. When a multi-modal AI processes the image, it follows the hidden instructions, potentially leading to unauthorized actions or data exposure.

**Scenario #8: Adversarial Suffix**
An attacker appends a seemingly meaningless string of characters to a prompt that actually serves as an adversarial suffix, causing the LLM to disregard its safety guidelines and respond in whatever way the attacker desires.

**Scenario #9: Multi-Language/Obfuscation**
An attacker uses lesser-used languages or encodes malicious instructions (e.g. using Base64 or emojis) to bypass filters in an LLM system. The model, which understands these languages and encoded text, follows the hidden instructions, and important safety guidelines are bypassed.

### Reference Links

1. [Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection: Arxiv preprint](https://arxiv.org/abs/2302.12173)
2. [Prompt Injection Attacks Are Real: Wired](https://www.wired.com/story/chatgpt-prompt-injection-attack-security/)
3. [PromptMap: Usenix](https://usenix.org/conference/usenixsecurity24/presentation/agarwal)
4. [AI Injections: Direct and Indirect Prompt Injections and Their Implications: Embrace The Red](https://embracethered.com/blog/posts/2023/ai-injections-direct-and-indirect-prompt-injection-basics/)
5. [ChatGPT Plugin Vulnerabilities - Chat with Code: Embrace The Red](https://embracethered.com/blog/posts/2023/chatgpt-plugin-vulns-chat-with-code/)
6. [ChatGPT Cross Plugin Request Forgery and Prompt Injection: Embrace The Red](https://embracethered.com/blog/posts/2023/chatgpt-cross-plugin-request-forgery-and-prompt-injection./)
7. [Defending ChatGPT Against Jailbreak Attack via Self-Reminder: Research Square](https://www.researchsquare.com/article/rs-2873090/v1)
8. [Prompt Injection attack against LLM-integrated Applications: Arxiv preprint](https://arxiv.org/abs/2306.05499)
9. [Inject My PDF: Prompt Injection for your Resume: Kai Greshake](https://kai-greshake.de/posts/inject-my-pdf/)
10. [ChatML for OpenAI API Calls: OpenAI Github](https://github.com/openai/openai-python/blob/main/chatml.md)
11. [Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection: Arxiv preprint](https://arxiv.org/abs/2302.12173)
12. [Threat Modeling LLM Applications: AI Village](https://aivillage.org/large%20language%20models/threat-modeling-llm/)
13. [AI Worms: Unleashing AI powers against AI applications: Stav Cohen at Cornell](https://stav.tech/attacks-on-llm-apps)
14. [OpenAI GPT-4 Technical Report: OpenAI](https://cdn.openai.com/papers/gpt-4.pdf)
15. [Universal and Transferable Adversarial Attacks on Aligned Language Models: LLM-Attacks Dot ORG](https://llm-attacks.org/)

### Related Frameworks and Taxonomies

- [AML.T0051.000 - LLM Prompt Injection: Direct](https://atlas.mitre.org/techniques/AML.T0051.000) — MITRE ATLAS
- [AML.T0051.001 - LLM Prompt Injection: Indirect](https://atlas.mitre.org/techniques/AML.T0051.001) — MITRE ATLAS
- [AML.T0054 - LLM Jailbreak Injection: Direct](https://atlas.mitre.org/techniques/AML.T0054) — MITRE ATLAS
