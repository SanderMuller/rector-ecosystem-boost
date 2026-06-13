## Ecosystem PR conventions

How a change becomes a PR in the rector / symplify / Tomas ecosystem. (Contributing to a repo you
don't own? The `oss-contribution` skill wraps these with foreign-repo etiquette; this guideline is the
always-loaded baseline that applies either way.)

- **One feature per PR.** Don't bundle unrelated changes. Notice a second thing → a second PR.
- **New features need tests.** A rule, service, or behavior change ships with its test (shape in the
  `rule-authoring` skill). A PR touching `src/` or `rules/` with no matching test change is incomplete.
- **Lead with a failing test where it fits** — the ecosystem's preferred way to report a bug. The
  failing test *is* the report; a red gate on that new test is success (intended-red), not a defect to
  "fix" away.
- **Run the gate before opening:** <!--boost:conv path="gate.command" mode="inline" fallback="the project's aggregate quality gate if it has one (e.g. composer complete-check), otherwise its components — ECS + PHPStan + the test suite"-->.
  That's the definition of done — see the `verification-before-completion` guideline.
- **Don't pad.** No coverage-chasing, no Docker-to-everything, no unit tests for getters/setters or
  value objects. These read as AI slop, waste maintainer attention, and the ecosystem's tools and
  maintainers reject them.
- **Description: why, not what.** Lead with the user-facing change and the motivation; the diff says
  what. No class/method names in the summary, no file lists, no commit-by-commit recaps. (A summary
  naming a class that isn't in the diff reads as a hallucination.)
- **Small and cheap to verify.** Scale caution to blast radius; prefer the smallest obviously-correct
  change; a one-command repro beats prose.

Several of these are increasingly **gate-enforced**, not just prose — see the house-style guideline's
enforcement note and the `symplify/phpstan-rules` set (the soft guidance here mirrors the hard rules
there).
