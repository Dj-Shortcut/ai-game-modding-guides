# 12. Worked example: an AI-assisted Rust rewrite

This is a case study from [mw2-rust-rust-rewrite](https://github.com/Dj-Shortcut/mw2-rust-rust-rewrite), an unfinished standalone Rust/Bevy prototype. It is included because the project shows both useful progress and the limits of AI-assisted rewrites. It is not a finished game and it does not contain the original game's files.

## TL;DR

- The project is a standalone Rust/Bevy game prototype, not a copy of a commercial game's assets or source.
- The useful first target was a small vertical slice: start a session, move, shoot, survive, save/load, and build a little world.
- An AI agent was most useful when the work was split into small issues with explicit acceptance checks.
- Headless checks, a compiler pass, and a real window prove different things. None of them alone proves that the game is finished.
- The biggest lesson: keep a written boundary between code present, headless-verified, graphically verified, and release-ready.

## The result

- **Project:** [mw2-rust-rust-rewrite](https://github.com/Dj-Shortcut/mw2-rust-rust-rewrite)
- **Stack:** Rust and Bevy
- **Content:** original authored models, materials, and audio; no original game files
- **Status:** unfinished development source, not a release build
- **Platform focus:** standalone PC prototype

The project has its own survival/building systems, FPS-style combat, and skating systems. It is inspired by several games, but it is not affiliated with their publishers.

## What works in the recorded development state

The repository records successful checks for parts of the following systems:

- a session that starts, moves, shoots, kills an NPC, dies, and respawns;
- inventory, crafting, consumables, ammunition, and save/load;
- a small authored world with building placement, collision, undo/redo, and scene save/load;
- skating, pushing, ollies, landings, and early rail/ledge grinding;
- native keyboard/mouse flows for several inventory, building, fishing, cooking, and lootbag routes;
- generated authored models and short CC0 audio cues.

These are feature-level results, not a claim that the whole product is finished. Hardware controllers, complete audio playback, all object variants, multiplayer, packaging, and the complete player flow remain separate work.

## What did not work as a completion test

Several checks were useful but easy to overread:

| Check | What it proves | What it does not prove |
|---|---|---|
| Rust compiler and headless probes | The tested logic and data paths run | That the window, input, camera, or sound feels right |
| Software-rendered window | Some authored content appears in a real window | That normal GPU hardware, audio, or a release package works |
| Synthetic controller input | Dispatch and bindings are wired | That a physical Xbox controller feels correct |
| Save/load fixtures | The named state survives a round trip | That every live user route is safe |
| A passing feature scenario | That scenario's contract | That unrelated systems or the whole game are complete |

Keeping these boundaries in the status file prevented a passing probe from being reported as a finished playable game.

## The plan that worked best

The project became manageable when each change had a narrow acceptance boundary:

1. Put the rule in a pure session/system API first.
2. Add a small deterministic probe for success, refusal, and state preservation.
3. Add save/load fields only when the feature actually needed persistence.
4. Connect native input and English feedback after the rule worked headlessly.
5. Run a focused graphical route in a real window.
6. Record what remains open instead of marking the whole feature complete.

For example, building repair was checked separately for range, ownership, health, proportional cost, full-health refusal, insufficient resources, save/load, and native input. That was slower than asking for "building repair", but it made failures local and reviewable.

## How the agent was kept on track

The repository used an `AGENTS.md`, a project status file, and issue-sized tasks. The rules included:

- all player-facing text stays in English;
- original game files, extracted data, and retail offsets stay out of the product repository;
- each task must state its verification boundary;
- a compiler check is not presented as a gameplay check;
- temporary probes and research stay outside the shipped product.

A useful task prompt was shaped like this:

```text
Implement [one small feature] in the existing Rust/Bevy project.

First inspect the current API and status notes. Keep the public save format
compatible unless this task requires a migration. Define the refusal cases
before editing. Add or update focused checks for success, refusal, and
save/load if relevant. Then connect the native input only after the rule is
verified. Report exactly what was checked and what remains unverified.
```

The important part was not the wording alone. The agent had to read the current state, edit a limited surface, and report evidence instead of guessing from a successful build.

## Dead ends and lessons

- **Starting with the full game idea:** too broad. Small vertical slices gave the agent a testable target.
- **Treating compile success as playability:** misleading. A real window and a human-controlled route were needed.
- **Adding input before the rule:** made failures hard to locate. The pure session rule came first.
- **Calling bindings "controller support":** too strong without physical hardware. Synthetic input only proves the code path.
- **Adding game files to make progress easier:** not appropriate for the repository. The product uses authored content and keeps research/development material separate.

## What I would do differently

Start with one tiny, complete user journey before adding a large catalogue of systems: launch, move, interact with one object, save, reload, and quit. Define the release gate before the first feature. Keep the status table from day one, and reserve the word "playable" for a route a person has actually completed in the intended build.

## Credits and legal boundary

This case study is based on the public project repository and its development notes. The project is an unofficial, standalone prototype. It includes no original commercial game assets or decompiled code. Players must supply any games they own themselves, and this write-up does not cover DRM, anti-cheat, or online play.

---

<sub>[Spot a mistake? [Edit this page on GitHub](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/12-worked-example-rust-rewrite.md).](https://github.com/trevaintdead/ai-game-modding-guides/issues/new) · Part of [AI Game Modding Guides](https://github.com/trevaintdead/ai-game-modding-guides)</sub>
