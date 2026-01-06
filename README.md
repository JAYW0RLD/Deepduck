Technical Architecture 🏗️
Deepduck is a Graph-based Autonomous Payload-Driven Vulnerability Scanner.
Unlike traditional linear fuzzers, it leverages a Knowledge Graph and Large Language Models (LLMs) to learn site structures and dynamically generate context-aware payload attack scenarios.
The core philosophy is "The Flock" — a collaborative swarm intelligence model where specialized workers operate asynchronously to conquer targets.
1. System Overview

Input: Single target URL (e.g., http://example.com)
Output: Markdown report (discovered vulnerabilities, payloads used, success rates, coverage map)
Dual Mode Architecture:
Safe Mode → For real-world site owners (blocks destructive payloads, enforces rate limiting, requires ownership verification)
Full Mode → For vulnerable labs (HTB, PortSwigger, pentest-ground.com) with unrestricted payload execution


2. Core Architecture: The Flock
A Priority Task Queue orchestrates three specialized workers in an asynchronous loop.
🧬 Central Nervous System

Task Queue
Central control hub managing all tasks (crawling, analysis, attacks)
Uses PriorityQueue: High-priority (e.g., login form analysis, P=3) over low-priority (e.g., link collection, P=5)

Knowledge Graph
In-memory graph database powered by NetworkX
Nodes: URLs, forms, parameters, DOM elements
Edges: link_click, form_submit, redirect, parameter_injection
Builds a comprehensive site map to avoid redundant exploration


🕵️ Explorer Worker

Role: Site structure discovery and graph expansion
Technologies: Selenium (Headless Chrome), BeautifulSoup, DOM Hashing
Logic:
Visits URLs → Parses DOM → Extracts links, forms, and injectable parameters
Anti-Rabbit Hole: compute_dom_hash() prevents infinite loops on structurally identical pages (e.g., pagination, calendars)
Enforces scope.yaml policies (only in-scope domains added to queue)


🧠 Assessment Worker (The Tactician)

Role: Vulnerability potential assessment and attack vector identification
Technologies: LLM integration (e.g., GPT-4o, Claude-3.5, or local models) + Advanced Prompt Engineering
Logic:
Feeds collected DOM, forms, and parameters to the LLM
LLM analyzes for vulnerability likelihood (SQLi, XSS, Command Injection, IDOR, etc.)
If exploitable, issues an ATTACK task to the queue with prioritized context
Context-Aware: Considers URL parameters, hidden fields, and graph relationships


⚔️ Attack Worker (The Forge)

Role: Actual exploitation execution
Technologies: Hybrid engine with standard tools + custom scripting
Logic (Hybrid Exploitation Engine):
Decision Phase: LLM selects optimal tool based on context
Standard Mode: Wraps proven tools (SQLMap, Nuclei, FFUF) for reliability (ENABLE_FORGE=false forces this)
Forge Mode (ENABLE_FORGE=true): LLM dynamically crafts and executes custom Python payloads for complex cases (e.g., business logic bypass)
Self-Healing Reflection Loop: On error, LLM analyzes logs, patches code, and retries


3. Key Technologies & Features
🛡️ The Hippocampus (Skill Registry)

Stores successful payloads, tool configurations, and outcomes in registry.json
Enables "experience learning" — reuses proven skills on similar nodes, tracks success rates, and prunes low performers

🔥 Forge Safety & Evasion

Dynamic script loading in sandboxed environment (importlib)
Built-in WAF evasion (auto-applies tamper scripts like space2comment on detection)
Safety Firewall: Blocks high-risk operations (e.g., os.system, subprocess) during code generation

📊 Reporting System

Auto-generates Markdown reports on session completion
Includes vulnerabilities, exploit details, graph coverage, and timeline

4. Data Flow Summary

Input → Explorer builds initial graph
Explorer → Assessment analyzes nodes → Queues attack tasks
Assessment → Attack executes payloads → Marks vulnerable nodes
Results → Hippocampus update → Final report generation
