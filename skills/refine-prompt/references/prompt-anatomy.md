# Prompt Anatomy

## Template

Adapt this; don't fill it in mechanically. For short prompts, write a paragraph that hits the same beats and drop the headers.

```
<Outcome: what should be true when you're done, and why it matters.>

Context
- <where things live, what access you have, what has already been tried>
- <the raw sources to start from>

Bar
- <the specific target, written as achievable>

Approach
- <methods and instruments a top practitioner would use; fields to draw on; beliefs to challenge>
  (Leave this out if the agent should choose its own approach.)

Verification
- <runnable or outside checks>
- Don't modify tests, graders, or data to make checks pass. If you can't meet the bar,
  or can't verify something, say so plainly and show what you did get.

Constraints and autonomy
- <off-limits areas; which actions need my OK; when to check in>

Deliverable
- <form, location, audience>
```

---

## Strip list: depreciating prompt craft

These patterns cost more than they return with current agents. Replace them unless the user specifically wants them.

| Pattern | Why it hurts | Replace with |
| :--- | :--- | :--- |
| "You are a world-class X" | Changes vocabulary, not methods | The methods and instruments that X actually uses |
| Numbered micro-steps for planning the agent can do itself | Caps the agent at the writer's plan; brittle when reality differs | Outcome, constraints, and verification. Keep steps only where order is a real constraint |
| "Double-check your work" or other self-grading | Self-evaluation is a weak judge | An outside check the agent can run |
| "If this is too hard, just…" or "do your best" | Lowers the bar; specific hard goals outperform "do your best" | A specific target plus an honest-reporting clause |
| Digested inputs ("here's a summary of the logs") | The agent inherits the summarizer's blind spots | A pointer to the raw source |
| ALL CAPS, threats, bribes, "this is VERY important" | Noise that crowds out the reason | One sentence explaining why it matters |
| Saturated benchmarks as goals (HumanEval and the like) | Already solved; proves nothing | A discriminating benchmark or an eval on the real workload (check the `bitter-lesson` baseline or search) |
| Goals that are commodity baselines | Not a contribution | A higher bar on reliability, cost, horizon, domain, or verification |
| Chunking or context-window workarounds the harness no longer needs | Depreciates with every model release | Let the agent search and read; mark any remaining workaround as a stopgap |
| "Output only X, no explanation" when the user must trust the result | Removes the evidence | Require a short verification summary alongside X |

## Keep: craft that still pays

- The reason behind each constraint.
- An example of good output when the format matters.
- An explicit definition of done.
- Order constraints that are real (migrations, irreversible operations, compliance sequences).
- Conventions the agent can't infer from the workspace.
- Who will read the result and what they'll do with it.

---

## Pre-send checklist

- Could an agent with no access to this conversation act on the prompt? Is every piece of context either included or pointed at?
- Is the outcome stated at the altitude the user chose, not lower?
- Is there an outside check, and can the agent run it itself?
- Is the bar specific, and did it pass the "already solved?" test?
- Is anything in the prompt compensating for a weakness the executor doesn't have?
- Is the honesty clause proportionate to the stakes?
- Is this the shortest version that does all of the above?
- Did you sneak in ambition the user declined?
