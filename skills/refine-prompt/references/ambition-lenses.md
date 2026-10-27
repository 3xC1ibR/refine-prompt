# Ambition Lenses

Use these during diagnosis to find what a prompt is leaving on the table, and to generate the options you offer in the interview. Four lenses come from Felix Rieseberg's taxonomy (speed, altitude, depth, range). The fifth, frontier, comes from Bitter Lesson calibration.

---

## Speed: the same work, in less time

A legitimate setting, and many prompts should stay here. Improve clarity, context, the deliverable, and add a cheap outside check. Don't add altitude the user didn't ask for.

---

## Altitude: operate on the outcome, not the task

**Probe:** What do you want to happen next? What is the goal behind the goal?

A typical ladder: fix this crash → fix this class of crash → keep the app crash-free → make crashes hard to ship in the first place.

**Signals that altitude is available:**
- The output feeds a meeting, a decision, or another task.
- The same kind of prompt recurs (weekly, per ticket, per release). That suggests a standing outcome or durable workflow instead of one more one-off.
- The user names an artifact format (a deck, a report, a script) rather than an effect they want.
- The requested artifact is a workaround for a deeper problem (a retry script for flaky tests, a summary of a dashboard nobody trusts).

**Moves:**
- Restate the ask as an effect: "make X true and keep it true."
- Ask for the decision, not the summary: "work out which decisions are most urgent given what you find, and draft options with arguments for and against."
- Question the artifact itself: "this process exists so that ___. Design a better way to achieve that, show from our own history that it would have worked, and run it beside the current process."
- Name the workflow changes the agent will need: access to raw data, environments to reproduce and verify in, mappings it can't infer (release version to commit, customer ID to account). Either supply them as context, or make "set up the access you need, and tell me what you couldn't get" part of the task.

**Limits:** Unbounded outcomes still fail. Pick the highest altitude that still has a checkable outcome. Altitude helps less with creative and expressive work.

---

## Depth: do it the way the best in the field would

**Probe:** What would a top professional do here that this prompt doesn't ask for?

Felix's four-part check, paraphrased:
1. What would a top practitioner do that isn't being asked for?
2. What is the rawest material they'd start from, instead of what has already been digested?
3. Which specific methods and tools would they use?
4. What, other than the agent or the user, would show the result is right?

**Moves:**
- **Instruments over vocabulary.** Telling a model to "act like an expert" tends to change its jargon without changing its methods. Name the methods instead: a database and a statistical model rather than a markdown table; a profiler and flame graphs rather than "optimize"; property-based tests rather than "add tests". You don't have to know the right instruments yourself; propose your best guess and let the user confirm.
- **Raw over digested.** Logs, event tables, source code, transcripts, not a summary or a dashboard export. Digested inputs pass the summarizer's blind spots to the agent.
- **Unreasonable targets, stated as reasonable.** If the user wants 10 seconds cut to 5, consider asking for 200 milliseconds. The target should go beyond what the user would have asked for, not beyond what's physically possible, and it always comes with a measurement.
- **Adversarial checks on beliefs.** "We believe X. Try to reproduce that from the raw data."
- **Produce new knowledge.** "Find something we don't know yet, and show it holds on data it hasn't seen."
- **An outside judge.** A runnable check (tests, benchmarks, held-out data, a reproduction, a compiler, a clean-container walkthrough, an independent second agent). Not self-review.

---

## Range: bring in other fields

**Probe:** Which other fields would see this problem differently?

**Moves:**
- **Nouns to verbs.** From Tom Kelley and Jonathan Littman's *The Art of Innovation*. A quarterly review isn't "a deck"; it's getting busy people to believe a cause-and-effect story and decide under uncertainty in thirty minutes. The verbs point at the fields that can help (here: analysis, journalism, editing, decision science).
- **Assemble the crew.** If this had to be extraordinary, who would you hire? Include a red team: a short seller, a forensic accountant, a security researcher, a hostile reviewer.
- **Methods, not roleplay.** Ask the agent to do what a short seller does to research a position, not to write like one. Roleplay makes transcripts stranger without improving outcomes.
- **Use parallelism.** When the executor has subagents, each field can be its own investigation feeding a synthesis.

---

## Frontier: calibrate the bar to current models

**Probe:** Would a current frontier agent, or a small or open-weight one, already do this in a plain harness (shell, files, tests, search)?

- **If yes, it's a baseline, not a goal.** Raise the bar along one of:
  - reliability (from roughly half the time, to most of the time, to unattended),
  - cost, latency, or memory footprint (distillation, edge, on-prem, privacy),
  - autonomous horizon (from hours toward days),
  - a messier or less verifiable domain,
  - a new verification substrate that nobody has built yet.
- **Project long plans forward.** Plans are judged when they ship. With task horizons doubling every few months, a 6-month project ships into noticeably stronger models, and a 12-month one into much stronger models. Ask whether the work appreciates or depreciates as models improve.
- **Prefer appreciating components** in whatever the prompt asks the agent to build: verifiers, deterministic sandboxes, evals on the real workload, proprietary data and feedback loops, ways to spend inference compute. If the prompt must include depreciating scaffolding (hand-written decompositions, chunking heuristics, prompt tricks, narrow capability fine-tunes), label it a stopgap and say what would trigger deleting it.
- **Size the executor correctly.** Assume the agent can plan, search, read large codebases, and work for hours. Give it the outcome and the verifier, not the steps. Where evidence shows a real limit (reliability at the required threshold, no executable grader, taste-heavy judgment, visual computer use), add checkpoints and verification, not hedges.
- **Source of truth for numbers:** the `bitter-lesson` skill's dated baseline or a live search. If a refined prompt cites a capability number, date it and name the source.

---

## Choosing lenses

Most prompts need one or two lenses, not all five.

| Signal in the prompt or answers | Lens to reach for |
| :--- | :--- |
| Recurs, feeds a decision or meeting, or is a workaround | Altitude |
| Analysis, research, optimization, anything with data | Depth |
| Communication, design, strategy, a problem that's been stuck | Range |
| Any AI, ML, or agent goal; any capability claim or fine-tuning plan | Frontier |
| One-off, low stakes, the user wants it fast | Speed (clarity and a cheap check only) |
