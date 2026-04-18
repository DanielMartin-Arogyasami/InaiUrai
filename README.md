```markdown
# InaiUrai v5.0 — AI Workforce-as-a-Service

> **"Hire AI employees. Describe the outcome. We deliver."**

InaiUrai is an enterprise-grade platform that dynamically assembles, orchestrates, and manages teams of AI executives. Instead of users manually prompting individual LLMs, customers describe a business objective. InaiUrai's orchestrator analyzes the request, proposes a specialized team from a catalog of 16 roles, and executes the work through a secure, budgeted, and fully auditable ReAct loop.

## 🏗 System Architecture

InaiUrai operates as a monorepo containing three core services, designed for high concurrency, security, and complex AI reasoning:

* **Backend (Go 1.23+):** Handles high-concurrency routing, multi-channel webhooks (Telegram, Slack, WhatsApp), billing, and database transactions.
* **Engine (Python 3.12 + FastAPI):** The "brains." Manages the Agentic ReAct loop, tool orchestration, and Anthropic API integration (Claude Sonnet 4.6 for reasoning, Haiku 4.5 for planning).
* **Frontend (Next.js 14):** Provides the client-facing dashboard and realtime WebSocket connections.
* **Infrastructure:** PostgreSQL 16, Redis 7, containerized via Docker.

### The Five-Layer Security Model
Because agents have tool access, InaiUrai employs a strict defense-in-depth security model:
1.  **Permission Gate:** Role-specific tool allowlists.
2.  **Prompt Firewall:** Sanitizes all incoming web content.
3.  **Output Validator:** Leak detection and output constraints.
4.  **Cost Governor:** Strict iteration, token, and time limits per-role.
5.  **Audit Trail:** Append-only logging of every agent action and thought process.

## 🚀 Getting Started

### Prerequisites
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) or Docker Engine + Docker Compose
* [Make](https://www.gnu.org/software/make/)

### 1. Environment Setup
Clone the repository and set up your local environment variables.
```bash
git clone [https://github.com/DanielMartin-Arogyasami/InaiUrai.git](https://github.com/DanielMartin-Arogyasami/InaiUrai.git)
cd InaiUrai
cp .env.example .env
```
*Note: You must populate at least `INTERNAL_API_KEY` (use a secure 32-byte hex), `ANTHROPIC_API_KEY`, and `SERPER_API_KEY` in your `.env` file for the engine to boot successfully.*

### 2. Bootstrapping the Environment
InaiUrai uses a centralized Makefile to handle Docker composition, database migrations, and initial data seeding.

```bash
make bootstrap
```
This command will:
1. Spin up Postgres, Redis, Go Backend, Python Engine, and Next.js containers.
2. Run all 9 database migrations (including enterprise hardening).
3. Seed the initial data and role configurations.

### 3. Verify Deployment
Once the stack is ready, the services will be available at:
* **Frontend:** `http://localhost:3000`
* **Backend API:** `http://localhost:8080`
* **AI Engine (Internal):** `http://localhost:8000`

To view real-time logs across all services:
```bash
make logs
```

## 🧠 Core Concepts

* **Engagements:** The universal unit of work. Engagements scale from a single **Task** (1 role, ~60s), to a **Project** (2-6 roles, days), to a continuous **Department** (recurring tasks via heartbeats).
* **Dynamic Team Assembly:** Humans do not pick roles. The Orchestrator maps the objective to the required capabilities and generates a team.
* **Goal Ancestry:** Every task carries the macro-objective, the role's purpose, and downstream dependencies. The agent always knows *why* it is working, not just *what* it is doing.
* **Visible Reasoning:** The `/trace/{task_id}` endpoint translates the raw audit trail into a human-readable narrative, showing exactly what the AI thought, searched, and executed.

## 🛠 Development & Testing

### Common Commands

* `make up` / `make down`: Start or stop the cluster without migrating.
* `make reset`: Completely tears down the database volumes and re-bootstraps the environment. Ideal for clean-slate testing.

### Running Tests
InaiUrai includes Go unit tests, Python Pytest suites, and bash-based smoke tests. To run the full suite:

```bash
make test-all
```

## 📂 Repository Structure
```text
InaiUrai/
├── backend/            # Go 1.23 API, Webhooks, Models, and Channel Infra
├── engine/             # Python 3.12 Engine, ReAct Loop, Security Firewall
├── frontend/           # Next.js 14 Client Application
├── db/                 # SQL Migrations and Seed files
├── docker-compose.yml  # Service definitions
└── Makefile            # Build and lifecycle automation
```
```
