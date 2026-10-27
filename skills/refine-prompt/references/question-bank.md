# Question Bank

Questions spend the user's time. Each entry below says what its answer changes in the rewrite. If that change wouldn't happen for this prompt, skip the question and write an assumption instead.

**Format for every question:** a hypothesis-led question, 2–4 concrete options built from your diagnosis (your recommendation first, marked), and room for a free-text answer. Phrase options in this prompt's own terms, never as bare labels like "Altitude".

> **Example (structured tool):**
> Q: "When the deck is done, what's it for?"
> Options: "Drive 2–3 decisions in the review (Recommended)" · "Just report the numbers" · "Replace the quarterly review with something better"
>
> **Example (plain text):**
> 1. When the deck is done, what's it for?
>    a) Drive 2–3 decisions in the review *(my guess)*
>    b) Just report the numbers
>    c) Replace the quarterly review with something better

---

## 1. Outcome and altitude

*Changes: the stated outcome, and whether the prompt targets a one-off task or a standing result.*

- "When this is done, what happens next?"
  - Option pattern: just the task, I'll take it from there · it feeds [a decision or meeting], so optimize for that · this recurs, so make it a standing workflow · I want the underlying problem gone
- "If this worked perfectly, what would you stop doing by hand?"
- "Who uses the output, and what do they do with it?"

## 2. Done and verification

*Changes: the verification section and the definition of done.*

- "How would you know it's right? What would convince a skeptic?"
  - Option pattern: tests or CI pass · a metric hits [X] on held-out data · it reproduces a known result · a named reviewer signs off · I'll eyeball it
  - If the answer is "I'll eyeball it", still propose one cheap runnable check and ask whether to include it.
- "Is there something the agent could run to check its own work?"

## 3. How far to reach

*Changes: the bar, and which lenses shape the rewrite.*

- "How far should this go?" Give one option per lens that applies, each written as a concrete version of this prompt.
- "What would a result ten times better than what you asked for look like?"
- "What target would feel unreasonable?" Then consider asking for that, stated as reachable and paired with a measurement.

## 4. Calibration challenges

*Ask when step 2 finds a commodity goal or a depreciating plan. Changes: the goal itself.*

- "Current agents already [do X] out of the box ([dated evidence, source]). Is the real goal…"
  - Option pattern: the same quality at a fraction of the cost, or on [constraint] hardware · higher unattended reliability on our own workload · longer autonomous runs · a domain where agents still fail · a verifier or eval that doesn't exist yet · the constraint is real: we need X under [specific limit]
- "This plan's value depends on models staying weak at [Y]. Over your [T]-month timeline, they likely won't. Keep it as a labeled stopgap, or redirect toward [appreciating alternative]?"

## 5. Raw material and access

*Changes: the context section and whether access setup becomes part of the task.*

- "What's the rawest source for this, and can the agent reach it?"
  - Option pattern: the repo · a database or warehouse · logs or telemetry · docs through a connector · I'll paste or attach it
- "What can it use to reproduce and verify? (staging, a sandbox, specific OS environments, test accounts)"

## 6. Constraints and non-goals

*Changes: the constraints section.*

- Budget or time limits; privacy and compliance; things not to touch; conventions the agent can't infer; anything that must not change.

## 7. Autonomy

*Changes: check-in policy and which actions need a human.*

- "Should it run to completion, or check in along the way?"
  - Option pattern: ask only before irreversible or external actions *(usually recommended)* · check in at milestones · run to done and report · propose a plan first and wait
- For long unattended runs, size checkpoints to what the reliability evidence supports.

## 8. History

*Often the highest-yield question when the prompt is a retry. Changes: context, and what to avoid.*

- "What has already been tried, by you or a model, and where did it fall short?"

## 9. Deliverable

*Changes: the deliverable section.*

- Form (PR, memo, dataset, doc, dashboard), location, audience, and length.

---

## Don't ask

- Anything you can find in the repo, workspace, connectors, or on the web.
- Preferences that wouldn't change the prompt.
- "Anything else?" as a question of its own. The presentation step already invites changes.
- Questions that hand your thinking to the user, like "what methods should it use?". Propose methods and ask them to confirm.
- A second round when a draft would answer it faster.
