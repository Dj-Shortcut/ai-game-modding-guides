# 4. Prompting and Workflow

## There are no magic prompts

The most experienced members agree on this: you tell the agent what you want, and it does it. People who've shipped these projects say they link an example repo, say "I want this for [my games]," and let the agent work.

The part people disagree on is how much detail to put in the first message. Here are both sides, so you can try each.

## Two schools of thought

**Short and loose.** Some members say a huge, perfectly written first prompt is a trap. They dictate by voice, ramble for a bit, and send a messy message. Their reasoning: the agent knows the efficient path, and over-specifying can send it down the wrong one. One member's current prompt in a long session was roughly "there's still some stuff missing, right? ok let's add it."

**Detailed with context.** Others argue that the more context you give, the better the result, and that good prompting saves time and usage. They care about efficiency, especially on plans with usage limits. This hasn't been settled in the server, and it's a good topic for someone to test and write up.

### What both sides agree on

- **Be specific about the problem, not the implementation.** "This looks bad, fix it" gives the agent nothing. "The door doesn't open when I press E next to it, and the log says X" does.
- **Give the goal and the evidence.** Say what you wanted, what happened, and paste the logs.
- **Models tunnel-vision.** If the agent is stuck on the wrong approach, say so and point it somewhere else.
- **A good example project beats a long explanation.** Linking SkyCraft or hl2-rs does more than describing them.

## Working in small steps

Asking for the whole game at once usually goes badly. These habits help:

- Ask for one thing at a time and have the agent tell you how to run it and what you should see.
- Commit to Git after every working step. Version control lets you undo mistakes.
- Ask the agent to write a plan first if the task is big or you want more control.

## Keep the project's memory on paper

Agents forget between sessions. Files don't. Keep three small documents in your project:

1. **A rules file** (`AGENTS.md` or `CLAUDE.md`): what the agent must always do or never do. Example projects keep one. See [`templates/AGENTS-starter.md`](../templates/AGENTS-starter.md).
2. **A development log** (`MODLOG.md`): what changed, how it was tested, what's still broken. See [`templates/MODLOG-template.md`](../templates/MODLOG-template.md).
3. **A design doc** (`docs/DESIGN.md`): how the project works, in plain language.

Ask the agent to update them as it goes.

## The handoff trick for stuck chats

When a chat gets confused or very long, one member recommends:

1. Ask the agent to write the project's status and the exact problem it's stuck on into a detailed `.md` file.
2. Open a **fresh chat** and give it that file.
3. Ask it to read the file and suggest ways to troubleshoot.

They say this helps most with less capable models. Another member simply closes and reopens chats to clear old context. A template is in [`templates/STATUS-handoff.md`](../templates/STATUS-handoff.md).

## Saving usage

- Long chats carry all their history, so they use more of your limit. Starting fresh with a handoff file helps.
- Close and reopen chats when you switch topics.
- One member suggests asking the agent, early on, to set up a documentation standard and to keep token efficiency in mind without losing functionality.
- Don't assume any of this is a rule. Plans and models change.

## How long things take

It depends on the model, how hard it thinks, and how big the task is. Members report anything from about 5 minutes to many hours for a single piece of work.

## Let the agent test what it can, and you test the rest

See [guide 5](05-testing-and-troubleshooting.md). The short version: the agent is bad at judging visuals and "feel." Make it log numbers and events, and you do the playtesting.

---

## The prompting debate, in members' own words

**This section is open. It's meant to be argued with.**

There was a long thread on this in the server and it got heated. Rather than pretend we settled it, here is what was actually said. Read both, try both, and post your results in [Discussions](https://github.com/trevaintdead/ai-game-modding-guides/discussions) or in the tech-support forum.

> **chazm** — "guys theres no tricks or special prompts, you literally just tell the ai to do stuff and itll do it. Thats all i do"
>
> "if you write some big detailed prompt exactly how you want it, then its gonna be worse than letting the ai wing it. The ai knows the best and most efficient path to the outcome you want."
>
> "the more specific the worse by far my man"

> **chazm**, later, citing Andrej Karpathy: *"One pattern I find useful for working with LLMs is a nice long ramble session. Sometimes the LLM needs more bits to understand what you're trying to achieve, but you're too lazy to type them."* — [source](https://x.com/karpathy/status/2079610838143623371)

> **Paragon-7** — "These are literally inference machines they require context. The more context you provide the better."
>
> "The more specific and accurate your prompt is, the better the weights will be set. The faster and more efficient the model is at doing the asked task."
>
> "The prompt is like 0.1% of the context of a chat."

> **Iroquois [MLBB]** — "AI tunnel visions on implementations a lot."
>
> "In this context I agree, in a context of a professional its the opposite."

> **laundry** — "ive found more specific prompts can cause tunnel vision on the wrong things, well in some cases."
>
> "my current prompt running right now is 'there's currently still some stuff missing right? ok lets add it'"

> **toast** — "i'd start asking it inside your ide to create a documentation standard. tell it that youre concerned with token efficiency, but you don't want to sacrifice functionality."

### What the disagreement is actually about

Reading those side by side, there are two separate arguments tangled together:

**1. Does over-specifying pick the wrong approach?** The short-and-loose side says yes: if you dictate the implementation, the agent commits to it, and models tunnel-vision on the first idea they form. The context side says you should describe the *goal* precisely and let the agent choose the route — precision about the outcome is not the same as dictating the method.

**2. Does it save time?** The context side's strongest argument is usage limits. If a vague prompt makes the agent wander and you burn your 5-hour window on dead ends, "efficient" wins even if the vague prompt would have eventually worked. One member pointed out a prompt is a tiny fraction of a chat's context, so prompt length itself may be the wrong thing to optimise.

### Our honest read

Not a verdict. Something you can try either way:

- **Be precise about the goal and the evidence.** Both sides agree on this and it's the thing that actually matters.
- **Be loose about the implementation.** The main documented failure mode is tunnel-vision, so pointing at a method rather than a result is a real risk.
- **When you're stuck, don't rewrite the prompt — change the context.** Start a fresh chat with a [`STATUS-handoff.md`](../templates/STATUS-handoff.md) file. That works regardless of which school you're in.
- **Long-running projects need both.** The loose approach is fine for a two-hour project. Once you're 200 commits in, the rules file and the log matter more than any individual prompt.

### Try it and tell us

If you run a controlled comparison, this is genuinely wanted here. Log the same task twice with a loose prompt and a detailed one, and record:

- how many turns it took
- whether it reached a working result
- how much of your usage limit it consumed
- what it got wrong

Post it in [Discussions](https://github.com/trevaintdead/ai-game-modding-guides/discussions) and we'll fold the best ones into this page.

### A few things that came up in the thread

- **Voice dictation.** Several people dictate instead of typing, which gets you a long rambling prompt for free and removes the temptation to over-edit it.
- **Ink-and-paper is cheaper than you think.** A prompt is a small part of a chat's cost, but a 200-turn chat is not. Starting fresh and handing over a file is the real saving.
- **Let it test, but stop it from looking.** It will try to visually verify the game, which it cannot do. See [guide 5](05-testing-and-troubleshooting.md).
- **Explain your constraints, not your implementation.** "It needs to work on a 2015 laptop" is context. "Use a thread pool" is an instruction.

---

---

<sub>[Spot a mistake? [Edit this page on GitHub](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/04-prompting-and-workflow.md).](https://github.com/trevaintdead/ai-game-modding-guides/edit/main/guides/04-prompting-and-workflow.md) &middot; [Open an issue](https://github.com/trevaintdead/ai-game-modding-guides/issues/new) &middot; Part of [AI Game Modding Guides](https://github.com/trevaintdead/ai-game-modding-guides)</sub>
