---
name: mvvm-boundaries
description: Inspect or change Dart code while preserving this repository's feature-first MVVM dependency boundaries.
---

# MVVM boundaries review and implementation

## Procedure
1. Locate the affected feature and inspect a neighboring implementation (start with `home`).
2. Identify which layer owns each responsibility before moving or adding code.
3. Confirm dependency direction: `presentation` → `data` (abstract repository). Cubit depends on the repository only — no domain or use-case layer. Entities live under `data/models/` (Hive on entities follows `home` when caching is required).
4. Keep transport/storage details in data sources; repositories orchestrate and return `Either<Failure, T>` via `safeCall` where confirmed in source.
5. Follow existing repository contracts, Cubit folding, and DI registration only where confirmed in source.
6. Check imports: presentation must not import Dio, Hive, or data sources directly.
7. Check DI registration and routing only if the change requires them.
8. Add/update tests and run the narrowest relevant checks.
9. Report any pre-existing architectural violations separately; do not refactor unrelated code.

## Do not
- Introduce a new architecture package, domain layer, or use-case layer without approval.
- Create abstractions only to satisfy a generic architecture ideal.
- Move files across layers without explaining the benefit and impact.
- Assume every feature must have exactly the same files; follow actual patterns and the requirements.
