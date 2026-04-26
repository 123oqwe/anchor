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
  ┌────────────┐                              ┌──────────────────────────────────┐
  │ anchor-    │   HTTP / SSE                 │   anchor-*-mcp servers (8 total) │
  │ backend    │ ◀──────── anchor-frontend    │   (each runs as a subprocess)    │
  │ (brain)    │           (Vite/React UI)    │                                  │
  │            │                              │   anchor-shell-mcp    (CLI)      │
  │ + admin :  │           anchor-admin       │   anchor-activity-mcp (window)   │
  │ 3002       │           (operator UI)      │   anchor-browser-mcp  (history)  │
  └─────┬──────┘                              │   anchor-input-mcp    (kbd/click)│
        │ MCP host                            │   anchor-screen-mcp   (vlm)      │
        │ ───────────────────────────────────▶│   anchor-system-mcp   (sysinfo)  │
        │                                     │   anchor-code-mcp     (git)      │
        │                                     └──────────────────────────────────┘
        │
        │ Plus: any 3rd-party MCP (gmail-mcp, apple-mcp, notion-mcp, linear-mcp...)
        ▼
  ┌──────────────────────────────────────┐
  │  Decision / Twin / Custom Agent +    │
  │  Memory + Knowledge Graph + Cron      │
  │  (all in anchor-backend)              │
  └──────────────────────────────────────┘
```

## Try it

```bash
# Once @anchor scope is published to npm:
npx -y @anchor/tray-app start

# For now (pre-publish), clone all 9 repos + run:
git clone https://github.com/123oqwe/anchor-backend ~/anchor-backend
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

## The 10 active repos

### Core (must run together)

| Repo | Role |
|------|------|
| **[anchor-backend](https://github.com/123oqwe/anchor-backend)** | Brain — cognition / memory / orchestration / 31 tools (~37k lines) |
| **[anchor-frontend](https://github.com/123oqwe/anchor-frontend)** | User UI — Dashboard / Advisor / Twin / Memory / Workspace |
| **[anchor-tray-app](https://github.com/123oqwe/anchor-tray-app)** | Launcher — Node CLI supervisor today + Tauri scaffold for future tray app |

### MCP servers (Cross-platform, each runs as a subprocess)

| Repo | Tools | What it does |
|------|-------|------|
| **[anchor-shell-mcp](https://github.com/123oqwe/anchor-shell-mcp)** | `shell_run` + 3 helpers | Safe shell exec — token-efficient alternative to wrapping every CLI |
| **[anchor-activity-mcp](https://github.com/123oqwe/anchor-activity-mcp)** | 4 | Active window tracking via `active-win` |
| **[anchor-browser-mcp](https://github.com/123oqwe/anchor-browser-mcp)** | 4 | Browser history reader (Chrome / Brave / Edge / Arc / Firefox / Safari) |
| **[anchor-input-mcp](https://github.com/123oqwe/anchor-input-mcp)** | 5 | Desktop GUI control — keystroke / click / type / screenshot |
| **[anchor-system-mcp](https://github.com/123oqwe/anchor-system-mcp)** | 4 | sysinfo + installed apps + package managers |
| **[anchor-screen-mcp](https://github.com/123oqwe/anchor-screen-mcp)** | 4 | Screen capture + vision LLM (Anthropic / OpenAI / Ollama) |
| **[anchor-code-mcp](https://github.com/123oqwe/anchor-code-mcp)** | 5 | Git-repo activity (active repos / commits / languages / themes) |

### Developer / operator tooling

| Repo | Role |
|------|------|
| **[anchor-admin](https://github.com/123oqwe/anchor-admin)** | Operator panel UI (port 3003 → backend admin :3002) |

### Experimental

| Repo | Role |
|------|------|
| **[anchor-core](https://github.com/123oqwe/anchor-core)** | 5-layer simplified re-implementation (~3500 lines). For exploration; production path is anchor-backend. |

### Archived (do not use)

- [anchor-archive](https://github.com/123oqwe/anchor-archive) — original monorepo before split
- [anchor-ui](https://github.com/123oqwe/anchor-ui) — Manus-built React prototype (superseded by anchor-frontend)
- [openclaw](https://github.com/123oqwe/openclaw) — earlier "lobster way" iteration
- [skills](https://github.com/123oqwe/skills) — clawhub skills archive

## Architecture in one paragraph

Anchor's brain (anchor-backend) holds your Personal Knowledge Graph, episodic + semantic memory, a Decision Agent, a Twin Agent that learns from your edits, and 7 autonomous-evolution mechanisms (Dream, GEPA, Skills crystallization, etc.). All OS-specific work — keystroke / screen / browser sqlite / system inspection — runs in independent MCP server subprocesses so the brain stays cross-platform. Plug in any 3rd-party MCP server (gmail-mcp, apple-mcp, notion-mcp...) to give anchor more eyes.

## Design principles (Karpathy-aligned)

1. **Think before coding** — every commit traces to a real product feature.
2. **Simplicity first** — if 200 lines could be 50, rewrite.
3. **Surgical changes** — don't refactor what isn't broken.
4. **Goal-driven execution** — every commit has a verifiable success criterion.

## License

MIT (each repo separately licensed; same MIT terms across the ecosystem).

## Status

🚧 Pre-launch. Targeting developer-audience release shortly. SaaS / signed-binary distribution comes later.
