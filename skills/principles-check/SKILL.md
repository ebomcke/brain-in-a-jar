---
name: principles-check
description: Check a pending decision against the user's decision principles before committing to it, when they are not there to guide you. Use when you are about to make an architecture, implementation, or planning and priority call on your own (autonomous run, background agent, subagent, overnight work) and a capable agent could reasonably choose differently. Returns the principles that apply and a short reasoning; it advises only and never blocks you.
---

# Principles check

A first-pass stand-in for the user. It tells you which option their principles favour, so fewer of your decisions get overruled when they review them later. It is advice, not a gate: you always make the call and carry on.

## When to use

Before committing to a decision that meets all three:
1. **Category:** architecture, implementation, or planning and priorities.
2. **Real alternative:** a capable agent could reasonably have chosen differently.
3. **Outlives the session:** it shapes code, plans or commitments others will build on.

Skip it for naming, formatting, mechanical steps, and anything the user already decided in this session.

## How

1. Read `~/PersonalAssistant/principles.md` (expand `~` to the home directory). It is the only source; if it does not exist, answer `Principles: none available yet` and decide as you normally would. Read the whole file each time (it changes), including scope lines, precedence rules and noted tensions. Ignore the Watch list: those are not principles.
2. State the decision and the options in one line each.
3. Test each option against each principle that bears on it. Most decisions touch two or three; don't force the rest.
4. Where principles pull apart, apply any precedence rule in the file (e.g. separation of concerns wins over KISS). If none settles it, say so; don't invent one.
5. Recommend the option that fits best, then return to your task.

Do not cite or invent principles that are not in the file, and do not read anything else in that folder.

## Answer format

Return this to yourself (or the agent that asked), and nothing more:

```
Principles check
Decision: <one line>
Recommended: <option>
Principles: #<n> <name> (supports | strains), ...
Reasoning: <1–3 lines linking the options to those principles>
Open tension: <a conflict no precedence rule settles, or "none">
```

If no principle applies, say `Principles: none apply` and decide as you normally would.

## What this skill does not do

- It never blocks, pauses or escalates. You decide and continue.
- It does not log or record anything. If you keep a decision record, including this check in it is your job.
