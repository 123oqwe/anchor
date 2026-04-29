# ⚓ Anchor

> Local-first personal AI runtime. Cross-platform. MCP-first. Your data stays on your machine.

Anchor is split across multiple repos so each piece ships independently. **This repo is the navigation hub.**

```
                          ┌──────────────────────┐
                          │  anchor-tray-app     │  one-line install + supervisor
                          │  (CLI today, Tauri   │
                          │   tray app planned)  │
                          └──────────┬───────────┘
                                     │ spawns + supervises
        ┌────────────────────────────┴────────────────────────────┐
        ▼                                                          ▼
  ┌──────────────────────┐                       ┌──────────────────────────────────┐
  │ anchor-backend       │   HTTP / SSE          │  anchor-*-mcp servers (7)        │
  │ :3001  (brain)       │ ◀──── anchor-frontend │  (each runs as a subprocess)     │
  │                      │       (Vite/React)    │                                  │
  │ Decision / Twin /    │                       │  shell-mcp     (CLI)             │
  │ Custom Agent +       │       anchor-admin    │  activity-mcp  (window)          │
  │ Memory + Graph +     │       (operator UI)   │  browser-mcp   (history)         │
  │ Cron + 31 tools      │                       │  input-mcp     (kbd/click)       │
  └──┬───────────────────┘                       │  screen-mcp    (vlm)             │
     │ shared anchor.db (WAL multi-process)      │  system-mcp    (sysinfo)         │
     │                                           │  code-mcp      (git)             │
     ▼                                           └──────────────────────────────────┘
  ┌──────────────────────┐
  │ anchor-admin-backend │ ◀── anchor-admin (operator UI :3003)
  │ :3002 (operator API) │
  └──────────────────────┘
     │
     ▼ (proxies /api/security/*)
  ┌──────────────────────┐
  │ anchor-security      │   out-of-band detection / pentest / forensics
  │ :3004                │   reads anchor.db read-only; pushes alerts
  └──────────────────────┘

  Plus: any 3rd-party MCP (gmail-mcp, apple-mcp, notion-mcp, linear-mcp...)
```

## Try it

```bash
# Once @anchor scope is published to npm:
npx -y @anchor/tray-app start

# For now (pre-publish), clone the active repos + run:
git clone https://github.com/123oqwe/anchor-backend ~/anchor-backend
git clone https://github.com/123oqwe/anchor-admin-backend ~/anchor-admin-backend
git clone https://github.com/123oqwe/anchor-security ~/anchor-security
git clone https://github.com/123oqwe/anchor-frontend ~/anchor-frontend
git clone https://github.com/123oqwe/anchor-admin ~/anchor-admin
git clone https://github.com/123oqwe/anchor-tray-app ~/anchor-tray-app
git clone https://github.com/123oqwe/anchor-shell-mcp ~/anchor-shell-mcp
git clone https://github.com/123oqwe/anchor-activity-mcp ~/anchor-activity-mcp
git clone https://github.com/123oqwe/anchor-browser-mcp ~/anchor-browser-mcp
git clone https://github.com/123oqwe/anchor-input-mcp ~/anchor-input-mcp
git clone https://github.com/123oqwe/anchor-system-mcp ~/anchor-system-mcp
git clone https://github.com/123oqwe/anchor-screen-mcp ~/anchor-screen-mcp
git clone https://github.com/123oqwe/anchor-code-mcp ~/anchor-code-mcp

cd ~/anchor-tray-app && pnpm install && pnpm build
ANTHROPIC_API_KEY=sk-... node dist/cli.js start --dev

# Open http://localhost:3001
```

## The 13 active repos

### Core runtime (must run together)

| Repo | Port | Role |
|------|------|------|
| **[anchor-backend](https://github.com/123oqwe/anchor-backend)** | 3001 | Brain — cognition / memory / orchestration / 31 tools (~37k lines, 8 layers) |
| **[anchor-admin-backend](https://github.com/123oqwe/anchor-admin-backend)** | 3002 | Operator API — RBAC / audit / observability / growth / blocklist (Sprint 5) |
| **[anchor-security](https://github.com/123oqwe/anchor-security)** | 3004 | Out-of-band security — 4 detectors / 69-attack pentest / alerts / forensics (Sprint 6) |
| **[anchor-frontend](https://github.com/123oqwe/anchor-frontend)** | 3000 | User UI — Dashboard / Advisor / Twin / Memory / Workspace |
| **[anchor-admin](https://github.com/123oqwe/anchor-admin)** | 3003 | Operator UI — Users / Audit / Experiments / Growth / Security |
| **[anchor-tray-app](https://github.com/123oqwe/anchor-tray-app)** | — | Launcher — Node CLI supervisor today + Tauri scaffold for future tray app |

### MCP servers (cross-platform, each runs as a subprocess)

| Repo | Tools | What it does |
|------|-------|------|
| **[anchor-shell-mcp](https://github.com/123oqwe/anchor-shell-mcp)** | `shell_run` + 3 helpers | Safe shell exec — token-efficient alternative to wrapping every CLI |
| **[anchor-activity-mcp](https://github.com/123oqwe/anchor-activity-mcp)** | 4 | Active window tracking via `active-win` |
| **[anchor-browser-mcp](https://github.com/123oqwe/anchor-browser-mcp)** | 4 | Browser history reader (Chrome / Brave / Edge / Arc / Firefox / Safari) |
| **[anchor-input-mcp](https://github.com/123oqwe/anchor-input-mcp)** | 5 | Desktop GUI control — keystroke / click / type / screenshot |
| **[anchor-system-mcp](https://github.com/123oqwe/anchor-system-mcp)** | 4 | sysinfo + installed apps + package managers |
| **[anchor-screen-mcp](https://github.com/123oqwe/anchor-screen-mcp)** | 4 | Screen capture + vision LLM (Anthropic / OpenAI / Ollama) |
| **[anchor-code-mcp](https://github.com/123oqwe/anchor-code-mcp)** | 5 | Git-repo activity (active repos / commits / languages / themes) |

### Documentation

| Repo | Role |
|------|------|
| **[anchor-os-spec](https://github.com/123oqwe/anchor-os-spec)** | Type-level architecture documentation. 12 canonical flows + result classes + lifecycle FSM as strict TypeScript. **Not a runtime dependency** — no production code imports it; use it as a design reference when reasoning about contracts. |

### Archived (do not use)

- [anchor-core](https://github.com/123oqwe/anchor-core) — 6-layer simplified re-implementation (~3500 lines). **Archived 2026-04-27**: parallel implementation conflicted with anchor-backend; one-engineer team can't sustain two codebases. anchor-backend is the only path forward.
- [anchor-archive](https://github.com/123oqwe/anchor-archive) — original monorepo before split
- [anchor-ui](https://github.com/123oqwe/anchor-ui) — Manus-built React prototype (superseded by anchor-frontend)
- [openclaw](https://github.com/123oqwe/openclaw) — earlier "lobster way" iteration
- [skills](https://github.com/123oqwe/skills) — clawhub skills archive

## Architecture in one paragraph

Anchor's brain (anchor-backend) holds your Personal Knowledge Graph, episodic + semantic memory, a Decision Agent, a Twin Agent that learns from your edits, and 7 autonomous-evolution mechanisms (Dream, GEPA, Skills crystallization, etc.). All OS-specific work — keystroke / screen / browser sqlite / system inspection — runs in independent MCP server subprocesses so the brain stays cross-platform. anchor-admin-backend gives a single operator a cockpit (RBAC / audit / experiments / growth) over the same shared sqlite, and anchor-security runs out-of-band detection + pentest against the live system without ever touching the user request path. Plug in any 3rd-party MCP server (gmail-mcp, apple-mcp, notion-mcp...) to give anchor more eyes.

## Design principles (Karpathy-aligned)

1. **Think before coding** — every commit traces to a real product feature.
2. **Simplicity first** — if 200 lines could be 50, rewrite.
3. **Surgical changes** — don't refactor what isn't broken.
4. **Goal-driven execution** — every commit has a verifiable success criterion.

## License

MIT (each repo separately licensed; same MIT terms across the ecosystem).

## Status

🚧 Pre-launch. Targeting developer-audience release shortly. SaaS / signed-binary distribution comes later.
