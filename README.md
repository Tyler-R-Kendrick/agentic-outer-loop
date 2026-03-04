# agentic-outer-loop

A repository template implementing the GitHub Agentic Outer Loop dev cycle using GitHub Agentic Workflows.

## Features

- 🔒 **Snyk Security Gate** – Automatically scans code on every push and PR using Snyk. Blocks merges when high/critical vulnerabilities are found.
- 🐛 **Automated Issue Creation** – Opens a GitHub issue with full context when Snyk detects vulnerabilities on the main branch.
- 🤖 **Copilot Coding Agent Assignment** – Automatically assigns security issues to GitHub Copilot Coding Agent for remediation.
- 👁️ **Copilot Code Review** – Requests a GitHub Copilot code review on every PR opened by the Copilot Coding Agent.
- 🛠️ **Dev Container** – Pre-configured development environment with all necessary tools and agent skills.
- 🔌 **MCP Servers** – Pre-configured Model Context Protocol servers for GitHub, Snyk, Azure, and Microsoft Learn Docs.

## Prerequisites

- GitHub Copilot license (Copilot Pro or Copilot Enterprise) with Copilot Coding Agent enabled
- Snyk account and API token
- Azure subscription (optional, for Azure MCP server)

## Quick Start

1. **Use this template** – Click "Use this template" to create a new repository from this template.

2. **Configure secrets** – In your repository Settings > Secrets and variables > Actions, add:
   - `SNYK_TOKEN` – Your [Snyk API token](https://app.snyk.io/account)
   - `COPILOT_PAT` – A GitHub [Personal Access Token](https://github.com/settings/tokens) with `repo` scope (needed to assign issues to Copilot Coding Agent)

3. **Configure Copilot environment secrets** – In Settings > Environments > `copilot`, add:
   - `COPILOT_MCP_GITHUB_TOKEN` – GitHub Personal Access Token for the GitHub MCP server
   - `COPILOT_MCP_SNYK_TOKEN` – Snyk API token for the Snyk MCP server

4. **Configure MCP servers for Copilot Coding Agent** – In Settings > Copilot > Coding agent > MCP configuration, add the configuration from [`.vscode/mcp.json`](.vscode/mcp.json) (adapted to the Copilot MCP JSON format; see [GitHub docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/extend-coding-agent-with-mcp)).

5. **Enable Copilot code review** – In Settings > Rules > Rulesets, create a ruleset that enables automated Copilot code review for your default branch (optional, in addition to the workflow-based review).

6. **Open in Dev Container** – Open in VS Code and reopen in the dev container to get all tools and agent skills installed automatically.

## Repository Structure

```
.
├── .devcontainer/
│   └── devcontainer.json          # Dev container configuration
├── .github/
│   ├── workflows/
│   │   ├── snyk-security.yml      # Snyk scan + issue creation + Copilot assignment
│   │   ├── copilot-setup-steps.yml # Copilot Coding Agent environment setup
│   │   └── copilot-review.yml     # Request Copilot code review on agent PRs
│   └── copilot-instructions.md    # Repo-level Copilot instructions
├── .vscode/
│   └── mcp.json                   # MCP server configurations
└── README.md
```

## Workflows

### `snyk-security.yml`

Triggered on every push to `main`/`master` and every PR targeting those branches.

- **On PR**: Runs Snyk code and dependency analysis. Fails the PR check if high/critical vulnerabilities are found.
- **On push to main**: Runs Snyk analysis. If vulnerabilities are found:
  1. Creates a GitHub issue labeled `security`, `bug`, and `copilot`
  2. Assigns the issue to GitHub Copilot Coding Agent

### `copilot-setup-steps.yml`

Runs in the Copilot Coding Agent's sandbox environment before it starts working on an issue. Installs:
- Node.js
- Snyk CLI
- GitHub CLI
- Azure CLI
- Vercel and awesome-copilot agent skills

### `copilot-review.yml`

Triggered when a PR is opened or marked ready for review by `copilot-swe-agent[bot]` or `github-copilot[bot]`, or when labeled `copilot`. Automatically requests a GitHub Copilot code review.

## MCP Servers

The `.vscode/mcp.json` file configures the following MCP servers for use in VS Code:

| Server | Description | Transport |
|--------|-------------|-----------|
| `github` | GitHub API access (issues, PRs, repos, workflows) | stdio (Docker) |
| `snyk` | Security vulnerability scanning and remediation | stdio (npx) |
| `azure` | Azure resource management | stdio (npx) |
| `microsoft-learn-docs` | Official Microsoft documentation ([reference](https://learn.microsoft.com/en-us/training/support/mcp-developer-reference)) | HTTP |

> **Note:** The GitHub MCP server runs via Docker. Ensure Docker is available in your environment.

## Agent Skills

The dev container and Copilot setup workflow automatically install the following agent skill collections:

- **[vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)** – React best practices, web design guidelines, React Native guidelines, composition patterns, and Vercel deploy
- **[github/awesome-copilot](https://github.com/github/awesome-copilot)** – A curated collection of GitHub Copilot skills for various development tasks

## Contributing

See [`.github/copilot-instructions.md`](.github/copilot-instructions.md) for development guidelines.
