# Customization repository

The tracked `.copilot/` files are the authoritative personal customizations. Keep them native, portable, and directly usable; cloud sandboxes copy this entire repository into home and run no installer.

## Installation contract

Apply these steps **only when explicitly asked to install or update these customizations**. Merely reading this file, including a copy in a sandbox home directory, must not trigger installation or changes to machine configuration.

1. Detect the actual execution host, installed clients/harnesses, native user destinations, and any `COPILOT_HOME` override. Configure where the agent runs, not an unrelated client machine. Prefer the current Copilot harness; handle legacy VS Code Local only if detected and requested. Consult current official docs when discovery rules are uncertain.
2. Read `README.md` and inventory existing destinations without exposing credentials. Compare managed content before changing it. Leave correct links/content alone. Ask before replacing divergent files or same-name MCP entries; back up originals outside this checkout and report a reversible recovery path.
3. Link individual `.copilot/` instruction/agent files and complete skill directories into native user destinations. Never replace the entire Copilot home or unrelated files. Prefer symlinks; if unavailable, copy and explain that updates require a refresh. Do not edit through links or place runtime state in the tracked tree.
4. Detect duplicate active sources, especially the old Caveman modular instruction and `context7-mcp` under another personal skill root. After backup and verification, retire only confirmed superseded, identical managed content; ask about divergent content. Do not delete third-party plugins or their built-in skills.
5. For requested MCP setup, merge selected entries from `mcp/local.json` into the native user MCP configuration. Preserve unrelated servers, settings, and authentication state; never link the entire mutable config to this template. Skip/report Context7 if its required environment variable is unset or empty; do not install it with an empty credential or resolve its reference into a file. Notion requires a separate local OAuth sign-in and exposes read/write tools; confirm the selected servers before enabling them.
6. Never request secrets in chat, print them, commit them, or copy credential stores. Users provide secrets through an appropriate local environment/credential mechanism or GitHub Agents secrets. Example env files are not automatically loaded. Do not change GitHub settings, authenticate, commit, push, or install dependencies without corresponding authorization.
7. Verify discovery and inclusion/invocation in fresh sessions of the intended clients, with no duplicate active copies. When authorized and authenticated, verify MCP with a low-impact tool call. Report changed destinations, checks actually run, copy-mode refresh requirements, recovery details, and anything unverified.

## Maintaining this repository

Keep shared definitions in `.copilot/`, sanitized MCP deployment templates in `mcp/`, and operational documentation in `README.md`. Preserve existing behavior except explicitly requested or necessary portability changes. Do not add machine-specific paths, credentials, runtime data, generated editor synchronization plugins, or duplicate shared/cloud source trees.
