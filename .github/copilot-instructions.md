# GitHub Copilot Instructions

## Repository Overview

This repository is a template for implementing GitHub Agentic Workflows. It demonstrates:
- Automated security scanning with Snyk as a check-in gate
- Automated issue creation and assignment to GitHub Copilot Coding Agent on security failures
- Automated code review by GitHub Copilot on PRs opened by the Copilot Coding Agent

## Development Guidelines

### Security
- All code changes must pass Snyk security analysis before merging
- High and critical severity vulnerabilities must be remediated before merging
- Run `snyk test` and `snyk code test` locally before submitting a PR

### Code Style
- Follow existing patterns and conventions in the codebase
- Keep changes focused and minimal
- Include clear commit messages that describe what changed and why

### Workflow
- Issues labeled `security` and `copilot` are assigned to the Copilot Coding Agent for remediation
- PRs opened by Copilot Coding Agent automatically receive a Copilot code review
- All PRs must pass the Snyk security gate before merging

### MCP Servers Available
The following MCP servers are configured for use in this environment:
- **GitHub**: Read repository info, issues, PRs, and workflow runs
- **Snyk**: Security vulnerability scanning and remediation guidance
- **Azure**: Azure resource management and deployment
- **Microsoft Learn Docs**: Official Microsoft documentation and code samples

### Agent Skills
The following agent skill collections are installed:
- **vercel-labs/agent-skills**: React best practices, web design guidelines, Next.js optimization
- **github/awesome-copilot**: A curated collection of GitHub Copilot skills for various tasks

## Secrets Required
Configure the following secrets in your repository settings:
- `SNYK_TOKEN`: Snyk API token for security scanning
- `COPILOT_PAT`: GitHub Personal Access Token (with `repo` scope) for assigning issues to Copilot Coding Agent

## MCP Server Secrets (Copilot Environment)
Configure the following in Settings > Environments > copilot:
- `COPILOT_MCP_GITHUB_TOKEN`: GitHub Personal Access Token for the GitHub MCP server
- `COPILOT_MCP_SNYK_TOKEN`: Snyk API token for the Snyk MCP server
