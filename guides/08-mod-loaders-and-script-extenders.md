# 8. Mod Loaders and Script Extenders (Reference)

This is the single most repeated question in the server: *"what do I even install to put my code inside this game?"*

Read this page once and you can skip the hunting. It lists what each engine family gives you and how hard the job looks.

For a passthrough mod you need exactly one thing: **a way to run your own code inside the host game.** Everything else on this page is a nice-to-have.

## The one table that matters

| Host game | Engine | Loader / extender | Language | Difficulty |
|-----------|--------|-------------------|----------|-----------|
| Skyrim / Skyrim SE / AE | Creation Engine | SKSE ([afkmods.com](https://afkmods.com/), [`ianpatt/skse64`](https://github.com/ianpatt/skse64)) | C++ + Papyrus | Easy |
| Fallout 4 | Creation Engine | F4SE ([afkmods.com](https://afkmods.com/), [`ianpatt/f4se`](https://github.com/ianpatt/f4se)) | C++ + Papyrus | Easy |
| Starfield | Creation Engine 2 | Same approach as F4SE | C++ | Medium |
| Minecraft: Java | — | Fabric, Forge, or [NeoForge](https://neoforged.net) | Java / Kotlin | Easy |
| Outer Wilds | Unity | [Outer Wilds Mod Loader](https://outerwildsmods.com/) + Unity mods | C# | Medium |
| GTA San Andreas / Vice City / GTA III | RenderWare | [plugin-sdk](https://github.com/DK22Pac/plugin-sdk) (ASI / CLEO plugins) | C++ / C | Medium |
| Most Unity games | Unity | [BepInEx](https://github.com/BepInEx/BepInEx) or [MelonLoader](https://github.com/LavaGang/MelonLoader) | C# | Easy–Medium |
| Most Unreal games | Unreal | [UE4SS](https://github.com/UE4SS-RE/RE-UE4SS) | Lua | Medium |
| GameMaker 2 / 3 games | GameMaker | [UndertaleModTool](https://github.com/UnderminersTeam/UndertaleModTool) | GML + tool | Easy |
| Ren'Py visual novels | Ren'Py | [Ren'Py SDK](https://www.renpy.org/doc/html/developer_tools.html) | Python | Easy |

## What the words mean

- **Script extender** — a third-party DLL that loads alongside the game and gives mods a scripting language and an API. SKSE and F4SE are the classic examples. You write a plugin, it runs in-process.
- **Mod loader / mod API** — a supported framework the modding community built for a game. Fabric and NeoForge for Minecraft. Same idea, more formal.
- **ASI loader** — the smallest possible thing: it loads `.asi` DLLs from a folder and does nothing else. plugin-sdk gives you a real SDK on top of it.
- **Mod manager** — a tool for installing and versioning other people's mods (MO2, Vortex, r2modman). Useful, not required.

**Key idea:** if a loader exists, the agent writes your mod against its API and you never touch the original binaries. That is the good path. If none exists, the agent has to reverse engineer the game, which is a different and much harder project.

## Why this decides whether your idea is realistic

Before you commit to a game pair, check this table:

1. **Does the host game have a loader?** If yes, a passthrough mod is realistic this weekend. If no, expect a research project.
2. **Does the gameplay game have a mod API or an SDK?** Same question. Minecraft (Fabric) is easy. A closed-source game with nothing is hard.
3. **Is there an existing mod that already does something similar?** If yes, read its source. You are not starting from zero.
4. **Is it single-player and offline?** If not, stop. See [guide 6](06-rules-legal-and-publishing.md).

The combination "host has a loader + gameplay game has an API" is exactly what makes SkyCraft-style projects work in hours rather than months. SkyCraft is Skyrim (SKSE) + Minecraft (Fabric): two of the best-documented modding targets in existence.

## Engine families, in more detail

### Creation Engine (Skyrim, Fallout 4)

The best-documented modding family for native-code work, and what SkyCraft, FalloutCraft and OWCraft are built on.

- **SKSE / F4SE** load a plugin DLL and expose a scripting layer (Papyrus) alongside it. Address Library gives plugins access to game functions.
- Mods usually split into two halves: a native plugin (C++) and a Papyrus script. A passthrough mod needs the native side, because it has to run every frame.
- Fallout 4 and Skyrim share enough architecture that SkyCraft ports between them with modest changes. That is why FalloutCraft exists as a small fork rather than a from-scratch project.

### Unity

The most common engine in modern indie games, and the easiest to get into.

- **BepInEx** patches the game at load and loads your C# assemblies. Works with both Mono and IL2CPP builds.
- **MelonLoader** does the same job with a different API and better support for more title variants. Pick one; don't install both.
- If the game is IL2CPP, expect an extra step where the original method bodies are stubs. Tools like Il2CppDumper recover the metadata so the loader can build real hooks. Your agent can handle this if you point it at the game directory.

### Unreal Engine

- **UE4SS** injects a Lua scripting layer, generates a live SDK dump, and gives you a property editor you can use to poke at a running game. The property editor alone makes it great for exploration: you can read the value of anything and find out what a variable does.
- Games ship in Unreal 4 and Unreal 5 with very different internals. UE4SS support varies by title, so check before you commit.

### GameMaker

- **UndertaleModTool** reads the game's data files and code as text, edits them, and writes them back. This is the easiest modding target on the list and the best one to learn on if you want to see how a game works internally.

### Open-source engine reimplementations

Not loaders, but the same family of work as a rewrite. These are worth reading because they are finished, licensed, and documented:

| Project | Original | What it shows |
|---------|----------|----------------|
| [OpenMW](https://github.com/OpenMW/openmw) | Morrowind | A full engine reimplementation with a long public history |
| [OpenRCT2](https://github.com/OpenRCT2/OpenRCT2) | RollerCoaster Tycoon 2 | A polished reimplementation that improved on the original |
| [OpenTTD](https://github.com/OpenTTD/OpenTTD) | Transport Tycoon Deluxe | Long-running, mature, well-documented |

These took years and many contributors. Read them for structure, not as a template for a weekend project.

## How to install a loader

The process is almost always the same four steps. Ask your agent to walk you through it, but it looks like:

1. Find the loader's official site and download the version matching **your game version exactly**.
2. Extract it into the game's install folder. There is usually no installer.
3. Start the game once. The loader writes a log and creates a plugins or mods folder.
4. Put your file in that folder and start the game again.

The log file is your friend. When something does not load, the loader almost always says why.

### Version mismatch is the number one problem

Loaders and mod APIs are pinned to specific game versions. A loader built for Skyrim 1.5.97 will not load correctly in 1.6.1170. Members have lost whole evenings to this.

- Write your exact game version in your README and in every issue you file.
- Keep a downgrade tool around if the game is old. GTA San AnSkateas needs GTA San Andreas at **version 1.0**, which is not the current Steam version, so it uses an open-source downgrader ([gtasa-open-downgrader](https://github.com/xxanqw/gtasa-open-downgrader)).

## Checklist before you pick a host game

- [ ] I know the engine (Creation Engine, Unity, Unreal, GameMaker, custom)
- [ ] There is a loader or mod API for it, and it's current
- [ ] It's single-player or offline
- [ ] I own it and the loader is a legitimate public tool
- [ ] I know the exact game version and whether I need to downgrade
- [ ] I've found at least one existing mod for this game, so I can read real code

If you can't tick all six, go and look at a different game. That is not giving up, it is the fastest way to get something on screen.

---

<sub>[Spot a mistake? [Edit this page on GitHub](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/08-mod-loaders-and-script-extenders.md).](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/08-mod-loaders-and-script-extenders.md) &middot; [Open an issue](https://github.com/trevaintdead/ai-game-modding-guides/issues/new) &middot; Part of [AI Game Modding Guides](https://github.com/trevaintdead/ai-game-modding-guides)</sub>
