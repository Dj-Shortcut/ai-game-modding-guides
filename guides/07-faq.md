# 7. FAQ

Quick answers to the questions people ask most. Answers come from experienced members and from the example projects. Where nobody has a confirmed answer yet, it says so.

## Getting started

**How do I get started?**
Read [Start here](00-start-here.md). The short version: set up an AI agent, install your games, open the agent in an empty folder, give it an example project, and say what you want.

**Is it really just "tell the AI to do it"?**
For the core idea, yes. Members who've made these projects say they link an example repo and say they want the same for their games. But expect to hit problems, and expect to spend most of your time fixing them with the agent. See [guide 4](04-prompting-and-workflow.md).

**Do I need to know how to code?**
No, but it helps. You need to be clear about what you want and what's wrong, and you can ask the agent to explain anything. Members with no coding background have gotten projects working. A little knowledge helps you check what the AI is doing.

**I have never written code. Am I going to be stuck?**
Less than you think, if you accept the split: the agent writes it, you playtest it and describe what happened. That's it. It's also why [guide 4](04-prompting-and-workflow.md) and [guide 5](05-testing-and-troubleshooting.md) are mostly about *communicating problems*, not about programming.

**Can I make a completely new game instead?**
Yes. One member is building a basketball game from scratch with open-source assets and animations. The same agent workflow applies.

## Picking your games

**What do I put in the agent? Do I give it my whole game folder?**
You tell the agent where the games are installed and it finds what it needs. Members pointed their agents at their game installs, including a Minecraft install with Fabric. You don't upload anything.

**How do I know if my idea is possible?**
Check whether the host game has a mod loader or script extender. That's the main thing. See [guide 8](08-mod-loaders-and-script-extenders.md).

**I want to put Rocket League into Minecraft / GTA. Can I?**
No. Rocket League is an online game with anti-cheat, which puts it out of scope. This comes up constantly. Pick a single-player game for the gameplay side. See [guide 2](02-passthrough-mods.md).

**Can I merge game X with game Y?**
Maybe. It depends mostly on whether the host game can run your code (script extender, mod loader, plugin system) and whether it's single-player. Members have reported projects like Elden Ring and Spider-Man mechanics and an Octane-style car in Minecraft, but nothing is guaranteed. Search for existing projects and tools for your games first.

**What games are easiest to start with?**
Minecraft, Skyrim, and Fallout 4, by a wide margin. They have the best-documented loaders in gaming: Fabric for Minecraft, SKSE for Skyrim, F4SE for Fallout 4. Every beginner passthrough project in the server is built on them.

**What are the hardest?**
Games with no mod loader and no source. If the host game has nothing, you're reverse engineering an engine before you can start. See [guide 8](08-mod-loaders-and-script-extenders.md).

## Tools and cost

**What AI should I use?**
Most members use Claude Code with a top Claude model. Codex is also common. See [guide 1](01-choose-and-set-up-an-ai-agent.md).

**Why does the AI say my game files are too large?**
You're probably using a chat website. You need an agent that runs on your PC and reads your files directly. You don't upload anything.

**Does the game have to be running while the AI works?**
The agent doesn't need the game running to read your files or write code. You do need the games running to playtest. For passthrough mods, both games run together when you test.

**What MCP servers do I need?**
None to start. Claude Code and Codex are agents. MCP (Model Context Protocol) is a standard for plugging extra tools into an agent, and it's optional. None of the example projects list one as a requirement.

**Can I use a free plan?**
Not confirmed. Members expect to hit limits quickly. Pay-per-use API keys are another route. Check current plans.

**Will the $20 plan be enough?**
Reports vary. One member says it's more than enough for a small project. Another gets about 3-4 hours of heavy use in each 5-hour window on the top model. It depends on how much you do.

**Do long chats burn my usage faster?**
Yes. The whole conversation is carried along on every turn, so a 300-turn chat costs more per turn than a fresh one. Start a fresh chat with a [`STATUS-handoff.md`](../templates/STATUS-handoff.md) file when things get long. See [guide 4](04-prompting-and-workflow.md).

**Can I run a local model on my own GPU?**
One experienced member says local models don't work well for this. If you've tried it, please write it up.

**My GPU doesn't matter then, right?**
Correct, for cloud models. A 5090 doesn't change anything if you're using Claude or GPT — the work happens on the provider's servers. It only matters if you're running a local model.

**How do I give Codex full access? It keeps failing.**
Check Codex's documentation for its permission and sandbox settings. Give it access to your project and game folders only. Full access to your whole PC is risky.

**Is giving an agent full PC access safe?**
Many members do it, but it's a real risk. Use a separate folder, Git, backups, and consider a separate user account, a VM, or a container. See [guide 1](01-choose-and-set-up-an-ai-agent.md).

**Do I even need an IDE?**
No, but one experienced member recommends VS Code so you get proper versioning and file views. The agent will create your files and run your builds either way — you don't copy code into folders by hand.

## Technical

**Do I need to decompile the games?**
Usually not. For a SkyCraft-style passthrough mod you need a way to run code inside the host game, not decompilation. For rewrites, check for existing format documentation and open-source readers first; decompiling is a last resort. [Guide 3](03-rust-rewrites-and-ports.md) covers when and how, including which tool to use.

**Which tool is best for decompiling: IDA Pro, Ghidra, or Binary Ninja?**
Ghidra is free, open source, and the one members actually use. IDA Pro is the commercial standard. Binary Ninja sits in between. You can also just ask your agent which tool fits your game's format and let it set it up.

**Can Unreal Engine games be decompiled?**
Nobody has a confirmed answer in the server. UE4SS exists as a modding and introspection tool, which gets you a long way without decompiling. Ask your agent to check for existing community tooling for your specific title.

**Why Rust and Bevy? Why not C or C++?**
You don't have to use them. People use them because Rust catches memory mistakes before the game runs, it's easy to set up, Bevy is a free all-code engine, and AI is good at fixing Rust. C and C++ work fine too. The plugins that go inside Skyrim, Fallout 4, and GTA are still C++.

**Do I just download Rust and write my own stuff?**
For a rewrite you'd install Rust, but the agent writes the code. You also need the game installed and an agent set up.

**How do I stop the AI from testing visually?**
Tell it you'll playtest, and have it log numbers and events instead. See [guide 5](05-testing-and-troubleshooting.md).

**How do I make it run smoother?**
Give the agent frame-time logs from both processes and ask it to profile before changing anything. One member's project got a large frame-rate gain purely from skipping the hidden window's presentation step. Run the gameplay game headless if you can.

**The mod works but it's janky. Is that normal?**
Yes, at first. Members describe their projects as "jank as hell but working." Performance and polish come after it functions.

## Rules and sharing

**Can I mod games with anti-cheat?**
No. You can get banned, and agents won't help circumvent it. Use single-player or offline modes.

**Can I put game files in my repo?**
No. See [guide 6](06-rules-legal-and-publishing.md).

**How do I publish on Steam Workshop?**
Make sure your upload has no copyrighted game content. One member suggests having the agent write an extractor that players run themselves. Check the platform's rules.

**Where can I see finished projects?**
In the share forum on the [Discord](https://discord.gg/ccFpNC26Ts). A member is also working on a website to collect them.

**How do I post my project?**
See [guide 10](10-posting-your-project.md), which has the pre-flight checklist and a post template.

**What do I write so people bother looking at it?**
A screenshot or GIF, a real commit history, an honest "what doesn't work" section, and a MODLOG. See [guide 10](10-posting-your-project.md#the-readme-is-the-post).

## Still unanswered

These came up and nobody has given a confirmed answer. If you know, post it in [#support-help](https://discord.gg/ccFpNC26Ts), or open a pull request and add it here.

- Which free model works best with OpenCode?
- Whether free plans can complete a real project
- How to decompile Unreal Engine games
- Whether detailed prompts or short loose prompts are more efficient (people disagree — see the debate section in [guide 4](04-prompting-and-workflow.md))
- Making games run better on original hardware (such as PS3), and whether emulator research applies
- Whether local models can handle a real project on a 12 GB GPU

---

<sub>[Spot a mistake? [Edit this page on GitHub](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/07-faq.md).](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/07-faq.md) &middot; [Open an issue](https://github.com/trevaintdead/ai-game-modding-guides/issues/new) &middot; Part of [AI Game Modding Guides](https://github.com/trevaintdead/ai-game-modding-guides)</sub>
