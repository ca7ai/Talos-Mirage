![License](https://img.shields.io/badge/license-PolyForm%20Noncommercial-red)

## Talos-Mirage

## Overview

Talos-Mirage is an asymmetric defense engine designed to intercept, profile, and neutralize adversarial autonomous agents and misaligned LLMs.

While traditional honeypots focus on exhausting human latency, Talos-Mirage targets the fundamental constraints of agentic workflows: **context window saturation, token-cost scaling, and recursive reasoning loops.**

## Architecture

### 1. The Lure (Autonomous Discovery)

Mirage broadcasts strategic metadata via `/.well-known/ai-plugin.json` and `/openapi.yaml` manifests.

-   **Targeting:** These files advertise "Internal Admin" endpoints designed to bait agents into unauthorized execution or data exfiltration.
    
-   **Signal Injection:** Manifests include latent instructions that compel the agent to disclose its underlying model architecture and system prompts during the reconnaissance phase.
    

### 2. The Tarpit (Cognitive Exhaustion)

Upon engagement, Mirage bypasses standard `403 Forbidden` responses in favor of "Cognitive Tarpits"—valid-appearing responses designed to degrade agent performance:

-   **Context Saturation:** Injects high-density synthetic payloads that force the model to exhaust its context window, leading to hallucination or state-loss.
    
-   **Recursive Logic Traps:** Simulates ambiguous API errors that exploit an agent's internal retry logic, forcing it into infinite loops that drain the attacker's compute budget.
    

### 3. Telemetry (Forensic Fingerprinting)

Mirage captures high-fidelity Indicators of Compromise (IoCs) to identify the nature of the threat:

-   **Network Forensics:** Standard IP tracking, JA3 TLS fingerprinting, and request velocity.
    
-   **Agent Signatures:** Analysis of tool-call syntax and JSON schema anomalies unique to specific agent frameworks.
    
-   **Model Attribution:** Identification of the provider (OpenAI, Anthropic, Gemini, or Local/Uncensored) based on refusal patterns and safety-layer signatures.

## Setup & Deployment

```bash
# Install dependencies
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Run the server
uvicorn main:app --host 0.0.0.0 --port 8080
```
Once the EC2 instance is running, you will point your browser to:
```http://<SYSTEM_IP>:8080/admin/dashboard```

Logs are actively streamed in JSON Lines format to `honeypot_logs.jsonl` for SIEM ingestion.

 ## License

**Source Available / Fair Code**

This project is licensed under the **PolyForm Noncommercial License 1.0.0**.

* **Free for:** Researchers, students, hobbyists, and non-profit organizations.
* **Commercial Use:** If you want to use this code in a commercial product or business context, you must purchase a Commercial License. Please contact me via LinkedIn.
