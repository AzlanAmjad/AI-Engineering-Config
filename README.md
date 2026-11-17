# AI Engineering Config

Personal Copilot customizations, versioned once and reused across machines and cloud sandboxes.

## Layout

| Source | Purpose |
| --- | --- |
| [`.copilot/copilot-instructions.md`](.copilot/copilot-instructions.md) | Always-on Caveman Mode engineering guidance |
| [`.copilot/agents/`](.copilot/agents/) | Ember, Principal Software Engineer, Prompt Engineer, and Code Reviewer |
| [`.copilot/instructions/`](.copilot/instructions/) | Modular Context7 guidance |
| [`.copilot/skills/`](.copilot/skills/) | Reusable Context7 documentation workflow |
| [`mcp/local.json`](mcp/local.json) | Sanitized local MCP definitions to merge into user configuration |
| [`mcp/cloud.json`](mcp/cloud.json) | Cloud-compatible MCP template for GitHub repository settings |
| [`mcp/.env.example`](mcp/.env.example) | Required variable names, never actual secrets |
| [`AGENTS.md`](AGENTS.md) | Small, environment-aware installation contract for an agent |

The Git-tracked files are authoritative. `.copilot/` contains real native files, not generated copies or links to a separate shared tree. There is no installer script, generator, or build step.

## Install on a machine

Clone this repository to a stable location, open it with an agent, and request:

> Install the customizations in this repository for my installed Copilot clients. Follow AGENTS.md. Detect the execution environment, preserve unrelated configuration, and ask before replacing conflicting content.

The agent discovers the appropriate native destinations for the actual host, clients, and any `COPILOT_HOME` override. It links individual managed files and skill directories, not the whole Copilot home. If symlinks are unavailable, it copies and reports the refresh requirement.

MCP setup is optional. Before enabling servers, select which ones you want and complete the secret/authentication setup below. An existing literal credential must never be copied into this repository.

Existing configuration is compared first. Correct links/content are left alone; conflicting files or MCP entries require confirmation and an external backup. Old active copies, such as Caveman Mode under modular instructions or Context7 under another skills root, must be reconciled so the same customization is not loaded twice.

### Update and recover

- Pull reviewed changes into each clone with `git pull`. Correct file/directory links see content changes; newly added or removed files still need the agent to reconcile installation.
- In copy mode, ask the agent to refresh managed copies after pulling.
- Edit source files here, review the diff, and commit/push intentionally. Git does not provide automatic live synchronization between machines.
- Restart or create a fresh session to verify instruction changes. Client refresh behavior differs.
- To undo installation, remove only the links/copies identified by the installation report and restore backed-up originals. Restore only affected MCP entries; preserve unrelated changes.

Do not store populated env files or backups in this checkout, even if ignored. Do not put runtime configuration, session history, or OAuth/token stores under the tracked source tree.

## Client support

| Client/environment | Strategy |
| --- | --- |
| VS Code Chat, current Copilot harness | Native Copilot user instructions, agents, skills, and MCP configuration on the execution host |
| VS Code Agents window, Copilot harness | Same native user sources; verify the selected session's host and harness |
| Copilot CLI | Same native user sources; respect configured home overrides |
| Copilot cloud sandbox | GitHub copies committed dotfiles into home on creation/resume; no installer |
| Actions-based Copilot session | Personal dotfiles do **not** apply; project/org configuration is a separate concern |
| Deprecated VS Code Local harness | Conditional compatibility only when detected and requested; profile storage and MCP formats differ |

Selecting a model does not select a harness. Current Copilot harness clients share native locations, but tool inventories, authentication state, and supported agent fields can differ. These user folders do not roam through VS Code Settings Sync; this repository supplies synchronization.

There are no prompt files to migrate. Use skills for reusable workflows in the current Copilot harness rather than adding `.prompt.md` files it does not load. Generated VS Code synchronization plugins and built-in skills are not source files to copy here.

### Imported customization adjustments

- Caveman Mode is now always-on rather than depending on modular discovery.
- Principal Software Engineer uses a host-neutral interactive question-tool reference.
- Prompt Engineer provides a concise assessment and improved prompt, not a private reasoning transcript.
- Ember retains its collaborative persona but omits named personal anecdotes and its unavailable external story-skill dependency. It does not invent real experiences or infer personal facts from typing style.
- Context7 instructions and skill retain their roles, use a shared per-question call budget, and report unavailable tools. Queries must not contain secrets, private code, or confidential context.
- Code Reviewer is an original reviewer for code changes and architecture. It can be selected directly or delegated a review; it does not depend on an orchestrator. It uses read-only inspection by default and requires explicit permission for checks that may change state. Terminal access is not an enforced read-only sandbox.

## Local MCP

[`mcp/local.json`](mcp/local.json) uses the portable `mcpServers` schema:

| Server | Authentication | Enabled tools |
| --- | --- | --- |
| `context7` | API key via `COPILOT_MCP_CONTEXT7_API_KEY` | `resolve-library-id`, `query-docs` |
| `notion` | Interactive OAuth against Notion's hosted MCP service | All server tools, including write capabilities; enable only if wanted |

The installation agent merges selected entries into the native user MCP configuration (normally `~/.copilot/mcp-config.json`, subject to host discovery and overrides). It must not replace unrelated servers or symlink that entire mutable file into this repository. Built-in GitHub and Playwright capabilities are not duplicated.

### Secrets

1. Use [`mcp/.env.example`](mcp/.env.example) as the variable-name contract. Obtain a Context7 API key through its dashboard; never paste it into agent chat.
2. Supply `COPILOT_MCP_CONTEXT7_API_KEY` through your local credential/environment mechanism. If using a populated env file, keep it outside this checkout with appropriate access restrictions.
3. Explicitly load it into the environment that launches Copilot, VS Code, or the relevant Agent Host on the execution machine. Copilot does not automatically read `.env` files; the portable format has no `envFile` or editor secret-input bridge.
4. Restart the affected host/client if necessary. Exporting a variable in a separate terminal does not update an already-running editor or remote agent.

The committed reference remains `"Bearer ${COPILOT_MCP_CONTEXT7_API_KEY}"`; never substitute the secret into a tracked or generated configuration. An unset or empty value is incomplete setup: leave Context7 uninstalled/disabled and report it, rather than treating an unauthenticated connection as success.

Complete Notion's OAuth sign-in through the local client's authentication flow. OAuth tokens remain client-managed and are not synchronized here. Local authorization does not imply cloud authorization.

Native portable references use `${NAME}`, not the deprecated VS Code editor configuration's `${env:NAME}` or `${input:...}` syntax. The templates deliberately live outside `.copilot/` so dotfiles do not implicitly deploy local MCP configuration to cloud sessions.

## Cloud sandboxes

This repository is intended to be selected as your Copilot dotfiles repository in personal **Copilot > Cloud** settings (shared with the Codespaces dotfiles selection).

GitHub requires `.copilot/` at the repository root, then copies the **entire repository** into the sandbox home directory on creation or resume. It does not run `install.sh`, `bootstrap.sh`, or any other dotfiles installer. Instructions, agents, and skills are already in their native locations and need no symlink setup.

After reviewing and pushing changes, create or resume a **Cloud** session. Running sandboxes do not receive changes immediately. Selecting **Actions** does not apply these dotfiles.

Because all root files are copied too, `AGENTS.md` limits installation actions to explicit requests. It must not cause sandbox sessions to install machine configuration simply because it is present in home.

### Cloud MCP and connectors

Dotfiles personalization is not a substitute for GitHub-managed MCP setup:

1. In **each target repository**, open **Settings > Copilot > MCP servers** and review/apply [`mcp/cloud.json`](mcp/cloud.json). It enables only Context7's two documentation tools.
2. In that target repository's **Settings > Secrets and variables > Agents**, create the secret `COPILOT_MCP_CONTEXT7_API_KEY`. Organization-level Agents secrets with appropriate repository access are another option.
3. Start a cloud session and verify an actual low-impact tool call.

Use **Agents secrets**, not Actions, Codespaces, or Dependabot secrets. `COPILOT_MCP_`-prefixed secrets are available only to MCP servers referencing them. Cloud sandboxes support Agents secrets, not Agents variables; use secrets for required values. There is no personal cross-repository secret distribution mechanism implemented here.

The JSON template is a versioned deployment artifact, not an auto-loaded cloud config. Changing it, or configuring secrets in this dotfiles repository, does not update settings/secrets in other repositories. Repository MCP settings are also shared with Copilot code review; tool availability there additionally depends on read-only tool annotations.

**Notion:** use the GitHub-managed Notion connector from Copilot's customization UI when available and permitted by policy. Connectors handle configuration/authentication and work in supported local clients and cloud sandboxes, but not Actions sessions. Do not also enable a duplicate local Notion entry if the connector already supplies it.

Repository-configured cloud MCP does not support remote OAuth, so the local Notion entry is deliberately absent from the cloud template. A Notion integration token cannot simply replace OAuth at its hosted endpoint. A separate token-based/self-hosted deployment would require a separately verified design.

## Verification

File presence alone is not proof of use:

- Confirm instructions, agents, and the Context7 skill appear without validation errors in the selected client's customization view.
- In a fresh session, verify Caveman instruction inclusion, select each custom agent, and invoke the Context7 skill without restating its contents.
- Verify Context7 resolves a public library and fetches documentation through an actual MCP call. For Notion, use a user-selected read-only request; do not create or edit content just to test authentication.
- Check for duplicate active sources after migration. Report credential-dependent or interactive checks that were not performed.
- For cloud acceptance, create/resume a sandbox after a reviewed push and repeat discovery/invocation checks. A local copy simulation checks packaging only, not GitHub integration.

## References

- [GitHub: Personalizing cloud sessions](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/personalize-cloud-sessions)
- [VS Code: Migrate customizations to the Copilot harness](https://code.visualstudio.com/docs/agent-customization/migrate-customizations)
- [VS Code: Custom instruction locations](https://code.visualstudio.com/docs/agents/reference/custom-instructions-reference)
- [VS Code: Custom agent file reference](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference)
- [VS Code: Portable MCP configuration and environment references](https://code.visualstudio.com/docs/agents/reference/mcp-configuration)
- [GitHub: CLI instructions](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions) and [skills](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills)
- [GitHub: Repository MCP settings](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/configure-mcp-servers) and [Agents secrets](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/configure-secrets-and-variables)
- [GitHub: Copilot connectors](https://docs.github.com/en/copilot/concepts/customizations/copilot-connectors) and [supported services](https://docs.github.com/en/copilot/reference/supported-connectors)
- [Context7: Endpoint, bearer authentication, and exact tool names](https://context7.com/docs/resources/all-clients)
- [Notion: Hosted MCP endpoint and OAuth setup](https://developers.notion.com/guides/mcp/get-started-with-mcp) (use Copilot's native schema here, not the legacy VS Code configuration shown in some provider examples)
