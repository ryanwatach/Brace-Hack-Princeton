# Demo spec — Flood stress test + budget planner (HackPrinceton Fall 2026)

Paste this into Claude Code (or share with the team) so everyone builds toward the same demo.
Claude: read this, compare it to the current codebase, list what exists vs. what's missing, and propose who builds which piece. Use subagents for independent pieces and load relevant skills (frontend/UI design, testing, 3D/animation) before building. Don't edit a teammate's area without checking with them.

## The pitch in one line
"Do nothing and this home/portfolio loses $X in the next flood. You have $500K and limited time — our agent tells you exactly what to do to lose the least."

## Constraints
- **Disaster:** flood only, Houston.
- **No weather forecasting.** It's a what-if simulation with one severity dial (e.g. 2 / 4 / 6 ft of water).
- **Data:** only sponsor data — Vexcel (ground elevation and surrounding terrain, trees, roof, exterior attributes, the new hurricane data) and CoStar (value, square footage, tenants). Matterport is optional (inside shots of the real home).
- **Never commit** CoStar data or API keys (data folder in `.gitignore`, keys in `.env`).
- **Total length: 2 minutes, hard cap.**

## The demo loop (story version)

### Beat 1 — Warning (0:00–0:10)
- Full-screen alert: **"Flood incoming"** on a real Houston home (pick a CoStar residential listing that has a Matterport scan, if possible).
- Severity dial visible (e.g. "4 ft of water").

### Beat 2 — Do nothing (0:10–0:35)
- Inside the house: a person (3D-scanned teammate, stylized) sitting at home.
- The flood hits; water rises, the room gets wrecked.
- A loss counter races up: **−$X** (big, red, dramatic).
- Quick "why" callouts pulled from data: low spot at the bottom of a sloping street (Vexcel elevation), tall trees over the roof (Vexcel), property value (CoStar).

### Beat 3 — The agent steps in (0:35–1:05)
- Prompt on screen: "Budget: $500K. Time before the flood: 48 hours."
- The agent visibly works (show its steps, not a chat box):
  1. Researches common flood-prevention measures and their costs.
  2. Tags each fix by time needed — **quick (hours):** sandbags / temporary flood barriers, move valuables and equipment up, clear drains, board windows; **long-term (weeks):** impact windows, raised utilities, permanent barriers.
  3. Picks the plan that avoids the most loss per dollar within the budget AND the time left.
  4. Explains trade-offs in one line each (e.g. "temporary barriers here instead of permanent walls — they fit the 48 hours").
- Output: a short ranked plan with cost and loss avoided per fix.

### Beat 4 — Fixes go in (1:05–1:30)
- **Aerial view** of the front and sides of the house (Vexcel imagery as the base).
- The fixes **animate into place** — stylized 3D models popping in like a satisfying construction reel (barriers rise, sandbags stack, trees get trimmed).
- Re-run the flood: water hits the barriers; the loss counter stops much lower: **−$Y**. Show "**$500K spent → $Z saved**."

### Beat 5 — Calm inside (1:30–1:45)
- Same person inside the same house, totally relaxed (meditating or sipping a drink) while the flood rages outside the window.
- Optional joke: he's on his laptop asking Claude to build him a hackathon-winning project.
- Use a generic drink can — no brand logos.

### Beat 6 — Action + close (1:45–2:00)
- One click: the agent drafts the contractor work order and an owner/tenant notice.
- Optional (only if built and time allows): send a pre-made "after the flood" photo from a phone → the agent updates the damage estimate and drafts an insurance claim summary.
- Closing line: "Do nothing: −$X. Our plan: $500K spent, $Z saved. Built on CoStar and Vexcel data."

## Optional interactive mode (if time)
Before Beat 3, let the judge place their own fixes with the same budget, run the flood, then show the AI's plan beating them: "Can you beat the AI?" Keep it simple: map + budget + run button + loss counter. Don't copy any existing game's name or look.

## Assets needed
- One demo home: address, CoStar value/sq ft, Vexcel imagery + elevation, Matterport scan (if available).
- Stylized 3D person (phone 3D scan of a teammate, cleaned up).
- Stylized 3D fix models: sandbags, temporary flood barrier, permanent barrier, raised equipment platform, window boards/impact windows, trimmed tree.
- Water / flood animation for inside and aerial views.
- Loss counter component (animated number).
- A pre-made "after the flood" photo (for the optional insurance beat).
- A recorded backup video of the full loop.

## Who owns what (confirm in the team chat)
- **Tazeem (P1):** agent logic (loss estimate, research, budget/time optimization, explanations), data wiring, final narration.
- **Kevin (P2):** dashboard UI, loss counter, plan display, aerial view screen.
- **Ryan (P4):** backend, saved runs, work-order/notice drafts, photo upload endpoint; also the 3D-scanned "person."
- **Jimmy (P3):** phone photo → claim summary beat (optional), plus whichever demo piece the team assigns (e.g. the fix animations or inside-the-house scenes), and the backup video.

## Definition of done
- The full loop runs start to finish in under 2 minutes, three times in a row, on the demo laptop.
- Every number on screen traces back to CoStar or Vexcel data (or a clearly stated assumption).
- The agent's steps are visible — no chat window.
- A backup video exists and plays offline.
