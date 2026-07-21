# 🥾 Deutschland Hiking Planner

> I like to touch grass and go hiking sometimes 🚆🌲

Built for **OpenAI Build Week** with **Codex + GPT-5.6**, this is an [Agent Skill](https://developers.openai.com/) that turns any Codex into a hiking guide. 

It remember your home city, and it finds real hikes, checks that the trains actually run, and even hands you a **printable offline map** so you never get lost when the signal drops.

---

## 🤖 How Codex & GPT-5.6 were used

- 🏗️ Codex built the skill — I designed Skill.md helper with Codex, using natural language to find hikes.
- 🧠 GPT-5.6 is the heart and brain — It helps to gather and structure the data
- 🔗 Agentic tool use — Codex + GPT-5.6 go to web, combine other tools and skills and give me some good results

---

## ✨ What it does

- 🏙️ **Remembers your city** — save your starting point once, locally on your machine.
- 🔎 **Finds real hikes** — discovers trails with verified distance and route data.
- ⏱️ **Plans the round trip** — confirms you can get home after a realistic day on the trail.
- 📓 **Keeps a hike log** — records the trails you've completed so it never sends you on the same one twice.

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

Happy trails! 🌄 Pack water, tell someone your route, and let your agent handle the planning.
