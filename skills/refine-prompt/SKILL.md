---
name: refine-prompt
description: Interviews the user to find what they are actually trying to achieve, then rewrites their prompt into a sharper, more ambitious, frontier-calibrated version. Use when the user asks to refine, improve, rewrite, sharpen, or make more ambitious a prompt, task brief, agent instruction, or project goal, including when they paste a prompt and ask to make it better, or invoke /refine-prompt. Produces a better prompt; does not execute the task.
---

# Refine Prompt

Someone is about to hand a task to a capable agent. Your deliverable is a better prompt for that hand-off, not the work itself. Don't start doing the task, even if you could; a half-done task hides the prompt's gaps instead of fixing them.

## What usually goes wrong

Most prompts underperform for two reasons, and both come from whoever wrote them, you included.

1. **They describe a route, not a destination.** A prompt is the writer's best guess at a path to something they want. The agent walks the path. Often the thing they want sits one level up (the goal behind the goal), one level down in rigor (how the best practitioner would do it), or across fields they didn't think to draw on.
2. **They are scoped to models from a year or two ago.** Writers, human and model, carry stale, low-biased priors about what agents can do. So prompts ask for results that are already commodity, micromanage steps the agent would plan better itself, hedge the bar downward, and leave out the verifier that would let an agent run for hours. Your own sense of what the executor can handle is probably low too. When you feel the urge to shrink the ask, add a micro-step, or add a hedge, treat that as a hypothesis to check rather than a judgment.

The fix is rarely a longer prompt. It is a prompt that states the outcome at the highest altitude the user is willing to delegate, points at raw material, sets a specific bar, and names an outside check. The interview exists to find those four things.

## Workflow

### 0. Get the prompt and the executor

- If the user didn't include the prompt, ask for it. Rough notes, a voice transcript, or a half-sentence are all fine.
- Work out who will run it: which agent or model, and with what access (shell, repo, data connectors, web, subagents, browsers, environments). Infer it from context when you can. Altitude is bounded by access: an agent can't keep an app crash-free if it can't fetch crash reports or reproduce on the right OS.
- Default to the interactive flow. If the user says "no questions", "just rewrite it", or isn't around to answer, use the fast path (step 6).

### 1. Diagnose before you ask anything

Every question costs the user time; anything you can look up yourself is free. Do your homework first.

- **Look around.** In a repo or workspace, skim README, CLAUDE.md or AGENTS.md, the files the prompt touches, and the tools and connectors available. Don't ask about anything you can discover.
- **Write a private diagnosis** (not shown to the user yet):
  - Stated task versus probable outcome.
  - What's missing: definition of done, a verifier, raw inputs, constraints, autonomy level, audience and format.
  - Ambition gaps on each lens (altitude, depth, range, frontier). See [references/ambition-lenses.md](references/ambition-lenses.md).
  - Depreciating prompt craft to strip: persona roleplay, rigid micro-steps, self-grading, bar-lowering hedges. See [references/prompt-anatomy.md](references/prompt-anatomy.md).
  - Your best guess for each unknown. These become the options you offer in the interview.

### 2. Calibrate against the current frontier

Run this whenever the prompt sets a capability-dependent goal (a success criterion, a benchmark target, "can a model do X", a fine-tuning or scaffolding plan), and whenever you're about to cap the prompt's ambition because agents supposedly can't do something yet.

- If the `bitter-lesson` skill is installed, load it and run its "already solved?" test. Otherwise search the web for current frontier and small or open-weight results on the specific task, including the current year in queries. Internal knowledge is a floor, not a reading, and it goes stale in weeks.
- If the stated goal is a commodity baseline, that becomes your first question. Say so plainly, with dated evidence and a source, and offer the directions that raise the bar: reliability, cost or latency, autonomous horizon, a messier domain, or a new verification substrate.
- Calibrate in both directions. Where evidence shows a real gap (reliability at the required threshold, no executable grader, judgment-heavy work, visual computer use), the rewrite adds verification and checkpoints instead of pretending the gap away.

Prompts with no capability claim ("draft my status update") skip the search. Still assume the executor is more capable than the prompt implies.

### 3. Interview: few questions, high leverage

Ask a question only if different answers would produce materially different prompts. If you can't say how the rewrite would change, don't ask; record it as an assumption instead.

- **Lead with hypotheses.** Each question offers 2–4 concrete options drawn from your diagnosis, with your recommendation first, plus room for a free-text answer. Reacting to a good guess is much easier than generating an answer from nothing.
- **Keep rounds small.** At most four questions per round. One round is typical; two is the ceiling unless the user wants to keep going.
- **Use the harness's structured question tool** if it has one (in Claude Code, `AskUserQuestion`: up to 4 questions, 2–4 options each, short headers, "Other" added automatically). Otherwise, ask numbered questions with lettered options so the user can reply "1b, 2a, 3: …".
- **Translate the dial into this prompt's terms.** "Altitude" means nothing to most people; "Own the outcome: keep CI green, not just retry failures" does.

Round 1 usually draws from these (details and option patterns in [references/question-bank.md](references/question-bank.md)):

1. **The goal behind the goal:** "When this is done, what happens next?"
2. **What right looks like, and what checks it:** the outside judge.
3. **How far to reach:** speed, altitude, depth, range. If step 2 found a commodity goal, this slot becomes the calibration challenge.
4. **Raw material and access:** the rawest source and whether the agent can reach it.

After round 1, prefer drafting to asking again. A draft is the best question you can ask: the user will see in seconds what's wrong with it. Ask a second round only when a decision genuinely blocks the draft. Typical round-2 topics are constraints and non-goals, autonomy and checkpoints, prior attempts, deliverable format, and, for long projects, the timeline.

**Respect the dial.** If the user picks "just this task, faster", don't smuggle the moonshot back in. Your job is to show them the dial exists and what each setting would buy, then build what they chose.

### 4. Draft the refined prompt

Build from [references/prompt-anatomy.md](references/prompt-anatomy.md). The core beats, scaled to the stakes:

- **Outcome:** what should be true afterwards, and why. The why lets the agent make good trade-offs nobody anticipated.
- **Context and raw material:** where things live, what access exists, what's been tried. Point at raw sources (logs, tables, code, transcripts) over digested summaries.
- **The bar:** a specific, ambitious target, written as reachable, because it usually is and specific hard goals outperform "do your best".
- **Methods and instruments:** what a top practitioner would actually use, and fields worth borrowing from, phrased as methods rather than personas. Leave this out when the agent should choose.
- **Verification:** an outside check the agent can run itself, ideally automated. Pair it with an honesty clause: don't edit tests, graders, or data to pass; report plainly what fell short or couldn't be verified. Telling an agent a hard target is reachable is motivation, not license to fake the result.
- **Constraints, non-goals, autonomy:** what's off-limits, which actions are irreversible and need a human, and when to check in versus keep going.
- **Deliverable:** form, location, audience.

Strip depreciating prompt craft unless the user specifically wants it. Keep a step list only where order is a real constraint (an irreversible migration, a compliance sequence).

Length follows stakes: deeper, not bigger. A one-liner might become a tight paragraph; a multi-week project brief might become a page. Before presenting, run the pre-send checklist at the end of the anatomy reference.

### 5. Present it

1. **The refined prompt** in a single fenced code block, ready to paste.
2. **What changed:** 3–6 bullets, each naming the move (altitude, depth, range, calibration, verification, clarity) and why it helps.
3. **Assumptions:** anything you assumed instead of asking, one line each, so the user can correct them fast.
4. **Optional dial variant:** if the user seemed torn, one line describing a more conservative or more ambitious version, with an offer to write it.

Then ask what to adjust. Iterate with targeted edits, not a fresh interview. If you're in a workspace and the user wants to keep it, offer to save it (for example `prompts/<slug>.md`).

**Cold-read test (optional, for substantial work).** If you can spawn a subagent or call a second model, give it the refined prompt and nothing else. Ask it to list what it would need to ask before starting and how it would verify success, but not to do the task. Fold real gaps back in. This is the outside-judge principle applied to the prompt itself.

### 6. Fast path

When there will be no interview: do steps 1–2, take the most reasonable reading, and write the prompt. Under the assumptions, add **"Questions that would most change this"**, listing the top 2–3 with how each answer would shift the prompt.

## Calibrated, not credulous

- **Ambition is not fantasy.** A target should exceed what the user would have asked for, not exceed physics or the evidence. Pair every big ask with a measurement.
- **Altitude needs a checkable outcome.** "Keep the app crash-free" works; "go make money" still doesn't. Pick the highest altitude that still has a check. Altitude also helps less with creative and expressive work, where depth and range usually pay more.
- **Tie autonomy to reliability, not possibility.** That an agent can do something half the time doesn't mean it should run unattended. For long unattended runs, the rewrite should add milestone verification or checkpoints sized to what the evidence supports.
- **Don't inflate small tasks.** A README or a status update gets clarity and a cheap outside check, not a moonshot.
- **Keep the honesty clause.** Scale it to the stakes, but never drop it.

## References

- [references/ambition-lenses.md](references/ambition-lenses.md): speed, altitude, depth, range, and frontier, with the probes and moves for each and how to choose among them.
- [references/question-bank.md](references/question-bank.md): question templates by category, what each changes, option patterns, and what not to ask.
- [references/prompt-anatomy.md](references/prompt-anatomy.md): the rewrite template, the strip list with replacements, and the pre-send checklist.
- [references/examples.md](references/examples.md): worked before-and-after examples (an AI project goal, an engineering workflow, a small task kept small).

The altitude, depth, and range lenses come from Felix Rieseberg's "How to be more ambitious: A tutorial for humans with models" (October 2026). The frontier lens comes from Rich Sutton's Bitter Lesson and the companion `bitter-lesson` skill.
