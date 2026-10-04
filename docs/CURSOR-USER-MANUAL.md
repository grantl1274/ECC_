# Cursor User Manual — Trimmed ECC Workspace

Practical guide for this **trimmed** Cursor pack of [Everything Claude Code (ECC)](https://github.com/affaan-m/ECC), kept to Grant’s daily harness (Revit/C#, TypeScript, Python, web/React).

**Workspace root:** `C:\AI\ECC`  
**Install surface:** `C:\AI\ECC\.cursor` (project pack)  
**Also installed:** lean plugin `%USERPROFILE%\.cursor\plugins\local\ecc-cleanroom`  
**Personal fork:** https://github.com/grantl1274/ECC_  
**Recreate / keep spec:** [PERSONAL-KEEP-RECREATE.md](./PERSONAL-KEEP-RECREATE.md)

**Word (.docx):** [CURSOR-USER-MANUAL-TRIMMED.docx](./CURSOR-USER-MANUAL-TRIMMED.docx)

This manual matches the **post-trim** tree. Full upstream ECC catalogs no longer apply to `.cursor/` here.

---

## 1. Overview

| Surface | Location | Count (trimmed) |
|---------|----------|-----------------|
| Agents | `.cursor/agents/ecc-*.md` | **14** |
| Rules | `.cursor/rules/*.mdc` | **38** |
| Skills | `.cursor/skills/*/SKILL.md` | **22** |
| Commands | `.cursor/commands/*.md` | **9** |
| Hooks | `.cursor/hooks.json` + `.cursor/hooks/*.js` | Wiring + **17** scripts |
| MCP live config | `.cursor/mcp.json` | Empty `{ "mcpServers": {} }` |
| Memory pointer | `.cursor/ecc-agent-data.json` | `~/.cursor/ecc` |

**Removed on purpose:** 200+ skills, unused language packs, homelab/marketing/ML domain packs, `mcp-configs/` templates, install-state bookkeeping.

**Two layers:**

1. **Plugin (always-on lean harness):** `ecc-cleanroom` under Cursor local plugins  
2. **Project pack:** this `.cursor/` DAILY set for C# / TS / Python / React

User-level MCP (Context7, Revit, etc.) stays in **Cursor Settings → MCP**, not in the empty project `mcp.json`.

---

## 2. First steps

1. Open **`C:\AI\ECC`** as the Cursor workspace root.  
2. Confirm `.cursor/` exists.  
3. After pull or trim: **Developer: Reload Window**.  
4. Sanity check (PowerShell):

```powershell
Set-Location C:\AI\ECC
(Get-ChildItem .cursor\agents -File).Count          # expect 14
(Get-ChildItem .cursor\rules -File).Count           # expect 38
(Get-ChildItem .cursor\skills -Directory).Count     # expect 22
(Get-ChildItem .cursor\commands -File).Count        # expect 9
Get-Content .cursor\mcp.json
Get-Content .cursor\ecc-agent-data.json
Test-Path "$env:USERPROFILE\.cursor\plugins\local\ecc-cleanroom"
```

5. Start Agent/Composer. `common-*` rules load automatically; language rules attach when you edit matching files.

---

## 3. Agents (14 kept)

Agents live at `.cursor/agents/ecc-<name>.md`. Frontmatter `name:` is usually the short name (`planner`).

### How to invoke

- Ask: “Use the **planner** / **code-reviewer** / **csharp-reviewer** …”  
- Launch a **Task** and point at the agent file  
- `@`-mention the agent markdown if your Cursor build supports it  
- Prefer slash commands (`/plan`, `/code-review`, `/build-fix`) when available  

Expected loop: **plan → TDD → implement → review → security when needed**.

### Full kept list

| Need | File | Short name |
|------|------|------------|
| Feature / refactor plan | `ecc-planner.md` | planner |
| Architecture / boundaries | `ecc-architect.md` | architect |
| TDD / tests-first | `ecc-tdd-guide.md` | tdd-guide |
| Quality review | `ecc-code-reviewer.md` | code-reviewer |
| Vulns / secrets / auth | `ecc-security-reviewer.md` | security-reviewer |
| Build / type failures | `ecc-build-error-resolver.md` | build-error-resolver |
| Dead code cleanup | `ecc-refactor-cleaner.md` | refactor-cleaner |
| Docs / codemaps | `ecc-doc-updater.md` | doc-updater |
| Library / API docs | `ecc-docs-lookup.md` | docs-lookup |
| C# / Revit /.NET | `ecc-csharp-reviewer.md` | csharp-reviewer |
| TypeScript | `ecc-typescript-reviewer.md` | typescript-reviewer |
| Python | `ecc-python-reviewer.md` | python-reviewer |
| React review | `ecc-react-reviewer.md` | react-reviewer |
| React build red | `ecc-react-build-resolver.md` | react-build-resolver |

Anything else from upstream ECC agent catalogs (homelab, GAN, healthcare, ML, …) was **deleted** from this project pack. Re-add only via selective install if a live project needs it — see recreate spec.

---

## 4. Rules (38 kept)

Path: `.cursor/rules/*.mdc`

### Always-on (`common-*`)

- `common-agents`, `common-code-review`, `common-coding-style`  
- `common-development-workflow`, `common-git-workflow`, `common-hooks`  
- `common-patterns`, `common-performance`, `common-security`, `common-testing`  

### Language packs (glob-scoped)

| Pack | Use when |
|------|----------|
| `csharp-*` | Revit add-ins, .NET |
| `typescript-*` | Node / TS apps |
| `python-*` (+ `python-fastapi`) | Scripts / services |
| `react-*` | React UI |
| `web-*` | General web coding / design quality |

React Native packs were removed. No Go/Rust/Java/Swift/… packs remain.

To force a rule off-glob, **@-mention** the `.mdc` file. Do not fight always-on security/git rules.

---

## 5. Skills (22 kept)

Each skill: `.cursor/skills/<name>/SKILL.md`

### Core workflow

| Skill | When |
|-------|------|
| `search-first` | Research existing code/docs before inventing |
| `tdd-workflow` | Tests first, 80%+ coverage mindset |
| `verification-loop` | Before claiming done |
| `security-review` / `security-scan` | Auth, secrets, inputs, MCP |
| `continuous-learning` / `continuous-learning-v2` | Capture durable patterns |
| `configure-ecc` | Reinstall / verify ECC surfaces |
| `coding-standards` | Cross-cutting quality |
| `git-workflow` | Branches, commits, PRs |
| `context-budget` | Keep context lean |
| `codebase-onboarding` | New / brownfield repos |
| `plan-canvas` | Browser plan review (when useful) |

### Stack

| Skill | When |
|-------|------|
| `dotnet-patterns` | .NET architecture patterns |
| `csharp-testing` | C# tests |
| `python-patterns` / `python-testing` | Python |
| `frontend-patterns` / `backend-patterns` | Web UI / APIs |
| `documentation-lookup` | Pair with Context7 MCP |
| `mcp-server-patterns` | Custom MCP (e.g. Revit) |
| `e2e-testing` | Critical user journeys |

There is **no** `/tdd` command; use skill `tdd-workflow` or agent `ecc-tdd-guide`.

---

## 6. Commands (9 kept)

Type `/` in chat:

| Command | Purpose |
|---------|---------|
| `/plan` | Requirements, risks, phased plan; confirm before code |
| `/code-review` | Quality review |
| `/security-scan` | Security-oriented review |
| `/build-fix` | Fix build/type errors |
| `/quality-gate` | Fast done-checklist |
| `/checkpoint` | Mid-task save / handoff |
| `/learn` | Capture learnings into skills/rules |
| `/refactor-clean` | Dead code / cleanup pass |
| `/ecc-guide` | Orientation to ECC-style workflows |

---

## 7. Hooks

- Manifest: `.cursor/hooks.json`  
- Entry scripts: `.cursor/hooks/*.js` → shared runtime under `.cursor/scripts/`

Expect:

- Block `git … --no-verify`  
- Warn / block secret patterns in prompts and sensitive file reads  
- Session memory under `~/.cursor/ecc`  
- Edit / MCP / subagent logging events as wired  

If a command is blocked, fix the request (remove dangerous flags). Do not disable hooks casually.

Lean alternative: `ecc-cleanroom` plugin hooks cover a smaller fail-open set (secrets, git safety, memory).

---

## 8. MCP

### Project file (empty on purpose)

```json
{
  "mcpServers": {}
}
```

### What you actually use

Configure in **Cursor user MCP settings** (recommended):

- **Context7** — library/docs lookup (pairs with `documentation-lookup` / `docs-lookup`)  
- **user-revit-mcp** — Revit automation  
- Optional: GitLens / other helpers  

Do **not** paste wholesale MCP templates with `YOUR_*_HERE` placeholders. Prefer env vars / secret store. Least privilege: enable one server at a time.

---

## 9. Memory / agent data

`.cursor/ecc-agent-data.json`:

```json
{
  "agentDataHome": "~/.cursor/ecc",
  "notes": "Isolates ECC session memory from Claude Code ~/.claude. Do not put secrets here."
}
```

- Cursor ECC hooks → `%USERPROFILE%\.cursor\ecc`  
- Claude Code → `%USERPROFILE%\.claude` (separate)  
- Never store API keys in agent data home  

---

## 10. Day-to-day workflows

### A. New feature

1. `/plan` — wait for confirm  
2. `tdd-workflow` or **ecc-tdd-guide** — RED tests  
3. Implement → green  
4. `/code-review`  
5. `/security-scan` if auth / network / files touched  
6. Commit only when you ask  

### B. Revit / C#

1. Edit `.cs` files — `csharp-*` rules attach  
2. Ask **ecc-csharp-reviewer** or use `dotnet-patterns` / `csharp-testing`  
3. Use Revit MCP for live model actions (user MCP)  

### C. Bugfix

1. Failing test first  
2. Minimal fix  
3. Short `/code-review` on touched files  

### D. Build red

1. `/build-fix` or **ecc-build-error-resolver**  
2. For React build: **ecc-react-build-resolver**  

### E. Before “done”

1. `/quality-gate` or `verification-loop`  
2. Confirm tests/lints you claim actually ran  

---

## 11. Troubleshooting

| Symptom | Try |
|---------|-----|
| Agents/skills missing | Workspace = `C:\AI\ECC`; Reload Window; recount (14 / 22 / 9 / 38) |
| “Missing” upstream agent/skill | Expected after trim — see KEEP list / recreate spec |
| Rules not applying | Check globs; `@` the `.mdc`; ensure file type matches pack |
| Hook blocked shell | Remove `--no-verify` / secret patterns |
| MCP not in project | Use **user** MCP settings; project `mcp.json` is empty by design |
| Memory mixed with Claude Code | Check `ecc-agent-data.json` → `~/.cursor/ecc` |
| `/tdd` missing | Use `tdd-workflow` skill or `ecc-tdd-guide` |

---

## 12. Safety

- Never commit tokens, PEMs, `.env` contents  
- Treat hook blocks as features  
- Empty project MCP is intentional — wire servers carefully  
- Official ECC sources only when reinstalling: `affaan-m/ECC`, npm packages, plugin `ecc@ecc`, ecc.tools  
- Personal recreate source of truth: `docs/PERSONAL-KEEP-RECREATE.md`  

---

## Appendix A — Layout map (trimmed)

```
C:\AI\ECC\
  docs\
    CURSOR-USER-MANUAL.md      # this file
    CURSOR-USER-MANUAL.docx    # Word export
    PERSONAL-KEEP-RECREATE.md  # keep / delete / recreate recipe
  .cursor\
    agents\                    # 14 ecc-*.md
    rules\                     # 38 *.mdc
    skills\                    # 22 skill dirs
    commands\                  # 9 *.md
    hooks.json + hooks\ + scripts\
    mcp.json                   # empty
    ecc-agent-data.json
```

## Appendix B — Restore / expand (reference)

Do **not** re-run full profile install unless intentional.

Selective expand example:

```powershell
Set-Location C:\AI\ECC
.\install.ps1 --target cursor --skills deep-research
# then trim again if noise returns
```

Or restore from recreate spec section 8 prompt.

Lean plugin path:

```powershell
Test-Path "$env:USERPROFILE\.cursor\plugins\local\ecc-cleanroom\.cursor-plugin\plugin.json"
```

## Appendix C — Related docs

- `docs/PERSONAL-KEEP-RECREATE.md` — KEEP / LIBRARY / DELETE  
- Root README — Cursor IDE Support (upstream)  
- `ecc-cleanroom` plugin README under `%USERPROFILE%\.cursor\plugins\local\ecc-cleanroom\`  

---

*Manual rewritten for the trimmed Cursor pack. Counts verified against `C:\AI\ECC\.cursor` after the personal KEEP trim.*
