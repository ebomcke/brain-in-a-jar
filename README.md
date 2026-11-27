# brain-in-a-jar 🧠🫙

Two Claude Code skills that learn how you make decisions, then let agents apply that judgement when you're not around.

- **`decision-log`** quietly records the architecture, implementation and planning decisions you make in any Claude session: what you chose, what else was on the table, and why. It asks at most one optional follow-up question, which you can ignore.
- **`principles-check`** lets an agent that's deciding on its own check its options against your principles first. It only advises: it never blocks the agent and never logs anything. Recording the check is the agent's job.

The loop: log decisions for a week or two, review the backlog with Claude to derive principles, write them to `principles.md`, and let agents use them.

## What stays local

Your data never goes into git:

- `decisions/` is the raw backlog, one Markdown file per decision.
- `principles.md` holds your principles. Start from `principles.template.md`.

Both are gitignored.

## Install

These steps assume the repo lives at `~/PersonalAssistant`, because both skills use that path. If you put it elsewhere, update the paths in both `SKILL.md` files and in the permission rules below.

1. Clone the repo:

   ```bash
   git clone https://github.com/ebomcke/brain-in-a-jar.git ~/PersonalAssistant
   ```

2. Make the skills available in every project:

   ```bash
   mkdir -p ~/.claude/skills
   ```

   ```bash
   ln -s ~/PersonalAssistant/skills/decision-log ~/.claude/skills/decision-log
   ```

   ```bash
   ln -s ~/PersonalAssistant/skills/principles-check ~/.claude/skills/principles-check
   ```

3. Let sessions in other folders write decisions and read principles without a permission prompt. Merge this into `~/.claude/settings.json`:

   ```json
   {
     "permissions": {
       "allow": [
         "Edit(~/PersonalAssistant/decisions/**)",
         "Read(~/PersonalAssistant/decisions/**)",
         "Read(~/PersonalAssistant/principles.md)"
       ]
     }
   }
   ```

4. Make `decision-log` trigger reliably. Add this to `~/.claude/CLAUDE.md`:

   ```markdown
   ## Decision log
   When I make an architecture, implementation, or planning and priority decision — especially
   when I override an agent's proposal — use the decision-log skill.
   ```

5. Start a new Claude Code session so the skills load. To check they're installed, ask Claude "which skills do you have?"

`principles-check` reports "none available yet" until you create `principles.md`.

## Reviewing your decisions

After a week or two of logging, open a Claude session in `~/PersonalAssistant` and ask it to review the backlog. Here's what worked for us:

1. Claude clusters the decisions into themes and proposes one generic principle at a time, citing the decisions behind it.
2. You keep, reword or drop each one. Dropped candidates go on the watch list.
3. A second pass looks for inconsistencies. These usually end up as a scope line, a precedence rule or a noted tension rather than a new principle.

Inferred reasoning in an entry is Claude's guess, not yours, so give it less weight than reasoning you stated.
