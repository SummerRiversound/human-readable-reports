# human-readable-reports

An agent skill that makes AI-written **analysis, reports, and findings docs**
easy for humans to read and share — fighting the default "AI verbosity / slop"
failure mode (walls of jargon, telegraphic bullets, dumped `file:line`, coined
term-of-art).

The content stays deep; the **reading** gets easy. Ten rules + a checklist +
a before/after example.

## Install

**Claude Code / Claude agents** — drop the folder into your skills directory:

```
~/.claude/skills/human-readable-reports/SKILL.md      # personal
<project>/.claude/skills/human-readable-reports/SKILL.md   # project-shared
```

The agent auto-loads it when a task matches the `description` (writing a report
or analysis doc for people).

**Other agent frameworks** — paste `SKILL.md`'s rules into your system prompt or
style guide. The rules are framework-agnostic and language-agnostic.

## What it does

- Leads with the point, in plain language, in the reader's own language.
- Opens with a concrete analogy for the core concept.
- Explains or drops jargon; turns formulas into words.
- Moves evidence (`file:line`, code) to end-notes, out of the prose.
- Uses progressive disclosure (collapsible detail) so collapsed = simple,
  expanded = complete.
- Presents analysis and options — doesn't set the reader's agenda.

## When not to use

For a spec or implementation plan an *agent* executes step-by-step, precision
beats plainness — keep the exact code locations inline. This skill is for the
human-facing report.

## License

MIT.
