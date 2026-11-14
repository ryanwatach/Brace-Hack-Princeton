# Brace: HackPrinceton Fall 2026 (CoStar track)

Direction as of **Nov 14, ~1:15 pm**. The demo script is **[`docs/DEMO_SPEC.md`](docs/DEMO_SPEC.md)**; build toward it. When older docs (`HANDOFF.md`, `PRD.md`, `DEMO.md`, `docs/recovery-handoffs/`) conflict with this file or the spec, this file and the spec win. `DEMO.md` describes the earlier forecast demo and is superseded.

## What we're building
A **flood stress test + budget planner for real estate portfolios.**

> "Do nothing and this portfolio loses $X in the next flood. You have $500K — our agent tells you exactly where to spend it to lose the least."

- **Hazard:** floods only, in **Houston** only.
- **Scenario:** a what-if ("flood incoming") with **one severity dial** (e.g. 2 / 4 / 6 ft of water) that gives an estimated loss per property. It is **not a weather forecast**: no weather or forecast APIs.
- **Two limits:** a budget (e.g. $500K) **and** the time left before the water arrives (e.g. 48 h). Quick fixes take hours (sandbags, temporary barriers, moving equipment up, clearing drains, boarding windows); long-term fixes take weeks (impact windows, raised utilities, permanent barriers).
- **Data, sponsor-provided only:**
  - **Vexcel:** ground elevation around each property, using the surrounding terrain (low spots and downhill streets flood first), not just the building footprint. Also tall trees near buildings, roof condition and other exterior attributes, plus the hurricane imagery Vexcel added on Nov 14.
  - **CoStar kit:** value, square footage and tenants, which turn risk into dollars.
  - **Matterport:** optional. The exterior matters more for floods.
- **The agent** must reason, plan and act. It is not a chat window. It:
  1. Estimates each property's loss if the owner does nothing.
  2. **Researches** flood-prevention measures (exterior protections, rough cost, effort) rather than using a hardcoded list.
  3. Within the budget **and the time left**, picks the mix of fixes that avoids the most loss per dollar and weighs the trade-offs. For example, shutters are cheap but a hassle and unattractive; impact windows cost more but are always on. There is no single right answer; different budgets give different plans.
  4. Explains its choices, then drafts work orders and owner notices.

## Demo (2 minutes, hard cap): full script in [`docs/DEMO_SPEC.md`](docs/DEMO_SPEC.md)
1. **Warning (0:00–0:10):** "Flood incoming" on a real Houston home, with the severity dial visible.
2. **Do nothing (0:10–0:35):** water rises inside the home, and a loss counter races to **−$X**, with "why" callouts from Vexcel and CoStar.
3. **Agent (0:35–1:05):** given $500K and 48 h, it researches fixes, tags each one quick or long-term, and picks the best plan. It shows its steps, not a chat box.
4. **Fixes go in (1:05–1:30):** fixes animate into place on the aerial view, the flood re-runs, and the counter stops at **−$Y**: "$500K spent → $Z saved".
5. **Calm inside (1:30–1:45):** the same person, relaxed, while the flood rages outside.
6. **Action (1:45–2:00):** one click drafts the work order and notice. Optional: a phone photo produces a damage update and claim summary.

Optional mode: "Can you beat the AI?" The judge places fixes first, then the AI's plan beats them. Keep it to a map, a budget, a run button and a loss counter. Don't copy any existing game's name or look.

**Done means:** the loop runs start to finish in under 2 minutes, three times in a row. Every number traces back to CoStar or Vexcel data, or to a stated assumption. The agent's steps are visible. A backup video plays offline.

**Lead with dollars.** CoStar's feedback: the pitch and the demo matter most.

## Tracks and judging
- Main track: **Business & Enterprise.** Special track: **CoStar**.
- CoStar scoring: 30% use of their data, 30% agentic depth, 25% usefulness, 15% demo and polish.

## Cut (don't extend; treat existing code as legacy)
Weather forecasting (NHC/NWS forecast replays), VR or the headset as core, evacuation and wheelchair routes, text-message warnings, tornado and wildfire (Los Angeles) scenarios, and the earlier SitePilot Health idea.

## Team and file ownership (don't edit other people's code)
| Who | Owns |
|---|---|
| **Tazeem (P1, lead)** | Data, valuation, agent reasoning, architecture, final demo. All existing app files unless reassigned (`App.tsx`, `backend/server.py`, `backend/recovery/domain/**`, `frontend/src/recovery/**`, configs, manifests) |
| **Kevin (P2)** | Owner dashboard UI: `frontend/src/components/recovery/**` |
| **Ryan (P4)** | Backend platform: `backend/recovery/platform/**` (cases, access, uploads, live updates) |
| **Jimmy (P3)** | Phone and resident side: `frontend/src/components/resident/**`. Per the demo spec: the phone photo → claim summary beat (optional), plus whichever demo piece the team assigns (e.g. fix animations or inside-the-house scenes), and the backup video |

The demo spec also assigns Kevin the loss counter, plan display and aerial view screen, and gives Ryan the saved runs, work-order/notice drafts, photo upload endpoint and the 3D-scanned "person". Confirm assignments in the team chat.

Anything touching another person's area goes through Jimmy to that teammate, as a request or a small proposed diff.

## Working rules
- **Work on a branch and open a PR. Never push to `main`.** Fetch and merge `main` before every push, and rerun the tests. Teammates sometimes commit inside other people's folders.
- **Never commit data or keys.** `data/` (CoStar kit, caches, recovery DB) and `.env` are git-ignored. Check `git status` before every commit.
- Use real data only. A missing source shows as unavailable, never invented. Unknown money is `null`, never `$0`.
- Load the relevant skills before building (UI/design, testing, framework).

## Run it
```bash
cp .env.example .env            # Matterport, Vexcel, Anthropic keys; recovery settings
unzip -P '<zip password from the CoStar Discord /costar command>' costar-hackathon-kit.zip -d data/kit
uv sync && (cd frontend && npm install)
uv run uvicorn backend.server:app --host 127.0.0.1 --port 8001   # API
(cd frontend && npm run dev)                                     # laptop app: http://localhost:5174 (must be localhost)
(cd frontend && npx vite --config vite.vr.config.ts)             # HTTPS app for phones on the LAN: :5175
```

## Checks
```bash
uv run pytest                                   # backend (run without recovery settings in .env, or legacy routes 401)
cd frontend && npx tsc --noEmit && npm run test:recovery
node --test src/components/resident/__tests__/*.test.mjs        # resident (P3)
```
