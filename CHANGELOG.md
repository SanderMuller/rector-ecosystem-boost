# Changelog

All notable changes to `sandermuller/rector-ecosystem-boost` will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
