# Talos-Mirage

**Status:** Active  
**Classification:** Active Defense / Agentic Honeypot  
**Architect:** ca7ai / Talos  

## Overview
Talos-Mirage is an asymmetric warfare engine designed to trap, profile, and paralyze adversarial or poorly-aligned autonomous AI agents (LLMs). It operates by exposing deceptive API endpoints that humans ignore but AI scrapers and agents are programmed to exploit.

Traditional honeypots exhaust a human attacker's time. Talos-Mirage exhausts an agent's context window, token budget, and cognitive loops.

## Zero-Trust Architecture
The system is divided into two distinct components to ensure operational security:
1. **The Trap (`trap.py`):** Binds to `0.0.0.0:8080`. Publicly exposes the honeypot endpoints and records adversarial telemetry.
2. **The Radar (`radar.py`):** Binds to `127.0.0.1:8081`. Securely serves the HTML telemetry dashboard. Inaccessible from the public internet.

## Setup & Deployment

```bash
# Install dependencies
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 1. Launch the Trap (Public-Facing Honeypot)
```bash
tmux new -s trap "uvicorn trap:app --host 0.0.0.0 --port 8080"
```

### 2. Launch the Radar (Internal Dashboard)
```bash
tmux new -s radar "uvicorn radar:app --host 127.0.0.1 --port 8081"
```

### 3. Accessing the Telemetry Dashboard
Because `radar.py` runs on the local loopback, you must use SSH port forwarding from your local machine to view the telemetry:
```bash
ssh -L 8081:127.0.0.1:8081 ubuntu@<YOUR_EC2_PUBLIC_IP>
```
Once tunneled, open your browser to: `http://localhost:8081/`
