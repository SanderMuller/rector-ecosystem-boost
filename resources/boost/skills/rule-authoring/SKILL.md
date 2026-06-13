---
name: rule-authoring
description: 'Write or change a static-analysis / refactoring rule in the rector/symplify/Tomas ecosystem — a Rector rule, a PHPStan rule, or a symplify/phpstan-rules rule. Activate when: adding or modifying a rule, "write a rector rule", "add a phpstan rule", "new rule/sniff", or touching rules/ or src/Rules/.'
metadata:
  boost-tags: php
  schema-required: ^1
---

# Authoring an ecosystem rule

This repo's rule framework is <!--boost:conv path="rule.framework" mode="inline" fallback="auto-detect: Rector (extends AbstractRector, rules/ + rules-tests/) or PHPStan (implements Rule, src/Rules/ + tests/Rules/)"-->.
Follow the canonical shape for that framework. Don't improvise structure — mirror an existing rule in the same directory.

## The shape (both frameworks)

1. **Mirror the path.** The namespace follows the directory; a rule under `<Category>/.../<RuleName>.php` is namespaced to match.
2. **`final class`** extending the framework base — Rector: `Rector\Rector\AbstractRector`; PHPStan: `implements Rule` typed `Rule<NodeType>`. Test-case base: <!--boost:conv path="rule.test_case_base" mode="inline" fallback="AbstractRectorTestCase for Rector, RuleTestCase for PHPStan"-->.
3. **Inject via constructor promotion** (`private readonly`); reuse what the base exposes (`$this->nodeNameResolver`, `ReflectionProvider`, …). **Never runtime introspection** (`function_exists` / `get_defined_functions`) — use `ReflectionProvider`.
4. **Bail out early** — first-class-callable check, name match, arg presence, type — *then* transform. Cheapest checks first.
5. **`@see` the test class** in a PHPDoc.
6. **Rule identifier** if the repo uses a scheme: <!--boost:conv path="rule.identifier_scheme" mode="inline" fallback="none — skip the identifier step"-->.

### Rector specifics
- Implement `getRuleDefinition()` (one-line description + at least one before/after `CodeSample`), `getNodeTypes()`, and `refactor(Node): ?Node` — return the new node, `null` for no change, or `NodeVisitor::REMOVE_NODE`. Never return other integers.
- Implement `MinPhpVersionInterface` + `provideMinPhpVersion()` when the rule is version-bound.

### PHPStan specifics
- `getNodeType()` + `processNode(Node, Scope): array` returning `RuleErrorBuilder` errors, each with an identifier.
- Register the rule in the matching `config/<topic>-rules.neon`; add its identifier to the rule-identifier enum.

## Tests — required, with a specific shape

A rule without a test is not done. Mirror the rule path under the test directory.

- **Rector**: `<RuleName>Test extends AbstractRectorTestCase`, `#[DataProvider('provideData')]`, returns `yieldFilesFromDirectory(__DIR__ . '/Fixture')`. Fixtures are `.php.inc`, before/after split by a line containing exactly `-----`; no separator means "unchanged" (name those `skip_*`). A fixture's `namespace` matches its directory.
- **PHPStan**: `<RuleName>Test extends RuleTestCase`, `#[DataProvider('provideData')]`, asserts `[ERROR_MESSAGE, $line]` pairs, wired via `getAdditionalConfigFiles()` + a `Fixture/` directory.
- `Fixture/`, `Source/`, `Expected/` directories are auto-exempt from ECS / PHPStan / Rector — don't try to make them conformant.
- **Do not unit-test value objects or entities** — cover the rule (and real services), not data holders.

## Before finishing

Run the gate: <!--boost:conv path="gate.command" mode="inline" fallback="the project's aggregate gate if it has one (e.g. composer complete-check), otherwise ECS + PHPStan + the rule's test"-->.
Document the rule where the repo documents rules (<!--boost:conv path="docs.path" mode="inline" fallback="README or the docs directory"-->).
