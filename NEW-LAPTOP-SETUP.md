# New laptop setup and migration prompt

Prepared on 2 November 2026 for this repository:
https://github.com/Raghavendra1204/paypal-ai

## Copy this prompt into Codex on the new laptop

Help me recreate my PayPal AI development environment on this laptop. Inspect this machine first, reuse working installations, and complete the setup and verification below. Ask me only for missing account access, required credentials, or decisions you cannot infer. Never print credentials or commit them to Git. Do not claim that installing a plugin proves its MCP tools work: verify the connection separately.

### 1. Install and check the development tools

- Install the Codex desktop app and sign in with my account.
- Install the standalone Codex CLI if it is missing. The old laptop used `npm install -g @openai/codex@latest`. Verify current official installation guidance and the installed CLI's supported commands before proceeding.
- Install a supported Node.js LTS release with npm, Git, and VS Code if needed.
- Confirm `node --version`, `npm --version`, `git --version`, and `codex --version` work in a regular terminal, not only in Codex's bundled environment.
- Make sure executable folders are on this laptop's PATH. Do not copy old executable paths or assume the username or drive letters match.
- Authenticate Git with my GitHub account using the normal browser/credential-manager flow. Do not request a personal access token in chat.

### 2. Clone the project and install its dependencies

- Clone `https://github.com/Raghavendra1204/paypal-ai.git` into a folder I choose and open it in Codex and VS Code.
- The initial project was pushed on branch `master`. Inspect the current remote/default branch instead of assuming it is `main`.
- Inspect repository instructions, package manifests, and existing code before editing anything.
- If `package-lock.json` exists and matches `package.json`, run `npm ci`. Otherwise explain the mismatch and repair only what is necessary.
- Run `npm ls ag-studio-react` to verify installation.
- The dependency recorded during migration is `ag-studio-react` with range `^3.0.0`; the lockfile records resolved versions. It brings in AG Studio/AG Grid dependencies. Let npm restore those; do not manually upload or copy `node_modules`.
- At migration time this repository contained dependency manifests and a Git ignore file, not a scaffolded React application. Do not invent a dev server command or assume a React UI already exists. Inspect the current repository for changes since then.
- APIMatic was discussed but not installed or integrated. Do not install it automatically as though it were an existing dependency.

### 3. Reinstall the PayPal Codex plugin

The old laptop installed PayPal from the `paypal/AI-Toolkit` marketplace. These commands succeeded there:

```powershell
codex plugin marketplace add paypal/AI-Toolkit
codex plugin add paypal@paypal-ai-toolkit
```

- Check the new CLI's help/current documentation if syntax has changed.
- The old installed plugin version was `1.2.0`; verify the version installed here and read its current setup instructions.
- The available skills included `paypal-routing` and `paypal-best-practices`.
- Reinstall the plugin through the supported installation flow rather than copying its cache directory.
- Read the plugin's setup instructions, adapting client-specific instructions to Codex. Do not configure Claude settings for a Codex setup.

### 4. Recreate and verify the local PayPal MCP connector

The successful fallback on the old laptop used the official local `@paypal/mcp` package with `--tools=all`, in sandbox mode. The connector was named `paypal-sandbox`. Remote `/sse` and `/http` attempts returned errors in our tests; do not assume those endpoints currently work without testing.

- Consult current official PayPal MCP documentation before choosing local versus remote transport.
- For the local route, install `@paypal/mcp` into a stable user-owned tools folder, separate from this project's application dependencies. Avoid relying on a temporary npm cache directory.
- Discover this laptop's actual Node executable and the installed package entry point.
- Configure a stdio MCP server with sandbox mode. The package expects `PAYPAL_ACCESS_TOKEN` and `PAYPAL_ENVIRONMENT=SANDBOX`.
- The old setup stored the token under the Windows user variable `PAYPAL_SANDBOX_ACCESS_TOKEN`, then mapped it to `PAYPAL_ACCESS_TOKEN` in a launcher. Recreate that mapping or configure an equivalent secure arrangement.
- Explicitly allow the required token variable into the MCP subprocess if Codex filters inherited environment variables. Do not assume that a token available to the terminal is automatically available to MCP.
- Use a stable launcher only if needed. Prefer straightforward process startup over nested shell quoting and npm downloads on every launch.
- If using a Node wrapper, launch the package as its own CLI process with stdin/stdout inherited. Merely importing a package that checks `require.main === module` does not run its CLI entry point.
- Allow sufficient MCP startup time; the old configuration used `startup_timeout_sec = 60`.
- Verify MCP initialization, tools/list, and one read-only invoice list request with a small page size. A prior successful test exposed 28 tools; this is historical context, not a required fixed count.
- Do not create invoices, orders, payments, refunds, or other transactions merely to test setup.
- Restart the connection or start a new Codex chat if required, then check that tools are actually callable in that chat. A successful separate test alone does not prove the current chat loaded them.
- Keep the environment sandbox-only unless I explicitly ask for production.

### 5. Configure fresh sandbox credentials locally

- My sandbox app and sandbox data belong to my PayPal account and remain online. They do not need migrating as local files.
- Use my sandbox app's matching client ID and client secret to request a new OAuth access token. A single client ID or secret is insufficient.
- Ask me to enter secrets through a local prompt or supported secret storage, not into chat or a committed file.
- The token request uses Basic authentication and form data `grant_type=client_credentials` at `https://api-m.sandbox.paypal.com/v1/oauth2/token`.
- Save only through the chosen local configuration mechanism. Never echo the token or put it in Git, command examples, screenshots, or documentation.
- Use `expires_in` from the response for the actual lifetime; do not assume exactly nine hours.
- The previous setup used manually renewed tokens. Automatic renewal was not implemented. Renewing a token does not require reinstalling plugins.
- Reconnect/restart the MCP process after updating the saved token so it reads the new value.
- Client secrets previously pasted into chat should be replaced before reuse.

### 6. Restore other Codex capabilities selectively

The old laptop had configuration entries for these plugins/capabilities. Entries alone do not establish enabled or working status. Check what the new app already supplies before installing anything:

- OpenAI bundled: app tools, browser, unified computer use, computer use, and visualizations.
- OpenAI primary runtime: documents, PDF, spreadsheets, presentations, and template creator.
- PayPal: `paypal@paypal-ai-toolkit`.
- Other available capabilities included Sites, plugin management, and Work Pets. Availability may depend on the app, account, permissions, or platform. Reconnect through supported settings if needed; do not copy internal runtime packages, account tokens, or IPC configurations.
- Built-in/system skills such as OpenAI Docs, image generation, skill creation, and skill installation should come from the new installation.
- Do not copy machine-generated internal MCP entries, sockets, process IDs, or bundled runtime paths from the old laptop.

The old laptop also had optional personal skills under `~/.agents/skills`. These are not npm project dependencies. Their original installation sources were not verified. If I want them migrated, identify trusted sources or transfer their instruction/resource folders privately after reviewing contents, licenses, executable scripts, and embedded secrets. Do not silently fetch similarly named skills from unverified repositories.

Personal skill inventory:

```text
ai-engineer
api-design-principles
api-testing-observability-api-mock
application-performance-performance-optimization
backend-architect
backtesting-frameworks
bats-testing-patterns
code-review-ai-ai-review
code-review-excellence
code-reviewer
data-engineer
database-admin
database-architect
debugger
debugging-toolkit-smart-debug
error-debugging-error-analysis
error-debugging-error-trace
error-debugging-multi-agent-review
error-diagnostics-smart-debug
fastapi-pro
fastapi-templates
find-skills
frontend-developer
frontend-security-coder
git-advanced-workflows
git-pr-workflows-git-workflow
gitops-workflow
nextjs-app-router-patterns
performance-testing-review-ai-review
performance-testing-review-multi-agent-review
postgresql
prompt-engineer
rag-implementation
react-modernization
react-native-architecture
react-state-management
security-requirement-extraction
security-scanning-security-dependencies
tailwind-design-system
ui-ux-designer
ui-visual-validator
web3-testing
```

These optional skill contents are not included in this repository by this migration guide. If I need exact copies, arrange a separate reviewed transfer from the old laptop.

### 7. Windows protection and permissions

- Windows Defender Controlled Folder Access blocked Node.js from writing to the old project's Documents folder. Check Defender events if npm stalls or file creation fails unexpectedly.
- Do not disable security protection. Explain the blocked executable and let me choose a narrow app allowance or a suitable project location.
- Investigate the actual error before recommending administrator access, repeated installs, or computer restarts.
- Use bounded network timeouts and inspect npm logs if downloads stall. Do not report a running install as successful.

### 8. Complete a migration report

Report what was installed, what was already present, what was restored from Git, which plugins/tools actually work, and any remaining user action. Specifically distinguish:

1. Project dependency installation.
2. Codex plugin/skill availability.
3. PayPal MCP startup and tools discovery.
4. Sandbox authentication and read-only API verification.
5. Optional personal skills that were not transferred.

Do not claim app functionality that has not been built or tested.

## What belongs in Git

Commit source code, documentation, `package.json`, `package-lock.json`, reviewed portable scripts, and credential-free examples. Do not commit `node_modules`, npm caches, `.env` secrets, access tokens, Codex authentication state, or the entire personal Codex configuration directory.

## Old-laptop locations: reference only

These paths describe the old machine and must not be copied unchanged into the new configuration:

- Project: `C:\Users\Dell\Documents\ChatGPT\paypal ai`
- Personal Codex configuration: `C:\Users\Dell\.codex\config.toml`
- Local connector launcher: `C:\Users\Dell\.codex\paypal-mcp-start.cjs`
- Optional personal skills: `C:\Users\Dell\.agents\skills`
- Old Node executable was on a different drive; discover the new installation instead.
- The old PayPal launcher referenced an npm cache directory. Reinstall into a stable tools folder on the new machine rather than recreating that cache path.

This document contains a migration prompt and inventory, not an automatic installation script or a backup of local credentials, chat history, plugin caches, or skill contents.

## References

- [Repository](https://github.com/Raghavendra1204/paypal-ai)
- [Codex local configuration](https://learn.chatgpt.com/docs/config-file/config-basic)
- [PayPal MCP setup](https://developer.paypal.com/ai-tools/mcp-server)
- [PayPal authentication](https://developer.paypal.com/api/rest/authentication)
