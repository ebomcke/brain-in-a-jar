---
name: decision-log
description: Log an architecture, implementation, or planning and priority decision the user makes to their private, local decision log, so their decision-making principles can be derived later. Use when the user overrides or corrects an agent's proposal ("no, don't add a queue"), chooses between laid-out options ("go with B"), or sets direction or priority unprompted ("do X before Y"), and a capable agent could reasonably have chosen differently; or when they say "log this decision". Not for naming, formatting, tooling setup without a trade-off, mechanical instructions, or plain approvals of a single proposal with no real trade-off.
---

# Decision log

Capture the user's decisions with minimal friction. The log is raw evidence for later analysis of the principles they decide by, so **accuracy about whose reasoning it is matters more than completeness**.

## When to log

The log will train a reviewer that steers agents, so it captures decisions where the user's judgement adds something an agent would not have had. Log only when **all three** hold:

1. **Category:** architecture, implementation, or planning and priorities.
2. **Real alternative:** a capable agent could reasonably have chosen differently, and the alternative was not obviously worse.
3. **Outlives the session:** it shapes code, plans or commitments others (people or agents) will build on.

Signal strength, highest first:

| Signal | Example | Log? |
|---|---|---|
| Overrides an agent | "No, don't add a queue, call it synchronously" | Always |
| Chooses between laid-out options | "Go with B" | Yes |
| Sets direction or priority unprompted | "Do X before Y this cycle" | Yes |
| Approves a single proposal | "Yes, do that" | Only if the proposal carried a real trade-off |

Skip:
- Naming, formatting, tooling setup with no trade-off.
- Mechanical instructions ("run the tests", "fix the typo").
- Plain approvals with no alternative in play.
- Restating a decision already logged in this session (update that entry instead).
- Choices Claude made itself without the user weighing in.

When a decision meets the bar but you are unsure of its category, log it with your best guess.

## How to log

1. **Write the entry first, silently.** Don't ask anything before writing. Fill what the conversation supports; leave the rest empty. Never invent options or reasoning that was not discussed.
2. **Then ask at most one short, optional question**, only if a useful field is missing, in a single line at the end of your normal reply. Pick the most valuable gap, usually:
   - "Logged. What did you trade away here? (skip is fine)"
   - "Logged. What would make you reverse this? (skip is fine)"
   If the reasoning was already stated, ask nothing. Just add one line: `Decision logged.`
3. **Never block or derail the work.** Carry on with the task in the same turn. Any reply that doesn't answer the question is a skip; don't ask again.
4. **If they answer**, add the answer to the entry with `reasoning_source: reflected` for that part, quoted in their own words.
5. **If the user says "don't ask today"** (or similar), keep logging for the rest of the session but ask no follow-up questions.

## Where

One file per decision, written with the Write tool (not Bash). Expand `~` to the user's home directory:

```
~/PersonalAssistant/decisions/YYYY/MM/YYYY-MM-DD-<short-kebab-slug>.md
```

Never overwrite another entry. If the path exists, append `-2`, `-3`. This backlog is gitignored; never commit it.

**Reversals.** If the decision reverses or replaces one already logged (same session, or found by searching `decisions/` for the topic), write a new entry rather than editing the old one. Set `supersedes:` to the old entry's slug (filename without `.md`), and set `superseded_by:` in the old entry to the new slug. Change nothing else in the old entry. Note in Context what new information prompted the reversal.

## Entry format

```markdown
---
date: YYYY-MM-DD
project: <working directory or repo name of this session>
decision: <one sentence, what was chosen>
category: architecture | implementation | planning
overrode_agent: true | false                         # true if they rejected or corrected an agent's proposal
supersedes:                                          # slug of the earlier entry this reverses or replaces, if any
superseded_by:                                       # left blank; set when a later entry reverses this one
options_considered: [<chosen>, <alternative>, ...]   # only those actually discussed
reasoning_source: stated | inferred | reflected | mixed
reversibility: easy | costly | one-way | unknown
confidence: high | medium | low | unknown            # the user's, not Claude's
tags: [<2–4 lowercase topics, e.g. architecture, hiring, scope, security>]
outcome:                                             # left blank; filled at review
---

## Context
<1–3 lines: the situation and what forced the choice.>

## Reasoning (stated)
<The user's own reasoning, quoted or closely paraphrased. Empty if they gave none.>

## Reasoning (inferred)
<Claude's reconstruction from context, clearly hedged. Empty if not needed.>

## Trade-offs
<What was given up or accepted as a risk, if known.>

## Reflection
<Answers to the follow-up question, verbatim. Empty if skipped.>
```

## Rules for the reasoning fields

- `stated` = the user said it. `inferred` = Claude's guess. `reflected` = answered on prompting. `mixed` = more than one of these sections is filled.
- Never move inferred reasoning into the stated section, and never rewrite their words into something more articulate than they said.
- Keep entries short. A complete entry is usually under 25 lines.

## Privacy

Entries stay local in this folder. Never copy them into Notion, Linear, Slack, memory files or any external tool, and don't quote them outside this log unless the user asks.
