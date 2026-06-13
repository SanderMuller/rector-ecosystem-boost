---
name: oss-contribution
description: 'Contribute to a repository you do NOT own or maintain (open source, an upstream you forked, another team). Use when opening or preparing a PR/patch to a foreign repo, or when the user mentions: contribute upstream, open-source PR, send a patch, fix in someone else''s repo. NOT for your own/home repos — use the project''s own pull-requests skill there.'
metadata:
  boost-tags: github
  schema-required: ^1
---

# Contributing to a repo you don't own

This skill governs contributions to a **foreign** repository — one you do not own or maintain. It
inverts the assumptions of an owner-repo PR skill: you follow the target's rules, not yours; you hand
every maintainer-facing action to a human; and you optimize for the maintainer's cost-to-trust, not
your own throughput.

## First: is this a foreign repo?

Treat the repo as foreign if any of these hold:

- there is an `upstream` remote distinct from your `origin` (you forked someone else's repo), or
- you are not a listed maintainer/owner of the repo, or
- the project conventions declare it foreign (`contribution.home: false`).

If it is your own/home repo, **stop** and use the project's own `pull-requests` skill instead. The rest
of this skill assumes foreign.

## The non-negotiable kernel (every foreign repo, regardless of its docs)

1. **Read the target's `CONTRIBUTING`, `CLAUDE.md`/`AGENTS.md`, and PR template FIRST** — before
   writing anything — and audit your diff against them before handing off. Most friction is a
   documented rule you skipped.
2. **One logical change per PR.** Don't bundle. Notice a second thing? Note it; don't fix it here.
3. **Ship via the project's sanctioned path.** Never commit to a protected branch; never hand-roll a
   flow that skips the project's gates.
4. **Never force-push to "fix" a PR under review** without a one-line reason — it destroys review
   history and reads as hiding something.
5. **Maintainer-facing prose is drafted for a human to post**, never posted by the agent: PR
   descriptions, review replies, issue comments. Draft it, hand it off.
6. **One PR open at a time** unless the maintainer invited more. Let it land or close before the next.
7. **Scale ambition to trust.** Gauge it from merged-PR count **and recent sentiment** — a fresh
   friction event resets you toward cold regardless of count. Rough ladder: *cold / 0 merges* → a
   ≤~20-line obvious fix, a typo, or a failing test; *a few merges* → one bugfix plus its test;
   *established* → a feature or a new rule. Never a large, high-blast-radius change (a core refactor,
   a cache rewrite) on a cold or freshly-strained relationship.

## Workflow

**Before anything, route the output by confidence × blast-radius** — not every change should be a PR.
High confidence + low blast-radius → a normal PR. Uncertain, or high blast-radius (core or shared
code) → prefer a *draft* PR, or an issue with a candidate patch and the specific open question, over a
confident-looking PR. A draft honestly signals "needs eyes"; a polished PR does not. When unsure,
lower the format, not the honesty.

1. **Confirm foreign + read the rules** (kernel #1). Resolve from project conventions: gate command,
   base branch, whether an issue is required first, PR title format. If a convention is missing, infer
   it from `composer.json` scripts (`check-cs`/`phpstan`/`test`/…), the presence of
   `ecs.php`/`phpstan.neon`/`rector.php`, and the test-dir layout — most repos have **no**
   CONTRIBUTING/CLAUDE/AGENTS, so the build config *is* the convention. If still unknown, **ask the
   user — do not guess.**
2. **Scope to the smallest change** that delivers the intent. If the project documents a
   "contribute a failing test" mode (many do), lead with that.
3. **Branch** from the target's default base (from conventions; no enforced name unless the target
   documents one).
4. **Run the gate** — <!--boost:conv path="gate.command" mode="inline" fallback="the resolved quality gate (composer complete-check / npm test), or its components check-cs + phpstan + phpunit where there is no aggregate"-->. This is the
   definition of done. Follow the `verification-before-completion` guideline (doc-only and
   intended-red carve-outs included). **Intended-red:** if the contribution is a *failing test* (a bug
   demonstration — a mode rector and others explicitly invite), the new test is *supposed* to fail;
   that red is the success condition, not a defect — do not "fix" it. The rest of the suite must pass.
5. **Adversarial self-review** before handing off (e.g. the `codex-review` skill) — the cheapest
   reviewer in the pipeline; required for any change touching executable code. For a doc/config-only
   change (Markdown, agent-config — nothing executable), it may be skipped with a one-line note — the
   same carve-out the `verification-before-completion` guideline applies to the gate.
6. **Draft the PR description** to a file/buffer for the human (see below).
7. **Hand off — do not open it yourself.** Push to your fork and let the human open the PR, or, if the
   project expects a draft, open a draft and tell the human exactly what needs their post (the
   description, the review request). Do **not** mark ready, request the maintainer as reviewer, or
   @-mention anyone without the human's say-so.

## Description (drafted for the human)

Lead with the user-facing change and the motivation (the *why*) — not the implementation; the diff is
the *what*. Keep the summary to 1–3 sentences, then add how to verify and any risk/edge notes.

**Banned in the summary:** class/trait/method names, file paths, package-version arrows, refactor
framing ("extracts", "consolidates") without the behavior it enables, commit-by-commit recaps. No
tables or link-walls in maintainer-facing text unless asked.

Quick test before handing off: would someone who doesn't read code understand what this delivers, and
could the summary be reused almost verbatim in release notes? If not, rewrite.

## What this skill never does

Open / merge / mark-ready a PR for the human; request reviewers; post comments or replies to
maintainers; force-push under review; bundle unrelated changes; add a dependency on your own tooling
to the target repo; introduce CI or gates the maintainer didn't ask for.
