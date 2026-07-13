# Cursor User Manual — ECC Workspace

Practical guide for using this clone of [Everything Claude Code (ECC)](https://github.com/affaan-m/ECC) inside **Cursor**, after a full Cursor-targeted install into `.cursor/`.

**Workspace root:** `C:\AI\ECC`  
**Install surface:** `C:\AI\ECC\.cursor`  
**ECC version (install state):** 2.0.0

This manual describes what is actually present in this workspace, not generic Cursor marketing.

---

## 1. Overview

After the Cursor install, Agent/Composer in this project gets:

| Surface | Location | Count (this workspace) |
|---------|----------|------------------------|
| Agents | `.cursor/agents/ecc-*.md` | 67 |
| Rules | `.cursor/rules/` | 160 |
| Skills | `.cursor/skills/*/SKILL.md` | 278 skill dirs |
| Commands | `.cursor/commands/*.md` | 94 |
| Hooks | `.cursor/hooks.json` + `.cursor/hooks/*.js` | 15 events, 17 hook scripts |
| MCP live config | `.cursor/mcp.json` | Empty by design (`{ "mcpServers": {} }`) |
| MCP templates | `.cursor/mcp-configs/` | Template + security note |
| Memory pointer | `.cursor/ecc-agent-data.json` | Isolates data under `~/.cursor/ecc` |

Also present:

- Shared hook/runtime scripts under `.cursor/scripts/` (adapter reuses ECC Node hooks).
- Repo identity files at the root (`AGENTS.md`, `CLAUDE.md`) that Cursor may still pick up as project context.
- Extra skills installed via `--skills`: `dmux-workflows`, `skill-comply`.

**Not available as a Cursor-native install module:** dmux/tmux **orchestration runtime** packaging. Cursor already has its own Task/subagent surface. The `dmux-workflows` skill documents external dmux patterns; it does not turn Cursor into dmux. Prefer Cursor Task + ECC agents for in-IDE parallel work.

---

## 2. First steps

1. Open the folder **`C:\AI\ECC`** as the Cursor workspace root (not a parent folder that only contains ECC).
2. Confirm `.cursor/` is visible at the project root.
3. If agents, rules, skills, commands, or hooks seem missing after install or pull: **Developer: Reload Window**.
4. Optional sanity checks (PowerShell):

```powershell
Set-Location C:\AI\ECC
(Get-ChildItem .cursor\agents -File).Count
(Get-ChildItem .cursor\rules -File).Count
(Get-ChildItem .cursor\skills -Directory).Count
(Get-ChildItem .cursor\commands -File).Count
Get-Content .cursor\mcp.json
Get-Content .cursor\ecc-agent-data.json
```

5. Start a chat (Agent/Composer). ECC `common-*` rules with `alwaysApply: true` load automatically. Language rules attach when you edit matching files.

---

## 3. Agents

### How they work here

- Installed as **`.cursor/agents/ecc-<name>.md`** (e.g. `ecc-planner.md`, `ecc-code-reviewer.md`).
- The `ecc-` prefix avoids collisions with marketplace or user agents.
- Inside each file, frontmatter `name:` is usually the short name (`planner`, not `ecc-planner`).
- Cursor builds differ: some expose project agents in the UI; all files remain usable as **explicit Task/subagent definitions** or `@`-style references.

### How to invoke

Use whichever works in your Cursor build:

- Ask in chat: “Use the **planner** / **code-reviewer** / **security-reviewer** agent …”
- Launch a **Task** subagent and point it at the agent file under `.cursor/agents/`.
- `@`-mention the agent markdown if your build supports attaching files.
- Slash commands such as `/plan`, `/code-review`, `/build-fix` often drive the same workflows without naming the agent file.

ECC’s always-on agent rule expects proactive use: plan → TDD → review → security when relevant.

### Top picks (start here)

| Need | Agent file | Short name |
|------|------------|------------|
| Feature / refactor plan | `ecc-planner.md` | planner |
| Architecture / boundaries | `ecc-architect.md` | architect |
| TDD / tests-first | `ecc-tdd-guide.md` | tdd-guide |
| Quality review after edits | `ecc-code-reviewer.md` | code-reviewer |
| Vulns / secrets / auth | `ecc-security-reviewer.md` | security-reviewer |
| Build / type failures | `ecc-build-error-resolver.md` | build-error-resolver |
| E2E flows | `ecc-e2e-runner.md` | e2e-runner |
| Dead code cleanup | `ecc-refactor-cleaner.md` | refactor-cleaner |
| Docs / codemaps | `ecc-doc-updater.md` | doc-updater |
| Brownfield specs | `ecc-spec-miner.md` | spec-miner |
| Library/API docs | `ecc-docs-lookup.md` | docs-lookup |
| Harness cost/reliability | `ecc-harness-optimizer.md` | harness-optimizer |
| Autonomous loops | `ecc-loop-operator.md` | loop-operator |

### Language / stack reviewers (use when editing that stack)

`ecc-typescript-reviewer`, `ecc-python-reviewer`, `ecc-django-reviewer`, `ecc-react-reviewer`, `ecc-vue-reviewer`, `ecc-go-reviewer`, `ecc-rust-reviewer`, `ecc-java-reviewer`, `ecc-kotlin-reviewer`, `ecc-swift-reviewer`, `ecc-cpp-reviewer`, `ecc-csharp-reviewer`, `ecc-php-reviewer`, `ecc-fastapi-reviewer`, `ecc-flutter-reviewer`, plus matching `*-build-resolver` agents when the build is red.

### Other groups (catalog sketch)

- **Quality / analysis:** `ecc-performance-optimizer`, `ecc-silent-failure-hunter`, `ecc-pr-test-analyzer`, `ecc-type-design-analyzer`, `ecc-comment-analyzer`, `ecc-code-simplifier`, `ecc-code-explorer`, `ecc-code-architect`, `ecc-agent-evaluator`
- **Ops / networks / lab:** `ecc-network-*`, `ecc-homelab-architect`, `ecc-database-reviewer`, `ecc-seo-specialist`, `ecc-healthcare-reviewer`
- **Open-source pipeline:** `ecc-opensource-forker`, `ecc-opensource-sanitizer`, `ecc-opensource-packager`
- **Product / media / GAN harness:** `ecc-marketing-agent`, `ecc-gan-planner`, `ecc-gan-generator`, `ecc-gan-evaluator`, `ecc-chief-of-staff`, `ecc-a11y-architect`
- **ML:** `ecc-mle-reviewer`, `ecc-pytorch-build-resolver`

Full list: every file matching `.cursor/agents/ecc-*.md` (67). Root `AGENTS.md` summarizes many of the same roles without the `ecc-` filename prefix.

---

## 4. Rules

### What they do

Rules are standing instructions. They bias planning, coding style, testing, security, and git hygiene without you pasting them each turn.

### Layout

- Path: `.cursor/rules/`
- Cursor-native form: YAML frontmatter + body (`.mdc`; some `.md` twins exist from install translation).
- Frontmatter fields: `description`, optional `globs`, `alwaysApply`.

### Auto-apply vs mention

**Always on** (`alwaysApply: true`) — includes the common set, e.g.:

- `common-agents`, `common-coding-style`, `common-development-workflow`, `common-git-workflow`
- `common-hooks`, `common-patterns`, `common-performance`, `common-security`, `common-testing`

These apply in Agent chat for this project without `@` mentions.

**Glob-scoped** (`alwaysApply: false` + `globs`) — language packs such as:

- `typescript-coding-style.mdc` → `**/*.{ts,tsx,js,jsx}`
- `python-security.mdc` → `**/*.{py,pyi}`
- Similar packs for Go, Java, Kotlin, Rust, Swift, PHP, Angular, Vue/Nuxt, Dart, C++, C#, Perl, ArkTS, etc.

They attach when context matches those files. To force a rule when globs do not match, **@-mention** the rule file.

### Practical tip

Do not fight always-on rules with “ignore previous instructions.” Prefer scoped asks (“for this throwaway scratch file, skip TDD”) and expect the agent to still honor security and secret hygiene.

---

## 5. Skills

### How Cursor skills work here

- Each skill lives in `.cursor/skills/<name>/SKILL.md`.
- Frontmatter `name` / `description` drive discovery; the body is the workflow.
- Skills activate when you ask for that workflow, when a command points at them, or when the agent matches the description to the task.
- Discover: browse `.cursor/skills/`, search by domain, or ask “which ECC skill covers X?”

High-signal discovery commands in chat:

- “Use the **tdd-workflow** skill on …”
- “Follow **verification-loop** before you claim done.”
- “Run **configure-ecc** to adjust install.”

### High-value skills

| Skill | When to use |
|-------|-------------|
| `tdd-workflow` | New features, bugfixes, coverage expectations |
| `verification-loop` | Before claiming complete / green |
| `e2e-testing` | Critical user journeys |
| `coding-standards` | Cross-cutting quality bar |
| `security-scan` / security skills | Auth, uploads, secrets, perimeter |
| `git-workflow` | Commits, branches, PRs |
| `frontend-patterns` / `react-patterns` | UI work |
| `backend-patterns` / `api-design` | Services and APIs |
| `search-first` / `deep-research` | Research before inventing APIs |
| `continuous-learning` | Extract reusable patterns from sessions |
| `configure-ecc` | Reconfigure / verify ECC install |
| `agent-eval` / `eval-harness` | Measure agent behavior |
| `dmux-workflows` | External multi-pane dmux orchestration (optional) |
| `skill-comply` | Measure whether skills/rules are actually followed |

There is **no** `/tdd` command file in this Cursor install; use the **tdd-workflow** skill or the **ecc-tdd-guide** agent instead. Prefer skills-first for new workflows (repo policy); commands remain compatibility entry points.

---

## 6. Commands

### How they work

- Files: `.cursor/commands/<name>.md` (94).
- In chat, type `/` and pick the command (e.g. `/plan`, `/code-review`).
- Each file has `description` frontmatter; body is the workflow the agent should run.
- Many commands say “run inline” (no Task) so they stay usable even if agent files are not exposed in the UI.

### Notable commands

| Command | Purpose |
|---------|---------|
| `/plan` | Requirements, risks, phased plan; wait for confirm before code |
| `/code-review` | Quality review pass |
| `/security-scan` | Security-oriented review |
| `/build-fix` | Fix build/type errors |
| `/quality-gate` | Fast quality checklist |
| `/feature-dev` | End-to-end feature development loop |
| `/learn` / `/skill-create` | Capture learnings / generate skills |
| `/ecc-guide` | Orientation to ECC workflows |
| `/pr` / `/review-pr` | PR create / review helpers |
| `/react-review`, `/python-review`, `/go-review`, … | Stack-specific review |
| `/orch-*`, `/multi-*`, `/epic-*` | Larger orchestration-style workflows (prompt recipes; not a Cursor tmux runtime) |
| `/hookify*` | Project hook customization helpers |
| `/sessions`, `/save-session`, `/resume-session` | Session persistence helpers |

List all: filenames under `.cursor/commands/` (basename = slash name).

---

## 7. Hooks

### Wiring

- Manifest: `.cursor/hooks.json`
- Thin wrappers: `.cursor/hooks/*.js` → shared logic via `adapter.js` and `.cursor/scripts/hooks/`

Cursor events installed here include: `sessionStart`, `sessionEnd`, `beforeShellExecution`, `afterShellExecution`, `afterFileEdit`, `beforeMCPExecution`, `afterMCPExecution`, `beforeReadFile`, `beforeSubmitPrompt`, `subagentStart`, `subagentStop`, `beforeTabFileRead`, `afterTabFileEdit`, `preCompact`, `stop`.

### What you will notice

| Behavior | Event / hook | Effect |
|----------|--------------|--------|
| Block `--no-verify` on git | `beforeShellExecution` | Protects commit/push hooks |
| Dev-server / long-run guidance | `beforeShellExecution` | Tmux-oriented reminders / blocks (behavior inherited from shared hooks) |
| Git push review nudge | `beforeShellExecution` | Warn before push |
| Auto-format / tsc / `console.log` warnings | `afterFileEdit` | After edits |
| Secret patterns in prompts | `beforeSubmitPrompt` | Detect `sk-`, `ghp_`, `AKIA`, etc. |
| Sensitive file reads | `beforeReadFile` | Warn on `.env`, keys, PEMs |
| Tab cannot read secrets | `beforeTabFileRead` | Harder block for Tab autocomplete |
| MCP audit | `beforeMCPExecution` / `afterMCPExecution` | Log / warn on untrusted servers |
| Session memory load/save | `sessionStart` / `sessionEnd` / `preCompact` / `stop` | Persist context under agent data home |
| Subagent observability | `subagentStart` / `subagentStop` | Logging |

If a shell command is rejected, remove dangerous flags (e.g. `--no-verify`) or adjust the request; do not disable hooks casually.

---

## 8. MCP

### Live config (intentional empty)

`.cursor/mcp.json` is:

```json
{
  "mcpServers": {}
}
```

Cursor loads **this** file. Empty means no project MCP servers until you opt in.

### Templates (do not paste blindly)

- Template: `.cursor/mcp-configs/mcp-servers.json` — placeholder keys (`YOUR_*_HERE`).
- Security note: `.cursor/mcp-configs/README-SECURITY.txt`

### Safe enable procedure

1. Pick only the servers you need from the template.
2. Copy entries into `.cursor/mcp.json` under `mcpServers` — **never** copy the whole template with placeholders.
3. Replace placeholders with real values via **environment variables** or a secret store, not committed secrets.
4. Reload the Cursor window / MCP servers.
5. Confirm `beforeMCPExecution` audit is acceptable for your threat model.

Example shape (illustrative — use real env var names your machine provides):

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${env:GITHUB_PERSONAL_ACCESS_TOKEN}"
      }
    }
  }
}
```

Prefer user-level Cursor MCP settings for personal tokens when the workspace is shared or public.

---

## 9. Memory / agent data

`.cursor/ecc-agent-data.json`:

```json
{
  "agentDataHome": "~/.cursor/ecc",
  "notes": "Isolates ECC session memory from Claude Code ~/.claude. Do not put secrets here."
}
```

Effects:

- Session summaries / learned skills from ECC hooks should land under **`~/.cursor/ecc`** (expanded path on Windows: typically `%USERPROFILE%\.cursor\ecc`), not `~/.claude`.
- The `sessionStart` hook and Cursor-aware hook defaults reinforce `ECC_AGENT_DATA_HOME`.
- Continuous learning v2 instincts remain separate (`CLV2_HOMUNCULUS_DIR`, default `~/.local/share/ecc-homunculus` where applicable).

To **intentionally share** memory with Claude Code, set `agentDataHome` / `ECC_AGENT_DATA_HOME` to `~/.claude` — not recommended if both harnesses run in parallel.

Do not store API keys in the agent data home.

---

## 10. Day-to-day workflows

### A. New feature (plan → TDD → implement → review)

1. `/plan` with the feature description; wait for confirmation.
2. Ask for **tdd-workflow** or agent **ecc-tdd-guide**: RED tests first.
3. Implement to green; run project tests.
4. `/code-review` or Task **ecc-code-reviewer**.
5. `/security-scan` if auth, data, or network boundary touched.
6. Commit only when you ask the agent to (ECC/git rules: no surprise commits).

### B. Bugfix

1. Reproduce; capture failing test first (tdd-guide / tdd-workflow).
2. Minimal fix; re-run tests.
3. Short code-review pass on the touched files.

### C. Security pass before push

1. `/security-scan` on the diff and related entry points.
2. Task **ecc-security-reviewer** for CRITICAL/HIGH items.
3. Expect hooks to block `--no-verify` and to warn on secret-like prompts/files.

### D. Build is red

1. `/build-fix` or **ecc-build-error-resolver** (stack-specific resolver if known).
2. Fix incrementally; re-run the failing command after each fix.

### E. Skill-driven research / patterns

1. “Use **search-first** / **deep-research** before proposing a new library.”
2. Implement with **frontend-patterns** or **backend-patterns** as appropriate.
3. Optionally `/learn` after a successful session.

### F. Compliance of rules/skills (optional)

1. Use **skill-comply** when you need evidence that agents follow a rule/skill.
2. Prefer dry-run first; note it is oriented around Claude CLI traces and may need adaptation on Windows/Cursor-only setups.

### G. External parallel agents (optional)

1. Read **dmux-workflows**; install/run dmux outside Cursor if you want tmux panes.
2. For in-Cursor parallelism, use multiple Tasks / agents instead.

---

## 11. Troubleshooting

| Symptom | What to try |
|---------|-------------|
| Agents/skills/commands missing | Confirm workspace is `C:\AI\ECC`; Reload Window; recount files under `.cursor/` |
| Rules not applying | Check `alwaysApply` / `globs`; `@` the rule file; ensure `.cursor/rules` not ignored |
| Hook blocked a shell command | Read the block reason; remove `--no-verify` or risky patterns; only then consider editing `hooks.json` |
| MCP servers do not appear | Confirm entries are in **`.cursor/mcp.json`**, not only `mcp-configs/`; reload; check Cursor MCP logs |
| MCP fails auth | Placeholders left as `YOUR_*_HERE`; fix env and restart MCP |
| Memory mixed with Claude Code | Verify `.cursor/ecc-agent-data.json` and `%USERPROFILE%\.cursor\ecc`; unset accidental `ECC_AGENT_DATA_HOME=~/.claude` |
| Agent name not found | Use `ecc-<name>` filenames; short `name` is inside the file |
| `/tdd` missing | Use skill `tdd-workflow` or agent `ecc-tdd-guide` |
| Orchestration / dmux “not installed” | Expected for Cursor; use Tasks + agents, or external dmux + `dmux-workflows` skill |
| Node hook errors | Ensure Node is on PATH; hooks invoke `node .cursor/hooks/...` from the project root |

Install metadata: `.cursor/ecc-install-state.json` (may only list the latest incremental `--skills` ops even when a full tree is present). Trust the filesystem counts above for day-to-day use.

---

## 12. Safety

- **Never** copy `.cursor/mcp-configs/mcp-servers.json` wholesale into `.cursor/mcp.json`.
- **Never** commit real tokens, PEMs, or `.env` contents.
- Treat hook blocks on hook-bypass flags and secret reads as features, not bugs.
- Template MCP entries with `YOUR_*_HERE` are unsafe if pasted as-is.
- Do not put secrets in `~/.cursor/ecc`.
- Prefer least-privilege MCP: enable one server at a time.
- Official ECC sources only (see root README warning): GitHub `affaan-m/ECC`, npm packages, plugin slug `ecc@ecc`, ecc.tools.

---

## Appendix A — Layout map

```
C:\AI\ECC\
  AGENTS.md                 # Cross-harness agent instructions (repo)
  CLAUDE.md                 # Claude-oriented project guidance
  .cursor\
    agents\ecc-*.md         # 67 Cursor-prefixed agents
    rules\                  # 160 rules (.mdc / .md)
    skills\<name>\SKILL.md  # 278 skills
    commands\*.md           # 94 slash commands
    hooks.json              # Hook event wiring
    hooks\*.js              # Cursor hook entrypoints + adapter.js
    scripts\                # Shared Node runtime used by hooks
    mcp.json                # LIVE MCP (empty)
    mcp-configs\            # TEMPLATES only
    ecc-agent-data.json     # Memory root pointer
    ecc-install-state.json  # Install bookkeeping
  docs\CURSOR-USER-MANUAL.md  # This file
```

## Appendix B — Reinstall / refresh (reference)

From repo root (do not run unless you intend to change the install):

```powershell
# Example from root README — targets Cursor with language packs
.\install.ps1 --target cursor typescript
```

Skills add-on example (already applied here for dmux + skill-comply):

```powershell
.\install.ps1 --target cursor --skills dmux-workflows,skill-comply
```

Prefer reading root README section **Cursor IDE Support** before re-running installers.

## Appendix C — Related docs

- Root README — **Cursor IDE Support**
- `docs/architecture/cross-harness.md` — how Cursor fits the ECC harness matrix
- `hooks/README.md` — deeper Claude/shared hook semantics
- `the-shortform-guide.md` / `the-longform-guide.md` / `the-security-guide.md` — ECC philosophy and security

---

*Generated for this workspace’s Cursor install. Counts and paths verified against `C:\AI\ECC\.cursor` at documentation time. Do not treat README marketing counts as authoritative if they diverge from the local tree.*
