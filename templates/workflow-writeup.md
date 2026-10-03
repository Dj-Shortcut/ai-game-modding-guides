# Workflow Write-Up Template

Use this when you're sharing a finished project. It is the thing the server is short of — almost nobody documents *how* they actually got there.

A feature list proves the thing works. A workflow write-up is what lets someone else do it too. Write one even if your project is small and imperfect; a rough honest one is worth more than a polished marketing page.

Copy to your repo as `WORKFLOW.md`, or paste it in a discussion thread.

---

## Why this exists

The most common complaint about AI-assisted projects is that people say "just tell the AI to do it" and then stop. That is genuinely most of the method, but the useful part is everything around it:

- which games you picked and why
- what you checked *before* starting
- which dead ends cost you the most time
- what actually worked

None of that fits in a README.

## Template

```markdown
# How I made [Project Name]

## TL;DR

[3-6 bullets. What it is, what stack, roughly how long it took, and the one
thing you'd tell someone starting this.]

## The result

- **Games:** [Game A] v[version] + [Game B] v[version]
- **Stack:** [SKSE C++ plugin / Fabric Java mod / Rust + Bevy]
- **Built with:** [agent and model]
- **Time:** [hours, roughly]
- **Cost:** [plan tier, roughly]

[Screenshot or GIF]

## What works

- [Feature]
- [Feature]

## What doesn't work

- [Honest list. This section is the most valuable part of the document.]

## Before you start: what to check

The research I did that saved me time. Be specific enough to act on.

1. **Does Game A have a mod loader?** [Which one, which version, where from.]
   Without this there is no easy path — you'd be reverse engineering.
2. **Existing projects I read first:** [links + one line on what each taught you]
3. **Existing mods for Game A:** [links]
4. **File formats / docs I found:** [links]
5. **Version traps:** [what I had to downgrade or match exactly]

## The plan

[What you built first, second, third — and why that order. Show the milestones
you aimed at. This is the part people can copy.]

1. [Milestone 1: e.g. "plugin loads and writes a log line"]
2. [Milestone 2: e.g. "position crosses from A to B"]
3. [Milestone 3: e.g. "B's objects spawn in A's world"]
4. ...

## Transport and architecture

[How the two halves talk. Shared memory, socket, file, IPC? What messages go
across and at what rate? Keep this concrete — it's the part people copy.]

## What I prompted, roughly

[Actual prompts you used, with the successful ones intact and the bad ones too.
Do not clean these up. Show that the first prompt didn't work.]

**This one worked:**
```
[prompt]
```

**This one was a waste:**
```
[prompt]
```
Why it was a waste: [explanation]

## Dead ends

The most valuable section. What didn't work, and what it cost.

- **[Approach]** — [why it failed]. Cost: [time / tokens / a broken build]
- **[Approach]** — [why it failed]

## Problems I hit, and what fixed them

| Symptom | Cause | Fix |
|---------|-------|-----|
| [error or behaviour] | [root cause] | [what you changed] |

## What I'd do differently

[Honest retrospective. This is what makes the document trustworthy and what
someone else will thank you for.]

## Credits

- [Project] — [what you reused, license]
- [Person] — [help you got]
- [Modding community for Game A] — [loader / docs]

## Legal

Unofficial fan project, not affiliated with or endorsed by the publisher.
No game assets included; players supply their own copies. Built with AI
coding agents.
```

## Good examples of sections people actually find useful

**The dead-ends section.** Specific, costly, and impossible to get anywhere else. "Tried a file-based transport first, spent two hours on Windows file locking, switched to shared memory" saves the next person two hours.

**Real prompts, unedited.** Especially the failures. It shows the method is iterative, which is the honest truth and also the reassurance a beginner needs.

**The "what I'd do differently".** Signals experience, and turns a flex into a lesson.

**Version traps.** Ultra-specific and universally useful. Which version you needed, and which downloader got you there.

## What not to include

- Game assets, screenshots of copyrighted content beyond fair use, or decompiled code
- Long transcripts of the whole session. Excerpts.
- Advertising. Post it here and it gets removed.
- Anything from a game you're not allowed to mod. See [guide 7](../guides/06-rules-legal-and-publishing.md).
