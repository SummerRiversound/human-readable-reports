# Examples

Each example is one complete report written twice: once the way a coding agent
writes it by default, and once with this skill. Both versions hold the same
facts. Nothing is added and nothing is lost; the evidence only moves to the end.

## Flaky CI test

A coding agent investigated a test that fails on CI about 1 run in 14 and wrote
up the cause for the team. The project is fictional; the failure pattern (test
workers sharing one database) is a common one.

- [Before](./flaky-ci-test/before.md): what the agent writes by default.
- [After](./flaky-ci-test/after.md): the same findings with the rules applied.

The after version is longer. The extra words are the step-by-step cause that
the before version squeezed into one arrow chain, and the code locations moved
into a notes section at the end.

| What changed | Rule |
| --- | --- |
| The title and first line state the cause and the fix, instead of "After an extensive investigation" | 2. Point first |
| "Nuke race", "W4 regime" and "cross-worker bleed" are gone; the failure is told as three numbered steps | 3. Concrete before abstract, 5. Explain or drop jargon |
| Arrow-chain bullets became sentences; the repeat results became a table | 4. Full sentences |
| "Test worker", "Redis" and "TTL" each get one plain line on first use | 1. Reader-first, 5. Explain or drop jargon |
| File paths, line numbers, commands and the error text moved to the notes | 6. Evidence to the back |
| Option table cells say what changes, not the config key | 7. Table cells read as phrases |
| The background paragraph, the hedged caveats and the sign-off are cut | 8. Cut the slop |
| The notes sit in a collapsed section | 9. Progressive disclosure |
| "Should eliminate about 95%" and "about 2x CI time" were never measured; they became "TBD" and "not measured" | 11. Numbers match the evidence |
| "Same root cause" for the session test was a guess; it became an open question | 11. Numbers match the evidence |
| "11/150" became "11 of the last 150 runs, about 1 in 14" | 11. Numbers match the evidence |
| Bold is kept for the verdict and the recommendation only | 12. Shared-document hygiene |
