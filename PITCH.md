# Deutschland Hiking Planner — About the project

## 💡 Inspiration
I love exploring Germany by train. With the **Deutschlandticket**, the whole country opens up for a flat monthly fare — but there's a catch: finding a great day hike that's *actually* reachable on that ticket is surprisingly painful. Trail sites tell you the route, transit apps tell you the trains, and the Deutschlandticket rules tell you what's covered — but nothing connects the three. I'd spend an evening juggling five browser tabs just to answer one question: *"Can I hike this on Saturday and get home?"* I wanted an assistant that did all of that for me.

## 🛠️ What it does
The **Deutschland Hiking Planner** is an **Agent Skill** for Codex (and any capable agent). You tell it your home city once, and it:

- finds real trails with verified distance and elevation,
- checks that the regional trains and buses to *both* trail ends are included in the Deutschlandticket (no sneaky ICE/IC),
- confirms the **return** trip works after a realistic day of walking,
- builds a **print-friendly offline PDF map + GPX** with landmarks, decision points, coordinates, and safety notes,
- and keeps a local log so it never sends you on the same hike twice.

Crucially, it always separates **verified facts** from **assumptions** and reminds you to recheck connections before you leave.

## 🤖 How I used Codex & GPT-5.6
- **Codex built the skill itself.** I used Codex to design and iterate on `Skill.md` and the `hike_state.py` helper — describing the hiking workflow in plain language and letting Codex turn it into structured instructions, argument-parsing CLI code, and safety guardrails.
- **GPT-5.6 is the reasoning engine at runtime.** When a user asks for a hike, GPT-5.6 does the hard thinking: cross-referencing trail data, transit timetables, and Deutschlandticket rules, then reasoning about eligibility, return margins, and daylight — while carefully separating verified facts from assumptions.
- **Agentic research and tool use.** Codex + GPT-5.6 drive the live web research, run the local state script, and generate the offline PDF + GPX — chaining multiple steps autonomously from a single natural-language request.
- **Fast iteration.** GPT-5.6's stronger instruction-following let me encode nuanced rules ("never invent a water source," "flag seasonal service," "always tell the user to recheck departures") and trust they'd be respected across varied user prompts.

## 🧠 What I learned
The biggest lesson: **Agent Skills are far more flexible than a traditional app.** Instead of hard-coding a hiking database, a transit API, and a PDF generator into a rigid application, I wrote the *reasoning and the guardrails* in Markdown and let the agent do live research, cross-check sources, and generate outputs on demand. Adding a new capability — a completed-hike log, an offline map, a privacy "forget me" command — was just a few lines of instructions, not a new backend. The skill grows by *describing* behavior, not by shipping more code.

## 🏗️ How I built it
- **`Skill.md`** — the heart of the project: the workflow, safety rules, and quality checks that turn a general agent into a careful hiking guide.
- **`scripts/hike_state.py`** — a tiny local state helper that stores the user's city and hike history on their own machine (`$CODEX_HOME/hiking-planner/state.json`), keeping everything private.
- **Codex + GPT-5.6** for live trail and timetable research, eligibility reasoning, and generating the offline navigation kit.
- **MIT-licensed and open** so anyone can use, fork, and extend it.

## 🧗 Challenges I faced
- **Trust and safety.** Hiking plans have real consequences, so I had to design the skill to never invent a landmark, water source, or "it'll be fine" connection — and to clearly flag seasonal or unverified features.
- **Deutschlandticket edge cases.** Coverage isn't obvious: some regional operators, replacement buses, and fare boundaries are tricky. Encoding "verify, don't assume" into the workflow was key.
- **Truly offline output.** A map is useless if you need signal to read it. Getting a paper PDF *and* GPX that stand alone — no app, no URL required — took careful design of the legend, coordinate grid, and turn-by-turn table.

## 🚀 Why it matters
This started as my personal itch — better weekend hikes in Germany — but it became a demonstration of how **Agent Skills unlock functionality that simple apps can't match**: live research, sound reasoning, safety guardrails, and easy extensibility, all from a lightweight, open, human-readable skill. 🌄🚆
