## LLM02:2025 Sensitive Information Disclosure

### Description

Sensitive information can affect both the LLM and its application context. This includes personal identifiable information (PII), financial details, health records, confidential business data, security credentials, and legal documents. Proprietary models may also have unique training methods and source code that are considered sensitive, particularly in closed-source or foundation models.

LLMs risk exposing sensitive data, proprietary algorithms, and confidential details through their output. This can result in unauthorized access to sensitive data, intellectual property, privacy violations, and security breaches. It's important for consumers of LLM applications to be aware of how to safely interact with LLMs and identify the risks associated with unintentionally providing sensitive data that may subsequently be returned by the LLM in output elsewhere.

To mitigate this risk, LLM applications should perform adequate data sanitization to prevent user data from entering training model data. LLM application owners should also have appropriate Terms of Use policies available to make consumers aware of how their data is processed and the ability to opt out of having their data included in the training model.

The interaction between the consumer and LLM application forms a two-way trust boundary, where we cannot inherently trust the client→LLM input or the LLM→client output. It's important to note that this vulnerability assumes that certain pre-requisites are out of scope, such as threat modeling exercises, securing infrastructure, and adequate sandboxing. Adding restrictions within the system prompt regarding the types of data the LLM should return can provide some mitigation against sensitive information disclosure, but the unpredictable nature of LLMs means such restrictions may not always be honored and could be circumvented via prompt injection or other vectors.

### Common Examples of Vulnerability

1. **PII Leakage:** Personal identifiable information (PII) surfaces in the LLM's responses due to having been present in training data or provided through prompting or connected systems.
2. **Proprietary Algorithm Exposure:** A poorly configured LLM output exposes proprietary algorithms, source code, or confidential business strategies, risking competitive advantage and intellectual property theft. For example, a risk of "Googling" style attacks when a model like ChatGPT provides accurate technical information or source code, belonging to proprietary training data, to a user that inquires about it.
3. **Confidential Business Data Disclosure:** The LLM's responses include sensitive business data such as internally-generated content, partner information, or client details, by having been trained on or by having been provided access to such data as part of its operational context.

### Prevention and Mitigation Strategies

1. **Data Sanitization:** Implement robust data sanitization techniques to prevent user data from being inadvertently incorporated into training model data, including data cleaning and scrubbing before training.
2. **Robust Input Validation and Sanitization:** Apply thorough input validation and sanitization methods to prevent potentially malicious or unexpected inputs from impacting the model's behavior, including filtration and data masking.
3. **Access Controls and Principle of Least Privilege:** When applying the model's operational context, implement strict access controls and the principle of least privilege for LLM access to external data sources. Provide the LLM application with only the data necessary for its intended function. Establish strong authentication and authorization mechanisms, such as OAuth and role-based access controls, and use encryption for sensitive data at rest and in transit to further minimize the risks of unauthorized access.
4. **Data Provenance and Tracking:** Maintain detailed logs of data sources and transformations used during LLM training and operation, enabling auditing of data access and identification of potential information leakage.
5. **Federated Learning:** Employ federated learning techniques that allow the model to learn from distributed data sources without centralizing sensitive data.
6. **User Education:** Educate users about the risks of providing sensitive information when interacting with LLMs. Provide guidelines for best practices, such as avoiding inputting confidential data, and implement clear Terms of Use policies.
7. **Secure Infrastructure Configuration:** Ensure that the LLM's infrastructure is securely configured, with network segmentation, encryption, and other security measures to prevent unauthorized access to sensitive data.
8. **Advanced Techniques:** For sensitive applications, consider using differential privacy, homomorphic encryption, or secure multi-party computation techniques to add additional layers of protection.

### Example Attack Scenarios

**Scenario #1:**
A user interacts with an LLM application that has been trained on a dataset containing personal health records. The user asks the model about common side effects of a particular medication. The model inadvertently includes PII from its training data in its response, revealing names and medical conditions of other patients.

**Scenario #2:**
A user asks an LLM for a code snippet to implement a specific algorithm, and the LLM unintentionally reveals proprietary source code from its training data. An attacker then uses this disclosed proprietary code to reverse-engineer a company's product, gaining a competitive advantage.

**Scenario #3:**
An LLM-based customer support chatbot exposes confidential business data, such as internal pricing strategies, discount structures, or customer segmentation details, in its responses. A competitor or malicious user exploits this information to undercut the company's pricing or target high-value clients.

### Reference Links

1. [Lessons learned from ChatGPT's Samsung leak: Cybernews](https://cybernews.com/security/chatgpt-samsung-leak-explained-lessons/)
2. [AI data  leak crisis: New tool prevents company secrets from being fed to ChatGPT: Fox Business](https://www.foxbusiness.com/politics/ai-data-leak-crisis-new-tool-prevents-company-secrets-from-being-fed-to-chatgpt)
3. [Using Differential Privacy to Build Secure Models: Smartnoise Project](https://smartnoise.org/)
4. [Thousands of images on the web are AI-generated. Here's a tool to detect fake ones: The Guardian](https://www.theguardian.com/technology/2023/feb/13/fake-ai-generated-images-detect-tool)
5. [A]() framework for exploring the consequences of AI-mediated enterprise knowledge: Microsoft](https://www.microsoft.com/en-us/research/publication/a-framework-for-exploring-the-consequences-of-ai-mediated-enterprise-knowledge-access-and-identifying-risks-to-workers/)

### Related Frameworks and Taxonomies

- [AML.T0024.000 - Infer Training Data Membership](https://atlas.mitre.org/techniques/AML.T0024.000) — MITRE ATLAS
- [AML.T0024.002 - Infer Training Data](https://atlas.mitre.org/techniques/AML.T0024.002) — MITRE ATLAS
- [AML.T0058 - Model Inversion](https://atlas.mitre.org/techniques/AML.T0058) — MITRE ATLAS
