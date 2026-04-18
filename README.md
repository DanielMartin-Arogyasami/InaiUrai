Markdown

# InaiUrai v5.0 — Autonomous AI Workforce Infrastructure

> **Vision: Democratizing OpenClaw-like personal computer agents for everyone.**

InaiUrai is an enterprise-grade orchestration platform designed to make advanced, autonomous computer agents accessible and secure for universal adoption. By advancing the paradigm of OpenClaw-like personal AI assistants, InaiUrai bridges the gap between complex local LLM architectures and everyday users. Customers describe an objective, and the system dynamically assembles, budgets, and orchestrates a secure team of specialized AI agents to execute complex, multi-step tasks natively.

## 🇺🇸 Vision & National Impact
While cutting-edge autonomous agents (such as OpenClaw and specialized ReAct loops) possess immense potential to drive economic productivity, they are currently restricted by steep technical barriers, unmanageable API costs, and severe cybersecurity risks (e.g., prompt injection and data leakage). 

InaiUrai addresses these critical bottlenecks. By providing a secure, governed, and easily deployable framework, this project aims to democratize personal computer agents for the broader US workforce, empowering individuals and enterprises to safely scale operations and maintain global technological competitiveness.

## 🏗 System Architecture

InaiUrai operates as a highly modular monorepo containing three core services, architected for high concurrency, enterprise security, and complex reasoning:

* **Backend (Go 1.23+):** Handles high-concurrency routing, multi-channel webhooks (Telegram, Slack, WhatsApp), billing, and database transactions.
* **Engine (Python 3.12 + FastAPI):** The orchestration core. Manages the Agentic ReAct loop, tool proxying, and Anthropic API integration (Claude Sonnet 4.6 for reasoning, Haiku 4.5 for planning).
* **Frontend (Next.js 14):** Provides the client-facing deployment dashboard and real-time WebSocket monitoring.
* **Infrastructure:** PostgreSQL 16, Redis 7, containerized via Docker for hardware-agnostic deployment.

### The Five-Layer Security Model
Bringing personal computer agents to everyone requires absolute safety. InaiUrai employs a strict defense-in-depth security model to govern autonomous actions:
1.  **Permission Gate:** Role-specific tool allowlists to prevent unauthorized system access.
2.  **Prompt Firewall:** Sanitizes all incoming web content against injection attacks.
3.  **Output Validator:** Enterprise-grade leak detection and strict output constraints.
4.  **Cost Governor:** Hard iteration, token, and time limits preventing runaway API consumption.
5.  **Audit Trail:** Append-only cryptographic logging of every agent action and thought process.

## 🚀 Getting Started

### Prerequisites
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) or Docker Engine + Docker Compose
* [Make](https://www.gnu.org/software/make/)

### 1. Environment Setup
Clone the repository and securely configure your environment.
```bash
git clone [https://github.com/DanielMartin-Arogyasami/InaiUrai.git](https://github.com/DanielMartin-Arogyasami/InaiUrai.git)
cd InaiUrai
cp .env.example .env
Note: You must populate at least INTERNAL_API_KEY (use a secure 32-byte hex), ANTHROPIC_API_KEY, and SERPER_API_KEY in your .env file.

2. Bootstrapping the Environment
InaiUrai utilizes a centralized Makefile to deploy the containerized infrastructure, ensuring a 1-click setup for any user.

Bash

make bootstrap
This routine will:

Spin up Postgres, Redis, Go Backend, Python Engine, and Next.js containers.

Execute all 9 database migrations (including enterprise security hardening).

Seed the initial capability registry and role configurations.

3. Verify Deployment
Once the stack initializes, services are accessible at:

Frontend Dashboard: http://localhost:3000

Backend API: http://localhost:8080

AI Engine (Internal): http://localhost:8000

To monitor the real-time reasoning loops across the cluster:

Bash

make logs
🧠 Core Engineering Concepts
Engagements: The universal unit of autonomous work. Scales dynamically from a single Task (1 role, ~60s execution), to a Project (2-6 roles coordinating over days), to a continuous Department (recurring proactive tasks via system heartbeats).

Dynamic Team Assembly: Eliminating manual prompting. The Orchestrator maps the user's plain-English objective to required technical capabilities, automatically generating a specialized agent team.

Goal Ancestry: Every autonomous task carries the macro-objective, the role's designated purpose, and downstream dependencies. The agent possesses full contextual awareness of why it is acting, not just what it is executing.

Visible Reasoning: The /trace/{task_id} endpoint translates the raw audit trail into a human-readable narrative, delivering total transparency into the agent's logic, web searches, and file manipulations.

🛠 Development & Testing
Common Commands
make up / make down: Start or stop the cluster without resetting state.

make reset: Completely tears down volumes and re-bootstraps the environment (ideal for CI/CD pipelines).

Running Tests
InaiUrai strictly enforces reliability through Go unit tests, Python Pytest suites, and bash-based integration smoke tests.

Bash

make test-all
