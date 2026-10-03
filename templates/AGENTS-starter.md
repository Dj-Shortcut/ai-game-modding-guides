# AGENTS.md — project rules for your AI agent

Copy this to the root of your project as `AGENTS.md` and fill in the bracketed parts. The agent reads this every session, so anything you put here is a rule it will follow without being reminded.

Every serious project in this space keeps one. It is the cheapest thing you can do to keep a long-running project on track.

---

## The filled-in template

```markdown
# AGENTS.md

## Project
[One sentence: what this project does.]

## Hard rules — never break these

1. **Never write game assets, decompiled code, or extracted game files into this
   repository.** They stay on this machine, untracked. If you need to read game
   data, read it from the install path at runtime or extract it to a gitignored
   folder.
2. **`.gitignore` is a whitelist.** It ignores everything and includes only
   source files. Do not switch it to a normal ignore list.
3. **Do not run `git commit` unless I asked.** Stage nothing beyond what the
   current task requires.
4. **Do not touch anything outside this project folder** unless I explicitly name
   the path. This includes my game installs — read them, never write to them.
5. **Single-player / offline games only.** If this touches anti-cheat, online play,
   or DRM, stop and tell me.
6. **Never put credentials in the repo or in any file you can read** — no API keys,
   no tokens, no passwords.

## How to work

- **Plan before code.** For anything more than a small fix, write the plan to
  `docs/DESIGN.md` first, then implement one step at a time.
- **One thing at a time.** Do not bundle unrelated changes. I want to be able to
  revert a single step.
- **Log, don't look.** You cannot see the game. Instrument instead: write
  positions, counts, timings, and state transitions to a log file so you can
  verify from the numbers. Do not try to visually inspect the game.
- **Tell me how to test it.** After every change, say the exact command to run and
  what I should see. "Done" without a test procedure is not done.
- **Ask before large refactors.** If you think the architecture is wrong, say so
  and explain, then wait for me.
- **Explain in plain language.** I am not the programmer here. If you use a term,
  explain it the first time.

## Honesty

- If something is not tested, write **"not tested"**. Never imply you verified
  something you didn't.
- If you are not sure, say you are not sure. A confident wrong answer costs me
  hours.
- If you hit something you cannot solve after two real attempts, stop and write
  up `STATUS.md` (see `templates/STATUS-handoff.md`) instead of trying variations
  at random.
- Record failures, not just successes. A dead end I can see is worth more than a
  dead end I have to watch you repeat.

## Keep these files updated

- `MODLOG.md` — add an entry after every change. Template in
  `templates/MODLOG-template.md`.
- `docs/DESIGN.md` — how the project works, in plain language. Update when the
  architecture changes, not on every commit.
- `README.md` — the "what works / what doesn't work" list. Test before you claim
  something works.

## Environment

- OS: [Windows 11 / Linux / macOS]
- Game A: [name] [exact version], installed at [path]
- Game B: [name] [exact version], installed at [path]
- Loader: [SKSE / F4SE / Fabric / ...] [version]
- Language and version: [e.g. Rust 1.8x, C++ with MSVC]
- Agent: [Claude Code / Codex / OpenCode]
```

---

## Why each rule is there

| Rule | Reason |
|------|--------|
| No game files in the repo | It's the one rule you can get a takedown notice for. FalloutCraft and OWCraft both ship extractors instead of data for this reason. |
| Whitelist `.gitignore` | A normal ignore list has to be updated every time you discover a new file type. A whitelist can't accidentally commit extracted data. gang-beasts-rust does this. |
| Don't commit unless asked | You want to review the diff before it becomes history. |
| Stay in the project folder | Agents with broad access will happily rewrite a config file you care about. |
| Single-player only | Banned accounts. Not negotiable. |
| No credentials | Agents read everything in the working directory. |
| Plan first | Long sessions go wrong when the model changes its mind halfway. |
| Log, don't look | The agent genuinely cannot see the game. Numbers are the only feedback channel it has. |
| Write "not tested" | An unverified claim in a README wastes someone else's afternoon. |
| Stop after two attempts | Looping burns your usage cap and produces random variations instead of a different approach. |

## Customising it

Add your own rules. Useful ones:

- "Never refactor files I have edited this session."
- "Always keep the public function signatures stable."
- "Every new file needs a comment at the top saying what it's for."
- "Do not add dependencies without telling me why."
- "Run the build before you tell me it's done."
- "Prefer the simplest thing that works. Don't build abstractions yet."

Then run `git commit -am "Add project rules"` so it's part of the project from day one.