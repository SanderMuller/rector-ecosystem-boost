## Rector/Symplify/Tomas ecosystem — house style

Shared conventions across `rectorphp/*`, `symplify/*`, and `TomasVotruba/*`. These repos are one
author's house style on one tooling spine (`rector/rector`, `phpstan/phpstan`,
`symplify/phpstan-rules`, `symplify/coding-standard`, `tomasvotruba/unused-public`,
`rector/type-perfect`). Per-repo specifics (exact gate command, package kind, min PHP, docs path) come
from the project conventions slots — this file is the shared part.

### Language & types

- `declare(strict_types=1);` at the top of every PHP file.
- PHP floor is per-repo (`php.min` slot; commonly `8.3`). Don't use syntax that breaks the floor.
- Types must be narrow (type-perfect: `no_mixed`, `null_over_false`, `narrow_param`, `narrow_return`).
  Prefer `null` over `false` for "no result".

### Classes

- `final` by default; `abstract` only when explicitly meant for extension.
- Constructor property promotion with `private readonly` for dependencies.
- No setters / setter ceremony (`NewOverSettersRule`, `NoReturnSetterMethodRule` enforce this).

### Forbidden

- No `var_dump` / `dd` / debug dumps.
- **No runtime introspection** — no `class_exists` / `function_exists` / `property_exists` /
  `get_defined_functions` as logic. Use PHPStan `ReflectionProvider`. (Correctness over speed: runtime
  checks vary per local environment/extensions.)
- No dynamic names (`NoDynamicNameRule`); no `@` error suppression.
- Don't let Rector/ECS rewrite `config/` class-string literals — they're intentional.

> **Enforcement varies by repo — follow these as expected *style* regardless, and don't assume a green
> gate proves compliance.** The gate only catches them where the package is wired in:
> `symplify/phpstan-rules` (forbidden funcs, dynamic names, setters) and `type-perfect` ship in
> rector-src but **not** every ecosystem repo (e.g. `class-leak` has neither). And even in rector-src
> the gate misses prose-only rules — `var_dump` is caught, `get_defined_functions` is **not**
> (dogfood-verified 2026-06-13). Channel B (adding such rules to `symplify/phpstan-rules`) is what
> closes that gap.

### Tests

- Cover new **services and rules** with unit tests. **Do not test value objects or entities.**
- Rule packages mirror the rule path under the tests dir; fixtures use the package's fixture
  convention (`.php.inc` for Rector rules; `Fixture/` + `configured_rule.neon` for PHPStan rules).
- `Fixture/`, `Source/`, `Expected/` dirs are auto-exempt from ECS/PHPStan/Rector — don't try to make
  them conformant.

### Adding a rule (shape)

Namespace mirrors the path. `final class` extends the framework's `AbstractRector` (Rector) or
`Rule`/`RuleTestCase` (PHPStan). Implement the framework's required methods; add a `@see` to the test
class; inject services via constructor promotion; bail out early (first-class-callable, name match,
arg presence, type) before transforming. Register it in the matching config set; add its identifier
to the scheme if the repo uses one (`rule.identifier_scheme` slot, e.g. `symplify.<camelName>`).
Document it where the repo documents rules (`docs.path` slot).

### Before finishing — the gate

Run the repo's gate (`gate.command` slot — e.g. `composer complete-check`, which is ECS + PHPStan +
phpunit) and its fixer (`gate.fix_command`, e.g. `composer fix-cs`). Fix anything they report. See the
`verification-before-completion` guideline.

### Don't

Introduce new abstractions/traits/helpers beyond the task; modify PHPStan/ECS ignore lists to silence
a new warning (fix the code); add runtime existence checks; push docs into the repo root (they belong
at `docs.path`).
