# 2. Passthrough Mods

A passthrough mod links two games that run at the same time. One game (the **host**) draws the world. The other game supplies the gameplay, like movement, blocks, or combat. They swap information constantly so each one sees what the other is doing.

## How it works

Say you want Minecraft inside Skyrim:

1. Skyrim runs normally and draws everything on screen.
2. Minecraft runs hidden in the background and simulates the player and blocks.
3. A plugin inside Skyrim and a mod inside Minecraft pass information back and forth (position, input, damage, blocks). SkyCraft does this through shared memory.
4. Skyrim draws what Minecraft says should be there.

Because both games run together, **every player needs a copy of both**.

One member described the idea well: two games exchanging state, where neither works without the other running. A normal mod would have one game containing the other's content.

## Examples to study

| Project | Games | Notes |
|---------|-------|-------|
| [SkyCraft](https://github.com/chasmlol/SkyCraft) | Skyrim + Minecraft | A script-extender plugin (C++) plus a Fabric mod (Java) |
| [FalloutCraft](https://github.com/zeyvu/FalloutCraft) | Fallout 4 + Minecraft | A fork of SkyCraft. Keeps its Minecraft mod, replaces the game-side plugin |
| [OWCraft](https://github.com/Yaekai/OWCraft) | Outer Wilds + Minecraft | Built on SkyCraft with a patch. Includes a design doc and development log |
| [GTA San AnSkateas](https://github.com/ryglizzy/GTA-San-AnSkateas) | GTA San Andreas + Skate 3 | A variation: a plugin loads a Rust rebuild of Skate 3's engine instead of running the whole second game |

Most of these are built on SkyCraft's design, so SkyCraft is the usual starting reference.

> A full step-by-step walkthrough of building one of these is in [guide 9](09-worked-example-passthrough-mod.md).

## The one thing that decides if it's possible

**Does the host game have a mod loader or script extender?**

With one, this is a realistic weekend project. Without one, the agent has to reverse engineer the game first, and that belongs in [guide 3](03-rust-rewrites-and-ports.md).

Check the table in [guide 8](08-mod-loaders-and-script-extenders.md) before you commit. Skyrim and Fallout 4 have SKSE and F4SE, Minecraft has Fabric, most Unity games have BepInEx or MelonLoader, and most Unreal games have UE4SS. That list is why SkyCraft-style projects are as common as they are.

## Do I need to decompile anything?

Usually not. For a SkyCraft-style mod, you point the agent at the SkyCraft project and say you want the same thing for your games. The agent works out the rest.

What you do need is a **way to run your own code inside the host game**: a script extender (SKSE for Skyrim, F4SE for Fallout 4), a mod loader (Outer Wilds Mod Loader), or a plugin SDK (plugin-sdk for GTA San Andreas). With one of those, the job is much easier.

Without one, the agent may have to reverse engineer the game. Members do use the agent to decompile with a tool like Ghidra when it's needed, and [guide 3](03-rust-rewrites-and-ports.md#do-i-need-to-decompile) covers how that works. Ask the agent to check what your game supports before it starts.

## Step by step

1. **Pick the host game and the gameplay game.** Check that both are single-player or offline, and that the host has a loader.
2. **Look for existing work.** Search for a mod loader, script extender, or existing mods for the host game. Two members lost hours by not doing this first.
3. **Install both games** and confirm they run normally. Install the host game's mod tools if it has them, and test the loader with an existing mod before you write anything.
4. **Open your agent in a new, empty project folder.**
5. **Send the starter prompt** (below).
6. **Let the agent make a plan and build.** Ask it to tell you what to run and what you should see.
7. **Playtest.** Describe exactly what happened. Paste logs from both games when something breaks.
8. **Repeat** until it works. Keep notes: see the handoff and log templates.
9. **Share it** as a GitHub repo with no game files. See [guide 6](06-rules-legal-and-publishing.md) and [guide 10](10-posting-your-project.md).

## Starter prompt

Experienced members say you don't need a perfect prompt. Keep it plain. Something like:

```
I want to make a passthrough mod like SkyCraft (https://github.com/chasmlol/SkyCraft), but for [Game A] and [Game B].

Clone SkyCraft locally and read its README and docs/DESIGN.md so you understand the architecture. I want the same approach as SkyCraft.

[Game A] is installed at [path]. [Game B] is installed at [path].

Before you build anything, tell me:
- does [Game A] have a mod loader or script extender we can use?
- does either game have online play or anti-cheat? (We don't touch those.)

Don't change any code yet. Just report what you found.
```

You can add more later, like what features you want first.

## If it gets stuck

Not a rule, but useful: when the agent goes in circles, aim for a smaller goal. A common order:

1. Get your code loading inside the host game and writing a line to a log.
2. Send one piece of data from one game to the other (like the player's position).
3. Send something back.
4. Make the player move in one game and show up in the other.
5. Add features one at a time (blocks, combat, vehicles, UI).

Steps 1 and 2 are the whole trick. Once one value crosses between the games, the architecture works and the rest is features.

## What to expect

- **It starts rough.** Several early projects describe themselves as experimental. Back up your saves.
- **Versions matter.** Mods are tied to game versions. Write down which versions you tested. GTA San AnSkateas needs GTA San Andreas at version 1.0, not the current Steam version.
- **Performance needs tuning.** You're running two games and a message channel at once. OWCraft's notes mention a large frame-rate gain from skipping a hidden window's presentation. Give the agent frame-time logs and it can find things like that.
- **Multiplayer isn't coming.** FalloutCraft lists multiplayer as not ported, and it won't be. These are single-player projects.

## Ideas that don't work, and why

These come up on the Discord constantly:

| Idea | Why not |
|------|---------|
| GTA x Rocket League | Rocket League is an online game with anti-cheat |
| Anything with a Rocket League car or asset in it | Same |
| Any online or multiplayer game | Out of scope entirely. See [guide 6](06-rules-legal-and-publishing.md) |
| A host game with no mod loader and no source | You'd be reverse engineering the whole engine first |

Being told no here saves you a weekend. [Guide 8](08-mod-loaders-and-script-extenders.md) lists which games have the loaders you need.

---

<sub>[Spot a mistake? [Edit this page on GitHub](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/02-passthrough-mods.md).](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/02-passthrough-mods.md) &middot; [Open an issue](https://github.com/trevaintdead/ai-game-modding-guides/issues/new) &middot; Part of [AI Game Modding Guides](https://github.com/trevaintdead/ai-game-modding-guides)</sub>
