---
name: human-readable-reports
description: >-
  Use when writing an analysis, report, findings, or status doc for humans to
  read and share — not a spec an agent executes. Fights AI verbosity/slop and
  makes deep content easy to read.
---

# Human-Readable Reports

A report is read by people who were not in the session. Dense jargon, fragment
bullets, `file:line` dumps in the prose, coined terms, padding, and caveats
nobody asked for make the reader the bottleneck. Keep the content deep — make
the *reading* easy.

## Rules

1. **Reader-first.** Write in the reader's language, at their level. Name the
   reader before writing. Default: a junior developer who was not in the
   session. Two others recur: a non-developer client or operator (no file /
   code / API / DB words at all) and a senior colleague (more formal
   register). When the reader hands you a structure ("1. … 2. …"), it becomes
   the heading skeleton.
2. **Point first (BLUF).** Each section opens with the takeaway in one plain
   sentence — understandable from that line alone.
3. **Concrete before abstract — the right kind of concrete.** An unfamiliar
   *concept word* gets one grounding line (a cache → "reusing a picture you
   already drew"). A *situation, cause or change* is told as the sequence that
   actually happens (1. the request comes in → 2. the template is called →
   3. it returns empty) and as "now it is X → after this it is Y" — no invented
   analogies, no comparisons to products the reader does not use. Examples the
   reader gives are illustrations, not facts to verify.
4. **Full sentences, not fragments.** "Method A skips the redraw when nothing
   changed," not "A · time-axis · skip render+send." Parallel items go in
   bullets or a table; cause and effect stays in sentences or numbered steps.
5. **Explain or drop jargon.** Gloss each technical term on first use; never
   ship your own coined names without a plain rewrite; turn formulas into words.
6. **Evidence to the back.** No `file:line`, stack traces, or code mid-sentence.
   Put them in a small "basis/sources" note at the section's end.
7. **Table cells read as phrases**, not code (`deleteOverBytes(k)` → "removes
   the least-recently-used first").
8. **Cut the slop.** Delete throat-clearing intros, hedging caveats, restated
   obviousness — keep the length the content needs, no more.
9. **Progressive disclosure.** Hide depth behind expandable sections (collapsible
   `<details>` or nested headings): summary always visible, detail on demand.
10. **Tone: teammate, not lecture.** Warm and direct, in the reader's polite
    register — like talking a colleague through it, not a manual.
11. **Numbers and claims match the evidence.** No unmeasured figure as a
    recommendation — write "TBD (set after measuring)" or drop it. State
    results as strongly as the evidence allows ("in this run, X appears to
    …"), limits in one short paragraph. Metrics read as counts the reader can
    picture ("recall 29%" → "finds about 3 in 10"). A "headline only" request
    gets headings only.
12. **Shared-document hygiene.** No decorative labels or spaced-out titles, no
    `--` separators (break the line), no "e.g." (write "for example"), section
    titles in the document's own language (a German doc gets "Zusammenfassung",
    not "Executive Summary"), no emoji, and no typographic symbols (①, ·, ×) in
    text people will type. Bold only the verdict or the one key line.
    Table-heavy PDFs go landscape with no wrapping in the first column. When a
    prior deliverable exists, follow its structure, file name and punctuation.
    A document too long to read gets a purpose-fitted digest first.
13. **Articles and talks** may open with a concrete scene instead of the
    takeaway, as long as it ties to the central question; argue case → meaning
    → claim; no filler claims ("these days", "revolutionary"); never invent
    the author's experiences or beliefs when drafting in their voice.
14. **Canonical docs state the final design only.** Specs, design docs and
    wireframe notes carry no work-item numbers (`AB#1234`), no "per §X"
    traceability cross-refs and no change rationale ("added because…"); that
    belongs in the commit message.

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
- [ ] Unfamiliar concept words get one grounding line; situations are told as
      the real sequence, not an analogy.
- [ ] No unglossed jargon / coined names / bare formulas.
- [ ] No `file:line` or code in body prose (moved to end-notes).
- [ ] Table cells read as sentences; slop removed.
- [ ] Depth is behind progressive disclosure.
- [ ] Tone reads like explaining to a teammate.
- [ ] Every number is measured or marked TBD; claims are no stronger than the
      evidence.
- [ ] No decoration, `--`, "e.g.", foreign-language headings, emoji or typed
      symbols.

## When NOT to apply

A spec or implementation plan an *agent executes step-by-step* needs precision
over plainness — keep exact `file:line`, signatures, and code inline. Ask: "will
a person read this to understand, or will an agent run it?"
