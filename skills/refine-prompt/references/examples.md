# Worked Examples

Three examples showing the flow from original prompt to diagnosis, questions, and refined prompt. They illustrate the moves; they aren't templates. Capability claims in your own runs should come from a live search or the `bitter-lesson` baseline, dated and sourced, not copied from here.

---

## 1. An AI project goal that's already a commodity baseline

**Original prompt**
> Help me plan an LLM project. Success = fine-tune an open model so it goes from fixing small bugs to working on large codebases.

**Diagnosis**
- Calibration fails. A live check shows that current frontier agents, and small or open-weight coding models, already do multi-file, repo-level work in a plain shell harness. The stated goal is a baseline, not a contribution.
- Fine-tuning "to make the model capable of X" is the bad reason for fine-tuning: it races the next checkpoint release and loses.
- Missing: the why, constraints (hardware, cost, privacy), an eval on the user's own work, and a timeline.

**Round 1 questions**
1. Current agents, including small open-weight ones, already work across large repos ([dated source]). What's the real goal?
   a) Frontier-level quality at a fraction of the cost, on our own hardware *(my guess)*
   b) Higher unattended reliability on our own codebase
   c) Longer autonomous runs, such as multi-day migrations
   d) A domain where current agents still fail (say which)
2. What's the binding constraint?
   a) A single 24GB GPU per developer *(my guess)*
   b) Per-task API cost
   c) Code can't leave our network
   d) None
3. How would you know it worked?
   a) Pass rate on our own merged PRs, replayed as tasks with their tests *(my guess)*
   b) A public benchmark
   c) Engineer acceptance rate in production

*User answers: 1a, 2c, 3a. Timeline about three months.*

**Refined prompt**
```
Goal: get frontier-agent quality on our own repo-level engineering work from a model that
runs inside our network, at a small fraction of frontier API cost. Code can't leave the
network, which is why we can't simply use a hosted frontier model.

Start by establishing the baseline; don't plan any training yet. Build an eval from 30–50 of
our merged multi-file PRs, held out by date, each replayed as a task in a sandboxed checkout
with that PR's tests as the grader. Run (A) the best current frontier agent, on a sanitized
subset we're allowed to share, and (B) the strongest open-weight coding models that fit our
hardware, all in the same plain harness: shell, file tools, test runner. Search for what is
current; the models you remember as best are probably out of date.

Only if B trails A meaningfully on first-attempt pass rate: propose a distillation plan that
trains on A's verified trajectories over our own task distribution. Estimate whether the gap
will survive the next three months of open-weight releases. If it likely won't, recommend
waiting and investing in the eval instead.

Verification: pass@1 and pass@3 with confidence intervals, cost and latency per task, and
human cleanup minutes on a 10-task sample. Never edit the PR tests or the grader, and flag any
trajectory that touches test files.

Deliverable: a one-page go / wait / change-the-goal memo, plus the eval harness committed to
the repo so we can rerun it against each new model release.
```

**What changed**
- **Calibration:** replaced a solved goal with an open one (quality under a hard privacy and cost constraint).
- **Bitter Lesson:** fine-tuning moved from "teach a capability" to "distill a proven behavior for economics", gated on measured evidence.
- **Appreciating asset:** the reusable eval harness outlives any single model.
- **Depth:** the eval uses the team's own PRs, not a public benchmark.
- **Verification:** includes an anti-gaming clause, since agents sometimes edit tests to pass.

---

## 2. A workaround hiding a better outcome

**Original prompt**
> Write a script that reruns failing tests in CI so flaky tests stop blocking merges.

**Diagnosis**
- **Altitude:** the retry script is a workaround. The likely outcome is that red builds mean real regressions and merges aren't blocked by noise.
- **Depth:** a practitioner would start from raw CI history, classify root causes, and reproduce each failure before fixing it.
- **Bitter Lesson:** blanket retries depreciate trust in CI and hide real bugs.

**Round 1 questions**
1. After merges are unblocked, what do you want?
   a) Flakiness gone, not hidden *(my guess)*
   b) Just unblock merges this week; deeper fixes later
2. Can the agent reach CI history through the API or CLI?
   a) Yes, the last 90 days *(my guess)*
   b) Only current logs
3. May it open PRs that change tests?
   a) Yes, one PR per root cause, with human review *(my guess)*
   b) Report only

*User answers: 1a, 2a, 3a.*

**Refined prompt**
```
Make CI trustworthy: a red build should mean a real regression, and flaky tests shouldn't
block merges.

Pull the last 90 days of CI runs. Find tests that both pass and fail on the same commit, and
rank them by how many merges they blocked. For the top offenders, find the actual cause
(timing, ordering, shared state, network, resource limits) by reproducing it: run each test
isolated and in shuffled order until it fails, and confirm the cause before fixing it. Fix
causes, not symptoms. Quarantine a test, with an owner and a ticket, only when the fix needs
a human decision.

Then add a standing guard: a nightly shuffled-order run that flags newly introduced flakes
within a day.

Verification: each fixed test passes 200 of 200 isolated and shuffled runs. Replay the last
30 days of CI history and report how many blocked merges the fixes would have prevented.
Blanket retries and skip markers don't count as fixes.

Constraints: one PR per root-cause class. Flag, and don't merge, any change to product code
behavior.

Deliverable: the PRs, plus a short table listing each test, its cause, the fix, and the
evidence.
```

**What changed**
- **Altitude:** moved from a retry script to trustworthy CI, with a standing guard so the problem stays fixed.
- **Depth:** raw history, reproduction before fixing, and root-cause classes.
- **Bar:** 200 of 200 runs, stated as the expectation.
- **Verification:** a historical replay gives an outside measure of impact.

---

## 3. A small task, kept small

**Original prompt**
> write a README for my repo

**Diagnosis**
- Speed is the right setting; no altitude is needed.
- **Depth, done cheaply:** the code is the raw source, because existing docs may be stale. A clean-room walkthrough is a near-free outside judge.
- Missing: the reader, and what they should be able to do afterwards.

**Round 1 question** (only one changes the prompt)
1. Who's the reader?
   a) A developer who should get it running in five minutes *(my guess)*
   b) Contributors who need the architecture
   c) Someone evaluating the project at a glance

*User answer: a.*

**Refined prompt**
```
Write README.md so a developer who has never seen this repo can install it and get it running
in under five minutes. Work out the real install path, configuration, and entry points from
the code itself; existing docs and comments may be stale. Then verify it: in a clean container,
follow only the README step by step, and fix the README (not the code) wherever you get stuck.
Keep it short: two sentences on what it is, a quickstart, configuration, and one example. Tell
me anything you couldn't get working.
```

**What changed**
- **Depth:** reads from the code rather than stale docs.
- **Verification:** the clean-container walkthrough acts as an outside judge.
- **Proportion:** no scope inflation and only one question, because only one answer would have changed the prompt.
