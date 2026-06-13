## Verification Before Completion

Before claiming any work is complete or successful, run the verification command fresh and confirm the
output. **Evidence before claims, always.**

### Required before any completion claim

1. **Run** the relevant command (in the current message, not from memory).
2. **Read** the full output.
3. **Confirm** it supports the claim.
4. **Then** state the result with evidence.

### Which commands

The concrete commands are project-specific. Resolve them from the project conventions slots
`verification.per_change` and `verification.at_completion` (command lists). If empty, read the
project's `CONTRIBUTING`/`CLAUDE`/`AGENTS` to find its gate (e.g. `composer complete-check`,
`npm test`) and propose using that. Do not invent a command.

### Two tiers

- **Per change (after each edit)** — the fast checks: code-style/format on the dirty set, plus the
  related tests only. Run every time you touch code.
- **At completion (once, before handing off)** — the slow checks: full static analysis and the full
  test suite. Run once, at the end. These are the definition of done.

### Doc-only / config-only change carve-out

If the change touches **no executable code** (only Markdown, docs, or agent-config files), the slow
tiers do not exercise it. State that explicitly with the diff as evidence — e.g. "no PHP changed, so
PHPStan and the test suite are provably unaffected; `git diff --stat` shows only `*.md`" — rather than
burning the full suite. Still run the fast style/lint pass if it touches files the linter reads.
*(Dogfood finding 2026-06-13: the generic gauntlet, taken literally, would run a multi-minute test
suite for a one-file Markdown move. The carve-out keeps "evidence before claims" honest without the
waste.)*

### Intended-red carve-out (failing-test contributions)

Some contributions are *supposed* to leave the gate red: a **failing test** that demonstrates a bug
(a mode rector and others explicitly invite). Here, "gate green = done" inverts — the new test
**failing** is the success condition. Do not "fix" the code to make it pass (that destroys the
contribution), and do not read the red as a defect. Confirm the *rest* of the suite still passes, and
state plainly: "the added test fails as intended, demonstrating <bug>; the rest of the suite is green."
*(Dogfood finding 2026-06-13: the literal gauntlet would have blocked or mis-"fixed" a failing-test PR.)*

### Capture the output

Append `|| true` to verification commands so the output is captured even on a non-zero exit, instead
of being swallowed and forcing a second run just to read the error.

### Never claim without evidence

Phrases like "should work now", "that should fix it", "looks correct", "I'm confident this works" mean
verification is missing. Run the command first, then report what actually happened.
