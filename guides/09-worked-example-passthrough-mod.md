# 9. Worked Example: A Passthrough Mod, Start to Finish

This walks through building a passthrough mod from nothing, using the same eight steps every time. The names are written as **Game A** (the host, which draws the world) and **Game B** (the gameplay game, which supplies the mechanics). Swap in your own.

Traced against the real SkyCraft design so you can compare as you go.

## Step 0: Pick a pair that can work

Most projects fail here rather than in the code.

Game A (host) needs:
- a loader or script extender; see the reference in [guide 8](08-mod-loaders-and-script-extenders.md)
- to be single-player or offline
- a world the player moves through, first or third person

Game B (gameplay) needs:
- a mod API or SDK, or a headless server mode
- gameplay that runs unseen: physics, inventory, combat
- ideally a way to run without drawing anything

### The pairs that go well

| Game A (host) | Game B | Why it works |
|---------------|--------|--------------|
| Skyrim (SKSE) | Minecraft (Fabric) | This is SkyCraft. Both have excellent modding support, and Minecraft has a headless mode. |
| Fallout 4 (F4SE) | Minecraft (Fabric) | FalloutCraft did exactly this as a fork of SkyCraft. |
| Outer Wilds (mod loader) | Minecraft (Fabric) | OWCraft added a patch to make it work. |
| GTA San Andreas (plugin-sdk) | Skate 3 (custom engine layer) | GTA San AnSkateas loads a Rust rebuild of Skate 3 rather than running Skate 3 itself. |

### The pairs that will waste your time

| Idea | Why not |
|------|---------|
| Anything with a Rocket League car | Rocket League is an online game with anti-cheat. Out of scope. See [guide 6](06-rules-legal-and-publishing.md). |
| GTA x Rocket League | Same problem. |
| Any online or multiplayer game | Same problem, plus it's a rule and not just a difficulty issue. |
| A game with no loader and no source | You'd be reverse engineering the whole thing first. That's the "rewrite" path, not passthrough. |

Being told no here is a win. You saved a weekend.

## Step 1: Install and verify

Install both games. Start each one. Confirm they run, and confirm you know where they're installed:

```
Game A: C:\Games\GameA
Game B:  C:\Users\you\AppData\Roaming\.minecraft
```

Also install Game A's loader and test it with an existing mod. If the loader won't load someone else's known-good mod, stop and fix that first. You want a boring, working baseline before you add anything.

## Step 2: Create the project

```bash
mkdir my-passthrough && cd my-passthrough
git init
```

Create three empty files and ask the agent to keep them updated. These are your memory across sessions:

- `AGENTS.md`: rules the agent must always follow
- `MODLOG.md`: what changed and how it was tested
- `docs/DESIGN.md`: how it works, in plain language

Copy [`templates/AGENTS-starter.md`](../templates/AGENTS-starter.md) and
[`templates/MODLOG-template.md`](../templates/MODLOG-template.md) to get started.

## Step 3: The first prompt

Keep it plain. You're pointing at a working example and stating the substitution.

```
I want to make a passthrough mod like SkyCraft (https://github.com/chasmlol/SkyCraft),
but for Game A and Game B.

Clone SkyCraft locally and read its README and docs/DESIGN.md so you understand
the architecture. I want the same approach as SkyCraft.

Game A is installed at [path]. Game B is installed at [path].

Before you build anything, tell me:
- does Game A have a mod loader or script extender we can use?
- does either game have online play or anti-cheat? (We don't touch those.)

Don't change any code yet. Just report what you found.
```

The last line matters. A read-only recon answer first costs one turn and saves you from a confident plan built on a wrong assumption.

**What you should get back:** a list of what exists for each game, which loader you'd use, and any blockers. If it says "Game A has no modding support," you have your answer for free. Go pick a different host game.

## Step 4: Get a plan, then get one line into a log

Ask for a plan before code:

```
Write the plan as docs/DESIGN.md. Keep it to: the two halves of the mod,
what data crosses between them, and the order we build it in.
Then implement step 1 only.
```

Step 1 is always the same: **your code loads inside Game A and writes one line to a log file.**

```
Run it. I want to see one line in the log that says your plugin loaded.
Don't do anything else yet.
```

Two games, one log line. Get that, and the rest is iteration.

## Step 5: Send one value across

Build the chain one value at a time. Player position first, because it's easy to see and easy to verify:

```
Step 2: send the player's world position from Game A to Game B, once per frame.
Use the same transport SkyCraft uses if you can. Log both the value you send
and the value Game B receives, so I can compare them.
```

Then run both games and walk around. Check the log. Positions should match.

**Verification trick:** log the value on both sides with a timestamp or frame counter. You should never have to eyeball whether two numbers match.

This is where the architecture gets proven. Once one float crosses the boundary, the hard part is done.

## Step 6: Send something back

Now close the loop:

```
Step 3: read the player's input or state in Game B and send it back so Game A
can act on it. Log both directions.
```

Two games talking. Everything after this is features.

## Step 7: Add one feature at a time

A sensible order, roughly smallest to largest:

1. Position Game A → Game B
2. Input or state Game B → Game A
3. Game A's player moves and it shows up in Game B
4. Spawn Game B's objects (blocks, enemies) in Game A's world
5. Combat
6. Inventory
7. UI, if either game needs it

After each one: **playtest, then commit.** If a step breaks, `git revert` is instant.

## Step 8: Make it not stutter

Passthrough mods run two games and a message channel at once, so performance is the real enemy. One member's project got a large frame-rate gain from skipping the hidden window's presentation step. Things worth asking about:

- Run Game B headless or fully hidden. No window, no rendering.
- Send deltas rather than full state, if the values are large.
- Fix your update rate. 20 Hz is usually plenty for position.
- Log frame times from both processes and compare. Numbers beat guessing.

```
Game A is dropping to 40fps. Frame times are in [log path]. Find the bottleneck
before changing anything. Tell me what the profile says first.
```

## Step 9: Publish

See [guide 10](10-posting-your-project.md) for posting it, and [guide 6](06-rules-legal-and-publishing.md) for the rules you must not break. The short version: your repo is code only, no game files, ever.

## What actually happened, honestly

- Members report 2 to 4 hours of back-and-forth before something playable, on a good pair with existing loaders.
- You will hit a wall around step 5 or 6. Everyone does.
- When you do: stop repeating prompts. Write a `STATUS.md`, open a fresh chat, hand it over. See [guide 5](05-testing-and-troubleshooting.md).

## The checklist

- [ ] Game A has a loader and it loads someone else's known-good mod
- [ ] Both games are single-player or offline, and you own them
- [ ] Game B can run without drawing anything
- [ ] `git init` done, and you committed
- [ ] `AGENTS.md` and `MODLOG.md` exist
- [ ] Agent gave you a recon report before writing code
- [ ] Your code loads and logs one line
- [ ] One value crosses, logged on both sides
- [ ] Features added one at a time, each tested and committed
- [ ] README says what works and what doesn't
- [ ] No game files in the repo

---

<sub>[Spot a mistake? [Edit this page on GitHub](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/09-worked-example-passthrough-mod.md).](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/09-worked-example-passthrough-mod.md) &middot; [Open an issue](https://github.com/trevaintdead/ai-game-modding-guides/issues/new) &middot; Part of [AI Game Modding Guides](https://github.com/trevaintdead/ai-game-modding-guides)</sub>
