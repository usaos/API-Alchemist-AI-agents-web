

AI Data Perception Node - Final Production v1.0‌
🚀 Overview

ASI Data Perception Node‌ is an intelligent full-stack scraping engine that dynamically generates and deploys two dedicated microservices—a synchronous scraper and a task-oriented Agent—based on a target URL and natural language extraction requirements.

Frontend AI Orchestrator‌: A pure HTML/JavaScript interface that prompts large language models (Gemini, OpenAI, etc.) to generate customized backend code.
Dual-Service Architecture‌: Produces one synchronous scraper service (simple fetch and parse) and one Agent service for handling asynchronous jobs, command queues, and API discovery.
Multi-Platform Deployment‌: Generates deployment bundles (ZIP) with proper configurations for ‌Vercel‌, ‌Cloudflare Workers‌, or ‌Docker‌ with both .sh and .bat scripts.
✨ Core Features
AI-Powered Code Generation‌: Translates plain language descriptions into production-ready scrapers and agents.
Two Independent Services‌:
scrape‌: Lightweight service focused on web content extraction. Provides automatic data extraction for static and dynamic websites.
agent‌: Advanced service offering /tasks, /agent/commands, OpenAPI discovery, and administrative features.
Comprehensive Security‌:
AES client-side encryption for API keys.
Mandatory compliance agreement for legal scraping.
Automatic robots.txt compliance, rate-limiting, and cache management.
Cross-Platform Deployment‌: One-click deployment scripts for major platforms with environment-based configuration.
Portable & Self-Contained‌: Pure frontend runs entirely in a modern browser; no server required for operation.
History & Template Reuse‌: Local storage for recent tasks and automatic selection of optimal selectors from shared template pools.
🛠️ Prerequisites

Before running, ensure you have:

A modern browser with JavaScript enabled (Chrome/Edge).
A ‌valid API key‌ for one of the supported LLMs:
Google Gemini‌
OpenAI‌
Qwen (Aliyun)‌
ERNIE Bot (Baidu)‌
(For deployment) Access tokens and accounts for your chosen platform (Vercel, Cloudflare, or Docker).
📦 Quick Start

Using this tool involves three main steps:

1. Load the Tool

Simply open the provided index.html file in a modern web browser. A compliance modal will appear; after agreeing, you will see the main interface.

2. Configure & Generate Code
Target URL‌: Enter the public webpage you wish to scrape.
Extraction Requirement‌: Provide a natural language description of the data to extract (e.g., “Extract all news titles, URLs, and publish dates”).
Cron (Optional)‌: Add a periodic scraping schedule (e.g., 0 8 * * * for daily at 08:00).
Deployment Platform‌: Select your target environment.
Add LLM Key‌: Input your LLM API key and select the provider for code generation. The key is encrypted with device-specific AES and saved locally.
Click ‌Generate Dual-Service Project‌. The tool will use your LLM key to generate the scraper and agent source code. View and test the output in the code preview panel.
3. Deploy the Generated Project

Click the ‌⬇️ Download ZIP‌ button to download the complete project bundle, which includes:

表格
File/Folder‌	‌Description‌
api/ or root directory	Contains scrape.py/agent.py (Python) or scrape.js/agent.js (Node.js).
.env.example	Environment variable template for secure keys (token, API keys, TTL, proxy).
requirements.txt (Python) / package.json (Node.js)	Runtime dependencies.
LICENSE	GNU Affero General Public License v3.0 (AGPL-3.0).
README.md	This documentation (English).
deploy.sh / deploy.bat	Automated one-click deployment scripts for Vercel/Cloudflare/Docker.

To deploy:

Unzip the archive, copy .env.example to .env, and configure it.
For Linux/macOS: run chmod +x deploy.sh && ./deploy.sh.
For Windows: double-click deploy.bat.

The script will handle platform‑specific API upload and service start-up, returning a public URL upon success.

⚙️ Service Architecture

The generated project contains two primary services that can run together or independently via a ‌mode switch‌.

表格
Service Name	‌Default Role‌	‌Agent-Mode Role‌
scrape‌	Main entry point serving standard endpoints.	Used as an underlying component for data fetching.
agent‌	Limited functionality (no async endpoints).	‌Fully active‌. Provides /agent/commands, asynchronous task management, KV-based audit logs, and distributed locking.

Toggle Agent Mode‌:

Via ‌request header‌: X-Agent-Mode: true.
Via ‌environment variable‌: AGENT_MODE_ENABLED=1.
📡 API Endpoints (After Deployment)

When deployed, the scraped project exposes the following endpoints, depending on the ‌agent mode‌ being enabled.

Core Scraper Endpoint:‌

POST /scrape – Main extraction logic for the target URL.

Agent Mode Endpoints (Full feature set):‌

/.well-known/ai-plugin.json – OpenAI plugin manifest for AI discovery.
/openapi.json – OpenAPI 3.1 specification (JSON format).
/agent/health – Health check returning service statistics and status.
/agent/commands – Natural‑language command queue.
POST‌ with a command string to submit a new job.
GET‌ (poll) to retrieve the command result.
/tasks – Full asynchronous task management system (submit, query, and cancel tasks).
/tasks/split – Intelligent pagination/directory tree splitting for large‑scale jobs.
/kv (Admin only) – Key‑Value store administration panel.
🔐 Configuration (.env)

Customize generated service behavior by editing environment variables:

表格
Variable‌	‌Description‌	‌Example‌
PLATFORM‌	Target platform for deployment.	vercel, cloudflare, or docker
VERCEL_TOKEN‌ (optional)	Vercel API token for automated deployment.	<your-vercel-token>
CF_API_TOKEN‌ (optional)	Cloudflare API token for Workers deployment.	<your-cf-token>
GEMINI_API_KEY‌ (optional)	Alternative LLM key to avoid passing keys via request headers.	AI...
AGENT_MODE_ENABLED‌	‌Switch:‌ Fully enable agent‑mode endpoints.	1 to activate
MAX_CONCURRENT_SCRAPE‌	Maximum parallel scrapers allowed.	8
TASK_TTL_HOURS‌	Time-to-live for KV task entries in hours.	72
LOG_TTL_DAYS‌	TTL for audit logs in days.	30
TEMPLATE_SOURCES‌	URL list for remote template pools (separated by ,).	https://cn‑templates.example.com
CRON_EXPR‌	Cron expression for periodic runs.	0 8 * * * (daily at 8 AM)
🌍 Local Testing (Development Mode)

To test without deploying to the cloud:

Python (Flask-based projects)
bash
# For scrape-only (Agent Mode disabled)
python local_scrape.py  # Service starts on port 5001

# For full agent capabilities
AGENT_MODE_ENABLED=1 python local_agent.py  # Starts on port 5002

Node.js (Express-based for Cloudflare Workers)
bash
npm install
node scrape.js # Run as standard Node server

🔄 Advanced Usage: CLI Commands (Internal)

The tool also supports command‑line use if the bundle is expanded.

node index.js --help: Show CLI help.
--platform cloudflare: Target platform for pre-deployment verification.
--deploy‑dir ./build: Build assets into a directory for manual upload.
🧑‍💻 Contributing

Contributions and improvements are welcome! Before submitting:

Clone the repository and create a new feature branch.
Ensure all existing functionality works by running the local development server.
Update the LICENSE notice when adding new source files.
Submit a pull request with a clear description of the changes and any new tests.
⚖️ License

This project is distributed under the ‌GNU Affero General Public License v3.0 (AGPL-3.0)‌.

The front‑end orchestrator (index.html) is likewise covered under the same license.
If you modify and run this software on a server accessible over a network, you must make the source code available to your users. See the full LICENSE for details.
📞 Support & Feedback
Report bugs or request features by opening an issue.
For general questions, review the FAQ or check the console logs when generating.
For security‑related issues: Contact the maintainer via a private channel.