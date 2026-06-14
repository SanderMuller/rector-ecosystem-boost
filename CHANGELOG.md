# Changelog

All notable changes to `sandermuller/rector-ecosystem-boost` will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- **`oss-contribution` skill — comment discipline + open-PR etiquette.** A "Replies and comments"
  section: review replies and issue comments are drafted for the human and kept short (one point per
  comment, no walls of text / tables / link-walls, concede fast) — a long AI comment is the slop
  maintainers call out. A "While a PR is open" section: re-check the PR's state (`git fetch`, re-read)
  before pushing to it, and convert the PR to draft before pushing changes that move what the reviewer
  is looking at.

### Changed

- **`pr-contribution` guideline** — dropped the closing "increasingly gate-enforced … `symplify/phpstan-rules`"
  sentence. It named an external repo that can drift and didn't accurately describe the list it was
  attached to (these are human-judgment conventions, not gate-checkable). Enforcement nuance already
  lives, repo-aware, in the house-style guideline's enforcement note.
- **`rector-ecosystem-house-style` guideline** — dropped the dated provenance line ("Generalised from
  the two existing CLAUDE.md files …, 2026-06-13"); a reader doesn't act on it and it dates the file.

## v1.0.0 - 2026-06-14

<!-- verified-sha: 762aa0ef02e264999d4bbe8be25b0b9982301cfe -->
Initial release of `sandermuller/rector-ecosystem-boost` — a Composer-distributed boost catalog of agent skills, coding standards, and PR conventions for the rector / symplify / TomasVotruba ecosystem.

### Added

- **`oss-contribution` skill** — the foreign-repo contribution playbook: read the target's rules first, route output by confidence × blast-radius, run the gate, draft maintainer-facing prose for a human (never post it as the agent), one PR at a time, scale ambition to trust.
- **`rule-authoring` skill** — the canonical shape for an ecosystem rule (Rector / PHPStan / symplify): mirror-path namespace, `final` + framework base, constructor injection, bail-early, the test + fixture conventions, no value-object tests. Resolves per-repo by the `rule.*` conventions slots.
- **`rector-ecosystem-house-style` guideline** — the shared rectorphp / symplify / TomasVotruba coding standards: strict types, final classes, ReflectionProvider over runtime introspection, no setters, no debug dumps, narrow types. Includes an enforcement note documenting where the gate does and does not catch violations.
- **`pr-contribution` guideline** — the ecosystem's PR conventions: one feature per PR, tests required for behavior changes, lead with a failing test, run the gate before opening, no coverage-chasing or Docker-to-everything padding, why-not-what descriptions.
- **`verification-before-completion` guideline** — evidence-before-claims with two tiers (per-change / at-completion), a doc-only carve-out (no PHP changed → no need to run the full suite), and an intended-red carve-out (a failing-test contribution is supposed to leave the gate red).
- **`conventions-schema.json`** — per-repo slot vocabulary (`gate.command`, `contribution.base_branch`, `package.kind`, `php.min`, `docs.path`, `rule.framework`, `rule.test_case_base`, `rule.identifier_scheme`) consumed by skills and guidelines at sync time, so one catalog tunes itself per repo.

**Full Changelog**: https://github.com/SanderMuller/rector-ecosystem-boost/commits/v1.0.0

## [Unreleased]
