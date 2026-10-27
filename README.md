# refine-prompt

An agent skill that turns a rough prompt into a sharper, more ambitious, frontier-calibrated one. It works by first interviewing you about what you're actually trying to achieve.

Open an agent, invoke the skill, and paste your prompt. The agent will:
1. Do its homework first, reading your repo, tools, and context.
2. Ask a few high-leverage questions, each with multiple-choice options built from its own best guesses.
3. Hand back a refined prompt, ready to paste, along with what changed, why, and what it assumed.

It produces the prompt; it does not do the task.

## Why This Exists

Prompts underperform for two reasons that compound:

1. **They describe a route, not a destination.** The writer's best guess at a path gets executed literally. The thing they actually want often sits one level up (the goal behind the goal), one level deeper (how the best practitioner would do it), or across fields they didn't think of.
2. **They're scoped to old models.** Humans and models both carry stale, low-biased priors about agent capability. Prompts end up asking for results that are already commodity, micromanaging steps the agent would plan better, hedging the bar down, and omitting the verifier that would let an agent run for hours. A model once suggested this as a project's success criterion: *"fine-tune a model so it goes from fixing small bugs to working on large codebases."* Current agents, including small ones, already do that.

## What the Skill Does

- **[`skills/refine-prompt/SKILL.md`](skills/refine-prompt/SKILL.md)**: the core loop.
  1. **Diagnose before asking:** find everything discoverable, then form a hypothesis for each unknown.
  2. **Calibrate:** run the "already solved?" test against live evidence, through the companion [`bitter-lesson`](https://github.com/3xC1ibR/bitter-lesson) skill if installed, or a web search otherwise. A commodity goal becomes the first question.
  3. **Interview:** at most four questions per round, hypothesis-led and multiple-choice, asked only if the answer would change the prompt. After round 1, it prefers showing a draft to asking more.
  4. **Rewrite:** outcome and why, raw material, a specific ambitious bar, methods (not personas), an outside check with an honesty clause, constraints and autonomy, and the deliverable. It strips depreciating prompt craft along the way.
  5. **Present and iterate:** the prompt in a code block, what changed, and its assumptions. Optionally, a cold-read test by a fresh subagent.
- **[`references/ambition-lenses.md`](skills/refine-prompt/references/ambition-lenses.md)**: speed, altitude, depth, and range (after Felix Rieseberg), plus a frontier lens (after the Bitter Lesson), with probes and moves for each.
- **[`references/question-bank.md`](skills/refine-prompt/references/question-bank.md)**: question templates by category, what each answer changes, option patterns, and what not to ask.
- **[`references/prompt-anatomy.md`](skills/refine-prompt/references/prompt-anatomy.md)**: the rewrite template, the strip list with replacements, and a pre-send checklist.
- **[`references/examples.md`](skills/refine-prompt/references/examples.md)**: worked examples covering an AI project goal, an engineering workaround, and a small task kept small.

## Design Choices

- **Ambition is a dial the user sets.** The skill shows what each setting would buy and then respects the choice. Small tasks get clarity and a cheap check, not a moonshot.
- **Calibrated, not credulous.** Ambitious targets come paired with a measurement and an honesty clause. Autonomy is sized to what reliability evidence supports.
- **No hardcoded capability numbers.** Those go stale in weeks. The `bitter-lesson` skill's dated baseline, or a live search, is the source of truth.

## Installation

```bash
git clone https://github.com/3xC1ibR/refine-prompt.git

# Generic agent skills directory
ln -sfn "$(pwd)/refine-prompt/skills/refine-prompt" ~/.agents/skills/refine-prompt

# Claude Code (personal skills)
ln -sfn "$(pwd)/refine-prompt/skills/refine-prompt" ~/.claude/skills/refine-prompt
```

Install alongside [`bitter-lesson`](https://github.com/3xC1ibR/bitter-lesson) for the calibration step.

## Usage

```
/refine-prompt Build a RAG system over our Confluence so engineers can ask questions about internal docs.
```
or
```
Use the refine-prompt skill on this: <your prompt>
```
Add "no questions" to skip the interview. The agent states its assumptions and lists the questions whose answers would most change the result.

## Credits

- Felix Rieseberg, *How to be more ambitious: A tutorial for humans with models* (October 2026), for the altitude, depth, and range lenses.
- Rich Sutton, *The Bitter Lesson*.

## License

MIT
