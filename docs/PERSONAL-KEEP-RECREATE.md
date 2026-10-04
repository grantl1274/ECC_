# Personal Keep + Recreate Spec (Grant)

**Purpose:** One file an AI can copy into any empty folder and use to recreate *only* the useful harness for this operator. Everything else in the full ECC tree may be deleted after this profile is installed elsewhere.

**Operator profile (evidence-backed):**
- OS: Windows 10 / PowerShell (never use `&&` in PowerShell; prefer `;`)
- Primary IDE: **Cursor** (`C:\AI\ECC\.cursor` project pack + local plugin `ecc-cleanroom`)
- Secondary: **Claude Code** (`~\.claude` manual full install already done once)
- GitHub home for this pack: `https://github.com/grantl1274/ECC_.git`
- MCP in use: **Context7** (docs), **user-revit-mcp** (Revit), GitLens/GitKraken helpers
- Languages that matter day-to-day: **C# / .NET (Revit)**, **TypeScript/JS**, **Python**, occasional frontend/React
- Workflow habits: plan → TDD → review → verify; Taskmaster-style task loops; research-first; no hook-skipping; no secrets in git
- Preference signal: you already kept a lean **ecc-cleanroom** plugin (8 agents / 7 skills / 8 commands) — full ECC (67 agents / 278 skills / 94 commands) is too heavy for daily load

**Upstream source of truth (do not vendor blindly):** `https://github.com/affaan-m/ECC`  
**This personal fork (optional):** `https://github.com/grantl1274/ECC_`

---

## 0. How an AI should use this file

1. Read this file end-to-end.
2. Recreate the **DAILY** surface only (sections 1–4).
3. Optionally install **LIBRARY** items on demand (section 5).
4. Do **not** install DELETE buckets (section 6) unless the human asks.
5. Verify with the checklist (section 7).
6. After DAILY is working in Cursor (and optionally Claude Code), the giant full-ECC clone can be deleted or left remote-only.

---

## 1. Recommended end state (target layout)

Prefer **two layers**, not one mega-clone:

### A. Always-on Cursor plugin (lean harness) — PRIMARY

Install/restore: `%USERPROFILE%\.cursor\plugins\local\ecc-cleanroom\`

Contains the reusable daily harness (agents, skills, commands, rules, hooks).

### B. Per-project `.cursor/` overlay — when opening real app repos

In each real project (e.g. Revit add-in, web app), keep only:

```text
.cursor/
  rules/          # common-* + csharp-* and/or typescript-* and/or python-* only
  agents/         # optional: copy ecc-csharp-reviewer / ecc-typescript-reviewer / ecc-python-reviewer if needed
  skills/         # OPTIONAL library skills, not the full 278
  commands/       # optional shortlist from section 3
  mcp.json        # ONLY servers you use (Context7 + Revit). Never blanket-enable templates.
  ecc-agent-data.json   # agentDataHome = ~/.cursor/ecc
```

### C. Claude Code user install — OPTIONAL / secondary

Only if you still use Claude Code CLI/Desktop daily:

```powershell
# From a fresh ECC clone (or this fork):
.\install.ps1 --profile minimal --target claude --with language:csharp --with language:typescript --with language:python
```

Do **not** restack `/plugin install ecc@ecc` on top of a full manual install.

---

## 2. DAILY — keep / recreate these first

### 2.1 Agents (8 core)

| Role | Cleanroom file | Equivalent upstream ECC Cursor agent |
|------|----------------|--------------------------------------|
| Plan features | `agents/planner.md` | `.cursor/agents/ecc-planner.md` |
| Architecture | `agents/architect.md` | `ecc-architect.md` |
| Code review | `agents/code-reviewer.md` | `ecc-code-reviewer.md` |
| Security | `agents/security-reviewer.md` | `ecc-security-reviewer.md` |
| TDD | `agents/tdd-guide.md` | `ecc-tdd-guide.md` |
| Build red | `agents/build-error-resolver.md` | `ecc-build-error-resolver.md` |
| Dead code | `agents/refactor-cleaner.md` | `ecc-refactor-cleaner.md` |
| Docs | `agents/doc-updater.md` | `ecc-doc-updater.md` |

**Also useful for YOUR stacks (add to DAILY when working those languages):**

| Stack | Agent |
|-------|--------|
| Revit / C# | `ecc-csharp-reviewer.md` |
| TypeScript | `ecc-typescript-reviewer.md` |
| Python | `ecc-python-reviewer.md` |
| React UI | `ecc-react-reviewer.md` + `ecc-react-build-resolver.md` |
| API docs | `ecc-docs-lookup.md` |

### 2.2 Skills (7 core + 8 personal)

**Core (cleanroom / always):**

1. `search-first`
2. `tdd-workflow`
3. `verification-loop`
4. `security-review`
5. `continuous-learning` (or `continuous-learning-v2` if installing from upstream)
6. `quality-gate` (cleanroom) — or upstream `verification-loop` + `/quality-gate` command
7. `parallel-worktrees` (cleanroom) — optional if you use git worktrees

**Personal add-ons (from upstream ECC skills/, keep reachable):**

1. `dotnet-patterns`
2. `csharp-testing`
3. `python-patterns`
4. `python-testing`
5. `coding-standards`
6. `git-workflow`
7. `documentation-lookup` (pairs with Context7)
8. `mcp-server-patterns` (you run custom MCP: Revit)
9. `configure-ecc` (only needed when reinstalling)
10. `frontend-patterns` / `backend-patterns` when doing web API/UI work

### 2.3 Commands (8)

Keep slash commands that map to the daily loop:

| Command | Why |
|---------|-----|
| `/plan` | multi-file features |
| `/code-review` | after meaningful edits |
| `/quality-gate` | before “done” |
| `/checkpoint` | mid-task save |
| `/learn` | capture durable rules/skills |
| `/security-scan` | auth, uploads, secrets, MCP |
| `/build-fix` | red builds |
| `/refactor-clean` | dead code pass |

### 2.4 Rules (always-on + language packs)

**Always-on (from cleanroom OR map from ECC `rules/common`):**

- `research-first` / common research posture
- `verification-before-done`
- `security-hygiene` / `common-security`
- `git-safety` / `common-git-workflow`
- `token-efficiency` / `common-performance` (context discipline)
- `subagent-delegation` / `common-agents`
- `common-coding-style`
- `common-testing`
- `common-development-workflow`
- `common-patterns`

**Language packs — install only what you touch:**

| Pack | When |
|------|------|
| `csharp-*` (coding-style, patterns, security, testing, hooks) | Revit / .NET |
| `typescript-*` | Node/TS apps |
| `python-*` (+ `python-fastapi` if FastAPI) | scripts / services |
| `react-*` / `web-*` | frontend only |

Do **not** install arkts/kotlin/swift/vue/nestjs/homelab packs unless a live project needs them.

### 2.5 Hooks (small fail-open set)

Prefer cleanroom hooks over full ECC hook runtime:

- session start / end memory
- block secrets in prompts
- warn on sensitive file reads
- deny `git ... --no-verify`
- pre-compact snapshot
- stop-time verification reminder

Upstream Cursor install can pull a large hooks stack — **skip** if cleanroom hooks already cover safety.

### 2.6 MCP (enable only these)

Live `.cursor/mcp.json` should stay minimal:

```json
{
  "mcpServers": {
    "context7": { "_comment": "paste real Context7 config from working Cursor user MCP" },
    "revit": { "_comment": "paste working user-revit-mcp config" }
  }
}
```

**Rules:**
- Never auto-enable every template under `.cursor/mcp-configs/`
- Disable unused servers; they burn context
- Keep secrets in user env / Cursor secrets — never commit tokens

### 2.7 Operator personal rules (already in Cursor user rules — preserve)

Recreate as Cursor user rules or project rules if wiped:

1. PowerShell-safe scripts (no illegal backticks / decorative unicode in `.ps1`)
2. Commit only when asked; no force-push main; no `--no-verify`
3. Verification before claiming done
4. Frontend design hard rules (when doing UI)
5. Research-first / Taskmaster loop habits for multi-step work
6. Winston/JSDoc habits when writing Node services (project-specific)

---

## 3. Fast recreate recipes (copy-paste)

### Recipe A — Restore lean Cursor plugin (preferred)

If `ecc-cleanroom` still exists:

```powershell
# Already living at:
#   %USERPROFILE%\.cursor\plugins\local\ecc-cleanroom
# Reload Cursor window after edits.
```

If missing, recreate by copying that tree from git history / backup, **or** build from this spec:
- Create plugin folder with `.cursor-plugin/plugin.json` (`name: ecc-cleanroom`)
- Add the 8 agents, 7–8 skills, 8 commands, 8 rules, hooks from section 2
- Restart Cursor

### Recipe B — Selective install from upstream ECC into a project

```powershell
git clone https://github.com/affaan-m/ECC.git C:\AI\ECC-src
Set-Location C:\AI\ECC-src
npm install

# Lean core without full hook overload if you prefer minimal:
.\install.ps1 --profile minimal --target cursor

# Then ADD language packs you actually use:
.\install.ps1 --target cursor csharp typescript python

# Pull specific skills by id when needed:
.\install.ps1 --target cursor --skills search-first,tdd-workflow,verification-loop,security-review,dotnet-patterns,csharp-testing,python-patterns,python-testing,git-workflow,documentation-lookup,coding-standards,mcp-server-patterns,configure-ecc
```

Then **delete** non-daily agents/rules/skills from `.cursor/` using section 6.

### Recipe C — Claude Code lean install

```powershell
Set-Location C:\AI\ECC-src
.\install.ps1 --profile minimal --target claude
.\install.ps1 --target claude csharp typescript python
```

Memory isolation if both Cursor and Claude Code run:

```powershell
# Cursor sessions → ~/.cursor/ecc (already set via .cursor/ecc-agent-data.json)
# Claude Code → ~/.claude (default)
```

### Recipe D — Rebuild ONLY this recreate doc into a new mini-repo

New repo contents:

```text
KEEP-RECREATE.md          # this file (rename to README if desired)
.cursor-plugin/plugin.json
agents/   (8+)
skills/   (core + personal)
commands/ (8)
rules/    (always-on + csharp/ts/python)
hooks.json + hooks/
mcp.json.example
AGENTS.md
```

Push to `grantl1274/ECC_` **or** a new small repo like `grantl1274/cursor-harness`. Then delete the full ECC clone.

---

## 4. Exact KEEP inventory (machine checklist)

Use this as a delete-guard. If a path is not listed, it is deletable after DAILY is restored.

### Agents KEEP
- planner, architect, code-reviewer, security-reviewer, tdd-guide, build-error-resolver, refactor-cleaner, doc-updater
- csharp-reviewer, typescript-reviewer, python-reviewer, docs-lookup
- (optional) react-reviewer, react-build-resolver

### Skills KEEP
- search-first, tdd-workflow, verification-loop, security-review, continuous-learning(+v2), configure-ecc
- dotnet-patterns, csharp-testing, python-patterns, python-testing, coding-standards
- git-workflow, documentation-lookup, mcp-server-patterns
- (optional) frontend-patterns, backend-patterns, codebase-onboarding, context-budget, plan-canvas

### Commands KEEP
- plan, code-review, quality-gate, checkpoint, learn, security-scan, build-fix, refactor-clean
- (optional) tdd, e2e, verify if present and used

### Rules KEEP
- all `common-*` that exist for coding-style, testing, security, git, agents, patterns, performance, hooks, development-workflow, code-review
- `csharp-*`, `typescript-*`, `python-*`
- cleanroom: research-first, verification-before-done, security-hygiene, git-safety, token-efficiency, subagent-delegation, typescript-essentials, python-essentials

### Docs KEEP (from this repo)
- `docs/PERSONAL-KEEP-RECREATE.md` ← **this file**
- `docs/CURSOR-USER-MANUAL.md` (+ `.docx` if you want human-readable)
- Root `AGENTS.md` only if trimmed to the keep agents (else replace with cleanroom AGENTS.md)

### Config KEEP
- `.cursor/ecc-agent-data.json`
- minimal `.cursor/mcp.json` (Context7 + Revit only)
- cleanroom `hooks.json` (+ hook scripts)

---

## 5. LIBRARY — keep reachable, not always loaded

Useful occasionally; install on demand or leave in a sidebar folder / skill router, **not** alwaysApply:

| Item | When to pull |
|------|----------------|
| `ecc-e2e-runner` + `e2e-testing` skill | browser/critical flows |
| `ecc-spec-miner` | brownfield → OpenSpec |
| `ecc-harness-optimizer` | cost/reliability tuning |
| `deep-research` / `exa-search` | heavy research (needs MCP) |
| `frontend-design-direction` / `frontend-a11y` | design/a11y passes |
| `postgres-patterns` / `database-reviewer` | when touching DB |
| `security-scan` skill + AgentShield docs | periodic audits |
| Full language reviewers (go/rust/java/…) | only if a project uses them |
| `dmux-workflows` | only if you actually run dmux/tmux multi-agents |

---

## 6. DELETE — safe to remove from local disk after recreate

After DAILY (sections 1–4) works, delete from local workspace:

### High-noise domains (you have no evidence of using them)
- Homelab / network / Cisco / WireGuard / Pi-hole skills & agents
- Supply-chain / logistics / customs / energy-procurement skills
- Healthcare / HIPAA packs (unless a project starts)
- ML / PyTorch / recsys packs (unless a project starts)
- Investor / marketing / brand / content-engine / X-api / crosspost / fal-ai / video editing packs
- GAN harness agents (`gan-*`) unless you run that loop
- Visa / nutrient document processing
- Prediction-market / DeFi / crypto token skills
- Scientific / PubMed / USPTO packs
- HarmonyOS / ArkTS / Flutter / Swift / Kotlin / Quarkus / Laravel / Spring Boot / Nest / Nuxt packs **unless** a live project needs one

### Install noise
- Full `.cursor/skills/` except KEEP list (278 → ~15–25)
- Full `.cursor/agents/` except KEEP list (67 → ~12)
- Full `.cursor/commands/` except KEEP list (94 → ~8–12)
- All `rules/*` language packs not csharp/typescript/python/react/web/common
- `.cursor/ecc-install-state.json` (regenerable)
- Upstream release/video/Hermes/dashboard/control-plane trees if not operating ECC as a product
- Duplicate installs (Cursor plugin + project full copy + Claude full copy) — pick one primary

### Process note
Deleting from disk ≠ deleting from `grantl1274/ECC_` remote. Prefer: keep remote as archive, local = lean harness only.

---

## 7. Verification checklist

After recreate:

```powershell
# Cursor plugin present
Test-Path "$env:USERPROFILE\.cursor\plugins\local\ecc-cleanroom\.cursor-plugin\plugin.json"

# Or project pack counts (example targets)
# agents ~8–12, skills ~15–25, commands ~8–12, rules = common + 1–3 language packs
```

Manual:
- [ ] `/plan` works or planner agent is invokable
- [ ] `/quality-gate` or verification-loop skill available
- [ ] Security rule blocks committing secrets / skip-hooks
- [ ] Context7 MCP answers a docs question
- [ ] Revit MCP responds (if Revit add-in work)
- [ ] Editing a `.cs` file attaches csharp rules
- [ ] Editing a `.ts`/`.py` file attaches those packs
- [ ] No hundreds of unused skills appear in every session context
- [ ] Claude Code (if used) does not duplicate Cursor full install

---

## 8. Suggested one-prompt for another AI

Copy this prompt with this file attached:

```text
Read docs/PERSONAL-KEEP-RECREATE.md.

Recreate ONLY the DAILY harness for Grant on this Windows machine:
1) Ensure ecc-cleanroom (or equivalent) exists under %USERPROFILE%\.cursor\plugins\local\
2) For the current project, install only KEEP agents/skills/commands/rules from the checklist
3) Wire mcp.json to Context7 + Revit only (no other MCP templates)
4) Remove or ignore DELETE-bucket skills/agents/rules from local disk
5) Run the verification checklist and report what was kept vs deleted

Do not reinstall the full ECC surface. Do not commit unless asked.
```

---

## 9. Why this cut (short evidence)

| Evidence | Implication |
|----------|-------------|
| `ecc-cleanroom` local plugin already curated | You want lean core, not 278 skills |
| `user-revit-mcp` enabled | C# / Revit pack is DAILY-adjacent |
| PowerShell + Windows operator rules | Windows install paths; PS safety matters |
| Context7 in MCP | `documentation-lookup` / docs-lookup agent worth keeping |
| Prior installs tried full Cursor + Claude | Duplication risk; pick Cursor primary |
| Full profile install then “rest can be deleted” | You explicitly want a recreate-spec, not the mega-tree daily |
| User rules: TDD, verification, security, git safety, plan-first | Match cleanroom core loop |

---

## 10. Status snapshot

| Item | Location |
|------|----------|
| ECC clone (source still complete under repo root) | `C:\AI\ECC` |
| **Trimmed project Cursor pack** | `C:\AI\ECC\.cursor` — DAILY KEEP only (trim run) |
| Lean plugin | `%USERPROFILE%\.cursor\plugins\local\ecc-cleanroom` |
| Claude Code install | `%USERPROFILE%\.claude` (full; not trimmed yet) |
| Public fork | `https://github.com/grantl1274/ECC_` |
| This recreate spec | `docs/PERSONAL-KEEP-RECREATE.md` |

### Trim results (`.cursor/` project pack)

| Surface | Before | After |
|---------|--------|-------|
| Agents | 67 | 14 |
| Skills | 278 | 22 |
| Commands | 94 | 9 |
| Rules | 160 | 38 (common + csharp/ts/python/react/web; RN + duplicate .md dropped) |

Removed: non-KEEP agents/skills/commands/rules, `ecc-install-state.json`, `mcp-configs/`, `.agents/`, project `the-security-guide.md`.  
Kept: `hooks.json` + `hooks/` + `scripts/` (runtime), `ecc-agent-data.json`, empty project `mcp.json` (user MCP stays in Cursor settings).
