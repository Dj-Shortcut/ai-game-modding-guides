# 1. Choosing and Setting Up an AI Agent

## Agent vs. chat website

This is the most common stumbling block.

- **A chat website** (claude.ai in the browser, ChatGPT in the browser) only sees what you paste or upload. It can't open your game folders, so you hit "file too large" errors and end up copy-pasting code by hand.
- **An agent** runs on your PC. It can read your game folders, create and edit files, run builds, and read logs. Nothing to upload.

You want an agent. If you're using the Claude desktop app, look for the Code mode (members say it's in the top left, and it's easy to miss). Check the provider's current docs for exact steps.

## Which agent

Members mostly use these. Pick one and stick with it while you learn.

| Tool | Notes from the community |
|------|--------------------------|
| **Claude Code** | The most mentioned. Works from the desktop app, a terminal, or a VS Code extension |
| **Codex** | Also widely used, including to run Ghidra-based decompiling |
| **OpenCode** | Works with many models, including free ones. Members asked which free model is best and nobody answered yet |
| **VS Code + Roo Code + OpenRouter** | A pay-per-use route. One member uses it with a DeepSeek model because a Claude plan is too expensive for them |

### A note on "MCP"

Several people confused these. **Claude Code and Codex are agents.** MCP (Model Context Protocol) is a standard for plugging extra tools into an agent. You do **not** need any MCP server to start. None of the example projects list one as a requirement.

### What the agent actually is

Almost all of these are a terminal program or a VS Code extension. The agent is just software running on your machine with the same file access your user account has — no special permissions, no sandbox, nothing protecting you. That is exactly why the safety section at the bottom of this guide matters.

You do not need an IDE, but one experienced member recommends VS Code so you get proper file views and diffs. The agent creates your files and runs your builds either way, so you are never copy-pasting code into folders by hand.

## Which model

Models change quickly, so check what's current. What members report:

- Most people doing this use a top-tier Claude model with Claude Code. Several mention Claude Opus 5.5.
- Others use OpenAI's models through Codex.
- Weaker or cheaper models work for smaller tasks, but members say they need more babysitting (see the handoff trick in [guide 4](04-prompting-and-workflow.md)).
- **Local models** (running on your own GPU): one experienced member said they don't work well for this. If you have a 12 GB GPU and are wondering, this is the answer so far. If you've made it work, please add a write-up.
- Your hardware (a 5090, for example) doesn't matter for cloud models. The AI runs on the provider's servers.

## Cost and usage limits

Plans change often, so check the provider's current page. What members report:

- Paid plans have a short reset window (about 5 hours) and a weekly limit.
- One member gets about 3-4 hours of constant use on the top Claude model out of each 5-hour window.
- Another said the $20 tier was "more than enough" for a from-scratch basketball game.
- Another suggested a $100 tier if you want to work for hours every day.
- **Free tiers:** nobody has confirmed whether you can do a real project on one. You can try, but expect to hit limits fast. A pay-per-use API key (OpenRouter and similar) is the other option.
- Long sessions use more of your limit, because the whole conversation is carried along. Start a fresh chat now and then with a short handoff note. See [guide 4](04-prompting-and-workflow.md).

## Setting up safely

Agents can read and delete files, and many members run them with broad access. That's convenient and also risky. A few habits make it much safer:

1. **Make one folder for the project** and run the agent inside it.
2. **Use Git from day one.** Commit after each working step so you can undo mistakes.
3. **Back up your game saves** before testing.
4. **Be careful with "full access" modes.** Some members run agents with full PC access. It's more convenient, but a mistake can hit files you care about. A separate Windows user account, a virtual machine, or a container (one member mentioned Podman) limits the damage.
5. **Don't put passwords or API keys in files the agent can read** or in your repo.
6. **If the agent keeps failing to get access** (a common Codex complaint), read that tool's docs on permission and sandbox settings and grant it access to your project folder and game folders only.
7. **Write down the rules instead of trusting your memory.** A rules file in your project means the agent follows them in every session, including the ones you forget. See [`templates/AGENTS-starter.md`](../templates/AGENTS-starter.md).

## Helping extras (optional)

- **VS Code** (or another editor) with your agent's extension. One experienced member recommends this so you get proper versioning and file views. It's not required.
- **Git and a GitHub account.** You'll need these to share your project.
- **[universal-modder](https://github.com/rehan-remade/universal-modder):** a toolkit of skills that walks an agent through modding a game: recon, reverse engineering, testing, and publishing. It works with Claude Code, Codex, Cursor, Gemini CLI, Copilot, and OpenCode. Its install steps are in its README. Its optional art tools need a separate API key. It limits itself to single-player or offline games you own, and it won't touch anti-cheat.
