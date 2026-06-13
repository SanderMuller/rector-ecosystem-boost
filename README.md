# oss-contribution-boost

[![Latest Version on Packagist](https://img.shields.io/packagist/v/sandermuller/oss-contribution-boost.svg?style=flat-square)](https://packagist.org/packages/sandermuller/oss-contribution-boost)
[![Total Downloads](https://img.shields.io/packagist/dt/sandermuller/oss-contribution-boost.svg?style=flat-square)](https://packagist.org/packages/sandermuller/oss-contribution-boost)
[![License](https://img.shields.io/packagist/l/sandermuller/oss-contribution-boost.svg?style=flat-square)](LICENSE)

Agent skills and guidelines for contributing to repositories you do **not** own — open source, an
upstream you forked, another team's repo. A Composer-distributed [boost](https://github.com/sandermuller/boost-core)
catalog: pure Markdown, no runtime code.

## What it ships

- **`oss-contribution` skill** — the foreign-repo contribution playbook: read the target's rules
  first, run its gate, draft maintainer-facing prose for a human (never post it as the agent), one PR
  at a time, scope ambition to trust.
- **`verification-before-completion` guideline** — evidence-before-claims, with doc-only and
  intended-red (failing-test) carve-outs.
- **`rector-ecosystem-house-style` guideline** — the shared rectorphp / symplify / TomasVotruba
  conventions (php-tagged).
- **`conventions-schema.json`** — per-repo slots (gate command, base branch, package kind, …) the
  skill resolves at sync time.

## Install

```bash
composer require --dev sandermuller/oss-contribution-boost
```

Allowlist it and sync with a boost engine
([`sandermuller/boost-core`](https://github.com/sandermuller/boost-core) or
[`laravel/boost`](https://github.com/laravel/boost)):

```php
// boost.php or .config/boost.php
return BoostConfig::configure()
    ->withAgents([Agent::CLAUDE_CODE, Agent::CODEX, Agent::CURSOR])
    ->withAllowedVendors(['sandermuller/oss-contribution-boost'])
    ->withTags(['github', 'php'])
    ->withConventions([/* gate.command, contribution.base_branch, … */]);
```

```bash
vendor/bin/boost sync
```

The skill and guidelines fan out into each configured agent's directory (Claude Code, Codex, Cursor,
…). Per-repo values are supplied through `withConventions(...)` against `conventions-schema.json`.

## License

MIT — see [LICENSE](LICENSE).
