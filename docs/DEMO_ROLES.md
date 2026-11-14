# Demo work split — Jimmy + Kevin (paste into Claude Code)

Paste this into Claude Code from the team repo root, then add one line at the top: **"I am Jimmy"** or **"I am Kevin"**.
Claude: work only on that person's half. Act as the main coordinator: plan first, use **subagents** for independent pieces (give each full context and the exact files it may touch), and **load every relevant skill** before building (frontend/UI design, 3D/animation, testing, video). Re-read the codebase first — it changes fast. Show a short plan and wait for OK before coding. Never commit CoStar data or API keys (data in a gitignored folder, keys in `.env`).

## The demo (2 minutes, flood in Houston)
Beat order:
1. **Warning** — "Flood incoming" + severity dial (2 / 4 / 6 ft).
2. **Before** — a person at home; the flood wrecks the house; loss counter races to **−$X**.
3. **Agent** — budget $500K + limited time (e.g. 48 hours). The agent researches flood fixes, tags them quick vs. long-term, and picks the plan that avoids the most loss per dollar. Show its steps — no chat window.
4. **Fixes go in** — aerial view; stylized 3D fixes pop into place (sandbags, temporary barriers, raised equipment, trimmed trees); flood re-runs; loss drops to **−$Y**.
5. **After** — same person inside, totally relaxed while the flood rages outside.
6. **Summary** — "$500K spent → $Z saved" + work-order button.
Optional: VR split-screen moment; phone photo → insurance claim summary.

## Shared contract (agree on this FIRST, both halves read it)
One scenario file, e.g. `demo/scenario.json`:
```json
{
  "property": { "name": "", "address": "", "value": 0, "sqft": 0, "aerialImage": "" },
  "flood": { "severityFt": 4, "hoursBeforeFlood": 48 },
  "budget": 500000,
  "lossBefore": 0,
  "lossAfter": 0,
  "fixes": [
    { "id": "barrier", "name": "", "cost": 0, "hours": 0, "type": "quick|long-term",
      "lossAvoided": 0, "selected": true, "position": { "x": 0, "y": 0 } }
  ]
}
```
Use fake numbers until Tazeem's agent outputs real ones in this same shape.

## Kevin — everything INSIDE the app
- Warning screen + severity dial
- Animated loss counter (−$X → −$Y)
- Agent plan panel: steps, fixes, cost, loss avoided, quick/long-term tags
- **3D aerial view** (e.g. three.js): Vexcel image as the ground, rising cartoon water, fixes popping into place at `fixes[].position`
- Summary screen + work-order button
- Clean screen recordings of each app beat → hand to Jimmy
- Don't build the AI video clips, VR mode, or final edit (Jimmy's)

## Jimmy — everything OUTSIDE the app + the final video
- **AI video clips** (image-to-video tool, e.g. Veo or Kling): (1) house wrecked by the flood, (2) same person relaxed while the flood rages outside. Start both from the same image so the room matches. Generic props only — no brand logos or known characters.
- **The person:** photo of Ryan (or a phone 3D scan → Mixamo rig, only if he appears in the 3D scene)
- **VR mode:** a split-screen stereo view of Kevin's aerial scene for the phone headset (iPhone Safari has no WebXR — prefer an Android phone, or use a plain side-by-side stereo render), plus footage of someone wearing it
- **Final edit:** clips + Kevin's recordings in beat order, voiceover + music, under 2 minutes
- **Backup video** that plays offline
- Optional: phone photo → agent updates the loss + drafts an insurance claim summary
- Don't edit Kevin's app screens or 3D view code — request changes through Kevin

## Handoffs
- **~4pm:** `scenario.json` agreed; Kevin has a rough aerial view; Jimmy has one test AI clip
- **~9pm:** Kevin sends first screen recordings; Jimmy makes a rough cut
- **Late tonight:** swap in Tazeem's real numbers; Kevin re-records
- **Sunday morning:** final edit + backup video

## Done when
- Full loop plays in under 2 minutes, three runs in a row
- Every number on screen comes from CoStar/Vexcel data (or a clearly stated assumption)
- Backup video exists and plays offline
