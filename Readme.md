# 🥾 Deutschland Hiking Planner

> Your friendly trail companion for planning day hikes across Germany — all reachable with just your **Deutschlandticket**. 🚆🌲

Built for **OpenAI Build Week** with **Codex + GPT-5.6**, this is an [Agent Skill](https://developers.openai.com/) that turns any capable coding agent into a thoughtful hiking guide. Tell it your home city, and it finds real, transit-accessible hikes, checks that the trains and buses actually run, and even hands you a **printable offline map + GPX** so you never get lost when the signal drops.

---

## 🤖 How Codex & GPT-5.6 were used

- 🏗️ **Codex built the skill** — I designed and iterated on `Skill.md` and the `hike_state.py` helper with Codex, turning a plain-language workflow into structured instructions, CLI code, and safety guardrails.
- 🧠 **GPT-5.6 is the reasoning engine** — at runtime it cross-references trail data, transit timetables, and Deutschlandticket rules, then reasons about eligibility, return margins, and daylight.
- 🔗 **Agentic tool use** — Codex + GPT-5.6 drive live web research, run the local state script, and generate the offline PDF + GPX, chaining multiple steps from a single request.
- ⚡ **Nuanced instruction-following** — GPT-5.6 reliably respects rules like "never invent a water source," "flag seasonal service," and "always tell the user to recheck departures."

---

## ✨ What it does

- 🏙️ **Remembers your city** — save your starting point once, locally on your machine.
- 🔎 **Finds real hikes** — discovers trails with verified distance, elevation, and route data.
- 🎫 **Checks Deutschlandticket coverage** — no ICE/IC surprises; it verifies regional trains & buses actually include you.
- ⏱️ **Plans the round trip** — confirms you can get home after a realistic day on the trail.
- 🗺️ **Builds an offline kit** — a print-friendly A4 PDF with landmarks, decision points, coordinates, and safety notes, plus the route GPX.
- 📓 **Keeps a hike log** — records the trails you've completed so it never sends you on the same one twice.

Everything it tells you separates **verified facts** from **assumptions** — so you can trust the plan and always recheck connections before you go.

---

## 🚀 How to use it

### In Codex (recommended)

1. Drop this skill folder into your Codex skills directory (or point Codex to it).
2. Make the state helper available — it stores data locally at `$CODEX_HOME/hiking-planner/state.json`.
3. Just ask, naturally:

   > “I live in Freiburg. Find me an easy hike this Saturday I can reach with my Deutschlandticket.”

   Codex reads the skill, saves your city, researches trails, verifies transit, and proposes a route. 🎉

4. Love a route? Ask for the map:

   > “Make me an offline map and GPX for that one.”

5. Back home? Log it:

   > “I did the Belchen summit loop today — add it to my history.”

### In other agents (Claude, Cursor, custom agents…)

This skill is just **Markdown + a small Python helper**, so it works anywhere an agent can read files and run scripts:

1. Load `Skill.md` as the agent's instructions (system prompt, tool skill, or context file).
2. Give the agent shell access so it can run the state helper:

   ```bash
   python3 scripts/hike_state.py show
   python3 scripts/hike_state.py set-city "Freiburg im Breisgau"
   python3 scripts/hike_state.py add-hike --title "Belchen summit loop" --date 2026-07-21 \
     --distance-km 12.4 --ascent-m 750 --start "Münsterplatz" --finish "Münstertal" \
     --notes "Sunny; bring water"
   python3 scripts/hike_state.py delete-city
   python3 scripts/hike_state.py delete-hike 1
   ```

3. Enable web browsing so it can verify live trail info, timetables, and Deutschlandticket rules.
4. Chat with it exactly like you would with Codex — the workflow is identical.

---

## 🔒 Privacy first

Your city and hike history live **only on your device** in a local state file. You can view, change, or erase them at any time — just ask the agent to forget, or run `delete-city` / `delete-hike`.

---

## 🛡️ Safety notes

- Connections change — the planner will **always** tell you to recheck departures shortly before you travel.
- Seasonal and unverified features (like a summer-only water tap) are clearly labeled and never assumed safe.
- The offline PDF + GPX are designed so you can navigate **without any app or signal**. 📵

---

## 🧰 What's inside

| File                    | Purpose                                                     |
| ----------------------- | ----------------------------------------------------------- |
| `Skill.md`              | The full agent skill — workflow, rules, and quality checks. |
| `scripts/hike_state.py` | Local state helper for your city and hike log.              |
| `Readme.md`             | You're reading it. 👋                                       |

---

Happy trails! 🌄 Pack water, tell someone your route, and let your agent handle the planning.
