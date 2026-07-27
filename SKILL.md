---
name: human-readable-reports
description: >-
  Use when writing an analysis, report, findings, or status doc for humans to
  read and share — not a spec an agent executes. Fights AI verbosity/slop and
  makes deep content easy to read.
---

# Human-Readable Reports

An AI report defaults to **verbosity and slop**: dense jargon, fragment bullets,
`file:line` dumps in the prose, coined term-of-art, caveats nobody asked for.
The reader becomes the bottleneck. Keep the content deep — make the *reading*
easy.

## Rules

1. **Reader-first.** Write in the reader's language, at their level. Know who
   they are (e.g. "mid-level dev sharing with the team").
2. **Point first (BLUF).** Each section opens with the takeaway in one plain
   sentence — understandable from that line alone.
3. **Analogy up front.** Ground the core concept in something concrete before
   any detail (e.g. a cache → "reusing a picture you already drew"; a stale
   entry → "the price changed but the old picture is still on the shelf").
4. **Full sentences, not fragments.** "Method A skips the redraw when nothing
   changed," not "A · time-axis · skip render+send."
5. **Explain or drop jargon.** Gloss each technical term on first use; never
   ship your own coined names without a plain rewrite; turn formulas into words.
6. **Evidence to the back.** No `file:line`, stack traces, or code mid-sentence.
   Put them in a small "basis/sources" note at the section's end.
7. **Table cells read as phrases**, not code (`deleteOverBytes(k)` → "removes
   the least-recently-used first").
8. **Cut the slop.** Delete throat-clearing intros, hedging caveats, restated
   obviousness. Shorter is kinder.
9. **Progressive disclosure.** Hide depth behind expandable sections (collapsible
   `<details>` or nested headings): summary always visible, detail on demand.
10. **Tone: teammate, not lecture.** Warm and direct, in the reader's polite
    register — like talking a colleague through it, not a manual.

## Before / after

> **Before:** `parseInput()` is O(n²) due to nested scan (utils.js:88); latency
> scales quadratically with list size → UI jank at scale.
>
> **After:** **The input parser gets slow as the list grows** — it re-scans the
> whole list for each item, so a long list makes the screen stutter. *(Code
> location in the notes below.)*

## Checklist

- [ ] Reader and level are known.
- [ ] Each section's first line is the plain takeaway.
- [ ] An analogy grounds the core concept.
- [ ] No unglossed jargon / coined names / bare formulas.
- [ ] No `file:line` or code in body prose (moved to end-notes).
- [ ] Table cells read as sentences; slop removed.
- [ ] Depth is behind progressive disclosure.
- [ ] Tone reads like explaining to a teammate.

## When NOT to apply

A spec or implementation plan an *agent executes step-by-step* needs precision
over plainness — keep exact `file:line`, signatures, and code inline. Ask: "will
a person read this to understand, or will an agent run it?"
