<div align="center">

# Astra - AI Pentester

### Autonomous AI pentesting agent specializing in Command Injection vulnerabilities.

Astra analyzes your source code, identifies attack vectors, and executes real exploits to prove vulnerabilities before they reach production. 

**This is the Astra Open Source version: run the full agent locally from your command line.**

</div>

---

## 🎯 Overview

Astra is a customized, agentic AI security tool that operates in multiple distinct phases to emulate a real penetration tester:

1. **Pre-Recon**: Analyzes the application's source code architecture and technology stack.
2. **Recon**: Discovers entry points, routes, and potential injection sinks.
3. **Vulnerability Analysis**: Deeply analyzes the data flow to identify exploitable vulnerabilities.
4. **Exploitation**: Safely executes generated payloads against the target application to confirm the vulnerability.
5. **Reporting**: Compiles a comprehensive final report with executive summaries and detailed technical evidence.

*Note: This specific fork of Astra has been stripped down and highly optimized to focus exclusively on **Injection vulnerabilities** (such as Command Injection).*

## 🚀 Quick Start

### Prerequisites

- **Docker**: Required to run the isolated worker container safely.
- **Node.js 18+ & pnpm**: Required for building and running the CLI in local mode.
- **OpenRouter API Key**: Astra is configured to use OpenRouter to access powerful LLMs (like `nvidia/nemotron-3-ultra-550b-a55b:free` or `inclusionai/ling-3.0-flash-fin:free`).

### 1. Build the Project

Ensure you have installed the dependencies and built the Docker image:

```bash
# Install Node dependencies
pnpm install

# Build the CLI and Worker source
pnpm run build

# Build the isolated Docker worker image
docker build -t astra-worker:latest .
```

### 2. Configure Environment Variables

Astra requires your API key and the selected model to be exported in your terminal before running:

```bash
export ASTRA_AI_API_KEY="sk-or-v1-YOUR-OPENROUTER-API-KEY"
export ASTRA_AI_MODEL="openrouter:inclusionai/ling-3.0-flash-fin:free"
```

*Note: Ensure your OpenRouter API key has sufficient credits or rate-limits for the selected model.*

### 3. Run a Scan

To start an autonomous scan, run the `./astra start` command from the root of this repository. 

Example running against a local target:

```bash
# ASTRA_FORWARD_HOSTS=false is required if you are not using standard docker host networking
ASTRA_FORWARD_HOSTS=false ./astra start \
  -u "https://cmdi.cyberjutsu-lab.tech:3001/" \
  -r "/path/to/target/source-code/" \
  -w "cmdi-scan-01" \
  --models-config "./models.json" \
  --keep-container \
  --follow
```

**Options:**
- `-u, --url`: The target application URL.
- `-r, --repo`: Absolute path to the target's source code directory.
- `-w, --workspace`: A unique name for this scan session. All logs, databases, and reports will be saved in `workspaces/<workspace-name>`.
- `--models-config`: Path to your custom models configuration (e.g., `models.json`).
- `--follow`: Stream the workflow logs to your terminal in real-time.
- `--keep-container`: Keep the Docker container alive after the scan finishes (useful for debugging).

## 📂 Project Structure

- `apps/cli/`: The command-line interface logic.
- `apps/worker/`: The Temporal worker containing the AI agents, prompts, and tool execution logic.
  - `apps/worker/prompts/`: Contains the `.txt` and `.hbs` prompt templates for each AI agent phase.
- `models.json`: Curated list of supported LLM models.
- `workspaces/`: Auto-generated directory containing scan logs, databases, and final reports (ignored by git).

## ⚠️ Disclaimer

**Astra actively executes real exploits.** 
Run this tool ONLY against applications and environments you explicitly own or have written authorization to test. Do not run Astra against production systems. The authors are not responsible for any misuse or damage caused by this software.
# check
