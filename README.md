<div align="center">

# 🦆 Deepduck
### The Self-Healing, Graph-Based Autonomous Red Teaming AI

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-Proprietary-red)]()
[![Status](https://img.shields.io/badge/Status-v6.1%20Stable-success)]()
[![Security](https://img.shields.io/badge/Focus-Offensive%20Security-orange)]()

</div>

---

## ⚠️ Legal Disclaimer

> **🛑 ACTIVELY READ BEFORE USE**
>
> This software is for **Educational Purposes** and **Authorized Security Testing** only.
> Usage of **Deepduck** for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state, and federal laws.
>
> The developers assume **no liability** and are not responsible for any misuse or damage caused by this program. Use at your own risk.

---

## 🔍 Overview

**Deepduck** is an advanced, agentic vulnerability scanner designed to simulate a human penetration tester. Unlike traditional linear scanners, Deepduck utilizes a **Knowledge Graph** to understand the topology of a target application and an **LLM-driven Brain** to make strategic decisions.

It features a biological architecture (Cortex, Hippocampus) that allows it to:
1.  **Traverse** complex web applications intelligently (Smart DFS/BFS).
2.  **Assess** vulnerabilities using state-of-the-art context analysis.
3.  **Heal** its own attack scripts upon runtime failure.

### Why "Deepduck"?
Just as a duck looks calm above water but paddles furiously beneath, Deepduck provides a simple interface while managing complex, chaotic attack vectors autonomously in the background.

---

## ✨ Key Features

### 🧠 The Hive Architecture
Deepduck operates on a distributed "Hive" model, orchestrating specialized workers:
-   **Explorer**: Browses the web, parses DOM, and builds the Knowledge Graph.
-   **Tactician**: Analyzes HTTP traffic and source code to identify theoretical vulnerabilities.
-   **Blacksmith (The Forge)**: Dynamically crafts custom Python exploits for unique business logic flaws.

### 🛡️ Hybrid Arsenal
-   **Standard Tools First**: Seamlessly integrates industry-standard tools (`SQLMap`, `Nuclei`, `FFUF`) for reliable detection.
-   **AI Scripting Fallback**: When standard tools fail, the AI generates, validates, and executes custom exploit scripts in a sandboxed environment.

### 🏥 Self-Healing Capabilities
Deepduck creates its own tools. If a generated script fails (e.g., syntax error, outdated library), the **Surgeon** module engages a feedback loop to:
1.  Analyze the traceback.
2.  Rewrite the code.
3.  Retry the attack (up to 3 times).

### 📊 Smart Reporting
Generates comprehensive Markdown reports detailing:
-   Attack Surface Map (Graph Topology).
-   Vulnerability Findings (Severity, Evidence).
-   Execution Logs.

---

## 🔒 Security & Core Logic
**Note:** Some core modules regarding the **Prompt Engineering** and **Cortex Logic** are purposely redacted or omitted in the public release to prevent abuse by malicious actors (`Script Kiddies`).

This repository contains the framework skeleton and safe standard modules. The core "Brain" logic is available only to authorized enterprise partners.

---

## 🏆 Achievements & Benchmarks
We track our system's performance against known vulnerable apps (e.g., testphp.vulnweb.com, OWASP Juice Shop).

👉 **[View ACHIEVEMENTS.md](./ACHIEVEMENTS.md)** (Performance Metrics)

---

## 📥 Installation & Usage

### Prerequisites
-   Python 3.10+
-   Chrome & ChromeDriver (for Headless Browser)
-   Valid API Keys (OpenAI / Anthropic / Solar)

### Setup
```bash
# Clone the repository
git clone https://github.com/your-username/deepduck.git

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your API keys
```

### Running the Engine
```bash
python main.py explore https://target.com --mode normal
```

---

## 🤝 Contribution & Sponsorship

### 🚫 Contribution Policy
**We are NOT accepting code contributions or Pull Requests at this time.**
The codebase is currently undergoing rapid architectural changes to establish a stable foundation. We aim to open-source more components once the core architecture is solidified.

### 💖 Sponsor Us
If you believe in the future of Autonomous Security Agents, consider organizing a sponsorship. Your support helps maintain the infrastructure and API costs required for development.

---

## 📧 Contact & Business

We provide **Security Audit Services** utilizing the full capabilities of Deepduck (Unrestricted Version).

If you represent an organization and wish to:
-   Request a Proof of Concept (PoC).
-   Schedule a Penetration Test.
-   discuss Enterprise Licensing.

**Contact us at:** `---@gmail.com` (Email placeholder)

---

<div align="center">
  <i>Built with 💻 and ☕ by the Deepduck Team.</i>
</div>
