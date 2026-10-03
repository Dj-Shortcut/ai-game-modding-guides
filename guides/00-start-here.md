# 0. Start Here

You have never done this before and want to know what's involved. This page is the map. The other guides fill in the details.

## What you're actually doing

You are not writing the code. You tell an AI agent what you want, it writes and builds the code, and you test the result by playing. Your job is to:

- decide what you want
- describe it and describe problems clearly
- playtest, because the AI can't really see or feel a game
- keep the project organized so you can recover when something goes wrong

Experienced members say the core of it is simple: install the games, open an agent, give it an example project, and say what you want. It is not a two-step process in practice, though. Expect to hit problems and spend most of your time fixing them with the agent.

## Pick your path

| I want to... | Read | Rough idea |
|--------------|------|------------|
| Put one game's gameplay inside another (Minecraft in Skyrim, Skate 3 in GTA) | [Passthrough mods](02-passthrough-mods.md) | Both games run at once and talk to each other |
| Rebuild a game's engine so it runs on its own | [Rust rewrites](03-rust-rewrites-and-ports.md) | Bigger job. Reads your game files at runtime |
| Just play what others made | See the share forum | Use the project's own install instructions |

If you're not sure, start with a passthrough mod. It's usually the faster way to see something working.

### Three shortcuts, in order

These three pages answer most of the questions people arrive with, so read them before anything else:

1. **[Which loaders and script extenders exist](08-mod-loaders-and-script-extenders.md)** — decides whether your game idea is even realistic. This is the single most repeated question in the server.
2. **[A full passthrough walkthrough](09-worked-example-passthrough-mod.md)** — the whole process end to end, with the prompts.
3. **[Posting your project](10-posting-your-project.md)** — what a finished project needs before others can use it.

## The steps, in order

1. **Pick your games.** Check that they're single-player or offline, and that you own them.
2. **Search for existing work first.** Look for mod loaders, existing mods, or decomp projects for your games. Two members said they wasted hours by skipping this.
3. **Set up an AI agent** on your PC. See [guide 1](01-choose-and-set-up-an-ai-agent.md).
4. **Install the games** and make sure they run normally.
5. **Open the agent in a new, empty project folder** and use a starter prompt from guide 2 or 3.
6. **Playtest and report back.** Describe what happened, paste logs.
7. **Keep notes** so a fresh chat can pick up where the old one left off. See [guide 4](04-prompting-and-workflow.md).
8. **Share it** as a GitHub repo, with no game files in it. See [guide 6](06-rules-legal-and-publishing.md) and [guide 10](10-posting-your-project.md).

## Honest expectations

- **Time:** a task can take anywhere from a few minutes to many hours, depending on the model and how hard you make it think. One member reported about 3-4 hours of back-and-forth before an Elden Ring + Spider-Man mashup worked, and described it as jank but working.
- **Cost:** agents use paid plans or API credits, and plans have usage limits. See [guide 1](01-choose-and-set-up-an-ai-agent.md).
- **Coding knowledge:** you don't need it to start, but a little helps. You can always ask the agent to explain what it did.
- **Rough edges:** early projects are experimental. Back up your saves.
- **Some things won't work.** Online games with anti-cheat are off the table. See [guide 6](06-rules-legal-and-publishing.md).

## Before you start: checklist

- [ ] I own the games and they're installed
- [ ] They are single-player or offline
- [ ] The host game has a [mod loader or script extender](08-mod-loaders-and-script-extenders.md)
- [ ] I've picked an AI agent (not just a chat website)
- [ ] I created a folder just for this project
- [ ] I know I'll playtest myself
- [ ] I've read the rules in [guide 6](06-rules-legal-and-publishing.md)

---

<sub>[Spot a mistake? [Edit this page on GitHub](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/00-start-here.md).](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/00-start-here.md) &middot; [Open an issue](https://github.com/trevaintdead/ai-game-modding-guides/issues/new) &middot; Part of [AI Game Modding Guides](https://github.com/trevaintdead/ai-game-modding-guides)</sub>
